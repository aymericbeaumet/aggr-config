---
title: Adding Keyboard Shortcuts To UIAlertView
link: https://steipete.me/posts/2013/adding-keyboard-shortcuts-to-uialertview/
source: steipete-me
published: 2013-03-06T22:38:00Z
updated: 2013-03-06T22:38:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Add keyboard shortcuts to UIAlertView and UIActionSheet for faster simulator testing by intercepting keyboard events with Enter and Escape keys.
content: extracted
html: 2013-03-06-adding-keyboard-shortcuts-to-uialertview.html
preview:
  file: 2013-03-06-adding-keyboard-shortcuts-to-uialertview.preview-5a50dfd976d7.webp
  width: 256
  height: 134
  color: '#e9e6e6'
images:
- source: https://steipete.me/posts/2013/adding-keyboard-shortcuts-to-uialertview/index.png
  original:
    file: 2013-03-06-adding-keyboard-shortcuts-to-uialertview.image-94628fb4a6c5.png
    width: 1200
    height: 630
  variants:
  - file: 2013-03-06-adding-keyboard-shortcuts-to-uialertview.image-8b983f8990dd.webp
    width: 320
    height: 168
  - file: 2013-03-06-adding-keyboard-shortcuts-to-uialertview.image-0a703160554b.webp
    width: 640
    height: 336
  - file: 2013-03-06-adding-keyboard-shortcuts-to-uialertview.image-57f0f2b51d74.webp
    width: 960
    height: 504
  - file: 2013-03-06-adding-keyboard-shortcuts-to-uialertview.image-7501a181032b.webp
    width: 1200
    height: 630
  color: '#fdfafa'
---

I’m not even home from the more-than-excellent [NSConference](http://nsconference.com) in Leicester, but had to hack on something super awesome that [Evan Doll of Flipboard](http://twitter.com/edog1203) presented earlier today. They added keyboard support to UIAlertView and UIActionSheet (amongst other things) — simply to **make debugging in the Simulator faster by accepting Esc and Enter keys** — something that Apple should have done anyway. There’s not much value in shipping this in release builds, except better support for bluetooth keyboards. And since this hack uses private API AND accesses a struct with a memory layout that could change, I don’t recommend shipping it. If you do, make sure that you whitelist iOS versions and block this method by default on unknown future versions of iOS. I’m using it in [PSPDFKit](http://pspdfkit.com) only when compiled in DEBUG mode for the Simulator.

The actual hack is mostly based on [this blog post about intercepting the keyboard on iOS](http://nacho4d-nacho4d.blogspot.co.uk/2012/01/catching-keyboard-events-in-ios.html), and it’s not pretty. I had to modify some constants to make it work on iOS 5/6:

```objective-c
#define GSEVENT_TYPE 2
#define GSEVENT_FLAGS 12
#define GSEVENTKEY_KEYCODE 15
#define GSEVENT_TYPE_KEYUP 10
// http://nacho4d-nacho4d.blogspot.co.uk/2012/01/catching-keyboard-events-in-ios.html
__attribute__((constructor)) static void PSPDFKitAddKeyboardSupportForUIAlertView(void) {
    @autoreleasepool {
        // Hook into sendEvent: to get keyboard events.
        SEL sendEventSEL = NSSelectorFromString(@"pspdf_sendEvent:");
        IMP sendEventIMP = imp_implementationWithBlock(^(id _self, UIEvent *event) {
            objc_msgSend(_self, sendEventSEL, event); // call original implementation.

            SEL gsEventSEL = NSSelectorFromString([NSString stringWithFormat:@"%@%@Event", @"_", @"gs"]);
            if ([event respondsToSelector:gsEventSEL]) {
                // Key events come in form of UIInternalEvents.
                // They contain a GSEvent object which contains a GSEventRecord among other things.
                int *eventMem = (int *)[event performSelector:gsEventSEL];
                if (eventMem) {
                    if (eventMem[GSEVENT_TYPE] == GSEVENT_TYPE_KEYUP) {
                        UniChar *keycode = (UniChar *)&(eventMem[GSEVENTKEY_KEYCODE]);
                        int eventFlags = eventMem[GSEVENT_FLAGS];
                        //NSLog(@"Pressed %d", *keycode);
                        [[NSNotificationCenter defaultCenter] postNotificationName:@"PSPDFKeyboardEventNotification" object:nil userInfo: @{@"keycode" : @(*keycode), @"eventFlags" : @(eventFlags)}];
                    }
                }
            }
        });
        PSPDFReplaceMethod(UIApplication.class, @selector(sendEvent:), sendEventSEL, sendEventIMP);

        // Add keyboard handler for UIAlertView.
        SEL didMoveToWindowSEL = NSSelectorFromString(@"pspdf_didMoveToWindow");
        IMP didMoveToWindowIMP = imp_implementationWithBlock(^(UIAlertView *_self, UIEvent *event) {
            objc_msgSend(_self, didMoveToWindowSEL, event); // call original implementation.

            static char kPSPDFKeyboardEventToken;
            if (_self.window) {
                id observerToken = [[NSNotificationCenter defaultCenter] addObserverForName:@"PSPDFKeyboardEventNotification" object:nil queue:nil usingBlock:^(NSNotification *notification) {
                    NSUInteger keyCode = [notification.userInfo[@"keycode"] integerValue];
                    if (keyCode == 41) /* ESC */ {
                        [_self dismissWithClickedButtonIndex:_self.cancelButtonIndex animated:YES];
                    }else if (keyCode == 40) /* ENTER */ {
                        [_self dismissWithClickedButtonIndex:_self.numberOfButtons-1 animated:YES];
                    }
                }];
                objc_setAssociatedObject(_self, &kPSPDFKeyboardEventToken, observerToken, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
            }else {
                id observerToken = objc_getAssociatedObject(_self, &kPSPDFKeyboardEventToken);
                if (observerToken) [[NSNotificationCenter defaultCenter] removeObserver:observerToken];
            }
        });
        PSPDFReplaceMethod(UIAlertView.class, @selector(didMoveToWindow), didMoveToWindowSEL, didMoveToWindowIMP);
    }
}
```

You will also need some swizzling helpers. Here’s what I use:

```objective-c
// http://www.mikeash.com/pyblog/friday-qa-2010-01-29-method-replacement-for-fun-and-profit.html
static void PSPDFSwizzleMethod(Class c, SEL orig, SEL new) {
    Method origMethod = class_getInstanceMethod(c, orig);
    Method newMethod = class_getInstanceMethod(c, new);
    if (class_addMethod(c, orig, method_getImplementation(newMethod), method_getTypeEncoding(newMethod))) {
        class_replaceMethod(c, new, method_getImplementation(origMethod), method_getTypeEncoding(origMethod));
    }else {
        method_exchangeImplementations(origMethod, newMethod);
    }
}

void PSPDFReplaceMethod(Class c, SEL orig, SEL newSel, IMP impl) {
    Method method = class_getInstanceMethod(c, orig);
    if (!class_addMethod(c, newSel, impl, method_getTypeEncoding(method))) {
        PSPDFLogError(@"Failed to add method: %@ on %@", NSStringFromSelector(newSel), c);
    }else PSPDFSwizzleMethod(c, orig, newSel);
}
```
