---
title: UITableViewController designated initializer woes
link: https://steipete.me/posts/2015/uitableviewcontroller-designated-initializer-woes/
source: steipete-me
published: 2015-04-15T09:15:00Z
updated: 2015-04-15T09:15:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Navigate the complications of subclassing UITableViewController after iOS 8.3 introduced designated initializers that break proper initialization patterns.
content: extracted
html: 2015-04-15-uitableviewcontroller-designated-initializer-woes.html
preview:
  file: 2015-04-15-uitableviewcontroller-designated-initializer-woes.preview-74f3c649401f.webp
  width: 256
  height: 134
  color: '#e6e4e4'
images:
- source: https://steipete.me/posts/2015/uitableviewcontroller-designated-initializer-woes/index.png
  original:
    file: 2015-04-15-uitableviewcontroller-designated-initializer-woes.image-49cc59685ba4.png
    width: 1200
    height: 630
  variants:
  - file: 2015-04-15-uitableviewcontroller-designated-initializer-woes.image-8d074bf46858.webp
    width: 320
    height: 168
  - file: 2015-04-15-uitableviewcontroller-designated-initializer-woes.image-57a39bf36f1a.webp
    width: 640
    height: 336
  - file: 2015-04-15-uitableviewcontroller-designated-initializer-woes.image-a57d5f99aedb.webp
    width: 1200
    height: 630
  color: '#fdfafa'
---

With Xcode 6 [1](https://steipete.me/posts/2015/uitableviewcontroller-designated-initializer-woes/#footnote1), Apple added support for the `NS_DESIGNATED_INITIALIZER` flag, and also added this to various framework classes. This is likely a byproduct of Swift, where the initializer call order is [much more strongly enforced](http://www.codingexplorer.com/designated-initializers-convenience-initializers-swift/). This goes as far as there’s a new `convenience` keyword to mark the separation of convenience vs designated initializers.

This is a good thing. It’s far too easy to break the initializer chain, even though [Apple’s documentation on it is superb](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/Initialization/Initialization.html).

With iOS 8.3, Apple made a modification in `UITableViewController` that by now, every one using this class will have seen. `initWithStyle:` is now a designated initializer. And while throughout the beta period, this was the only designated one, the GM suddenly added both `initWithNibName:bundle:` and `initWithCoder:` to the list as well - making this class quite inconvenient to subclass.

Most of your subclasses will have their own designated initializer since they likely depend on some object at initialization time. If that’s the case, you generally want to prevent users from calling the *wrong* initializer, even if they are marked designated in the superclass.

A common idiom to do this is to declare them unavailable:

The above code belongs to the header and results in compile-time warnings. Since Objective-C is dynamic, this makes calling `init` harder, but does not prevent you from shooting yourself in the foot. To be complete, let’s also block this in the implementation:

Combining the two makes it really hard to create objects that are not correctly initialized. So of course I also tried to apply this pattern to `UITableViewController`… which results in an assert. Here’s the designated initializer chain I’d expect:

`[PSPDFTableViewController initWithAnnotations:]` -> `[UITableViewController initWithStyle:]` -> `[UIViewController initWithNibName:bundle:]` -> `[NSObject init]`.

However, Apple didn’t really play by the rules in `UITableViewController`. It calls `[super init]` inside `initWithStyle:`. `init` is overridden in `UIViewController` to call `initWithNibName:bundle:` since this is the designated initializer per documentation, even though it’s not annotated with the `NS_DESIGNATED_INITIALIZER` decoration. This results in following call order:

`[PSPDFTableViewController initWithAnnotations:]` -> `[UITableViewController initWithStyle:]` -> `[UIViewController init]` -> `[PSPDFTableViewController initWithNibName:bundle:]` (which asserts!).

This is very unfortunate, and I assume it’s not easy to correct since there surely are apps out there who rely on this call order. We work around this by wrapping our implementation into a clang diagnostic block to ignore “-Wobjc-designated-initializers”, but it doesn’t prevent anyone from creating the controller with an invalid state. Maybe Apple [fixes this conditionally in iOS 9](https://twitter.com/steipete/status/587374783614083072). [rdar://problem/20549233.](http://www.openradar.me/20549233)

What other ways are there do deal with it? Did I miss something here?

* * *

1. To be correct, \[Clang commit r196314\](http://lists.cs.uiuc.edu/pipermail/cfe-commits/Week-of-Mon-20131202/094628.html) landed in Xcode 5.1, so technically this already had support for designated initializers by using the \`objc\_designated\_initializer\` attribute directly.
