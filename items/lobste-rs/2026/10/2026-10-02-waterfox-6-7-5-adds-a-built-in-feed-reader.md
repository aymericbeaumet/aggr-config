---
title: Waterfox 6.7.5 Adds a Built-in Feed Reader
link: https://www.waterfox.com/releases/6.7.5/
source: lobste-rs
published: 2026-10-02T02:17:04Z
updated: 2026-10-02T02:17:04Z
first_seen: 2026-10-03T11:34:56.593287151Z
authors:
- waterfox.com via chai
labels:
- browsers
- release
summary: Comments
content: extracted
html: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.html
preview:
  file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.preview-253795f51d39.webp
  width: 256
  height: 134
  alt: Waterfox 6.7.5 — Read the Feed — Release Notes
  color: '#311e40'
images:
- source: https://www.waterfox.com/open-graph/releases/6.7.5.png
  original:
    file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-556fbe7dd3bd.png
    width: 1200
    height: 630
  variants:
  - file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-5a55660d7371.webp
    width: 320
    height: 168
  - file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-b79c09bea891.webp
    width: 640
    height: 336
  - file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-36af32435431.webp
    width: 960
    height: 504
  - file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-9116965afe85.webp
    width: 1200
    height: 630
  color: '#000001'
- source: https://www.waterfox.com/_astro/01-feed-manager-light.ErH7GxPB_Z11qH05.png
  original:
    file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-ee634219a93e.png
    width: 2880
    height: 1570
  color: '#faf9fd'
- source: https://www.waterfox.com/_astro/03-feed-reader.hcPoTq6W_Z1dEBES.png
  original:
    file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-b4bd52448f6f.png
    width: 2880
    height: 1700
  color: '#fbfafe'
- source: https://www.waterfox.com/_astro/04-feed-subscription-preview.Cve7mLzg_ZflT9p.png
  original:
    file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-dc1a11d61db8.png
    width: 2880
    height: 1310
  color: '#faf9fd'
- source: https://www.waterfox.com/_astro/05-feed-settings.Dslgkbbv_Z1M6MVs.png
  original:
    file: 2026-10-02-waterfox-6-7-5-adds-a-built-in-feed-reader.image-fdd29fd98421.png
    width: 2240
    height: 1670
  color: '#fbfbfd'
---

[All releases](https://www.waterfox.com/releases/)

Desktop

A new feed manager and built-in reader are now available natively within Waterfox.

Posted September 30, 2026

Toggle table of contents. Current section: New

## New

- **Manage your feeds:** The feed manager brings posts from your Live Bookmarks together in one place. You are able to browse all your feeds together, choose a bookmark folder or individual subscription, search articles, filter unread posts, and switch between list and grid views. Open it from **Settings → Feeds → Open Feeds**, or visit `about:feeds`.

  ![Waterfox feed manager showing unread articles from Waterfox, Smashing Magazine, CSS-Tricks and Mozilla Hacks, organized into bookmark folders](https://www.waterfox.com/_astro/01-feed-manager-light.ErH7GxPB_Z11qH05.png)
- **Read without leaving the built-in manager:** Based on the Reader View natively available for supported websites, Waterfox can now render supported feed content. Posts can be marked as read or unread, articles can be saved for later, or open the original website. Read status and saved articles are kept between browser sessions.

  ![Mozilla Hacks' Intent to Ship: JPEG XL article displayed in Waterfox's feed reader, with text controls and options to mark it unread, save it or open the original website](https://www.waterfox.com/_astro/03-feed-reader.hcPoTq6W_Z1dEBES.png)
- **Feed previews:** Supported feeds opened directly in Waterfox now show recent posts and a subscription form instead of raw XML. You can also use **Add feed** in the feed manager to preview a feed URL before subscribing. The subscription panel now shows more information about the feed, confirms where it was saved, and lets you undo a new subscription.

  ![Preview of the official Waterfox RSS feed, showing recent posts and a subscription form with a name and bookmark location](https://www.waterfox.com/_astro/04-feed-subscription-preview.Cve7mLzg_ZflT9p.png)
- **Dedicated feed settings:** Under **Settings → Feeds**, you can enable or disable feed discovery, select whether articles open in Waterfox’s reader or on the original website, and control automatic article image loading.

  ![Waterfox Feeds settings with controls for feed discovery, article opening, automatic image loading, and OPML import and export](https://www.waterfox.com/_astro/05-feed-settings.Dslgkbbv_Z1M6MVs.png)

## Changed

- Feed articles open in Waterfox’s reader by default. Choose **Open the original article** under **Settings → Feeds** if you prefer visiting the website.
- OPML import and export have moved to **Settings → Feeds**.
- OPML import and export now preserve nested bookmark folders.

## Fixed

- Security fixes described in [Mozilla Foundation Security Advisory 2026-100](https://www.mozilla.org/en-US/security/advisories/mfsa2026-100/).
- Feed discovery now checks that a feed can be loaded and parsed before offering it in the address bar, rather than showing broken or invalid feed links.
- Manually refreshing a failed feed can now retry without waiting for the normal refresh interval, while still respecting server retry delays.
