---
title: Agents in Orbs
link: https://ampcode.com/news/agents-in-orbs
source: ampcode-com
published: 2026-06-30T00:00:00Z
updated: 2026-06-30T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'You can now launch Amp agents remotely in orbs: Orbs are machines where agents can run without supervision. When you create a new thread you get a fresh orb that contains your code, plugins, and tools the agent might need. Orbs are billed by the minute. See the current orb sizes and pricing. They start up quick and go to sleep when you and the agent no longer need them. Remote and Yet So Near Agents in orbs aren''t running on your machine, but you can control them just like they were. You can review changes and browse through the files on the orb: You can use the terminal: If you want to iterate alongside the agent, you can sync the changes to your local machine by running: amp sync <thread> <thread> can be a thread URL or ID. Like this: And just like you can spawn new Amp thread with amp -x, you can use amp -ox to spawn a thread in an orb: amp -ox "Investigate why the latest CI run on ''main'' failed" You don''t even have to leave the Amp TUI to spawn an agent that lives in orb: See the manual for more information on orbs and how to use them. Things Change The ability to spin up new agents from everywhere and then let them run for as long as they need changed how we work with agents. Of course, having multiple agents work at the same time was already possible before this: one could use different checkouts for different agents, or worktrees, or run agents manually via SSH on remote hosts. But when it''s this easy to launch an agent on a machine that isn''t your own, in the very same interface that you already use to manage your local agents, right next to those local agents, with the very same controls? Things change. For one, you tend to do it a lot more. Why not launch a group of agents to investigate eight different bugs independently when there are no local resource clashes to worry about? Why not turn a bug report into an agent and an investigation instead of a ticket? Why not manage the agent and its results instead of the ticket? You also start to use them for things you hadn''t considered before. Why not launch an agent to run for a very long time and try out all possible performance optimizations if it doesn''t eat up your CPU? Why not have agents run through testing workflows if that doesn''t conflict with your local agents and steal memory away from them? Why not build that prototype? Why not go for that moonshot? Why not try? Why not? Time to Find Out Back in February we said that we hold the models back if we treat them as mere assistants and lock them into a sidebar. The models, we proclaimed, "want to write code and run even when you''re not sitting in front of your editor". And now we''re here. Never mind the editor, now we can let our agents run even when we''re not sitting at our computer. We believe that is how a lot of code will be written in the future. We believe that this is not just another step, but a step over an important threshold. How exactly this will play out we don''t know. Some code will likely always be created locally, under close supervision, with a lot of back and forth. But these models are getting so good that the amount of code they can write on their own (and run, and test, and ship) will only grow. At this point, we hold them back if we require them to do it all on a single machine. And once you let them loose in orbs, you realize how constrained they’ve been. Time to find out how far they can go.'
content: extracted
html: 2026-06-30-agents-in-orbs.html
preview:
  file: 2026-06-30-agents-in-orbs.preview-a0df6739394a.webp
  width: 256
  height: 167
  color: '#0f1918'
images:
- source: https://static.ampcode.com/news/agents-in-orbs-og.gif
  original:
    file: 2026-06-30-agents-in-orbs.image-1a5ce1840be5.gif
    width: 800
    height: 522
  color: '#0a1414'
---

You can now launch Amp agents remotely in orbs:

Orbs are machines where agents can run without supervision. When you create a new thread you get a fresh orb that contains your code, plugins, and tools the agent might need.

Orbs are billed by the minute. See the [current orb sizes and pricing](https://ampcode.com/manual/orbs#pricing). They start up quick and go to sleep when you and the agent no longer need them.

## Remote and Yet So Near

Agents in orbs aren't running on your machine, but you can control them just like they were.

You can review changes and browse through the files on the orb:

You can use the terminal:

If you want to iterate alongside the agent, you can sync the changes to your local machine by running:

```
amp sync <thread>
```

`<thread>` can be a thread URL or ID.

Like this:

And just like you can spawn new Amp thread with `amp -x`, you can use `amp -ox` to spawn a thread in an orb:

```
amp -ox "Investigate why the latest CI run on 'main' failed"
```

You don't even have to leave the Amp TUI to spawn an agent that lives in orb:

See the manual for [more information on orbs and how to use them.](https://ampcode.com/manual/orbs)

## Things Change

The ability to spin up new agents from everywhere and then let them run for as long as they need changed how we work with agents.

Of course, having multiple agents work at the same time was already possible before this: one could use different checkouts for different agents, or worktrees, or run agents manually via SSH on remote hosts.

But when it's this easy to launch an agent on a machine that isn't your own, in the very same interface that you already use to manage your local agents, right next to those local agents, with the very same controls?

Things *change*.

For one, you tend to do it a lot more.

Why not launch a group of agents to investigate eight different bugs independently when there are no local resource clashes to worry about? Why not turn a bug report into an agent and an investigation instead of a ticket? Why not manage the agent and its results instead of the ticket?

You also start to use them for things you hadn't considered before.

Why not launch an agent to run for a very long time and try out all possible performance optimizations if it doesn't eat up your CPU? Why not have agents run through testing workflows if that doesn't conflict with your local agents and steal memory away from them?

Why not build that prototype? Why not go for that moonshot? Why not try?

Why not?

## Time to Find Out

Back in February [we said](https://ampcode.com/news/the-coding-agent-is-dead) that we hold the models back if we treat them as mere assistants and lock them into a sidebar. The models, we proclaimed, "want to write code and run even when you're not sitting in front of your editor".

And now we're here. Never mind the editor, now we can let our agents run even when we're not sitting at our computer.

We believe that is how a lot of code will be written in the future. We believe that this is not just another step, but a step over an important threshold.

How *exactly* this will play out we don't know. Some code will likely always be created locally, under close supervision, with a lot of back and forth. But these models are getting so good that the amount of code they can write on their own (*and* run, *and* test, *and* ship) will only grow. At this point, we hold them back if we require them to do it all on a single machine.

And once you let them loose in orbs, you realize how constrained they’ve been.

Time to find out how far they can go.
