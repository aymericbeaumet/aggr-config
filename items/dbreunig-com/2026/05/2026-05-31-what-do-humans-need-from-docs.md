---
title: What Do Humans Need From Docs?
link: https://www.dbreunig.com/2026/05/31/what-do-humans-need-from-docs.html
source: dbreunig-com
published: 2026-05-31T16:36:00Z
updated: 2026-09-15T14:59:45Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- development
- docs
- dspy
- skills
- writing
summary: When an agent can explain anything, should we keep writing docs for humans?
content: extracted
html: 2026-05-31-what-do-humans-need-from-docs.html
preview:
  file: 2026-05-31-what-do-humans-need-from-docs.preview-8c3dec3b07ab.webp
  width: 256
  height: 150
  color: '#8e999f'
images:
- source: https://www.dbreunig.com/img/textbooks.jpg
  original:
    file: 2026-05-31-what-do-humans-need-from-docs.image-710961e99816.jpg
    width: 2440
    height: 1430
  color: '#cde6f3'
- source: https://www.dbreunig.com/img/resized/textbooks.jpg
  original:
    file: 2026-05-31-what-do-humans-need-from-docs.image-bfda01996895.jpg
    width: 1600
    height: 937
  color: '#cde6f3'
extra:
  thumbnail: https://www.dbreunig.com/img/textbooks.jpg
---

### When an agent can explain anything, should we keep writing docs for humans?

![](https://www.dbreunig.com/img/resized/textbooks.jpg)

A few weeks ago [I posted](https://x.com/dbreunig/status/2052099660454776981), “No one wanted to write documentation, but everyone is writing skills.”

[Anthropic describes Skills](https://support.claude.com/en/articles/12512176-what-are-skills) as, “folders of instructions, scripts, and resources that \[an agent\] loads dynamically to improve performance on specialized tasks.” At first blush this sounds more complex than it is; in reality skills are stupidly simple. ~95% of the time they’re just Markdown, one file or a few, organized in folders that allow for progressive disclosure.

You know, kinda like docs.

* * *

### Why do people write skills, but not docs?

So why are people happily [churning out skills](https://www.skills.sh) after resisting doc writing for *decades*? I think there are a few reasons:

1. **The agent does most of the work.** Most skills are written by pointing an agent at a codebase, reference materials, or a chat session. Sometimes we don’t even read these files! A quick gloss over the summary is *fine* if it’s just for you.
2. **You can iterate as you use it.** The first draft of the skill will almost certainly be rough, but as you use it you can add hot fixes to correct poor instructions. Ask your agent to edit it in place.
3. **A skill is immediately valuable.** You might slave over docs to create an artifact that people *might* read, eventually. Maybe. Skills, on the other hand, deliver value the next session you need them.
4. **A skill doesn’t have to be aggressively edited.** At least at first. But when writing docs, editing and making sure your words effectively communicate is often a major blocker. Agents are more forgiving.

To be clear: there are *badly* written skills that unnecessarily [bloat and poison your context](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html). But on the whole, writing skills is much easier than writing documentation. And better yet: skills double as documentation.

This latter point reminds me of a comment [Simon Willison made](https://simonwillison.net/2025/May/25/claude-4-system-prompt/): “system prompts act as a sort of unofficial manual for how best to use \[AI\] tools.” Skills, which function as sort of a 3rd party system prompt, share this quality. Crack open a library or product’s skill and you’re often reading documentation that’s *better* than their website.

* * *

### Do we need docs that are just for humans?

If skills are great docs, is there still a need to write docs specifically for *humans* to read? If so, *what is the job of human-centric docs*?

I thought about this question a lot over the last few months, while sketching out [new docs for DSPy](https://dspy.ai/getting-started/program-dont-prompt/). After many conversations, I arrived at the following:

1. **Build mental models, not a reference.** Humans don’t require exhaustive documentation, they require *mental models*. Agents can find the details.
2. **Teach the art of the possible.** We need to arm them with an understanding of what a tool enables. Help them ask better questions and write better instructions.
3. **Explain the why.** Readers who understand a tool’s intent can reason beyond the obvious.
4. **Detail the design decisions.** What did the tool deliberately choose *not* to do or include? Great docs always shared this. It remains valuable.
5. **Don’t optimize for completeness.** This will be the hardest for most devs, who are excited to talk about the nuances. But only introduce what you need to build an effective mental model and trust the agent to facilitate the rest.

*The goal is to prepare your audience to prompt an agent effectively.*

* * *

### [`scaffold-docs`](https://github.com/dbreunig/scaffold-docs-skill): A skill for writing human-centric docs

As we iterated on the new [DSPy docs](https://dspy.ai/getting-started/program-dont-prompt/), I also iterated on a skill for writing human-centric docs. It’s called [`scaffold-docs`](https://github.com/dbreunig/scaffold-docs-skill), and [you can get it here](https://github.com/dbreunig/scaffold-docs-skill).

`scaffold-docs` is different than most documentation writing skills. To start, it is iterative and collaborative. At multiple points throughout the process, you will be prompted to review a markdown artifact and make edits and tweaks.

`scaffold-docs` is opinionated about organization, and rigidly adheres to a three-tier structure:

1. **Getting Started:** a narrative tutorial covering a single representative use case
2. **Diving Deeper:** one file per topic, organized around intent and design decisions
3. **Reference:** per-module API spec, lookup-oriented

Each section is built in passes: structure first, then headers and topic sentences, then full prose (a technique I shamelessly cribbed from [Matt Palmer](https://mattpalmer.io)). The agent pauses for your review between passes and does not advance until you approve.

`scaffold-docs` also prescribes style rules. The goal is to be clear, but not dry. Importantly, these rules have also been applied to the skill files themselves. During my initial search of skills, too often I’d find markdown files that didn’t follow the rules they espoused. These examples poison the context.

I encourage everyone to use `scaffold-docs` for their own projects, or to fork it entirely. It’s good, but not a free ride. Following the completion of the [DSPy docs](https://dspy.ai/getting-started/program-dont-prompt/), I’ve pointed it at several repos I’m familiar with and the output is excellent. You’ll still need to edit and guide the tool with your taste, but I’m happy with where it landed.
