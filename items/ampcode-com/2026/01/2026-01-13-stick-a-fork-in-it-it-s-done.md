---
title: Stick a Fork in It, It's Done
link: https://ampcode.com/news/stick-a-fork-in-it
source: ampcode-com
published: 2026-01-13T00:00:00Z
updated: 2026-01-13T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Update (2025-05-06): Four months after this was posted, we removed handoff. So, if you want to fork a thread, the other recommendation here stands: make a new thread and mention the original thread by URL or ID. We''re ripping out the Fork command. We added thread forking back in July 2025 (now ancient, primordial history) as a way to conveniently share context for branching experiments or side quests in Amp. Today we have better ways of sharing context between threads: handoff and thread mentions, which treat threads as first-class stores of context. Perhaps there is a great potential UX out there for fork, but we want Amp to be simple as well as powerful. We''d rather spend our time perfecting handoff and thread mentions than support fork. Handoff Handoff is great for extracting useful context from your thread for the next goal at hand. This means you can start a new thread with only the necessary context. Use thread: handoff from the command palette. Prompt your task and a new thread will be started with the necessary context already in the prompt. Thread Mentions Thread mentions let you pull information from other threads into your current thread. You can reference multiple threads, merging context from many sources. Use thread:new and then use the enter shortcut to start a new thread with a reference to the main thread. Or, use @@ to search for the thread you want to pull context from. Once you run your prompt, Amp will read the threads and extract context pertinent to your task. Managing Threads Using new threads as branches leads to many threads, often running in parallel. To manage them: use thread: switch to previous or thread: switch to parent to return to the main thread. use the thread: map to get a birds eye view and easily navigate back to the main thread (CLI only for now).'
content: extracted
html: 2026-01-13-stick-a-fork-in-it-it-s-done.html
preview:
  file: 2026-01-13-stick-a-fork-in-it-it-s-done.preview-f069464fba11.webp
  width: 256
  height: 134
  color: '#5d564c'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Stick+a+Fork+in+It%2C+It%27s+Done&date=January+13%2C+2026&tagline=We%27re+ripping+out+the+Fork+command&sig=6bb79f2412b6d2b0c5bb3c3b5d9af4baefa3eaca705ebc4c7f4a0ed8d9b28a6c
  original:
    file: 2026-01-13-stick-a-fork-in-it-it-s-done.image-f3a74cff1738.png
    width: 1200
    height: 630
  variants:
  - file: 2026-01-13-stick-a-fork-in-it-it-s-done.image-fc6e224fe36f.webp
    width: 320
    height: 168
  - file: 2026-01-13-stick-a-fork-in-it-it-s-done.image-f51037e3c109.webp
    width: 640
    height: 336
  - file: 2026-01-13-stick-a-fork-in-it-it-s-done.image-e439fbe2716a.webp
    width: 960
    height: 504
  - file: 2026-01-13-stick-a-fork-in-it-it-s-done.image-21b5d3d09fa6.webp
    width: 1200
    height: 630
  color: '#221b16'
---

**Update (2025-05-06):** Four months after this was posted, we removed handoff. So, if you want to fork a thread, the other recommendation here stands: make a new thread and mention the original thread by URL or ID.

We're ripping out the Fork command.

We added [thread forking](https://ampcode.com/news/thread-forking) back in July 2025 (now ancient, primordial history) as a way to conveniently share context for branching experiments or side quests in Amp.

Today we have better ways of sharing context between threads: [handoff](https://ampcode.com/manual#handoff) and [thread mentions](https://ampcode.com/manual#referencing-threads), which treat threads as first-class stores of context.

Perhaps there is a great potential UX out there for `fork`, but we want Amp to be simple as well as powerful. We'd rather spend our time perfecting `handoff` and `thread mentions` than support `fork`.

## Handoff

[Handoff](https://ampcode.com/news/handoff) is great for extracting useful context from your thread for the next goal at hand. This means you can start a new thread with only the necessary context.

- Use `thread: handoff` from the command palette.
- Prompt your task and a new thread will be started with the necessary context already in the prompt.

## Thread Mentions

Thread mentions let you pull information from other threads into your current thread. You can reference multiple threads, merging context from many sources.

- Use `thread:new` and then use the `enter` shortcut to start a new thread with a reference to the main thread.
- Or, use `@@` to search for the thread you want to pull context from.
- Once you run your prompt, Amp will [read the threads](https://ampcode.com/news/read-threads) and extract context pertinent to your task.

## Managing Threads

Using new threads as branches leads to many threads, often running in parallel. To manage them:

- use `thread: switch to previous` or `thread: switch to parent` to return to the main thread.
- use the [`thread: map`](https://ampcode.com/news/thread-map) to get a birds eye view and easily navigate back to the main thread (CLI only for now).
