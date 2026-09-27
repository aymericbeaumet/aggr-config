---
title: Fixing UITextView On iOS 7
link: https://steipete.me/posts/2014/fixing-uitextview-on-ios-7/
source: steipete-me
published: 2014-01-08T20:03:00Z
updated: 2014-01-08T20:03:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Fix the severe scrolling and content positioning bugs in iOS 7's UITextView with PSPDFTextView, a drop-in replacement.
content: extracted
html: 2014-01-08-fixing-uitextview-on-ios-7.html
preview:
  file: 2014-01-08-fixing-uitextview-on-ios-7.preview-5d02abbb0706.webp
  width: 256
  height: 134
  color: '#efeded'
images:
- source: https://steipete.me/posts/2014/fixing-uitextview-on-ios-7/index.png
  original:
    file: 2014-01-08-fixing-uitextview-on-ios-7.image-009b696c6482.png
    width: 1200
    height: 630
  variants:
  - file: 2014-01-08-fixing-uitextview-on-ios-7.image-ec5371ed496e.webp
    width: 320
    height: 168
  - file: 2014-01-08-fixing-uitextview-on-ios-7.image-d98950f9da96.webp
    width: 640
    height: 336
  - file: 2014-01-08-fixing-uitextview-on-ios-7.image-9c522ad53adc.webp
    width: 960
    height: 504
  - file: 2014-01-08-fixing-uitextview-on-ios-7.image-b5bdba56398b.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://github.com/steipete/PSPDFTextView/raw/master/Example/broken.gif
  original:
    file: 2014-01-08-fixing-uitextview-on-ios-7.image-9d00436898c7.gif
    width: 405
    height: 387
  color: '#f8e48e'
- source: https://github.com/steipete/PSPDFTextView/raw/master/Example/fixed.gif
  original:
    file: 2014-01-08-fixing-uitextview-on-ios-7.image-3dc4a65583aa.gif
    width: 405
    height: 387
  color: '#f8e48e'
---

`UITextView` on iOS 7 is [a lot more powerful](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextView_Class/Reference/UITextView.html#//apple_ref/occ/instp/UITextView/linkTextAttributes), since Apple switched over from using [WebKit](http://www.cocoanetics.com/2012/12/uitextview-caught-with-trousers-down/) to [TextKit](https://developer.apple.com/library/ios/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/CustomTextProcessing/CustomTextProcessing.html) for rendering. It’s also very much a 1.0, and has some [rather terrible](http://inessential.com/2014/01/07/uitextview_scroll-to-typing_bug) [bugs](https://devforums.apple.com/message/918284#918284). In fact, they go so far that people started writing [replacements for the whole scrolling logic](https://github.com/jaredsinclair/JTSTextView).

Of course, people reported these issues in [PSPDFKit](http://pspdfkit.com) as well, so I had to find a workaround. I’m using `contentInset` when the keyboard (iPhone) or another view (iPhone/iPad) goes up, which is pretty much completely ignored by `UITextView` in iOS 7. This is frustrating mainly because it works perfectly in iOS 6.

At first, my solution was based on a category, but after discovering more and more needed hooks, I moved over to a subclass that [automatically forwards all delegate methods](https://github.com/steipete/PSPDFTextView/blob/ee9ce04ad04217efe0bc84d67f3895a34252d37c/PSPDFTextView/PSPDFTextView.m#L148-164). This has the advantage of more shared code, and we might be able to remove all those horrible hacks once iOS 8 comes out. I certainly hope so, and will write a few more radars.

So, what’s fixed in `PSPDFTextView`?

- When adding a new line, `UITextView` will now properly scroll down. Previously, you needed to add at least one character for this to happen.
- Scrolling to the caret position now considers `contentInset`. `UITextView` completely ignored this.
- Typing will also consider `contentInset` and will update the scroll position accordingly.
- Pasted text will scroll to the caret position.

![UITextView](https://github.com/steipete/PSPDFTextView/raw/master/Example/broken.gif)

![PSPDFTextView](https://github.com/steipete/PSPDFTextView/raw/master/Example/fixed.gif)

To enable these fixes, simply use `PSPDFTextView` instead of `UITextView`:

[https://github.com/steipete/PSPDFTextView](https://github.com/steipete/PSPDFTextView)

This is working quite well for my use case, but there surely are edge cases where this won’t be enough (like when using rich text). I also tried using the new `textContainerInset` but this didn’t work as intended and didn’t solve my scrolling problems.

I have to give credit to countless people who searched for the same solution — this very much was a community-oriented fix. Sadly, this doesn’t seem to be a priority for Apple, since it’s still broken in iOS 7.1b3.

Please fork the repo and send a pull request if you have any ideas on how to simplify the code or find an even better workaround.
