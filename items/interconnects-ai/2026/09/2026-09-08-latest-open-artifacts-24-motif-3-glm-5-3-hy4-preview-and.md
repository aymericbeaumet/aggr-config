---
title: 'Latest open artifacts (#24): Motif-3, GLM-5.3, Hy4-preview and open model licenses'
link: https://www.interconnects.ai/p/latest-open-artifacts-24-motif-3
source: interconnects-ai
published: 2026-09-08T14:15:25Z
updated: 2026-09-08T14:15:25Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Florian Brand
summary: The open model ecosystem continues to expand in its breadth
content: extracted
html: 2026-09-08-latest-open-artifacts-24-motif-3-glm-5-3-hy4-preview-and.html
preview:
  file: 2026-09-08-latest-open-artifacts-24-motif-3-glm-5-3-hy4-preview-and.preview-52ad81970554.webp
  width: 256
  height: 171
  color: '#94afbb'
images:
- source: https://substackcdn.com/image/fetch/$s_!J8Bn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2908b12f-8586-49cf-b34b-e1d5bc61bf19_1196x798.png
  original:
    file: 2026-09-08-latest-open-artifacts-24-motif-3-glm-5-3-hy4-preview-and.image-a7e472aba759.jpg
    width: 1196
    height: 798
  color: '#c7e6f7'
- source: https://substackcdn.com/image/fetch/$s_!vpGN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ef41d30-ff6b-4294-986c-1a6340953e33_2692x1396.png
  original:
    file: 2026-09-08-latest-open-artifacts-24-motif-3-glm-5-3-hy4-preview-and.image-eb9540d91782.jpg
    width: 1456
    height: 755
  color: '#f3f3f1'
- source: https://substackcdn.com/image/fetch/$s_!IEWy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f3d7cd7-e6db-43f4-82e8-63bc295c25df_4239x2643.png
  original:
    file: 2026-09-08-latest-open-artifacts-24-motif-3-glm-5-3-hy4-preview-and.image-aadf09fdd8f5.jpg
    width: 1456
    height: 908
  color: '#fcfdfd'
- source: https://substackcdn.com/image/fetch/$s_!H5FQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F571de54f-e9c2-4c7b-97e9-f9e250fe9071_4239x2643.png
  original:
    file: 2026-09-08-latest-open-artifacts-24-motif-3-glm-5-3-hy4-preview-and.image-aadf09fdd8f5.jpg
    width: 1456
    height: 908
  color: '#fcfdfd'
---

Avid Artifacts readers know that we have been covering not only models but also their licenses for quite some time. There was a period when custom licenses were all the rage, for example [the custom Qwen2.5 72B-Instruct license](https://huggingface.co/Qwen/Qwen2.5-72B-Instruct/blob/main/LICENSE) or [the Llama licenses](https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct/blob/main/LICENSE). DeepSeek had a [custom license for DeepSeek V3](https://huggingface.co/deepseek-ai/DeepSeek-V3/blob/main/LICENSE-MODEL) before [R1 changed it to MIT](https://huggingface.co/deepseek-ai/DeepSeek-R1/blob/main/LICENSE), which has resulted in many (Chinese) model makers adopting MIT or Apache 2.0 licenses in 2025.

In 2026, open models are more competitive than ever, which has led to two interesting developments: Western model makers adopt open licenses, with both [Google](https://huggingface.co/google/gemma-4-31B-it) and [Meta](https://huggingface.co/meta-models/Muse-Glimmer-30B) switching to Apache 2.0. Chinese model makers at the frontier, however, are becoming more restrictive: [Kimi K3](https://huggingface.co/moonshotai/Kimi-K3) comes with a license which requires commercial agreements for those who run inference or fine-tuning services, and [MiniMax M3](https://huggingface.co/MiniMaxAI/MiniMax-M3/blob/main/LICENSE) requires agreements above a revenue threshold and has prohibited use cases.

The newest addition is Zhipu’s [GLM-5.3](https://huggingface.co/zai-org/GLM-5.3), which switched from MIT (GLM-5.2 and earlier) to a custom license with the following clause for inference and fine-tuning providers:

> *If the Licensee or any of its affiliates operates a Model as a Service business, and the aggregate revenue of the Licensee and its affiliates exceeds 10 billion US dollars (or the equivalent in other currencies) in total over any consecutive 12 months, the Licensee must pass Z.AI’s security review before using the Software or its derivative works for any commercial purpose. The scope and method of the security review shall be reasonably determined by Z.AI.*

While the 10 billion US dollar threshold is very high compared to other licenses of this kind, “affiliates” is not defined in the license, which adds uncertainty and creates barriers to adoption. Furthermore, the license is provided in both English and Chinese, with the Chinese text using “关联方” for affiliated parties, which [does have a definition in Chinese law](https://kjs.mof.gov.cn/zt/kjzzss/kuaijizhunzeshishi/200806/t20080618_46245.htm).

We are by no means legal experts and there are obvious reasons why those licenses are created. However, we want to highlight the issues that come with creating such licenses, especially in a world with a lot of valid open and closed alternatives.

[Share](https://www.interconnects.ai/p/latest-open-artifacts-24-motif-3?utm_source=substack&utm_medium=email&utm_content=share&action=share)

- **[Motif-3](https://huggingface.co/Motif-Technologies/Motif-3)** by [Motif-Technologies](https://huggingface.co/Motif-Technologies): Motif is one of the few hidden gems out there, showcasing innovation in their model training with very limited resources compared to others. Motif-3 comes with an MIT license and impressive scores for its size. Given the trajectory of model releases from Motif 2.6B, which we covered [in 2025](https://artifactshub.ai/Motif-Technologies/motif-2-6b) and [Motif-2-12.7B](https://artifactshub.ai/Motif-Technologies/motif-2-12-7b-instruct), the improvements are impressive.

- **[dots3-note-prev](https://huggingface.co/dots-studio/dots3-note-prev)** by [dots-studio](https://huggingface.co/dots-studio): RedNote/Xiaohongshu, the Chinese Instagram, is also getting more serious about model training, although they aren’t exactly [a newcomer](https://artifactshub.ai/rednote-hilab/dots-llm1-inst), having released models as early as 2025. dots3 was also able to win the IMO 2026 with a perfect score using an internal harness. We expect more from them in the near future.

  [![General Reasoning and Agent evaluation results](https://substackcdn.com/image/fetch/$s_!vpGN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ef41d30-ff6b-4294-986c-1a6340953e33_2692x1396.png "General Reasoning and Agent evaluation results")](https://substackcdn.com/image/fetch/$s_!vpGN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ef41d30-ff6b-4294-986c-1a6340953e33_2692x1396.png)

- **[Qwen3.8-Flash-Next](https://huggingface.co/Qwen/Qwen3.8-Flash-Next)** by [Qwen](https://huggingface.co/Qwen): A preview of the next version of Qwen models in terms of architecture: 125B-A6B with 51B n-gram embeddings. It uses GDN and Qwen Sparse Attention. Similar to [Qwen3-Next-80B-A3B-Instruct](https://artifactshub.ai/Qwen/qwen3-next-80b-a3b-instruct), we expect similar architectures to become more popular and the ecosystem to fix integrations by the time Qwen4 drops.

- **[GLM-5.3-Flash](https://huggingface.co/zai-org/GLM-5.3-Flash)** by [zai-org](https://huggingface.co/zai-org): This release perfected the version of the Chinese model playbook we’ve written about [in 2025](https://www.interconnects.ai/p/latest-open-artifacts-16-whos-building): The model got released as a free-to-use “stealth model” under the name “Ox-Alpha” on OpenRouter and OpenCode, which got people excited to try it out in the first place. They then speculated about its creator and size, alleging it is a >1T model from Cursor/xAI, Gemini or a new pre-train from open source labs. Because the model is relatively performant, people kept speculating for days about its creator, thus building up hype. It also dampens the accusations of benchmaxxing which accompany every (open) model release.

  [![bench_53](https://substackcdn.com/image/fetch/$s_!IEWy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f3d7cd7-e6db-43f4-82e8-63bc295c25df_4239x2643.png "bench_53")](https://substackcdn.com/image/fetch/$s_!IEWy!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7f3d7cd7-e6db-43f4-82e8-63bc295c25df_4239x2643.png)

- **[Hy4-preview](https://huggingface.co/tencent/Hy4-preview)** by [tencent](https://huggingface.co/tencent): Tencent is becoming a serious player in the open model space, increasing the size of their flagship model while spinning the post-training flywheel. The result, Hy4-preview, is a competent model which currently has an issue with overthinking. However, if the trajectory from [Hy3-preview](https://artifactshub.ai/tencent/hy3-preview) to [Hy3](https://artifactshub.ai/tencent/hy3) is any indication, the final model might be a legit shot at the front ranks of open models.

View more details on all the models in this issue at our [Artifacts Hub](https://artifactshub.ai/?picks=0).

[Visit artifactshub.ai](https://artifactshub.ai/?picks=0)

- **[NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16)** by [nvidia](https://huggingface.co/nvidia): An update to Nemotron, which comes with performance — but especially speed improvements — across the board.

  [![bench_53](https://substackcdn.com/image/fetch/$s_!H5FQ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F571de54f-e9c2-4c7b-97e9-f9e250fe9071_4239x2643.png "bench_53")](https://substackcdn.com/image/fetch/$s_!H5FQ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F571de54f-e9c2-4c7b-97e9-f9e250fe9071_4239x2643.png)

- **[Ling-3.0-flash](https://huggingface.co/inclusionAI/Ling-3.0-flash)** by [inclusionAI](https://huggingface.co/inclusionAI): Ant Ling is a frequent guest at the Artifacts Log; they are now on their third iteration of models, adopting a hybrid design (KDA + Gated MLA), similar to others. They also release [a small 7.9B-A1.3B](https://huggingface.co/inclusionAI/Ling-3.0-tiny) version.

- **[Qwen3.8-2.4T-A95B](https://huggingface.co/Qwen/Qwen3.8-2.4T-A95B)** by [Qwen](https://huggingface.co/Qwen): In a rather surprising turn of events, Alibaba started to openly release their biggest versions of Qwen as well. However, it comes with a custom license and its performance is behind other models of its size.
