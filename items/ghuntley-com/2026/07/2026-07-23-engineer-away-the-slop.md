---
title: engineer away the slop
link: https://ghuntley.com/slop/
source: ghuntley-com
published: 2026-07-23T16:40:16Z
updated: 2026-07-23T16:40:16Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Geoffrey Huntley
summary: 🎉 It's been a busy six months, that's for sure. I'm not going to bury the lede here; the short TLDR is I'm joining the folks over at https://antithesis.com/. If I wind back time to November 2024, it was apparent
content: extracted
html: 2026-07-23-engineer-away-the-slop.html
preview:
  file: 2026-07-23-engineer-away-the-slop.preview-ad629a75b7bf.webp
  width: 256
  height: 256
  color: '#cbc3c4'
images:
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/07/Symbolic-traditional-tattoo-art-print-depicting-a-software-engineer-meticulously-debugging-code-with-a-metaphorical-weaving-loom--rendered-in-vibrant--retro-inspired-colors-on-a-clean-white-background.-The-composition-emphasizes-intricate-o.jpg
  original:
    file: 2026-07-23-engineer-away-the-slop.image-dc48b3d121fe.jpg
    width: 1024
    height: 1024
  color: '#fefefe'
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-29c86fea-4d88-4df2-aed6-6e51e7e8b9b7.svg
  original:
    file: 2026-07-23-engineer-away-the-slop.image-bddfd8f40413.png
    width: 288
    height: 288
  variants:
  - file: 2026-07-23-engineer-away-the-slop.image-d4fa793d450e.webp
    width: 288
    height: 288
  color: '#fe9e90'
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/snoutyplays.DX648_wc_ZAzB9t-cbb49132-0709-4d0a-8bb5-9b8d003e8c57.jpg
  original:
    file: 2026-07-23-engineer-away-the-slop.image-7ad0a06b56e4.jpg
    width: 1200
    height: 675
  color: '#155657'
---

🎉

It's been a busy six months, that's for sure. I'm not going to bury the lede here; the short TLDR is I'm joining the folks over at [https://antithesis.com/](https://antithesis.com/?utm_source=ghuntley).

* * *

If I [wind back time to November 2024](https://ghuntley.com/oh-fuck), it was apparent to me back then that our profession would change. To be frank, in the last six months our profession has changed more than it has in the last 30 years. Software authoring has been commoditised. Everyone is now a software developer, but being a software developer does not mean that they're an engineer.

The word engineer [is used trivially in our profession](https://ghuntley.com/squirrel-burgers/), but in other industries the word engineer means failures are unacceptable.

Now I'm a little bit old and crusty these days; I turn 44 next week, and I've seen some absolutely horrible codebases, but if I'm to be honest, I can remember some of the first code that I wrote, and I have regrets. If you caught my talk at the AI Engineer World Fair, one of the things I shared was some deep concerns that we are entering into another Eternal September. You see, [we didn't create enough software engineers after the 2000s dot-com implosion](https://ghuntley.com/screwed/) to properly support an apprenticeship model of learning in our industry.

It's now twenty-six years since that event, and now that everyone can create software, we've got some hard questions to solve. But whilst many things have changed, the job of software engineers is to produce experiences without defects.

So I've been pondering how we are going to fix this predicament.

Here's my hypothesis:

1. The discipline/techniques of formal verification and deterministic system testing are about to cross the chasm. There's a whole lot of brownfield software out there that's been written over the last 30 years that is being affected by the [infinite software crisis](https://www.youtube.com/watch?v=eIoohUmYpGI&ref=ghuntley.com) (“how do we do code review now?” / “the volume of code/change is too high”) all at once.
2. Not enough skilled practitioners in the discipline/techniques of formal verification and deterministic system testing exist.
3. Whilst the costs of building a simulator for a project (new or retrofitting an existing project) are significantly cheaper now, there are entire categories and classes of problems that will not surface unless you emulate a deterministic computer (which provides a forceful way to make *anything* deterministic).
4. Antithesis, when used in conjunction with adversarial code reviews by an LLM and with [language analyzers driven by pre-commit hooks](https://ghuntley.com/pressure/), will be a key component in software factories that enables people/agents to deliver reliable software without the burden of having to learn this specialized knowledge.

Creation is now near-free. Verification/understanding is not, yet. It's time to engineer away the slop.

[How Antithesis finds bugs (with help from the Super Mario Bros.) | Antithesis](https://antithesis.com/blog/sdtalk/?utm_source=ghuntley)

Can solving Super Mario Bros. help solve your distributed systems issues?

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-29c86fea-4d88-4df2-aed6-6e51e7e8b9b7.svg)Antithesis Will Wilson Co-founder & CEO

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/snoutyplays.DX648_wc_ZAzB9t-cbb49132-0709-4d0a-8bb5-9b8d003e8c57.jpg)

[Testing a single-node, single threaded, distributed system written in 1985](https://www.youtube.com/watch?v=zc4cqtibTzs)
