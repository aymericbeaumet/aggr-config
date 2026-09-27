---
title: Fixing UISearchDisplayController On iOS 7
link: https://steipete.me/posts/2013/fixing-uisearchdisplaycontroller-on-ios-7/
source: steipete-me
published: 2013-10-04T19:00:00Z
updated: 2013-10-04T19:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Fix the broken animation, frame positioning, and status bar issues in UISearchDisplayController on iOS 7 with this comprehensive solution.
content: extracted
html: 2013-10-04-fixing-uisearchdisplaycontroller-on-ios-7.html
preview:
  file: 2013-10-04-fixing-uisearchdisplaycontroller-on-ios-7.preview-3e7a56018bca.webp
  width: 256
  height: 134
  color: '#eae7e7'
images:
- source: https://steipete.me/posts/2013/fixing-uisearchdisplaycontroller-on-ios-7/index.png
  original:
    file: 2013-10-04-fixing-uisearchdisplaycontroller-on-ios-7.image-dec444d02b3d.png
    width: 1200
    height: 630
  variants:
  - file: 2013-10-04-fixing-uisearchdisplaycontroller-on-ios-7.image-255db67a2c58.webp
    width: 320
    height: 168
  - file: 2013-10-04-fixing-uisearchdisplaycontroller-on-ios-7.image-64ffa77fc3de.webp
    width: 640
    height: 336
  - file: 2013-10-04-fixing-uisearchdisplaycontroller-on-ios-7.image-11570206c935.webp
    width: 960
    height: 504
  - file: 2013-10-04-fixing-uisearchdisplaycontroller-on-ios-7.image-7a7c11364478.webp
    width: 1200
    height: 630
  color: '#fdfafa'
---

iOS 7 is great, but it’s still very much a 1.0. I’ve spent a lot of time working around iOS 7-specific bugs in [PSPDFKit](http://pspdfkit.com) and will share some of my work here.

This is how UISearchDisplayController looks on iOS 7:

Your browser does not support the video tag.

Pretty bad, eh? This is how it should look:

Your browser does not support the video tag.

Here’s the code to fix it. It doesn’t use private API (although there is some view hierarchy fighting), and it’s fairly clean. It uses my [UIKit legacy detector](https://gist.github.com/steipete/6526860), so this code won’t do any harm on iOS 5/6:

I imagine one could subclass UISearchDisplayController directly and internally forward the delegate to better package this fix, but I only need it in one place so the UIViewController was a good fit.

Note that this uses some awful things like `dispatch_async(dispatch_get_main_queue()`, but it shouldn’t do any harm even if Apple fixes its controller sometime in the future.

New posts, shipping stories, and nerdy links straight to your inbox.

2× per month, pure signal, zero fluff.
