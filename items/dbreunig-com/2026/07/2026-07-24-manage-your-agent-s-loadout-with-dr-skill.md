---
title: Manage Your Agent’s Loadout with Dr. Skill
link: https://www.dbreunig.com/2026/07/24/manage-your-agent-s-loadout-with-dr-skill.html
source: dbreunig-com
published: 2026-07-24T15:55:00Z
updated: 2026-09-15T14:59:45Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- agents
- ai
- ci
- context
- devops
- skills
summary: Keep your context clean by keeping your agent's skills and tools in check.
content: extracted
html: 2026-07-24-manage-your-agent-s-loadout-with-dr-skill.html
preview:
  file: 2026-07-24-manage-your-agent-s-loadout-with-dr-skill.preview-d97b425e75c2.webp
  width: 256
  height: 134
  color: '#7a7062'
images:
- source: https://www.dbreunig.com/img/sf_beach_og.jpg
  original:
    file: 2026-07-24-manage-your-agent-s-loadout-with-dr-skill.image-246e578187ce.jpg
    width: 1200
    height: 630
  color: '#1a130a'
- source: https://www.dbreunig.com/img/resized/drskill-1.png.jpg
  original:
    file: 2026-07-24-manage-your-agent-s-loadout-with-dr-skill.image-e8b70f25fae5.jpg
    width: 1600
    height: 669
  color: '#0f1217'
- source: https://www.dbreunig.com/img/resized/drskill-2.png.jpg
  original:
    file: 2026-07-24-manage-your-agent-s-loadout-with-dr-skill.image-df307bf6e04b.jpg
    width: 1600
    height: 667
  color: '#0f1217'
- source: https://www.dbreunig.com/img/resized/drskill-3.png.jpg
  original:
    file: 2026-07-24-manage-your-agent-s-loadout-with-dr-skill.image-1b1526e80f69.jpg
    width: 1600
    height: 668
  color: '#0f1217'
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

### Keep Your Agent’s Skills and Tools in Check

When I wrote “[How to Fix Your Context](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html)”, it was really hard to find real-world examples of the **Tool Loadout** pattern. But as [Skills](https://learn.microsoft.com/en-us/agent-framework/agents/skills?pivots=programming-language-csharp) have grown in popularity, our contexts have become cluttered.

Recently, I met a developer who told me how his enterprise agent was loading *over 600 skills*. All the skills created by colleagues were getting pulled in *by default*, without alerting the dev, silently degrading the context before a word was typed.

Last week, my [Hermes agent](https://hermes-agent.nousresearch.com/) kept loading the wrong note-taking skill. SSH’ing into the machine, I discovered Hermes ships with nearly 100 skills. Deleting the stock note-taking skill helped, but didn’t fully solve the issue. And I wasn’t eager to pore over every Hermes skill.

So I built [`drskill`](https://github.com/dbreunig/drskill), a tool that evaluates all the skills and MCPs in your global or project environment.

Install `drskill` with `uv tool install drskill` or `pip install drskill`.

Then run `drskill scan`:

![](https://www.dbreunig.com/img/resized/drskill-1.png.jpg)

Here, `drskill` spots an error (my [`plumb`](https://www.dbreunig.com/2026/03/04/the-spec-driven-development-triangle.html) skill is missing a description) and some warnings (a skill I was developing for [Overture](https://overturemaps.org) is installed twice, in two different places). `drskill` prints the path (and a solution, if relevant) and lets me fix these quickly.

`drskill` looks for 34 different issue categories, across MCPs and Skills. If you pass in the `--deep` flag, it will use an LLM (you need to provide your own key) to judge whether two skills are distinct, if their descriptions collide, or whether their scopes overlap.

If I run `drskill list`, I can see my current loadout. Adding `--harness claude-code` limits it to just what Claude Code sees:

![](https://www.dbreunig.com/img/resized/drskill-2.png.jpg)

If I want to know what parts of the loadout I actually *use*, I can run `drskill audit`. This scans my traces and finds all the times skills or tools are called and prints a report:

![](https://www.dbreunig.com/img/resized/drskill-3.png.jpg)

In only a week, `drskill` has been earning its keep. Beyond debugging Hermes and spotting some local issues, it’s been helpful when building my own skills and testing how they might play nicely in production.

I’ve heard from people using it to:

- **Learn why an agent reaches for the wrong skill or tool**, e.g. two descriptions overlap so a router cannot tell them apart.
- **Catch config risks before they ship**, e.g. a secret in a committed file or an unpinned server package.
- **Notice when their loadout changes**, e.g. a skill that drifted from its lockfile or a server that rewrote a tool description.
- **Write skill and tool descriptions that don’t clash with other libraries**.
- **See which skills and MCP tools their agents actually use**, and read the queries that triggered them.

`drskill` has some nice convenience functions for using it locally or as part of CI.

[Check it out!](https://github.com/dbreunig/drskill) And let me know how it helps you or could be improved.
