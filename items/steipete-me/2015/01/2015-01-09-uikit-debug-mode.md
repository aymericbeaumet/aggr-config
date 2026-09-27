---
title: UIKit Debug Mode
link: https://steipete.me/posts/2015/uikit-debug-mode/
source: steipete-me
published: 2015-01-09T15:05:00Z
updated: 2015-01-09T15:05:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Unlock hidden UIKit debugging features by bypassing Apple's InternalBuild checks to access secret flags for logging touches, gestures, and animations.
content: extracted
html: 2015-01-09-uikit-debug-mode.html
preview:
  file: 2015-01-09-uikit-debug-mode.preview-3be0aa56211c.webp
  width: 256
  height: 134
  color: '#f1efef'
images:
- source: https://steipete.me/posts/2015/uikit-debug-mode/index.png
  original:
    file: 2015-01-09-uikit-debug-mode.image-7dd9d609308f.png
    width: 1200
    height: 630
  variants:
  - file: 2015-01-09-uikit-debug-mode.image-99915cc7278a.webp
    width: 320
    height: 168
  - file: 2015-01-09-uikit-debug-mode.image-1e8a570b1aaa.webp
    width: 640
    height: 336
  - file: 2015-01-09-uikit-debug-mode.image-f09a0e10359d.webp
    width: 960
    height: 504
  - file: 2015-01-09-uikit-debug-mode.image-315c9ddeb98a.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2015/uikit-debug-mode/popover-rect-debug.png
  original:
    file: 2015-01-09-uikit-debug-mode.image-29295a64d5e8.png
    width: 1318
    height: 1176
  color: '#d8d8d8'
---

A while ago, I’ve [stumbled on a string called `UIPopoverControllerPaintsTargetRect`](https://twitter.com/steipete/status/546976070512435200) in some UIKit disassembly - definitely worth investigating! Now that I finally own [IDA](https://www.hex-rays.com/products/ida/), I did some research. Turns out there’s a hidden preferences file under `/Library/Preferences/com.apple.UIKit` that UIKit queries for these settings.

I’ve used [Aspects](https://github.com/steipete/Aspects) to swizzle `NSUserDefaults` and enable this key when queried. This actually works, but only under iOS 7, since iOS 8 uses the newer `UIPopoverPresentationController` and that one doesn’t fully support target rect drawing (or it’s compiled out in our release version of UIKit.)

![UIKit Debug Mode showing purple overlay on the popover target rectangle](https://steipete.me/assets/img/2015/uikit-debug-mode/popover-rect-debug.png)

(Screenshot from [PSPDFKit - the Leading iOS PDF Framework](http://pspdfkit.com). Note the purple overlay on the bookmark icon.)

Digging deeper, I found a bunch of very interesting and useful flags for debugging, which print extensive logging for touches, gestures, animations and more. I’ve listed the most interesting flags in the gist at the end of this article.

The process was easy for `UIPopoverControllerPaintsTargetRect` but quite a bit harder for most other flags, as these are protected by a check to `CPIsInternalDevice()` which lives in the private `AppSupport.framework`. All it does is query [libMobileGestalt](http://iphonedevwiki.net/index.php/LibMobileGestalt.dylib) for a few settings; checking if `"InternalBuild"` is true or if alternatively `"Oji6HRoPi7rH7HPdWVakuw"` is set to YES.

I’ve tried to use `dlsym` to get [`MGSetAnswer()`](https://github.com/Cykey/ios-reversed-headers/blob/master/MobileGestalt/MobileGestalt.h) and set the values manually, however this doesn’t work - it seems that only a few values are modifiable here. So instead, I’ve used [Facebook’s fishhook](https://github.com/facebook/fishhook) to redirect all calls from `MGGetBoolAnswer` and manually return YES if it’s queried for “InternalBuild”. Granted, we could also hook `CPIsInternalDevice` instead; both will work.

Want to try for yourself? Check out my [UIKitDebugging repository](https://github.com/steipete/UIKitDebugging) and add all files to your repository. Remember, that’s just for debugging and to satisfy your curiosity, don’t ship any of that.

Here’s some interesting output. The touch and gesture logging seems very useful.

There are a few other interesting flags like the infamous `UISimulatedApplicationResizeGestureEnabled`, I’ve only listed the most interesting ones in the gist…
