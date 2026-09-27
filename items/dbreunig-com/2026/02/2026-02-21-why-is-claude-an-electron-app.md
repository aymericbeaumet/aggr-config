---
title: Why is Claude an Electron App?
link: https://www.dbreunig.com/2026/02/21/why-is-claude-an-electron-app.html
source: dbreunig-com
published: 2026-02-21T18:00:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- software development
- spec driven development
summary: If coding agents make native ports cheap, why does Anthropic still ship Claude as an Electron app? Because code is cheap but maintenance isn't.
content: extracted
html: 2026-02-21-why-is-claude-an-electron-app.html
preview:
  file: 2026-02-21-why-is-claude-an-electron-app.preview-d97b425e75c2.webp
  width: 256
  height: 134
  color: '#7a7062'
images:
- source: https://www.dbreunig.com/img/sf_beach_og.jpg
  original:
    file: 2026-02-21-why-is-claude-an-electron-app.image-246e578187ce.jpg
    width: 1200
    height: 630
  color: '#1a130a'
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

### If code is free, why aren’t all apps native?

The state of coding agents can be summed up by [this fact](https://x.com/dbreunig/status/2024970389156495365?s=46)

> Claude spent $20k on an agent swarm implementing (kinda) a C-compiler in Rust, but desktop Claude is an Electron app.

If you’re unfamiliar, Electron is a coding framework for building desktop applications using web tech, specifically HTML, CSS, and JS. What’s great about Electron is it allows you to build one desktop app that supports Windows, Mac, and Linux. Plus it lets developers use existing web app code to get started. It’s great for teams big and small. [Many apps you probably use every day are built with Electron](https://en.wikipedia.org/wiki/List_of_software_using_Electron?wprov=sfti1): Slack, Discord, VS Code, Teams, Notion, and more.

There are downsides though. Electron apps are bloated; each runs its own Chromium engine. The minimum app size is usually a couple hundred megabytes. They are often laggy or unresponsive. They don’t integrate well with OS features.

(These last two issues *can* be addressed by smart development and OS-specific code, but they rarely are. The benefits of Electron (one codebase, many platforms, it’s just web!) don’t incentivize optimizations outside of HTML/JS/CSS land.)

But these downsides are dramatically outweighed by the ability to build and maintain one app, shipping it everywhere.

But now we have coding agents! [And one thing coding agents are proving to be pretty good at is cross-platform, cross-language implementations given a well-defined spec and test suite](https://www.dbreunig.com/2026/02/06/the-rise-of-spec-driven-development.html).

On the surface, this ability should render Electron’s benefits obsolete! Rather than write one web app and ship it to each platform, we should write *one spec and test suite* and use coding agents to ship *native* code to each platform. If this ability is real and adopted, users get snappy, performant, native apps from small, focused teams serving a broad market.

But we’re still leaning on Electron. Even Anthropic, one of the leaders in AI coding tools, who keeps publishing flashy agentic coding achievements, still uses Electron in the Claude desktop app. And it’s slow, buggy, and bloated app.

*So why are we still using Electron and not embracing the agent-powered, spec driven development future?*

For one thing, coding agents are *really* good at the first 90% of dev. But that last bit – nailing down all the edge cases and continuing support once it meets the real world – remains hard, tedious, and requires plenty of agent hand-holding.

Anthropic’s [Rust-base C compiler](https://www.anthropic.com/engineering/building-c-compiler) slammed into this wall, after screaming through the bulk of the tests:

> The resulting compiler has nearly reached the limits of Opus’s abilities. I tried (hard!) to fix several of the above limitations but wasn’t fully successful. New features and bugfixes frequently broke existing functionality.

The resulting compiler *is* impressive, given the time it took to deliver it and the number of people who worked on it, but it is largely unusable. That last mile is *hard*.

And this gets even worse once a program meets the real world. Messy, unexpected scenarios stack up and development never really ends. Agents make it easier, sure, but hard product decisions become challenged and require human decisions.

Further, with 3 different apps produced (Mac, Windows, and Linux) the surface area for bugs and support increases 3-fold. Sure, there are local quirks with Electron apps, but most of it is mitigated by the common wrapper. Not so with native!

A good test suite and spec *could* enable the Claude team to ship a Claude desktop app native to each platform. But the resulting overhead of that last 10% of dev and the increased support and maintenance burden will remain.

For now, Electron still makes sense. Coding agents are amazing. But the last mile of dev and the support surface area remains a real concern.

* * *

Over at [Hacker News](https://news.ycombinator.com), Claude Code’s [Boris Cherney](https://borischerny.com) [chimes in](https://news.ycombinator.com/item?id=47106368):

> Boris from the Claude Code team here.
>
> Some of the engineers working on the app worked on Electron back in the day, so preferred building non-natively. It’s also a nice way to share code so we’re guaranteed that features across web and desktop have the same look and feel. Finally, Claude is great at it.
>
> That said, engineering is all about tradeoffs and this may change in the future!

There we go: developer familiarity and simpler maintainability across multiple platforms is worth the “tradeoffs”. We have incredible coding agents that are great at transpilation, but there remain costs that outweigh the costs of shipping a non-native app.
