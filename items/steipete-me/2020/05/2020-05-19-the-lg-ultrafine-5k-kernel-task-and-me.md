---
title: The LG UltraFine 5K, kernel_task, and Me
link: https://steipete.me/posts/2020/the-lg-ultrafine5k-kerneltask-and-me/
source: steipete-me
published: 2020-05-19T06:00:00Z
updated: 2020-05-19T06:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: A four-year saga with the problematic LG UltraFine 5K display and the surprising discovery that plugging it into the wrong MacBook side causes performance issues.
content: extracted
html: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.html
preview:
  file: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.preview-5810c31fb568.webp
  width: 256
  height: 134
  color: '#ebe9e9'
images:
- source: https://steipete.me/posts/2020/the-lg-ultrafine5k-kerneltask-and-me/index.png
  original:
    file: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.image-c5dbec8c9927.png
    width: 1200
    height: 630
  variants:
  - file: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.image-9bba561bdcfc.webp
    width: 320
    height: 168
  - file: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.image-e452934037fe.webp
    width: 640
    height: 336
  - file: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.image-6e89abe1c2e0.webp
    width: 960
    height: 504
  - file: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.image-2fdda4210860.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2020/appleintelframebuffer/lg-box.jpg
  original:
    file: 2020-05-19-the-lg-ultrafine-5k-kernel-task-and-me.image-36adae3da559.jpg
    width: 2048
    height: 1536
  color: '#e5e7f4'
---

![](https://steipete.me/assets/img/2020/appleintelframebuffer/lg-box.jpg)

A good story is nuanced and complicated, and it contains surprise twists and a happy ending. Me owning an LG UltraFine 5K delivers on all of that. So let’s dive right in:

> [View post on X](https://twitter.com/i/web/status/1253550223445708800)

## Background

I own one of the [cursed](https://mjtsai.com/blog/2020/02/03/macos-display-problems/) LG UltraFine 5K displays.[^1] In fact, I don’t just own one; I own about 10 of them. But I realize now that blindly trusting in Apple hardware is a mistake.

## Problems

Since we first purchased the displays, we’ve been having issues with them, starting with [delayed shipping](https://mjtsai.com/blog/2016/12/20/lg-5k-ultrafine-display-delayed/), [ghosting](https://www.reddit.com/r/mac/comments/a2vf8i/lg_5k_ultrafine_ghosting/), and [Wi-Fi interference](https://www.macrumors.com/2017/03/15/lg-ultrafine-5k-shielding-fixed/), and moving on to compatibility — there’s no HDMI, DisplayPort, or similar. The only way to use these monitors is with a modern Mac.[^2] As for the compatibility issue, that was a known tradeoff, and for me, it was acceptable. After all, the benefit of only having a single cable as a modern docking station and a beautiful panel outweighed the drawbacks. I still remember [my innocent excitement](https://twitter.com/steipete/status/819869709294325760).

Since receiving these displays, we’ve had to return most of them to get fixes for various issues, and we’ve patiently updated the firmware multiple times with [LG’s crappy Screen Manager](https://twitter.com/steipete/status/915141641308172288) software. There are also [issues with expanding batteries, and Apple has blamed the LG 5K](https://twitter.com/steipete/status/1232654186598281216), saying just don’t use it a lot and you’ll be UltraFine.

## The Great Flickering

With the 2017 MacBook Pro, I had an extraordinary amount of fun, since plugging in the LG was causing graphic issues on the MacBook display. I wrote radars, called Apple Support, and even took a cab to a repair center with the screen in tow in order to prove the hardware was broken. It’s difficult when neither the screen itself nor the MacBook have issues, but the combination of the two causes problems.

> [View post on X](https://twitter.com/i/web/status/956863946404827136)

Nobody knew what was going on, and this all occurred before we had an Apple Store in Austria, so getting help there was out of the question. Apple eventually agreed to change my logic board for free, but only after countless hours of phone calls and emails and after this issue had been escalated multiple times. Mind you, this was all with active AppleCare and a machine that wasn’t even one year old. The replacement of my logic board took more than a week, and after it was returned, it had the exact same issue. I mostly gave up and just didn’t use my external screen.

## kernel\_task Goes Omnomnom

The graphic issues disappeared with the 2018 MacBook Pro. However, the biggest issue was also the weirdest: Sometimes, after longer use, my MacBook Pro became unusably slow. Mere seconds after removing the external screen, kernel\_task disappeared back into the CPU activity ground noise. Sometimes I had hours after disconnecting in which the MacBook worked, but other times this happened really fast. (I use [iStat Menus](https://bjango.com/mac/istatmenus/) here.)

> [View post on X](https://twitter.com/i/web/status/1128703168697839617)

I was never able to reliably reproduce this, nor could Apple Support help. They also claimed to not know anything about this issue. I again mostly gave up and didn’t use the external screen.

## 16-Inch MacBook Pro and LG UltraFine

I spend between eight and ten hours per day on my MacBook Pro, so updating it every year is a good investment. Of course, this also gave me the opportunity to test each hardware generation with the LG display.

When Apple announced the 16-inch MacBook Pro, I just had to get one. I don’t enjoy splitting my work across multiple machines, and I like to move around in my apartment, so it had to be a portable one. Over the years, Apple has managed to build beefier and beefier portables. However, its 8-core 15-inch MacBook Pro is severely thermal throttled — [using this tool](https://software.intel.com/content/www/us/en/develop/articles/intel-power-gadget.html), you can see how it runs at top speed for a few seconds, only to be thermal throttled.

The 16-inch MacBook Pro was marketed as “[thicker](https://www.theverge.com/2019/11/18/20971297/macbook-pro-16-inch-battery-apple-thickness-teardown-ifixit)” as a way of fixing the throttling, and it delivered! Everything is noticeably faster. It also fixed my biggest issue with the whole MacBook Pro lineup so far: the dreaded kernel\_task that eats up CPU like Pac-Man eats ghosts.

Then, in April, I stumbled upon a [Stack Exchange question](https://apple.stackexchange.com/questions/363337/how-to-find-cause-of-high-kernel-task-cpu-usage) that seemed to explain what was going on all these years.

> high CPU usage by kernel\_task is caused by high Thunderbolt Left Proximity temperature, which is caused by charging and having normal peripherals plugged in at the same time.

I HAD PLUGGED THE SCREEN INTO THE WRONG SIDE. YES.

The 16-inch MacBook Pro doesn’t seem to suffer from this temperature sensor misplacement and can drive the LG UltraFine without slowdown on both ends. But all generations before have this issue (tested with 2016 and later, purchased every generation.) [My posting about the issue on Twitter made waves, and I even ended up being mentioned in some tech articles](https://www.trustedreviews.com/news/is-there-really-a-wrong-way-to-charge-a-macbook-pro-4026796).

The bad news: The LG can provide 87 watts of power, but the notebook comes with a 96-watt adaptor. This means that the battery is constantly compensating. Play StarCraft for three hours and your computer will shut off with a dead battery. Even when compiling code in Xcode, the battery will fluctuate. To deal with this, [Apple recommends using a separate power adapter](https://twitter.com/BesherMaleh/status/1206434150078656512) when using the 16-inch MacBook Pro.

## Brightness Bugs

Though the above issue was solved, there is a new bug that started happening with the 16-inch MacBook Pro: Sometimes after wakeup, the gamma settings of the screen are way off (usually too bright, but it can also be [too dark](https://twitter.com/tkanzakic/status/1263345836538421248?s=21)). This can be fixed via unplugging/replugging or via [toggling True Tone](https://twitter.com/nicholascooke/status/1263244851266510848?s=21). This bug is [independent](https://twitter.com/sdw/status/1263226044435140608?s=21) [of](https://twitter.com/alihilal94/status/1263227224217595904?s=21) [the](https://twitter.com/oleg_v_soloviev/status/1263314894918729735?s=21) [screen](https://twitter.com/nicholascooke/status/1263244851266510848?s=21) you use though. (Apple: FB7722012) Also, don’t confuse this bug with the [eye-burning brightness bug after reboot](https://macperformanceguide.com/blog/2020/20200107_1436-2019MacPro-LG5K-maximum-brightness-after-reboot.html) (I know it’s hard to keep up with all the different bugs these days). Luckily, Apple fixed the latter bug in Catalina 10.15.4.

> [View post on X](https://twitter.com/i/web/status/1263225822485381120)

## Conclusion

Do I recommend this setup? Yes. I am writing this story on the LG display after all. I mostly use the separate power plug to fix the “missing 9-watt problem.” If you have top-notch hardware, it is a beautiful panel, and it seems Apple has fixed all the issues after a four-year hiatus. (And by fixed, I mean they released new hardware, aka [Fix In Next Chip](https://twitter.com/blelbach/status/1258680434432458752).) Besides, every time I [look around for other screens](https://mjtsai.com/blog/2019/06/26/the-great-monitor-search-continues/), they all seem terrible.

And it’s true: Once you are used to Retina, you don’t want to go back to 4K. Maybe this is all a years-long elaborate ploy to make me buy a [Pro Display XDR](https://www.apple.com/pro-display-xdr/). Maybe I am just a [bug magnet](https://twitter.com/steipete/status/1253979468164673536). Or maybe it’s [not just me](https://mjtsai.com/blog/2017/01/09/lg-ultrafine-5k-reviews/).

Is this the end of the story? Find out in [Kernel Panics and Surprise boot-args](https://steipete.com/posts/kernel-panic-surprise-boot-args/).

Update: LG released [an update to the 5K Display](http://www.lgnewsroom.com/2019/07/lg-introduces-new-ultrafine-5k-display-2/) that increases power output to 94 watts.

[^1]: LG released [a second generation](https://9to5mac.com/2019/07/30/new-lg-ultrafine-5k-display/) of the LG UltraFine 5K display, and it adds support for 3840 x 2160 @ 60Hz over USB-C DisplayPort, so you can now connect an iPad Pro. Sporadic reports also mention fewer issues with the second generation.

[^2]: I managed [to get it working](https://www.reddit.com/r/hackintosh/comments/ae8d6c/is_it_possible_to_build_a_hackintosh_that/) with a Gigabyte GC-Titan Ridge card on a Hackintosh.
