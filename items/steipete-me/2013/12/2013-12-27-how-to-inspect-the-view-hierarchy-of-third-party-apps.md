---
title: How To Inspect The View Hierarchy Of Third-Party Apps
link: https://steipete.me/posts/2013/how-to-inspect-the-view-hierarchy-of-3rd-party-apps/
source: steipete-me
published: 2013-12-27T18:42:00Z
updated: 2013-12-27T18:42:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to inspect view hierarchies of third-party iOS apps using a jailbroken device and debugging tools like Reveal for design insights.
content: extracted
html: 2013-12-27-how-to-inspect-the-view-hierarchy-of-third-party-apps.html
preview:
  file: 2013-12-27-how-to-inspect-the-view-hierarchy-of-third-party-apps.preview-b179961c0896.webp
  width: 256
  height: 134
  color: '#e5e3e3'
images:
- source: https://steipete.me/posts/2013/how-to-inspect-the-view-hierarchy-of-3rd-party-apps/index.png
  original:
    file: 2013-12-27-how-to-inspect-the-view-hierarchy-of-third-party-apps.image-7bc6cd929d65.png
    width: 1200
    height: 630
  variants:
  - file: 2013-12-27-how-to-inspect-the-view-hierarchy-of-third-party-apps.image-0470c1cca118.webp
    width: 320
    height: 168
  - file: 2013-12-27-how-to-inspect-the-view-hierarchy-of-third-party-apps.image-acdc31f5b229.webp
    width: 640
    height: 336
  - file: 2013-12-27-how-to-inspect-the-view-hierarchy-of-third-party-apps.image-f6227e6895be.webp
    width: 1200
    height: 630
  color: '#fdfafa'
---

I’m generally not a big fan of jailbreaks. Mostly this is because they’re used for piracy and all the hacks result in weird crashes that generally are impossible to reproduce. Still, [I was quite excited about the recent iOS 7 jailbreak](https://twitter.com/steipete/status/414759423102689281), since it enables us to attach the debugger to third-party apps and do a little bit of runtime analysis.

Why? Because it’s fun, and it can inspire you to solve things differently. Studying the view hierarchy of complex apps can be rather revealing and it’s interesting to see how others are solving similar problems. This was one thing that took many iterations to get right in [PSPDFKit, our iOS PDF framework](http://pspdfkit.com).

So, how does this work? It’s actually super simple.

1. [Jailbreak your device of choice](http://evasi0n.com/). I’ve used an iPad 4 here. Make sure it runs iOS 7.0.x. Both arm7(s) and arm64 devices will work now. **Don’t jailbreak a device that’s used in production.** Otherwise you lose a lot of security features as well. I only jailbreak a clean device and install some apps to inspect.

2. Open Cydia and install OpenSSH, nano, and Cydia Substrate (previously called MobileSubstrate).

3. Copy the Reveal library. Find out the device IP address via Settings and execute the following in your terminal (this assumes you have installed [Reveal](http://revealapp.com/) already):\
    `scp -r /Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/Reveal.framework root@192.168.0.X:/System/Library/Frameworks`\
    `scp /Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/libReveal.dylib root@192.168.0.X:/Library/MobileSubstrate/DynamicLibraries`.\
    For Spark Inspector, you would use `scp "/Applications/Spark Inspector.app/Contents/Resources/Frameworks/SparkInspector.dylib" root@192.168.0.X:/Library/MobileSubstrate/DynamicLibraries`\
    Note: The default SSH password on iOS is ‘alpine.’

4. SSH into the device and create following text file with `nano /Library/MobileSubstrate/DynamicLibraries/libReveal.plist`: `{ Filter = { Bundles = ( "<App ID>" ); }; }`\
    Previously, this worked with wildcard IDs, but this approach has problems with the updated Cydia Substrate. So simply add the App ID of the app you want to inspect, and then restart the app.

5. Respring with `killall SpringBoard` or simply restart the device.

Done! Start your app of choice and select it in Reveal. (This should also work similary for [SparkInspector](http://sparkinspector.com/).) Attaching via LLDB is a bit harder and I won’t go into details here since this could also be used to pirate apps. Google for ‘task\_for\_pid-allow,’ ‘debugserver,’ and ‘ldid’ if you want to try this.

> Tweetbot. The action bar is simply part of the cell. And the "No Tweets Found" view is always below the UITableView. [pic.twitter.com/Xp6OKlvaJh](http://t.co/Xp6OKlvaJh)
>
> — Peter Steinberger (@steipete) [December 27, 2013](https://twitter.com/steipete/statuses/416573601937375233)

> AppStore.app is super complex. SKUISoftwareSwooshCollectionViewCell. No wonder it took so long to become native. [pic.twitter.com/hjFdImyqSP](http://t.co/hjFdImyqSP)
>
> — Peter Steinberger (@steipete) [December 27, 2013](https://twitter.com/steipete/statuses/416579994027298816)

> Chrome is basically a full-screen UIWebView with some controls on top. [pic.twitter.com/CFfEM7j6vT](http://t.co/CFfEM7j6vT)
>
> — Peter Steinberger (@steipete) [December 27, 2013](https://twitter.com/steipete/statuses/416584566024208384)

> Surprise. PS Express is all native, even the classes are well structured. "AdobeCleanBoldFontButton". [pic.twitter.com/YW1xMNPSP2](http://t.co/YW1xMNPSP2)
>
> — Peter Steinberger (@steipete) [December 27, 2013](https://twitter.com/steipete/statuses/416579309412036608)

> Twitter. There's still a small part of [@lorenb](https://twitter.com/lorenb) in there. (ABCustomHitTestView, ABSubTabBar) (Also: T1 as namespace?) [pic.twitter.com/R62JAY4DDQ](http://t.co/R62JAY4DDQ)
>
> — Peter Steinberger (@steipete) [December 27, 2013](https://twitter.com/steipete/statuses/416574990440738816)

New posts, shipping stories, and nerdy links straight to your inbox.

2× per month, pure signal, zero fluff.
