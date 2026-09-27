---
title: The Cost of Overfitting the Harness
link: https://www.dbreunig.com/2026/05/10/overfitting-the-harness.html
source: dbreunig-com
published: 2026-05-10T16:26:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
summary: As labs train their own harness behavior into models and drop fine-tuning, frontier models start to look like appliances, not platforms.
content: extracted
html: 2026-05-10-the-cost-of-overfitting-the-harness.html
preview:
  file: 2026-05-10-the-cost-of-overfitting-the-harness.preview-d97b425e75c2.webp
  width: 256
  height: 134
  color: '#7a7062'
images:
- source: https://www.dbreunig.com/img/sf_beach_og.jpg
  original:
    file: 2026-05-10-the-cost-of-overfitting-the-harness.image-246e578187ce.jpg
    width: 1200
    height: 630
  color: '#1a130a'
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

OpenAI [winding down fine tuning](https://x.com/bradenjhancock/status/2053309599248453999?s=20) is an interesting development and one to watch.

On one hand, model maximalists will argue the largest models keep getting better at more things, so the need to adjust the weights of them is less necessary.

On the other hand, the big labs keep pushing their models to a handful of use cases while training their harness designs into the model, rendering them less generalized. There’s an argument *this is fine*, because coding and reasoning abilities will solve most other problems.

But what we end up with are models build for their own harnesses. [Mario Zechner](https://x.com/badlogicgames/status/2052496187006054847?s=20) was wrestling with GPT in the OSS [Pi harness](https://pi.dev) this week, trying to wrangle out specific in-harness behaviors, with Claude fighting him every step of the way.

If this continues, there’s a world where 3rd party harnesses become less valuable when used with frontier lab models because the [1st party harness behavior is already *baked in*](https://www.dbreunig.com/2025/06/03/comparing-system-prompts-across-claude-versions.html). And there’s no longer a fine tuning escape hatch to generalize this behavior away.

In this world, frontier models will resemble appliances, not general platforms[^1]. With their harness trained in and no ability to adjust it? This might make application building easier for some enterprises, but the trade off is lock in. For many, improved reliability will be worth it.

[^1]: I’m reminded of John Siracusa’s “[Naked Robotic Core](https://hypercritical.fireside.fm/86)” model for the iPhone, that it ideally is a common denominator device that can support many shapes of applications and interfaces.
