---
title: Peekaboo MCP – lightning-fast macOS screenshots for AI agents
link: https://steipete.me/posts/2025/peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents/
source: steipete-me
published: 2025-06-07T11:00:00Z
updated: 2025-06-07T11:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Turn your blind AI into a visual debugger with instant screenshot capture and analysis
content: extracted
html: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.html
preview:
  file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.preview-559c3f312db1.webp
  width: 256
  height: 134
  color: '#e3e0e0'
images:
- source: https://steipete.me/posts/2025/peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents/index.png
  original:
    file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-9a94a155a362.png
    width: 1200
    height: 630
  variants:
  - file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-cd81ed9f0425.webp
    width: 320
    height: 168
  - file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-4cc5c828a702.webp
    width: 640
    height: 336
  - file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-98ece61cc3a0.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2025/peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents/banner.png
  original:
    file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-d285d591ef67.png
    width: 1536
    height: 1024
  color: '#051b33'
- source: https://cursor.com/deeplink/mcp-install-dark.png
  original:
    file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-27df65dd243c.png
    width: 337
    height: 78
  variants:
  - file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-7e32f24b894e.webp
    width: 337
    height: 78
  color: '#000000'
- source: https://cursor.com/deeplink/mcp-install-light.png
  original:
    file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-64ce78446ca2.png
    width: 337
    height: 78
  variants:
  - file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-fcf473a4b301.webp
    width: 337
    height: 78
  color: '#fefefe'
- source: https://steipete.me/assets/img/2025/peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents/cursor-40-tools.png
  original:
    file: 2025-06-07-peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents.image-3e2207d1a5b3.png
    width: 1024
    height: 870
  color: '#252524'
---

![](https://steipete.me/assets/img/2025/peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents/banner.png)

**TL;DR**: Peekaboo is a macOS-only MCP server that enables AI agents to capture screenshots of applications, or the entire system, with optional visual question answering through local or remote AI models.

> Without screenshots, agents debug blind—Peekaboo gives them eyes.

## What Peekaboo Can Do

Peekaboo provides three main tools that give AI agents visual capabilities:

- **`image`** - Capture screenshots of screens or specific applications
- **`analyze`** - Ask AI questions about captured images using vision models
- **`list`** - Enumerate available screens and windows for targeted captures

Each tool is designed to be powerful and flexible. The most powerful feature is visual question answering - agents can ask questions about screenshots like “What do you see in this window?” or “Is the submit button visible?” and get accurate answers. This saves context space since asking specific questions is much more efficient than returning raw image data.

Peekaboo supports both cloud and local vision models, letting you choose between accuracy and privacy.

 ![Install Peekaboo in Cursor IDE](https://cursor.com/deeplink/mcp-install-dark.png) ![Install Peekaboo in Cursor IDE](https://cursor.com/deeplink/mcp-install-light.png)

## Design Philosophy

### Less is More

The most important rule when building MCPs: **Keep the number of tools small**. Most agents struggle once they encounter more than 40 different tools. My approach is to make every tool very powerful but keep the total count minimal to avoid cluttering the context.

![Cursor showing 40+ tools can become overwhelming](https://steipete.me/assets/img/2025/peekaboo-mcp-lightning-fast-macos-screenshots-for-ai-agents/cursor-40-tools.png)

### Lenient Tool Calling

Another crucial principle: **tool calling should be lenient**. Agents make mistakes with parameters, so rather than returning errors, Peekaboo tries to understand their intent. Being overly strict just forces unnecessary retry loops - MCPs should be forgiving since agents aren’t infallible.

### Fuzzy Window Matching

Peekaboo implements fuzzy window matching because agents don’t always know exact window titles. If an agent asks for “Chrome” but the window is titled “Google Chrome - Peekaboo MCP”, we still match it. Partial matches work, case doesn’t matter, and common variations are understood.

For more insights on building robust MCP tools, check out my guide: [MCP Best Practices](https://steipete.me/posts/2025/mcp-best-practices).

## Local vs Cloud Vision Models

Peekaboo supports both local and cloud vision models. While cloud models like GPT-4o offer superior accuracy, local models provide privacy, cost control, and offline operation.

For local inference, I recommend LLaVA as the default for its balance of accuracy and performance. For resource-constrained systems, Qwen2-VL provides excellent results with lower requirements.

Model specifications and requirements

**[LLaVA](https://ollama.com/library/llava) (Large Language and Vision Assistant)**

- `llava:7b` - ~4.5GB download, ~8GB RAM required
- `llava:13b` - ~8GB download, ~16GB RAM required
- `llava:34b` - ~20GB download, ~40GB RAM required
- Best overall quality for vision tasks

**[Qwen2-VL](https://ollama.com/library/qwen2-vl)**

- `qwen2-vl:7b` - ~4GB download, ~6GB RAM required
- Excellent performance with lower resource requirements
- Ideal for less powerful machines

**Installation:**

```bash
# Install your chosen model
ollama pull llava:latest        # or llava:7b, llava:13b, etc.
ollama pull qwen2-vl:7b        # for resource-constrained systems
```

## My MCP Ecosystem

Peekaboo is part of a growing collection of MCP servers I’m building:

- **[claude-code-mcp](https://github.com/steipete/claude-code-mcp)** - Integrates Claude Code into Cursor for task offloading
- **[macos-automator-mcp](https://github.com/steipete/macos-automator-mcp)** - Run AppleScript and JXA on macOS
- **[Terminator](https://github.com/steipete/Terminator)** - External terminal so agents don’t get stuck on long-running commands

Each serves a specific purpose in building autonomous AI workflows.

## Technical Architecture

Peekaboo combines TypeScript and Swift for the best of both worlds. TypeScript provides excellent [MCP support](https://github.com/modelcontextprotocol/typescript-sdk) and easy distribution via npm, while Swift enables direct access to Apple’s [ScreenCaptureKit](https://developer.apple.com/documentation/screencapturekit) for capturing windows without focus changes.

My initial [AppleScript prototype](https://github.com/steipete/Peekaboo/blob/main/peekaboo.scpt) had a fatal flaw: it required focus changes to capture windows. The Swift rewrite uses ScreenCaptureKit to access the window manager directly - no focus changes, no user disruption.

The system uses a Swift CLI that communicates with a Node.js MCP server, supporting both local models and cloud providers with automatic fallback. Built with Swift 6 and the new Swift Testing framework ([now that I have experience with it!](https://steipete.me/posts/migrating-700-tests-to-swift-testing)), Peekaboo delivers fast, non-intrusive screenshot capture with intelligent window matching.

For detailed testing instructions using the MCP Inspector, see the [Peekaboo README](https://github.com/steipete/Peekaboo#testing--debugging).

## The Vision: Autonomous Agent Debugging

Peekaboo is like one puzzle piece in a larger set of MCPs I’m building to help agents stay in the loop. The goal is simple: if an agent can answer questions by itself, you don’t have to intervene and it can simply continue and debug itself. This is the holy grail for building applications with CI - you want to do everything so the agent can loop and work until what you want is done.

When your build fails, when your UI doesn’t look right, when something breaks - instead of stopping and asking you “what do you see?”, the agent can take a screenshot, analyze it, and continue fixing the problem autonomously. That’s the power of giving agents their eyes.

👻 [Peekaboo MCP](https://www.peekaboo.dev/) is available now - ⭐ [the repo](https://github.com/steipete/Peekaboo) if this saves you a debug session!
