---
title: Cops Can Bypass iPhone's Automatic Reboot to Get into Locked Phones
link: https://www.404media.co/cops-can-bypass-iphone-automatic-inactivity-reboot-graykey/
source: hnrss-org
published: 2026-10-01T14:38:35Z
updated: 2026-10-01T14:38:35Z
first_seen: 2026-10-01T19:33:44.897168732Z
authors:
- speckx
labels:
- hacking
- news
content: extracted
html: 2026-10-01-cops-can-bypass-iphone-s-automatic-reboot-to-get-into.html
preview:
  file: 2026-10-01-cops-can-bypass-iphone-s-automatic-reboot-to-get-into.preview-f9c1201c2565.webp
  width: 256
  height: 156
  color: '#5d696a'
images:
- source: https://storage.ghost.io/c/0f/76/0f76b548-bc58-4f25-abc3-3f5ebca07da4/content/images/size/w1200/2026/09/graykey-device.png
  original:
    file: 2026-10-01-cops-can-bypass-iphone-s-automatic-reboot-to-get-into.image-f76cc98cb019.png
    width: 1200
    height: 732
  variants:
  - file: 2026-10-01-cops-can-bypass-iphone-s-automatic-reboot-to-get-into.image-a509dfb94fd9.webp
    width: 320
    height: 195
  - file: 2026-10-01-cops-can-bypass-iphone-s-automatic-reboot-to-get-into.image-3e8084c344b1.webp
    width: 640
    height: 390
  - file: 2026-10-01-cops-can-bypass-iphone-s-automatic-reboot-to-get-into.image-bc0097157e84.webp
    width: 960
    height: 586
  - file: 2026-10-01-cops-can-bypass-iphone-s-automatic-reboot-to-get-into.image-09ca59975aed.webp
    width: 1200
    height: 732
  color: '#030505'
---

A company that makes phone hacking devices claims to have developed a solution that freezes iPhones in a state that lets cops more easily access sensitive data inside them, according to a video obtained by 404 Media.

This is the latest salvo in the never-ending battle between Apple and companies that help cops — sometimes those in [authoritarian](https://www.404media.co/cellebrite-unlocked-this-journalists-phone-cops-then-infected-it-with-malware/) [countries](https://techcrunch.com/2026/06/25/cellebrite-said-it-cut-off-russia-but-russia-used-is-tools-anyway/?ref=404media.co) — break into iPhones.

In November 2024, [404 Media revealed](https://www.404media.co/apple-quietly-introduced-iphone-reboot-code-which-is-locking-out-cops/) Apple quietly introduced a new feature in iOS that automatically reboots an iPhone that has not been unlocked for 72 hours. The idea behind this so-called “inactivity reboot” is to revert the phone to a state that makes it harder for police to break into the device, and thus extract sensitive data from it with forensics technology.

At the time of Apple’s change, law enforcement agents [expressed concern](https://www.404media.co/police-freak-out-at-iphones-mysteriously-rebooting-themselves-locking-cops-out/) about this new feature, given that oftentimes they can’t immediately try to break into iPhones that have been seized. That could be because police are still waiting for a court authorization to do so, or there is simply a backlog of devices to unlock, for example.

The new technology to get around inactivity reboot was developed by Magnet Forensics, the company behind GrayKey, a popular tool sold to law enforcement agencies that [allows them to unlock and access data stored in iPhones and Android smartphones](https://www.404media.co/leaked-documents-show-what-phones-secretive-tech-graykey-can-unlock-2/). Magnet has developed a new device called GrayKey Preserve and a feature for its regular GrayKey devices called Evidence Preservation Mode, according to the video.

“This is an absolute game changer for iOS forensics and a function that I wish we had years ago,” a Magnet employee says in the leaked video, specifically mentioning that the solution is targeted at the iPhone’s inactivity reboot feature and the data it makes unavailable. GrayKey Preserve and Evidence Preservation Mode are also designed to combat another iPhone feature that automatically deletes certain data — such as cached locations, and recently deleted photos and iMessages — after a certain number of days. “We're gonna be able to preserve that data for an infinite amount of time.”

The educational and tutorial video, which was made exclusively for law enforcement agents and appears to be dated early 2025, does not explain the technical details behind this new product and feature, but gives some strong hints as to how it works. 404 Media granted the person who provided it anonymity because they were not authorized to share it with third parties.

“Now, one of the many things that the GrayKey Preserve is going to do, as part of all of this, inside of our initial access, is it's going to enable Airplane Mode, or more specifically, it's gonna disable our radio transmissions like Bluetooth, Wi-Fi and cellular. In doing this, that's not only going to allow you to preserve the data in the field, but also isolate the data in the field,” the employee explains. “Even if that device doesn't have the ability to turn on Airplane Mode or to turn off the transmitters through the Control Center of iOS. Once initial access is gained and the device is in a preserved state, we also disable all of those radios to make sure that that data cannot reach that device.”

The idea behind GrayKey Preserve and Evidence Preservation Mode is to keep the iPhone in a state known as After First Unlock, or AFU. Having an iPhone in that state essentially allows cops to access data that would otherwise be much harder to obtain if the phone was in a Before First Unlock, or BFU state. If the iPhone is in BFU, certain sensitive data is encrypted, and it can be significantly harder — sometimes even virtually impossible — to brute-force the device’s passcode and unlock it. 404 Media previously obtained lists of iPhones [that GrayKey](https://www.404media.co/leaked-documents-show-what-phones-secretive-tech-graykey-can-unlock-2/) and competitor [Cellebrite could access](https://www.404media.co/leaked-docs-show-what-phones-cellebrite-can-and-cant-unlock/), with variations between phones in an AFU and BFU state.

“That AFU state is captured,” by GrayKey Preserve and Evidence Preservation Mode, the employee says. “Even if that device does reboot for any number of reasons, memory maintenance or the power is lost or whatever, the AFU state is not lost. This is the true magic behind the GrayKey Preserve and the Evidence Preservation Mode function.”

When using this new solution, law enforcement agents only see the iPhone’s software version and device model, rather than any data within, according to the employee and a screenshot of GrayKey’s customer interface included in the video. That allows the cops to preserve the data they care about without actually seeing it before they are authorized to do so.

Apple and Magnet did not respond to a request for comment.

404 Media shared a transcript of the video with Jiska Classen, a researcher at the Hasso Plattner Institute who studies iPhone security. While Classen said that it’s impossible to know for sure how Magnet’s new feature works based on the video, she posited some theories and agreed that it is “quite a game changer” or “at least puts things back to where they were before inactivity reboot.”

She thinks Magnet has found a way to manipulate the iPhone’s clock, effectively “slowing down time” or even “stopping the clock from ticking, even after a reboot.” Most likely, according to her, the GrayKey may disable the iPhone tasks that set data to expire.

What is certain is that the ball is now in Apple’s court to figure out how Magnet got around inactivity reboot, and find a way to prevent the company and its technology from freezing iPhones — and the sensitive data stored in them — in time.
