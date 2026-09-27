---
title: Hacking with Aspects
link: https://steipete.me/posts/2014/hacking-with-aspects/
source: steipete-me
published: 2014-05-06T00:45:00Z
updated: 2014-05-06T00:45:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Explore how Apple detects popover presentation in UIImagePickerController and learn to bypass the restriction using my Aspects library.
content: extracted
html: 2014-05-06-hacking-with-aspects.html
preview:
  file: 2014-05-06-hacking-with-aspects.preview-4e8cabb4c478.webp
  width: 256
  height: 134
  color: '#f1eeee'
images:
- source: https://steipete.me/posts/2014/hacking-with-aspects/index.png
  original:
    file: 2014-05-06-hacking-with-aspects.image-4896cdbb61eb.png
    width: 1200
    height: 630
  variants:
  - file: 2014-05-06-hacking-with-aspects.image-0d588cd6d9cd.webp
    width: 320
    height: 168
  - file: 2014-05-06-hacking-with-aspects.image-31cc19acc4ab.webp
    width: 640
    height: 336
  - file: 2014-05-06-hacking-with-aspects.image-d2e0f950603a.webp
    width: 960
    height: 504
  - file: 2014-05-06-hacking-with-aspects.image-e4f9bcdb6b92.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: http://f.cl.ly/items/0V1B2r1K0Z2Q2k0u1o1J/Screen%20Shot%202014-05-06%20at%2000.02.00%20.png
  original:
    file: 2014-05-06-hacking-with-aspects.image-db707704d819.png
    width: 360
    height: 873
  color: '#d2d5da'
---

I’ve recently spent a few days extracting and polishing the AOP code from [PSPDFKit](http://pspdfkit.com), and the result of this is called [Aspects - a delightful, simple library for aspect oriented programming.](https://github.com/steipete/Aspects)

Now Aspects is a great new tool in your toolkit. It allows to call code before, instead or after the original implementation, and there’s no need to manually call super, cast `objc_msgSend` or any of that other stuff you have to should do on swizzling. Use it with reason, it has a few great use cases, some are well-explaind on the GitHub page.

It’s also great for hacking and debugging. While testing the example on an iPad that still runs iOS 6, I found this exception: `// *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: 'On iPad, UIImagePickerController must be presented via UIPopoverController'`

![](http://f.cl.ly/items/0V1B2r1K0Z2Q2k0u1o1J/Screen%20Shot%202014-05-06%20at%2000.02.00%20.png)

Right, Apple fixed that in iOS 7. But I was more curious how this is actually implemented. It’s actually quite tricky to detect if you are inside a popover or not, and sometimes this quite important to know. Has Apple some “secret sauce” they’re using here? I opened Hopper to find out.

That’s roughly their code, converted back from assembly. Interesting that there’s a `_UIImagePickerControllerAllowAnySuperview` to disable this check. You have to wonder where they are using that… The check is otherwise quote straightforward. The interesting part is here: `[_UIPopoverView popoverViewContainingView:self.view]`.

Let’s look up that as well…

Ha. **There’s no secret sauce here.** Apple is simply iterating the view hierarchy to find the `_UIPopoverView`. Fair enough, it’s a simple solution. Sadly there’s no `_UIPopoverView` for us mere mortals, it’s a private class.

Now, let’s test if this disassembly is actually correct! First, we’ll disable Apple’s check:

That’s all - this makes the controller work perfectly where it threw an exception before. The popover restriction was a pure could be a political one, or there are edge cases we don’t know.

## Putting it all together

Now, we want to implant our own check using Aspects. `PLLibraryView` is again private, so we’ll use a runtime class lookup to hook it globally. I also commented out the property check since this would disable our own checking code.

That’s it!

This code isn’t of much use, but it’s interesting how Apple checks these things internally, and that their popover detection really is just subview querying. And while `_UIPopoverView` is private, we could easily make this check working without flagging private API by testing for parts of the class name…
