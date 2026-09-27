---
title: zld — A Faster Version of Apple's Linker
link: https://steipete.me/posts/2020/zld-a-faster-linker/
source: steipete-me
published: 2020-06-05T08:00:00Z
updated: 2020-06-05T08:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: How to speed up iOS build times by 40% using zld, a drop-in replacement for Apple's linker, with practical integration tips for real projects.
content: extracted
html: 2020-06-05-zld-a-faster-version-of-apple-s-linker.html
preview:
  file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.preview-60f38be9351a.webp
  width: 256
  height: 134
  color: '#ebe8e8'
images:
- source: https://steipete.me/posts/2020/zld-a-faster-linker/index.png
  original:
    file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-d480343a5f3b.png
    width: 1200
    height: 630
  variants:
  - file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-db160668a102.webp
    width: 320
    height: 168
  - file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-c2f6a47f3623.webp
    width: 640
    height: 336
  - file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-3d480bc561fe.webp
    width: 960
    height: 504
  - file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-2ae71eaa008a.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2020/zld/benchmarks.png
  original:
    file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-3a49c8175f6c.png
    width: 1200
    height: 742
  variants:
  - file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-75c071a0d333.webp
    width: 320
    height: 198
  - file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-01974c19e3a3.webp
    width: 640
    height: 396
  - file: 2020-06-05-zld-a-faster-version-of-apple-s-linker.image-4e406f83bbbd.webp
    width: 1200
    height: 742
  color: '#fdfdfd'
---

![](https://steipete.me/assets/img/2020/zld/benchmarks.png)

zld is [a drop-in replacement of Apple’s linker](https://github.com/michaeleisel/zld) that uses optimized data structures and parallelizing to speed things up. It comes with a great promise:

> “Feel free to file an issue if you find it’s not at least 40% faster for your case” — [Michael Eisel, Maintainer](https://github.com/michaeleisel)

In our setup, zld indeed improves overall build time by approximately 25 percent, measured from a clean build to the running application. Building [PSPDFCatalog](https://pspdfkit.com/guides/ios/current/getting-started/example-projects/) in debug mode with [ccache](https://pspdfkit.com/blog/2015/ccache-for-fun-and-profit/) enabled and everything precached takes roughly:

- ld — 4:40min
- zld — 3:30min

If you’re asking yourself, is this safe? Well, [Instagram uses it too](https://twitter.com/alanzeino/status/1268230184215252992?s=21).

Heads up: `zld` seems to [cause issues when using the Swift trunk toolchain](https://steipete.me/posts/building-with-swift-trunk/).

## Installation

zld is easy to enable for your project:

1. `brew install michaeleisel/zld/zld`
2. `OTHER_LDFLAGS = -fuse-ld=/usr/local/bin/zld`

In our setup, things aren’t quite so easy, as we have a few additional requirements:

- The build should work independently of `zld` installed, so people can opt in on their own and don’t have a surprising build failure after pulling master. This is even truer for CI.
- We have a large [monorepo](https://pspdfkit.com/blog/2019/benefits-of-a-monorepo/) with different projects in different folders, which is managed by shared `xcconfig` files.

I wrote a `zld-detect` wrapper that conditionally forwards to `zld` if found. Otherwise, it uses Apple’s default linker:

```plaintext
#!/bin/sh

# /usr/local/bin is not always included in the Xcode context.
export PATH="$PATH:/usr/local/bin"

# Detect if zld is available.
if type -p zld >/dev/null 2>&1; then
  exec zld "$@"
else
  exec ld "$@"
fi
```

The second problem (different paths) was solved by defining a `REPOROOT = "$(SRCROOT)/../..";` in each project, so that we could build a path from the root of the monorepo and only have one location for the `zld-detect` script:

```plaintext
OTHER_LDFLAGS = -ObjC -Wl,-no_uuid -fuse-ld=$(REPOROOT)/iOS/Resources/zld-detect
```

## Mac Catalyst

After implementing the above, our Mac Catalyst builds started failing:

```plaintext
Building for Mac Catalyst, but linking in .tbd built for , file '/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/System/Library/Frameworks//CoreImage.framework/CoreImage.tbd' for architecture x86_64
```

It seems there’s some special code in the linker that helps with linking the correct framework for Mac Catalyst, which isn’t yet part of the v510 release. Apple released [v520 and v530 of the ld64 project](https://opensource.apple.com/source/ld64/), so there’s a good chance this will be fixed once `zld` merges with upstream ([Issue #43](https://github.com/michaeleisel/zld/issues/43)).

Writing this conditionally in `xcconfig` is tricky, as there’s no support for a separate architecture like `[sdk=maccatalyst]` (Apple folks: FB6822740).

Here’s how things look if we put everything together:

```plaintext
// Settings to improve link time performance for debug/test builds
// https://github.com/michaeleisel/zld#a-faster-version-of-apples-linker
// Linker fails for Mac Catalyst — maybe try once it's updated to v530.
// This is always defined as YES or NO.
PSPDF_ZLD = -fuse-ld=$(REPOROOT)/iOS/Resources/zld-detect
PSPDF_LINKER_MACCATALYST_YES = ""
PSPDF_LINKER_MACCATALYST_NO = $(PSPDF_ZLD)
// This will be the case on iOS.
PSPDF_LINKER_MACCATALYST_ = $(PSPDF_ZLD)
PSPDF_LINKER_IF_NOT_CATALYST = $(PSPDF_LINKER_MACCATALYST_$(IS_MACCATALYST))
PSPDF_NORELEASE_LDFLAGS = -ObjC -Wl,-no_uuid $(PSPDF_LINKER_IF_NOT_CATALYST)
```

In `Defaults-Debug.xcconfig` and `Defaults-testing.xcconfig`

```plaintext
// Use fast linker if available.
OTHER_LDFLAGS = $(inherited) $(PSPDF_NORELEASE_LDFLAGS)
```

Update: Xcode also supports the [`LD`](https://twitter.com/thi_dt/status/1268848373953474560) `xcconfig` key to make this even easier to configure.

That’s it! [Let me know on Twitter](https://twitter.com/steipete) if this was helpful.
