---
title: Video CDs Break Windows Explorer
link: https://clydesnotes.blogspot.com/2026/08/video-cds-break-windows-explorer.html
source: hnrss-org
published: 2026-09-26T19:30:39Z
updated: 2026-09-26T19:30:39Z
first_seen: 2026-09-28T06:49:16.850449770Z
authors:
- ClydeN
content: extracted
html: 2026-09-26-video-cds-break-windows-explorer.html
preview:
  file: 2026-09-26-video-cds-break-windows-explorer.preview-5c3035229efa.webp
  width: 256
  height: 134
  color: '#d5d8d9'
images:
- source: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmEXpyEYrKp1IrBYcszlrHRpBjTAvVf4YsSvuuaJSHMt4GIe1IK5wY29MGno4NiTYDAQd3LWEchLC65hmShxwPVBp1xZhpb0YICEog5e1m7kA4FrUsw2XFK5yzD8_plHvcetHaTC1t7yU7hGFIpt4LUePB98vCxqjltTa7oww8Wyy1bUQ0FOeLv0fqqAFQ/w1200-h630-p-k-no-nu/vcdbug.png
  original:
    file: 2026-09-26-video-cds-break-windows-explorer.image-1fec590bbd1d.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-26-video-cds-break-windows-explorer.image-e4aae6be77d1.webp
    width: 320
    height: 168
  - file: 2026-09-26-video-cds-break-windows-explorer.image-6a6e497c5052.webp
    width: 640
    height: 336
  - file: 2026-09-26-video-cds-break-windows-explorer.image-e6c8afe7738b.webp
    width: 960
    height: 504
  - file: 2026-09-26-video-cds-break-windows-explorer.image-4ac0a52a11c8.webp
    width: 1200
    height: 630
  color: '#fbfbfb'
- source: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmEXpyEYrKp1IrBYcszlrHRpBjTAvVf4YsSvuuaJSHMt4GIe1IK5wY29MGno4NiTYDAQd3LWEchLC65hmShxwPVBp1xZhpb0YICEog5e1m7kA4FrUsw2XFK5yzD8_plHvcetHaTC1t7yU7hGFIpt4LUePB98vCxqjltTa7oww8Wyy1bUQ0FOeLv0fqqAFQ/s1600/vcdbug.png
  original:
    file: 2026-09-26-video-cds-break-windows-explorer.image-7e45d2be8c63.png
    width: 1600
    height: 777
  variants:
  - file: 2026-09-26-video-cds-break-windows-explorer.image-3d1ae059866a.webp
    width: 320
    height: 155
  - file: 2026-09-26-video-cds-break-windows-explorer.image-53a1f6f6f173.webp
    width: 640
    height: 311
  - file: 2026-09-26-video-cds-break-windows-explorer.image-04f5fe7de8d4.webp
    width: 960
    height: 466
  - file: 2026-09-26-video-cds-break-windows-explorer.image-0a6e7c331bf8.webp
    width: 1280
    height: 622
  - file: 2026-09-26-video-cds-break-windows-explorer.image-f5247d9f46bd.webp
    width: 1600
    height: 777
  color: '#fafbfb'
---

There is an odd bug in Windows related to [Video CDs](https://en.wikipedia.org/wiki/Video_CD).\
 To reproduce it:

1. Insert a Video CD (if you don't have one, you can download [this file](https://archive.org/download/video-cd-sampler/Video%20CD%20Sampler.7z), extract it and burn it with something that supports bin+cue images, like AnyBurn).
2. Open Explorer and copy the Video CD contents to the PC (you can copy just the *MPEGAV* folder, which contains the big multimedia files).

The typical outcome is that at some point the copying speed drops to zero and it stays there:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmEXpyEYrKp1IrBYcszlrHRpBjTAvVf4YsSvuuaJSHMt4GIe1IK5wY29MGno4NiTYDAQd3LWEchLC65hmShxwPVBp1xZhpb0YICEog5e1m7kA4FrUsw2XFK5yzD8_plHvcetHaTC1t7yU7hGFIpt4LUePB98vCxqjltTa7oww8Wyy1bUQ0FOeLv0fqqAFQ/s1600/vcdbug.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhmEXpyEYrKp1IrBYcszlrHRpBjTAvVf4YsSvuuaJSHMt4GIe1IK5wY29MGno4NiTYDAQd3LWEchLC65hmShxwPVBp1xZhpb0YICEog5e1m7kA4FrUsw2XFK5yzD8_plHvcetHaTC1t7yU7hGFIpt4LUePB98vCxqjltTa7oww8Wyy1bUQ0FOeLv0fqqAFQ/s1600/vcdbug.png)

When that happens, Windows Explorer is in a "broken" state. You can close the copy window and eject the VCD and it might look like everything is fine, but there are problems. For example, new files and folders are not visible without manually refreshing the view, some parts of the Windows UI become glitchy and the Windows Explorer process can't be restarted.

The only way to get the system working properly again is to restart the PC. A "soft" restart from Windows doesn't work, though; it just gets stuck at the "Restarting" screen forever. You need to do it by pressing the restart or power button. To investigate this bug more easily, I used virtual machines (VMware Workstation works best here and you can reboot the VM without affecting the host).

Here are my findings:

- The bug first appeared with the monthly updates 2023-03 (KB5023706) for Windows 11 and 2023-04 (KB5025221) for Windows 10. The initial releases of Windows 10 and Windows 11 22H2 with no updates do not have this bug.
- You can often reproduce the bug even with a virtual CD drive (by mounting the VCD image with Alcohol 120%, for example). But doing it with a physical CD is more reliable, maybe just because the copying process lasts longer and the bug has more time to appear.
- If you can't reproduce the bug the first time, delete the copied files, reboot and try again.
- Playing the VCD content with Windows Media Player does not trigger the bug.
- Extracting the content with a dedicated app like VCDGear does not trigger the bug.
- Copying files with a 3rd party app (FastCopy, TeraCopy) does not trigger the bug, unless the app uses the standard Windows method for copying (Total Commander does, by default).
- The bug cannot always be reproduced. But I've seen it on different systems, even with untouched Windows installations. I also found a few reports from other people ([example 1](https://www.reddit.com/r/techsupport/comments/16kto9o/old_cds_are_trying_to_kill_my_computers_off_what/), [example 2](https://www.reddit.com/r/DataHoarder/comments/1gruw45/copying_family_videos_from_vcddvd_format_into_pc/)).

I have reported the bug to Microsoft through the Feedback Hub at the end of 2025. Nothing has happened since — no feedback, no confirmation, no fix. Granted, it *is* a rare bug that very few people will encounter these days, to put it mildly. But a bug that affects system stability is still somewhat urgent, I think.

#### Why are Video CDs different and how to deal with them?

Even though VCDs appear as regular data discs in Windows Explorer, they're actually not. While searching for clues online, I found out that, for example, Linux doesn't let you copy the files the same way that Windows does. That caused some complaints from users over the years, which prompted some explanations.

> VCD's look like standard iso9660 filesystems, but they are not. Attempting to use the filesystem entries for other than pointers to the start of files will result in pointing off the disk due to invalid data. Typically when you copy a VCD you want the still images, menus, CD audio, i-frame index, etc. Copying the disk with dd retains that. Pulling the files off (and ignoring errors, the way Windows does) loses that. ([source](https://www.linuxquestions.org/questions/fedora-35/copy-standard-video-cd%27s-avseq01-dat-avseq02-dat-525215/))

> About .DAT files.  The ~600 MB file visible on the first track of the mounted VCD is not a real file! It is a so called ISO gateway, created to allow Windows to handle such tracks (Windows does not allow raw device access to applications at all). Under Linux you cannot copy or play such files (they contain garbage). Under Windows it is possible as its iso9660 driver emulates the raw reading of tracks in this file. ([source](http://www.mplayerhq.hu/DOCS/HTML/en/vcd.html))

Also, even if you successfully copy the .dat files several times without triggering the bug, the resulting files are not identical, probably because there's no error detection and error correction like on a typical data disc. To rip a Video CD properly, it's therefore better to use dedicated software, such as VCDGear, which will extract the AV content and try to fix the mpeg stream errors. Or clone the whole VCD with imaging software to a bin+cue image.

To sum up: yes, copying VCD files in Windows Explorer was never the optimal way to get the content. But many people were doing that, because the UI says it's *possible* and it more or less worked fine. A Windows update in early 2023 changed something that affects copying those files. I would expect a fix that either restores the old behavior or just prevents copying VCD files and redirects users to alternative ways. Anything would be better than making the system unstable.
