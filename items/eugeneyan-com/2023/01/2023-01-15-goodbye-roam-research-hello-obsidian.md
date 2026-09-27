---
title: Goodbye Roam Research, Hello Obsidian
link: https://eugeneyan.com//writing/roam-to-obsidian/
source: eugeneyan-com
published: 2023-01-15T00:00:00Z
updated: 2023-01-15T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- productivity
- til
summary: How to migrate and sync notes & images across devices
content: extracted
html: 2023-01-15-goodbye-roam-research-hello-obsidian.html
preview:
  file: 2023-01-15-goodbye-roam-research-hello-obsidian.preview-c9683190f5cf.webp
  width: 256
  height: 134
  color: '#33325c'
images:
- source: https://eugeneyan.com/assets/og_image/roam-to-obsidian.jpg
  original:
    file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-4f8d8c62f703.jpg
    width: 1200
    height: 630
  color: '#031436'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-01-15-goodbye-roam-research-hello-obsidian.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

I was bored over the weekend and migrated my notes from Roam Research to Obsidian. It was easier than expected and took under an hour. Here are the steps I took.

First, I selectively followed this [guide](https://notes.nicolevanderhoeven.com/Migrating+from+Roam+to+Obsidian) to download my notes as a zip file (steps 1) and create an Obsidian vault. Then, I [downloaded the images](https://github.com/nicolevanderhoeven/deroamify/blob/main/downloadfirebase.py) in my notes into `/assets` (step 11). A few notes were in folders because they had `/` in the title; I cleaned these up by hand.

As part of format conversion (step 4), I incorrectly enabled “Roam Research tag fixer”. As a result, my `#tag` and `#[[tag]]` were converted to `[[tag]]`. Fortunately, my tags always came after `tags:` and were on the first line of each file. Thus, I wrote some [basic regex](https://gist.github.com/eugeneyan/95d09eaeff0c99d468d8c459cfed5218) to fix it.

Next, I set up [obsidian-git](https://github.com/denolehov/obsidian-git) for syncing across devices. Installation on [Mac](https://github.com/denolehov/obsidian-git/wiki/Installation#plugin-installation) and [mobile](https://github.com/denolehov/obsidian-git/wiki/Installation#mobile) was straightforward. The first sync took seconds and subsequent syncs were under a second.

Syncing images was trickier as git doesn’t do well with large commits. For my 1k+ existing images (mostly screenshots of talks and papers), I [iteratively committed](https://gist.github.com/eugeneyan/c4da21af315b62fc3b88163541c816e3) those that were less than 1,024kb in size. Images greater than 1,024kb were shrunk—by converting image format from `png` to `jpg`—before committed. Syncing the images to GitHub took about 15 minutes. New images will be synced via obsidian-git.

A few community plug-ins I use:

- [obsidian-git](https://github.com/denolehov/obsidian-git): To sync notes across devices via git
- [obsidian-outliner](https://github.com/vslinko/obsidian-outliner): Easier working with bullets
- [obsidian-minimal](https://github.com/kepano/obsidian-minimal) and [obsidian-minimal-settings](https://github.com/kepano/obsidian-minimal-settings): Minimal display theme

Others have also recommended their favorite plug-ins on this [tweet](https://twitter.com/eugeneyan/status/1614847315914936321).

I’m loving Obsidian so far. It feels snappier than web-based Roam during startup and while using it. It’s also more customizable. I don’t foresee it hindering my [Zettelkasten workflow](https://eugeneyan.com/writing/note-taking-zettelkasten/). While I have a forever-free Roam graph (I was an early adopter) and there’s no push factor, I’m going to stick with Obsidian for a bit and see how it goes.

Are you an Obsidian user as well? What features or plug-ins do you recommend?

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Jan 2023). Goodbye Roam Research, Hello Obsidian. eugeneyan.com. https://eugeneyan.com/writing/roam-to-obsidian/.

or

```
@article{yan2023obsidian,
  title   = {Goodbye Roam Research, Hello Obsidian},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Jan},
  url     = {https://eugeneyan.com/writing/roam-to-obsidian/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
