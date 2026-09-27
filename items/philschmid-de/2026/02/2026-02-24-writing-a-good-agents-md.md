---
title: Writing a Good AGENTS.md
link: https://www.philschmid.de/writing-good-agents
source: philschmid-de
published: 2026-02-24T00:00:00Z
updated: 2026-02-24T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn what to include, what to skip, and how to structure your AGENTS.md for best results.
content: extracted
html: 2026-02-24-writing-a-good-agents-md.html
preview:
  file: 2026-02-24-writing-a-good-agents-md.preview-13f31d8854a6.webp
  width: 256
  height: 136
  alt: Writing a Good AGENTS.md
  color: '#0a0a0a'
images:
- source: https://www.philschmid.de/static/blog/writing-good-agents/thumbnail.jpg
  original:
    file: 2026-02-24-writing-a-good-agents-md.image-19903eb855c5.jpg
    width: 1700
    height: 900
  color: '#000000'
---

An `AGENTS.md` (or `GEMINI.md`) file is the single highest-leverage configuration point for coding agents. It's injected into **every** conversation, acting as the agent's onboarding document to your codebase. But research shows that doing it wrong actively hurts performance. Here's how to do it right, backed by data from ["Evaluating AGENTS.md" (ETH Zurich, 2025)](https://arxiv.org/abs/2602.11988) and [practical experience from HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md).

## The Data Says: Less Is More

- **Auto-generated AGENTS.md files reduce task success rates by ~3%** on average across multiple agents and models, while increasing inference cost by over 20% ([§4.2](https://arxiv.org/html/2602.11988v1#S4)).
- **Human-written AGENTS.md files only marginally improve performance (~4%)**, and still increase cost by up to 19% due to extra steps the agent takes.
- **Stronger models don't generate better context files.** GPT-5.2-generated files improved performance on one benchmark by 2% but degraded it on another by 3% ([§4.4](https://arxiv.org/html/2602.11988v1#S4)).
- **Codebase overviews in AGENTS.md don't help agents navigate faster.** Agents found relevant files in roughly the same number of steps with or without an overview section ([§4.2](https://arxiv.org/html/2602.11988v1#S4)).
- **LLM-generated files are redundant with existing docs.** When all other documentation was removed, LLM-generated files actually improved performance by 2.7%—they only hurt when they duplicate what's already discoverable ([§4.2](https://arxiv.org/html/2602.11988v1#S4)).
- **Instructions ARE followed—that's the problem.** Agents respect AGENTS.md instructions, but unnecessary requirements make tasks harder, increasing reasoning tokens by 14–22% ([§4.3](https://arxiv.org/html/2602.11988v1#S4)).

## What to Include

- **The WHAT:** Your tech stack, project structure, and what each part does. Critical for monorepos—tell the agent what the apps, shared packages, and services are.
- **The WHY:** The purpose of the project and its key components. Help the agent understand intent, not just structure.
- **The HOW:** How to build, test, and verify changes. Include non-obvious tooling (e.g., `uv` instead of `pip`, `bun` instead of `npm`). Tools mentioned in AGENTS.md get used 160x more often than unmentioned ones ([§4.3](https://arxiv.org/html/2602.11988v1#S4)).

## What NOT to Include

- **Detailed codebase overviews or directory listings.** The paper found these don't help agents navigate faster, and agents can discover structure themselves.
- **Code style guidelines.** Use linters and formatters instead—they're faster, cheaper, and deterministic. LLMs are in-context learners and will follow existing patterns in your code.
- **Task-specific instructions** that only apply sometimes. Since AGENTS.md goes into every session, non-universal instructions dilute focus. Frontier models can reliably follow ~150–200 instructions; the agent harness already uses ~50 of those.
- **Auto-generated content.** Don't use `/init` or let the agent write its own AGENTS.md. The data shows this hurts more than it helps.

## How to Structure It

- **Keep it short.** General consensus is <300 lines; HumanLayer keeps theirs under 60 lines. Every line goes into every session—make each one count.
- **Use progressive disclosure.** Don't put everything in AGENTS.md. Instead, keep task-specific docs in separate files (e.g., `agent_docs/running_tests.md`, `agent_docs/database_schema.md`) and list them in AGENTS.md with brief descriptions so the agent reads them only when relevant.
- **Prefer pointers over copies.** Reference `file:line` locations rather than embedding code snippets that will go stale.
- **Write it yourself, deliberately.** A bad line in AGENTS.md cascades into bad plans, bad code, and bad results across every session. Treat it like infrastructure, not a scratchpad.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
