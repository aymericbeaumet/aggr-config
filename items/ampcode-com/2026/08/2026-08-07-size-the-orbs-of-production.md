---
title: Size the Orbs of Production!
link: https://ampcode.com/news/size-the-orbs-of-production
source: ampcode-com
published: 2026-08-07T00:00:00Z
updated: 2026-08-07T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'People are using a lot of orbs. We love to see that. We''ve shipped a lot of things so that Amp subscriptions keep covering the whole month of orb usage for almost everyone, even as orb usage grows quickly. Way back on July 27, we cut orb prices by 20% for everyone. This week, we shipped a lot more improvements. We added a new a1.medium size with 4 CPUs and 8 GB of memory. It is 50% cheaper and a better fit for most projects than the previous a0.medium. Orbs now auto-pause after 5 minutes of inactivity, down from 15 minutes. We''ve sped up orb startup time considerably, especially when another team member has recently created an orb in the same Amp project. You can now choose which orb size to use per-thread, so you can pick a smaller default to save money but go big for especially resource-intensive work. When starting new orb threads from the Amp CLI with amp -ox ''...'', the new flag --orb-size <size> lets you specify which orb size to use (instead of the project''s default). When asking the agent to create other threads, you can now tell it to use smaller (or larger) orbs, which lets you use smaller orbs for simpler fan-out tasks on projects. Prices for orbs have gone down or stayed the same at every level and for every compute/memory combination. The new set of orb sizes is: a1.tiny: 1 CPU · 2 GB memory · $0.08/hour a1.small: 2 CPUs · 4 GB memory · $0.17/hour a1.medium: 4 CPUs · 8 GB memory · $0.33/hour a1.large: 8 CPUs · 16 GB memory · $0.66/hour a1.xxlarge: 16 CPUs · 32 GB memory · $1.32/hour We''ve automatically upgraded projects to the equivalent new orb sizes. If you want to use a different orb size, you can update your projects'' settings on the web, or ask Amp to do so using the amp projects subcommands.'
content: extracted
html: 2026-08-07-size-the-orbs-of-production.html
preview:
  file: 2026-08-07-size-the-orbs-of-production.preview-7662cde738e9.webp
  width: 256
  height: 134
  color: '#635847'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Size+the+Orbs+of+Production%21&date=August+7%2C+2026&tagline=Use+orbs+more+for+less.+We%27ve+shipped+a+lot+of+improvements+to+make+orbs+cheaper+and+more+efficient.&backgroundImage=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fsize-the-orbs-of-production.jpg&sig=4d8f26cf76a18d54e06593756ce7846264576e371a1a300755ddca4f27134ea6
  original:
    file: 2026-08-07-size-the-orbs-of-production.image-5d0971747a64.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-07-size-the-orbs-of-production.image-193c1dd0e8cc.webp
    width: 320
    height: 168
  - file: 2026-08-07-size-the-orbs-of-production.image-285e2a2f5211.webp
    width: 640
    height: 336
  - file: 2026-08-07-size-the-orbs-of-production.image-ed2a0906c747.webp
    width: 960
    height: 504
  - file: 2026-08-07-size-the-orbs-of-production.image-fb54fb24adac.webp
    width: 1200
    height: 630
  color: '#231b11'
- source: https://static.ampcode.com/news/size-the-orbs-of-production-dark.png?v=2
  original:
    file: 2026-08-07-size-the-orbs-of-production.image-3d3fdce4bee8.png
    width: 1806
    height: 922
  variants:
  - file: 2026-08-07-size-the-orbs-of-production.image-580330d4b7ff.webp
    width: 320
    height: 163
  - file: 2026-08-07-size-the-orbs-of-production.image-5328abc48d97.webp
    width: 640
    height: 327
  - file: 2026-08-07-size-the-orbs-of-production.image-937277a9c035.webp
    width: 960
    height: 490
  - file: 2026-08-07-size-the-orbs-of-production.image-77e40138ed7a.webp
    width: 1280
    height: 653
  - file: 2026-08-07-size-the-orbs-of-production.image-badffc07edee.webp
    width: 1600
    height: 817
  - file: 2026-08-07-size-the-orbs-of-production.image-b260ed265bc3.webp
    width: 1806
    height: 922
  color: '#0c1515'
---

People are using a lot of [orbs](https://ampcode.com/manual/orbs). We love to see that. We've shipped a lot of things so that [Amp subscriptions](https://ampcode.com/pricing) keep covering the whole month of orb usage for almost everyone, even as orb usage grows quickly.

Way back on July 27, we [cut orb prices by 20% for everyone](https://x.com/sqs/status/2081807392040452225).

This week, we shipped a lot more improvements.

We added a new `a1.medium` size with 4 CPUs and 8 GB of memory. It is 50% cheaper and a better fit for most projects than the previous `a0.medium`.

Orbs now auto-pause after 5 minutes of inactivity, down from 15 minutes.

We've sped up orb startup time considerably, especially when another team member has recently created an orb in the same Amp project.

You can now choose which orb size to use per-thread, so you can pick a smaller default to save money but go big for especially resource-intensive work.

![The new thread dialog with the five a1 orb sizes](https://static.ampcode.com/news/size-the-orbs-of-production-dark.png?v=2)

When starting new orb threads from the Amp CLI with [`amp -ox '...'`](https://ampcode.com/manual/orbs#amp-ox), the new flag `--orb-size <size>` lets you specify which orb size to use (instead of the project's default).

When asking the agent to create other threads, you can now tell it to use smaller (or larger) orbs, which lets you use smaller orbs for simpler fan-out tasks on projects.

[Prices for orbs](https://ampcode.com/manual/orbs#pricing) have gone down or stayed the same at every level and for every compute/memory combination. The new set of orb sizes is:

- `a1.tiny`: 1 CPU · 2 GB memory · $0.08/hour
- `a1.small`: 2 CPUs · 4 GB memory · $0.17/hour
- `a1.medium`: 4 CPUs · 8 GB memory · $0.33/hour
- `a1.large`: 8 CPUs · 16 GB memory · $0.66/hour
- `a1.xxlarge`: 16 CPUs · 32 GB memory · $1.32/hour

We've automatically upgraded projects to the equivalent new orb sizes. If you want to use a different orb size, you can update your projects' settings on the web, or ask Amp to do so using the `amp projects` subcommands.
