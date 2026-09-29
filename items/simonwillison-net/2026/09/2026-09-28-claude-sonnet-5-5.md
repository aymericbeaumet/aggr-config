---
title: Claude Sonnet 5.5
link: https://simonwillison.net/2026/Sep/28/claude-sonnet-5-5/
source: simonwillison-net
published: 2026-09-28T22:07:38Z
updated: 2026-09-28T22:07:38Z
first_seen: 2026-09-29T01:16:22.439760281Z
labels:
- ai
- anthropic
- claude
- generative-ai
- llm-release
- llms
- pelican-riding-a-bicycle
summary: 'Claude Sonnet 5.5 New Sonnet model from Anthropic today. They say it "runs 30%+ faster, and costs up to 30% less for most work" - it''s priced the same as Sonnet 5 but appears to beat it on every benchmark, and should be cheaper to run as well. Here are some pelicans riding bicycles. Sonnet 5.5 suffered from the same bug as Opus 5.5: the "max" thinking effort pelican thought for 128,000 tokens (at a cost of $1.28) before running out of tokens and failing to produce an SVG. Here''s the pelican it gave me for thinking effort "xhigh", at a cost of 5.74 cents and taking 41 seconds: Sonnet 5.5 appears to be almost as good as Opus 5.5 on some coding tasks, including various viral 3D animation tricks. The most interesting thing about Sonnet 5.5 is that it''s now the model used for the free tier on claude.ai. OpenAI''s ChatGPT free tier uses Luna 5.6, which means Anthropic currently have a much more capable free offering. I ran this prompt against that free tier: build me an HTML page that renders a three-dimensional pelican riding a bicycle using WebGL And got back this page, which is a solid effort. Anthropic''s announcement reiterates that Haiku 5.5 will be available "in the coming weeks". I really hope that one is price-competitive with GPT-6 Luna! Tags: ai, generative-ai, llms, anthropic, claude, pelican-riding-a-bicycle, llm-release'
content: extracted
html: 2026-09-28-claude-sonnet-5-5.html
preview:
  file: 2026-09-28-claude-sonnet-5-5.preview-44cfabd5ad50.webp
  width: 256
  height: 192
  alt: It's good- correct bicycle frame, legs either side of the frame, feet touching the pedals, chain in the right place, it is wearing a misshapen blue bicycle helmet though.
  color: '#a7cccd'
images:
- source: https://static.simonwillison.net/static/2026/claude-sonnet-5.5-pelican-xhigh.webp
  original:
    file: 2026-09-28-claude-sonnet-5-5.image-ebf4d664722b.webp
    width: 914
    height: 686
  color: '#8ac248'
---

**[Claude Sonnet 5.5](https://www.anthropic.com/claude-sonnet-5-5)**. New Sonnet model from Anthropic today. They say it "runs 30%+ faster, and costs up to 30% less for most work" - it's priced the same as Sonnet 5 but appears to beat it on every benchmark, and should be cheaper to run as well.

[Here are some pelicans riding bicycles](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F1d85a9be7f3ecce26e7f1569161a0d01). Sonnet 5.5 suffered from [the same bug as Opus 5.5](https://simonwillison.net/2026/Sep/22/opus-and-sol-and-luna/#claude-opus-5-5-max-over-thinks-to-the-point-of-breaking): the "max" thinking effort pelican thought for 128,000 tokens (at a cost of $1.28) before running out of tokens and failing to produce an SVG.

Here's the pelican it gave me for thinking effort "xhigh", at a cost of 5.74 cents and taking 41 seconds:

![It's good- correct bicycle frame, legs either side of the frame, feet touching the pedals, chain in the right place, it is wearing a misshapen blue bicycle helmet though.](https://static.simonwillison.net/static/2026/claude-sonnet-5.5-pelican-xhigh.webp)

Sonnet 5.5 appears to be almost as good as Opus 5.5 on some coding tasks, including various [viral 3D animation tricks](https://x.com/claudeai/status/2104674987164782598).

The most interesting thing about Sonnet 5.5 is that it's now the model used for the free tier on [claude.ai](https://claude.ai/). OpenAI's ChatGPT free tier uses Luna 5.6, which means Anthropic currently have a much more capable free offering.

I ran this prompt against that free tier:

> `build me an HTML page that renders a three-dimensional pelican riding a bicycle using WebGL`

And got back [this page](https://static.simonwillison.net/static/2026/claude-sonnet-5.5-free-3d-pelican.html), which is a solid effort.

Anthropic's announcement reiterates that Haiku 5.5 will be available "in the coming weeks". I really hope that one is price-competitive with GPT-6 Luna!
