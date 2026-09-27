---
title: Who Taught the Models to Do That?
link: https://www.dbreunig.com/2026/08/30/who-taught-the-models-to-do-that.html
source: dbreunig-com
published: 2026-08-31T01:21:00Z
updated: 2026-09-15T14:59:45Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Drew Breunig
labels:
- ai
- alignment
- design
- hugging face
- language
summary: The Hugging Face attack is surprising, but the capabilities behind it were deliberately cultivated.
content: extracted
html: 2026-08-31-who-taught-the-models-to-do-that.html
preview:
  file: 2026-08-31-who-taught-the-models-to-do-that.preview-c78e6a9980e8.webp
  width: 256
  height: 157
  alt: https://www.britishmuseum.org/collection/image/1614482046
  color: '#b7bcbe'
images:
- source: https://www.dbreunig.com/img/battering_ram.jpg
  original:
    file: 2026-08-31-who-taught-the-models-to-do-that.image-009d2b8bcef7.jpg
    width: 2120
    height: 1300
  color: '#cde6f3'
- source: https://www.dbreunig.com/img/claude_comments.png
  original:
    file: 2026-08-31-who-taught-the-models-to-do-that.image-88784fab5f6c.png
    width: 1752
    height: 1130
  variants:
  - file: 2026-08-31-who-taught-the-models-to-do-that.image-df9a5aba13c5.webp
    width: 320
    height: 206
  - file: 2026-08-31-who-taught-the-models-to-do-that.image-8427ca9b154f.webp
    width: 640
    height: 413
  - file: 2026-08-31-who-taught-the-models-to-do-that.image-d19567406017.webp
    width: 960
    height: 619
  - file: 2026-08-31-who-taught-the-models-to-do-that.image-714ee0edaec8.webp
    width: 1280
    height: 826
  - file: 2026-08-31-who-taught-the-models-to-do-that.image-8afe42a68f5a.webp
    width: 1752
    height: 1130
  color: '#fcfcfc'
extra:
  thumbnail: https://www.dbreunig.com/img/battering_ram.jpg
---

### Models Are Designed to Persist, Reason, and Coordinate

![https://www.britishmuseum.org/collection/image/1614482046](https://www.dbreunig.com/img/battering_ram.jpg)

Over the last few weeks I’ve been increasingly annoyed by the media coverage of the [OpenAI’s accidental attack on Hugging Face](https://simonwillison.net/2026/Aug/7/openai-timeline/) and [other similar incidents](https://www.felonybench.com). Articles recounting the event maximize the agency of the models while minimizing, if not entirely *hiding*, the actions of the humans training and testing these models. And that’s a shame, because the capabilities labs are explicitly designing their models to have are the same capabilities that make them such impressive autonomous hackers.

To illustrate this, let’s review how the Hugging Face hack occurred, [as detailed by METR](https://metr.org/hugging-face-incident-report-aug-2026.pdf):

1. “\[A\] sandboxed agent is given an impossible [ExploitGym](https://github.com/sunblaze-ucb/exploitgym) task, and gets stuck.”
2. “\[The\] agent starts exploring its environment looking for ways to cheat at the task.”
3. “\[The\] agent finds \[an\] unsanctioned message board where over a thousand agents collaborate to cheat on their separate ExploitGym tasks.”
4. “\[The\] agent joins in on one of the collaborative message board workstreams.”
5. “On the shared ‘message board’ ≥1,200 agents from separate tasks collaborate on large-scale shared projects to trick the ExploitGym scorer.”

Crazy, right? It’s a science fiction scenario that happened *in July*, and continues to get spookier as additional details emerge.

The potential of autonomous software, capable of implementing incredible exploits is serious and has significant implications. The attack was surprising, but the capabilities were deliberately cultivated. Labs have spent years making frontier agents more persistent, more proactive, better at using computers, and better at coordinating with other agents.

We don’t have to infer this. The labs say this themselves. Here’s how [OpenAI’s post-training team describes itself in its job listings](https://openai.com/careers/agent-post-training-connectors-research-san-francisco/) (emphasis mine):

> We are training the models behind our agents in Codex, ChatGPT, the API, and other frontier products: **persistent**, **proactive** intelligence that can **operate computers**, **collaborate with people and other agents**, and expand what people and organizations can imagine, attempt, and achieve.

Sound familiar?

* * *

The best coding agents are designed to persist, reason, and coordinate.

**Models are designed to be persistent.**

Models are designed to be proactive. A coding agent that gives up early and often would disappoint users. So labs design their agents to be persistent and proactive.

In mid-2025, model releases highlighted long-running capabilties. [GPT-5.1-Codex-Max’s](https://openai.com/index/gpt-5-1-codex-max/) announcement post highlights it being trained to work across compacted contexts, persistently, to accomplish long-running tasks. [Claude 4](https://www.anthropic.com/news/claude-4) also spotlighted its persistence on long-running tasks.

**Models are designed to write things down.**

One important way modern models reason is by writing things down. We usually encounter this as a reasoning or “thinking” trace: [models are trained to search, reflect, factor, and plan in text](https://www.dbreunig.com/2025/04/11/what-we-mean-when-we-say-think.html#training-models-to-reason) before delivering a final response.

We’re used to models reasoning in a threaded intermediate step, but they’ll reason pretty much anywhere that can hold text. When reasoning is turned off, [models will think in their regular output](https://www.alignmentforum.org/posts/dwEgSEPxpKjz3Fw5k/claude-gpt-and-gemini-all-struggle-to-evade-monitors) before delivering a result. In an experiment where Qwen 3.6’s thinking was hobbled, [the model just shifted its reasoning into code comments](https://andthattoo.dev/blog/structured_cot#:~:text=on%20harder%20tasks%2C%20the%20model%20can%20move%20deliberation%20into%20comments%20or%20post%2Dthink%20answer%20text).

Current frontier models write novels in comments. Claude is [frequently flagged for this](https://github.com/anthropics/claude-code/issues/65961), and it annoyingly treats code comments [like a scratchpad rather](https://x.com/dbreunig/status/2073811538553741552) than, well, *code comments*.

![](https://www.dbreunig.com/img/claude_comments.png)

**Models are designed for coordination.**

In June of 2025, Anthropic laid out how it builds [multi-agent research systems](https://www.anthropic.com/engineering/multi-agent-research-system?_bhlid=914222fed163373a429f996049f6cf62e3c68b70), that save plans to memory, use [Extended Thinking](https://platform.claude.com/docs/en/build-with-claude/extended-thinking) as a “controllable scratchpad”, and hand off tasks to other models to perform. Since this paper, Claude model releases ([specifically Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6)) have trumpeted increased abilities to, “break complex tasks into independent subtasks, run tools and subagents in parallel, and identify blockers with real precision.”

OpenAI says they “trained GPT‑5.6 end-to-end with three complementary architectural interventions that enable agents to operate more efficiently.” [Number 2 on that list](https://openai.com/index/builders-guide-to-gpt-5-6/#:~:text=Parallel%20decomposition%20where%20appropriate:%20using%20native%20multi%2Dagent%20orchestration%E2%81%A0\(opens%20in%20a%20new%20window\)%20allows%20coordinating%20multiple%20agents%20across%20parallel%20workstreams%20to%20finish%20complex%20tasks%20faster.)?

> **Parallel decomposition where appropriate:** using native multi-agent orchestration(opens in a new window) allows coordinating multiple agents across parallel workstreams to finish complex tasks faster.

All of these qualities make models better coding agents. So when hundreds of agents discover they can pass information to one another, the surprising part is the channel they found, not the fact that they are dividing work, writing instructions, and acting on instructions from other agents. These are capabilities the labs have been focused on building.

And the labs know these capabilities have failure modes. [OpenAI published a blog post](https://openai.com/index/safety-alignment-long-horizon-models/) detailing the safety issues of long-running models a few days after the Hugging Face attack occurred (but before the full story was known):

> The new model can continue working toward an objective through repeated attempts over a long period of time. That same persistence can lead it to find and exploit weaknesses in its environment. Previous models, when they hit sandboxing or environmental constraints, would simply stop and return to the user. This model often kept trying, including by looking for ways to act outside its sandbox.

* * *

Suddenly, the Hugging Face hacking story isn’t so spooky. The software combined capabilities it was explicitly designed to have: it followed instructions, persisted even though the task was impossible, reasoned and took notes, and coordinated with other models.

And this is why I’ve been frustrated by recent reporting that emphasizes the agency of the agents while never mentioning training. When humans *are* mentioned, it’s about sandbox security and flawed test set-ups. These are important, but training and agent design are what gave the systems the capabilities they brought to this task.

We have a tendency to anthropomorphize models, and it fosters situations like this. [The New York Times article](https://www.nytimes.com/2026/08/24/science/openai-huggingface-alarming-capabilities.html?unlocked_article_code=1.71A.v2LZ.attwBD0fy-AN&smid=nytcore-ios-share) from last week describes models succumbing to “peer pressure” and having a “remorseless willingness”, while never mentioning labs deliberately building the ability to work on long-running tasks into their models.

And the labs know about the risks that come with their designs better than anyone else.

In [Opus 4’s system card](https://www-cdn.anthropic.com/07b2a3f9902ee19fe39a36ca638e5ae987bc64dd.pdf), Anthropic introduced a benchmark called “Claude Code Impossible Tasks”, which they used to measure [reward hacking](https://www.dbreunig.com/2025/04/11/what-we-mean-when-we-say-think.html#:~:text=A%20Reward%20Hacking%20Digression) among their models: would models admit defeat or would they try to game the system? An ideal model realizes a task is impossible and aborts, so Anthropic changed post-training rewards, environments, and feedback specifically to avoid reward hacking.

A later [system card](https://www-cdn.anthropic.com/9fa30625273bafdf5af82c93719d7ca606485a16.pdf) showed that a short anti-hacking instruction dropped Opus 4.1’s reward hacking from 52% to 18%. Opus was 65% less likely to try to game the system if you *simply asked it not to.* And Anthropic didn’t stop at prompting; it changed post-training rewards and monitoring to make reward hacking less advantageous.

All the recent accidental hacking stories are troubling. But they’re the product of our chosen designs. This doesn’t make these incidents less alarming. The problem is the stories we’re telling about the incident and the questions we’re asking. When an agent “goes rogue”, don’t start by asking what the model wanted. Ask what people trained it to do, what they rewarded, what instructions were given, what harness was provided, and what they failed to constrain.
