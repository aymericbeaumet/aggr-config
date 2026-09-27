---
title: Patching Claude Code for debugging and /cost support for Max users
link: https://mariozechner.at/posts/2025-08-06-cc-antidebug
source: mariozechner-at
published: 2025-08-06T00:00:00Z
updated: 2025-08-06T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: cc-antidebug patches Claude Code to enable Node.js debugger support for SDK-based apps and restores the /cost command functionality for Max plan users
content: extracted
html: 2025-08-06-patching-claude-code-for-debugging-and-cost-support-for-max.html
preview:
  file: 2025-08-06-patching-claude-code-for-debugging-and-cost-support-for-max.preview-48e8a528e9c8.webp
  width: 256
  height: 132
  color: '#1e1e1e'
images:
- source: https://mariozechner.at/posts/2025-08-06-cc-antidebug/media/header.png
  original:
    file: 2025-08-06-patching-claude-code-for-debugging-and-cost-support-for-max.image-086269cc9a4e.png
    width: 1316
    height: 678
  color: '#191919'
---

![](https://mariozechner.at/posts/2025-08-06-cc-antidebug/media/header.png)\
cc-antidebug patches

- [No debugging allowed](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_0)
- [But at what cost?](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_1)
- [Patch all the things](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_2)
- [cc-antidebug](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_3)
  - [CLI Usage](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_4)
  - [Programmatic Usage](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_5)
- [Results](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_6)
- [What else can we do?](https://mariozechner.at/posts/2025-08-06-cc-antidebug#toc_7)

I love Claude Code, it's really good, but it also has some annoyances.

## No debugging allowed

I've recently looked into using the [Claude Code SDK](https://docs.anthropic.com/en/docs/claude-code/sdk). The SDK comes in different flavors. The simplest one is just invoking Claude on the CLI with `--print`. There's also a TypeScript SDK and a Python SDK. Both of these basically spawn Claude Code as a child process and then communicate with it via stdin and stdout.

When you debug a Node.js app that uses the Claude Code TypeScript SDK, the SDK call will fail. Claude Code has anti-debugging functionality that detects if a debugger is attached. When I use VS Code's JavaScript Debug Terminal, it sets environment variables that get inherited through the process tree. The spawned Claude Code process sees these debugger environment variables and immediately exits. This is very annoying.

## But at what cost?

If you type `/cost` into Claude Code logged in through your Pro or Max plan, Claude Code will not show you the token usage and cost of the current session. Instead, you'll be greeted with a little message that tells you, basically, don't worry your little head about it. You have a plan. You don't need to know the numbers. Numbers are bad for you. That too is very annoying.

## Patch all the things

I've already managed to disable the anti-debugging inside of Claude Code, as you've learned by reading my [last amazing blog post in full](https://mariozechner.at/posts/2025-08-03-cchistory/). The way I approach this mirrors how I would approach patching a native executable. It's just way easier with JavaScript.

First I formatted the Claude Code executable using Biome. Then I searched for strings that would be used to detect a debugger being attached to the Node.js process, like `--inspect-brk`. Once I found the function that does the anti-debugging check, I just wrote a little function using unholy regex to rip out the checks and replace them with no-ops. The function now tells Claude Code, "nope, no debugging here. Everything's totally fine."

The same principle can be applied to allowing `/cost` to show us token usage and costs, even if we are logged in with a Max plan. Claude Code will output this string: "With your Claude Max subscription, no need to monitor cost — your subscription includes Claude Code usage". So again, I just locate the string in the formatted binary, look at the context surrounding it, and write a bunch more unholy regexes to replace it with a no-op that says: "No plan here. You can totally go ahead and just show the token usage and cost. Numbers are good now."

## cc-antidebug

Now I like tools that just do the thing and I don't have to think about it. And I like to share my tools. Which is why I've built [cc-antidebug](https://github.com/badlogic/cc-antidebug) which is now available on NPM. Here's how you can use it.

### CLI Usage

Just want to patch your global install once? Use this. And you can also restore the original binary. Note that Claude Code will update every now and then. You'll need to reapply the patch.

```bash
# Apply the patch
npx @mariozechner/cc-antidebug patch

# Restore the original binary
npx @mariozechner/cc-antidebug restore
```

### Programmatic Usage

You can also use it inside your Node.js apps. This is relevant for CI and other similar use cases where you want to just temporarily patch and restore the unpatched version again:

```bash
npm install @mariozechner/cc-antidebug
```

```javascript
import { patchClaudeBinary, restoreClaudeBinary } from "@mariozechner/cc-antidebug";

// Apply the patch
patchClaudeBinary();

// Do your debugging work here...

// Restore the original
restoreClaudeBinary();
```

## Results

## What else can we do?

Well, I suppose the sky is the limit. Even though the source code is obfuscated and minified, it's still pretty readable (after formatting with Biome). And if you can't read it or follow the execution flow, just give it to Claude. If you patch your Claude Code installation with cc-antidebug, you can even debug interactive Claude Code sessions. That way you can identify and eliminate other annoyances. Of course, Claude Code changes over time so your patches might not apply until the heat death of the universe. But it's easy enough to update your patches for new Claude Code versions.

This page respects your privacy by not using cookies or similar technologies and by not collecting any personally identifiable information.
