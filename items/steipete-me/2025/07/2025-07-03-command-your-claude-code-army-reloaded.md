---
title: Command your Claude Code Army, Reloaded
link: https://steipete.me/posts/command-your-claude-code-army-reloaded/
source: steipete-me
published: 2025-07-03T00:00:00Z
updated: 2025-07-03T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Enhance your Claude Code workflow with VibeTunnel terminal title management for better multi-session tracking
content: extracted
html: 2025-07-03-command-your-claude-code-army-reloaded.html
preview:
  file: 2025-07-03-command-your-claude-code-army-reloaded.preview-bc65035827e3.webp
  width: 256
  height: 134
  color: '#eae8e8'
images:
- source: https://steipete.me/posts/command-your-claude-code-army-reloaded/index.png
  original:
    file: 2025-07-03-command-your-claude-code-army-reloaded.image-c69d6ffb9b77.png
    width: 1200
    height: 630
  variants:
  - file: 2025-07-03-command-your-claude-code-army-reloaded.image-c1a3e23c8a83.webp
    width: 320
    height: 168
  - file: 2025-07-03-command-your-claude-code-army-reloaded.image-34c9fc23047d.webp
    width: 640
    height: 336
  - file: 2025-07-03-command-your-claude-code-army-reloaded.image-cdcc3e624f20.webp
    width: 960
    height: 504
  - file: 2025-07-03-command-your-claude-code-army-reloaded.image-57c1b36b39b1.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2025/command-your-claude-code-army-reloaded/vibetunnel.png
  original:
    file: 2025-07-03-command-your-claude-code-army-reloaded.image-cd17562987af.png
    width: 794
    height: 902
  color: '#c4c4f2'
---

![](https://steipete.me/assets/img/2025/command-your-claude-code-army-reloaded/vibetunnel.png)

Managing multiple Claude Code sessions just got a whole lot easier. With [VibeTunnel](https://vibetunnel.sh/)’s new terminal title management feature, you can now see at a glance what each Claude instance is working on across your projects.

The screenshot above shows the power of this feature: each Claude session displays exactly what it’s working on, and Claude does this automatically without us having to ask for it. You can also set custom titles via `vt title "Custom title"` for more control. Clicking on a session selects that terminal, and you can also click on the folder icon to open Finder or on the Git info to open your Git client. This is all new in VibeTunnel 1.0 Beta 6, which you can download at [vibetunnel.sh](https://vibetunnel.sh/).

I tried the solution from my [previous post](https://steipete.me/posts/2025/commanding-your-claude-code-army/), but Claude kept rewriting the terminal title, so I needed a better solution—hence this VibeTunnel integration. Note that this only works for Claude instances that are started with the `vt` command as prefix (e.g., `vt claude`).

## VibeTunnel Terminal Title Management

Here’s the complete section to add to your `~/.claude/CLAUDE.md` file:

```plaintext
## VibeTunnel Terminal Title Management

When working in VibeTunnel sessions, actively use the `vt title` command to communicate your current actions and progress:

### Usage
vt title "Current action - project context"

### Guidelines
- **Update frequently**: Set the title whenever you start a new task, change focus, or make significant progress
- **Be descriptive**: Use the title to explain what you're currently doing (e.g., "Analyzing test failures", "Refactoring auth module", "Writing documentation")
- **Include context**: Add PR numbers, file names, or feature names when relevant
- **Think of it as a status indicator**: The title helps users understand what you're working on at a glance
- If `vt` command fails (only works inside VibeTunnel), simply ignore the error and continue

### Examples
# When starting a task
vt title "Setting up Git app integration"

# When debugging
vt title "Debugging CI failures - playwright tests"

# When working on a PR
vt title "Implementing unique session names - github.com/amantus-ai/vibetunnel/pull/456"

# When analyzing code
vt title "Analyzing session-manager.ts for race conditions"

# When writing tests
vt title "Adding tests for GitAppLauncher"

### When to Update
- At the start of each new task or subtask
- When switching between different files or modules
- When changing from coding to testing/debugging
- When waiting for long-running operations (builds, tests)
- Whenever the user might wonder "what is Claude doing right now?"

This helps users track your progress across multiple VibeTunnel sessions and understand your current focus.
```

## Implementation

To enable this feature in your Claude Code setup, you have two options:

1. **Automatic setup**: Simply paste the URL of this blog post into Claude and tell it to set it up for you.

2. **Manual setup**: Add the configuration above to your global Claude rules at `~/.claude/CLAUDE.md`. You can also check out the [full gist](https://gist.github.com/steipete/c297c84e1684c330b3325825d835da03) for additional implementation details.

By keeping your terminal titles updated, you can effectively manage your “Claude Code Army” - running multiple AI assistants in parallel across different projects while maintaining clear visibility into what each one is doing.
