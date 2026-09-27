---
title: 'Agents: Inner Loop vs Outer Loop'
link: https://www.philschmid.de/inner-loop-vs-outer-loop
source: philschmid-de
published: 2026-02-20T00:00:00Z
updated: 2026-02-20T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Most agent frameworks share the same hardcoded tool loop; what differs is how the model uses it. This post explains the inner loop—an agent verifying its own work within a task—and the outer loop—an agent carrying lessons across tasks via persistent memory, skills, and rules files—and why both are needed for agents that feel reliable and get smarter over time.
content: extracted
html: 2026-02-20-agents-inner-loop-vs-outer-loop.html
preview:
  file: 2026-02-20-agents-inner-loop-vs-outer-loop.preview-a6d168750668.webp
  width: 256
  height: 136
  alt: 'Agents: Inner Loop vs Outer Loop'
  color: '#090909'
images:
- source: https://www.philschmid.de/static/blog/inner-loop-vs-outer-loop/thumbnail.jpg
  original:
    file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-426c74d67ac2.jpg
    width: 1700
    height: 900
  color: '#000000'
- source: https://www.philschmid.de/static/blog/inner-loop-vs-outer-loop/bad-agent.png
  original:
    file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-bef3e0243aa0.png
    width: 3922
    height: 2705
  variants:
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-6a272908eb43.webp
    width: 320
    height: 221
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-711ec823eb5f.webp
    width: 640
    height: 441
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-11a545680ea8.webp
    width: 960
    height: 662
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-26be6368a37d.webp
    width: 1280
    height: 883
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-d380286b6b4d.webp
    width: 1600
    height: 1104
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-dbb32f29275f.webp
    width: 3922
    height: 2705
  color: '#fdfdfd'
- source: https://www.philschmid.de/static/blog/inner-loop-vs-outer-loop/good-agent.png
  original:
    file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-a8d4858f5a46.png
    width: 4770
    height: 4820
  variants:
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-49db0faceef5.webp
    width: 320
    height: 323
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-15d1dbd45905.webp
    width: 640
    height: 647
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-776c9df38245.webp
    width: 960
    height: 970
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-55febb3e0f0e.webp
    width: 1280
    height: 1293
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-7d1293fd1359.webp
    width: 1600
    height: 1617
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-afa7f77f5bdb.webp
    width: 4770
    height: 4820
  color: '#fdfdfd'
- source: https://www.philschmid.de/static/blog/inner-loop-vs-outer-loop/outer-loop.png
  original:
    file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-087c9cadb3a7.png
    width: 4329
    height: 2753
  variants:
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-613e87d54f71.webp
    width: 320
    height: 204
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-b5c7f34627e2.webp
    width: 640
    height: 407
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-91409691e036.webp
    width: 960
    height: 611
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-811536771992.webp
    width: 1280
    height: 814
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-539787f0f5dd.webp
    width: 1600
    height: 1018
  - file: 2026-02-20-agents-inner-loop-vs-outer-loop.image-a1a866f71222.webp
    width: 4329
    height: 2753
  color: '#fcfcfc'
---

When people talk about AI agents "closing the loop," they usually mean the agent verifies its own work before responding, which can be confusing as "If the tool call loop is hardcoded, how does the model decide to verify its work?"

Short answer: **The loop is hardcoded. What the model does *inside* the loop is not.**

Every agent framework runs roughly the same cycle: model generates → if tool call, execute it → feed result back → model generates again → repeat until the model returns text (no more tool calls). That loop is scaffolding. It's the same for every agent.

The difference is what the model *chooses to do* within that loop. A model that "closes the loop" doesn't need a special loop, it uses the existing one to call verification tools *before* deciding it's done.

## The inner loop: agent verifies its own work

The **inner loop** is what happens during a single task, before the agent responds to user with a text. The agent writes code, runs the tests, reads the error, fixes the edge case, re-runs and only then generates a text response for the user. It's the tight feedback cycle between the model and its tools.

**Example:** You ask "fix the failing test in `auth.ts`."

A weak agent edits the file and says "Done!" A strong agent edits → creates tests → runs the tests → sees a failure → fixes the edge case → runs again → sees green → then responds. Same infrastructure, different behavior.

**Bad agent** — edits and stops:

![bad-agent](https://www.philschmid.de/static/blog/inner-loop-vs-outer-loop/bad-agent.png)

**Good agent** — verifies before responding:

![bad-agent](https://www.philschmid.de/static/blog/inner-loop-vs-outer-loop/good-agent.png)

Both agents use the exact same hardcoded tool loop. The difference is the good agent *chose* to create/call verfications before responding. Nothing forced it to.

Where does that choice come from? Today, mostly from the system prompt ("always run tests after code changes"). Increasingly, from post-training — e.g. test pass/fail as a reward signal, so the model internalizes the verify step.

## The outer loop: learning across turns

The **outer loop** is what happens across multiple turns/sessions between the user and the agent over time. The user gives the agent a task, it works on it (inner loop), returns a result, and then the user comes back with the next task. The question is: did the agent learn anything from the last turn?

Without persistent memory, every turn is a clean slate. The agent that failed on pagination yesterday will fail on it again today.

![outer-loop](https://www.philschmid.de/static/blog/inner-loop-vs-outer-loop/outer-loop.png)

Almost no agent does this natively today. The outer loop requires persistent state, skills, rules files, or notes that survive between turns and sessions. Some early examples:

- **AGENTS.md:** Manual persistent instructions the user writes for future turns
- **Session handoff documents:** Structured summaries an agent writes for follow-up work
- **SKILL.md:** Agent analyzes failures and auto-generates Skills so the next task doesn't repeat mistakes

The inner loop is about reliability within a task. The outer loop is about getting smarter over time.

## TL;DR

- **The loop is hardcoded.** Every agent has the same generate → tool call → feed back cycle.
- **What the agent does inside the loop is learned.** A good agent calls verification tools before responding. A weak one just stops.
- **Inner loop** = verify within a task (write tests, run them, read files back, check against original ask).
- **Outer loop** = carry lessons across turns (persistent memory, skills, rules files).
- **Closing the loop ≠ new infrastructure.** It's the agent making better decisions within existing infrastructure.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
