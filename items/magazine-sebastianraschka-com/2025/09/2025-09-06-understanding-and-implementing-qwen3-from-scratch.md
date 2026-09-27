---
title: Understanding and Implementing Qwen3 From Scratch
link: https://magazine.sebastianraschka.com/p/qwen3-from-scratch
source: magazine-sebastianraschka-com
published: 2025-09-06T11:10:21Z
updated: 2025-09-06T11:10:21Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Sebastian Raschka, PhD
summary: A Detailed Look at One of the Leading Open-Source LLMs
content: extracted
html: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.html
preview:
  file: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.preview-31ae501ce6a7.webp
  width: 256
  height: 161
  color: '#babec0'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/00515b82-fd01-4c5b-b1b9-2d4b38100be0_1236x775.png
  original:
    file: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.image-f35d7d4cd2d7.png
    width: 1236
    height: 775
  variants:
  - file: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.image-c751f35ac3b2.webp
    width: 320
    height: 201
  - file: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.image-a30fe3ec8396.webp
    width: 640
    height: 401
  - file: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.image-cf93e0cdf894.webp
    width: 960
    height: 602
  - file: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.image-2929874402b5.webp
    width: 1236
    height: 775
  color: '#fdfefe'
- source: https://substackcdn.com/image/fetch/$s_!_APX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb3d5266-b65a-45ea-9f8f-98d7d8038b8e_3432x1536.png
  original:
    file: 2025-09-06-understanding-and-implementing-qwen3-from-scratch.image-0f776a4992bf.jpg
    width: 1456
    height: 652
  color: '#fbfcfc'
---

Previously, I compared the most notable open-weight architectures of 2025 in [The Big LLM Architecture Comparison](https://magazine.sebastianraschka.com/p/the-big-llm-architecture-comparison). Then, I zoomed in and discussed the various architecture components in [From GPT-2 to gpt-oss: Analyzing the Architectural Advances](https://magazine.sebastianraschka.com/p/from-gpt-2-to-gpt-oss-analyzing-the) on a conceptual level.

Since all good things come in threes, before covering some of the noteworthy research highlights of this summer, I wanted to now dive into these architectures hands-on, in code. By following along, you will understand how it actually works under the hood and gain building blocks you can adapt for your own experiments or projects.

For this, I picked Qwen3 ( [initially released in May](https://arxiv.org/abs/2505.09388) and updated in July) because it is one of the most widely liked and used open-weight model families as of this writing.

The reasons why Qwen3 models are so popular are, in my view, as follows:

1. A developer- and commercially friendly open-source ( [Apache License v2.0](https://huggingface.co/Qwen/Qwen3-0.6B/blob/main/LICENSE)) without any strings attached beyond the original open-source license terms (some other open-weight LLMs impose additional usage limits)

2. The performance is really good; for example, as of this writing, the open-weight 235B-Instruct variant is ranked 8 on the [LMArena leaderboard](https://lmarena.ai/leaderboard/text), tied with the proprietary Claude Opus 4. The only 2 other open-weight LLMs that rank higher are DeepSeek 3.1 (3x larger) and Kimi K2 (4x larger). On September 5th, [Qwen3 released a 1T parameter “max” variant](https://x.com/Alibaba_Qwen/status/1963991502440562976) on their platform that beats Kimi K2, DeepSeek 3.1, and Claude Opus 4 on all major benchmarks; however, this model is closed-source for now.

3. There are many different model sizes available for different compute budgets and use-cases, from 0.6B dense models to 480B parameter Mixture-of-Experts models.

This is going to be a long article due to the from-scratch code in pure PyTorch. While the code sections may look verbose, I hope that they help explain the building blocks better than conceptual figures alone!

**Tip 1:** If you are reading this article in your email inbox, the narrow line width may cause code snippets to wrap awkwardly. For a better experience, I recommend [opening it in your web browser](https://magazine.sebastianraschka.com/p/qwen3-from-scratch).

**Tip 2:** You can use the table of contents on the left side of the website for easier navigation between sections.

[![](https://substackcdn.com/image/fetch/$s_!_APX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb3d5266-b65a-45ea-9f8f-98d7d8038b8e_3432x1536.png)](https://substackcdn.com/image/fetch/$s_!_APX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbb3d5266-b65a-45ea-9f8f-98d7d8038b8e_3432x1536.png)

Figure 1: Preview of the Qwen3 Dense and Mixture-of-Experts architectures discussed and (re)implemented in pure PyTorch in this article.
