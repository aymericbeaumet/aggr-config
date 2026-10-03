---
title: Agent Skills
link: https://ampcode.com/news/agent-skills
source: ampcode-com
published: 2025-12-10T00:00:00Z
updated: 2025-12-10T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Amp supports agent skills. Skills let the agent lazily-load specific instructions on how to use local tools. We like skills because they improve agent tool use performance in a very context efficient way. Skills are installed to .agents/skills/ in your workspace by default. Amp also reads from ~/.config/agents/skills/ for user-level skills, and .claude/skills/ and ~/.claude/skills/ for compatibility with existing skills. Our team has been doing a ton of experimenting with skills over the past few weeks. Here are a few that we have found particularly useful: Agent Sandbox: Isolated execution environment for running untrusted code safely. Agent Skill Creator: Meta-skill for creating Claude agents autonomously with comprehensive skill architecture patterns. BigQuery: Expert use of the bq cli tool for querying BigQuery datasets. Tmux: Run servers and long-running tasks in the background. Web Browser: Interact with web pages via Chrome DevTools Protocol for clicking, filling forms, and navigation. Read more about how to use skills in our manual https://ampcode.com/manual#agent-skills'
content: extracted
html: 2025-12-10-agent-skills.html
preview:
  file: 2025-12-10-agent-skills.preview-e4d4bf9e5902.webp
  width: 256
  height: 134
  color: '#5f584e'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Agent+Skills&date=December+10%2C+2025&tagline=Reuse+your+existing+agent+skills+in+Amp&sig=1089b48efeb7edf53dcf6531f328317c6673972156e0ceba75027a3a7309ef30
  original:
    file: 2025-12-10-agent-skills.image-cd7ae4b104e0.png
    width: 1200
    height: 630
  variants:
  - file: 2025-12-10-agent-skills.image-cf51fd36ff37.webp
    width: 320
    height: 168
  - file: 2025-12-10-agent-skills.image-5f3a8a460bdd.webp
    width: 640
    height: 336
  - file: 2025-12-10-agent-skills.image-d27a283ce177.webp
    width: 960
    height: 504
  - file: 2025-12-10-agent-skills.image-f22668d90da8.webp
    width: 1200
    height: 630
  color: '#221b16'
---

Amp supports agent skills. Skills let the agent lazily-load specific instructions on how to use local tools. We like skills because they improve agent tool use performance in a very context efficient way.

Skills are installed to `.agents/skills/` in your workspace by default. Amp also reads from `~/.config/agents/skills/` for user-level skills, and `.claude/skills/` and `~/.claude/skills/` for compatibility with existing skills.

Our team has been doing a ton of experimenting with skills over the past few weeks. Here are a few that we have found particularly useful:

- **[Agent Sandbox](https://github.com/disler/agent-sandbox-skill)**: Isolated execution environment for running untrusted code safely.
- **[Agent Skill Creator](https://github.com/FrancyJGLisboa/agent-skill-creator)**: Meta-skill for creating Claude agents autonomously with comprehensive skill architecture patterns.
- **[BigQuery](https://github.com/ampcode/amp/blob/main/.agents/skills/bigquery/SKILL.md)**: Expert use of the bq cli tool for querying BigQuery datasets.
- **[Tmux](https://github.com/ampcode/amp-contrib/blob/main/.agents/skills/tmux/skill.md)**: Run servers and long-running tasks in the background.
- **[Web Browser](https://github.com/mitsuhiko/agent-stuff/blob/main/skills/web-browser/SKILL.md)**: Interact with web pages via Chrome DevTools Protocol for clicking, filling forms, and navigation.

Read more about how to use skills in our manual [https://ampcode.com/manual#agent-skills](https://ampcode.com/manual#agent-skills)
