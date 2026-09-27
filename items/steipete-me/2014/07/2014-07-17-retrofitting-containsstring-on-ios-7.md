---
title: 'Retrofitting containsString: on iOS 7'
link: https://steipete.me/posts/2014/retrofitting-containsstring-on-ios-7/
source: steipete-me
published: 2014-07-17T23:40:00Z
updated: 2014-07-17T23:40:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Backport iOS 8''s convenient NSString containsString: method to iOS 7 using runtime patching that won''t conflict with Apple''s implementation.'
content: extracted
html: 2014-07-17-retrofitting-containsstring-on-ios-7.html
preview:
  file: 2014-07-17-retrofitting-containsstring-on-ios-7.preview-31b0a8a2e33e.webp
  width: 256
  height: 134
  color: '#ebe8e8'
images:
- source: https://steipete.me/posts/2014/retrofitting-containsstring-on-ios-7/index.png
  original:
    file: 2014-07-17-retrofitting-containsstring-on-ios-7.image-4f9cff935b77.png
    width: 1200
    height: 630
  variants:
  - file: 2014-07-17-retrofitting-containsstring-on-ios-7.image-0bdb948c038d.webp
    width: 320
    height: 168
  - file: 2014-07-17-retrofitting-containsstring-on-ios-7.image-fac79d9854c1.webp
    width: 640
    height: 336
  - file: 2014-07-17-retrofitting-containsstring-on-ios-7.image-4a02d8925d15.webp
    width: 960
    height: 504
  - file: 2014-07-17-retrofitting-containsstring-on-ios-7.image-7abf4c344b88.webp
    width: 1200
    height: 630
  color: '#fdfafa'
---

[Daniel Eggert](https://twitter.com/danielboedewadt) asked me on Twitter what’s the best way to retrofit the new `containsString:` method on `NSString` for iOS 7. Apple quietly added this method to Foundation in iOS 8 - it’s a small but great addition and reduces common code ala `[path rangeOfString:@"User"].location != NSNotFound` to the more convenient and readable `[path containsString:@"User"]`.

Of course you *could* always add that via a category, and in this case everything would probably work as expected, but we really want a *minimal invasive solution* that only patches the runtime on iOS 7 (or below) and doesn’t do anything on iOS 8 or any future version where this is implemented.

This code is designed in a way where it won’t even be compiled if you raise the minimum deployment target to iOS 8. Using `__attribute__((constructor))` is generally considered bad, but here it’s a minimal invasive addition for a legacy OS and we also want this to be called very early, so it’s the right choice.

New posts, shipping stories, and nerdy links straight to your inbox.

2× per month, pure signal, zero fluff.
