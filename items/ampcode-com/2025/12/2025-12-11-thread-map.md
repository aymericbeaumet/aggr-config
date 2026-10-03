---
title: Thread Map
link: https://ampcode.com/news/thread-map
source: ampcode-com
published: 2025-12-11T00:00:00Z
updated: 2025-12-11T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Two days ago, Lewis wrote about working with short threads — a lot of them, connected via handoff and thread mentions and forks. In his post, he showed a diagram, a map of one feature spread across 13 threads. That map? It exists now, we built it: Run threads: map in the Amp CLI command palette to try it out. You''ll see a top-down view of all threads connected to your current thread via mentions, handoffs, or forks. If you hit Enter, you''ll open the selected thread and can continue your work there. (Yes, it''s only available in the Amp CLI right now, but coming to other clients soon.) If you only use handoff and forks occasionally, you might not need this yet. But if you do work with many short, connected threads — like Lewis or Igor — this map might make it even easier, because you can see the shape of your work. Here are some patterns we''ve noticed so far: 1. Hub-and-Spokes One thread will form a core from which many other threads can be created. This might be an initial implementation thread, or a context-gathering research thread. The spokes might be refactoring threads, or subfeatures. They don''t need the context of the other spokes—by linking only to the hub thread, the context window of each spoke remains lean and relevant. 2. Chain Many short threads chained together. This is a common pattern when one change depends on another. This pattern often emerges when using the handoff feature to extract only the relevant context from a previous thread, allowing you to keep threads short but still continue serially dependent work. This is common in research or exploratory tasks, where the desired state is unknown. It''s not uncommon for the end of a chain to lead to the central node of a hub-and-spokes pattern; a desired state is found and work can be more easily parallelised. What''s Next? Our bet is that there are many more patterns out there, waiting to be recognised. Let us know what you find.'
content: extracted
html: 2025-12-11-thread-map.html
preview:
  file: 2025-12-11-thread-map.preview-ea249e7542c7.webp
  width: 256
  height: 134
  color: '#5d554c'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Thread+Map&date=December+11%2C+2025&tagline=A+birdseye+view+of+the+threads+you+forked%2C+handed+off%2C+and+referenced&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fthread-map-screenshot.png&sig=204f6f364179d636442f3aa8c1bb0a1d017a65b247e6414207dae6f1f8b70e7b
  original:
    file: 2025-12-11-thread-map.image-b0f0541a6ed4.png
    width: 1200
    height: 630
  variants:
  - file: 2025-12-11-thread-map.image-df9e5f71fac3.webp
    width: 320
    height: 168
  - file: 2025-12-11-thread-map.image-95fe686b5ad6.webp
    width: 640
    height: 336
  - file: 2025-12-11-thread-map.image-59ae2aedd175.webp
    width: 960
    height: 504
  - file: 2025-12-11-thread-map.image-d90874659c2e.webp
    width: 1200
    height: 630
  color: '#211c1b'
- source: https://ampcode.com/(marketing)/news/thread-map2.jpg
  original:
    file: 2025-12-11-thread-map.image-e59d86e3cb69.jpg
    width: 643
    height: 676
  color: '#fefefe'
- source: https://static.ampcode.com/news/thread-map-bicycle-spokes.png
  original:
    file: 2025-12-11-thread-map.image-3e626bffd5af.png
    width: 940
    height: 762
  color: '#292c33'
- source: https://static.ampcode.com/news/thread-map-chains.png
  original:
    file: 2025-12-11-thread-map.image-f8b1243e1c49.png
    width: 630
    height: 762
  color: '#292c33'
- source: https://static.ampcode.com/news/thread-map-patterns-slow.gif
  original:
    file: 2025-12-11-thread-map.image-dbe118ab0568.gif
    width: 300
    height: 300
  color: '#282b32'
---

Two days ago, Lewis wrote about [working with short threads](https://ampcode.com/200k-tokens-is-plenty) — a lot of them, connected via [handoff](https://ampcode.com/manual#handoff) and [thread mentions](https://ampcode.com/manual#referencing-threads) and forks. In his post, he showed a diagram, a *map* of one feature spread across 13 threads.

That map? It exists now, we built it:

![Old whisperer of the orb reading a thread map](https://ampcode.com/\(marketing\)/news/thread-map2.jpg)

Run `threads: map` in the [Amp CLI command palette](https://ampcode.com/news/command-palette) to try it out.

You'll see a top-down view of all threads connected to your current thread via mentions, handoffs, or forks. If you hit `Enter`, you'll open the selected thread and can continue your work there.

(Yes, it's only available in the Amp CLI right now, but coming to other clients soon.)

If you only use handoff and forks occasionally, you might not need this yet. But if you do work with many short, connected threads — like [Lewis](https://ampcode.com/200k-tokens-is-plenty) or [Igor](https://x.com/bedesqui/status/1998374596144406754) — this map might make it even easier, because you can see the *shape* of your work.

Here are some patterns we've noticed so far:

## 1\. Hub-and-Spokes

![Bicycle spokes pattern](https://static.ampcode.com/news/thread-map-bicycle-spokes.png)

One thread will form a core from which many other threads can be created. This might be an initial implementation thread, or a context-gathering research thread. The spokes might be refactoring threads, or subfeatures. They don't need the context of the other spokes—by linking only to the hub thread, the context window of each spoke remains lean and relevant.

## 2\. Chain

![Chains pattern](https://static.ampcode.com/news/thread-map-chains.png)

Many short threads chained together. This is a common pattern when one change depends on another. This pattern often emerges when using the `handoff` feature to extract only the relevant context from a previous thread, allowing you to keep threads short but still continue serially dependent work. This is common in research or exploratory tasks, where the desired state is unknown.

It's not uncommon for the end of a chain to lead to the central node of a hub-and-spokes pattern; a desired state is found and work can be more easily parallelised.

## What's Next?

Our bet is that there are many more patterns out there, waiting to be recognised. Let us know what you find.

![Thread map patterns](https://static.ampcode.com/news/thread-map-patterns-slow.gif)
