---
title: Introducing our Artifacts Hub and Adoption Dashboard
link: https://www.interconnects.ai/p/introducing-our-artifacts-hub-and
source: interconnects-ai
published: 2026-08-03T14:03:08Z
updated: 2026-08-03T14:03:08Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Nathan Lambert
summary: Scaling our curation and measurement of the open ecosystem.
content: extracted
html: 2026-08-03-introducing-our-artifacts-hub-and-adoption-dashboard.html
preview:
  file: 2026-08-03-introducing-our-artifacts-hub-and-adoption-dashboard.preview-98e105b4a17c.webp
  width: 256
  height: 141
  color: '#eaebea'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/dff5e29d-7c81-4f81-a64b-10d9b9bbecbc_2856x1577.png
  original:
    file: 2026-08-03-introducing-our-artifacts-hub-and-adoption-dashboard.image-356650931c52.png
    width: 2856
    height: 1577
  color: '#fbfbfa'
- source: https://substackcdn.com/image/fetch/$s_!SS_C!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05be06c4-ba9f-4245-a8a1-2ce44fce18eb_2914x1808.png
  original:
    file: 2026-08-03-introducing-our-artifacts-hub-and-adoption-dashboard.image-e5c8fa74487f.jpg
    width: 1456
    height: 903
  color: '#fbfaf9'
- source: https://substackcdn.com/image/fetch/$s_!Dpf-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F789e2fc2-5474-443f-88c2-569d809afd80_1676x2310.png
  original:
    file: 2026-08-03-introducing-our-artifacts-hub-and-adoption-dashboard.image-8a914fc87930.png
    width: 1456
    height: 2007
  color: '#fbfaf9'
- source: https://substackcdn.com/image/fetch/$s_!d2WL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F12533d49-cbf1-4d2e-8eb2-fe5da0d500bd_2252x2702.png
  original:
    file: 2026-08-03-introducing-our-artifacts-hub-and-adoption-dashboard.image-f0a98ab08c25.jpg
    width: 1456
    height: 1747
  color: '#fbfbfb'
---

##### *No voiceover for this quick post, but a quick video version is [here](https://youtu.be/vSXTiEepGqM).*

We’re expanding our open models coverage into standalone projects that let you go deeper on the state of the open model ecosystem. The new free sources of data are:

1. The [Artifacts Hub](https://artifactshub.ai/) — a curated view of the models trending on Hugging Face, highlighting inference tokens via [Open Router](https://openrouter.ai/), model intelligence via [Artificial Analysis](https://artificialanalysis.ai/), and our [tailored adoption metrics](https://atomproject.ai/relative-adoption-metric) building on top of [Hugging Face](https://huggingface.co/) ’s data.

2. Our [Adoption Dashboard](https://dashboard.interconnects.ai/) — a living dashboard of download and derivative model numbers by geography and organization. This highlights the US-China gap and growing players in the open ecosystem.

To date, our primary efforts on Interconnects have been release recaps for popular models like [Kimi K3](https://www.interconnects.ai/p/kimi-k3-the-open-weights-escalation), [GLM 5.2](https://www.interconnects.ai/p/glm-52-is-the-step-change-for-open), [DeepSeek R1](https://www.interconnects.ai/p/deepseek-r1-recipe-for-o1), etc. and monthly round-ups of the open models that matter, [Artifacts Log](https://www.interconnects.ai/t/artifacts-log). We’re expanding on these, building on the tools and internal data we’ve collected for other projects like [The ATOM Project](https://atomproject.ai/) (and [report](https://arxiv.org/abs/2604.07190)). This allows us to capture our ecosystem view of open models, develop methods for understanding adoption of giant MoE models, and everything in between. We’re sharing them freely to help the open ecosystem find its strengths and grow.

[Visit the Artifacts Hub](https://artifactshub.ai/)

The Artifacts Hub right now covers 792 models released in the last two years, across the core text-focused language models and multimodal generative models. At Interconnects we follow the data of every model on Hugging Face, analyze the core few thousand LLMs (this [list](https://github.com/Interconnects-AI/tracked-models) is public on GitHub and regularly updated), and hand select these core few hundred for further explanation.

[![](https://substackcdn.com/image/fetch/$s_!SS_C!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05be06c4-ba9f-4245-a8a1-2ce44fce18eb_2914x1808.png "")](https://substackcdn.com/image/fetch/$s_!SS_C!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F05be06c4-ba9f-4245-a8a1-2ce44fce18eb_2914x1808.png)

We built the Hub as a way to go deeper on this analysis in collaboration with [Project VAIL](https://www.projectvail.com/) — an AI verification startup who has been one of the most loyal fans of our open model curation.

For the most popular models, the Hub let’s you quickly see how far behind the model was in terms of frontier intelligence based on Artificial Analysis’s Intelligence Index, compare Hugging Face and Open Router adoption to similar models, glance at relative adoption metric (RAM) scores for time-size normalized downloads, or look at the [VAIL similarity index](https://arxiv.org/abs/2603.19022) of models with related generations. A snapshot for what you’d see for something like GLM-5.2 is below.

[![](https://substackcdn.com/image/fetch/$s_!Dpf-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F789e2fc2-5474-443f-88c2-569d809afd80_1676x2310.png)](https://substackcdn.com/image/fetch/$s_!Dpf-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F789e2fc2-5474-443f-88c2-569d809afd80_1676x2310.png)

Our other project is much lighter weight, but far overdue. Ever since we wrote The ATOM Project, we’ve been seeing the US-vs-China model adoption plot on a recurring basis in the AI ecosystem. We’d update the plot from time to time, but not enough. Now, we’re making the crucial data for that report and the ecosystem available in a daily updating dashboard.

[Visit our dashboard](https://dashboard.interconnects.ai/)

[![](https://substackcdn.com/image/fetch/$s_!d2WL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F12533d49-cbf1-4d2e-8eb2-fe5da0d500bd_2252x2702.png)](https://substackcdn.com/image/fetch/$s_!d2WL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F12533d49-cbf1-4d2e-8eb2-fe5da0d500bd_2252x2702.png)

It’s core to our mission at Interconnects to enable the open ecosystem. Right now, as the world figures out how to use open models productively — especially in cost-competitive ways to frontier models — providing more transparency on what is happening is the best way for us to figure out what is working.

We’d love to hear how we can make this better, please get in touch. We’re also interested in how others could use our curated data for other products or research in the open ecosystem. Get in touch at `mail@interconnects.ai`.

*Thanks again to the teams at Hugging Face, Open Router, and Artificial Analysis for making this possible — most of all the Hugging Face. Thank you to VAIL for the motivation and support in making these projects.*
