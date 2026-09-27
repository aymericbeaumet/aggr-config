---
title: Fixing What Apple Doesn't
link: https://steipete.me/posts/2014/fixing-what-apple-doesnt/
source: steipete-me
published: 2014-01-04T15:09:00Z
updated: 2014-01-04T15:09:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Fix the misaligned label in iOS 7's printer controller by swizzling UIPrinterSearchingView's layoutSubviews method.
content: extracted
html: 2014-01-04-fixing-what-apple-doesn-t.html
preview:
  file: 2014-01-04-fixing-what-apple-doesn-t.preview-7c974c5d8338.webp
  width: 256
  height: 134
  color: '#efeded'
images:
- source: https://steipete.me/posts/2014/fixing-what-apple-doesnt/index.png
  original:
    file: 2014-01-04-fixing-what-apple-doesn-t.image-23b9a80b69a0.png
    width: 1200
    height: 630
  variants:
  - file: 2014-01-04-fixing-what-apple-doesn-t.image-b8f4f83faeed.webp
    width: 320
    height: 168
  - file: 2014-01-04-fixing-what-apple-doesn-t.image-9b0875262089.webp
    width: 640
    height: 336
  - file: 2014-01-04-fixing-what-apple-doesn-t.image-cd957e67f4e3.webp
    width: 960
    height: 504
  - file: 2014-01-04-fixing-what-apple-doesn-t.image-209af275cdef.webp
    width: 1200
    height: 630
  color: '#fdfafa'
---

It’s one of those days where Apple’s sloppiness on iOS 7 is driving me nuts. Don’t get me wrong; I have a lot of respect in pulling off something as big as iOS 7 in such a short amount of time. It’s just that I see what’s coming in iOS 7.1 and so many annoyances of iOS 7 still aren’t fixed.

> Can’t stand Apple’s missing attention to detail in iOS 7. I’m just going to hack and patch this myself. [pic.twitter.com/Nnd176rPlz](http://t.co/Nnd176rPlz)
>
> — Peter Steinberger (@steipete) [January 4, 2014](https://twitter.com/steipete/statuses/419462996617097216)

No, I’m not talking about the offset arrow, [the background](https://twitter.com/steipete/status/419463332190781440) — I already made peace with that. But the offset label just looks like crap. (rdar://15748568) And since it’s still there in iOS 7.1b2, let’s fix that.

First off, we need to figure out how the class is called. We already know that it’s inside the printer controller. A small peak with Reveal is quite helpful:

![](https://steipete.me/images/posts/UIPrinterSearchingView.png)

So `UIPrinterSearchingView` is the culprit. Some more inspection shows that it’s fullscreen and the internal centering code is probably just broken or was somehow hardcoded. Let’s swizzle `layoutSubviews` and fix that. [When looking up the class via the iOS-Runtime-Headers, it seems quite simple](https://github.com/nst/iOS-Runtime-Headers/blob/d4cb1012a73d8126ab51fa951d4b4150e4c2d115/Frameworks/UIKit.framework/UIPrinterSearchingView.h), so our surgical procedure should work out fine:

Let’s run this again:

> Ah, much better. [pic.twitter.com/8yqoa6lrXU](http://t.co/8yqoa6lrXU)
>
> — Peter Steinberger (@steipete) [January 4, 2014](https://twitter.com/steipete/statuses/419469468562366464)

Done! Now obviously this is a bit risky — things could look weird if Apple greatly changes this class in iOS 8, so we should test the betas and keep an eye on this. But the code’s written defensively enough that it should not crash. I’m using some internal helpers from [PSPDFKit](http://pspdfkit.com) that should be obvious to rewrite — comment on the gist if you need more info.

Best thing: The code just won’t change anything if Apple ever decides to properly fix this.

New posts, shipping stories, and nerdy links straight to your inbox.

2× per month, pure signal, zero fluff.
