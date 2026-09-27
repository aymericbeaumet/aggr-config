---
title: Reflections on AI Engineer Summit 2023
link: https://eugeneyan.com//writing/aieng-reflections/
source: eugeneyan-com
published: 2023-10-15T00:00:00Z
updated: 2023-10-15T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- ai
- llm
- misc
summary: The biggest deployment challenges, backward compatibility, multi-modality, and SF work ethic.
content: extracted
html: 2023-10-15-reflections-on-ai-engineer-summit-2023.html
preview:
  file: 2023-10-15-reflections-on-ai-engineer-summit-2023.preview-daef9d623ba3.webp
  width: 256
  height: 134
  color: '#5c4e2e'
images:
- source: https://eugeneyan.com/assets/og_image/ai-lego.jpg
  original:
    file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-ee2d94849a1e.jpg
    width: 1200
    height: 630
  color: '#262626'
- source: https://eugeneyan.com/assets/ml-tech-debt.webp
  original:
    file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-a53532ab187a.webp
    width: 1200
    height: 411
  variants:
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-461f05d6566f.webp
    width: 320
    height: 110
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-10-15-reflections-on-ai-engineer-summit-2023.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

I recently attended the first-ever AI Engineer Summit in San Francisco. There, I gave a [talk on building blocks for LLM systems](https://eugeneyan.com/speaking/ai-eng-summit/) and also got to chat with folks on the edge of building and deploying LLM products. Here are some key takeaways:

**Evals and serving costs are the biggest challenges when deploying.** This is based on the early results of Amplify Partners’ [survey on AI Engineering](https://www.amplifypartners.com/blog-posts/ai-engineering-survey). Out of over 800 responses so far, evals and serving costs came out tops as the biggest pain points in deployment. While it’s reassuring to know that I’m not the only one struggling with this, it also means I’ll have to invest in them instead of waiting to be a fast follower.

**There is *still* no good way to do evals.** The spectrum of eval techniques is wide, ranging from deterministic yet brittle [assert statements](https://github.com/openai/evals/blob/main/docs/eval-templates.md#basic-eval-templates) to stochastic and expensive [model grading via a 3P LLM](https://github.com/openai/evals/blob/main/docs/eval-templates.md#the-model-graded-eval-template), with little in between.

Asserts check the output of LLMs for expected words or numbers. Thus, while they’re deterministic, they’re also somewhat brittle. In the Eval Driven Development workshop, they built an eval for an agent [checking a page on Amazon](https://x.com/eugeneyan/status/1711480439398084708) for the cheapest book. Their eval expected “6.78” but the model returned “$6.78”, thus failing the test case.

Another example is checking the tables used in generated SQL. I learned that LLMs, even the strong ones, sometimes ignore the list of tables provided and make up new tables in the generated SQL. A simple check is to assert that the tables in the generated SQL are limited to the list of tables provided.

But aren’t LLM outputs too open-ended to adopt an assert-based approach? Here’s Bryan Bischof (Head of AI at Hex) at a panel discussion: “If you’re not able to find a simple way to eval the responses, you’re probably not trying hard enough”. I’d agree.

An alternative to asserts is using an LLM to grade outputs. However, from folks who’ve been doing this for months (e.g., AutoGPT team), this quickly gets expensive at scale. To address this, they’ve resorted to caching responses (e.g., [VCR.py](https://vcrpy.readthedocs.io)). Thus, if we only update an isolated component in the system, we just need to eval that component and use the cached response for the rest of the system.

There’s one exception that’s relatively straightforward to eval: Code generation. We check if the code runs and produces the expected output. (This may explain the relative success of coding assistants.) Thus, perhaps code is a good medium for eval. Then, the challenge becomes formulating expected responses as code.

If we don’t have good automated evals, a useful mechanism is a periodic human eyeballing. Some LLM-based products do this weekly, on a sample of a few dozen queries, to get an intimate feel of how their product is improving or regressing week over week. This allowed them to quickly notice regressions with each model rollout and take corrective action.

**Cost-effectiveness depends on the task.** For complex tasks, LLMs are relatively cheap. For example, getting GPT-4 to write a data connector plugin would cost ~$10 and a few hours. In comparison, an average dev might take days or weeks. Counterintuitively, using LLMs to automate simple repetitive tasks, such as evals, is expensive. Thus, it makes better sense, at least for now, to use LLMs for complex work vs. basic, frequent tasks.

**LLMs need to be backward compatible with existing systems.** Just like ML was just a small part of a larger production system, LLMs will be too. [We’ve learned this lesson before](https://papers.nips.cc/paper_files/paper/2015/hash/86df7dcfd896fcaf2674f757a2463eba-Abstract.html).

![Image](https://eugeneyan.com/assets/ml-tech-debt.webp "Image")

From Hidden Technical Debt in Machine Learning Systems ([source](https://papers.nips.cc/paper_files/paper/2015/hash/86df7dcfd896fcaf2674f757a2463eba-Abstract.html))

Thus, we should think about integration sooner rather than later. JSON ([OpenAI function calling](https://platform.openai.com/docs/guides/gpt/function-calling)) and XML ([which Claude prefers](https://docs.anthropic.com/claude/docs/configuring-gpt-prompts-for-claude#add-xml-tags)) seem promising. Other tools include [Guidance](https://github.com/guidance-ai/guidance), [Instructor](https://github.com/jxnl/instructor), and [TypeChat](https://github.com/microsoft/TypeChat) though the first of them doesn’t seem actively maintained.

**Guardrails and moderation were less of a concern than expected**. Few people I interacted with were concerned about them. I think this is because most use cases focus on LLM strengths, such as creativity and analyzing complex information (e.g., marketing copy, reading docs and writing code), and less on weaknesses like factual consistency. This suggests that it may be easier to build around use cases that have lesser moderation concerns, at least for now. However, use cases in industries that have low error and risk tolerance, such as finance and healthcare, remain tricky.

(Alternatively, it could be a blind-spot that devs are overlooking. Similarly, most folks I spoke to were finetuning on 3P LLM outputs despite it being against the terms of service.)

That said, in Shreya’s talk on Guardrails, it seems that the vision for Guardrails has expanded beyond the initial [RAIL spec](https://github.com/guardrails-ai/guardrails#-rail-spec) to include guards based on ML, DL classifiers, and self-reflection. She also spoke at length on the need to have guardrails on correctness which included embedding similarity, NLI, and self-reflection. Maybe this is based on chats with users and market demand?

**There was surprisingly little discussion on caching to save costs.** The only mention was at AutoGPT’s workshop, where they discussed using VCR.py to cache responses for evals. I wonder if it’s because the inputs to most LLM workloads are so unique that the benefits of caching are negligible, or if (semantic) caching is too unreliable to work in production.

Personally, I’m counting on caching to save ≥ 50% of LLM costs for large-scale systems. These systems, such as recsys, search, and customer support, likely have repetitive inputs and outputs that can benefit from caching. We just need to find smart ways to build caches for them—[even something as naive as exact match can go a long way](https://eugeneyan.com/writing/llm-patterns/#how-to-apply-caching).

**Retrieval-augmented generation (RAG) isn’t going away soon**. There were three talks and an entire workshop dedicated to RAG databases and techniques. From the conversations and questions, it seems most teams are leaning heavily into RAG, at least for the short-to-medium term. Questions included how to combine metadata filters with vector search, how to effectively chunk/augment documents, how to eval, access control, etc.

**Everyone can code now.** With Replit making [code assistance available to everyone](https://blog.replit.com/ai4all) and [GitHub Copilot achieving ≥ 100M ARR](https://x.com/eugeneyan/status/1711789238956818444), coding assistants are here to stay. It’s getting easier to start coding—education systems would jump on this opportunity to teach the next generation how to interact with machines and data.

**2024 may be the year of multimodal.** OpenAI demoed GPT-4V converting images to text, and then using that text to generate new images. Their talk included a slide on how text will likely be the “connective tissue” that binds multi-modality.

The world is still figuring out LLMs so it’s unclear if [large multimodal models (LMMs)](https://arxiv.org/abs/2309.17421) will take off yet. Nonetheless, text as the common medium makes sense—it’s flexible and can be used to compose structured data, code, logs, etc.

**To finetune and self-host, or call APIs?** Most folks I spoke to see finetuning and self-hosting small, specialized models as the way to improve performance and reduce cost. I’m [of the same opinion](https://twitter.com/eugeneyan/status/1713372360232046915). Nonetheless, some of us wondered if 3rd-party LLMs will just improve over time like SSDs did, becoming so performant and cheap that it doesn’t make sense to use anything else. I’ll be watching closely how this plays out. 👀

**SF engineers have incredible work ethic.** I attended a Sunday 9 am workshop at an obscure co-working space: At [9 am the seats were half taken](https://x.com/eugeneyan/status/1711049672746578064), and by [9.15 there weren’t enough chairs](https://x.com/swyx/status/1711058563895017843). For the Monday 8.45 workshops at the conference venue, by [8.20 it was half filled](https://x.com/eugeneyan/status/1711401732008546377), and by [9.00 it was standing room only](https://x.com/altryne/status/1711448781533990992).

The questions they asked were amazing too. It was apparent that they already had a design doc in their heads and were asking questions about trade-offs, evals, cost, risk, etc. The energy was an order of magnitude above other conferences I’ve attended. These attendees were sprinting like rabbits running from wolves, not the wolves chasing what’s merely lunch. That said, this may not be a representative sample as the organized hand-picked 500 attendees over 10x as many applicants.

Overall, one of the best conferences I’ve attended in a while. Can’t wait for [the next one](https://ti.to/software-3/ai-engineer-worlds-fair/discount/AIE).

OG image prompt: yellow lego block, alien technology style, dark grey background –ar 16:9

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Oct 2023). Reflections on AI Engineer Summit 2023. eugeneyan.com. https://eugeneyan.com/writing/aieng-reflections/.

or

```
@article{yan2023aieng,
  title   = {Reflections on AI Engineer Summit 2023},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Oct},
  url     = {https://eugeneyan.com/writing/aieng-reflections/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
