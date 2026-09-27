---
title: Commanding Your Claude Code Army
link: https://steipete.me/posts/2025/commanding-your-claude-code-army/
source: steipete-me
published: 2025-06-05T09:00:00Z
updated: 2025-06-05T09:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: How a simple terminal trick helps me manage multiple Claude Code instances without losing my mind (or my terminal tabs)
content: extracted
html: 2025-06-05-commanding-your-claude-code-army.html
preview:
  file: 2025-06-05-commanding-your-claude-code-army.preview-8b8c3831fc07.webp
  width: 256
  height: 134
  color: '#eceaea'
images:
- source: https://steipete.me/posts/2025/commanding-your-claude-code-army/index.png
  original:
    file: 2025-06-05-commanding-your-claude-code-army.image-de24fd03e5c7.png
    width: 1200
    height: 630
  variants:
  - file: 2025-06-05-commanding-your-claude-code-army.image-013b52403b26.webp
    width: 320
    height: 168
  - file: 2025-06-05-commanding-your-claude-code-army.image-9aead962b985.webp
    width: 640
    height: 336
  - file: 2025-06-05-commanding-your-claude-code-army.image-48129ac54374.webp
    width: 960
    height: 504
  - file: 2025-06-05-commanding-your-claude-code-army.image-31eb4a85f87c.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2025/commanding-your-claude-code-army/header.png
  original:
    file: 2025-06-05-commanding-your-claude-code-army.image-e35fad2b098b.png
    width: 1726
    height: 1060
  color: '#292c33'
---

![](https://steipete.me/assets/img/2025/commanding-your-claude-code-army/header.png)

> **Update**: There’s a better solution now! Check out [Command Your Claude Code Army, Reloaded](https://steipete.me/posts/2025/command-your-claude-code-army-reloaded/) for VibeTunnel integration that gives Claude automatic title management.

**TL;DR**: Running multiple Claude Code instances? Here’s a ZSH trick to keep your terminal titles organized and your sanity intact.

I’ll admit it - I’ve become a [Claude Code](https://www.anthropic.com/claude-code) power user. At any given moment, I have at least three instances running: one for writing blog posts, another fixing that “quick” bug that’s now in hour three, and a third doing something I definitely forgot about. My terminal looks like a Claude Code convention.

## The Problem: Too Many Claudes, Not Enough Context

[Ghostty](https://ghostty.org/) (yes, with two T’s - it’s my favorite terminal) only shows the process name in the title. When you have six tabs all saying “claude”, finding the right one becomes a game of terminal roulette.

This is especially painful when you’re running Claude with the `--dangerously-skip-permissions` flag (yes, I live dangerously - see my post about [Claude Code being my computer](https://steipete.me/posts/2025/claude-code-is-my-computer/)). The last thing you want is to accidentally run the wrong command in the wrong directory with full system permissions.

To make matters worse, Claude Code has a habit of changing the terminal title to whatever it feels like - usually something unhelpful like “Running command…” or just “claude”. It’s like having a helpful assistant who keeps reorganizing your desk while you’re trying to work.

## The Solution: Terminal Title Magic

Here’s my approach - a clean ZSH setup that keeps terminal management code separate from my main configuration. First, I add one line to my `~/.zshrc`:

```zsh
# Source Claude wrapper with dynamic terminal title
[ -f ~/.config/zsh/claude-wrapper.zsh ] && source ~/.config/zsh/claude-wrapper.zsh
```

Then I create `~/.config/zsh/claude-wrapper.zsh` with the actual implementation:

```zsh
#!/usr/bin/env zsh
# Simple, elegant terminal title management for Claude

# Set terminal title
_set_title() { 
    print -Pn "\e]2;$1\a" 
}

# Claude wrapper with custom terminal title
cly() {
    local folder=${PWD:t}  # Just the current folder name
    
    # Set title to show we're running Claude
    _set_title "$folder — Claude"
    
    # Start a background process to continuously reset the title
    # (prevents Claude from changing it)
    (
        while true; do
            _set_title "$folder — Claude"
            sleep 0.5
        done
    ) &
    local title_pid=$!
    
    # Run Claude with dangerous permissions
    "$HOME/.claude/local/claude" --dangerously-skip-permissions "$@"
    local exit_code=$?
    
    # Kill the background title setter
    kill $title_pid 2>/dev/null
    wait $title_pid 2>/dev/null  # Clean up zombie process
    
    # Restore normal title
    _set_title "%~"
    
    # Return the original exit code
    return $exit_code
}

# Update terminal title before each prompt (using proper ZSH hooks)
_claude_precmd() {
    _set_title "%~"
}

# Add our precmd function to the array (doesn't overwrite existing hooks)
if [[ -z ${precmd_functions[(r)_claude_precmd]} ]]; then
    precmd_functions+=(_claude_precmd)
fi
```

When I run `cly`, my terminal title changes from `~/Projects/blog` to `~/Projects/blog — Claude`. Revolutionary? No. Life-changing? Absolutely.

> *Hot tip: Just ask Claude to read this blog post and set it up for you! (in yolo mode, of course)*

Now if you’ll excuse me, I need to check on my Claude instances. I think one of them is refactoring my entire codebase without asking. Again.
