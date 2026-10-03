---
title: Slashing Custom Commands
link: https://ampcode.com/news/slashing-custom-commands
source: ampcode-com
published: 2026-01-29T00:00:00Z
updated: 2026-01-29T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'We''re removing custom commands in favor of skills. They were two ways of doing the same thing, except that only you could invoke custom commands, and only the agent could invoke skills. Now that you can invoke skills directly, there''s no reason to keep custom commands around anymore. Migrating Custom Commands to Skills It''s easy and takes just a few minutes. Let Amp Do It For You If you have custom commands in .agents/commands/ or ~/.config/amp/commands/, ask Amp to migrate them for you with the following prompt: Migrate my custom commands from .agents/commands/ to skills in .agents/skills. For each command: 1. Create a new skill directory in .agents/skills/ named after the command 2. If it''s a markdown file, rename it to SKILL.md and add frontmatter with name and description 3. If it''s an executable, create a SKILL.md that describes when and how to run the script, and move the executable to a scripts/ subdirectory 4. Delete the original command file Also do this for global custom commands in ~/.config/amp/commands (which should be migrated to skills ~/.config/agents/skills). Or Do It Manually Markdown commands: Move your .md file to a skill directory: # Before .agents/commands/code-review.md # After .agents/skills/code-review/SKILL.md Add frontmatter: --- name: code-review description: Review code for quality and best practices --- [Your existing content] Executable commands: Move the executable into a scripts/ subdirectory: # Before .agents/commands/deploy # After .agents/skills/deploy/ ├── SKILL.md └── scripts/ └── deploy In SKILL.md, tell the agent how to use it: --- name: deploy description: Deploy the application --- Run `{baseDir}/scripts/deploy` to deploy.'
content: extracted
html: 2026-01-29-slashing-custom-commands.html
preview:
  file: 2026-01-29-slashing-custom-commands.preview-4387fafe3fa7.webp
  width: 256
  height: 134
  color: '#5c554b'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Slashing+Custom+Commands&date=January+29%2C+2026&tagline=Custom+commands+are+gone.+Use+skills+instead.&sig=649be8223a542cf8e10599f307d83cc285be2bdcb89d75c438de282835ef383f
  original:
    file: 2026-01-29-slashing-custom-commands.image-ea6958168469.png
    width: 1200
    height: 630
  variants:
  - file: 2026-01-29-slashing-custom-commands.image-daf2c392a726.webp
    width: 320
    height: 168
  - file: 2026-01-29-slashing-custom-commands.image-308252c2c82a.webp
    width: 640
    height: 336
  - file: 2026-01-29-slashing-custom-commands.image-c6343c133c47.webp
    width: 960
    height: 504
  - file: 2026-01-29-slashing-custom-commands.image-1076f54d14d4.webp
    width: 1200
    height: 630
  color: '#221b16'
---

We're removing custom commands in favor of [skills](https://ampcode.com/manual#agent-skills). They were two ways of doing the same thing, except that only you could invoke custom commands, and only the agent could invoke skills. Now that you can invoke skills directly, there's no reason to keep custom commands around anymore.

## Migrating Custom Commands to Skills

It's easy and takes just a few minutes.

### Let Amp Do It For You

If you have custom commands in `.agents/commands/` or `~/.config/amp/commands/`, ask Amp to migrate them for you with the following prompt:

```
Migrate my custom commands from .agents/commands/ to skills in .agents/skills.

For each command:
1. Create a new skill directory in .agents/skills/ named after the command
2. If it's a markdown file, rename it to SKILL.md and add frontmatter with name and description
3. If it's an executable, create a SKILL.md that describes when and how to run the script, and move the executable to a scripts/ subdirectory
4. Delete the original command file

Also do this for global custom commands in ~/.config/amp/commands (which should be migrated to skills ~/.config/agents/skills).
```

### Or Do It Manually

**Markdown commands:** Move your `.md` file to a skill directory:

```
# Before
.agents/commands/code-review.md

# After
.agents/skills/code-review/SKILL.md
```

Add frontmatter:

```markdown
---
name: code-review
description: Review code for quality and best practices
---

[Your existing content]
```

**Executable commands:** Move the executable into a `scripts/` subdirectory:

```
# Before
.agents/commands/deploy

# After
.agents/skills/deploy/
├── SKILL.md
└── scripts/
    └── deploy
```

In `SKILL.md`, tell the agent how to use it:

```markdown
---
name: deploy
description: Deploy the application
---

Run `{baseDir}/scripts/deploy` to deploy.
```
