---
title: What I learned from looking at 900 most popular open source AI tools
link: https://huyenchip.com//2024/03/14/ai-oss.html
source: huyenchip-com
published: 2024-03-14T00:00:00Z
updated: 2024-03-14T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: '[Hacker News discussion, LinkedIn discussion, Twitter thread] Update (Feb 2026): The full list of open source AI repos is hosted at Good AI List, updated daily. It’s balooned to 15K repos, and you can submit missing repos. You can also find some of them on my cool-llm-repos list on GitHub. Four years ago, I did an analysis of the open source ML ecosystem. Since then, the landscape has changed, so I revisited the topic. This time, I focused exclusively on the stack around foundation models. Data I searched GitHub using the keywords gpt, llm, and generative ai. If AI feels so overwhelming right now, it’s because it is. There are 118K results for gpt alone. To make my life easier, I limited my search to the repos with at least 500 stars. There were 590 results for llm, 531 for gpt, and 38 for generative ai. I also occasionally checked GitHub trending and social media for new repos. After MANY hours, I found 896 repos. Of these, 51 are tutorials (e.g. dair-ai/Prompt-Engineering-Guide) and aggregated lists (e.g. f/awesome-chatgpt-prompts). While these tutorials and lists are helpful, I’m more interested in software. I still include them in the final list, but the analysis is done with the 845 software repositories. It was a painful but rewarding process. It gave me a much better understanding of what people are working on, how incredibly collaborative the open source community is, and just how much China’s open source ecosystem diverges from the Western one. The New AI Stack I think of the AI stack as consisting of 3 layers: infrastructure, model development, and application development. Infrastructure At the bottom is the stack is infrastructure, which includes toolings for serving (vllm, NVIDIA’s Triton), compute management (skypilot), vector search and database (faiss, milvus, qdrant, lancedb), …. Model development This layer provides toolings for developing models, including frameworks for modeling & training (transformers, pytorch, DeepSpeed), inference optimization (ggml, openai/triton), dataset engineering, evaluation, ….. Anything that involves changing a model’s weights happens in this layer, including finetuning. Application development With readily available models, anyone can develop applications on top of them. This is the layer that has seen the most actions in the last 2 years and is still rapidly evolving. This layer is also known as AI engineering. Application development involves prompt engineering, RAG, AI interface, … Outside of these 3 layers, I also have two other categories: Model repos, which are created by companies and researchers to share the code associated with their models. Examples of repos in this category are CompVis/stable-diffusion, openai/whisper, and facebookresearch/llama. Applications built on top of existing models. The most popular types of applications are coding, workflow automation, information aggregation, … Note: In an older version of this post, Applications was included as another layer in the stack. AI stack over time I plotted the cumulative number of repos in each category month-over-month. There was an explosion of new toolings in 2023, after the introduction of Stable Diffusion and ChatGPT. The curve seems to flatten in September 2023 because of three potential reasons. I only include repos with at least 500 stars in my analysis, and it takes time for repos to gather these many stars. Most low-hanging fruits have been picked. What is left takes more effort to build, hence fewer people can build them. People have realized that it’s hard to be competitive in the generative AI space, so the excitement has calmed down. Anecdotally, in early 2023, all AI conversations I had with companies centered around gen AI, but the recent conversations are more grounded. Several even brought up scikit-learn. I’d like to revisit this in a few months to verify if it’s true. In 2023, the layers that saw the highest increases were the applications and application development layers. The infrastructure layer saw a little bit of growth, but it was far from the level of growth seen in other layers. Applications Not surprisingly, the most popular types of applications are coding, bots (e.g. role-playing, WhatsApp bots, Slack bots), and information aggregation (e.g. “let’s connect this to our Slack and ask it to summarize the messages each day”). AI engineering 2023 was the year of AI engineering. Since many of them are similar, it’s hard to categorize the tools. I currently put them into the following categories: prompt engineering, AI interface, Agent, and AI engineering (AIE) framework. Prompt engineering goes way beyond fiddling with prompts to cover things like constrained sampling (structured outputs), long-term memory management, prompt testing & evaluation, etc. AI interface provides an interface for your end users to interact with your AI application. This is the category I’m the most excited about. Some of the interfaces that are gaining popularity are: Web and desktop apps. Browser extensions that let users quickly query AI models while browsing. Bots via chat apps like Slack, Discord, WeChat, and WhatsApp. Plugins that let developers embed AI applications to applications like VSCode, Shopify, and Microsoft Offices. The plugin approach is common for AI applications that can use tools to complete complex tasks (agents). AIE framework is a catch-all term for all platforms that help you develop AI applications. Many of them are built around RAG, but many also provide other toolings such as monitoring, evaluation, etc. Agent is a weird category, as many agent toolings are just sophisticated prompt engineering with potentially constrained generation (e.g. the model can only output the predetermined action) and plugin integration (e.g. to let the agent use tools). Model development Pre-ChatGPT, the AI stack was dominated by model development. Model development’s biggest growth in 2023 came from increasing interest in inference optimization, evaluation, and parameter-efficient finetuning (which is grouped under Modeling & training). Inference optimization has always been important, but the scale of foundation models today makes it crucial for latency and cost. The core approaches for optimization remain the same (quantization, low-ranked factorization, pruning, distillation), but many new techniques have been developed especially for the transformer architecture and the new generation of hardware. For example, in 2020, 16-bit quantization was considered state-of-the-art. Today, we’re seeing 2-bit quantization and even lower than 2-bit. Similarly, evaluation has always been essential, but with many people today treating models as blackboxes, evaluation has become even more so. There are many new evaluation benchmarks and evaluation methods, such as comparative evaluation (see Chatbot Arena) and AI-as-a-judge. Infrastructure Infrastructure is about managing data, compute, and toolings for serving, monitoring, and other platform work. Despite all the changes that generative AI brought, the open source AI infrastructure layer remained more or less the same. This could also be because infrastructure products are typically not open sourced. The newest category in this layer is vector database with companies like Qdrant, Pinecone, and LanceDB. However, many argue this shouldn’t be a category at all. Vector search has been around for a long time. Instead of building new databases just for vector search, existing database companies like DataStax and Redis are bringing vector search into where the data already is. Open source AI developers Open source software, like many things, follows the long tail distribution. A handful of accounts control a large portion of the repos. One-person billion-dollar companies? 845 repos are hosted on 594 unique GitHub accounts. There are 20 accounts with at least 4 repos. These top 20 accounts host 195 of the repos, or 23% of all the repos on the list. These 195 repos have gained a total of 1,650,000 stars. On Github, an account can be either an organization or an individual. 19/20 of the top accounts are organizations. Of those, 3 belong to Google: google-research, google, tensorflow. The only individual account in these top 20 accounts is lucidrains. Among the top 20 accounts with the most number of stars (counting only gen AI repos), 4 are individual accounts: lucidrains (Phil Wang): who can implement state-of-the-art models insanely fast. ggerganov (Georgi Gerganov): an optimization god who comes from a physics background. Illyasviel (Lyumin Zhang): creator of Foocus and ControlNet who’s currently a Stanford PhD. xtekky: a full-stack developer who created gpt4free. Unsurprisingly, the lower we go in the stack, the harder it is for individuals to build. Software in the infrastructure layer is the least likely to be started and hosted by individual accounts, whereas more than half of the applications are hosted by individuals. Applications started by individuals, on average, have gained more stars than applications started by organizations. Several people have speculated that we’ll see many very valuable one-person companies (see Sam Altman’s interview and Reddit discussion). I think they might be right. 1 million commits Over 20,000 developers have contributed to these 845 repos. In total, they’ve made almost a million contributions! Among them, the 50 most active developers have made over 100,000 commits, averaging over 2,000 commits each. See the full list of the top 50 most active open source developers here. The growing China''s open source ecosystem It’s been known for a long time that China’s AI ecosystem has diverged from the US (I also mentioned that in a 2020 blog post). At that time, I was under the impression that GitHub wasn’t widely used in China, and my view back then was perhaps colored by China’s 2013 ban on GitHub. However, this impression is no longer true. There are many, many popular AI repos on GitHub targeting Chinese audiences, such that their descriptions are written in Chinese. There are repos for models developed for Chinese or Chinese + English, such as Qwen, ChatGLM3, Chinese-LLaMA. While in the US, many research labs have moved away from the RNN architecture for language models, the RNN-based model family RWKV is still popular. There are also AI engineering tools providing ways to integrate AI models into products popular in China like WeChat, QQ, DingTalk, etc. Many popular prompt engineering tools also have mirrors in Chinese. Among the top 20 accounts on GitHub, 6 originated in China: THUDM: Knowledge Engineering Group (KEG) & Data Mining at Tsinghua University. OpenGVLab: General Vision team of Shanghai AI Laboratory OpenBMB: Open Lab for Big Model Base, founded by ModelBest & the NLP group at Tsinghua University. InternLM: from Shanghai AI Laboratory. OpenMMLab: from The Chinese University of Hong Kong. QwenLM: Alibaba’s AI lab, which publishes the Qwen model family. Live fast, die young One pattern that I saw last year is that many repos quickly gained a massive amount of eyeballs, then quickly died down. Some of my friends call this the “hype curve”. Out of these 845 repos with at least 500 GitHub stars, 158 repos (18.8%) haven’t gained any new stars in the last 24 hours, and 37 repos (4.5%) haven’t gained any new stars in the last week. Here are examples of the growth trajectory of two of such repos compared to the growth curve of two more sustained software. Even though these two examples shown here are no longer used, I think they were valuable in showing the community what was possible, and it was cool that the authors were able to get things out so fast. My personal favorite ideas So many cool ideas are being developed by the community. Here are some of my favorites. Batch inference optimization: FlexGen, llama.cpp Faster decoder with techniques such as Medusa, LookaheadDecoding Model merging: mergekit Constrained sampling: outlines, guidance, SGLang Seemingly niche tools that solve one problem really well, such as einops and safetensors. Conclusion Even though I included only 845 repos in my analysis, I went through several thousands of repos. I found this helpful for me to get a big-picture view of the seemingly overwhelming AI ecosystem. I hope the list is useful for you too. Please do let me know what repos I’m missing, and I’ll add them to the list!'
content: extracted
html: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.html
preview:
  file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.preview-7cab6d7d5de1.webp
  width: 256
  height: 144
  alt: Generative AI Stack Over Time
  color: '#efe9e6'
images:
- source: https://huyenchip.com/assets/pics/ai-oss/2-ai-timeline.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-02de3ddf1150.png
    width: 1999
    height: 1125
  variants:
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-a9079825ae38.webp
    width: 320
    height: 180
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-207292aedf08.webp
    width: 640
    height: 360
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-f9eda220fe1d.webp
    width: 960
    height: 540
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-335180e501e7.webp
    width: 1280
    height: 720
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-457e2cea207a.webp
    width: 1600
    height: 900
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-a0ffc92ad385.webp
    width: 1999
    height: 1125
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/ai-oss/1-ai-stack.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-7a75a9bb7aa5.png
    width: 1692
    height: 766
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/ai-oss/3-ai-applications.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-29227756e00b.png
    width: 1228
    height: 1052
  color: '#fefefe'
- source: https://huyenchip.com/assets/pics/ai-oss/4-prompt-engineering.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-7f3aa4be16db.png
    width: 1999
    height: 739
  variants:
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-fb0eea7af840.webp
    width: 320
    height: 118
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-8a06b885e750.webp
    width: 640
    height: 237
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-eed8b4e9274a.webp
    width: 960
    height: 355
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-a9d2cd3be4e7.webp
    width: 1280
    height: 473
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-8d5f6a04ac91.webp
    width: 1600
    height: 591
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-46435e45d63b.webp
    width: 1999
    height: 739
  color: '#f9f9f9'
- source: https://huyenchip.com/assets/pics/ai-oss/5-ai-engineering.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-d9877138a405.png
    width: 1999
    height: 1075
  variants:
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-ef3dbb77e578.webp
    width: 320
    height: 172
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-d4a384eb7a98.webp
    width: 640
    height: 344
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-e0c88b57b4d6.webp
    width: 960
    height: 516
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-5e3b665febc7.webp
    width: 1280
    height: 688
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-c79397f08ac3.webp
    width: 1600
    height: 860
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-b9536fab7f9e.webp
    width: 1999
    height: 1075
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/ai-oss/6-model-development.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-b16583ec4fd5.png
    width: 1999
    height: 1078
  variants:
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-915675c9eaa5.webp
    width: 320
    height: 173
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-87a5c0a3b2aa.webp
    width: 640
    height: 345
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-bd312ad89737.webp
    width: 960
    height: 518
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-e11592b10dc8.webp
    width: 1280
    height: 690
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-f80b0725090c.webp
    width: 1600
    height: 863
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-d3bb06ba7919.webp
    width: 1999
    height: 1078
  color: '#fdfdfc'
- source: https://huyenchip.com/assets/pics/ai-oss/7-top-accounts.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-629a72ac4e0f.png
    width: 1554
    height: 1050
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/ai-oss/8-top-accounts-stars.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-d02cf2ea5fe0.png
    width: 1904
    height: 1390
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/ai-oss/9-indie.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-df88c5163544.png
    width: 1222
    height: 898
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/ai-oss/10-indie-stars.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-6ce80ec510f0.png
    width: 1999
    height: 1178
  variants:
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-65213f344c34.webp
    width: 320
    height: 189
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-1825bf59ce73.webp
    width: 640
    height: 377
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-8783d590c917.webp
    width: 960
    height: 566
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-8885653cfcf8.webp
    width: 1280
    height: 754
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-976213ed295a.webp
    width: 1600
    height: 943
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-47afa2c767f2.webp
    width: 1999
    height: 1178
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/ai-oss/11-devs.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-360ad20a2863.png
    width: 1999
    height: 714
  variants:
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-917054a92f6b.webp
    width: 320
    height: 114
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-d65e9dd75f8d.webp
    width: 640
    height: 229
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-9149b6118cd4.webp
    width: 960
    height: 343
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-833ef69fa7e8.webp
    width: 1280
    height: 457
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-4058a57c858a.webp
    width: 1600
    height: 571
  - file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-5c0b9867cdb3.webp
    width: 1999
    height: 714
  color: '#fafafa'
- source: https://huyenchip.com/assets/pics/ai-oss/12-hype-curve.png
  original:
    file: 2024-03-14-what-i-learned-from-looking-at-900-most-popular-open-source.image-1cc739d2bf31.png
    width: 1536
    height: 1034
  color: '#fefefd'
---

\[*[Hacker News discussion](https://news.ycombinator.com/item?id=39709912), [LinkedIn discussion](https://www.linkedin.com/posts/chiphuyen_generativeai-aiapplications-llmops-activity-7174153467844820993-ztSE), [Twitter thread](https://twitter.com/chipro/status/1768388213008445837)*\]

**Update (Feb 2026)**: *The full list of open source AI repos is hosted at [Good AI List](https://goodailist.com), updated daily. It’s balooned to 15K repos, and you can submit missing repos. You can also find some of them on my [cool-llm-repos](https://github.com/stars/chiphuyen/lists/cool-llm-repos) list on GitHub.*

Four years ago, I did an analysis of the [open source ML ecosystem](https://huyenchip.com/2020/06/22/mlops.html). Since then, the landscape has changed, so I revisited the topic. This time, I focused exclusively on the stack around foundation models.

## Data

I searched GitHub using the keywords `gpt`, `llm`, and `generative ai`. If AI feels so overwhelming right now, it’s because it is. There are 118K results for `gpt` alone.

To make my life easier, I limited my search to the repos with at least 500 stars. There were 590 results for `llm`, 531 for `gpt`, and 38 for `generative ai`. I also occasionally checked GitHub trending and social media for new repos.

After MANY hours, I found 896 repos. Of these, 51 are tutorials (e.g. [dair-ai/Prompt-Engineering-Guide](https://github.com/dair-ai/Prompt-Engineering-Guide)) and aggregated lists (e.g. [f/awesome-chatgpt-prompts](https://github.com/f/awesome-chatgpt-prompts)). While these tutorials and lists are helpful, I’m more interested in software. I still include them in the final list, but the analysis is done with the 845 software repositories.

It was a painful but rewarding process. It gave me a much better understanding of what people are working on, how incredibly collaborative the open source community is, and just how much China’s open source ecosystem diverges from the Western one.

## The New AI Stack

I think of the AI stack as consisting of 3 layers: infrastructure, model development, and application development.

![Generative AI Stack](https://huyenchip.com/assets/pics/ai-oss/1-ai-stack.png)

1. **Infrastructure**

   At the bottom is the stack is infrastructure, which includes toolings for serving ([vllm](https://github.com/vllm-project/vllm), [NVIDIA’s Triton](https://github.com/triton-inference-server/server)), compute management ([skypilot](https://github.com/skypilot-org/skypilot)), vector search and database ([faiss](https://github.com/facebookresearch/faiss), [milvus](https://milvus.io/), [qdrant](https://github.com/qdrant/qdrant), [lancedb](https://github.com/lancedb/lancedb)), ….

2. **Model development**

   This layer provides toolings for developing models, including frameworks for modeling & training (transformers, pytorch, DeepSpeed), inference optimization (ggml, openai/triton), dataset engineering, evaluation, ….. Anything that involves changing a model’s weights happens in this layer, including finetuning.

3. **Application development** With readily available models, anyone can develop applications on top of them. This is the layer that has seen the most actions in the last 2 years and is still rapidly evolving. This layer is also known as AI engineering.

   Application development involves prompt engineering, RAG, AI interface, …

Outside of these 3 layers, I also have two other categories:

- **Model repos**, which are created by companies and researchers to share the code associated with their models. Examples of repos in this category are `CompVis/stable-diffusion`, `openai/whisper`, and `facebookresearch/llama`.
- **Applications** built on top of existing models. The most popular types of applications are coding, workflow automation, information aggregation, …

**Note**: *In an older version of this post, **Applications** was included as another layer in the stack.*

### AI stack over time

I plotted the cumulative number of repos in each category month-over-month. There was an explosion of new toolings in 2023, after the introduction of Stable Diffusion and ChatGPT. The curve seems to flatten in September 2023 because of three potential reasons.

1. I only include repos with at least 500 stars in my analysis, and it takes time for repos to gather these many stars.
2. Most low-hanging fruits have been picked. What is left takes more effort to build, hence fewer people can build them.
3. People have realized that it’s hard to be competitive in the generative AI space, so the excitement has calmed down. Anecdotally, in early 2023, all AI conversations I had with companies centered around gen AI, but the recent conversations are more grounded. Several even brought up scikit-learn. I’d like to revisit this in a few months to verify if it’s true.

![Generative AI Stack Over Time](https://huyenchip.com/assets/pics/ai-oss/2-ai-timeline.png)

In 2023, the layers that saw the highest increases were the applications and application development layers. The infrastructure layer saw a little bit of growth, but it was far from the level of growth seen in other layers.

#### Applications

Not surprisingly, the most popular types of applications are coding, bots (e.g. role-playing, WhatsApp bots, Slack bots), and information aggregation (e.g. “let’s connect this to our Slack and ask it to summarize the messages each day”).

![Breakdown of popular AI applications](https://huyenchip.com/assets/pics/ai-oss/3-ai-applications.png)

#### AI engineering

2023 was the year of AI engineering. Since many of them are similar, it’s hard to categorize the tools. I currently put them into the following categories: prompt engineering, AI interface, Agent, and AI engineering (AIE) framework.

**Prompt engineering** goes way beyond fiddling with prompts to cover things like constrained sampling (structured outputs), long-term memory management, prompt testing & evaluation, etc.

![A list of prompt engineering tools](https://huyenchip.com/assets/pics/ai-oss/4-prompt-engineering.png)

**AI interface** provides an interface for your end users to interact with your AI application. This is the category I’m the most excited about. Some of the interfaces that are gaining popularity are:

- Web and desktop apps.
- Browser extensions that let users quickly query AI models while browsing.
- Bots via chat apps like Slack, Discord, WeChat, and WhatsApp.
- Plugins that let developers embed AI applications to applications like VSCode, Shopify, and Microsoft Offices. The plugin approach is common for AI applications that can use tools to complete complex tasks (agents).

**AIE framework** is a catch-all term for all platforms that help you develop AI applications. Many of them are built around RAG, but many also provide other toolings such as monitoring, evaluation, etc.

**Agent** is a weird category, as many agent toolings are just sophisticated prompt engineering with potentially constrained generation (e.g. the model can only output the predetermined action) and plugin integration (e.g. to let the agent use tools).

![AI engineering stack over time](https://huyenchip.com/assets/pics/ai-oss/5-ai-engineering.png)

#### Model development

Pre-ChatGPT, the AI stack was dominated by model development. Model development’s biggest growth in 2023 came from increasing interest in inference optimization, evaluation, and parameter-efficient finetuning (which is grouped under Modeling & training).

Inference optimization has always been important, but the scale of foundation models today makes it crucial for latency and cost. The core approaches for optimization remain the same (quantization, low-ranked factorization, pruning, distillation), but many new techniques have been developed especially for the transformer architecture and the new generation of hardware. For example, in 2020, 16-bit quantization was considered state-of-the-art. Today, we’re seeing [2-bit quantization](https://arxiv.org/abs/2212.09720) and [even lower than 2-bit](https://arxiv.org/abs/2402.17764).

Similarly, evaluation has always been essential, but with many people today treating models as blackboxes, evaluation has become even more so. There are many new evaluation benchmarks and evaluation methods, such as comparative evaluation (see [Chatbot Arena](https://huyenchip.com/2024/02/28/predictive-human-preference.html#correctness_of_chatbot_arena_ranking)) and AI-as-a-judge.

![Model Development Stack Over Time](https://huyenchip.com/assets/pics/ai-oss/6-model-development.png)

#### Infrastructure

Infrastructure is about managing data, compute, and toolings for serving, monitoring, and other platform work. Despite all the changes that generative AI brought, the open source AI infrastructure layer remained more or less the same. This could also be because infrastructure products are typically not open sourced.

The newest category in this layer is vector database with companies like Qdrant, Pinecone, and LanceDB. However, many argue this shouldn’t be a category at all. Vector search has been around for a long time. Instead of building new databases just for vector search, existing database companies like DataStax and Redis are bringing vector search into where the data already is.

## Open source AI developers

Open source software, like many things, follows the long tail distribution. A handful of accounts control a large portion of the repos.

### One-person billion-dollar companies?

845 repos are hosted on 594 unique GitHub accounts. There are 20 accounts with at least 4 repos. These top 20 accounts host 195 of the repos, or 23% of all the repos on the list. These 195 repos have gained a total of 1,650,000 stars.

![Most active GitHub accounts](https://huyenchip.com/assets/pics/ai-oss/7-top-accounts.png)

On Github, an account can be either an organization or an individual. 19/20 of the top accounts are organizations. Of those, 3 belong to Google: `google-research`, `google`, `tensorflow`.

The only individual account in these top 20 accounts is lucidrains. Among the top 20 accounts with the most number of stars (counting only gen AI repos), 4 are individual accounts:

- [lucidrains](https://github.com/lucidrains) (Phil Wang): who can implement state-of-the-art models insanely fast.
- [ggerganov](https://github.com/ggerganov) (Georgi Gerganov): an optimization god who comes from a physics background.
- [Illyasviel](https://github.com/lllyasviel) (Lyumin Zhang): creator of Foocus and ControlNet who’s currently a Stanford PhD.
- [xtekky](https://github.com/xtekky): a full-stack developer who created gpt4free.

![Most active GitHub accounts](https://huyenchip.com/assets/pics/ai-oss/8-top-accounts-stars.png)

Unsurprisingly, the lower we go in the stack, the harder it is for individuals to build. Software in the infrastructure layer is the least likely to be started and hosted by individual accounts, whereas more than half of the applications are hosted by individuals.

![Can you do this alone?](https://huyenchip.com/assets/pics/ai-oss/9-indie.png)

Applications started by individuals, on average, have gained more stars than applications started by organizations. Several people have speculated that we’ll see many very valuable one-person companies (see [Sam Altman’s interview](https://fortune.com/2024/02/04/sam-altman-one-person-unicorn-silicon-valley-founder-myth/) and [Reddit discussion](https://www.reddit.com/r/ChatGPT/comments/1ajwj5z/one_person_billion_dollar_company/)). I think they might be right.

![Can you do this alone?](https://huyenchip.com/assets/pics/ai-oss/10-indie-stars.png)

### 1 million commits

Over 20,000 developers have contributed to these 845 repos. In total, they’ve made almost a million contributions!

Among them, the 50 most active developers have made over 100,000 commits, averaging over 2,000 commits each. See the full list of the top 50 most active open source developers [here](https://huyenchip.com/llama-devs).

![Most active open source developers](https://huyenchip.com/assets/pics/ai-oss/11-devs.png)

## The growing China's open source ecosystem

It’s been known for a long time that China’s AI ecosystem has diverged from the US (I also mentioned that in a [2020 blog post](https://huyenchip.com/2020/12/27/real-time-machine-learning.html#mlops_china_vs_us)). At that time, I was under the impression that GitHub wasn’t widely used in China, and my view back then was perhaps colored by China’s 2013 ban on GitHub.

However, this impression is no longer true. There are many, many popular AI repos on GitHub targeting Chinese audiences, such that their descriptions are written in Chinese. There are repos for models developed for Chinese or Chinese + English, such as [Qwen](https://github.com/QwenLM/Qwen), [ChatGLM3](https://github.com/THUDM/ChatGLM3), [Chinese-LLaMA](https://github.com/ymcui/Chinese-LLaMA-Alpaca).

While in the US, many research labs have moved away from the RNN architecture for language models, the RNN-based model family [RWKV](https://github.com/BlinkDL/RWKV-LM) is still popular.

There are also AI engineering tools providing ways to integrate AI models into products popular in China like WeChat, QQ, DingTalk, etc. Many popular prompt engineering tools also have mirrors in Chinese.

Among the top 20 accounts on GitHub, 6 originated in China:

1. [THUDM](https://github.com/THUDM): Knowledge Engineering Group (KEG) & Data Mining at Tsinghua University.
2. [OpenGVLab](https://github.com/OpenGVLab): General Vision team of Shanghai AI Laboratory
3. [OpenBMB](https://github.com/OpenBMB): Open Lab for Big Model Base, founded by ModelBest & the NLP group at Tsinghua University.
4. [InternLM](https://github.com/InternLM): from Shanghai AI Laboratory.
5. [OpenMMLab](https://github.com/open-mmlab): from The Chinese University of Hong Kong.
6. [QwenLM](https://github.com/QwenLM): Alibaba’s AI lab, which publishes the Qwen model family.

## Live fast, die young

One pattern that I saw last year is that many repos quickly gained a massive amount of eyeballs, then quickly died down. Some of my friends call this the “hype curve”. Out of these 845 repos with at least 500 GitHub stars, 158 repos (18.8%) haven’t gained any new stars in the last 24 hours, and 37 repos (4.5%) haven’t gained any new stars in the last week.

Here are examples of the growth trajectory of two of such repos compared to the growth curve of two more sustained software. Even though these two examples shown here are no longer used, I think they were valuable in showing the community what was possible, and it was cool that the authors were able to get things out so fast.

![Hype curve](https://huyenchip.com/assets/pics/ai-oss/12-hype-curve.png)

## My personal favorite ideas

So many cool ideas are being developed by the community. Here are some of my favorites.

- Batch inference optimization: [FlexGen](https://github.com/FMInference/FlexGen), [llama.cpp](https://github.com/ggerganov/llama.cpp/pull/1375)
- Faster decoder with techniques such as [Medusa](https://github.com/FasterDecoding/Medusa), [LookaheadDecoding](https://github.com/hao-ai-lab/LookaheadDecoding)
- Model merging: [mergekit](https://github.com/cg123/mergekit)
- Constrained sampling: [outlines](https://github.com/outlines-dev), [guidance](https://github.com/guidance-ai/guidance), [SGLang](https://github.com/sgl-project/sglang)
- Seemingly niche tools that solve one problem really well, such as [einops](https://github.com/arogozhnikov/einops) and [safetensors](https://github.com/huggingface/safetensors).

## Conclusion

Even though I included only 845 repos in my analysis, I went through several thousands of repos. I found this helpful for me to get a big-picture view of the seemingly overwhelming AI ecosystem. I hope the [list](https://goodailist.com) is useful for you too. Please do let me know what repos I’m missing, and I’ll add them to the list!
