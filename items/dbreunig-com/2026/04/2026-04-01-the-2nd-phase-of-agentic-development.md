---
title: The 2nd Phase of Agentic Development
link: https://www.dbreunig.com/2026/04/01/the-2nd-phase-of-agentic-development.html
source: dbreunig-com
published: 2026-04-01T23:26:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- coding
- software engineering
- spec driven development
summary: The first wave of agentic coding gave us clones and ports. The next wave attacks old problems with new designs, hardened by synthetic tests.
content: extracted
html: 2026-04-01-the-2nd-phase-of-agentic-development.html
preview:
  file: 2026-04-01-the-2nd-phase-of-agentic-development.preview-d97b425e75c2.webp
  width: 256
  height: 134
  color: '#7a7062'
images:
- source: https://www.dbreunig.com/img/sf_beach_og.jpg
  original:
    file: 2026-04-01-the-2nd-phase-of-agentic-development.image-246e578187ce.jpg
    width: 1200
    height: 630
  color: '#1a130a'
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

Yesterday we talked about how [cheap code is fueling an era of idiosyncratic tooling](https://www.dbreunig.com/2026/03/26/winchester-mystery-house.html), and previously we’ve talked about [the rise of spec driven development](https://www.dbreunig.com/2026/02/06/the-rise-of-spec-driven-development.html). In that second piece, we ran through some of the initial examples of spec driven development with agents:

> By far, the hardest part of starting a SDD project is creating the tests. Which is why many developers are opting for borrowing existing test sets or deriving by referencing a source of truth.
>
> - [**Anthropic wrote a C compiler in Rust**](https://www.anthropic.com/engineering/building-c-compiler). They used [existing test suites](https://gcc.gnu.org/onlinedocs/gccint/Torture-Tests.html) and used GCC as a source of truth for validation and generating new tests.
> - [**Vercel created a bash emulator in TypeScript**](https://github.com/vercel-labs/just-bash). They created and curated an amazing set of [shell script spec tests](https://github.com/vercel-labs/just-bash/tree/main/src/spec-tests) and [have been feeding these to Ralph](https://x.com/cramforce/status/2015513111487553667?s=20). (To make this even more meta, I’ve been following their commits and [Clauding them into Python](https://github.com/dbreunig/just-bash-py)).
> - [**Pydantic created a Python emulator…in Python**](https://github.com/pydantic/monty). This sounds silly, but it’s useful in the same way Vercel’s `just-bash` is: it’s a super lightweight sandbox for AI agents. (In fact, I’ve [already wrapped it in a `CodeInterpreter`](https://github.com/dbreunig/dspy-monty-interpreter) for use with DSPy’s [RLM](https://alexzhang13.github.io/blog/2025/rlm/) module)

The first wave of agentic development brought us *clones* and *ports*. When code is incredibly cheap, and you want the code to flow, you can either rely on [your own fast feedback](https://www.dbreunig.com/2026/03/26/winchester-mystery-house.html) or leverage existing test suites. These early projects opted for the latter, as did many [tokenmaxxers](https://www.nytimes.com/2026/03/20/technology/tokenmaxxing-ai-agents.html) who are [rebuilding their dependencies in Rust or Go](https://github.com/Dicklesworthstone#the-frankensuite).

Two releases this week, however, suggest we’re starting to enter a second phase of open source agentic coding projects. The first brought us *clones*, this next phase brings us *reimaginings*. Consider the following two projects:

- **[Cheng Lou](https://x.com/_chenglou) created [a TypeScript library for laying out text on web pages](https://github.com/chenglou/pretext):** Pretext measures and lays out paragraphs, without using CSS while bypassing DOM measurements and reflow. In a nutshell, this makes tricky things involving text layout dramatically faster and much simpler to implement (Cheng provides many [demos in his thread](https://x.com/_chenglou/status/2037713766205608234)).
- **[Cloudflare Launched EmDash, a modern CMS](https://blog.cloudflare.com/emdash-wordpress/):** EmDash is described as, “the spiritual successor to WordPress.” It’s written in TypeScript, serverless, sandboxes plugins for security, and uses [Astro](https://astro.build/), a fast modern web framework.

The Cloudflare post really spells out the pattern we’re seeing here: the team looked at all the jobs people hire WordPress to perform and asked, how would we solve those if we started today?

> WordPress powers over 40% of the Internet. It is a massive success that has enabled anyone to be a publisher, and created a global community of WordPress developers. But the WordPress open source project will be 24 years old this year. Hosting a website has changed dramatically during that time. When WordPress was born, AWS EC2 didn’t exist. In the intervening years, that task has gone from renting virtual private servers, to uploading a JavaScript bundle to a globally distributed network at virtually no cost. It’s time to upgrade the most popular CMS on the Internet to take advantage of this change.

What they ended up with is something fast, serverless, and secure. They didn’t *clone* WordPress, they *reimagined* it by focusing on the *job to be done*.

Cheng Lou took the same route: he didn’t *port* CSS to Go or Rust to get his speed gains. Rather, he focused on one hard, important job to be done that CSS doesn’t do very well and *reimagined* it with everything we’ve learned and built, unhindered by the baggage of CSS.

Now what does this have to do with coding agents? Isn’t this something we could have done (and did) before?

Previously, the built up ecosystems and mature code of existing software projects made reimagining foolish. Teams *did* create modern CMS projects, but WordPress’ massive size and momentum meant newcomers could only carve out small niches of adoption, if they didn’t fail entirely. The odds weren’t good and the costs of trying were high, so most people sighed and moved on.

Coding agents make reimagining practical because the cost to perform them is so, *so* much lower. Code is cheap. We can take more shots, more often, to counter the embedded standards.

Further, reimagining a new standard was a *long* road. If you survived the initial build and executed a good launch, picking up a small core of users, you earned the job of writing countless bugfixes, optimizations, and security patches. Mature software like WordPress is battle tested; it’s had decades of feedback and found flaws. How can a newcomer compete?

Well, [Cheng Lou demonstrates an interesting approach](https://x.com/_chenglou/status/2037715226838343871), again using agents:

> The engine’s tiny (few kbs), aware of browser quirks, supports all the languages you’ll need, including Korean mixed with RTL Arabic and platform-specific emojis.
>
> This was achieved through showing Claude Code and Codex the browsers ground truth, and have them measure & iterate against those at every significant container width, running over weeks.

With agents and LLMs, we can synthetically test our new tools, patching them as we challenge them.

I think we’re going to see a lot more *reimaginings*, where people attack old problems with modern tactics. Coding agents lower the costs of taking on stalwarts and raise our ability to rapidly harden our software. I can think of many software tools that people rely on but *don’t like*. Those are the prime targets for reimagining.
