---
title: Building with Swift Trunk Development Snapshots
link: https://steipete.me/posts/2020/building-with-swift-trunk/
source: steipete-me
published: 2020-06-12T15:00:00Z
updated: 2020-06-12T15:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: A troubleshooting guide for building with Swift trunk development snapshots, documenting compilation errors, linker issues, and their solutions.
content: extracted
html: 2020-06-12-building-with-swift-trunk-development-snapshots.html
preview:
  file: 2020-06-12-building-with-swift-trunk-development-snapshots.preview-9d672ff1b537.webp
  width: 256
  height: 134
  color: '#e7e4e4'
images:
- source: https://steipete.me/posts/2020/building-with-swift-trunk/index.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-e00eb16c79b9.png
    width: 1200
    height: 630
  variants:
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-577d7a30a332.webp
    width: 320
    height: 168
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-c9d363a807f2.webp
    width: 640
    height: 336
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-18495b97d21f.webp
    width: 960
    height: 504
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-773a151514f7.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2020/swift-trunk/swift-trunk.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-660c35af0d39.png
    width: 1548
    height: 790
  color: '#fdfdfd'
- source: https://steipete.me/assets/img/2020/swift-trunk/isOSVersion.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-dea1df64de90.png
    width: 1858
    height: 390
  variants:
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-89e885172669.webp
    width: 320
    height: 67
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-f6d60b8342ae.webp
    width: 640
    height: 134
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-a471275eed5d.webp
    width: 960
    height: 202
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-2fa6f862fefd.webp
    width: 1280
    height: 269
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-2ca0a8aeff67.webp
    width: 1600
    height: 336
  - file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-549af7242198.webp
    width: 1858
    height: 390
  color: '#323232'
- source: https://steipete.me/assets/img/2020/swift-trunk/not-macho.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-8550616b67d1.png
    width: 2184
    height: 462
  color: '#323232'
- source: https://steipete.me/assets/img/2020/swift-trunk/gesture.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-a28f4faed165.png
    width: 1180
    height: 1054
  color: '#fbfcfb'
- source: https://steipete.me/assets/img/2020/swift-trunk/xcodecrash.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-5cb1044838cd.png
    width: 3804
    height: 2386
  color: '#fcfcfc'
- source: https://steipete.me/assets/img/2020/swift-trunk/buildsystem.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-3ffe43998345.png
    width: 630
    height: 346
  color: '#35373a'
- source: https://steipete.me/assets/img/2020/swift-trunk/swift-catalyst-crash.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-9c01bfdfc4d2.png
    width: 2062
    height: 1384
  color: '#474747'
- source: https://steipete.me/assets/img/2020/swift-trunk/catalyst-objc.png
  original:
    file: 2020-06-12-building-with-swift-trunk-development-snapshots.image-3f9a538f3d1c.png
    width: 1188
    height: 832
  color: '#555557'
---

![](https://steipete.me/assets/img/2020/swift-trunk/swift-trunk.png)

I recently started the adventure of building PSPDFKit with the [Swift trunk development snapshot](https://swift.org/download/). I did this both in order to verify a fix for the [SR-12933 LLDB debugging issue](https://steipete.com/posts/couldnt-irgen-expression/) and to be better prepared for the Xcode 12 release at WWDC.

I’m documenting my adventure with the June 10 Swift trunk toolchain — may it help Google warriors, as some of the errors didn’t yield any useful results. Let’s skip the download-install-select-in-Xcode part and go straight to the issues.

## libclang\_rt.profile\_iossim.a Not Found

You might see the following error early on:

```plaintext
File not found: /Library/Developer/Toolchains/swift-DEVELOPMENT-SNAPSHOT-2020-06-09-a.xctoolchain/usr/lib/clang/10.0.0/lib/darwin/libclang_rt.profile_iossim.a
```

Then you might run into a variant of [SR-12001](https://bugs.swift.org/browse/SR-12001) and need to copy some files into the toolchain that are not shipped with it by default:

```plaintext
sudo cp `xcode-select -p`/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/*/lib/darwin/libclang_rt.*.a /Library/Developer/Toolchains/swift-DEVELOPMENT-SNAPSHOT-2020-06-09-a.xctoolchain/usr/lib/clang/10.0.0/lib/darwin
```

## Undefined Symbol: \_isOSVersionAtLeast

![](https://steipete.me/assets/img/2020/swift-trunk/isOSVersion.png)

Initially I missed a few files (I only copied `libclang_rt.profile_iossim.a`) and then got yet a different error: `Undefined symbols for architecture arm64: "___isOSVersionAtLeast"`.

Run the above `cp` command to fix.

## Uncaught Exception of Type tbb::captured\_exception

Next up, the linker crashed:

```plaintext
BB Warning: Exact exception propagation is requested by application but the linked library is built without support for it

libc++abi.dylib: terminating with uncaught exception of type tbb::captured_exception: Unidentified exception
```

This turned out to be [zld](https://steipete.me/posts/zld-a-faster-linker/) — removing the `xcconfig` setting solved this. This will be fixed eventually, as Apple’s ld is open source and zld is just a faster fork. I [documented this in the Swift Forum](https://forums.swift.org/t/swift-toolchain-fails-to-compile-with-tbb-unidentified-exception/37434) for others to find.

## Archive Member with Length Is Not Mach-O or LLVM Bitcode

This one took me a while! We are using different configurations in PSPDFKit, so some lower-level parts compile with our release configuration and some compile with debug (you still want a fast PDF render experience when working on the UI).

![](https://steipete.me/assets/img/2020/swift-trunk/not-macho.png)

This seems to be a bug; there’s no reason this should fail, as it’s perfectly OK to compile different objects with different optimizer settings. Changing this to be the same settings everywhere did work around the issue.

## Missing APINotes

Apple uses [API notes](https://pspdfkit.com/blog/2018/first-class-swift-api-for-objective-c-frameworks/) to make the mapping from Objective-C to Swift easier. They are currently to a degree tightly coupled with Clang itself, and as the [`UIPointerInteraction`](https://pspdfkit.com/blog/2020/supporting-pointer-interactions/) API is very new, these notes haven’t been upstreamed yet — so we get a different, non-optimized API in Swift when compiling with trunk.

![](https://steipete.me/assets/img/2020/swift-trunk/gesture.png)

To conditionally disable Swift code, I used an `#ifdef` and set the following in our `xcconfig` file:

```plaintext
OTHER_SWIFT_FLAGS = -D SWIFT_TRUNK_TOOLCHAIN_WORKAROUND
```

This still disables the code and isn’t a real fix, but I assume Apple will eventually update trunk to include these API notes.

## libLTO.dylib Could Not Be Loaded

If you get an `libLTO.dylib could not be loaded` error, you might have a setup where Link Time Optimization is enabled in at least one of your projects. I fixed this by making sure LTO is off everywhere:

```plaintext
LLVM_LTO = NO
```

## Xcode and Xcode Build System Crash

To be honest, I’m just adding this for the sake of completeness. This happens at random times and isn’t really related to Swift trunk. A restart fixes it.

![](https://steipete.me/assets/img/2020/swift-trunk/xcodecrash.png)

Build system crashes are especially fun because Xcode doesn’t crash, but you still have to quit and restart it manually. One could make the argument that it would be more convenient if Xcode would simply crash as well.

![](https://steipete.me/assets/img/2020/swift-trunk/buildsystem.png)

## Swift Compiler Crash: Impossible SILDeclRef

Compiling for iOS now worked, but Catalyst crashed with the following error: `impossible SILDeclRef loc UNREACHABLE executed at /Users/buildnode/jenkins/workspace/oss-swift-package-osx/swift/lib/SIL/IR/SILDeclRef.cpp:155!`.

![](https://steipete.me/assets/img/2020/swift-trunk/swift-catalyst-crash.png)

Turns out this is already known and happens any time you run this compiler with a `Swift.swiftinterface` from the SDK. It will be [fixed in a few days](https://twitter.com/slava_pestov/status/1271150466404155399).

## Catalyst Warning

We expose some internal Swift classes to Objective-C that are only available in newer versions of the OS. Pointer library methods have been added to Catalyst, and we declare this availability for both iOS and Catalyst, but the trunk version doesn’t know about the `maccatalyst` platform.

![](https://steipete.me/assets/img/2020/swift-trunk/catalyst-objc.png)

The only way to get rid of this error seems to be to remove the define, but since this is only a warning, I skipped that.

## Conclusion

Using the [Swift trunk toolchain](https://swift.org/download/) is a rocky road, and I can only recommend this if you feel adventurous or really want to help Apple verify a bug fix. However, I appreciate that Apple provides prebuilt packages, and I’m sure the issues above will be ironed out eventually.
