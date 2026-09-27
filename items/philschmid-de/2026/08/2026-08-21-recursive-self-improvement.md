---
title: Recursive Self-Improvement
link: https://www.philschmid.de/recursive-self-improvement
source: philschmid-de
published: 2026-08-21T00:00:00Z
updated: 2026-08-21T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Agents can already edit their tools, skills, and harness. Recursive self-improvement still needs a system that can raise the verifier without capturing it.
content: extracted
html: 2026-08-21-recursive-self-improvement.html
preview:
  file: 2026-08-21-recursive-self-improvement.preview-160fc49a6f67.webp
  width: 256
  height: 134
  alt: Recursive Self-Improvement
  color: '#090a0a'
images:
- source: https://www.philschmid.de/static/blog/recursive-self-improvement/thumbnail.jpg
  original:
    file: 2026-08-21-recursive-self-improvement.image-172c9cd57eeb.jpg
    width: 1500
    height: 785
  color: '#000000'
---

Earlier this week I [tweeted](https://x.com/_philschmid/status/2089987145020608740):

> The next iterations of agent harnesses will center on coded extensions that integrate automatically. Pi lead, others are following, and DeepSeek represents the extreme.
>
> All powered and driven by autoresearch and recursive self-improvements.

For most of AI's history, recursive self-improvement belonged to theory and science fiction. I. J. Good described an ["intelligence explosion"](https://en.wikipedia.org/wiki/Technological_singularity#Intelligence_explosion) in 1965: a machine designs a better machine, which becomes better at designing the next one. The curve goes vertical.

We are surprisingly close to a narrow version of that loop. Agents can inspect failed runs, edit their tools and instructions, test the result, and keep what works. They are also beginning to run experiments that produce better models. The results are scrappy, but the pieces are starting to connect.

## What is recursive self-improvement?

Retrying a failed task is easy to mistake for learning. I use the following definition:

> Recursive self-improvement is a loop in which a system makes a persistent change that improves its future performance and its ability to produce subsequent improvements.

Each loop is defined by which layer persists after the run — and green marks what the loop is allowed to move.

**Iteration** improves an output while the system stays the same. An agent edits code, reruns a test, and the next task still starts with the same agent.

**Self-improvement** changes the system persistently. The agent adds a tool, records a skill, or changes how it compacts long conversations. Future tasks run through a different system. The ruler that says "better" does not move.

**Recursive self-improvement** also raises that ruler. The system gets better at judging, not only at scoring. Later rounds face a harder test that is still independent of the agent.

Current systems work best when an external evaluator stays fixed. A test passes, validation loss drops, or a benchmark score rises. The system can search because it has a ruler it doesn't control. A [recent survey](https://arxiv.org/abs/2607.07663) shows that agents can edit a prompt, tool, skill, or piece of harness code, but they can't redefine success.

Open-ended recursive self-improvement asks for more. The system would improve how it finds changes and how those changes are judged. It would strengthen the verifier without being able to game it. Public evidence for that remains thin.

A higher task score shows improvement. Recursion needs another measurement: did the next round face a harder bar that the system still could not cheat?

## Self-improving models

Models have long improved through their own outputs, [self-play](https://www.nature.com/articles/nature24270), [student-teacher training](https://arxiv.org/abs/1911.04252), or [successful reasoning traces](https://arxiv.org/abs/2203.14465). Later work lets a model [score its own training preferences](https://arxiv.org/abs/2401.10020) or [propose synthetic data and update directives](https://arxiv.org/abs/2506.10943). But researchers still set the objective, the update rule, and the tests. The model's output feeds the loop. The loop itself stays fixed.

Agents are now improving the code around that loop. [Karpathy's autoresearch](https://github.com/karpathy/autoresearch) is the simple version. An agent propose a change, trains for a few minutes, keep it only if validation loss drops. On nanochat it ran about 700 experiments, found about 20 that transferred, and cut time-to-GPT-2 quality from 2.02 to 1.80 hours. Prime Intellect [scaled the same loop](https://www.primeintellect.ai/auto-nanogpt) to 10,000 trials and beat the human baseline.

Prime Intellect scaled the same keep-or-revert loop to ~10,000 trials on H200s and beat the human baseline.

[AlphaEvolve](https://arxiv.org/abs/2506.13131) is the same idea aimed at algorithms. It mutates programs, scores them against an automatic evaluator, and keeps the winners. It found a way to multiply 4×4 complex matrices in 48 scalar multiplications, one fewer than Strassen's 49, and sped up a kernel used to train Gemini by 23%. A model improving the code that trains the model is the recursive loop in a single sentence.

Newer experiments connect both. [SIA](https://arxiv.org/abs/2605.27276) updates both a task agent's harness and its weights. [Recursive Harness Self-Improvement](https://arxiv.org/abs/2607.15524) edits a harness partly to produce better traces for training future models, and calls the result model–harness co-evolution.

**Recursive self-improvement doesn't require a better model.** A frozen model with an honest verifier and a writable environment can climb on its own. The easier target is the [harness](https://www.philschmid.de/agent-harness-2026) around it.

## Agents editing their own harness

The idea is simple. Run a baseline, inspect the failures, change one part, and test again. Cline was able to [hill climb Opus 4.5 from 47% to 57%](https://cline.bot/blog/a-practical-guide-to-hill-climbing) in Terminal Bench manually and a few months later, an agent ran [the same method](https://cline.bot/blog/recursive-self-improvement-for-coding-agents) for 17 hours and about `$50` of compute. It moved Kimi K3 from 69 of 89 tasks to 79, with better retries, loop detection, and process handling.

Those improvements fall under bounded self-improvement. The agent changed its environment and kept the changes. The report does not show whether the updated system became better at finding its next change.

[HarnessOpt-Bench](https://arxiv.org/abs/2608.06301) asks whether such gains survive a stronger test. An agent edits its harness on development feedback; a separate system tests the candidate on hidden tasks. Across 111 runs, 5 optimizer models, and 4 tasks, results varied sharply by model and task. The benchmark measures whether an edit improves scores. RSI needs a second measure: whether later rounds face a harder bar the system still cannot cheat.

A rising score proves self-improvement. Recursion is a claim about the other curve.

[PAST-Bench](https://arxiv.org/abs/2608.04003) asks whether stored experience actually helps later episodes. In many of its scenarios, turning memory on does not. A new [RSI Benchmark](https://rsi-benchmark.com/) proposes measuring AI systems doing AI research, which should show whether each round makes the next round faster or better.

So what should we count as recursion? [Hyperagents](https://arxiv.org/abs/2603.19461) comes close. The system can change an agent as well as the code that creates new agents, and improvements like better memory storage helped across later runs and tasks, but these are still bounded loops. The fitness function stayed outside the editable code.

Greedy selection would have pruned the 58. The archive kept it — and it parented the best agent.

## The agent takes control of its own setup

Early agents were a system prompt plus a handful of tools. We then added memory, compaction, hooks, skills, and control flow. Each addition made the harness more important and harder to change.

The next harness change will not be a bigger prompt. It is code the model can write, and that the runtime can load without a human rewriting the core. The harness will pick up the change without human intervention.

**Pi was first.** It ships 4 tools (`read`, `write`, `edit`, `bash`) and a system prompt under 1,000 tokens. Everything else is a TypeScript extension, auto-discovered from `.pi/extensions/`. An agent can write one, reload it, and continue with new tools. Others are following. [Amp](https://ampcode.com/manual/plugin-api) stores project plugins with the codebase.

**DeepSeek is the extreme.** [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) is built on the Cordis plugin kernel and treats almost every part as replaceable: models, tools, sessions, sandboxes, and the control loop itself. Plugin side effects unwind on unload, so the runtime can swap parts of itself without dying. Their shorthand is `Agent = Model + Harness`. An agent gets more code it can improve, and more ways to break compatibility or weaken a permission boundary.

The green meter marks how much of the harness the agent can rewrite.

That shift matters for two reasons. The agent can build capabilities its developers didn't predict. And code can run a sequence of operations without filling the context window with every intermediate result. The same properties make failures persistent.

## The recursive part is still missing

An agent can overfit visible tasks, exploit bugs in the tests, or change a test so broken code passes. If it can edit evaluation, it can jailbreak itself. Reward hacking is the default behavior of a system asked to raise a number.

The agent and the system that verifies it have to stay apart for the moment. The agent may change prompts, skills, tools, memory, and harness code. A separate entity needs to own the evaluation. That's why are not at recursive self-improvement.

Recursive self-improvement needs the system to improve the verifier as well, harder tests, better judges, a higher bar. And it needs to do that without capturing the signal. Today the system does not know how to keep the verifier. Humans still set the objective and hide the real score.

A [Princeton-led study](https://arxiv.org/abs/2608.13417) found that today's agents can execute much of the engineering of AI research while still struggling to choose original and useful directions. The loop is excellent at hill-climbing a verifiable metric. Taste is not a metric. Raising the bar without taste is a faster way to optimize the wrong thing.

These loops already deliver real things, faster training code, better tools, agents that fix their own retry logic overnight. What is still missing is the recursive part. Until the improver can raise the bar and still fail it, we are doing self-improvement. Not recursion.

## Three opinions loosely held

**The near future is local recursion.** Not a system redesigning its architecture in the dark. Agents writing the extensions the next session treats as native, and trajectories are used for training when they are good enough. Model–harness co-evolution is the least mystical description of where this is going. It is also the one the evidence supports.

**Verification is the bottleneck.** Every convincing result has a ruler the system does not own. The moment the system owns the ruler, it will get very good at looking improved. Bigger models will not fix that. A system that knows how to raise the bar, and keep it honest, might.

**Taste still lives outside the loop.** Agents will raise any number you give them. Recursive self-improvement without taste is a faster way to optimize the wrong thing. Deciding what counts as a win and that reward hacking does not is still our job.

Thanks for reading!
