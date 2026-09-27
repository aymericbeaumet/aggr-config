---
title: Network Kernel Core Dump
link: https://steipete.me/posts/2020/network-kernel-core-dump/
source: steipete-me
published: 2020-05-21T10:00:00Z
updated: 2020-05-21T10:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Step-by-step instructions from Apple for capturing macOS kernel core dumps over a network connection between two Macs.
content: extracted
html: 2020-05-21-network-kernel-core-dump.html
preview:
  file: 2020-05-21-network-kernel-core-dump.preview-ec1229991d7a.webp
  width: 256
  height: 134
  color: '#f0eded'
images:
- source: https://steipete.me/posts/2020/network-kernel-core-dump/index.png
  original:
    file: 2020-05-21-network-kernel-core-dump.image-9c3738aaf25a.png
    width: 1200
    height: 630
  variants:
  - file: 2020-05-21-network-kernel-core-dump.image-19b6581715d2.webp
    width: 320
    height: 168
  - file: 2020-05-21-network-kernel-core-dump.image-3b8ab35b0449.webp
    width: 640
    height: 336
  - file: 2020-05-21-network-kernel-core-dump.image-c563d2c31d95.webp
    width: 960
    height: 504
  - file: 2020-05-21-network-kernel-core-dump.image-62245db0058c.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://pbs.twimg.com/media/EUBGuLIXgAEAQ5n?format=jpg&name=4096x4096
  original:
    file: 2020-05-21-network-kernel-core-dump.image-6fbef2c075d5.jpg
    width: 2224
    height: 1796
  color: '#fcfcfc'
---

![](https://pbs.twimg.com/media/EUBGuLIXgAEAQ5n?format=jpg&name=4096x4096)

A week after Apple’s initial “macOS Core Dump” reply, and me sending a lot of questions their way, I got a really nice, human reply that explains the process via networking and a second Mac.

If you started here, read the backstory first: [How to macOS Core Dump](https://steipete.me/posts/how-to-macos-core-dump/)

I am sharing this for future reference:

Any Mac will work as a coredump server; you just need a gigabyte or so of free space per coredump. The kernel dump client can only be configured to transmit on a hard-wired Ethernet port, either built-in or over Thunderbolt. There is no support for transmitting core dumps across the AirPort interface, USB Ethernet, or across third-party Ethernet interfaces. This is an issue for early MacBook Air models which have no built-in Ethernet or Thunderbolt interfaces.

On the server (non-panicking) machine run:

- `sudo mkdir /PanicDumps`
- `sudo chown root:wheel /PanicDumps`
- `sudo chmod 1777 /PanicDumps`
- `sudo launchctl load -w /System/Library/LaunchDaemons/com.apple.kdumpd.plist`

To verify that the core dump server is active `sudo launchctl list | grep kdump`\
 This should return: `- 0 com.apple.kdumpd`

On the client (panicking) machine run the following commands Locate the IP address of the core dump server.\
 `sudo nvram boot-args="debug=0xd44 _panicd_ip=10.0.40.2 kdp_match_name=en7"`

Where `10.0.40.2` is replaced by the IP address of the server and en7 is replaced by the name of the client’s Ethernet interface. You can use this command to show all of the network interfaces on the system: `ifconfig -a`

Then reboot: `sudo reboot`

If you hang, NMI the machine by hitting the buttons Left-⌘ + Right-⌘ + Power. This will generate a coredump file on the server machine in the directory `/PanicDumps`. If you panic, the coredump file will be generated automatically on the server machine in the `/PanicDumps` directory. Compress the coredump file saved to `/PanicDumps`, and attach that to a feedback report. (Attachments have to be a zip, not a folder)

New posts, shipping stories, and nerdy links straight to your inbox.

2× per month, pure signal, zero fluff.
