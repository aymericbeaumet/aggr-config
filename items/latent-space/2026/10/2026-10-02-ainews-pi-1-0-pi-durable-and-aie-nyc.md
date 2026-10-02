---
title: '[AINews] Pi 1.0, Pi Durable, and AIE NYC'
link: https://www.latent.space/p/ainews-pi-10-pi-durable-and-aie-nyc
source: latent-space
published: 2026-10-02T06:40:53Z
updated: 2026-10-02T06:40:53Z
first_seen: 2026-10-02T08:58:34.290726633Z
summary: the minimalist harness goes stable... and TypeScript!
content: extracted
html: 2026-10-02-ainews-pi-1-0-pi-durable-and-aie-nyc.html
preview:
  file: 2026-10-02-ainews-pi-1-0-pi-durable-and-aie-nyc.preview-1f170d87ac2c.webp
  width: 256
  height: 144
  color: '#262222'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/RjfbvDXpFls
  original:
    file: 2026-10-02-ainews-pi-1-0-pi-durable-and-aie-nyc.image-a5737b38b777.jpg
    width: 728
    height: 410
  color: '#070709'
---

***Last call for regular tickets for [AI Engineer NYC](https://ai.engineer/nyc)!** See you in 2 weeks!*

*As an exclusive for Latent Space subscribers, the first 30 of you can [take a 30% off code](https://app.ai.engineer/e/ai-engineer-new-york-2026?discount=LS-NYC26) if it helps (for new tickets only, no refunds).*

Pi is often mentioned in the same breath as OpenClaw, as [we did earlier this year](https://www.latent.space/p/ainews-sci-fi-with-a-touch-of-madness?utm_source=publication-search):

[www.youtube.com](https://www.youtube.com/watch?v=RjfbvDXpFls)

 but today is time for the increasingly well regarded [Earendil](https://www.youtube.com/watch?v=_Zcw_sVF6hU&pp=0gcJCS4MAYcqIYzv), which Pi joined, to have its day in the sun, with both [Pi 1.0](https://news.ycombinator.com/item?id=49926069) and [Pi Durable](https://news.ycombinator.com/item?id=49925969) hitting the front page of HN.

**[Pi 1.0](https://earendil.com/posts/pi-1-0/):**

- [Codemode](https://earendil.com/posts/you-said-no-mcp/) (native support for MCP, Jev and image models)

- Extension support for [virtual models](https://github.com/earendil-works/pi/releases)

- Deferred tool loading

- Cache warming for anthropic models

- [Mid-conversation system messages](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/session-format.md) (transcript-aware prompt and tool changes)

- A new TUI theme

- Full-screen mode by default

**[Pi Durable](https://earendil.com/posts/pi-durable/)** ports Pi to TypeScript and externalizes all stateful components of Pi:

- **Crash Survival:** Every step is recorded as a checkpointed task. If a process fails or restarts, agents and subagents automatically resume from their last exact state.

- **Portability:** It runs anywhere with a JavaScript runtime (like Node, Bun, or Cloudflare) and uses pluggable storage backends (Memory, SQLite, JSONL) and flexible remote or local execution environments.

- **Concurrency:** A single harness can run multiple parallel, branching conversations—such as a main channel and separate threads—without blocking one another.

- **Extensibility:** Developers can bundle custom system prompts, tools, hooks, and durable tasks (e.g., multi-step checkout processes with rollback capabilities) into installable “Extensions.”

- **Context Management:** Automatic background compaction summarizes older messages to maintain token limits without pausing the agent’s active work.

- **Multiplayer & State Sync:** Application state (like a to-do list) is stored in documents directly alongside the conversation transcripts, allowing multiple users or UIs to connect, watch, and steer the same agent simultaneously.

- **Hot-Swapping:** Tool and extension code can be updated dynamically while the agent is running, with the next tool call automatically picking up the new code.

> AI News for 10/01/2026-9/30/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**Frontier and Multimodal Launches: Gemini 4 Argon, GPT-6.1 Sol and FLUX 3**

- **Gemini 4 Argon**: Google announced a new generation of Gemini, with contributors highlighting revised pretraining mixtures, long-horizon post-training data, and internal applications in memory optimization, code migration and mathematics. These are developer accounts of how the model was built and used—not independent evidence of general superiority ( [Google researcher](https://x.com/mirrokni/status/2105500370675921213)).

  - **Validation**: Google says new Gemini revisions now undergo weeks of testing by thousands of internal software engineers before release ( [Logan Kilpatrick](https://x.com/OfficialLoganK/status/2105521401566486875)).

  - **Contested readiness**: A circulated Bloomberg report attributed coding weaknesses to anonymous insiders; a subsequent post reported a senior DeepMind engineer rejecting that account. Treat the practical coding-quality dispute as unresolved, rather than interpreting either benchmarks or employee reactions as decisive ( [reported criticism](https://x.com/kimmonismus/status/2105570914574209283), [reported rebuttal](https://x.com/kimmonismus/status/2105709302484729879)).
- **GPT-6.1 Sol**: OpenAI’s update is primarily an efficiency story. Sam Altman called it the company’s fastest-growing model and said serving performance had improved after launch-time load problems ( [update](https://x.com/sama/status/2105688354834756036)).

  - **Measured economics**: Artificial Analysis reports $0.72 per Intelligence Index task at maximum effort, versus $1.04 for GPT-6 Sol and $3.26 for Astra. Fewer turns and cheaper cache reads—not simply fewer generated tokens—drive the improvement ( [results](https://x.com/ArtificialAnlys/status/2105491868608004578), [explanation](https://x.com/ArtificialAnlys/status/2105449959554441580)).

  - **Multimodal fix**: OpenAI also corrected image encoding for Luna and Sol. Luna gained one Intelligence Index point, including improvements on visual-document and knowledge-work evaluations; Sol changed negligibly ( [measurement](https://x.com/ArtificialAnlys/status/2105491868608004578)).
- **Solar Mini 4**: Upstage’s proprietary text-only reasoning model reports 35B total/3B active parameters, a 1M-token context window and 262K maximum output. Weights are not released, so parameter counts remain vendor-reported ( [analysis](https://x.com/ArtificialAnlys/status/2105459219059401036)).

  - **Pricing**: $0.10/$0.40/$0.01 per million input/output/cache-hit tokens.

  - **Trade-offs**: Artificial Analysis scores it 24 overall and 83% on long-context reasoning, but only 1% on Terminal-Bench 4.0. Despite 208 tokens/s output, approximately 88K output tokens per task produce a 7.1-minute average completion time and roughly five times Luna’s task cost.
- **FLUX 3 Image**: Black Forest Labs launched native generation up to 4K, up to ten reference images, bounding-box layout control and targeted multi-turn editing. Preserving every untouched pixel is a vendor capability claim, not independently established here ( [announcement](https://x.com/bfl_ai/status/2105734605621825738)).

  - **Availability**: Commercial weights are available; an open-weight variant is promised in coming weeks. Hosted access includes fal and Krea ( [fal](https://x.com/fal/status/2105745492474802514), [Krea](https://x.com/krea_ai/status/2105769469880799716)).

  - **Pricing**: BFL announced a temporary 50% API discount through October 8, without supplying base prices in these posts ( [details](https://x.com/robrombach/status/2105765028460732816)).
- **Interactive video agents**: Tavus introduced Griffin, a video-to-video interaction model. It claims 48% of live participants mistook it for a human, versus under 3% for earlier systems; that result should not be generalized into an unrestricted “Turing test passed” conclusion without the test protocol ( [announcement](https://x.com/tavus/status/2105704169009246248)).

  - **Enterprise deployment**: Separately, Synthesia launched Sessions: conversational avatars for roleplay and survey interviews, extending its previous one-way training-video product ( [launch](https://x.com/synthesiaIO/status/2105591908659540474)).
