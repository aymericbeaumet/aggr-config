---
title: Agents don't need memory, they need documentation
link: https://liao.gg/blog/agents-dont-need-memory
source: hnrss-org
published: 2026-10-03T17:03:37Z
updated: 2026-10-03T17:03:37Z
first_seen: 2026-10-04T06:19:25.883170106Z
authors:
- kmeh
content: extracted
html: 2026-10-03-agents-don-t-need-memory-they-need-documentation.html
preview:
  file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.preview-76fe11150fdf.webp
  width: 256
  height: 192
  alt: A tangle of colorful wires
  color: '#c2bbb8'
images:
- source: https://liao.gg/images/blog/agents-dont-need-memory/tangled-1600.webp
  original:
    file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-0ede6b575c04.webp
    width: 1600
    height: 1200
  variants:
  - file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-3b04320b1d0b.webp
    width: 320
    height: 240
  color: '#e8e8e8'
- source: https://liao.gg/images/blog/agents-dont-need-memory/change-the-loop.png
  original:
    file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-7e29bacd2b04.png
    width: 3200
    height: 1280
  variants:
  - file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-b864a4b5062b.webp
    width: 320
    height: 128
  - file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-77bc1ca83f80.webp
    width: 640
    height: 256
  - file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-4fc5de9ada01.webp
    width: 960
    height: 384
  - file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-e733c78e566f.webp
    width: 1280
    height: 512
  - file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-209c1aa33a70.webp
    width: 1600
    height: 640
  - file: 2026-10-03-agents-don-t-need-memory-they-need-documentation.image-bba6fbaea914.webp
    width: 3200
    height: 1280
  color: '#f6f5ee'
---

![A tangle of colorful wires](https://liao.gg/images/blog/agents-dont-need-memory/tangled-1600.webp)

A memory plugin analyzes your conversations. It generates 1,000 isolated snippets and inserts them into a vector database. With your every prompt, it attaches the five most similar snippets; if the agent is confused (which it is), it manually searches for more. That’s the product they call “memory.”

It’s a strange thing to use when you think about what problem you’re trying to solve. You want your agent to understand your project. To know where a feature is, why it was built, what you agreed on and what you care about. Instead, you get a lottery over RAG snippets, injected on every prompt, hoping the right ones float up.

Even when it does work, the agent still doesn’t understand your project. The entire memory plugin ecosystem is solving the wrong problem.

**Because agents don’t need memory. They need documentation.**

## It’s All Just RAG

Every memory plugin on the market works the same way:

1. Go through session transcripts
2. Generate snippets of “memories”
3. Insert into a RAG database
4. On every prompt, retrieve the top 5 and inject
5. Need more? Give the agents a tool to search through the RAG database

That’s the whole architecture. Some tools are extra fancy; they let the agent search through past transcripts word for word. Or they implement some kind of multi-tier memory system that classifies short or long-term memory. Or they add a bunch of background daemons to review, merge, or deduplicate memories. “Dreamers” that rewrite memories overnight. Continuous context compression. Rerankers. Et cetera.

Each plugin tries to add new token-burning “features” to fix the same flawed architecture underneath. And that’s why none of them reliably work.

## The Problem with Recall

All of these memory plugins suffer from the same broad set of problems.

- **Memories are surfaced by similarity.** Similarity search ranks how close two snippets are in embedding space. That’s it. You don’t know which is correct, current, or what’s missing.
- **Memories are stored without context.** A RAG snippet can only contain so much. You lose everything else: context, motivations, lessons, environment, and more.
- **The past is treated as truth.** All of these plugins rely on recall; whether it’s search through transcripts or a vector database. But the codebase changes everyday; so how accurate are each of the 500 snippets about authentication?
- **Agents can’t search for what they don’t know.** Even if you do expose a search tool to the agent, how would the agent know when to use it? The agent doesn’t know what it doesn’t know.
- **The store is unauditable.** There are 10,000 embeddings in SQLite. Which memories exist? Which are stale? Which have never been retrieved? Which are incorrect and secretly affecting the way your agent works?

These are only five of the many problems that memory plugins face, attempt, and fail to fix, because they all make the same assumption:

> *Agents forget: that’s the problem. So the fix is to remember. To remember better, we should capture more, index better, retrieve smarter.*

Their entire thesis revolves around capturing and recalling the past. But that’s not how anyone else handles knowledge. No one rewatches a team meeting from 3 years ago to remember constraints around a feature. People write things down and use those records instead.

Similarly, the solution is *NOT* to give an agent a search tool that it uses to search over the past 10 million tokens worth of conversations, reconstructing fragments of what happens.

The solution, is document-based memory.

Today, people use AI to pump out products, features, and slop at the speed of light, all without reading or understanding a single line of code. Under these circumstances, it’s easy to see how documentation ends up becoming an afterthought when it should be more important than ever.

## Documentation Over Recall

People already knew agents needed context. That’s why they invented `AGENTS.md` files: so agents don’t jump into a codebase blind. It works. But oftentimes, that single file is the ONLY documentation that the project has.

A single file is not enough. The agent needs an entire brain, a structured workspace where it can record instructions, specs, decisions, research, indexes, anything without being asked. Instructions for how the code review process works, specs detailing what was discussed with the user, reusable research into a foreign library or API.

When the agent works, the agent can read files from the brain to gain relevant and complete context. After the agent works, it updates what’s outdated, adding new documents where necessary, while the full picture is still in context. In this way, the agentic loop changes from **prompt → build → forget** to **prompt → consult → build → update**. Memory turns from a RAG database you bolt onto the agent into a workspace you can read, update, and even share.

## Putting It to the Test

I recognized this problem over a year ago when I first started programming with AI. I wanted a way for an agent to remember work across sessions, so I started by creating an `internal/` folder where I asked the agent to write everything down: specs, plans, indexes. I tasked the agent with always reading the appropriate documents and index before doing work and updating afterwards.

This set of rudimentary instructions slowly transformed into a formal system, and then into a plugin called **Operator Memory** that I’ve been using regularly within all of my projects.

![Diagram comparing the prompt, build, forget loop with Operator Memory's prompt, consult, build, update loop](https://liao.gg/images/blog/agents-dont-need-memory/change-the-loop.png)

Operator Memory provides document-based memory using the model described above. Operator provides your agent with a Markdown brain where it can persist important knowledge: instructions, specs, research, indexes. Before working, the agent always consults the brain for relevant documents. After working, the agent updates the brain; revisiting stale documents, adding documents where missing.

No vector databases. No embeddings. No summarizers, curators, updaters, dreamers, or any other token-burning background daemon. No black-box retrieval.

With Operator, everything is a plain Markdown document that you can read, update, commit, and share with your team. I’ve been using this system for over a year. If you want to try it out, it’s free and open source: [https://github.com/aerovato/operator-memory](https://github.com/aerovato/operator-memory)
