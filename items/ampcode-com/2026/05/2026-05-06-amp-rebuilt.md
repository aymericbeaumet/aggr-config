---
title: Amp, Rebuilt
link: https://ampcode.com/news/neo
source: ampcode-com
published: 2026-05-06T00:00:00Z
updated: 2026-05-06T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Today we''re starting to roll out the new Amp. Not all of it, not yet. But the first piece: a rebuilt Amp CLI. Codename: Neo. In The Coding Agent is Dead we wrote about where this is going: agents with longer leashes, less handholding, and many more places to run. Not just one agent in one terminal. Agents prompted from anywhere, running everywhere. That''s the new Amp we''re building. But the terminal still matters and will matter. There will be moments where you want the agent right next to you. So we rebuilt the CLI first. It is still Amp in your terminal. But it''s running on a completely new architecture: remote-controllable, compaction-first, plugin-powered, and much faster. Built for what''s coming. Let''s walk through it. Remote Control When you start a thread in the new Amp CLI, you can now remote control it from ampcode.com. You''ll not only get live updates but you can also send messages, queue and dequeue them, or cancel what the agent is currently doing: The architecture that enables this is the reason we rewrote Amp. And remote control is just the start. Update (2026-2026-05-27): You can now require passkey authentication to use remote control. No More Manual Context Management A core principle behind the rebuild: build for what the frontier models can do now, in 2026, and what they will be able to do in the future. Do not build for what once was. Today''s leading frontier models are great at handling compaction. So Amp now manages context for you. You don''t have to watch context percentages anymore, or decide when to handoff, or extract information from a thread in a panic. When the context window fills up, Amp now compacts the thread: it summarizes the current context, starts a fresh window with that summary, and keeps going. Compaction now runs automatically when the context window is 90% full. It was also the first thing we added to the new architecture. During one migration, we had to shut it off for a day and everyone complained. One beta-user reported: "I love having auto-compaction. NOT missing handoff..." So handoff is out. Compaction is in. Plugins With this release we''re officially releasing the Amp Plugin API. Amp plugins can: Handle events — amp.on(...) for tool calls, tool results, and agent lifecycle events Add tools — amp.registerTool(...) for custom tools the agent can call Add commands — amp.registerCommand(...) for command palette actions Show UI elements — ctx.ui.notify(...), ctx.ui.confirm(...), ctx.ui.input(...), and ctx.ui.select(...) Ask AI questions — amp.ai.ask(...) for yes/no classification with confidence and reasoning Here, for example, is a plugin that registers a tool called ask_user_choice. The agent can use it to present the user with options: Update (September 2026): ask_user_choice is now a built-in Amp tool available to everyone, so you no longer need this plugin. If you added one like it, tell Amp: "Remove the ask-user-choice plugin; Amp now has a built-in ask_user_choice tool." The example below still shows how plugin tools and UI work. // .amp/plugins/ask-user-choice.ts import type { PluginAPI } from ''@ampcode/plugin'' export default function (amp: PluginAPI) { amp.registerTool({ name: ''ask_user_choice'', description: ''Present the user with a multiple choice question when there are several possible approaches and you need them to pick one. Use when you have 2-5 concrete options to choose from.'', inputSchema: { type: ''object'', properties: { question: { type: ''string'', description: ''The question to ask the user'' }, options: { type: ''array'', items: { type: ''string'' }, description: ''The options to choose from (2-5 items)'', }, }, required: [''question'', ''options''], }, async execute(input, ctx) { const question = input.question as string const options = input.options as string[] const optionsList = options.map((opt, i) => `${i + 1}. ${opt}`).join(''\n'') const answer = await ctx.ui.input({ title: question, helpText: `${optionsList}\n\nType the number of your choice`, submitButtonText: ''Select'', }) if (!answer) return ''User dismissed the question without choosing.'' const index = parseInt(answer.trim(), 10) - 1 if (index >= 0 && index < options.length) { return `User selected option ${index + 1}: ${options[index]}` } return `User responded with: ${answer}` }, }) } That''s it: a single file in .amp/plugins and Amp gets a new tool. It looks like this: The Amp Plugin API documentation has more examples, including a full permissions plugin. Queuing & Steering Queuing messages is now the default. When you send a message while the agent is busy, it''ll get added to the queue instead of stopping and interrupting the agent. This, too, we think fits the models of today and tomorrow better. They work for longer and need fewer mid-flight yanks. If you want to fast-track a queued message, you can steer. Steering lets you send a queued message as soon as possible, not just when the agent becomes idle. The next time a tool result is sent up to the agent, for example. Use ↑ to select a queued message, then steer it with ⏎: You can also hit Esc Esc to interrupt the agent and send immediately. Permissions Amp will no longer ask for permission before running tools. What was once the --dangerously-allow-all flag is now the default behavior for users who have not configured permissions. The old permissions system still exists. It''s now a built-in plugin. If your existing Amp settings already opt into permissions — through amp.permissions, amp.dangerouslyAllowAll: false, or amp.guardedFiles.allowlist — Amp loads that plugin and works as before. (When the plugin is active, it applies in both amp and amp --execute.) Why change the default? A year ago tool calls were simpler to check: inspect the name, inspect the arguments, do string-based matching, allow or deny. Now, frontier models write throwaway scripts to get stuff done. They chain shell commands. It''s near-impossible to determine statically whether a tool invocation will be destructive or not. When a model writes five 20-line Python scripts in parallel to do something, checking whether a tool call contains rm -rf gives you a false sense of security. On top of that, there are now custom skills and scripts, specifically built for agents. And different organizations have different policies around which model is allowed to call which tool. So permissions now live in the Plugin API. If you need a policy, build the one that matches your setup. Point Amp at the Amp Plugin API and ask it to help you. Performance & Efficiency The old Amp CLI got slow with huge threads. Neo doesn''t. Here''s a comparison, using a thread with around 5000 messages: Metric Old New Improvement CPU% (mean ± sd) 84.1% ± 1.6% 17.4% ± 8.8% 79% less CPU CPU% (peak) 86.3% 25.8% — Memory (idle) 1814 MB 540 MB 70% less memory Rendering performance has improved, too. Before: After: What''s Gone We also removed features. Of course we did, otherwise it wouldn''t be an Amp release, would it? Our goal is to keep you on the frontier. Amp should not make you work like it''s still 2025. Some features made sense when models needed more babysitting, more manual context management, more careful steering. They don''t anymore. When a feature starts tying you to the old way to use agents, it goes. Handoff is gone. As described above, compaction made it obsolete. There are some valid use cases for Handoff even when there''s enough space left in the context, but we don''t think it warrants the complexity introduced by many small, connected threads. You can also still reference other threads and Amp will read them and extract the relevant information. For example, you can use Ctrl+O and thread: new to create a new thread, then hit Enter to quickly insert a reference to the previous thread. Amp will use that reference along with the rest of your prompt to read the previous thread. Amp no longer rolls back file changes when you edit or restore a message. We''ve found ourselves using this less and less as models advanced. The models are now good enough to undo changes for you, with more finesse than a rollback. And, the truth is, the rollback feature was always best-effort: if the agent wrote and ran code that generated files, we didn''t keep track of that without elaborate snapshotting. Skill management: Amp still supports Agent Skills but we no longer offer commands or subcommands to add, remove, or update skills. That''s better done by separate tools, such as skills. User-invokable skills: We also removed support for user-invokable skills. The latest generation of models now invokes skills reliably. Themes: Custom themes made it harder to keep the CLI legible, polished, and recognizably Amp. We’d rather ship one good interface than support many broken-looking ones. Manual bash invocation: in the old Amp CLI you could invoke bash commands by using $ and $$ in the prompt editor. An interesting idea a year ago, but now with models being ever more capable at running commands on their own and without blowing up their context window (and that context window being unlimited, practically) it''s no longer useful. Rollout We’re rolling Neo out over the next several days. If you want to skip the line, send us an email. We''ll flip the switch for you. (Update 2026-05-12: We''ve briefly paused adding more people to the Neo beta as we fix some bugs.) This is the first piece of the new Amp. More soon.'
content: extracted
html: 2026-05-06-amp-rebuilt.html
preview:
  file: 2026-05-06-amp-rebuilt.preview-09288df6e861.webp
  width: 256
  height: 134
  color: '#635c50'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Amp%2C+Rebuilt&date=May+6%2C+2026&tagline=The+Amp+CLI+has+been+rebuilt+from+the+ground+up.&backgroundImage=https%3A%2F%2Fampcode.com%2F%28marketing%29%2Fnews%2Fneo-bg-1.jpg&sig=01f75f87fe81a8611fb17676cb11194b7cc729f3e3882d99d11078f21bf03134
  original:
    file: 2026-05-06-amp-rebuilt.image-5accfcf44c69.png
    width: 1200
    height: 630
  variants:
  - file: 2026-05-06-amp-rebuilt.image-fcd1a557098c.webp
    width: 320
    height: 168
  - file: 2026-05-06-amp-rebuilt.image-b88895dc0e3c.webp
    width: 640
    height: 336
  - file: 2026-05-06-amp-rebuilt.image-866a4f58fb14.webp
    width: 960
    height: 504
  - file: 2026-05-06-amp-rebuilt.image-5617c5052aae.webp
    width: 1200
    height: 630
  color: '#221c19'
- source: https://static.ampcode.com/news/neo-ask-user-choice.png
  original:
    file: 2026-05-06-amp-rebuilt.image-395f0fdcfe5e.png
    width: 1922
    height: 1280
  color: '#1e2021'
---

Today we're starting to roll out the new Amp.

Not all of it, not yet. But the first piece: a rebuilt Amp CLI. Codename: Neo.

In [The Coding Agent is Dead](https://ampcode.com/news/the-coding-agent-is-dead) we wrote about where this is going: agents with longer leashes, less handholding, and many more places to run. Not just one agent in one terminal. Agents prompted from anywhere, running everywhere.

That's the new Amp we're building.

But the terminal still matters and will matter. There will be moments where you want the agent right next to you.

So we rebuilt the CLI first. It *is* still Amp in your terminal. But it's running on a completely new architecture: remote-controllable, compaction-first, plugin-powered, and much faster. Built for what's coming.

Let's walk through it.

## Remote Control

When you start a thread in the new Amp CLI, you can now remote control it from [ampcode.com](https://ampcode.com).

You'll not only get live updates but you can also send messages, queue and dequeue them, or cancel what the agent is currently doing:

The architecture that enables this is the reason we rewrote Amp. And remote control is just the start.

Update (2026-2026-05-27): You can now [require passkey authentication](https://ampcode.com/news/proof-of-human) to use remote control.

## No More Manual Context Management

A core principle behind the rebuild: build for what the frontier models can do now, in 2026, and what they will be able to do in the future. Do not build for what once was.

Today's leading frontier models are great at handling compaction.

So Amp now manages context for you.

You don't have to watch context percentages anymore, or decide when to [handoff](https://ampcode.com/news/handoff), or extract information from a thread in a panic.

When the context window fills up, Amp now compacts the thread: it summarizes the current context, starts a fresh window with that summary, and keeps going.

Compaction now runs automatically when the context window is 90% full.

It was also the first thing we added to the new architecture. During one migration, we had to shut it off for a day and everyone complained. One beta-user reported: "I love having auto-compaction. NOT missing handoff..."

So [handoff](https://ampcode.com/news/handoff) is out. Compaction is in.

## Plugins

With this release we're officially releasing the [Amp Plugin API](https://ampcode.com/manual/plugin-api).

Amp plugins can:

- **Handle events** — `amp.on(...)` for tool calls, tool results, and agent lifecycle events
- **Add tools** — `amp.registerTool(...)` for custom tools the agent can call
- **Add commands** — `amp.registerCommand(...)` for command palette actions
- **Show UI elements** — `ctx.ui.notify(...)`, `ctx.ui.confirm(...)`, `ctx.ui.input(...)`, and `ctx.ui.select(...)`
- **Ask AI questions** — `amp.ai.ask(...)` for yes/no classification with confidence and reasoning

Here, for example, is a plugin that registers a tool called `ask_user_choice`. The agent can use it to present the user with options:

> **Update (September 2026):** `ask_user_choice` is now a built-in Amp tool available to everyone, so you no longer need this plugin. If you added one like it, tell Amp: "Remove the ask-user-choice plugin; Amp now has a built-in `ask_user_choice` tool." The example below still shows how plugin tools and UI work.

```typescript
// .amp/plugins/ask-user-choice.ts

import type { PluginAPI } from '@ampcode/plugin'

export default function (amp: PluginAPI) {
	amp.registerTool({
		name: 'ask_user_choice',
		description:
			'Present the user with a multiple choice question when there are several possible approaches and you need them to pick one. Use when you have 2-5 concrete options to choose from.',
		inputSchema: {
			type: 'object',
			properties: {
				question: { type: 'string', description: 'The question to ask the user' },
				options: {
					type: 'array',
					items: { type: 'string' },
					description: 'The options to choose from (2-5 items)',
				},
			},
			required: ['question', 'options'],
		},
		async execute(input, ctx) {
			const question = input.question as string
			const options = input.options as string[]
			const optionsList = options.map((opt, i) => `${i + 1}. ${opt}`).join('\n')

			const answer = await ctx.ui.input({
				title: question,
				helpText: `${optionsList}\n\nType the number of your choice`,
				submitButtonText: 'Select',
			})

			if (!answer) return 'User dismissed the question without choosing.'

			const index = parseInt(answer.trim(), 10) - 1
			if (index >= 0 && index < options.length) {
				return `User selected option ${index + 1}: ${options[index]}`
			}
			return `User responded with: ${answer}`
		},
	})
}
```

That's it: a single file in `.amp/plugins` and Amp gets a new tool. It looks like this:

![The ask_user_choice tool in action](https://static.ampcode.com/news/neo-ask-user-choice.png)

The [Amp Plugin API](https://ampcode.com/manual/plugin-api) documentation has more examples, including a full permissions plugin.

## Queuing & Steering

Queuing messages is now the default. When you send a message while the agent is busy, it'll get added to the queue instead of stopping and interrupting the agent.

This, too, we think fits the models of today and tomorrow better. They work for longer and need fewer mid-flight yanks.

If you want to fast-track a queued message, you can *steer*.

Steering lets you send a queued message as soon as possible, not just when the agent becomes idle. The next time a tool result is sent up to the agent, for example.

Use ↑ to select a queued message, then steer it with ⏎:

You can also hit Esc Esc to interrupt the agent and send immediately.

## Permissions

Amp will no longer ask for permission before running tools.

What was once the `--dangerously-allow-all` flag is now the default behavior for users who have not configured permissions.

The old permissions system still exists. It's now a built-in plugin. If your existing Amp settings already opt into permissions — through `amp.permissions`, `amp.dangerouslyAllowAll: false`, or `amp.guardedFiles.allowlist` — Amp loads that plugin and works as before. (When the plugin is active, it applies in both `amp` and `amp --execute`.)

Why change the default?

A year ago tool calls were simpler to check: inspect the name, inspect the arguments, do string-based matching, allow or deny. Now, frontier models write throwaway scripts to get stuff done. They chain shell commands.

It's near-impossible to determine statically whether a tool invocation will be destructive or not.

When a model writes five 20-line Python scripts in parallel to do something, checking whether a tool call contains `rm -rf` gives you a false sense of security.

On top of that, there are now custom skills and scripts, specifically built for agents. And different organizations have different policies around which model is allowed to call which tool.

So permissions now live in the Plugin API.

If you need a policy, build the one that matches your setup. Point Amp at the [Amp Plugin API](https://ampcode.com/manual/plugin-api) and ask it to help you.

## Performance & Efficiency

The old Amp CLI got slow with huge threads. Neo doesn't. Here's a comparison, using a thread with around 5000 messages:

| Metric           | Old          | New          | Improvement     |
| ---------------- | ------------ | ------------ | --------------- |
| CPU% (mean ± sd) | 84.1% ± 1.6% | 17.4% ± 8.8% | 79% less CPU    |
| CPU% (peak)      | 86.3%        | 25.8%        | —               |
| Memory (idle)    | 1814 MB      | 540 MB       | 70% less memory |

Rendering performance has improved, too.

Before:

After:

## What's Gone

We also removed features. Of course we did, otherwise it wouldn't be an Amp release, would it?

Our goal is to keep you on the frontier. Amp should not make you work like it's still 2025.

Some features made sense when models needed more babysitting, more manual context management, more careful steering. They don't anymore. When a feature starts tying you to the old way to use agents, it goes.

**Handoff** is gone. As described above, compaction made it obsolete. There are some valid use cases for Handoff even when there's enough space left in the context, but we don't think it warrants the complexity introduced by many small, connected threads.

You can also still reference other threads and Amp will read them and extract the relevant information.

For example, you can use Ctrl+O and `thread: new` to create a new thread, then hit Enter to quickly insert a reference to the previous thread. Amp will use that reference along with the rest of your prompt to read the previous thread.

**Amp no longer rolls back file changes when you edit or restore a message.** We've found ourselves using this less and less as models advanced. The models are now good enough to undo changes for you, with more finesse than a rollback. And, the truth is, the rollback feature was always best-effort: if the agent wrote and ran code that generated files, we didn't keep track of that without elaborate snapshotting.

**Skill management**: Amp still supports [Agent Skills](https://agentskills.io/home) but we no longer offer commands or subcommands to add, remove, or update skills. That's better done by separate tools, such as [`skills`](https://github.com/vercel-labs/skills).

**User-invokable skills**: We also removed support for user-invokable skills. The latest generation of models now invokes skills reliably.

**Themes**: Custom themes made it harder to keep the CLI legible, polished, and recognizably Amp. We’d rather ship one good interface than support many broken-looking ones.

**Manual bash invocation**: in the old Amp CLI you could invoke bash commands by using `$` and `$$` in the prompt editor. An interesting idea a year ago, but now with models being ever more capable at running commands on their own and without blowing up their context window (and that context window being unlimited, practically) it's no longer useful.

## Rollout

We’re rolling Neo out over the next several days. If you want to skip the line, send us an email. We'll flip the switch for you. (Update 2026-05-12: We've briefly paused adding more people to the Neo beta as we fix some bugs.)

This is the first piece of the new Amp.

More soon.
