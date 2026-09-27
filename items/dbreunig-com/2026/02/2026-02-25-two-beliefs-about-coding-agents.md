---
title: Two Beliefs About Coding Agents
link: https://www.dbreunig.com/2026/02/25/two-things-i-believe-about-coding-agents.html
source: dbreunig-com
published: 2026-02-25T22:12:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- software development
summary: Talented developers underestimate the intuition they bring to their prompts, and coding agents amplify existing skills rather than replace them.
content: extracted
html: 2026-02-25-two-beliefs-about-coding-agents.html
preview:
  file: 2026-02-25-two-beliefs-about-coding-agents.preview-d97b425e75c2.webp
  width: 256
  height: 134
  color: '#7a7062'
images:
- source: https://www.dbreunig.com/img/sf_beach_og.jpg
  original:
    file: 2026-02-25-two-beliefs-about-coding-agents.image-246e578187ce.jpg
    width: 1200
    height: 630
  color: '#1a130a'
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

There’s a lot of noise about how AI is changing programming these days. It can be a bit overwhelming.

If you hang out on social media, you’ll hear wild claims about people running 12 agents at once, for days. Or people hacking bots together, giving them $10k, and letting them roam the web.

The challenge with all of this is that coding agents *really are* performing some science fiction feats which were barely imaginable just 12 months ago. But at the same time, the ecosystem is incentivizing the most outlandish claims, so punters keep telling tall tales. Separating the signal from the noise is near impossible.

I’m lucky enough to talk to a range of developers and teams, spanning a variety of company sizes and a broad array of skill sets. From these conversations, two beliefs have emerged and solidified about coding agents and their (current) impact on coding.

Let’s start with belief number one:

**Most talented developers do not appreciate the impact of the intuitive knowledge they bring to their coding agent.**

We’ve all seen the posts by developer luminaries. They haven’t written code in weeks. They gave a hard problem to Claude Code or Codex and *it just worked*.

But what we don’t see is their prompts. And having seen *many* prompts by *many* types of devs, I would wager their prompts are relatively specific and offer more guidance to the LLM than your average user. And these specifics don’t have to be exhaustive. Even knowing the right terms to use can have enormous impact and activate an entirely different set of weights in the model than someone writing, “the search is broken fix it.”

Skilled programmers, with plenty of experience, don’t even think about how to ask correctly. They just do, intuitively. And things work well. If the agent and dev go through multiple turns, this effect gets even more significant.

I wish we could see more prompts and traces, from a wide range of developers, to better understand the range of code. And, just as interestingly, how hard and long agents have to work to achieve the goal. For now we can just browse public repos on Github, where the range of coding quality is quite broad.

Which brings me to the second belief:

**Most work people are sharing are incredible personal tools, but they are not capital-P products.**

There’s an app I really like called “[StreetPass](https://streetpass.social).” It’s a browser extension that watches web pages you visit and collects Mastodon accounts it finds, letting you easily follow them if you wish. It’s small and charming. A perfect extension.

Recently, I realized I wanted a version of StreetPass, but for RSS feeds instead of Mastodon accounts. I forked StreetPass, fired up Claude Code, and had [a working version quickly](https://github.com/dbreunig/feedpass). You can use this, but I’m not supporting it. I won’t be pushing it to the App Store or Chrome Web Store. I won’t be building a version that doesn’t leverage [Feedbin](https://feedbin.com). I have no idea if it works on Chrome or Firefox. It’s personal software that I use almost daily.

Most agentic coding projects we see being hyped are like this.

All those things I won’t do, those are the things that would turn my *personal software* into a *Product*. And we haven’t even gotten to marketing, support, and more. As we covered when we [touched on Claude’s desktop app](https://www.dbreunig.com/2026/02/21/why-is-claude-an-electron-app.html), the last 10% of product development and support is where the pain is. And that’s still a long road. As they say: [Code today is free, as in puppies](https://www.dbreunig.com/2026/02/06/the-rise-of-spec-driven-development.html).

But I want to be clear about couple things.

First, I know many teams shipping agent written code into products. But they test, support, review, and so much more. But when we make big claims like “coding is solved” or “code is free”, we need to be clear about *what* we’re talking about building[^1].

Second, our ability to manifest personal software easily *is amazing* and powerful. I am continually inspired by the things people build (for example, I loved [Simon’s presentation software he whipped up for FOO Camp](https://simonwillison.net/2026/Feb/25/present/)). His presentation app is so tailored to him, in the past the math would never justify the time spent building it to support a market of maybe a dozen. But now he gets his dream!

Similarly, my RSS finder extension is a feature not an app and (sadly) there isn’t a large market for RSS today. But with Claude Code (and open source code to build upon!) I can build just what I wanted in moments.

* * *

I am sure as our scaffolding and models improve, this stuff will get more accessible and more resilient, but I don’t expect these two beliefs to go away. Providing AI with the right instructions to obtain *just* what you want, will always be a challenge.

Coding agents amplify existing skills.

[^1]: [Grady Booch](https://x.com/Grady_Booch/status/2026736492488568955) has a good post about this today. Things are getting higher level, and changing fast, but engineering remains.
