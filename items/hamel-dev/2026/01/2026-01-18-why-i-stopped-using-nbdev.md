---
title: Why I Stopped Using nbdev
link: https://hamel.dev/blog/posts/ai-stack/
source: hamel-dev
published: 2026-01-18T08:00:00Z
updated: 2026-01-18T08:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Hamel Husain
labels:
- ai
- nbdev
- tools
summary: 'Programmers love to proclaim they’ve found the best tool. Paul Graham called Lisp his “secret weapon.” DHH described Ruby as “a magical glove that just fit my brain perfectly.” Pieter Levels ships million-dollar products with vanilla PHP and jQuery. These declarations aren’t about the languages themselves. They’re about developers finding tools that fit how they think. When the environment clicks, you move fast. I had that experience with nbdev, a development environment for literate programming that I helped build and maintain1. I created hundreds of projects with it and was one of its biggest proponents. Today, I no longer use it. AI coding tools changed the trade-offs. Fighting the AI The beauty of nbdev is its workflow. You write code, documentation and tests in one source of truth: Jupyter notebooks. Afterwards, these notebooks are transpiled into a Python library and documentation website. This workflow is idiosyncratic. AI coding tools, trained on vast amounts of conventional source code, get confused. They struggle to differentiate between editing the notebook and editing the final source code. It feels like fighting the AI instead of working with it. I write software to solve problems, not to write code. I want to work in an environment where AI has the highest chance of success. With nbdev, I was swimming upstream. Some argue that AI tools encourage lazy thinking: that without guardrails, developers skip the hard work of breaking problems into steps. But thinking step-by-step is a human skill. Notebooks don’t force you to write clean code. AI tools don’t force you to think carefully. Discipline comes from the developer, not the environment. Tools Don’t Matter As Much As I Thought A central promise of literate programming is better documentation. By keeping code and docs in one place, you reduce the chance they become stale. Strangely, many nbdev projects lacked sufficient documentation for my taste. Sometimes, this helped me learn a codebase by contributing to the docs. Other times, it was frustrating. This reinforced my belief that good documentation comes from effort, not tooling. This workflow is also less compelling now. AI can read a codebase without documentation and give you an overview on the fly. It can help maintain documentation that is separate from the code, handling the tedious parts. Keeping code and docs together isn’t the selling point it used to be. Collaboration and Adoption nbdev asks developers to adopt a different system. It does not meet them where they are. Cursor won because it felt familiar and let developers change their habits slowly, rather than demanding a new workflow on day one. I didn’t worry about collaboration as much before. But collaborating with AI is table stakes. The same impediments that get in the way of collaborating with humans tend to get in the way of collaborating with AI. Today, developers increasingly span greater scope. Backend people do frontend. PMs create prototypes. Everyone is more polyglot. Idiosyncratic frameworks isolate you from your team to a greater extent than ever before. Idiosyncratic tooling once had a hidden upside: it filtered for a certain kind of contributor. Now I believe it’s more of a liability. What I’m Using Now Because I have invested thousands of hours into nbdev, it’s difficult to admit there are better tools for the outcomes I want. But I must check my ego at the door2. My daily drivers now include Amp, Cursor, and Claude Code. I still enjoy notebooks, but only for data analysis, machine learning, or other exploratory workflows where the iterative, visual nature of notebooks shines. More importantly, AI has nudged me out of my previous “Python for everything” mindset. I now use different languages for different tasks. For web development, I prefer the Next.js stack. Using a notebook for web development (even with specialized tooling) adds unnecessary complexity for me. This isn’t arbitrary preference. AI performs best on code with abundant training data for specific domains. TypeScript recently overtook both Python and JavaScript on GitHub, driven partly by the fact that typed languages make AI-generated code more reliable in production3. Even Jane Street, famous for its OCaml-heavy infrastructure, now uses Python for machine learning and data work. A Place for Joy None of this means idiosyncratic tools are worthless. Lisp, Haskell, and APL each teach you something different about computing. Joy is a valid reason to choose a language, even with less AI support. It’s just not my focus right now. My joy resides in solving problems, and I want tools that maximize my leverage. For that, conventional wins. Footnotes I joined the nbdev project in 2020 while at GitHub. That same year I built fastpages, a notebook blogging system that informed nbdev’s documentation approach. Along the way I contributed to tools like ghapi and fastcore. In 2022, I helped lead a complete rewrite of nbdev. I discussed the philosophy behind this work on the Vanishing Gradients Podcast and at Data Council. I was briefly interested in commercializing nbdev, but decided not to pursue it.↩︎ Other top nbdev maintainers and power users like Sylvain Gugger, Wasim Lorgat, Isaac Flath, Zach Mueller, and Wayde Gilliam have made similar moves.↩︎ Research shows 94% of LLM compilation errors in TypeScript come from type violations, suggesting type systems can guide better code generation. See Mündler et al., 2025.↩︎'
content: extracted
html: 2026-01-18-why-i-stopped-using-nbdev.html
preview:
  file: 2026-01-18-why-i-stopped-using-nbdev.preview-0c6c305dcdec.webp
  width: 256
  height: 144
  color: '#5f4966'
images:
- source: https://hamel.dev/blog/posts/ai-stack/nbdev2-cover.png
  original:
    file: 2026-01-18-why-i-stopped-using-nbdev.image-e0717ff25639.png
    width: 1280
    height: 720
  variants:
  - file: 2026-01-18-why-i-stopped-using-nbdev.image-080a7b760a09.webp
    width: 320
    height: 180
  - file: 2026-01-18-why-i-stopped-using-nbdev.image-1a1d8dd5822e.webp
    width: 640
    height: 360
  - file: 2026-01-18-why-i-stopped-using-nbdev.image-ef02c332eb81.webp
    width: 960
    height: 540
  - file: 2026-01-18-why-i-stopped-using-nbdev.image-804c36ff827c.webp
    width: 1280
    height: 720
  color: '#291b48'
---

Programmers love to proclaim they’ve found the best tool. Paul Graham called Lisp his “[secret weapon](https://paulgraham.com/avg.html).” DHH described Ruby as “[a magical glove that just fit my brain perfectly](https://rubyonrails.org/doctrine).” Pieter Levels ships million-dollar products with [vanilla PHP and jQuery](https://lexfridman.com/pieter-levels-transcript/).

These declarations aren’t about the languages themselves. They’re about developers finding tools that fit how they think. When the environment clicks, you move fast.

I had that experience with [nbdev](https://www.fast.ai/posts/2022-07-28-nbdev2.html), a development environment for literate programming that I helped build and maintain[^1]. I created hundreds of projects with it and was one of its [biggest](https://www.youtube.com/watch?v=rX1yGxJijsI) [proponents](https://github.blog/developer-skills/programming-languages-and-frameworks/nbdev-a-literate-programming-environment-that-democratizes-software-engineering-best-practices/).

Today, I no longer use it. AI coding tools changed the trade-offs.

## Fighting the AI

The beauty of nbdev is its workflow. You write code, documentation and tests in one source of truth: Jupyter notebooks. Afterwards, these notebooks are transpiled into a Python library and documentation website.

This workflow is idiosyncratic. AI coding tools, trained on vast amounts of conventional source code, get confused. They struggle to differentiate between editing the notebook and editing the final source code. It feels like fighting the AI instead of working with it.

I write software to solve problems, not to write code. I want to work in an environment where AI has the highest chance of success. With nbdev, I was swimming upstream.

Some argue that AI tools encourage lazy thinking: that without guardrails, developers skip the hard work of breaking problems into steps. But thinking step-by-step is a human skill. Notebooks don’t force you to write clean code. AI tools don’t force you to think carefully. Discipline comes from the developer, not the environment.

## Tools Don’t Matter As Much As I Thought

A central promise of literate programming is better documentation. By keeping code and docs in one place, you reduce the chance they become stale.

Strangely, many nbdev projects lacked sufficient documentation for my taste. Sometimes, this helped me learn a codebase by [contributing](https://fastpages.fast.ai/fastcore/) to the docs. Other times, it was frustrating. This reinforced my belief that good documentation comes from effort, not tooling.

This workflow is also less compelling now. AI can read a codebase without documentation and give you an overview on the fly. It can help maintain documentation that is separate from the code, handling the tedious parts. Keeping code and docs together isn’t the selling point it used to be.

## Collaboration and Adoption

nbdev asks developers to adopt a different system. It does not meet them where they are. Cursor won because it felt familiar and let developers change their habits slowly, rather than demanding a new workflow on day one.

I didn’t worry about collaboration as much before. But collaborating with AI is table stakes. The same impediments that get in the way of collaborating with humans tend to get in the way of collaborating with AI.

Today, developers increasingly span greater scope. Backend people do frontend. PMs create prototypes. Everyone is more polyglot. Idiosyncratic frameworks isolate you from your team to a greater extent than ever before. Idiosyncratic tooling once had a hidden upside: it filtered for a certain kind of contributor. Now I believe it’s more of a liability.

## What I’m Using Now

Because I have invested thousands of hours into nbdev, it’s difficult to admit there are better tools for the outcomes I want. But I must check my ego at the door[^2].

My daily drivers now include Amp, Cursor, and Claude Code. I still enjoy notebooks, but only for data analysis, machine learning, or other exploratory workflows where the iterative, visual nature of notebooks shines.

More importantly, AI has nudged me out of my previous “Python for everything” mindset. I now use different languages for different tasks. For web development, I prefer the Next.js stack. Using a notebook for web development (even with specialized tooling) adds unnecessary complexity for me.

This isn’t arbitrary preference. AI performs best on code with abundant training data for specific domains. [TypeScript recently overtook both Python and JavaScript on GitHub](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/), driven partly by the fact that typed languages make AI-generated code more reliable in production[^3]. Even Jane Street, famous for its OCaml-heavy infrastructure, [now uses Python](https://www.efinancialcareers.com/news/python-ocaml-jane-street-ai) for machine learning and data work.

## A Place for Joy

None of this means idiosyncratic tools are worthless. Lisp, Haskell, and APL each teach you something different about computing. Joy is a valid reason to choose a language, even with less AI support. It’s just not my focus right now. My joy resides in solving problems, and I want tools that maximize my leverage. For that, conventional wins.

[^1]: I [joined the nbdev project in 2020](https://github.blog/developer-skills/programming-languages-and-frameworks/nbdev-a-literate-programming-environment-that-democratizes-software-engineering-best-practices/) while at GitHub. That same year I built [fastpages](https://www.youtube.com/watch?v=cduXZwZaBbM), a notebook blogging system that informed nbdev’s documentation approach. Along the way I contributed to tools like [ghapi](https://github.blog/developer-skills/programming-languages-and-frameworks/learn-about-ghapi-a-new-third-party-python-client-for-the-github-api/) and [fastcore](https://fastpages.fast.ai/fastcore/). In 2022, I helped lead a [complete rewrite](https://www.fast.ai/posts/2022-07-28-nbdev2.html) of nbdev. I discussed the philosophy behind this work on the [Vanishing Gradients Podcast](https://vanishinggradients.fireside.fm/9) and at [Data Council](https://www.youtube.com/watch?v=rX1yGxJijsI). I was briefly interested in commercializing nbdev, but [decided not to pursue it](https://hamel.dev/blog/posts/nbdev/).

[^2]: Other top nbdev maintainers and power users like [Sylvain Gugger](https://github.com/sgugger), [Wasim Lorgat](https://github.com/seeM), [Isaac Flath](https://github.com/isaac-flath), [Zach Mueller](https://github.com/muellerzr), and [Wayde Gilliam](https://ohmeow.com/) have made similar moves.

[^3]: Research shows 94% of LLM compilation errors in TypeScript come from type violations, suggesting type systems can guide better code generation. See [Mündler et al., 2025](https://arxiv.org/abs/2504.09246).
