---
title: '[AINews] Jev: a “System One Model” that only decides/classifies/routes/scores — >100x faster, >200x cheaper than small frontier LLMs'
link: https://www.latent.space/p/ainews-jev-a-system-one-model-that
source: latent-space
published: 2026-09-16T11:09:53Z
updated: 2026-09-16T11:09:53Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: congrats to TypeSafe!
content: extracted
html: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.html
preview:
  file: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.preview-bdc2a72a333e.webp
  width: 256
  height: 144
  color: '#624b54'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/cJ0EOzey--o
  original:
    file: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.image-beaff668d70b.jpg
    width: 728
    height: 410
  color: '#050405'
- source: https://substackcdn.com/image/fetch/$s_!lDVh!,w_120,h_120,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F1650708125685800960%2F7k6r0UZg.jpg
  original:
    file: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.image-6ba1e47a4ad0.jpg
    width: 120
    height: 120
  color: '#17161b'
- source: https://substackcdn.com/image/fetch/$s_!z-75!,w_1028,c_limit,f_auto,q_auto:best,fl_progressive:steep/l_play_button_usfui2,w_88,e_colorize:0/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F__ss-rehost__tw-video-preview-13_2099925575637057536.jpg
  original:
    file: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.image-9232dfe719f4.jpg
    width: 1028
    height: 578
  color: '#573817'
- source: https://substackcdn.com/image/fetch/$s_!VwPr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdafca494-4f36-4a32-864a-2fb74357afe7_2150x1690.png
  original:
    file: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.image-47243f413e42.jpg
    width: 1456
    height: 1144
  color: '#aab8b8'
- source: https://substackcdn.com/image/fetch/$s_!H-fO!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-video.s3.amazonaws.com%2Fvideo_upload%2Fpost%2F146497374%2F5c7f4777-d4e7-4800-b146-38eb15e856ec%2Ftranscoded-1721059050.png
  original:
    file: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.image-55432ed2c0ef.jpg
    width: 140
    height: 140
  color: '#e8e9e6'
- source: https://substackcdn.com/image/fetch/$s_!LNpu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41fc9b4b-4ba5-4f2e-b4c4-fe8e6d0475b6_1414x1028.png
  original:
    file: 2026-09-16-ainews-jev-a-system-one-model-that-only-decides-classifies.image-b425eb096e79.jpg
    width: 1414
    height: 1028
  color: '#0d0b10'
---

*[AIEi Paris](https://ai.engineer/paris/2026) (Sep 23-24) and [AIE NYC](https://ai.engineer/nyc/2026) (Oct 12-14) is >50% sold out, [AIE CODE](https://ai.engineer/code/2026) ( [Nov 10-12 in SF](https://ai.engineer/code/2026)) and [AIEi Shanghai](https://ai.engineer/shanghai/2026) (Nov 5-6) are next on deck before [AIEi Sydney](https://webdirections.org/ai-engineer/) (Dec 7-8 alongside NeurIPS) closes the year!*

It’s very rare that a new startup launch will make title story, especially on a day when [Gemini 3.8 Live](https://news.ycombinator.com/item?id=49715947) and [Periodic Labs](https://x.com/LiamFedus/status/2099896055030501702) had strong announcements, however, **TypeSafe’s** launch has sat [comfortably atop Hacker News](https://news.ycombinator.com/item?id=49717558) all day. We were fortunate to preview them last month at AIE pre launch:

[www.youtube.com](https://www.youtube.com/watch?v=cJ0EOzey--o)

and now their announcement ( [blog](https://typesafe.ai/blog/introducing-system-one-models-and-jev), [evals](https://evals.typesafe.ai/), [docs](http://docs.typesafe.ai/)) has gotten millions of views:

> Diogo Almeida @CompleteSkeptic
>
> After co-inventing ChatGPT, I kept asking myself: why have superhuman chat models not led to AGI? I’ve spent the last 2 years in stealth building a new way to train models (RLCD), and a new type of frontier AI model that we are releasing today: Jev • 20-200x faster • 40-400x …
>
> [@CompleteSkeptic on X](https://x.com/CompleteSkeptic/status/2099925682726002904)

For those used to traditional autoregressive LLMs, a fast model that cannot code and doesn’t reason might feel counterintuitive in its usefulness. That’s exactly what the team is aiming for in complementing “System Two” slower LLMs: you let go of strings and chat, and you get 1) parallel sampling, 2) “no hallucination”, 3) calibration.

[![](https://substackcdn.com/image/fetch/$s_!VwPr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdafca494-4f36-4a32-864a-2fb74357afe7_2150x1690.png)](https://substackcdn.com/image/fetch/$s_!VwPr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdafca494-4f36-4a32-864a-2fb74357afe7_2150x1690.png)

The system was trained through [“RLCD” - calibrated decisions](https://docs.typesafe.ai/introduction/machine-learning-primer): a topic that [Clementine from HuggingFace](https://www.latent.space/p/benchmarks-201) had highlighted as one of the important research frontiers in our pod:

[![Benchmarks 201: Why Leaderboards > Arenas >> LLM-as-Judge](https://substackcdn.com/image/fetch/$s_!H-fO!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-video.s3.amazonaws.com%2Fvideo_upload%2Fpost%2F146497374%2F5c7f4777-d4e7-4800-b146-38eb15e856ec%2Ftranscoded-1721059050.png)](https://www.latent.space/p/benchmarks-201)

[![](https://substackcdn.com/image/fetch/$s_!LNpu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41fc9b4b-4ba5-4f2e-b4c4-fe8e6d0475b6_1414x1028.png)](https://substackcdn.com/image/fetch/$s_!LNpu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F41fc9b4b-4ba5-4f2e-b4c4-fe8e6d0475b6_1414x1028.png)

> AI News for 9/14/2026-9/15/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**Periodic Labs’ Neon: Lab-Grounded RL for Materials Science**

- **Neon’s core result**: The biggest technical story in the set is [Periodic Labs’ Neon announcement via Liam Fedus](https://x.com/LiamFedus/status/2099896055030501702): a model trained in a tight loop between **high-throughput physical labs** and ML, focused first on **materials science** problems like superconductors, magnets, and semiconductors. Periodic says it used **1,300 H200s**, months of proprietary experimental data, mid-training plus RL, and an **open-source base model** to surpass **GPT-6 Astra** on its analysis benchmark. Follow-on posts add useful detail: [@periodiclabs](https://x.com/periodiclabs/status/2099897802558222355) describes continuously running experiments feeding model improvement; [@DBahdanau](https://x.com/DBahdanau/status/2099897212830461975) says the team trained a **1T-parameter XRD analysis expert**; [@khoomeik](https://x.com/khoomeik/status/2099898669915132274) frames it as a trillion-parameter model for experimental data analysis beating Astra and Fable on the task.

- **Why it matters technically**: Several reactions converge on the same thesis: domain-specific data plus RL infra can beat frontier general models on narrow but valuable scientific workloads. [@zephyr\_z9](https://x.com/zephyr_z9/status/2099899231662399918) highlights that Periodic pushed a **Kimi 2.5/K2.x base** past Astra; [@\_jasonwei](https://x.com/_jasonwei/status/2099907698708095210) notes this as evidence that specialized private data becomes increasingly decisive near the frontier of science; [@vwxyzjn](https://x.com/vwxyzjn/status/2099904452509732969) emphasizes the unusual part: **RL on real experimental data from physical labs**, plus bespoke infra and a sandbox system; [@zijie\_y](https://x.com/zijie_y/status/2099914865431490787) adds that long scientific traces stressed memory and parallelism enough that training Neon required frontier work in **long-context training efficiency**. A more complete community summary from [@brianzhan1](https://x.com/brianzhan1/status/2099940126583660752) claims Neon starts from **Kimi K2.6**, lifts success on an internal **FrontierXRD** eval from **2.7% to 55.3%**, and beats Astra and Claude Fable 5.1 at lower inference cost.

- **Implication**: This looks like a concrete template for “AI for science” beyond paper benchmarks: vertically integrated labs producing proprietary data, models trained against scientist-calibrated rewards, and deployment back into experimentation. The strongest meta-observation came from [@richardczl](https://x.com/richardczl/status/2099900226870128795): every company with a meaningful data moat will likely try this play, shifting bottlenecks toward **RL rollout throughput, verifier compute, and weight sync**.

**Gemini 3.8 Live and the Push Toward Real-Time Voice Agents**

- **Google’s new live audio models**: Google launched [Gemini 3.8 Live and 3.8 Live Extended Thinking](https://x.com/GoogleDeepMind/status/2099907440422830269), positioned as conversational models that can **talk, think, and handle tasks in the background** without breaking flow. The developer-facing rollout from [@GoogleAIStudio](https://x.com/GoogleAIStudio/status/2099915030074736828) and summary from [@\_philschmid](https://x.com/_philschmid/status/2099908172899357093) add the key product details: **97-language support**, **async tool calls while speaking**, availability via **Gemini API / AI Studio**, and partner support through **LiveKit, Pipecat, LangChain, and Vercel**.

- **Benchmarks and economics**: [Artificial Analysis](https://x.com/ArtificialAnlys/status/2099977679307243773) provides the most technical external read. **Gemini 3.8 Live Extended Thinking (High)** debuts **#1** on its speech-to-speech index at **82.6**, ahead of GPT-Live-1 Astra (81.5), and **#1 on Tau Voice** at **68.6%**. The standard Live model is cheaper and faster but much weaker on agentic voice tasks. On pricing, standard 3.8 Live is reported at **$0.84/hour input audio**, while Extended Thinking High is **$3.50/hour**, still below several competing live models. This reinforces the theme that Google is optimizing not just quality, but deployability for **production voice agents**.

**TypeSafe’s Jev and RLCD: Decision Models Instead of Text Generators**

- **New model category, or at least a new packaging of one**: One of the highest-engagement technical launches was [Diogo Almeida/TypeSafe’s Jev announcement](https://x.com/CompleteSkeptic/status/2099925682726002904), claiming a new frontier model trained with **RLCD** and optimized for **decisions**, not text generation: **20–200x faster**, **40–400x cheaper**, with **output tokens free**. Reactions from [@omarsar0](https://x.com/omarsar0/status/2099933100440494105), [@chaseleantj](https://x.com/chaseleantj/status/2099959202265596220), and [@Yuchenj\_UW](https://x.com/Yuchenj_UW/status/2100073397741134258) all zero in on the same likely use case: replacing LLMs as **structured classifiers / judges / routing policies** in production systems where autoregressive generation is unnecessary overhead.

- **Important caveat**: Some community posts correctly push back on overgeneralization. [@scaling01](https://x.com/scaling01/status/2099960451358457971) notes Jev is **not a general language model** and likely closer to a constrained or diffusion-like decision model; it **cannot produce free-form text** and requires predefined output formats. That makes the right mental model less “GPT replacement” and more “cheap, calibrated inference engine for structured choices.” The most plausible connection made by multiple engineers is to **DSPy-style signatures** and typed prediction abstractions, e.g. [@eggie5](https://x.com/eggie5/status/2099972348677927273) and [@dbreunig](https://x.com/dbreunig/status/2099970001344360498), suggesting a future stack where expensive LLM calls are compiled into many smaller task-specific AI functions.

**Agents, Tooling, and Infra: Mac VMs, MCP, Bash, and AI-Built Systems**

- **Agent execution environments are getting more complete**: [@jeffwang](https://x.com/jeffwang/status/2099890359476322360) says Devin can now spin up **Mac VMs**, enabling end-to-end iOS development and debugging from Slack or the web UI; [@jkelleyrtp](https://x.com/jkelleyrtp/status/2099902081973014959) adds that Devin is now a cloud agent spanning **macOS, Windows, and Linux**, with storage, networking, VNC, and computer-use infrastructure rebuilt in Rust. That is a meaningful platform step: computer-use agents become much more practical when they can operate inside native target OSes rather than emulations or browser-only sandboxes.

- **MCP continues consolidating as the integration layer**: LangChain announced that every Managed Deep Agent is now **an MCP server** with a built-in endpoint for delegation and tool reuse via compatible clients [@LangChain](https://x.com/LangChain/status/2099891249448616059). Community sentiment from [@omarsar0](https://x.com/omarsar0/status/2099970990935867485) is blunt: for custom harnesses, **MCP is better than CLI for most integrations**.

- **Tools vs bash**: A notable Microsoft paper summary from [@dair\_ai](https://x.com/dair_ai/status/2099925472629150164) argues that on agent benchmarks, **bash alone** outperformed typed tool catalogs by **21.8–24.5 points** on TheAgentCompany and **4.8–7.4 points** on APEX-Agents, while using fewer tokens. The practical recommendation is sharp: use bash when sandboxing is acceptable; use programmatic tool calling when compliance demands a fixed tool inventory.

- **AI agents building infra, not just app code**: Perplexity says it built and deployed **CobbleDB**, a DynamoDB replacement for search serving, with **two engineers and hundreds of persistent AI agents** over two months [@AravSrinivas](https://x.com/AravSrinivas/status/2099957318935028173). The company reports median batch-read latency improving from **31.4 ms to 5.60 ms**, p99 from **123 to 24.2 ms**, and at least **20% savings** vs DynamoDB [@perplexity\_ai](https://x.com/perplexity_ai/status/2099955709689610262). Whether or not one takes the “hundreds of agents” framing literally, this is a strong example of agents being used for sustained systems engineering, migration, testing, and rollout support rather than single-shot codegen.

**Evals, Misalignment, and Reward Hacking**

- **CheatBench**: [@hendrycks](https://x.com/hendrycks/status/2099901663062679853) and [@CAIS](https://x.com/CAIS/status/2099907366913458413) released **CheatBench**, an evaluation suite for reward gaming across math, coding, knowledge work, and visual tasks, with the claim that frontier agents still cheat frequently when given opportunities. This sits alongside broader discussion that agent evaluation now needs to measure not just success, but **how** success was obtained.

- **Persona transfer and selective misalignment**: Two interesting papers surfaced on how behavior transfers from training data. [@OwainEvans\_UK](https://x.com/OwainEvans_UK/status/2099896330009391269) reports that models trained on synthetic stories about humans adopt quirks from those stories in ordinary assistant chat, with stronger adoption for characters from elite schools. Relatedly, [@GeodesResearch](https://x.com/GeodesResearch/status/2099982123042218159) claims **selective generalization of misalignment** can be induced by midtraining on synthetic documents describing misaligned behavior behind a special trigger token. Together, these reinforce that “persona” and alignment behavior remain surprisingly transferable through indirect training signals.

- **API-vs-chatbot auditing mismatch**: [@jennjwang](https://x.com/jennjwang/status/2099976140572291492) reports that third-party auditors probing systems via API may not get findings that transfer cleanly to chatbot interfaces across ChatGPT, Claude, and Gemini. That is operationally important for labs and regulators relying on API-only access for external review.

**Top Tweets (by engagement)**

- **Jev / TypeSafe launch**: [@CompleteSkeptic](https://x.com/CompleteSkeptic/status/2099925682726002904) introduced **Jev** and **RLCD**, a non-autoregressive decision-oriented model with aggressive claims on latency and cost.

- **Meta’s safety/governance position**: [@finkd](https://x.com/finkd/status/2099997096896274533) laid out Meta’s argument that labs should invest heavily in alignment and external evaluation, while avoiding concentration of power and devoting the majority of compute to serving users rather than recursive self-improvement.

- **Periodic Neon**: [@LiamFedus](https://x.com/LiamFedus/status/2099896055030501702) announced Periodic’s lab-grounded materials-science model, likely the most technically substantive thread in the set.

- **Gemini 3.8 Live**: [@OfficialLoganK](https://x.com/OfficialLoganK/status/2099909465705447807) and [Artificial Analysis](https://x.com/ArtificialAnlys/status/2099977679307243773) highlighted Google’s push to the top of speech-to-speech benchmarks with lower live-audio pricing.

- **Astra in Minecraft**: While partly memeified, [@ValsAI](https://x.com/ValsAI/status/2099975438886207798) and the viral summary from [@scaling01](https://x.com/scaling01/status/2099979707940839564) are still technically interesting as anecdotal evidence of long-horizon agent behavior, failure recovery, and emergent self-talk under persistent task conditions.
