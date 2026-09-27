---
title: 'Latest open artifacts (#22): Zyphra, Cohere, and Poolside are expanding the breadth of the ecosystem'
link: https://www.interconnects.ai/p/artifacts-22-zyphra-cohere-and-poolside
source: interconnects-ai
published: 2026-06-28T17:03:07Z
updated: 2026-06-28T17:03:07Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Florian Brand
summary: An assessment of the open ecosystem and the motivations behind releasing models
content: extracted
html: 2026-06-28-latest-open-artifacts-22-zyphra-cohere-and-poolside-are.html
preview:
  file: 2026-06-28-latest-open-artifacts-22-zyphra-cohere-and-poolside-are.preview-d7a541ff155c.webp
  width: 256
  height: 144
  color: '#716c65'
images:
- source: https://substackcdn.com/image/fetch/$s_!Yzio!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2f6a1684-d8af-4a82-a0fc-ba65d6a6dde0_1024x576.jpeg
  original:
    file: 2026-06-28-latest-open-artifacts-22-zyphra-cohere-and-poolside-are.image-f8f6ef0b0553.jpg
    width: 1024
    height: 576
  color: '#474836'
- source: https://substackcdn.com/image/fetch/$s_!1nXJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90449d43-2181-4090-9e4b-8c94a05b111c_1223x707.png
  original:
    file: 2026-06-28-latest-open-artifacts-22-zyphra-cohere-and-poolside-are.image-17ba878aa42f.jpg
    width: 1223
    height: 707
  color: '#fbfcfb'
- source: https://substackcdn.com/image/fetch/$s_!sooJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d1ecb70-0e3d-435e-aabf-0e2b6e7d7138_1435x747.png
  original:
    file: 2026-06-28-latest-open-artifacts-22-zyphra-cohere-and-poolside-are.image-eb3d91aa7b7e.png
    width: 1435
    height: 747
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!ylaJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e166405-1e61-4b66-8774-0ab29072e3f4_2401x1709.png
  original:
    file: 2026-06-28-latest-open-artifacts-22-zyphra-cohere-and-poolside-are.image-a80fbf64e437.png
    width: 1456
    height: 1036
  color: '#006ee7'
---

A trend we [continue to see](https://www.interconnects.ai/p/latest-open-artifacts-20-new-orgs) in open model releases is that the ecosystem is becoming more diverse, with an increasing number of organizations releasing a wide range of models. A year ago, open artifacts and the open model landscape more broadly were dominated by a handful of (Chinese) players. This has shifted, with us increasingly featuring more niche companies all over the world.

While it is hard to know the exact motivations of the companies themselves, we can broadly observe the following categories:

- **“Pure” model makers**: These are companies whose stated goal is to train models that are at the frontier, or at least close to it. This includes many Chinese companies, such as DeepSeek, Zhipu, and Minimax, but also Western ones like Poolside, Arcee, and Zyphra. It also increasingly includes sovereign AI players, such as Cohere, Sovereign, Mistral, and Trillion Labs. The recent Mythos episode has woken up some policymakers, which may lead to increased interest in sovereign model training.

- **Big Tech**: For Big Tech companies, including Alibaba’s Qwen, Google’s Gemma, and, to some extent, NVIDIA, the motivations are more diverse. Alibaba uses model releases to upsell its closed models, while NVIDIA benefits from a flourishing open model ecosystem as it increases interest in and usage of its GPUs. This vested interest is different from the Llama era of open Western models, where the motivations for open releases were less clear (and ultimately did not hold).

- **Product companies**: Some companies, such as JetBrains, Zed, Krea, and Photoroom, mainly sell products that use AI as a core component. As they don’t want to be cut off from [accessing closed models](https://techcrunch.com/2025/06/03/windsurf-says-anthropic-is-limiting-its-direct-access-to-claude-ai-models/) or want to offer something unique, they can train highly specialized, small models that fit their product needs. Thus, open-sourcing those model weights does not hurt their bottom line.

This diversity of makers and models [fits our hypothesis](https://www.interconnects.ai/p/the-next-phase-of-open-models?utm_source=publication-search) that more companies will develop a long-tail of models and the number of companies chasing the absolute, open frontier will diminish.

[Share](https://www.interconnects.ai/p/artifacts-22-zyphra-cohere-and-poolside?utm_source=substack&utm_medium=email&utm_content=share&action=share)

While not every model release fits neatly into one of these categories, the broader point is that open model development is not driven by a single type of actor or motivation. This diversity is one of the strengths of the open ecosystem and can be seen in the tech reports of model releases, which reuse training methods, architecture choices and data from other open model releases.

Attempts to slow or ban this ecosystem are not only futile, as the history of tech-related bans has shown, but also [unsafe and anti-freedom](https://www.interconnects.ai/p/banning-open-source-ai-would-be-a). Such restrictions would concentrate AI development and usage among the select few, which ultimately endangers outsiders’ ability to freely adopt one of the most important technologies of our lifetime.

- **[NVIDIA-Nemotron-3-Ultra-550B-A55B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Ultra-550B-A55B-BF16)** by [nvidia](https://huggingface.co/nvidia): The big version of the Nemotron series, which uses LatentMoE to be even faster than comparable models. Just like the other Nemotron models, the vast majority of the data is open source. And, to top it all off: NVIDIA commits to using the [OpenMDW](https://www.linuxfoundation.org/press/linux-foundation-releases-openmdw-1.1-nvidia-adopts-openmdw-for-cosmos-isaac-gr00t-ising-and-nemotron-ai-model-families) license, which is tailored specifically for model weights (and data) and drops its custom license. While MIT and Apache are in the same spirit as OpenMDW, only the latter really covers model weights, while the former are software licenses that do not really apply to model weights.

  [![](https://substackcdn.com/image/fetch/$s_!1nXJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90449d43-2181-4090-9e4b-8c94a05b111c_1223x707.png)](https://substackcdn.com/image/fetch/$s_!1nXJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F90449d43-2181-4090-9e4b-8c94a05b111c_1223x707.png)

- **[command-a-plus-05-2026-bf16](https://huggingface.co/CohereLabs/command-a-plus-05-2026-bf16)** by [CohereLabs](https://huggingface.co/CohereLabs): Cohere, which is becoming more of a regular entrant into Artifacts lately, released their flagship, Command A+, under Apache 2.0. Previous iterations of the series have been released under a non-commercial license, so this change is more than welcome! Command A+ combines multi-modal, multi-lingual and agentic capabilities as a 218B-A25B MoE, making it usable with a single B200 (when using 4-bit).

- **[GLM-5.2](https://huggingface.co/zai-org/GLM-5.2)** by [zai-org](https://huggingface.co/zai-org): The biggest story in this Artifacts is GLM-5.2, which we have covered in a separate [blog](https://www.interconnects.ai/p/glm-52-is-the-step-change-for-open) as well. The model continues to impress and is genuinely usable for everyday work, not a huge regression compared to the best closed models available right now. Interestingly enough, the raw download numbers since release are more in line with other model releases, with GLM-5.2 being roughly in line with GLM-5 after release.

  [![](https://substackcdn.com/image/fetch/$s_!sooJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d1ecb70-0e3d-435e-aabf-0e2b6e7d7138_1435x747.png)](https://substackcdn.com/image/fetch/$s_!sooJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d1ecb70-0e3d-435e-aabf-0e2b6e7d7138_1435x747.png)

- **[ZAYA1-74B-preview](https://huggingface.co/Zyphra/ZAYA1-74B-preview)** by [Zyphra](https://huggingface.co/Zyphra): Zyphra, which trains on AMD GPUs and is known as some sort of insider tip in the research community due to their tech reports with interesting architecture choices, has released some new models, with a 74B-A4B MoE and an [8B-A0.6B MoE](https://huggingface.co/Zyphra/ZAYA1-8B) ( [tech report](https://arxiv.org/abs/2605.05365)) being their current flagship releases.

- **[Laguna-M.1](https://huggingface.co/poolside/Laguna-M.1)** by [poolside](https://huggingface.co/poolside): Poolside, which we covered in [the last Artifacts](https://www.interconnects.ai/p/latest-open-artifacts-21-open-model), also released their flagship model under Apache 2.0! They also commit to open releases [going forward](https://x.com/poolsideai/status/2067623663562637683?s=20):

  > Open weights are now our default. We’ll keep building toward the frontier and releasing increasingly capable models in the open.

- **[Kimi-K2.7-Code](https://huggingface.co/moonshotai/Kimi-K2.7-Code)** by [moonshotai](https://huggingface.co/moonshotai): An update to Kimi focusing a lot on token efficiency.

- **[Step-3.7-Flash](https://huggingface.co/stepfun-ai/Step-3.7-Flash)** by [stepfun-ai](https://huggingface.co/stepfun-ai): An update to Step-Flash, which is really strong in Math in particular.

- **[Nemotron-Labs-Diffusion-14B](https://huggingface.co/nvidia/Nemotron-Labs-Diffusion-14B)** by [nvidia](https://huggingface.co/nvidia): An experimental model which can be used in three different modes: autoregressive, diffusion, and self-speculation. Each of these modes is suitable for a different use case.

  [![An illustration of Tri-Mode LMs](https://substackcdn.com/image/fetch/$s_!ylaJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e166405-1e61-4b66-8774-0ab29072e3f4_2401x1709.png "An illustration of Tri-Mode LMs")](https://substackcdn.com/image/fetch/$s_!ylaJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e166405-1e61-4b66-8774-0ab29072e3f4_2401x1709.png)
