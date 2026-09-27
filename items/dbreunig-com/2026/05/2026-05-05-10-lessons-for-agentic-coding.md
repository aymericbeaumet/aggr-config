---
title: 10 Lessons for Agentic Coding
link: https://www.dbreunig.com/2026/05/04/10-lessons-for-agentic-coding.html
source: dbreunig-com
published: 2026-05-05T03:55:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- development
summary: Ten lessons for coding with agents, from implementing to learn to remembering that code is cheap but maintenance and security aren't.
content: extracted
html: 2026-05-05-10-lessons-for-agentic-coding.html
preview:
  file: 2026-05-05-10-lessons-for-agentic-coding.preview-c306aa546168.webp
  width: 256
  height: 150
  color: '#a1a6a7'
images:
- source: https://www.dbreunig.com/img/hand_scroll.jpg
  original:
    file: 2026-05-05-10-lessons-for-agentic-coding.image-13b65f5858bc.jpg
    width: 1554
    height: 910
  color: '#e0f0fd'
extra:
  thumbnail: https://www.dbreunig.com/img/hand_scroll.jpg
---

### What should we do when code is cheap?

![](https://www.dbreunig.com/img/hand_scroll.jpg)

Lately, this blog has featured a lot of writing about agentic coding. Frontier models are *really* good at coding these days, [much better than they are at other tasks](https://www.dbreunig.com/2025/04/11/what-we-mean-when-we-say-think.html#the-strengths--limits-of-reasoning-models). Coding with agents feels like a preview of the future, a playground for seeing how far we can push agent capabilities. It’s invigorating, rewarding, and deeply *weird*.

I’ve been keeping a running list of tips for agentic coding: guidelines or rules one might give to someone just getting started with Codex, Claude Code, Pi, or any other agent. Ideally each tip is generalizable guidance, relevant to any agentic programming. I’m also looking for durable lessons that will stick around as models and harnesses improve.

Below is my current list: *10 Lessons for Agentic Coding*. Ten’s a nice round number; a good time to put this out there.

To be clear: I take credit only for honing and compiling these guidelines. As [Kshetrajna Raghavan](https://x.com/kshetrajna) said to me today, “It’s *crazy* how we’re all converging on similar lessons.”

(If you think I’ve missed anything below, please [reach out](https://www.dbreunig.com/contact.html)!)

* * *

### 10 Lessons for Agentic Coding

1.  **Implement to learn.** You can go far with [Spec-Driven Development](https://www.dbreunig.com/2026/02/06/the-rise-of-spec-driven-development.html), but [the act of writing code surfaces decisions you hadn’t considered and makes your spec better](https://www.dbreunig.com/2026/03/04/the-spec-driven-development-triangle.html). When code is cheap, implement to learn.
2.  **Rebuild often.** Implement early and *often* to learn more. Fork and recode crazy thought experiments. Find out how far you can take feature. Of course, you want to iterate and compound your efforts, but cheap code means you can reconnoiter and reinvent in ways you never could.
3.  **Invest in end-to-end tests.** When we can reinvent our code cheaply, we should spend time writing tests that measure our product’s *functions*, not *how* it performs them. We want behavioral contracts that grant us the freedom to rebuild and reimplement.
4.  **Document intent.** Tests detail our goals while code encodes our methods, but neither captures the *why*. Your intent motivates your decisions, and persisting it alongside the code helps you and your agent compound those decisions in a consistent direction.
5.  **Keep your specs in sync.** Update your specs, the markdown files containing your goals and plans, [as you advance your code and your tests](https://www.dbreunig.com/2026/03/04/the-spec-driven-development-triangle.html). Treating your spec as a frozen artifact written before work begins, you’ll fail to capture learnings during implementation. Keeping it current lets it constantly inform your and your agents’ choices, and makes frequent rebuilds easier.
6.  **Find the hard stuff.** Work on a project long enough and things will stop being easy. You’ll speed through the boilerplate work, the obvious design decisions, and start hitting the ugly, difficult work: intuitive design, performance, security, resilience, and systemic architecture. Anyone can vibe the easy stuff. [The hard work is where the value is](https://www.dbreunig.com/2023/12/27/deja-connu.html). Find it and dig in.
7.  **Automate everything that’s easy.** To spend more time on the hard stuff, minimize the time you spend on easy things. Distill learnings into skills, build loops, automate code reviews, and let your tools compound. But careful: don’t get stuck in [a Mystery House](https://www.dbreunig.com/2026/03/26/winchester-mystery-house.html).
8.  **Develop your taste.** When code arrives fast but feedback doesn’t, the only source of feedback that keeps up is your own. The better you know your domain, your users, and their problems, the further you can go without checking in.
9.  **Agents amplify experience.** Talented developers underestimate how much intuition they bring to their prompts: the right terms, the right framing, and the right level of specificity. If you know your stack, you can save countless cycles during both implementation and debugging, and cut down needless agent exploration. Pair technical expertise coupled with great taste for an unbeatable advantage.
10. **Code is cheap, but maintenance, support, and security aren’t.** Agentic code is “[free as in puppies](https://www.dbreunig.com/2026/02/06/the-rise-of-spec-driven-development.html).” Support [isn’t cheap](https://www.dbreunig.com/2026/02/21/why-is-claude-an-electron-app.html) and [neither is security](https://www.dbreunig.com/2026/04/14/cybersecurity-is-proof-of-work-now.html). Build fast, but mind the maintenance you’re adopting.
