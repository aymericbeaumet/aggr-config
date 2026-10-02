---
title: Context Language Models
link: https://arxiv.org/abs/2609.37725
source: hnrss-org
published: 2026-10-01T14:51:33Z
updated: 2026-10-01T14:51:33Z
first_seen: 2026-10-02T02:35:29.351097324Z
authors:
- emersonmacro
content: extracted
html: 2026-10-01-context-language-models.html
preview:
  file: 2026-10-01-context-language-models.preview-41145fe12e79.webp
  width: 256
  height: 149
  alt: arXiv logo
  color: '#ece6e5'
images:
- source: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  original:
    file: 2026-10-01-context-language-models.image-88f5088404c9.png
    width: 1200
    height: 700
  variants:
  - file: 2026-10-01-context-language-models.image-fc6386ce1e2e.webp
    width: 320
    height: 187
  - file: 2026-10-01-context-language-models.image-49eabc2e18a4.webp
    width: 640
    height: 373
  - file: 2026-10-01-context-language-models.image-f275dcf4bd8a.webp
    width: 1200
    height: 700
  color: '#fefefe'
---

Authors: [Rulin Shao](https://arxiv.org/search/cs?searchtype=author&query=Shao,+R), [Shannon Zejiang Shen](https://arxiv.org/search/cs?searchtype=author&query=Shen,+S+Z), [Junjie Oscar Yin](https://arxiv.org/search/cs?searchtype=author&query=Yin,+J+O), [Yuetai Li](https://arxiv.org/search/cs?searchtype=author&query=Li,+Y), [Minheng Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang,+M), [Hamish Ivison](https://arxiv.org/search/cs?searchtype=author&query=Ivison,+H), [Radha Poovendran](https://arxiv.org/search/cs?searchtype=author&query=Poovendran,+R), [Nathan Lambert](https://arxiv.org/search/cs?searchtype=author&query=Lambert,+N), [Teng Xiao](https://arxiv.org/search/cs?searchtype=author&query=Xiao,+T), [Mike Lewis](https://arxiv.org/search/cs?searchtype=author&query=Lewis,+M), [Wen-tau Yih](https://arxiv.org/search/cs?searchtype=author&query=Yih,+W), [Luke Zettlemoyer](https://arxiv.org/search/cs?searchtype=author&query=Zettlemoyer,+L), [Pang Wei Koh](https://arxiv.org/search/cs?searchtype=author&query=Koh,+P+W)

[View PDF](https://arxiv.org/pdf/2609.37725) [HTML (experimental)](https://arxiv.org/html/2609.37725v1)

> Abstract:We introduce Context Language Models (CLMs), language models that natively manage their own context. We implement this by treating the context as a file and allowing the model to make unrestricted updates to this file. This allows the model to learn what is most important to maintain in context, and naturally extends to multi-agent systems where multiple agent contexts coexist as files. Building CLMs zero-shot with existing models outperforms SOTA context management strategies across a variety of tasks: 11.4% higher accuracy with 21.5% fewer FLOPs on BrowseComp-Plus, 5% higher scores with 59% fewer FLOPs on 12-hour EdgeBench, and 65% greater improvement with the same compute on a 24-hour multi-repository agent-swarm task. Moreover, by shifting context management from external harness control to intrinsic model behavior, CLMs naturally enable both in-context and parametric learning of context-management strategies. We show that CLMs can be steered with natural-language instructions evolved through a standard skill-optimization loop, improving held-out accuracy by up to 35.9 points on a context-management task while reducing compute. We also introduce an online reinforcement learning method for CLMs, improving Qwen3.5-9B performance on BrowseComp-Plus by 47.6% while using 12% fewer FLOPs. Finally, we co-design Suffix Cache Reuse for CLM serving, further reducing server-side compute by 35% relative to standard SGLang at matched performance.

|           |                                                                                                                                               |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Subjects: | Artificial Intelligence (cs.AI); Computation and Language (cs.CL); Machine Learning (cs.LG)                                                   |
| Cite as:  | [arXiv:2609.37725](https://arxiv.org/abs/2609.37725) \[cs.AI\]                                                                               |
|           | (or [arXiv:2609.37725v1](https://arxiv.org/abs/2609.37725v1) \[cs.AI\] for this version)                                                     |
|           | [https://doi.org/10.48550/arXiv.2609.37725](https://doi.org/10.48550/arXiv.2609.37725)  arXiv-issued DOI via DataCite (pending registration) |

## Submission history

From: Rulin Shao \[[view email](https://arxiv.org/show-email/0e649254/2609.37725)\] \
 **\[v1\]** Tue, 29 Sep 2026 14:50:08 UTC (3,068 KB)
