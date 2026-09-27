---
title: 'stats.store: Privacy-First Sparkle Analytics'
link: https://steipete.me/posts/2025/stats-store-privacy-first-sparkle-analytics/
source: steipete-me
published: 2025-06-18T09:00:00Z
updated: 2025-06-18T09:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: How curiosity about VibeTunnel users led me to build stats.store - a free, open source analytics backend for Sparkle using AI tools, all while cooking dinner.
content: extracted
html: 2025-06-18-stats-store-privacy-first-sparkle-analytics.html
preview:
  file: 2025-06-18-stats-store-privacy-first-sparkle-analytics.preview-676814a4ffe6.webp
  width: 256
  height: 134
  color: '#e9e7e7'
images:
- source: https://steipete.me/posts/2025/stats-store-privacy-first-sparkle-analytics/index.png
  original:
    file: 2025-06-18-stats-store-privacy-first-sparkle-analytics.image-a2d31f0cdcca.png
    width: 1200
    height: 630
  variants:
  - file: 2025-06-18-stats-store-privacy-first-sparkle-analytics.image-59ad01c9e8ef.webp
    width: 320
    height: 168
  - file: 2025-06-18-stats-store-privacy-first-sparkle-analytics.image-2b6edda39952.webp
    width: 640
    height: 336
  - file: 2025-06-18-stats-store-privacy-first-sparkle-analytics.image-d2303553781d.webp
    width: 960
    height: 504
  - file: 2025-06-18-stats-store-privacy-first-sparkle-analytics.image-c689066619d1.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2025/stats-store/hero.png
  original:
    file: 2025-06-18-stats-store-privacy-first-sparkle-analytics.image-9e7253763324.png
    width: 1024
    height: 683
  color: '#fad7fe'
---

![](https://steipete.me/assets/img/2025/stats-store/hero.png)

**tl;dr**: I built a [free, open source analytics backend](https://stats.store) for Sparkle because all existing solutions are ancient, and I wanted to know how many people use [VibeTunnel](https://vibetunnel.sh) without being creepy about it.

I’m philosophically against deep app analytics. Everything I make is open source and free, and integrating an analytics SDK just didn’t feel right. Sparkle has this neat little [system profiling](https://sparkle-project.org/documentation/system-profiling/) feature that’s perfect for people like me - it just tells you the macOS version and how many people opened your app this week. No IP addresses, no invasive tracking, just enough to stay motivated.

The problem? When I checked for backend implementations, [everything was ancient](https://sparkle-project.org/documentation/system-profiling/). So naturally, I thought: “How hard can it be?”

## Building stats.store with agents

I fired up [Google’s AI Studio](https://aistudio.google.com/) and wrote a simple spec (you can see it on [GitHub](https://github.com/steipete/stats-store/tree/main/docs/spec.md)). Then I fed it into [v0](https://v0.dev) - they use Next.js, Supabase, Tailwind, and [shadcn/ui](https://ui.shadcn.com/), a stack I already know and love.

After about five or six rounds of typing “continue” into v0, I had something workable. But v0 is great for frontend, less so when things get complex. It started making mistakes it couldn’t fix. So I [synced to GitHub](https://github.com/steipete/stats-store) and let [Claude Code](https://steipete.me/posts/2025/claude-code-is-my-computer/) take over. Claude added tests, dark mode, improved the design, and fixed the API flow.

Total time? Six hours. All while working on the next VibeTunnel release. Plus an hour picking the perfect domain (I really like stats.store).

## What is stats.store?

It’s exactly what I needed: a modern, privacy-first analytics backend for Sparkle. You get basic update statistics, macOS version distribution, and a clean dashboard - all while respecting user privacy.

**What gets collected:**

- **System info:** macOS version, CPU type (Intel/Apple Silicon), Mac model, core count, RAM, system language
- **App usage:** Version numbers, update timestamps, daily unique users (via salted IP hashes)

**What doesn’t get collected:**

- No IP addresses, names, or emails
- No usage patterns or behavior tracking
- No location data beyond language preference
- No device IDs or fingerprinting

Everything is designed to answer simple questions: How many people use my app? What macOS versions should I support? When do people actually update? The whole thing is [open source and MIT-licensed](https://github.com/steipete/stats-store).

## Free for Open Source

Here’s the deal: if you’re building open source Mac apps, it’s completely free. I’ll cover the hosting (Vercel and Supabase make this pretty cheap anyway).

Want in? Just [email me](mailto:peter@steipete.me). I’ll add your app to the backend - and by backend, I mean I’ll literally go into Supabase and edit the table. We’re keeping it scrappy. All you have to do is [redirect the appcast URLs](https://github.com/steipete/stats-store#how-it-works-), stats.store will fetch the correct one and pass them along.

If enough people join, we’ll have a nice community dataset showing how fast Mac users update their OS and what hardware they’re running. Could be interesting!

## Try It

Check out [stats.store](https://stats.store). If you have a Sparkle-based Mac app and want simple, privacy-respecting analytics, let me know. Together we can build something useful for the Mac developer community.

*Questions? Hit me up on [Twitter](https://twitter.com/steipete) or [email](mailto:peter@steipete.me).*
