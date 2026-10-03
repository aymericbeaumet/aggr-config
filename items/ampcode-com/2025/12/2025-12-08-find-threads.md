---
title: Find Threads
link: https://ampcode.com/news/find-threads
source: ampcode-com
published: 2025-12-08T00:00:00Z
updated: 2025-12-08T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Amp can now search your threads. A few weeks ago we shipped the ability to read other threads. That was the first step: reference a thread by URL or ID, let Amp pull out the relevant context. But what if you don''t have the thread ID at hand? What if the only thing you know about a thread is that it changed or created a specific file? Or some keywords? That''s what the new find_thread tool is for. It lets Amp search your threads in two ways: Keyword search: find threads that mention specific terms. "Find threads where we discussed the database migration." The agent in Amp will then use the same search functionality as on the thread feed and return matches. File search: find threads that touched a specific file. Think of it like git blame, but for Amp. "Which thread last modified this file?" Amp looks at file changes across threads and tells you which conversations touched it. Here''s how we''ve been using it to find threads: "Which Amp thread created core/src/tools/tool-service.ts?" "Search my threads to find the one in which we added the explosion animation. I want to continue working on that." "Show me all threads that modified src/terminal/pty.rs" "Find and read the thread that created scripts/deploy.sh." "Find the thread in which we added @server/src/backfill-service.ts, read it, and extract the SQL snippet we used to test the migration." The find_thread tool is the sibling to read_thread. First you find, then you read. Together they turn your Amp threads into reusable context.'
content: extracted
html: 2025-12-08-find-threads.html
preview:
  file: 2025-12-08-find-threads.preview-b0cb3ba3cb89.webp
  width: 256
  height: 134
  color: '#5f584d'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Find+Threads&date=December+8%2C+2025&tagline=Find+Amp+threads+by+keyword+or+by+which+files+they+touched&sig=0f4d6873261626fb62f9cd702942368e3a93f9a7195eaff25095877991bfaddf
  original:
    file: 2025-12-08-find-threads.image-5b2b0eb82224.png
    width: 1200
    height: 630
  variants:
  - file: 2025-12-08-find-threads.image-8ee01b77e1e4.webp
    width: 320
    height: 168
  - file: 2025-12-08-find-threads.image-a3b68e0a31b1.webp
    width: 640
    height: 336
  - file: 2025-12-08-find-threads.image-20041baac672.webp
    width: 960
    height: 504
  - file: 2025-12-08-find-threads.image-1b57634f5a49.webp
    width: 1200
    height: 630
  color: '#221b16'
- source: https://static.ampcode.com/news/find-thread.png
  original:
    file: 2025-12-08-find-threads.image-2fa78159f395.png
    width: 1564
    height: 944
  color: '#272927'
---

Amp can now search your threads.

A few weeks ago we shipped [the ability to read other threads](https://ampcode.com/news/read-threads). That was the first step: reference a thread by URL or ID, let Amp pull out the relevant context.

But what if you don't have the thread ID at hand? What if the only thing you know about a thread is that it changed or created a specific file? Or some keywords?

That's what the new `find_thread` tool is for. It lets Amp search your threads in two ways:

**Keyword search**: find threads that mention specific terms. "Find threads where we discussed the database migration." The agent in Amp will then use the same search functionality as on [the thread feed](https://ampcode.com/threads) and return matches.

**File search**: find threads that touched a specific file. Think of it like `git blame`, but for Amp. "Which thread last modified this file?" Amp looks at file changes across threads and tells you which conversations touched it.

Here's how we've been using it to find threads:

- "Which Amp thread created core/src/tools/tool-service.ts?"
- "Search my threads to find the one in which we added the explosion animation. I want to continue working on that."
- "Show me all threads that modified src/terminal/pty.rs"
- "Find and read the thread that created scripts/deploy.sh."
- "Find the thread in which we added @server/src/backfill-service.ts, read it, and extract the SQL snippet we used to test the migration."

The `find_thread` tool is the sibling to `read_thread`. First you find, then you read. Together they turn your Amp threads into reusable context.

![Find thread in action](https://static.ampcode.com/news/find-thread.png)
