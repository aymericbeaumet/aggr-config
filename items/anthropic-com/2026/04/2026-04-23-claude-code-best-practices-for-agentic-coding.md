---
title: 'Claude Code: Best practices for agentic coding'
link: https://www.anthropic.com/engineering/claude-code-best-practices
source: anthropic-com
published: 2026-04-23T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: As agents grow more capable, so does their potential blast radius. The engineering question is how to cap it. Here’s what we’ve learned building containment for claude.ai, Claude Code, and Cowork.
content: extracted
html: 2026-04-23-claude-code-best-practices-for-agentic-coding.html
preview:
  file: 2026-04-23-claude-code-best-practices-for-agentic-coding.preview-b4245e46bcc9.webp
  width: 256
  height: 256
  alt: How we contain Claude across products
  color: '#0b0b0b'
images:
- source: https://www-cdn.anthropic.com/images/4zrzovbb/website/47d14a71a7a759af39e1bc36ee68d65eb16ad74d-1000x1000.svg
  original:
    file: 2026-04-23-claude-code-best-practices-for-agentic-coding.image-18b67cc28c2e.png
    width: 1000
    height: 1000
  variants:
  - file: 2026-04-23-claude-code-best-practices-for-agentic-coding.image-bbfaa1c57ddd.webp
    width: 320
    height: 320
  - file: 2026-04-23-claude-code-best-practices-for-agentic-coding.image-ff9302f3e0df.webp
    width: 640
    height: 640
  - file: 2026-04-23-claude-code-best-practices-for-agentic-coding.image-ed4e43b87267.webp
    width: 1000
    height: 1000
  color: '#000000'
---

Claude Code is an agentic coding environment. Unlike a chatbot that answers questions and waits, Claude Code can read your files, run commands, make changes, and autonomously work through problems while you watch, redirect, or step away entirely. This changes how you work. Instead of writing code yourself and asking Claude to review it, you describe what you want and Claude figures out how to build it. Claude explores, plans, and implements. But this autonomy still comes with a learning curve. Claude works within certain constraints you need to understand. This guide covers patterns that have proven effective across Anthropic’s internal teams and for engineers using Claude Code across various codebases, languages, and environments. For how the agentic loop works, see [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works).

* * *

Most best practices are based on one constraint: Claude’s context window fills up fast, and performance degrades as it fills. Claude’s context window holds your entire conversation, including every message, every file Claude reads, and every command output. However, this can fill up fast. A single debugging session or codebase exploration might generate and consume tens of thousands of tokens. This matters since LLM performance degrades as context fills. When the context window is getting full, Claude may start “forgetting” earlier instructions or making more mistakes. The context window is the most important resource to manage. To see how a session fills up in practice, [watch an interactive walkthrough](https://code.claude.com/docs/en/context-window) of what loads at startup and what each file read costs. Track context usage continuously with a [custom status line](https://code.claude.com/docs/en/statusline), and see [Reduce token usage](https://code.claude.com/docs/en/costs#reduce-token-usage) for strategies on reducing token usage.

* * *

## Give Claude a way to verify its work

Claude stops when the work looks done. Without a check it can run, “looks done” is the only signal available, and you become the verification loop: every mistake waits for you to notice it. Give Claude something that produces a pass or fail, and the loop closes on its own. Claude does the work, runs the check, reads the result, and iterates until the check passes. The check is anything that returns a signal Claude can read in the conversation: a test suite, a build exit code, a linter, a script that diffs output against a fixture, or a [browser screenshot](https://code.claude.com/docs/en/chrome) compared against a design. Run [`/verify`](https://code.claude.com/docs/en/skills#run-and-verify-your-app) yourself after Claude’s check passes to confirm the change against the running app.

| Strategy                              | Before                                                  | After                                                                                                                                                                                                     |
| ------------------------------------- | ------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Provide verification criteria**     | *”implement a function that validates email addresses"* | *"write a validateEmail function. example test cases: [user@example.com](mailto:user@example.com) is true, invalid is false, [user@.com](mailto:user@.com) is false. run the tests after implementing”* |
| **Verify UI changes visually**        | *”make the dashboard look better"*                      | *"\[paste screenshot\] implement this design. take a screenshot of the result and compare it to the original. list differences and fix them”*                                                             |
| **Address root causes, not symptoms** | *”the build is failing"*                                | *"the build fails with this error: \[paste error\]. fix it and verify the build succeeds. address the root cause, don’t suppress the error”*                                                              |

Once the check exists, decide how hard it gates the stop:

- **In one prompt**: ask Claude to run the check and iterate in the same message, as in the table above.
- **Across a session**: set the check as a [`/goal` condition](https://code.claude.com/docs/en/goal). A separate evaluator re-checks it after every turn and Claude keeps working until the goal resolves. If Claude stalls, Claude Code eventually stops the run with the goal still set — see [how /goal evaluation works](https://code.claude.com/docs/en/goal#how-evaluation-works).
- **As a deterministic gate**: a [Stop hook](https://code.claude.com/docs/en/hooks#stop) runs your check as a script and blocks the turn from ending until it passes. [Stop input](https://code.claude.com/docs/en/hooks#stop-input) covers the cap on consecutive blocks.
- **By a second opinion**: a [verification subagent](https://code.claude.com/docs/en/sub-agents) or a [dynamic workflow](https://code.claude.com/docs/en/workflows) that checks its own findings has a fresh model try to refute the result, so the agent doing the work isn’t the one grading it.

Each step trades setup for attention. The prompt version works on any task today. The `/goal` and Stop hook versions are what let an unattended run finish correctly without you. Have Claude show evidence rather than asserting success: the test output, the command it ran and what it returned, or a screenshot of the result. Reviewing evidence is faster than re-running the verification yourself, and it works for sessions you weren’t watching.

* * *

## Explore first, then plan, then code

Letting Claude jump straight to coding can produce code that solves the wrong problem. Use [plan mode](https://code.claude.com/docs/en/permission-modes#analyze-before-you-edit-with-plan-mode) to separate exploration from execution. The recommended workflow has four phases:

1

2

3

4

* * *

## Provide specific context in your prompts

Claude can infer intent, but it can’t read your mind. Reference specific files, mention constraints, and point to example patterns.

| Strategy                                                                                         | Before                                               | After                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------ | ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Scope the task.** Specify which file, what scenario, and testing preferences.                  | *”add tests for foo.py"*                             | *"write a test for foo.py covering the edge case where the user is logged out. avoid mocks.”*                                                                                                                                                                                                                                                                    |
| **Point to sources.** Direct Claude to the source that can answer a question.                    | *”why does ExecutionFactory have such a weird api?"* | *"look through ExecutionFactory’s git history and summarize how its api came to be”*                                                                                                                                                                                                                                                                             |
| **Reference existing patterns.** Point Claude to patterns in your codebase.                      | *”add a calendar widget"*                            | *"look at how existing widgets are implemented on the home page to understand the patterns. HotDogWidget.php is a good example. follow the pattern to implement a new calendar widget that lets the user select a month and paginate forwards/backwards to pick a year. build from scratch without libraries other than the ones already used in the codebase.”* |
| **Describe the symptom.** Provide the symptom, the likely location, and what “fixed” looks like. | *”fix the login bug"*                                | *"users report that login fails after session timeout. check the auth flow in src/auth/, especially token refresh. write a failing test that reproduces the issue, then fix it”*                                                                                                                                                                                 |

Vague prompts can be useful when you’re exploring and can afford to course-correct. A prompt like `"what would you improve in this file?"` can surface things you wouldn’t have thought to ask about.

### Provide rich content

You can provide rich data to Claude in several ways:

- **Reference files with `@`** instead of describing where code lives. Claude reads the file before responding.
- **Paste images directly**. Copy/paste or drag and drop images into the prompt.
- **Give URLs** for documentation and API references. Use `/permissions` to allowlist frequently-used domains.
- **Pipe in data** by running `cat error.log | claude` to send file contents directly.
- **Let Claude fetch what it needs**. Tell Claude to pull context itself using Bash commands, MCP tools, or by reading files.

* * *

## Configure your environment

A few setup steps make Claude Code significantly more effective across all your sessions. For a full overview of extension features and when to use each one, see [Extend Claude Code](https://code.claude.com/docs/en/features-overview).

### Write an effective CLAUDE.md

CLAUDE.md is a special file that Claude reads at the start of every conversation. Include Bash commands, code style, and workflow rules. This gives Claude persistent context it can’t infer from code alone. There’s no required format for CLAUDE.md files, but keep it short and human-readable. For example:

CLAUDE.md

Run `/context` to confirm Claude loaded the file. CLAUDE.md is loaded every session, so only include things that apply broadly. For domain knowledge or workflows that are only relevant sometimes, use [skills](https://code.claude.com/docs/en/skills) instead. Claude loads them on demand without bloating every conversation. Keep it concise. For each line, ask: *“Would removing this cause Claude to make mistakes?”* If not, cut it. Bloated CLAUDE.md files cause Claude to ignore your actual instructions!

| ✅ Include                                            | ❌ Exclude                                          |
| ---------------------------------------------------- | -------------------------------------------------- |
| Bash commands Claude can’t guess                     | Anything Claude can figure out by reading code     |
| Code style rules that differ from defaults           | Standard language conventions Claude already knows |
| Testing instructions and preferred test runners      | Detailed API documentation (link to docs instead)  |
| Repository etiquette (branch naming, PR conventions) | Information that changes frequently                |
| Architectural decisions specific to your project     | Long explanations or tutorials                     |
| Developer environment quirks (required env vars)     | File-by-file descriptions of the codebase          |
| Common gotchas or non-obvious behaviors              | Self-evident practices like “write clean code”     |

If Claude keeps doing something you don’t want despite having a rule against it, the file is probably too long and the rule is getting lost. If Claude asks you questions that are answered in CLAUDE.md, the phrasing might be ambiguous. Treat CLAUDE.md like code: review it when things go wrong, prune it regularly, and test changes by observing whether Claude’s behavior actually shifts. For a checked-in CLAUDE.md, run [`/doctor`](https://code.claude.com/docs/en/commands#all-commands) and Claude proposes cuts for content it can derive from the codebase. If Claude keeps skipping one instruction, add emphasis such as “IMPORTANT” to that line alone. If you emphasize many lines, none of them stands out. Check CLAUDE.md into git so your team can contribute. The file compounds in value over time. CLAUDE.md files can import additional files using `@path/to/import` syntax. For import rules and where CLAUDE.md files can live, see [CLAUDE.md files](https://code.claude.com/docs/en/memory#claude-md-files).

### Configure permissions

With Claude Code v2.1.283 or later, auto mode is the [built-in starting permission mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) for interactive terminal and VS Code sessions: a separate classifier model reviews most actions instead of you and blocks only what looks risky, such as scope escalation, unknown infrastructure, or hostile-content-driven actions. On earlier versions, auto mode is the built-in starting permission mode only on Pro, Max, and Team plans. In Manual mode, Claude Code asks before actions that might modify your system: file writes, Bash commands, MCP tools. That’s safe but tedious. After the tenth approval you’re clicking through rather than reviewing. Two tools cut those interruptions in Manual mode and apply in auto mode as well:

- **Permission allowlists**: permit specific tools you know are safe, like `npm run lint` or `git commit`
- **Sandboxing**: enable OS-level isolation that restricts filesystem and network access, allowing Claude to work more freely within defined boundaries

Read more about [permission modes](https://code.claude.com/docs/en/permission-modes), [permission rules](https://code.claude.com/docs/en/permissions), and [sandboxing](https://code.claude.com/docs/en/sandboxing).

### Use CLI tools

CLI tools are the most context-efficient way to interact with external services. If you use GitHub, install the `gh` CLI. Claude knows how to use it for creating issues, opening pull requests, and reading comments. Without `gh`, Claude can still use the GitHub API, but unauthenticated requests often hit rate limits. Claude is also effective at learning CLI tools it doesn’t already know. Try prompts like `Use 'foo-cli-tool --help' to learn about foo tool, then use it to solve A, B, C.`

### Connect MCP servers

With [MCP servers](https://code.claude.com/docs/en/mcp), you can ask Claude to implement features from issue trackers, query databases, analyze monitoring data, integrate designs from Figma, and automate workflows.

### Set up hooks

[Hooks](https://code.claude.com/docs/en/hooks-guide) run scripts automatically at specific points in Claude’s workflow. Unlike CLAUDE.md instructions which are advisory, hooks are deterministic and guarantee the action happens. Claude can write hooks for you. Try prompts like *“Write a hook that runs eslint after every file edit”* or *“Write a hook that blocks writes to the migrations folder.”* Edit `.claude/settings.json` directly to configure hooks by hand, and run `/hooks` to browse what’s configured.

### Create skills

[Skills](https://code.claude.com/docs/en/skills) extend Claude’s knowledge with information specific to your project, team, or domain. Claude applies them automatically when relevant, or you can invoke them directly with `/skill-name`. Create a skill by adding a directory with a `SKILL.md` to `.claude/skills/`:

.claude/skills/api-conventions/SKILL.md

Skills can also define repeatable workflows you invoke directly:

.claude/skills/fix-issue/SKILL.md

Run `/fix-issue 1234` to invoke it. Use `disable-model-invocation: true` for workflows with side effects that you want to trigger manually.

### Create custom subagents

[Subagents](https://code.claude.com/docs/en/sub-agents) run in their own context with their own set of allowed tools. They’re useful for tasks that read many files or need specialized focus without cluttering your main conversation.

.claude/agents/security-reviewer.md

Tell Claude to use subagents explicitly: *“Use a subagent to review this code for security issues.”*

### Install plugins

[Plugins](https://code.claude.com/docs/en/plugins/overview) bundle skills, hooks, subagents, and MCP servers into a single installable unit from the community and Anthropic. If you work with a typed language, install a [code intelligence plugin](https://code.claude.com/docs/en/plugins/code-intelligence) to give Claude precise symbol navigation and automatic error detection after edits. For guidance on choosing between skills, subagents, hooks, and MCP, see [Extend Claude Code](https://code.claude.com/docs/en/features-overview#match-features-to-your-goal).

* * *

## Communicate effectively

Ask Claude the questions you’d ask another engineer, and for larger features have Claude interview you and write a spec before you start implementing.

### Ask codebase questions

When onboarding to a new codebase, use Claude Code for learning and exploration. You can ask Claude the same sorts of questions you would ask another engineer:

- How does logging work?
- How do I make a new API endpoint?
- What does `async move { ... }` do on line 134 of `foo.rs`?
- What edge cases does `CustomerOnboardingFlowImpl` handle?
- Why does this code call `foo()` instead of `bar()` on line 333?

Using Claude Code this way is an effective onboarding workflow, improving ramp-up time and reducing load on other engineers. No special prompting required: ask questions directly.

### Let Claude interview you

Claude asks about things you might not have considered yet, including technical implementation, UI/UX, edge cases, and tradeoffs. Replace `[brief description]` with your feature before sending the prompt.

Once the spec is complete, start a fresh session to execute it. The new session has clean context focused entirely on implementation, and you have a written spec to reference. The most useful specs are self-contained: they name the files and interfaces involved, state what is out of scope, and end with an end-to-end verification step that proves the feature works. Time spent making the spec precise pays off more than time spent watching the implementation.

* * *

## Manage your session

Conversations are persistent and reversible. Use this to your advantage!

### Course-correct early and often

The best results come from tight feedback loops. Though Claude occasionally solves problems perfectly on the first attempt, correcting it quickly generally produces better solutions faster.

- **`Esc`**: stop Claude mid-action with the `Esc` key. Context is preserved, so you can redirect.
- **`Esc + Esc` or `/rewind`**: press `Esc` twice or run `/rewind` to open the rewind menu and restore previous conversation and code state, or summarize from a selected message.
- **`"Undo that"`**: have Claude revert its changes.
- **`/clear`**: reset context between unrelated tasks. Long sessions with irrelevant context can reduce performance.

If you’ve corrected Claude more than twice on the same issue in one session, the context is cluttered with failed approaches. Run `/clear` and start fresh with a more specific prompt that incorporates what you learned. A clean session with a better prompt almost always outperforms a long session with accumulated corrections.

### Manage context aggressively

Claude Code automatically compacts conversation history when you approach context limits, which preserves important code and decisions while freeing space. During long sessions, Claude’s context window can fill with irrelevant conversation, file contents, and commands. This can reduce performance and sometimes distract Claude.

- Use `/clear` frequently between tasks to reset the context window entirely
- When auto compaction triggers, Claude summarizes what matters most, including code patterns, file states, and key decisions
- For more control, run `/compact <instructions>`, like `/compact Focus on the API changes`
- To compact only part of the conversation, use `Esc + Esc` or `/rewind`, select a message checkpoint, and choose **Summarize from here** or **Summarize up to here**. The first condenses messages from that point forward while keeping earlier context intact; the second condenses earlier messages while keeping recent ones in full. See [the rewind menu’s summarize options](https://code.claude.com/docs/en/checkpointing#rewind-and-summarize).
- Customize compaction behavior in CLAUDE.md with instructions like `"When compacting, always preserve the full list of modified files and any test commands"` to ensure critical context survives summarization
- For questions that don’t need to stay in context, use [`/btw`](https://code.claude.com/docs/en/interactive-mode#side-questions-with-%2Fbtw). The answer never enters conversation history, so you can check a detail without growing context.

### Use subagents for investigation

Since context is your fundamental constraint, use subagents to keep research out of it. When Claude researches a codebase it reads lots of files, all of which consume your context. Subagents run in separate context windows and report back summaries:

You can also use subagents for verification after Claude implements something. See [Add an adversarial review step](https://www.anthropic.com/engineering/claude-code-best-practices#add-an-adversarial-review-step).

### Rewind with checkpoints

Claude automatically snapshots files before each change so a checkpoint can restore them. Double-tap `Escape` or run `/rewind` to open the rewind menu. You can restore conversation only, restore code only, restore both, or summarize from a selected message. See [Checkpointing](https://code.claude.com/docs/en/checkpointing) for details. Instead of carefully planning every move, you can tell Claude to try something risky. If it doesn’t work, rewind and try a different approach. Checkpoints are saved with the conversation, so you can close your terminal, resume the session later, and still rewind.

### Resume conversations

Claude Code saves conversations locally, so when a task spans multiple sittings you don’t have to re-explain the context. Run [`claude --continue`](https://code.claude.com/docs/en/sessions#resume-a-session) to pick up where you left off, or `claude --resume` to choose from a list. Give sessions descriptive names like `oauth-migration` so you can find them later. See [Manage sessions](https://code.claude.com/docs/en/sessions) for the full set of resume, branch, and naming controls.

* * *

## Automate and scale

Once you’re effective with one Claude, multiply your output with parallel sessions, non-interactive mode, and fan-out patterns.

### Run non-interactive mode

With `claude -p "your prompt"`, you can run Claude non-interactively, without an interactive prompt. The run still creates a resumable session unless you pass `--no-session-persistence`. [Non-interactive mode](https://code.claude.com/docs/en/headless) is how you integrate Claude into CI pipelines, pre-commit hooks, or any automated workflow. The output formats let you parse results programmatically: plain text, JSON, or streaming JSON.

The first command prints plain text. The `json` format returns a single JSON object with a `result` field. The `stream-json` format prints one JSON object per line, starting with an init event.

### Run multiple Claude sessions

Pick the parallel approach that fits how much coordination you want to do yourself, and add messaging when the sessions need to pass findings between them:

- [Worktrees](https://code.claude.com/docs/en/worktrees): run separate CLI sessions in isolated git checkouts so edits don’t collide
- [Cross-session messaging](https://code.claude.com/docs/en/cross-session-messaging): let the sessions you run yourself pass findings to each other
- [Desktop app](https://code.claude.com/docs/en/desktop#work-in-parallel-with-sessions): manage multiple local sessions visually, optionally each in its own worktree
- [Use Claude Code in the cloud](https://code.claude.com/docs/en/claude-code-on-the-web): run sessions on Anthropic-managed infrastructure by default
- [Agent view](https://code.claude.com/docs/en/agent-view): research preview. Run `claude agents` to dispatch sessions that keep running in the background and watch them from one screen
- [Agent teams](https://code.claude.com/docs/en/agent-teams): experimental and disabled by default. Automated coordination of multiple sessions with shared tasks, messaging, and a team lead

Beyond parallelizing work, multiple sessions enable quality-focused workflows. A fresh context improves code review since Claude won’t be biased toward code it just wrote. For example, use a Writer/Reviewer pattern:

| Session A (Writer)                                                      | Session B (Reviewer)                                                                                                                                                     |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Implement a rate limiter for our API endpoints`                        |                                                                                                                                                                          |
|                                                                         | `Review the rate limiter implementation in @src/middleware/rateLimiter.ts. Look for edge cases, race conditions, and consistency with our existing middleware patterns.` |
| `Here's the review feedback: [Session B output]. Address these issues.` |                                                                                                                                                                          |

You can do something similar with tests: have one Claude write tests, then another write code to pass them.

### Fan out across files

For large migrations or analyses, you can distribute work across many parallel Claude invocations. Run [`/batch <instruction>`](https://code.claude.com/docs/en/commands#all-commands) to have Claude split the change across 5 to 30 subagents. Each subagent works in its own worktree. To drive the fan-out from your own script instead, loop over `claude -p`:

1

2

3

You can also integrate Claude into existing data/processing pipelines:

### Run autonomously with auto mode

For uninterrupted execution with background safety checks, use [auto mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode). A classifier model reviews commands before they run, blocking scope escalation, unknown infrastructure, and hostile-content-driven actions while letting routine work proceed without prompts.

When the classifier repeatedly blocks actions in a non-interactive run with the `-p` flag, Claude Code doesn’t stop the run. See [when auto mode falls back](https://code.claude.com/docs/en/permission-modes#when-auto-mode-falls-back) for what happens instead and for the thresholds.

### Add an adversarial review step

The longer Claude works unattended, the more an independent check matters before you count the work as done. A reviewer running in a fresh [subagent](https://code.claude.com/docs/en/sub-agents) context sees only the diff and the criteria you give it, not the reasoning that produced the change, so it evaluates the result on its own terms. For a correctness check, run the bundled [`/code-review` skill](https://code.claude.com/docs/en/commands), which reviews the current diff for bugs in a fresh subagent and returns findings to the session. To check the diff against your plan instead, write the review prompt yourself. Name the work to check, the plan to check it against, and what counts as a finding:

Because the reviewer runs as a subagent, the implementing session receives the gaps directly and can fix them and re-review without you copying findings between windows.

* * *

## Avoid common failure patterns

These are common mistakes. Recognizing them early saves time:

- **The kitchen sink session.** You start with one task, then ask Claude something unrelated, then go back to the first task. Context is full of irrelevant information.

  > **Fix**: `/clear` between unrelated tasks.

- **Correcting over and over.** Claude does something wrong, you correct it, it’s still wrong, you correct again. Context is polluted with failed approaches.

  > **Fix**: After two failed corrections, `/clear` and write a better initial prompt incorporating what you learned.

- **The over-specified CLAUDE.md.** If your CLAUDE.md is too long, Claude ignores half of it because important rules get lost in the noise.

  > **Fix**: Ruthlessly prune. If Claude already does something correctly without the instruction, delete it or convert it to a hook.

- **The trust-then-verify gap.** Claude produces a plausible-looking implementation that doesn’t handle edge cases.

  > **Fix**: Always provide verification (tests, scripts, screenshots). If you can’t verify it, don’t ship it.

- **The infinite exploration.** You ask Claude to “investigate” something without scoping it. Claude reads hundreds of files, filling the context.

  > **Fix**: Scope investigations narrowly or use subagents so the exploration doesn’t consume your main context.

* * *

## Develop your intuition

The patterns in this guide aren’t set in stone. They’re starting points that work well in general, but might not be optimal for every situation. Sometimes you *should* let context accumulate because you’re deep in one complex problem and the history is valuable. Sometimes you should skip planning and let Claude figure it out because the task is exploratory. Sometimes a vague prompt is exactly right because you want to see how Claude interprets the problem before constraining it. Pay attention to what works. When Claude produces great output, notice what you did: the prompt structure, the context you provided, the mode you were in. When Claude struggles, ask why. Was the context too noisy? The prompt too vague? The task too big for one pass? Over time, you’ll develop intuition that no guide can capture. You’ll know when to be specific and when to be open-ended, when to plan and when to explore, when to clear context and when to let it accumulate.

- [How Claude Code works](https://code.claude.com/docs/en/how-claude-code-works): the agentic loop, tools, and context management
- [Extend Claude Code](https://code.claude.com/docs/en/features-overview): skills, hooks, MCP, subagents, and plugins
- [Common workflows](https://code.claude.com/docs/en/common-workflows): step-by-step recipes for debugging, testing, PRs, and more
- [CLAUDE.md](https://code.claude.com/docs/en/memory): store project conventions and persistent context
