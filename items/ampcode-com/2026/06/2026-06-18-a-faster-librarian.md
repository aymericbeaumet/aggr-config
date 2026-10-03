---
title: A Faster Librarian
link: https://ampcode.com/news/a-faster-librarian
source: ampcode-com
published: 2026-06-18T00:00:00Z
updated: 2026-06-18T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'The Librarian is now ~3x faster and 43% cheaper, with the same quality. It now runs on GPT-5.5 (no reasoning) with websocket mode and an updated system prompt that encourages more parallel exploration. The Librarian fires ~8 tool calls in parallel per turn, up from ~3 with Sonnet, and wraps up a search in ~5 turns instead of ~15. In our internal eval, about a quarter of that speedup comes from OpenAI''s websocket mode and the rest from switching to GPT-5.5 with no reasoning: Sonnet-4.6 (medium) GPT-5.5 (none) Latency (mean) 237s 81s (2.9x faster) ↳ gain from websocket — ~1.3x ↳ gain from model — ~2.2x Quality (F1, mean) 0.47 0.48 Average cost $1.21 $0.69 Here''s a comparison: How does Kubernetes'' HorizontalPodAutoscaler handle missing pod metrics when scaling down — does it assume missing pods are at 100% of their resource requests, or 100% of the target utilization? Cite the function and logic in the source. Sonnet 4.6 (left) took 2 minutes and cost $1.08, while GPT-5.5 (right) took 40 seconds and cost just $0.47.'
content: extracted
html: 2026-06-18-a-faster-librarian.html
preview:
  file: 2026-06-18-a-faster-librarian.preview-0c92a1d5ef98.webp
  width: 256
  height: 134
  color: '#5a5247'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=A+Faster+Librarian&date=June+18%2C+2026&tagline=The+Librarian+is+%7E3x+faster+and+43%25+cheaper.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Flibrarian-latency.png&sig=d66ead20f4379baa6619604a9768c729ec5ecec0e47a1b19a33f22d13fd738ab
  original:
    file: 2026-06-18-a-faster-librarian.image-db1125ca7c18.png
    width: 1200
    height: 630
  variants:
  - file: 2026-06-18-a-faster-librarian.image-659d5530883e.webp
    width: 320
    height: 168
  - file: 2026-06-18-a-faster-librarian.image-6ae30255b49f.webp
    width: 640
    height: 336
  - file: 2026-06-18-a-faster-librarian.image-81c01d6542bb.webp
    width: 960
    height: 504
  - file: 2026-06-18-a-faster-librarian.image-48009fb5e2a6.webp
    width: 1200
    height: 630
  color: '#231e1a'
---

The [Librarian](https://ampcode.com/news/librarian) is now ~3x faster and 43% cheaper, with the same quality.

It now runs on GPT-5.5 (no reasoning) with [websocket mode](https://ampcode.com/news/faster-deep-rush) and an updated system prompt that encourages more parallel exploration. The Librarian fires ~8 tool calls in parallel per turn, up from ~3 with Sonnet, and wraps up a search in ~5 turns instead of ~15.

In our internal eval, about a quarter of that speedup comes from OpenAI's websocket mode and the rest from switching to GPT-5.5 with no reasoning:

|                       | Sonnet-4.6 (medium) | GPT-5.5 (none)    |
| --------------------- | ------------------- | ----------------- |
| Latency (mean)        | 237s                | 81s (2.9x faster) |
| ↳ gain from websocket | —                   | \~1.3x            |
| ↳ gain from model     | —                   | \~2.2x            |
| Quality (F1, mean)    | 0.47                | 0.48              |
| Average cost          | $1.21               | $0.69             |

Here's a comparison:

> How does Kubernetes' HorizontalPodAutoscaler handle missing pod metrics when scaling down — does it assume missing pods are at 100% of their resource requests, or 100% of the target utilization? Cite the function and logic in the source.

Sonnet 4.6 (left) took 2 minutes and cost $1.08, while GPT-5.5 (right) took 40 seconds and cost just $0.47.
