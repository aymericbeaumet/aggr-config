---
title: How to Work and Compound with AI
link: https://eugeneyan.com//writing/working-with-ai/
source: eugeneyan-com
published: 2026-05-03T00:00:00Z
updated: 2026-05-03T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- ai
- mechanism
- productivity
summary: Context as infra, taste as config, verification for autonomy, scale via delegation, closing the loop.
content: extracted
html: 2026-05-03-how-to-work-and-compound-with-ai.html
preview:
  file: 2026-05-03-how-to-work-and-compound-with-ai.preview-c9d35c5871cf.webp
  width: 256
  height: 134
  color: '#f9fafa'
images:
- source: https://eugeneyan.com/assets/og_image/working-with-ai.jpg
  original:
    file: 2026-05-03-how-to-work-and-compound-with-ai.image-71fa5df94692.jpg
    width: 1200
    height: 630
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2026-05-03-how-to-work-and-compound-with-ai.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2026-05-03-how-to-work-and-compound-with-ai.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2026-05-03-how-to-work-and-compound-with-ai.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2026-05-03-how-to-work-and-compound-with-ai.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2026-05-03-how-to-work-and-compound-with-ai.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2026-05-03-how-to-work-and-compound-with-ai.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

> Translations: [Korean](https://damoang.net/ai/3302) by DG Hong

How can we work effectively with AI? What’s the workflow, how does it scale, and how do we improve our systems over time? And ideally, it should compound. Every finished artifact—code, docs, analysis, decisions—becomes context for the next session. And each correction updates a config that reduces future errors. While I’m still learning, I’ve repeated my answers often enough that I’m writing it here so the next time I’m asked I can share a link instead.

If you use AI regularly, you likely already apply many of these practices. Nonetheless, I believe the underlying principles apply broadly: provide good context, encode your taste as config, make verification easy, delegate bigger tasks, and close the loop. If a practice does not fit, adapt the principle and invent your own. Also notice, as you read, that none of this is specific to AI. It’s simply how you onboard and work with any new collaborator.

## Context as infrastructure

**Help models nagivate your context.** For example, all my code lives in `~/src` and all my knowledge work lives in `~/vault` (organized into `projects/`, `notes/`, `kb/` and so on). When our work is organized, it makes it easier for the model to retrieve context using `grep` or `glob`. And by having a clean directory tree, it’s more straightforward to navigate the directory, and find and lean on prior code, project docs, analysis, etc. to improve the work being done.

**Connect models to your organization’s context.** Models can benefit from organizational knowledge which likely lives in Slack, Drive, Mail, etc. Most have [MCPs](https://modelcontextprotocol.io/docs/getting-started/intro) for Claude Code, Cowork, Claude.ai. On top of these, I also maintain a `INDEX.md` per project. It’s an annotated index of the relevant docs and channels, and each entry includes the URL, owner, and a brief paragraph explaining what’s inside and when to read it. The annotation helps a lot. A bare list of URLs forces the model to open every link to figure out what’s relevant, wasting time and context. By annotating upfront, we do the heavy lifting once and store it in the index.

**Onboard each new session like a new hire.** With each new session, the model starts with a blank slate. Thus, it helps to treat the per-project `CLAUDE.md` like the onboarding doc we’d hand to a new teammate on day one. Claude scanned my per-project `CLAUDE.md` files and highlighted that they included glossaries for acronyms, project code names, and teammates with the same first name. I also have a suggested reading order in the `CLAUDE.md`, like telling the model to skim `INDEX.md` first, then `TODOS.md`, and finally specific topic notes.

**Build your memory layer.** By default, models don’t remember what happened in the last session, so anything worth persisting should be written to disk. I split my memory layer into two buckets. `~/vault` holds facts such as project state, artifacts, and domain knowledge; `~/.claude` (along with its `CLAUDE.md`, `skills/`, `guides/`) contains my preferences, workflows, and personal taste. The former provides context while the latter provides configuration.

## Taste as configuration

**Start with `~/.claude/CLAUDE.md`.** Claude reads this at the start of every session. I think of it as a behavioral contract. My `CLAUDE.md` contains preferences like how direct to be, when to push back, how to handle mistakes, what to teach me, etc. Here’s a trimmed version:

```xml
<behavior>
- Be direct and push back when you disagree; if my approach has problems, say so.
- When unsure about something, say you're unsure rather than guessing confidently.
- When something fails, investigate the root cause before retrying.
- Keep diffs scoped to the task: no drive-by reformats or unrelated refactors.
...
</behavior>

<teaching>
I'm always picking up new systems and domains. When a key term surfaces that I 
likely haven't internalized, explain it in 1-2 sentences and then move on. Format:

> 💡 followed by 1 - 2 sentence explanation
...
</teaching>
```

**Scope it by directory: global, then repo, then project.** Put preferences that apply everywhere (e.g., behavior, long-term goals, teaching) in `~/.claude/CLAUDE.md`. Put conventions for a specific a repo (e.g., linting, naming, pull requests) in the repo’s root. Put project-specific context (i.e., directory layout, domain knowledge) in the project directory. When you start Claude Code in a subdirectory, it walks up the tree and loads each `CLAUDE.md`. And when the model navigates into a subdirectory mid-session, the model picks up that directory’s `CLAUDE.md` too. More in the [docs](https://code.claude.com/docs/en/memory#how-claude-md-files-load).

**When CLAUDE.md gets too long, split it out.** A long `CLAUDE.md` can become a context tax. It loads everything every session even if the session doesn’t need it. To fix this, refactor chunks into guides that load lazily. Don’t `@import` them (because that just inlines them). Instead, tell your `CLAUDE.md` to read them when relevant. This way, a session that’s building evals skips the guide on writing docs. Here’s an example guide section:

```xml
<guides>
- Docs, 1-pagers, any writing: ~/.claude/guides/writing.md
- Eval building and reports: ~/.claude/guides/evals.md
- Dashboards: ~/.claude/guides/dashboards.md
...
</guides>
```

**If you do something ≥ once a week, make it a skill.** A skill is a markdown file with a name, trigger, and procedure that the model loads on demand. Think of skills as workflows written in markdown. They can include logic. For example, my `/polish` skill looks at the artifact diff. If it produces a metric, it runs the associated eval. If it renders in a browser, it checks the output via Claude in Chrome. If neither, it runs the code and reads the output or error. Skills encode both the steps and the judgment of which steps apply. A few I have include:

- `/polish`: checks for bugs, simplifies the code, verify the output (via evals, Claude in Chrome, or something else), iterate until no critical feedback, draft the PR
- `/write`: interviews me for the outline, spawn research subagents, writes the draft, gives feedback via adversarial critic, iterate until no critical feedback
- `/daily`: reads my calendar, slack, PRs, yesterday’s log, etc and writes today’s priorities

I tend to keep `SKILL.md` small and focused on the workflow and routing. The knowledge, like templates and scripts, are separate files that the model reads and runs only when needed, just like lazy-loaded guides.

**Bootstrap skills by doing the task once and then asking the model to make it a skill.** This is how I build most skills. First, I do the task once, interactively, in a normal session. Then, I ask the model to turn what we just did into a skill. Next, I run the skill on the same or similar task. Inevitably, I’ll need to correct the output, which I do in the same session so feedback is logged in the session transcript. Finally, I ask the model to update the skill based on the corrections and feedback. You can also seed a skill with exapmles of the desired output. Ask the model to extract the patterns, like how you organize your code, or the structure and tone of your docs.

**Refine skills via the transcript, not the file directly.** The first version of the skill rarely works perfect because it overfits the original session. This is normal. When you run it and need to update the output, correct it within the session. Try not to open and edit `SKILL.md` directly. Providing feedback in the session gives the model before-and-after pairs which accumulate in the transcript—here’s what we did, here’s what I wanted, and why. Once the output is right, ask the model to merge the feedback into the skill. After a few rounds, the skill converges and you barely have to edit the final output.

**Nonetheless, not every task needs this context.** For brainstorming, exploration, and rough drafts, I enjoy using simple mode (`CLAUDE_CODE_SIMPLE=1 claude`). Here, `CLAUDE.md` still loads but the agentic harness—hooks, skills, tool-heavy loops—doesn’t. This gets me closer to the model, which is what I want when I’m thinking out loud rather than shipping.

## Verification for autonomy

**Shift verification left; catch errors at write time.** I think of verification as a ladder. The bottom is cheap and deterministic; the top is expensive and requires judgement. We want to address issues at the lowest possible rung. Near the bottom are post-edit hooks that run `ruff format`, `ruff check --fix` on files the model just updated. This happens deterministically and doesn’t cost tokens. Higher on the ladder are tests, evals, LLM reviews, etc.

**Make it easy for the model to verify the work.** Give the model feedback loops to improve its output. If the system produces a metric, let the model run the eval and optimize it. If the output renders in a browser, let the model inspect it via Claude in Chrome. If neither, let the model run it and read the error. For example, when building Docker images, I let the model build, read the error, edit the Dockerfile, and rebuild. If I’m tuning a harness, the model runs evals, reads the transcripts, and fixes failures. When building a dashboard, the model checks in Chrome that tooltips render, labels don’t overlap, and the narrative matches the numbers.

**For long-running tasks, have models watch models.** Long sessions can drift as errors build up. One fix is to run a secondary session with fresh context to read the original spec and the recent turns of the primary session. My minimal setup uses two tmux panes, one for the primary dev, one for the pair programmer. Initial instructions and follow-up prompts are appended to a shared file. Periodically, the pair programmer spins up, checks the spec against the primary’s recent transcript, and if something’s off, provides feedback to course correct.

We can do this in various ways. For example, the pair programmer can watch for execution drift—is the model doing the task right? This is local and tactical, like ignoring an error, reporting a bad metric, or diverging from the spec. There’s also direction drift—is the model doing the right task? These are bigger picture and strategic, and occur when the model misinterprets the original intent and spends hours building the wrong thing. Check for execution drift often and direction drift occasionally.

## Scaling via delegation

**Delegate increasingly bigger chunks of work.** Sometimes, we pair-program with models: short tasks, fast feedback, staying in the loop. This well works for fast iterations, exploratory analysis, and prototyping. But with increasingly stronger models, we should aim to delegate bigger tasks. Explain your intent, constraints, and success criteria upfront, then let the model work. You can’t delegate what you can’t verify, so this requires first defining success criteria and metrics. The shift is from giving instructions, one at a time, to fleshing out plans and letting the model execute them end to end:

> “Given these eval suites, build isolated containers per suite and smoke-test that each builds. Then, do the full run, log the eval metrics and transcripts, and use subagents to read the transcripts and confirm the evals ran correctly. Run each eval n times for confidence intervals. Finally, generate the report, verify it follows the report guide, and slack me the results and report URL.”

**Run sessions in parallel and find the bottleneck.** Delegating bigger tasks means we can run more at once. Claude says I typically run three to six sessions simultaneously. The bottleneck has shifted from doing the work to writing clear specs and reviewing outputs fast enough to keep the pipeline moving—the middle is hollowing out. If parallel sessions share a repo, use git worktrees so each session gets its own checkout and don’t overwrite each other’s changes.

**Make sessions easy to observe.** When running multiple sessions, I need to know their state and which one needs attention. On my mac, a stop hook plays a sound when a session finishes (example below). My tmux window titles use a status emoji (⏳ working; 🟢 complete) and a short Haiku-generated label so I know what each pane is doing. The Claude Code status line shows context usage and the current mode. Together, the stop-hook sound signals a finished task, the tmux titles shows which one, and the status line provides the details.

```plaintext
# Example stop hook alert
"Stop": [
    {
    "hooks": [
        {
        "type": "command",
        "command": "if command -v afplay >/dev/null 2>&1; then afplay -v 1.0 /System/Library/Sounds/Glass.aiff; else tput bel; fi"
        }
    ]
}
```

**You can check in even if AFK.** `/remote-control` in Claude Code makes this easy. While commuting or waiting in line, I open the code tab in the Claude app to see what’s running and what’s blocked, and if needed, unblock a stalled session with additional context or new instructions. This keeps sessions moving instead of sitting idle for hours. Only do this if there’s something urgent though, not when you’re trying to be present or touch grass.

## Closing the loop

**Keep the context rich by working in the open.** When we do our work in shared docs, repos, and channels, it makes it easier for everyone—including models—to retrieve and benefit from the context. What we share today becomes part of the org context tomorrow. Try this simple test: could a new teammate replicate your work from last week using only the shared context? If yes, you’re contributing well to the org context; if not, that precious context is stuck in your head. I automate this somewhat via instructions in my `CLAUDE.md` to post short updates in a worklog channel whenever I finish a substantial task, with links to the artifact PR or doc.

**Mine your transcripts for config updates.** Have the model read past session transcripts to find gaps. When I scanned ~2,500 of my past user turns, a sizable percentage contained phrases like *“can you also…“*, *“did you check…“*, *“still wrong”*, etc. These suggest that the model should have done something unprompted, and I should update the `CLAUDE.md` or skill, or that a verification step is missing or broken. Hit counts show how often a correction happens and the transcripts show exactly what failed. This is why I make corrections within the session, so I can use the transcript as input for my next `CLAUDE.md` or skills update.

**Refactor and prune periodically.** As configs grow, they can overlap or conflict with each other. As a result, if the model ignores a rule, it can be because another rule contradicts it. Fix this by refactoring periodically. Each rule or preference should live in exactly one place (though critical instructions can be repeated in the main `CLAUDE.md`). I also check for stray directory-level `settings.json` and consolidate them back into `~/.claude`.

While the specific setup will likely change as models get better, I think the principles will remain relevant: provide good context, encode your taste, make verification cheap, delegate more, and close the loop. What we’re doing is training a collaborator, one feedback at a time. And if you think about it, these principles apply to how we work with a human team too.

To get started, have your model read this [SETUP.txt](https://eugeneyan.com/assets/SETUP.txt) and help you apply it. Also, I’d love to learn what practices or principles you’ve found valuable—please comment below or [reach out!](https://x.com/eugeneyan)

p.s. This isn’t *just* about personal tooling. It’s also how you’d design agent harnesses, set team norms, and build org infrastructure. Try reading it again with those layers in mind.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (May 2026). How to Work and Compound with AI. eugeneyan.com. https://eugeneyan.com/writing/working-with-ai/.

or

```
@article{yan2026default,
  title   = {How to Work and Compound with AI},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2026},
  month   = {May},
  url     = {https://eugeneyan.com/writing/working-with-ai/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
