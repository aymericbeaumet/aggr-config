---
title: Custom Agents
link: https://ampcode.com/news/custom-agents
source: ampcode-com
published: 2026-06-19T00:00:00Z
updated: 2026-06-19T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'You can now create custom agents in Amp with plugins. You can use these custom agents as your main Amp agent, or as subagents. You can use them as a small part of a tool pipeline that you invoke with amp -x. Or you can spawn 25 custom worker agents, then switch between them. Each custom agent comes with a custom orb color. Here is how you define a custom agent in an Amp plugin: // .amp/plugins/focused-reviewer-agent.ts import type { PluginAPI } from ''@ampcode/plugin'' export default function (amp: PluginAPI) { // Create the agent const reviewer = amp.createAgent({ name: ''focused-reviewer'', model: ''openai/gpt-5.5'', instructions: [ ''You are a focused code-review subagent.'', ''Inspect only the files and concerns named by the caller.'', ''Return concise findings with severity, evidence, and suggested fixes.'', ].join('' ''), tools: ''all'', display: { label: ''reviewer'', color: ''#d97706'' }, }) // Register a tool. This agent acts as a subagent amp.registerTool({ name: ''focused_review'', description: ''Run a focused code-review subagent.'', inputSchema: { type: ''object'', properties: { request: { type: ''string'' }, }, required: [''request''], }, async execute(input, ctx) { // Run a one-shot agent turn const result = await reviewer.run(input, { parentThreadID: ctx.thread.id, }) return result.text }, }) // Or register the agent as a selectable main thread mode amp.registerAgentMode({ key: ''focused-reviewer'', description: ''Code Review Expert'', agent: reviewer.definition, }) } Threads Once you have defined an agent, you can create threads: // Spawn a new thread const thread = await reviewer.createThread({ // Tell the UI switch to this thread show: true, }) // Get an existing thread const thread = amp.threads.get(input.threadID) The Thread object lets you interact with a thread in many different ways, and is where the real power comes in. Send a message to a Thread Add a new user message to a thread by calling thread.appendUserMessage(). The call returns as soon as Amp has accepted the message; it does not wait for inference to complete before returning. await thread.appendUserMessage({ type: ''user-message'', content: ''Review the auth changes in this branch.'', }) Wait for the Agent''s response When you do want to wait, call waitForResponse() on the thread. It resolves with the next assistant message after the agent finishes its turn. const reply = await thread.waitForResponse() Example: Spawn an async thread that responds to the main thread These are just a few primitives provided by the Plugin API. Together, they compose into unique workflows. An example used on the Amp team: spawn an agent in an asynchronous thread, and give it the tools it needs to respond to the parent when it needs to. amp.registerTool({ name: ''start_async_review'', description: ''Start a review in a background thread.'', inputSchema: { type: ''object'', properties: {} }, async execute(_input, ctx) { const thread = await reviewer.createThread({ parentThreadID: ctx.thread.id, }) await thread.appendUserMessage({ type: ''user-message'', content: [ ''Review the auth changes in this branch.'', `When you are done, call send_to_thread with threadID ${ctx.thread.id}`, ''and include your review in the message.'', ].join('' ''), }) return `Started background review in ${thread.id}.` }, }) Full documentation is in the manual. Happy Hacking.'
content: extracted
html: 2026-06-19-custom-agents.html
preview:
  file: 2026-06-19-custom-agents.preview-28768d7a9e29.webp
  width: 256
  height: 134
  color: '#5f584d'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Custom+Agents&date=June+19%2C+2026&tagline=Plugins+can+now+create+agents%2C+run+them+once%2C+and+keep+talking+to+their+threads.&sig=5bf0d5a577b5c7fc1a43dae02711130bc5f0323bca5c4133aa38de000d7f391c
  original:
    file: 2026-06-19-custom-agents.image-6bf6d4ceb63b.png
    width: 1200
    height: 630
  variants:
  - file: 2026-06-19-custom-agents.image-0b4391ecb0b1.webp
    width: 320
    height: 168
  - file: 2026-06-19-custom-agents.image-cc6e1e8f297e.webp
    width: 640
    height: 336
  - file: 2026-06-19-custom-agents.image-9f9319fa9b86.webp
    width: 960
    height: 504
  - file: 2026-06-19-custom-agents.image-35402fb38afe.webp
    width: 1200
    height: 630
  color: '#221b16'
---

You can now create custom agents in Amp with plugins.

You can use these custom agents as your main Amp agent, or as subagents. You can use them as a small part of a tool pipeline that you invoke with `amp -x`. Or you can spawn 25 custom worker agents, then switch between them.

Each custom agent comes with a custom orb color.

Here is how you define a custom agent in an Amp plugin:

```ts
// .amp/plugins/focused-reviewer-agent.ts
import type { PluginAPI } from '@ampcode/plugin'

export default function (amp: PluginAPI) {
	// Create the agent
	const reviewer = amp.createAgent({
		name: 'focused-reviewer',
		model: 'openai/gpt-5.5',
		instructions: [
			'You are a focused code-review subagent.',
			'Inspect only the files and concerns named by the caller.',
			'Return concise findings with severity, evidence, and suggested fixes.',
		].join(' '),
		tools: 'all',
		display: { label: 'reviewer', color: '#d97706' },
	})

	// Register a tool. This agent acts as a subagent
	amp.registerTool({
		name: 'focused_review',
		description: 'Run a focused code-review subagent.',
		inputSchema: {
			type: 'object',
			properties: {
				request: { type: 'string' },
			},
			required: ['request'],
		},

		async execute(input, ctx) {
			// Run a one-shot agent turn
			const result = await reviewer.run(input, {
				parentThreadID: ctx.thread.id,
			})
			return result.text
		},
	})

	// Or register the agent as a selectable main thread mode
	amp.registerAgentMode({
		key: 'focused-reviewer',
		description: 'Code Review Expert',
		agent: reviewer.definition,
	})
}
```

## Threads

Once you have defined an agent, you can create threads:

```ts
// Spawn a new thread
const thread = await reviewer.createThread({
	// Tell the UI switch to this thread
	show: true,
})

// Get an existing thread
const thread = amp.threads.get(input.threadID)
```

The `Thread` object lets you interact with a thread in many different ways, and is where the real power comes in.

### Send a message to a Thread

Add a new user message to a thread by calling `thread.appendUserMessage()`. The call returns as soon as Amp has accepted the message; it does not wait for inference to complete before returning.

```ts
await thread.appendUserMessage({
	type: 'user-message',
	content: 'Review the auth changes in this branch.',
})
```

### Wait for the Agent's response

When you do want to wait, call `waitForResponse()` on the thread. It resolves with the next assistant message after the agent finishes its turn.

```ts
const reply = await thread.waitForResponse()
```

## Example: Spawn an async thread that responds to the main thread

These are just a few primitives provided by the Plugin API. Together, they compose into unique workflows. An example used on the Amp team: spawn an agent in an asynchronous thread, and give it the tools it needs to respond to the parent when it needs to.

```ts
amp.registerTool({
	name: 'start_async_review',
	description: 'Start a review in a background thread.',
	inputSchema: { type: 'object', properties: {} },
	async execute(_input, ctx) {
		const thread = await reviewer.createThread({
			parentThreadID: ctx.thread.id,
		})

		await thread.appendUserMessage({
			type: 'user-message',
			content: [
				'Review the auth changes in this branch.',
				`When you are done, call send_to_thread with threadID ${ctx.thread.id}`,
				'and include your review in the message.',
			].join(' '),
		})

		return `Started background review in ${thread.id}.`
	},
})
```

Full documentation is [in the manual](https://ampcode.com/manual/plugin-api). Happy Hacking.
