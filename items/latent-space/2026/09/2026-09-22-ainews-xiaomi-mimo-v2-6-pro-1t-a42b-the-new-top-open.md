---
title: '[AINews] Xiaomi MiMo-V2.6-Pro 1T-A42B: the new top Open Weights model, trained for $3M'
link: https://www.latent.space/p/ainews-xiaomi-mimo-v26-pro-1t-a42b
source: latent-space
published: 2026-09-22T06:30:56Z
updated: 2026-09-22T06:30:56Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: crowning a new Chinese frontier lab
content: extracted
html: 2026-09-22-ainews-xiaomi-mimo-v2-6-pro-1t-a42b-the-new-top-open.html
preview:
  file: 2026-09-22-ainews-xiaomi-mimo-v2-6-pro-1t-a42b-the-new-top-open.preview-538fe7d90d7a.webp
  width: 256
  height: 242
  color: '#eaecec'
images:
- source: https://substackcdn.com/image/fetch/$s_!qfA0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fmedia%2FHSxCs7wa0AANR91.jpg
  original:
    file: 2026-09-22-ainews-xiaomi-mimo-v2-6-pro-1t-a42b-the-new-top-open.image-96fdffbddc14.jpg
    width: 1200
    height: 1134
  color: '#fcfcfc'
- source: https://substackcdn.com/image/fetch/$s_!ZnE9!,w_120,h_120,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F2042402069320290304%2FA8C1lP07.jpg
  original:
    file: 2026-09-22-ainews-xiaomi-mimo-v2-6-pro-1t-a42b-the-new-top-open.image-64eeb0359d9f.jpg
    width: 120
    height: 120
  color: '#8842fc'
- source: https://substackcdn.com/image/fetch/$s_!3_JT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2e1163e-d6ca-41dc-93b6-65900faa37d1_1508x900.png
  original:
    file: 2026-09-22-ainews-xiaomi-mimo-v2-6-pro-1t-a42b-the-new-top-open.image-bcc97e1ef496.jpg
    width: 1456
    height: 869
  color: '#fbfbfb'
- source: https://substackcdn.com/image/fetch/$s_!zbvL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf7a24ce-df99-410f-85ef-d4f83c7649c3_1542x818.png
  original:
    file: 2026-09-22-ainews-xiaomi-mimo-v2-6-pro-1t-a42b-the-new-top-open.image-66d098381894.jpg
    width: 1456
    height: 772
  color: '#fcfcfc'
---

*Meet Xiaomi and other top Chinese frontier labs at [AIE Shanghai](https://ai.engineer/shanghai)!*

This is a first for the “Apple of China” phone maker-turned-frontier lab: “*The MiMo-V2.6 series includes two **natively omnimodal** models: **MiMo-V2.6-Pro** is our most capable model to date, while **MiMo-V2.6-Flash** strikes the best balance between intelligence, efficiency, and cost. We are also rolling-out **MiMo-V2.6-Pro-UltraSpeed, delivering up to 20x faster output speed** at the same quality, for users who require extreme generation speed.”*

> Artificial Analysis @ArtificialAnlys
>
> MiMo-V2.6-Pro debuts as the top open weights model on the Artificial Analysis Intelligence Index (46). At $0.13 per Intelligence Index task, it lands on the Intelligence vs. Cost per Task Pareto frontier @Xiaomi has just released MiMo-V2.6-Pro, an open weights model with major …
>
> [@ArtificialAnlys on X](https://x.com/ArtificialAnlys/status/2102128560962187701)

Xiaomi is not traditionally considered one of the [six Chinese AI Tigers](https://en.wikipedia.org/wiki/AI_tiger), so it is very surprising to the established order of names you have come to know and love. And… it is natively omnimodal!

[![](https://substackcdn.com/image/fetch/$s_!3_JT!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2e1163e-d6ca-41dc-93b6-65900faa37d1_1508x900.png)](https://substackcdn.com/image/fetch/$s_!3_JT!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2e1163e-d6ca-41dc-93b6-65900faa37d1_1508x900.png)

Xiaomi made news a few days ago when [Fuli Luo, a former DeepSeek star engineer now at Xiaomi](https://x.com/_LuoFuli/status/2100296686719610932), started [publishing their final RL training runs](https://mimo.xiaomi.com/rl/) live, which showed an [abnormal amount of transparency in their internal metrics](https://x.com/eliebakouch/status/2100316319459500128).

As they note in their [technical report](https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Pro-RL/blob/main/MiMo_V2_6_technical_report.pdf), they scaled RL compute along three axes:

1. **Larger batches and higher throughput**: large batches on a fully asynchronous architecture, with 1,568 samples per update, training at up to 1M context length, and 3.5 to 3.7B tokens per step.

2. **More tasks and richer environments**: a multi-task training suite spanning coding, general agents, visual and cyber, mixed across several harnesses so that gains in one capability reinforce the others.

3. **More grader compute**: relative comparison within each group gives long-horizon RL tasks more precise and more diverse reward signals, closes a self-improvement loop, and steers the model toward shorter paths and fewer tokens per task.

[![](https://substackcdn.com/image/fetch/$s_!zbvL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf7a24ce-df99-410f-85ef-d4f83c7649c3_1542x818.png)](https://substackcdn.com/image/fetch/$s_!zbvL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf7a24ce-df99-410f-85ef-d4f83c7649c3_1542x818.png)

**ALL** of this tooling, including the environments, will be open sourced.- the **environment code and training recipes**, but the complete **7k+ task datasets** have not yet been released.

- **Coding / software engineering:** [Code recipes, dataset loader and rewards](https://github.com/XiaomiMiMo/verl/tree/mimo-oss/recipes/code)

- **Cyber / vulnerability reproduction:** [ARVO environment and training recipe](https://github.com/XiaomiMiMo/verl/tree/mimo-oss/recipes/arvo)

- **General / knowledge work:** [General environment, tools and training recipe](https://github.com/XiaomiMiMo/verl/tree/mimo-oss/recipes/general)

- **Visual / web development:** [Web-development environment and grading](https://github.com/XiaomiMiMo/verl/tree/mimo-oss/recipes/design/webdev)

- **Music generation:** [Data preparation and music scorer](https://github.com/XiaomiMiMo/verl/tree/mimo-oss/recipes/design/music)

- **Composable mini-harnesses:** [Agent configurations](https://github.com/XiaomiMiMo/verl/tree/mimo-oss/config/agent)

- **Shared environment adapters:** [mimoagent environments](https://github.com/XiaomiMiMo/mimoagent/tree/mimo-oss/src/mimoagent/environments)

> AI News for 9/19/2026-9/21/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**Open Models, Competition, and the China Gap**

- **Open models remain the central policy and market story**: [Nathan Lambert](https://x.com/natolambert/status/2102006127877660735) shared a congressional briefing on open-model performance, adoption, and U.S.-China competition, followed by a [public summary](https://x.com/natolambert/status/2102035165660770730). The broader argument resurfaced elsewhere: [@Yuchenj\_UW](https://x.com/Yuchenj_UW/status/2102082682603925935) claims frontier coding capability has plateaued since **Opus 4.8**, while open-source models keep closing the gap at **10–50x lower cost**; [@ClementDelangue](https://x.com/ClementDelangue/status/2102085295650828487) similarly argues APIs are overkill for many real-world use cases and that specialized models will take share. Counterpoint: [@teortaxesTex](https://x.com/teortaxesTex/status/2102089221066354801) argues frontier has actually split into new higher tiers, with internal models and top closed models still well ahead.

- **The release cadence from Chinese labs is now difficult to dismiss**: [@Thom\_Wolf](https://x.com/Thom_Wolf/status/2102123398230954053) compiled an unusually dense **\~10-week run** of open releases including **Kimi K3, Qwen3.8-Max, DeepSeek V4-Pro, GLM-5.3, Hy4 Preview, Atria Dawn**, and more. This is reinforced by a Bloomberg-sourced note via [@Polymarket](https://x.com/Polymarket/status/2102076673835364449) that startups are increasingly building custom models on open weights to cut cost and reduce dependence on OpenAI/Anthropic. The subtext across several tweets: open-weight capability is no longer confined to midsized models; multiple teams are shipping **frontier-scale MoEs** with credible cost-performance stories.

**Xiaomi MiMo-V2.6 and RL as the New Scaling Lever**

- **MiMo-V2.6 is the biggest open-model release in the set**: [@XiaomiMiMo](https://x.com/XiaomiMiMo/status/2102138559952290106) launched **MiMo-V2.6 Pro and Flash**, described as open omnimodal models with weights, technical report, RL environments, and training code. [Artificial Analysis](https://x.com/ArtificialAnlys/status/2102128560962187701) says **MiMo-V2.6-Pro** debuts as the top open-weights model on its **Intelligence Index (46)**, with **1.02T total / 42B active** parameters and strong cost efficiency at **$0.435/M input** and **$0.87/M output** tokens. [@victormustar](https://x.com/victormustar/status/2102130481676395003) notes the models are under **MIT license**.

- **What stood out technically was not just the model, but the RL stack**: [@eliebakouch](https://x.com/eliebakouch/status/2102045988143710664) highlighted Xiaomi’s environment/data-factory paper for generating RL tasks from open repositories with “agents in the loop” for robustness and anti-cheating. Later commentary points to a second paper and unusually high transparency: [@xeophon](https://x.com/xeophon/status/2102128537276686594) notes Xiaomi wants to release **\~7K RL environments**, and [@eliebakouch](https://x.com/eliebakouch/status/2102136275708879078) emphasizes the team shipped model + tech report **less than a week after the final RL run**. A recurring interpretation, from [@bertgodel](https://x.com/bertgodel/status/2102139330127102120) and [@Thom\_Wolf](https://x.com/Thom_Wolf/status/2102137611011674173), is that **high-quality open RL environments** may now be as strategically important as pretraining corpora were in the last cycle.

- **RL cost/throughput details drew attention because they compress timelines**: [@zephyr\_z9](https://x.com/zephyr_z9/status/2102131465383551123) cites **130 hours**, **75B tokens**, and **$2.6M** for the RL run behind the result; [@tianjun\_zhang](https://x.com/tianjun_zhang/status/2102152886520291472) says the MiMo family scales RL on **JAX + TPU**, where scaling is “mostly a config change, not a code rewrite.” If these numbers hold up, the implication is that post-training/RL is becoming a far cheaper route to frontier-adjacent gains than many assumed.

**Decision Models, Jev, and the Return of Specialized Inference**

- **Jev was the dominant product/theme discussion**: Multiple posts converged on the same framing: this is “just” classification/routing, but with modern model intelligence and much better latency/cost. [@karpathy](https://x.com/karpathy/status/2102124533729955960) calls it a point on the Pareto frontier for **“no thinking, single token, low latency acceptable intelligence”**. [@willdepue](https://x.com/willdepue/status/2102070249453469823) describes it as a **zero-shot classifier with frontier-ish intelligence**, while [@ClementDelangue](https://x.com/ClementDelangue/status/2102071917926613310) argues the excitement shows there is large latent demand for specialized models rather than ever-larger generalists.

- **The ecosystem around Jev expanded quickly**: [@sarah\_edo](https://x.com/sarah_edo/status/2102025642862600634) built a Chrome extension that uses Jev to select and fill relevant WebMCP tools per keystroke. [LangChain](https://x.com/LangChain/status/2102081155277246532) added **Jev-as-a-judge** to LangSmith; [@hwchase17](https://x.com/hwchase17/status/2102065131202945152) and [@Hacubu](https://x.com/Hacubu/status/2102064714851455363) pushed **SemIf**, an open-source decision model, through the LangSmith Gateway. [@omarsar0](https://x.com/omarsar0/status/2102066232383979749) reports using Jev to retag **\~2.3K papers** in **83 seconds for $0.14**, with **579** high-confidence changes and manual validation of disagreements.

- **The more durable takeaway is architectural**: [DSPyOSS](https://x.com/DSPyOSS/status/2102020195036381245) argues that asking frontier agents is like managing people, while hand-writing decision-model programs is analogous to writing assembly; both extremes are useful, but brittle if overused. Several posts emphasized where these models fit best: routing, approval gates, trace scoring, tool selection, discrete document decisions, and low-cost supervision inside larger agent loops rather than as standalone “smart agents.”

**Inference, Tooling, and Systems Optimizations**

- **Tokenizer and post-training infra both got substantive upgrades**: Hugging Face’s [tokenizers v1 RC](https://x.com/LysandreJik/status/2102035784434061735) claims **up to 30x faster tokenization**, improved multithread scaling, lower memory use, and much smaller package size; [@art\_zucker](https://x.com/art_zucker/status/2102031477148139838) framed it as a new SOTA tokenization library. Separately, [Halo](https://x.com/whitecircle/status/2102087563913609534) launched as a post-training framework claiming **up to 2.8x throughput** over stock TRL while keeping models in native Hugging Face format.

- **Inference-side engineering remains a major lever**: [@RisingSayak](https://x.com/RisingSayak/status/2102008307598909734) showed how KV caching is incorporated into **QwenImage 2.1**, separating fixed context from changing image positions and yielding a **2.55x speedup**; the thread cites **50.57s → 19.86s** DiT time on a warmed A100 with moderate memory overhead. [vLLM](https://x.com/vllm_project/status/2102051703579373706) published tuned serving configs for **Qwen3.8-2.4T** on **GB300 NVL72**, showing a Pareto frontier from **5K total tok/s/GPU** at high throughput to **180 output tok/s/user** at low latency. In video workloads, [vLLM](https://x.com/vllm_project/status/2102142730134814943) also integrated **PyNvVideoCodec/NVDEC**, removing CPU decode bottlenecks and reporting **2x+ throughput** at **8×H100**.

- **Compression/quantization is still moving fast**: [@ZhihuFrontier](https://x.com/ZhihuFrontier/status/2102039001129750565) summarized Tencent Hunyuan’s engineering behind packing **Hy4 Preview (770B)** into **214 GiB** via mixed-precision quantization averaging **\~2.38 bits/weight**, including custom CUDA kernels in patched llama.cpp. On the edge/local side, [@vikhyatk](https://x.com/vikhyatk/status/2102057813224956186) released **Parakeet Redux**, compressing NVIDIA’s speech model from **1.2GB to 178MB**, running at **113x realtime on CPU**, while beating the base model on **25-language FLEURS** and staying within **0.3 WER** on English.

**Agents, Security, and Human-in-the-Loop Control**

- **Computer-use systems are becoming more productionized, but security is now central**: [Patrick Wardle](https://x.com/patrickwardle/status/2102045926474785265) reported a serious local-hijack flaw in **Muse**, arguing broad OS access makes such assistants a high-value attack surface. In contrast, [DeepLearningAI](https://x.com/DeepLearningAI/status/2102070383398502862) highlighted Meta’s design philosophy for Muse-like agents: assume prompt injection will happen, keep real credentials away from the model, isolate tools in containers, and use an independent outbound-call gatekeeper.

- **Commercial agents are also being pushed deeper into workflows**: [Cognition](https://x.com/cognition/status/2102104259219406886) introduced **Devin Cloud in Terminal** and **devin ssh**, making the model’s VM directly accessible from the CLI and allowing handoff between Devin and the user’s machine. [GitHub Copilot](https://x.com/gimenete/status/2102063244491858051) teased **editable diffs** in the desktop app, while [@pierceboggan](https://x.com/pierceboggan/status/2102119009982497060) showed a Sentry-integrated canvas for moving from crash report to fix.

- **A recurring systems point: inference and agent infra are shifting toward test-time compute**: [@sarahookr](https://x.com/sarahookr/status/2102024028047437840) predicts compute moving from pretraining—where marginal FLOPs yield less—to **test-time compute**, requiring “very different infrastructure.” That theme also showed up in persistent-cache discussions for local serving, e.g. [@TheZachMueller](https://x.com/TheZachMueller/status/2102068537221152878) on SGLang’s multi-level **hiCache** (GPU/RAM/disk) for preserving KV cache across model swaps and restarts.

**Top tweets (by engagement)**

- **Grok 4.7 release**: [SpaceXAI](https://x.com/SpaceXAI/status/2102069815225586149) announced **Grok 4.7**, described as a notable improvement over 4.6 at the same price/speed. Follow-on evals were mixed: [Artificial Analysis](https://x.com/ArtificialAnlys/status/2102074909623271513) reported **56** on its Coding Agent Index with gains on DeepSWE/Terminal-Bench/SWE-Atlas-QnA, while [Vals](https://x.com/ValsAI/status/2102086608476590432) saw it rank **#24** on its Vals Index, **down 5 points** from Grok 4.6 despite gains in legal/medical.

- **OpenAI’s automated model-training workflow**: A widely shared summary from [@wallstengine](https://x.com/wallstengine/status/2102047257784881556) reports that OpenAI has largely automated parts of training experimental models, including GPU kernel writing and code optimization, with internal agents collaborating and compressing some experiments from years to about a week.

- **OpenAI mathematics advisory group and claims of solved open problems**: [OpenAI](https://x.com/OpenAI/status/2102093145051943229) announced an independent advisory group of mathematicians to guide assessment and communication of AI advances in mathematics. Attention then shifted to the stronger claim, amplified by [@AndrewCurran\_](https://x.com/AndrewCurran_/status/2102121553211412975) and others, that an internal OpenAI model has resolved **100+ long-standing open problems** across mathematics. This was among the most consequential but least independently evaluated items in the set.

- **Open-sourcing of valuable data assets**: [@ClementDelangue](https://x.com/ClementDelangue/status/2102046770947613026) highlighted **Eidon AI** open-sourcing **1,274 hours** of egocentric robotics data (**13,451 recordings**) as a rare case of a startup preserving impact for the community after shutdown.

- **[Qwen-Image-2.1 released!](https://www.reddit.com/r/LocalLLaMA/comments/1wlgrft/qwenimage21_released/)** (Activity: 2485): **Qwen-Image-2.1 was released with open weights as a unified 7B image generation/editing model, positioned as a faster, lower-cost member of the Qwen-Image series ( [blog](https://qwen.ai/blog?id=qwen-image-2.1), [GitHub](https://github.com/QwenLM/Qwen-Image-2.1), [Hugging Face](https://huggingface.co/Qwen/Qwen-Image-2.1)). Key technical additions include native RGBA/transparent image generation and editing, support for up to 10 reference images, multi-image inference acceleration, and localized edit control for tasks like object removal, attribute changes, product/portrait-preserving edits, panoramas, infographics, typography, and virtual try-ons.** Comments primarily highlight the native transparency pipeline and local-edit interface; one example uses colored circles to target three regions simultaneously for removal, hair recoloring, and clothing replacement, suggesting interest in more controllable multi-region editing workflows.

  - **Qwen-Image-2.1** is reported to add *native transparent image generation* and transparent-image editing support, which is technically notable because alpha-channel workflows are often handled as post-processing or masking rather than directly by the image model. The linked example shows transparent-output capability: [https://preview.redd.it/59fu834idoqh1.png?width=767&format=png&auto=webp&s=5fb81b135b35dac70f9d38a9995d7c1a7a2877dd](https://preview.redd.it/59fu834idoqh1.png?width=767&format=png&auto=webp&s=5fb81b135b35dac70f9d38a9995d7c1a7a2877dd)

  - The model appears to support **multi-region local editing via visual annotations**, where circled regions can be referenced in the prompt and edited simultaneously. One example asks it to *“remove the metal watch in the blue circle, change the hair in the red circle to black, and replace the area in the green circle with gray short-sleeved linen pajamas,”* demonstrating combined object removal, attribute modification, and region replacement in a single edit pass: [https://preview.redd.it/cvh09tyvdoqh1.jpeg?width=1242&format=pjpg&auto=webp&s=32077f7420def5bec85160e2e982d6aef5efce54](https://preview.redd.it/cvh09tyvdoqh1.jpeg?width=1242&format=pjpg&auto=webp&s=32077f7420def5bec85160e2e982d6aef5efce54)

  - Several commenters highlight the model size: **Qwen-Image-2.1 is described as** `7B` **parameters**, which is significantly smaller than prior Qwen image models that commenters say were **over** `20B`. This size reduction is viewed as important for local inference feasibility, with one user specifically noting interest from the perspective of a **16GB VRAM GPU** such as the RTX 5060 Ti 16GB.
- **[Clarification on the Qwen-image-2.1 license](https://www.reddit.com/r/LocalLLaMA/comments/1wm4o8x/clarification_on_the_qwenimage21_license/)** (Activity: 948): **The image is a non-meme screenshot of a [Qwen Developers X post](https://i.redd.it/36k6lzy2gtqh1.jpeg) clarifying that Qwen-Image-2.1 outputs are not considered licensed “Materials”, so users retain rights to generated images/content. This matters because the model license reportedly still contains a non-commercial restriction on use of the Materials, creating ambiguity over whether commercial image generation is allowed even if generated outputs are user-owned.** Commenters welcomed the clarification, with one user saying Qwen-Image-2.1 “easily beats all current Flux models.” Another noted they can run it locally via **ComfyUI int8** on a `16 GB RTX 5060 Ti` peaking around `15.2 GB` VRAM, but warned the Hugging Face LICENSE file may not yet reflect the clarified intent.

  - A commenter reports running **Qwen-Image-2.1 locally** in **ComfyUI** using `int8` quantization on a **16 GB RTX 5060 Ti**, with VRAM peaking around `15.2 GB`. They describe the model as suitable for local testing but note that licensing uncertainty around generated outputs was the main blocker for broader/client use.

  - Several commenters highlight a legal/implementation mismatch: the **Hugging Face README** was apparently clarified, but the actual **LICENSE** file still contains Section `2(b)` language prohibiting commercial “use” of the Materials. One user emailed `model-business@notice.qwencloud.com` asking whether the license text will be updated, because the tweet/README intent may not be sufficient for client or commercial work.

  - The key technical/legal distinction being debated is whether “commercial use not allowed” applies only to **serving, redistributing, or monetizing the model/materials**, versus also restricting **outputs generated by the model**. Commenters argue that until the canonical license file is updated, downstream users comparing it with permissive **Apache-2.0/MIT-style** model licenses may reasonably avoid commercial workflows despite the clarification.
