---
title: 'llm.codes: Make Apple Docs AI-Readable'
link: https://steipete.me/posts/2025/llm-codes-transform-developer-docs/
source: steipete-me
published: 2025-06-14T13:22:16Z
updated: 2025-06-14T13:22:16Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Built this when Claude couldn't read Apple's docs. Now it converts 69+ documentation sites to clean llms.txt. Free, instant, no BS.
content: extracted
html: 2025-06-14-llm-codes-make-apple-docs-ai-readable.html
preview:
  file: 2025-06-14-llm-codes-make-apple-docs-ai-readable.preview-c8df1e8354ea.webp
  width: 256
  height: 134
  color: '#ebe8e8'
images:
- source: https://steipete.me/posts/2025/llm-codes-transform-developer-docs/index.png
  original:
    file: 2025-06-14-llm-codes-make-apple-docs-ai-readable.image-e4c5e1d594f6.png
    width: 1200
    height: 630
  variants:
  - file: 2025-06-14-llm-codes-make-apple-docs-ai-readable.image-c2690926a373.webp
    width: 320
    height: 168
  - file: 2025-06-14-llm-codes-make-apple-docs-ai-readable.image-f672f04c06cf.webp
    width: 640
    height: 336
  - file: 2025-06-14-llm-codes-make-apple-docs-ai-readable.image-00fbcbd276be.webp
    width: 960
    height: 504
  - file: 2025-06-14-llm-codes-make-apple-docs-ai-readable.image-e377e02d1d78.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2025/llm-codes-transform-developer-docs/hero.png
  original:
    file: 2025-06-14-llm-codes-make-apple-docs-ai-readable.image-4d0ff6d2c024.png
    width: 1536
    height: 1024
  color: '#2877fe'
---

![](https://steipete.me/assets/img/2025/llm-codes-transform-developer-docs/hero.png)

**TL;DR**: [llm.codes](https://llm.codes) converts JavaScript-heavy Apple docs (and 69+ other sites) into a clean llms.txt that AI agents can actually read.

> **Quick Start**: Try it now with Apple’s Foundation Models docs: [llm.codes](https://llm.codes?https://developer.apple.com/documentation/foundationmodels)

Even the smartest models can’t fetch fresh docs - especially when the docs are hidden behind JavaScript. While working on [Vibe Meter](https://vibemeter.ai/), Claude tried to convince me that [it wasn’t possible to make a proper toolbar in SwiftUI](https://x.com/steipete/status/1933819029224931619) and went down to AppKit. Even when I asked it to google for a solution, nothing changed.

## The Real Problem: JavaScript-Heavy Documentation

The core issue? [Apple’s documentation heavily uses JavaScript](https://developer.apple.com/documentation/swiftui/), and Claude Code (or most AI agents to date) simply cannot parse that. It will fail and see nothing. So if you’re working with a component where documentation only exists on JavaScript-rendered pages, you’re completely stuck.

## Enter llm.codes

That’s when I built the docs converter. [llm.codes](https://llm.codes) allows you to point to documentation and fetch everything as clean Markdown. While it’s optimized for Apple documentation, it supports a wide range of developer documentation sites. Here’s what you get:

- **Your AI can finally see Apple docs** - No blind spots from JavaScript pages
- **70% smaller files** - More context space for your actual code
- **Works with 69+ sites** - AWS, Tailwind, PyTorch, PostgreSQL, and more

**Supported Documentation Sites**

**Mobile Development**

- Apple Developer Documentation
- Android Developer Documentation
- React Native
- Flutter
- Swift Package Index

**Programming Languages**

- Python, TypeScript, JavaScript (MDN), Rust, Go, Java, Ruby, PHP, Swift, Kotlin

**Web Frameworks**

- React, Vue.js, Angular, Next.js, Nuxt, Svelte, Django, Flask, Express.js, Laravel

**Cloud Platforms**

- AWS, Google Cloud, Azure, DigitalOcean, Heroku, Vercel, Netlify

**Databases**

- PostgreSQL, MongoDB, MySQL, Redis, Elasticsearch, Couchbase, Cassandra

**DevOps & Infrastructure**

- Docker, Kubernetes, Terraform, Ansible, GitHub, GitLab

**AI/ML Libraries**

- PyTorch, TensorFlow, Hugging Face, scikit-learn, LangChain, pandas, NumPy

**CSS Frameworks**

- Tailwind CSS, Bootstrap, Material-UI, Chakra UI, Bulma

**Build Tools & Testing**

- npm, webpack, Vite, pip, Cargo, Maven, Jest, Cypress, Playwright, pytest

**And more**: Any GitHub Pages site (\*.github.io)

llm.codes uses [Firecrawl](https://www.firecrawl.dev/referral?rid=9CG538BE) under the hood, and I pay for the credits to keep this service free for everyone.

## Real-World Example

Remember my toolbar problem? [Here’s what happened](https://x.com/steipete/status/1933819029224931619): I dragged the generated SwiftUI markdown from [my agent-rules repository](https://github.com/steipete/agent-rules/blob/main/docs/swiftui.md) into the terminal, and suddenly Claude wrote exactly the code I wanted.

The key insight: When you work on a component, just ask Claude to read the docs. It will load everything it needs into its context and produce vastly better code.

For people who think [@Context7](https://x.com/Context7AI) is the answer: if you use the context7 mcp for SwiftUI, you get [sample code from 2019](https://context7.com/ivanvorobei/swiftui), which will produce horribly outdated code. You need current documentation, not ancient examples.

## Beyond Just llm.codes

I used this trick before in my post about [migrating 700 tests to Swift Testing](https://steipete.me/posts/2025/migrating-700-tests-to-swift-testing). With llm.codes, you get significantly smaller markdown files, which preserves more token context space for your agent.

I also maintain a [collection of pre-converted Markdown documentation files](https://github.com/steipete/agent-rules/tree/main/docs) in my agent-rules repository, that go beyond just documentation.

The converter itself? Completely vibe-coded with Claude and [open source](https://github.com/amantus-ai/llm-codes). I chose the stack (Next.js, Tailwind, Vercel) but didn’t write a single line of TypeScript-and it worked beautifully on the first try.

## Try It Out

[Convert a page →](https://llm.codes?https://developer.apple.com/documentation/foundationmodels)

**No sign-up needed**.

AI agents are the future of coding. Until docs catch up, llm.codes is your bridge to that future.
