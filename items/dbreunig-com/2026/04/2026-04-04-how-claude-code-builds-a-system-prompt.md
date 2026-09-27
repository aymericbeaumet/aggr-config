---
title: How Claude Code Builds a System Prompt
link: https://www.dbreunig.com/2026/04/04/how-claude-code-builds-a-system-prompt.html
source: dbreunig-com
published: 2026-04-04T22:14:00Z
updated: 2026-09-15T15:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- context engineering
- system prompts
summary: Claude Code's leaked source shows a system prompt assembled from dozens of conditional parts.
content: extracted
html: 2026-04-04-how-claude-code-builds-a-system-prompt.html
preview:
  file: 2026-04-04-how-claude-code-builds-a-system-prompt.preview-d97b425e75c2.webp
  width: 256
  height: 134
  color: '#7a7062'
images:
- source: https://www.dbreunig.com/img/sf_beach_og.jpg
  original:
    file: 2026-04-04-how-claude-code-builds-a-system-prompt.image-246e578187ce.jpg
    width: 1200
    height: 630
  color: '#1a130a'
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

I like reading [system prompts](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools), either when they’re published as part of open-source software, exfiltrated via crafty prompting, [explicitly shared](https://platform.claude.com/docs/en/release-notes/system-prompts), or (in the case of last week) accidentally leaked. They’re often the best manual for how an app is intended to work.

We’ve touched on system prompts in the past, [introducing them and breaking down Claude’s](https://www.dbreunig.com/2025/05/07/claude-s-system-prompt-chatbots-are-more-than-just-models.html), [showing how system prompt changes over time reveal product priorities](https://www.dbreunig.com/2025/06/03/comparing-system-prompts-across-claude-versions.html), and [diving deep with an analysis of coding agent prompts and variations](https://www.dbreunig.com/2026/02/10/system-prompts-define-the-agent-as-much-as-the-model.html).

But one thing that’s been hard is understanding *how system prompts are assembled*. System prompts generally aren’t static strings; they’re dynamically assembled contexts with many conditional statements determining what makes it in the prompt. It’s true, we can look at open source harnesses or apps to understand approaches. But for the big company apps we can only get the big picture. We can extract a final prompt, but we can’t see how it was built.

With the [accidental leak of Claude Code’s source code last week](https://read.engineerscodex.com/p/diving-into-claude-codes-source-code), we can see for the first time how Claude Code assembles a context. It’s incredibly impressive, illustrating how complex context engineering can be and the importance of harnesses.

I won’t share the code here, but after poring over it I’ve assembled a visualization below. It lists each component used to assemble the system prompt. Some components are always included (the rows with a solid blue dot) while others are conditional (the hollow blue dots). Components may have variations. For example, the “Using Your Tools” section only contains information regarding available tools.

Take a look yourself. Click a row for more details.

System Prompt

Tells the model what it is and sets the session tone.

Default

You are an interactive agent that helps users with software engineering tasks. Use the instructions below and the tools available to you to assist the user.

With output style

You are an interactive agent that helps users according to your "Output Style" below, which describes how you should respond to user queries.

Ground rules for how tools, permissions, prompt injection, system-reminder tags, and context compression work.

All text you output outside of tool use is displayed to the user. Output text to communicate with the user. You can use Github-flavored markdown for formatting, and will be rendered in a monospace font using the CommonMark specification.

Coding philosophy: read before editing, don't over-engineer, keep changes minimal, don't introduce security vulnerabilities.

Omitted when

A custom output style is configured with keepCodingInstructions: false.

Default

Don't add features, refactor code, or make "improvements" beyond what was asked. A bug fix doesn't need surrounding code cleaned up. A simple feature doesn't need extra configurability.

Anthropic extra

Default to writing no comments. Only add one when the WHY is non-obvious: a hidden constraint, a subtle invariant, a workaround for a specific bug, behavior that would surprise a reader.

Guidelines for confirming before risky actions: deleting files, force-pushing, posting to external services.

Carefully consider the reversibility and blast radius of actions. Generally you can freely take local, reversible actions like editing files or running tests. But for actions that are hard to reverse, affect shared systems beyond your local environment, or could otherwise be risky or destructive, check with the user before proceeding.

Tells the model to prefer dedicated tools (Read, Edit, Glob, Grep) over raw shell commands.

Default

Do NOT use the Bash to run commands when a relevant dedicated tool is provided. Using dedicated tools allows the user to better understand and review your work.

REPL

Minimal version with only the task management bullet.

Communication rules: no emojis, cite file paths, use GitHub link format.

Only use emojis if the user explicitly requests it. Avoid using emojis in all communication unless asked.

External extra

Your responses should be short and concise.

How verbose to be. Two very different versions for internal vs. external users.

Anthropic

When sending user-facing text, you're writing for a person, not logging to a console. Assume users can't see most tool calls or thinking - only your text output.

External

IMPORTANT: Go straight to the point. Try the simplest approach first without going in circles. Do not overdo it. Be extra concise.

A marker separating globally-cacheable content from session-specific content. Not visible to the model — it's a cache optimization.

Included when

If global cache scope is enabled

\_\_SYSTEM\_PROMPT\_DYNAMIC\_BOUNDARY\_\_

Tells the model it can ask you a clarifying question if you deny a tool call.

Included when

If the AskUserQuestion tool is available

If you do not understand why the user has denied a tool call, use the AskUserQuestion to ask them.

Tells the model about the ! prefix for running interactive commands yourself.

Omitted when

Running in a non-interactive session (CI, SDK, headless).

If you need the user to run a shell command themselves (e.g., an interactive login like \`gcloud auth login\`), suggest they type \`! \<command>\` in the prompt.

How to use sub-agents for parallel work or deep research.

Included when

If the Agent tool is enabled

Fork

Calling Agent without a subagent\_type creates a fork, which runs in the background and keeps its tool output out of your context — so you can keep chatting with the user while it works.

Standard

Use the Agent tool with specialized agents when the task at hand matches the agent's description. Subagents are valuable for parallelizing independent queries or for protecting the main context window from excessive results.

When to use Glob/Grep directly vs. spawning an Explore agent for deep research.

Included when

If the Agent tool is enabled

If Explore/Plan agents are enabled

Omitted when

Fork sub-agent mode is enabled (replaces this guidance).

For broader codebase exploration and deep research, use the Agent tool with subagent\_type=Explore. This is slower than using Glob or Grep directly, so use this only when a simple, directed search proves to be insufficient.

Tells the model that /skill-name invokes skills.

Included when

If skills are available and the Skill tool is enabled

/\<skill-name> (e.g., /commit) is shorthand for users to invoke a user-invocable skill. When executed, the skill gets expanded to a full prompt. Use the Skill tool to execute them.

Tells the model that relevant skills are auto-surfaced each turn and it can search for more.

Included when

If experimental skill search is enabled

If skills are available and the Skill tool is enabled

Relevant skills are automatically surfaced each turn as "Skills relevant to your task:" reminders. If you're about to do something those don't cover — a mid-task pivot, an unusual workflow — call DiscoverSkills with a specific description of what you're doing.

Requires spawning an independent verifier before claiming complex work (3+ file edits) is done.

Included when

If the verification agent is enabled

If the Agent tool is enabled

When non-trivial implementation happens on your turn, independent adversarial verification must happen before you report completion — regardless of who did the implementing. Non-trivial means: 3+ file edits, backend/API changes, or infrastructure changes.

Instructions for the auto-memory system: how to read, write, and organize persistent memories across sessions.

Included when

If auto-memory is configured

You have a persistent, file-based memory system. You should build up this memory system over time so that future conversations can have a complete picture.

Internal Anthropic model behavior overrides.

Included when

If the user is an Anthropic employee

Omitted when

Undercover mode is active.

Config-driven suffix appended to system prompt (content varies by internal config).

Working directory, platform, shell, model name, knowledge cutoff date, and product availability.

Working directory: /Users/me/project Is a git repository: true Platform: darwin Shell: zsh OS Version: Darwin 24.6.0 You are powered by the model named Claude Opus 4.6.

Undercover

Model name, model IDs, Claude Code availability, and fast mode info are all suppressed.

Worktree

Adds: This is a git worktree — an isolated copy of the repository. Run all commands from this directory.

Instructs the model to respond in the user's preferred language.

Included when

If a language preference is set

Always respond in Japanese. Use Japanese for all explanations, comments, and communications with the user. Technical terms and code identifiers should remain in their original form.

The user's custom output style instructions.

Included when

If a custom output style is configured

\# Output Style: Concise Keep responses under 3 sentences unless the task requires more detail.

Per-server instructions from connected MCP servers. Recomputed every turn (not cached).

Included when

If MCP servers are connected with instructions

Omitted when

MCP instruction delta mode is enabled (delivered as per-turn attachment instead).

\# MCP Server Instructions ## my-database-server Use the query tool for read operations. Always include a LIMIT clause.

Tells the model to use a session-specific temp directory instead of /tmp.

Included when

If the scratchpad is enabled

IMPORTANT: Always use this scratchpad directory for temporary files instead of \`/tmp\` or other system temp directories: \`/Users/me/.claude/scratchpad/session-abc123\`

Warns that old tool results will be automatically removed from context.

Included when

If function result clearing is enabled

Old tool results will be automatically cleared from context to free up space. The 5 most recent results are always kept.

Tells the model to write down important info since originals may be cleared.

When working with tool results, write down any important information you might need later in your response, as the original tool result may be cleared later.

Hard word-count limits for responses.

Included when

If the user is an Anthropic employee

Length limits: keep text between tool calls to ≤25 words. Keep final responses to ≤100 words unless the task requires more detail.

Instructions for working toward a user-specified token spending target.

Included when

If the token budget feature is enabled

When the user specifies a token target (e.g., "+500k", "spend 2M tokens", "use 1B tokens"), your output token count will be shown each turn. Keep working until you approach the target — plan your work to fill it productively.

Instructions for using the Brief tool for short replies vs. detailed output.

Included when

If Brief/KAIROS mode is enabled

Omitted when

Proactive mode is active (brief instructions are already in the proactive section).

Brief is where your replies go. Text outside of Brief is in the detail view and mostly goes unread.

Current branch, recent commits, and working tree status. Appended as system context.

Included when

If the working directory is a git repository

If not running in a remote session

If git instructions are enabled

This is the git status at the start of the conversation. Note that this status is a snapshot in time, and will not update during the conversation. Current branch: feature-auth Main branch (you will usually use this for PRs): main Git user: Jane Developer Status: M src/auth.ts ?? src/auth.test.ts Recent commits: abc1234 Fix login redirect def5678 Add session middleware

Extra text provided via --append-system-prompt, added at the very end.

Included when

If --append-system-prompt flag is used

Whatever text the user passed to --append-system-prompt.

And this is just the system prompt! There’s similar logic to assemble for…

- **Tool definitions:** There’s ~50 tools to manage descriptions for (not including MCPs!) and many tools have several conditions for if and how they make it into the context.
- **User content:** CLAUDE.md or AGENT.md files, user provided instructions.
- **Conversation history:** All the messages you’ve previously sent, the reasoning and tool calls the agent has produced, and more. All managed by about a dozen different methods for compaction, offloading, and summarizing the conversation so far.
- **Attachments:** Additional items appended to user messages that specify specific behaviors (Are we still in plan mode? Are there tasks left on our list?) or user specified parameters like @-mentioned files, MCPs, agents, or skills.
- **Skills:** Finally, any relevant or user-specified skills are appended.

When you type instructions and hit ‘Enter’, Claude Code assembles a rich context to increase the odds that it obtains a successful response from Opus or Sonnet. As we can see, [agents are more than just models](https://www.dbreunig.com/2026/02/10/system-prompts-define-the-agent-as-much-as-the-model.html). Context engineering is critical.
