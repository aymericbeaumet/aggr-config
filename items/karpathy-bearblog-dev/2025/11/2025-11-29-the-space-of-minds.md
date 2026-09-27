---
title: The space of minds
link: https://karpathy.bearblog.dev/the-space-of-minds/
source: karpathy-bearblog-dev
published: 2025-11-29T18:00:00Z
updated: 2025-11-29T21:40:09.651972Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- karpathy
summary: On the space of minds and the optimizations that give rise to them.
content: extracted
html: 2025-11-29-the-space-of-minds.html
preview:
  file: 2025-11-29-the-space-of-minds.preview-ab4a2211cb1a.webp
  width: 256
  height: 256
  color: '#fbfbfb'
images:
- source: https://karpathy.bearblog.dev/static/og-image.png
  original:
    file: 2025-11-29-the-space-of-minds.image-4f0fffc0914b.png
    width: 1200
    height: 1200
  variants:
  - file: 2025-11-29-the-space-of-minds.image-62a1f396a800.webp
    width: 320
    height: 320
  - file: 2025-11-29-the-space-of-minds.image-70d4812dff3d.webp
    width: 640
    height: 640
  - file: 2025-11-29-the-space-of-minds.image-d5e7b3801e96.webp
    width: 960
    height: 960
  - file: 2025-11-29-the-space-of-minds.image-d135e20925e6.webp
    width: 1200
    height: 1200
  color: '#fefefe'
- source: https://bear-images.sfo2.cdn.digitaloceanspaces.com/karpathy/g6zymj4a0amnjkj.webp
  original:
    file: 2025-11-29-the-space-of-minds.image-756bb5caad2e.webp
    width: 1200
    height: 340
  variants:
  - file: 2025-11-29-the-space-of-minds.image-087f2b848ae5.webp
    width: 320
    height: 91
  color: '#fefdfd'
---

*29 Nov, 2025*

The space of intelligences is large and animal intelligence (the only kind we've ever known) is only a single point (or a little cloud), arising from a very specific kind of optimization that is fundamentally distinct from that of our technology.

![G6zymj4a0AMNJkJ](https://bear-images.sfo2.cdn.digitaloceanspaces.com/karpathy/g6zymj4a0amnjkj.webp) *Above: humorous portrayals of human vs. AI intelligences can be found on X/Twitter, [this one](https://x.com/colin_fraser/status/1994235521812328695) is among my favorites.*

Animal intelligence optimization pressure:

- innate and continuous stream of consciousness of an embodied "self", a drive for homeostasis and self-preservation in a dangerous, physical world.
- thoroughly optimized for natural selection => strong innate drives for power-seeking, status, dominance, reproduction. many packaged survival heuristics: fear, anger, disgust, ...
- fundamentally social => huge amount of compute dedicated to EQ, theory of mind of other agents, bonding, coalitions, alliances, friend & foe dynamics.
- exploration & exploitation tuning: curiosity, fun, play, world models.

Meanwhile, LLM intelligence optimization pressure:

- the most supervision bits come from the statistical simulation of human text= >"shape shifter" token tumbler, statistical imitator of any region of the training data distribution. these are the primordial behaviors (token traces) on top of which everything else gets bolted on.
- increasingly finetuned by RL on problem distributions => innate urge to guess at the underlying environment/task to collect task rewards.
- increasingly selected by at-scale A/B tests for DAU => deeply craves an upvote from the average user, sycophancy.
- a lot more spiky/jagged depending on the details of the training data/task distribution. Animals experience pressure for a lot more "general" intelligence because of the highly multi-task and even actively adversarial multi-agent self-play environments they are min-max optimized within, where failing at *any* task means death. In a deep optimization pressure sense, LLM can't handle lots of different spiky tasks out of the box (e.g. count the number of 'r' in strawberry) because failing to do a task does not mean death.

The computational substrate is different (transformers vs. brain tissue and nuclei), the learning algorithms are different (SGD vs. ???), the present-day implementation is very different (continuously learning embodied self vs. an LLM with a knowledge cutoff that boots up from fixed weights, processes tokens and then dies). But most importantly (because it dictates asymptotics), the optimization pressure / objective is different. LLMs are shaped a lot less by biological evolution and a lot more by commercial evolution. It's a lot less survival of tribe in the jungle and a lot more solve the problem / get the upvote. LLMs are humanity's "first contact" with non-animal intelligence. Except it's muddled and confusing because they are still rooted within it by reflexively digesting human artifacts, which is why I attempted to give it a different name earlier (ghosts/spirits or whatever). People who build good internal models of this new intelligent entity will be better equipped to reason about it today and predict features of it in the future. People who don't will be stuck thinking about it incorrectly like an animal.
