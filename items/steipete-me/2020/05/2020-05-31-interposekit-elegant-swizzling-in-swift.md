---
title: InterposeKit — Elegant Swizzling in Swift
link: https://steipete.me/posts/2020/interposekit/
source: steipete-me
published: 2020-05-31T10:00:00Z
updated: 2020-05-31T10:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Introducing InterposeKit, a modern Swift library for elegant method swizzling that leverages Swift 5.2's callAsFunction for type-safe Objective-C method interception.
content: extracted
html: 2020-05-31-interposekit-elegant-swizzling-in-swift.html
preview:
  file: 2020-05-31-interposekit-elegant-swizzling-in-swift.preview-d8e6ffd738c0.webp
  width: 256
  height: 134
  color: '#eae7e7'
images:
- source: https://steipete.me/posts/2020/interposekit/index.png
  original:
    file: 2020-05-31-interposekit-elegant-swizzling-in-swift.image-da42ae4cd57c.png
    width: 1200
    height: 630
  variants:
  - file: 2020-05-31-interposekit-elegant-swizzling-in-swift.image-5d79995b8ea9.webp
    width: 320
    height: 168
  - file: 2020-05-31-interposekit-elegant-swizzling-in-swift.image-c19cafd09998.webp
    width: 640
    height: 336
  - file: 2020-05-31-interposekit-elegant-swizzling-in-swift.image-3c4147b96cbd.webp
    width: 960
    height: 504
  - file: 2020-05-31-interposekit-elegant-swizzling-in-swift.image-60f8f02cc8a9.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2020/interposekit/logo.png
  original:
    file: 2020-05-31-interposekit-elegant-swizzling-in-swift.image-95b1da84c693.png
    width: 1355
    height: 320
  color: '#fefdfd'
- source: https://steipete.me/assets/img/2020/interposekit/interposekit-code.png
  original:
    file: 2020-05-31-interposekit-elegant-swizzling-in-swift.image-212ecb4abcae.png
    width: 1534
    height: 720
  color: '#292a2f'
---

![](https://steipete.me/assets/img/2020/interposekit/logo.png)

I built a thing! [InterposeKit](https://github.com/steipete/InterposeKit) is a modern library for elegantly swizzling in Swift. It’s on GitHub, fully written in Swift 5.2+, and works on `@objc dynamic` Swift functions or Objective-C instance methods.

The inspiration for InterposeKit was [a race condition in Mac Catalyst](https://steipete.me/posts/mac-catalyst-crash-hunt/), which required some tricky swizzling to fix. With InterposeKit, this is now much cleaner. Since everything’s [explained much better on the project website](http://interposekit.com/), I’m just writing some random thoughts on building this that didn’t fit into the README.

## GitHub Actions

Wow! I didn’t have the time to play with this before, but damn is it 💖 good. GitHub Actions is fast, easy to set up, reliable, and superbly well integrated — all the way down to automatic badges for CI state.

There are a few annoyances, like not being able to run Docker containers on macOS (this isn’t technical, just [a money thing](https://github.community/t/why-is-docker-not-installed-on-macos/17017/2)). The default Jazzy setup to generate documentation runs via Docker, so I had to [jump through some hoops](https://github.com/steipete/InterposeKit/blob/85f6c2dcc465811048cac0b31c4edc8bb71d4268/Sources/InterposeKit/InterposeKit.swift#L305-L319) to make my project compile on Linux.

## Swift and Type Aliases

![](https://steipete.me/assets/img/2020/interposekit/interposekit-code.png)

It’s extremely unfortunate that the `@convention()` modifier can’t be used on existing type aliases — this would have made Interpose way more convenient. I’m honestly tempted to write a proposal to get this into Swift because it would be cool and I’d be really interested in the learning experience.

> [View post on X](https://twitter.com/i/web/status/1266799174563041282)

## Swift Package Manager

I finally watched [WWDC2019:410 Creating Swift Packages](https://developer.apple.com/videos/play/wwdc2019/410/) and [WWDC2019:408 Adopting Swift Packages in Xcode](https://developer.apple.com/videos/play/wwdc2019/408/) (Hi Boris!) and I really like SwiftPM. Yes, there’s a still a lot to do, but it’s getting [Package Resources (SE-0271)](https://github.com/apple/swift-evolution/blob/master/proposals/0271-package-manager-resources.md) with Xcode 12, and the integration with GitHub is nice. Not being allowed to [delete `DerivedData` anymore](https://www.jessesquires.com/blog/2020/02/24/replacing-cocoapods-with-swiftpm/) will be difficult though.

## Class Load Events

`objc_addLoadImageFunc` is a [big no-no](https://twitter.com/steipete/status/1266464092082114561?s=21), and it probably [shouldn’t even exist in the header](https://twitter.com/jckarter/status/1266466247748677632?s=21) at all. However, there’s `_dyld_register_func_for_add_image`, which is great. This includes a C callback, and while Swift does a really good job of making it just blend into the language, this callback is not a block and it can’t capture state. I eventually found out that I can put everything [into a struct, as long as it’s all static](https://github.com/steipete/InterposeKit/blob/85f6c2dcc465811048cac0b31c4edc8bb71d4268/Sources/InterposeKit/InterposeKit.swift#L259-L293), in order to not have this pollute my global namespace.

Why would I need this? There was [a particular bug in Mac Catalyst](https://steipete.me/posts/mac-catalyst-crash-hunt/) that required such a trick.

## Swift 5.2 callAsFunction

Well well well… here I was [bitchin’](https://twitter.com/steipete/status/1227191768153829376?s=20) about Swift getting useless features, only to be extremely happy about `callAsFunction` a few months later. In InterposeKit, I use it to have a shorthand for calling the original implementation of a function. [It even does generics](https://github.com/steipete/InterposeKit/blob/85f6c2dcc465811048cac0b31c4edc8bb71d4268/Sources/InterposeKit/InterposeKit.swift#L175-L178)!

## imp\_implementationWithBlock

`imp_implementationWithBlock` has [no way to undo](https://github.com/steipete/InterposeKit/blob/85f6c2dcc465811048cac0b31c4edc8bb71d4268/Sources/InterposeKit/InterposeKit.swift#L130) or deregister the IMP, so once you submit a block that captures state, you have a permanent memory leak? Oh well.

## Closing Thoughts

This is my first Swift-specific open source project, apart from the usual gists. I’d like to learn, so please: BE harsh on me. I had a lot of fun and built this in a weekend. It helped me forget time and space (and current world events) for a bit and just tinker. I’m also sure I got things wrong, so please do tell me what can be made better.
