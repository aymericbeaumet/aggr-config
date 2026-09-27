---
title: My Workflow for Understanding LLM Architectures
link: https://magazine.sebastianraschka.com/p/workflow-for-understanding-llms
source: magazine-sebastianraschka-com
published: 2026-04-18T11:24:36Z
updated: 2026-04-18T11:24:36Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Sebastian Raschka, PhD
summary: A learning-oriented workflow for understanding new open-weight model releases
content: extracted
html: 2026-04-18-my-workflow-for-understanding-llm-architectures.html
preview:
  file: 2026-04-18-my-workflow-for-understanding-llm-architectures.preview-f2cf4a97cb15.webp
  width: 256
  height: 143
  color: '#f2f1f0'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/43071df5-b297-42f2-b875-36915f5e33a2_1826x1020.png
  original:
    file: 2026-04-18-my-workflow-for-understanding-llm-architectures.image-de49771b2932.png
    width: 1826
    height: 1020
  color: '#fcfcfc'
- source: https://substackcdn.com/image/fetch/$s_!NNCh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdfd5ee24-27f0-4d06-a987-e1c7247057d3_4800x2700.png
  original:
    file: 2026-04-18-my-workflow-for-understanding-llm-architectures.image-1ba94e4422d5.jpg
    width: 1456
    height: 819
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!evXx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc06a1e90-e1bf-411f-964b-ce89852491a3_4168x1699.png
  original:
    file: 2026-04-18-my-workflow-for-understanding-llm-architectures.image-1f334a7021a9.jpg
    width: 1456
    height: 594
  color: '#fbfbfb'
---

Many people asked me over the past months to share my workflow for how I come up with the LLM architecture sketches and drawings in my articles, talks, and the [LLM-Gallery](https://sebastianraschka.com/llm-architecture-gallery/). So I thought it would be useful to document the process I usually follow.

The short version is that I usually start with the official technical reports, but these days, papers are often less detailed than they used to be, especially for most open-weight models from industry labs.

The good part is that if the weights are shared on the Hugging Face Model Hub and the model is supported in the Python [transformers](https://github.com/huggingface/transformers) library, we can usually inspect the config file and the reference implementation directly to get more information about the architecture details. And “working” code doesn’t lie.

[![](https://substackcdn.com/image/fetch/$s_!NNCh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdfd5ee24-27f0-4d06-a987-e1c7247057d3_4800x2700.png "")](https://substackcdn.com/image/fetch/$s_!NNCh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdfd5ee24-27f0-4d06-a987-e1c7247057d3_4800x2700.png)

Figure 1: The basic motivation for this workflow is that papers are often less detailed these days, but a working reference implementation gives us something concrete to inspect.

I should also say that this is mainly a workflow for open-weight models. It doesn’t really apply to models like ChatGPT, Claude, or Gemini, where the weights and details are proprietary.

Also, this is intentionally a fairly manual process. You could automate parts of it. But if the goal is to learn how these architectures work, then doing a few of these by hand is, in my opinion, still one of the best exercises.

[![](https://substackcdn.com/image/fetch/$s_!evXx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc06a1e90-e1bf-411f-964b-ce89852491a3_4168x1699.png)](https://substackcdn.com/image/fetch/$s_!evXx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc06a1e90-e1bf-411f-964b-ce89852491a3_4168x1699.png)

Figure 2: At a high level, the workflow goes from config files and code to architecture insights.
