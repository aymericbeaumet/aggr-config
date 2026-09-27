---
title: Cybersecurity Looks Like Proof of Work Now
link: https://www.dbreunig.com/2026/04/14/cybersecurity-is-proof-of-work-now.html
source: dbreunig-com
published: 2026-04-14T14:42:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- development
- mythos
- security
summary: 'AISI''s tests of Mythos suggest a new security economy: to harden a system you must spend more tokens finding exploits than attackers do.'
content: extracted
html: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.html
preview:
  file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.preview-b6108314d10d.webp
  width: 256
  height: 128
  color: '#bfc8c3'
images:
- source: https://www.dbreunig.com/img/locks_and_keys.jpg
  original:
    file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-712b227d97ca.jpg
    width: 1280
    height: 640
  color: '#e2e6e6'
- source: https://www.dbreunig.com/img/the_last_ones_chart.png
  original:
    file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-408fd1dd150c.png
    width: 1980
    height: 1237
  variants:
  - file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-464ae1ed2b66.webp
    width: 320
    height: 200
  - file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-face279ff4a1.webp
    width: 640
    height: 400
  - file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-eba10514fc56.webp
    width: 960
    height: 600
  - file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-530fe593fa59.webp
    width: 1280
    height: 800
  - file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-78a6cd145cd9.webp
    width: 1600
    height: 1000
  - file: 2026-04-14-cybersecurity-looks-like-proof-of-work-now.image-0374c73b7c03.webp
    width: 1980
    height: 1237
  color: '#fcfcfc'
extra:
  thumbnail: https://www.dbreunig.com/img/locks_and_keys.jpg
---

### Is security spending more tokens than your attacker?

Last week we learned about Anthropic’s Mythos, a new LLM so “[strikingly capable at computer security tasks](https://red.anthropic.com/2026/mythos-preview/)” that Anthropic didn’t release it publicly. Instead, [only critical software makers have been granted access](https://www.anthropic.com/glasswing), providing them time to harden their systems.

We quickly blew through our standard stages of processing big AI claims: shock, existential fear, hype, skepticism, criticism, and (finally) moving onto the next thing. I encouraged people to take a wait-and-see approach, as security capabilities are tailor-made for impressive demos. Finding exploits is a clearly defined, [verifiable](https://www.dbreunig.com/2025/12/29/2025-in-review.html) search problem. You’re not building a complex system, but poking at one that exists. A problem well suited to throwing millions of tokens at.

Yesterday, the first 3rd party analysis landed, [from the AI Security Institute](https://www.aisi.gov.uk/blog/our-evaluation-of-claude-mythos-previews-cyber-capabilities) (AISI), largely supporting Anthropic’s claims. Mythos is *really* good, “a step up over previous frontier models in a landscape where cyber performance was already rapidly improving.”

The entire report is worth reading, but I want to focus on the following chart, detailing the ability of different models to successfully complete a simulated, complex corporate network attack:

![](https://www.dbreunig.com/img/the_last_ones_chart.png)

“[The Last Ones](https://arxiv.org/abs/2603.11214)” is, “a 32-step corporate network attack simulation spanning initial reconnaissance through to full network takeover, which [AISI](https://www.aisi.gov.uk/blog/our-evaluation-of-claude-mythos-previews-cyber-capabilities) estimates to require humans 20 hours to complete.” The lines are the average performance across multiple runs (10 runs for Mythos, Opus 4.6, and GPT-5.4), with the “max” lines representing the best of each batch. Mythos was the only model to complete the task, in 3 out of its 10 attempts.

This chart suggests an interesting security economy: **to harden a system we need to spend more tokens discovering exploits than attackers spend exploiting them**.

AISI budgeted 100M tokens for each attempt. That’s $12,500 per Mythos attempt, $125k for all ten runs. Worryingly, none of the models given a 100M budget showed signs of diminishing returns. “Models continue making progress with increased token budgets across the token budgets tested,” AISI notes.

If Mythos continues to find exploits so long as you keep throwing money at it, security is reduced to a brutally simple equation: **to harden a system you need to spend more tokens discovering exploits than attackers will spend exploiting them**.

You don’t get points for being clever. You win by paying more. It is a system that echoes cryptocurrency’s [proof of work](https://en.wikipedia.org/wiki/Proof_of_work) system, where success is tied to raw computational work. It’s a [low temperature lottery](https://x.com/lateinteraction/status/2042025859003920574): buy the tokens, maybe you find an exploit. Hopefully you keep trying longer than your attackers.

This calculus has a few immediate takeaways:

**First, open source software remains critically important.**

For those of you who aren’t exposed to AI maximalists, this statement feels absurd. But lately, after the [LiteLLM](https://docs.litellm.ai/blog/security-update-march-2026) and [Axios](https://www.elastic.co/security-labs/how-we-caught-the-axios-supply-chain-attack) supply chain scares, many have argued for reimplementing dependency functionality using coding agents.

Here’s Karpathy, [just a few weeks ago](https://x.com/karpathy/status/2036487306585268612?s=20):

> Classical software engineering would have you believe that dependencies are good (we’re building pyramids from bricks), but imo this has to be re-evaluated, and it’s why I’ve been so growingly averse to them, preferring to use LLMs to “yoink” functionality when it’s simple enough and possible.

If security is purely a matter of throwing tokens at a system, [Linus’s law](https://en.wikipedia.org/wiki/Linus%27s_law) that, “given enough eyeballs, all bugs are shallow,” expands to include tokens. If corporations that rely on OSS libraries spend to secure them with tokens, it’s likely going to be more secure than your budget allows. Certainly, this has complexities: cracking a widely used OSS package is inherently more valuable than hacking a one-off implementation, which incentivizes attackers to spend more on OSS targets.

**Second, hardening will be an additional phase for agentic coders.**

We’ve already been seeing developers break their process into two steps, development and code review, often using different models for each phase. As this matures, we’re seeing purpose-built tooling meeting this pattern. Anthropic launched a [code review](https://code.claude.com/docs/en/code-review) product that costs $15-20 per review.

If the above Mythos claims hold, I suspect we’ll see a three phase cycle: development, review, and hardening.

1. **Development:** Implement features, iterate quickly, guided by human intuition and user feedback.
2. **Review:** Document, refactor, and other gardening tasks, async, applying best practices with each PR.
3. **Hardening:** Identify exploits, autonomously, until the budget runs out.

Critically, human input is the limiter for the first phase and money is the limiter for the last. This quality inherently makes them separate stages (why spend to harden before you have something?). Previously, security audits were rare, discrete, and inconsistent. Now we can apply them constantly, within an optimal (we hope!) budget.

Code remains [cheap](https://www.dbreunig.com/2026/03/26/winchester-mystery-house.html), unless it needs to be secure. Even if costs go down as inference optimizations, unless models reach the point of diminishing security returns, you *still* need to buy more tokens than attackers do. The cost is fixed by the market value of an exploit.
