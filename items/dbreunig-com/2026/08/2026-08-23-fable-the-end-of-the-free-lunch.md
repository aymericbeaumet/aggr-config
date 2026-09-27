---
title: Fable & The End of the Free Lunch
link: https://www.dbreunig.com/2026/08/23/fable-the-end-of-moore-s-law.html
source: dbreunig-com
published: 2026-08-23T18:35:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- fable
- glm
- harnesses
- routers
summary: Fable's price ended the free lunch of ever-cheaper frontier models. Now we have to think about which model gets which work.
content: extracted
html: 2026-08-23-fable-the-end-of-the-free-lunch.html
preview:
  file: 2026-08-23-fable-the-end-of-the-free-lunch.preview-d97b425e75c2.webp
  width: 256
  height: 134
  color: '#7a7062'
images:
- source: https://www.dbreunig.com/img/sf_beach_og.jpg
  original:
    file: 2026-08-23-fable-the-end-of-the-free-lunch.image-246e578187ce.jpg
    width: 1200
    height: 630
  color: '#1a130a'
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

There’s some talk today about how agentic coders are balking at Anthropic’s pricing and adopting alternatives. I was reminded of a thought I had in the weeks following Fable’s release: the free lunch was over.

When [Moore’s Law](https://en.wikipedia.org/wiki/Moore%27s_law) was in full effect, it didn’t make sense to ruthlessly optimize your code. In 18 months, a CPU would arrive that would double your performance. Herb Sutter famously referred to this as, “the free lunch,” in a [seminal essay](http://www.gotw.ca/publications/concurrency-ddj.htm).

When Moore’s Law slowed in the mid-2000s (specifically, single-threaded performance stagnated), we suddenly had to think about parallelization, architecture, memory locality, etc.

*We had to think about what work went where.*

Prior to Fable, it felt silly to waste *too* much time improving your coding harness or context strategies. A new model would arrive at the same price (or cheaper!) and paper over most of your problems.

But then Fable landed. It was (and still is!) *incredible*. But the cost was so high and Opus was *good enough* (as was 5.6, K3, and even GLM) for *most* of the code we needed.

*So we started to think about what work went where.*

GLM 5.2 is worth focusing on. It came out the same week as Fable and is roughly 1/9th the cost (and ~1/5th the cost of Opus 5). Is GLM 1/9th the quality of Fable? Perhaps, for certain classes of tasks. But for most rote coding it’s more than sufficient. *Especially* when provided with great context. I frequently chat with Fable to interrogate and shape a design, before handing off a brief to GLM.

I get pushback that falling inference prices will eventually bring us back to sending everything through the largest models. But I’m not so sure: those same gains will benefit the K3s and Qwens, and as we [continue to develop better harnesses](https://www.dbreunig.com/2026/08/14/harnesses-are-situated-agents.html) it will be easier to provide weaker (but still great) models with sufficient context to perform well.

Plus, Fable’s *other* shock likely locks in this change. Fable’s access controls, dynamic degradation, and required data retention spooked enough companies (and countries!) into thinking about where they send their traces and where they get their tokens.
