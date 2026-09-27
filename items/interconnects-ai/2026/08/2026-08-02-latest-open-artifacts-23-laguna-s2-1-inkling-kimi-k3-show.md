---
title: 'Latest open artifacts (#23): Laguna S2.1, Inkling, & Kimi K3 show the utility of open models on the Pareto frontier'
link: https://www.interconnects.ai/p/latest-open-artifacts-23-laguna-s21
source: interconnects-ai
published: 2026-08-02T13:01:40Z
updated: 2026-08-02T13:01:40Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Florian Brand
summary: Capacity to train strong models is proliferating.
content: extracted
html: 2026-08-02-latest-open-artifacts-23-laguna-s2-1-inkling-kimi-k3-show.html
preview:
  file: 2026-08-02-latest-open-artifacts-23-laguna-s2-1-inkling-kimi-k3-show.preview-2407a8f61173.webp
  width: 256
  height: 144
  color: '#75897f'
images:
- source: https://substackcdn.com/image/fetch/$s_!RSM6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7a71a4f9-5ac5-447e-82ff-899915627512_2048x1152.jpeg
  original:
    file: 2026-08-02-latest-open-artifacts-23-laguna-s2-1-inkling-kimi-k3-show.image-f40d29694063.jpg
    width: 2048
    height: 1152
  color: '#96bab8'
- source: https://substackcdn.com/image/fetch/$s_!7ZQ1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43fdd9f2-fa1f-4092-a362-eb66c5047d2a_3500x2500.png
  original:
    file: 2026-08-02-latest-open-artifacts-23-laguna-s2-1-inkling-kimi-k3-show.image-b82d350e3084.jpg
    width: 1456
    height: 1040
  color: '#fcfcfc'
- source: https://substackcdn.com/image/fetch/$s_!pSqt!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd076a897-cc02-4f80-81d8-f7969d4332b2_2066x970.png
  original:
    file: 2026-08-02-latest-open-artifacts-23-laguna-s2-1-inkling-kimi-k3-show.image-fc72a26652cd.jpg
    width: 1456
    height: 684
  color: '#fcfcfc'
- source: https://substackcdn.com/image/fetch/$s_!G3cH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febdf2641-6b32-4c0a-8c93-087c75280057_2166x1007.png
  original:
    file: 2026-08-02-latest-open-artifacts-23-laguna-s2-1-inkling-kimi-k3-show.image-e05732f0a286.jpg
    width: 1456
    height: 677
  color: '#fdfdfd'
---

Consolidation has been one of the paths that many astute observers predicted for the near-future of labs training models. It was labelled as inevitable, as training costs are increasing by orders of magnitude every year. Yet, as someone who in 2024 would’ve predicted consolidation really picking up come 2026 or 2027, where are we? We’re at a place where more companies are training strong models — easily investing hundreds of millions to billions of dollars in the total effort still — and an increasing number of organizations are releasing these models openly.

The demand for tokens is incredibly high, and likely to increase as models get more efficient and unlock more possible use cases. All of these labs we thought would need to consolidate are realizing that building token machines is a likely path to value, and more companies will identify that source of value over time.

The prime example is Thinking Machines — when they announced their company in February 2025, very few people would’ve put them in the bucket of an open models company, myself included. Now their open model finetuning service is making hundreds of millions in revenue per year and they’re releasing the best open-weight models built in the U.S.A. — ahead of the early leaders in NVIDIA with Nemotron and Arcee’s Trilogy.

[Share](https://www.interconnects.ai/p/latest-open-artifacts-23-laguna-s21?utm_source=substack&utm_medium=email&utm_content=share&action=share)

On the other side of the ecosystem is the sustained pace from the Chinese labs, with newer entrants like Xiaomi still accumulating mindshare in the broader AI economy. Having predicted consolidation for a long time, it now seems like a safer bet is to predict continued adoption, and try to imagine the role that open models play there. How much can revenue-share licenses like Kimi K3 stick? How much market share can open models take? We’re entering the decisive era.

This is one of the most packed recaps of open models we’ve ever had, we’re excited!

- **[Inkling](https://huggingface.co/thinkingmachines/Inkling)** by [thinkingmachines](https://huggingface.co/thinkingmachines): The first model from Thinking Machines is a 975B-A41B multimodal MoE that supports text, images, and audio as inputs and produces text as output. While it is not the strongest model among peers (in China) in its size class, it is positioned to be a great base for fine-tuning, e.g., through their commercial offering, Tinker. They also release [a smaller version](https://huggingface.co/thinkingmachines/Inkling-Small) (276B-A12B), which is really competitive for its size.

- **[Hy3](https://huggingface.co/tencent/Hy3)** by [tencent](https://huggingface.co/tencent): A 295B-A21B MoE from Tencent. It improves over its predecessor across all metrics. Most notable, however, is the license change: While the previous version (covered [in Artifacts 21](https://www.interconnects.ai/p/latest-open-artifacts-21-open-model)) used a custom and rather restrictive license, Tencent switched to Apache 2 for this release. The model was also able to proof [a 50 year old math problem](https://x.com/TencentHunyuan/status/2082655737541726636?s=20) (with a dedicated harness and Sol as a judge, although it is unclear how important the latter really is).

  [![](https://substackcdn.com/image/fetch/$s_!7ZQ1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43fdd9f2-fa1f-4092-a362-eb66c5047d2a_3500x2500.png)](https://substackcdn.com/image/fetch/$s_!7ZQ1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43fdd9f2-fa1f-4092-a362-eb66c5047d2a_3500x2500.png)

- **[Laguna-S-2.1](https://huggingface.co/poolside/Laguna-S-2.1)** by [poolside](https://huggingface.co/poolside): Poolside quickly rose out of nowhere to become a frequent guest at Artifacts, marking its third appearance in three consecutive months. S2.1 is a newly pre- and post-trained version of the 118B-A8B MoE that fits on a DGX Spark, which brought it a lot of attention. Poolside also adopted the OpenMDW license, which is an Apache 2.0-like free license but has better legal backing for AI models specifically. The company also goes into more detail in [its blog](https://poolside.ai/blog/introducing-laguna-s-2-1#what-changed-in-the-training-loop), which includes all the [evaluation trajectories](https://trajectories.poolside.ai/). This is a lot of transparency for an open model release!

- **[DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731)** by [deepseek-ai](https://huggingface.co/deepseek-ai): Just one day after OpenAI has dropped the prices of their [smallest model by 80%](https://openai.com/index/advancing-the-price-performance-frontier-with-gpt-5-6/), the whale dropped an update to their V4 Flash model, beating Luna at the pareto frontier. The bigger model is not updated yet, so it remains to be seen where it will land in terms of performance. For the initial V4 releases, the Flash version was the star of the show in terms of performance per parameter, while [Pro was rather underwhelming.](https://www.interconnects.ai/p/latest-open-artifacts-21-open-model)

  [![](https://substackcdn.com/image/fetch/$s_!pSqt!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd076a897-cc02-4f80-81d8-f7969d4332b2_2066x970.png)](https://substackcdn.com/image/fetch/$s_!pSqt!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd076a897-cc02-4f80-81d8-f7969d4332b2_2066x970.png)

- **[Kimi-K3](https://huggingface.co/moonshotai/Kimi-K3)** by [moonshotai](https://huggingface.co/moonshotai): This is the biggest open model release in some time, and we covered it in [a separate post](https://www.interconnects.ai/p/kimi-k3-the-open-weights-escalation) and a [podcast episode](https://www.interconnects.ai/p/open-models-recap-more-on-kimi-k3). It was released under a noncommercial license, requiring inference and fine-tuning providers to enter into a commercial agreement. Kevin Xu and Graham Webster [argue in a post](https://interconnect.substack.com/p/kimis-tightrope-openness-and-revenue) that these licenses enable potential future government action against US entities doing business with Chinese AI companies:

> *But if a US company needs a contract with Moonshot to provide the inference tokens that Kimi K3 generates, the picture looks different. Some of the policy tools US officials and others have debated as potential levers to restrict Chinese open model use would more clearly apply.*

- **[LongCat-2.0](https://huggingface.co/meituan-longcat/LongCat-2.0)** by [meituan-longcat](https://huggingface.co/meituan-longcat): The Chinese DoorDash is back again. This time, the company released another big MoE with 1.6T parameters. While the model itself is not the most capable for its size beyond benchmarks, it was trained entirely on Ascend 910s, making it the first non-Huawei, non-toy model trained entirely on Chinese accelerators. Other Chinese chips are mostly used for inference (if at all).

- **[Laguna-XS-2.1](https://huggingface.co/poolside/Laguna-XS-2.1)** by [poolside](https://huggingface.co/poolside): An update to the small (33B-A3B) MoE from Poolside.

- **[Motif-3-Beta](https://huggingface.co/Motif-Technologies/Motif-3-Beta)** by [Motif-Technologies](https://huggingface.co/Motif-Technologies): A preview of a 314B-A13B MoE by the Korean Motif. This is by far the company’s most ambitious model, as it is considerably larger and introduces some architectural innovations like GDLA and mHC.

- **[Apertus-v1.5-70B](https://huggingface.co/swiss-ai/Apertus-v1.5-70B)** by [swiss-ai](https://huggingface.co/swiss-ai): A continued pre-train of the fully open-source Apertus 1.0, using 2T more tokens.

- **[Instella-MoE-16B-A3B-Think](https://huggingface.co/amd/Instella-MoE-16B-A3B-Think)** by [amd](https://huggingface.co/amd): A 16B-A3B MoE trained by AMD on Instinct cards. AMD also provides all the different stages, from the [base](https://huggingface.co/amd/Instella-MoE-16B-A3B-Base) to the [SFT](https://huggingface.co/amd/Instella-MoE-16B-A3B-SFT) checkpoints, as well as [MidTrain](https://huggingface.co/amd/Instella-MoE-16B-A3B-Midtrain) and [DPO](https://huggingface.co/amd/Instella-MoE-16B-A3B-DPO).

  [![Instella-MoE cost vs. performance](https://substackcdn.com/image/fetch/$s_!G3cH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febdf2641-6b32-4c0a-8c93-087c75280057_2166x1007.png "Instella-MoE cost vs. performance")](https://substackcdn.com/image/fetch/$s_!G3cH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Febdf2641-6b32-4c0a-8c93-087c75280057_2166x1007.png)
