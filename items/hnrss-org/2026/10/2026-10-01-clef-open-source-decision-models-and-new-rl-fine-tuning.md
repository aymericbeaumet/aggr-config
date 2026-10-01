---
title: 'Clef: Open-source decision models, and new RL fine-tuning platform'
link: https://blog.cloudflare.com/clef-decision-models/
source: hnrss-org
published: 2026-10-01T16:18:57Z
updated: 2026-10-01T16:18:57Z
first_seen: 2026-10-01T19:33:44.897168732Z
authors:
- jasondavies
content: extracted
html: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.html
preview:
  file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.preview-355cd115f942.webp
  width: 256
  height: 134
  color: '#f6eae5'
images:
- source: https://blog.cloudflare.com/_emdash/api/media/file/01M3TJV43SPQCPKJ6GBXFCDKNE.01M3TJV53VYDMVNCZDPH1FBFYN.png
  original:
    file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-32d7d9025bea.png
    width: 1200
    height: 628
  variants:
  - file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-076476ce46e4.webp
    width: 320
    height: 167
  - file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-6929eafb82a7.webp
    width: 640
    height: 335
  - file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-a256389e76c7.webp
    width: 960
    height: 502
  - file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-b85f7380b2c6.webp
    width: 1200
    height: 628
  color: '#fefdfd'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M3W09VBACVT4420YA2SDXZE8.01M3W09YK4NRJQNV2S8HMMCQF0.png&w=1430&h=588&f=webp&fit=cover&position=center
  original:
    file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-49c229a8e1f0.webp
    width: 1430
    height: 588
  variants:
  - file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-296729c98ec2.webp
    width: 320
    height: 132
  color: '#fdfdfd'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M3TJPNG3PJHDJ88KJZJ7Q29T.png&w=1430&h=752&f=webp&fit=cover&position=center
  original:
    file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-f9681b8af3ec.webp
    width: 1430
    height: 751
  variants:
  - file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-87c150097845.webp
    width: 320
    height: 168
  color: '#fefefd'
- source: https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M3TJPQEG983EA9PS20PQZS6R.png&w=1430&h=926&f=webp&fit=cover&position=center
  original:
    file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-27f1a6244fc2.webp
    width: 1430
    height: 925
  variants:
  - file: 2026-10-01-clef-open-source-decision-models-and-new-rl-fine-tuning.image-426972b59ebe.webp
    width: 320
    height: 207
  color: '#fefdfd'
---

Over the last few weeks, there has been lots of buzz around decision models such as [Typesafe AI’s Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) System One model. While classifier models have been around for some time, Jev introduces a new decision model concept into the world of AI — a model that produces bounded structured outputs cheaply, quickly and consistently that can be added into a workflow when a decision is required. These models are capable enough to work over any set of inputs without constantly retraining the model to incorporate new classification categories. This contrasts with the world of Large Language Models (LLMs), which are largely non-deterministic, but are open-ended enough to reason and generate text and tool calls for agentic workloads.

Today, we’re releasing two Cloudflare-trained decision models, Clef and Clef-flash, [hosted on Workers AI](https://developers.cloudflare.com/workers-ai/models/clef). Clef is currently the leader when evaluated against the [Jev Decision Index](https://huggingface.co/spaces/multimodalart/jev-decision-index), you can view full results on the [live benchmark demo site](https://clef-evals.workers-ai-mle.workers.dev). These models are smarter, faster, and fully Jev-API compatible, so you can experiment with these hosted models easily. We’re fully open-sourcing these [models on Hugging Face](https://huggingface.co/Cloudflare/clef) under an Apache 2.0 license for you to run locally and experiment with yourselves.

![decision-index-vs-latency.png](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M3W09VBACVT4420YA2SDXZE8.01M3W09YK4NRJQNV2S8HMMCQF0.png&w=1430&h=588&f=webp&fit=cover&position=center)

Lastly, we’re excited to debut our new reinforcement learning (RL) product, which allows customers to fine-tune Clef to suit their use cases as well.

## What is a decision model?

A decision model makes classifications to help agents decide how to act, based on certain probabilities. For example, you can pass in a customer support message (inputs) and ask if it is urgent and which team should handle it. A decision model will return typed answers with probabilities (outputs), which your code can use to route the ticket, trigger an escalation, or defer to a human. This means that a human does not necessarily need to be in the loop for agentic decisions anymore — agents can programmatically gather context, make decisions, and take actions on tasks, or defer to a human when needed.

![](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M3TJPNG3PJHDJ88KJZJ7Q29T.png&w=1430&h=752&f=webp&fit=cover&position=center)

Specifically at Cloudflare, we’ve been testing our new Clef model on our Threat Intelligence team to help us classify website domains. By giving a domain to Clef (with Browser Run) it can quickly identify categories that the domain falls under — for example, it might classify a domain with a 95% chance it is a fashion website, 85% ecommerce, <1% phishing, etc. This classification took our Clef model 2.2s to fetch, render, and classify the website. In contrast, our fastest general LLM gpt-oss-120b took 4.7s in the same workflow, and only returned two classifications. As a user, you can imagine how a 2x savings in latency and results can help us improve our threat intelligence workflows and be faster in identifying malicious or legitimate domains. Generalize this to any use case where you need to make quick programmatic decisions, and you unlock powerful agentic workflows that are able to autonomously decide, reason, and execute.

In music theory, a clef is a symbol placed at the beginning of a musical staff that assigns specific pitch names to the lines and spaces. A decision model is analogous to a music clef because it helps define the domain of the context and the subsequent notes (actions) that follow it. We chose Clef as the name of our family of decision models, as it serves similar purposes, and the CF hearkens to Cloudflare.

## How is Clef different from other decision models?

Although the market is getting increasingly saturated with decision models, Clef has some unique properties that make us excited to release it to the public. First, it has a vision encoder so it’s able to take in images and classify visual content. This is different from Jev, which only does text classification today. Secondly, our model has a 64k context window (compared to Jev’s 32k), which allows users to squeeze more input state for the model to classify against.

Third, our model is accurate and powerful, scoring competitively against other decision models on the market across various quality benchmarks. We shortlisted some evaluations below that are important for decision-making as defined by the [Jev Decision Index](https://huggingface.co/spaces/multimodalart/jev-decision-index) and scored some of the more popular models on the market for it. Check out the table below for benchmarks, or view the scores on our [live decision index demo site](https://clef-evals.workers-ai-mle.workers.dev):

|                              |                                                     |                                                                 |                                                                            |                        |                                                          |                                                            |
| ---------------------------- | --------------------------------------------------- | --------------------------------------------------------------- | -------------------------------------------------------------------------- | ---------------------- | -------------------------------------------------------- | ---------------------------------------------------------- |
| **Benchmark**                | [**Clef**](https://huggingface.co/Cloudflare/clef) | [**Clef-flash**](https://huggingface.co/Cloudflare/clef-flash) | [**Jev**](https://typesafe.ai/blog/introducing-system-one-models-and-jev) | **DiffusionGemma Jev** | [**Kev 9B**](https://huggingface.co/jaredpalmer/kev-9b) | [**Laya**](https://huggingface.co/convaiinnovations/laya) |
| BFCL · case exact            | 98.47                                               | **98.76**                                                       | 95.75                                                                      | 96.52                  | 94.51                                                    | 38.13                                                      |
| ToolRet · nDCG@10            | **69.19**                                           | 66.43                                                           | 65.28                                                                      | 61.21                  | 64.26                                                    | 12.69                                                      |
| API-Bank · accuracy          | 91.93                                               | **93.11**                                                       | 88.19                                                                      | 83.66                  | 56.30                                                    | 11.41                                                      |
| Home appliances · case exact | 82.95                                               | **97.73**                                                       | 52.27                                                                      | 42.05                  | 25.00                                                    | 0.00                                                       |
| When2Call · accuracy         | 72.37                                               | 65.58                                                           | **80.97**                                                                  | 75.44                  | 49.62                                                    | 11.94                                                      |
| BANKING77 · macro-F1         | **94.20**                                           | 90.93                                                           | 79.74                                                                      | 74.28                  | 84.83                                                    | 14.29                                                      |
| CLINC150+OOS · macro-F1      | **97.43**                                           | 66.77                                                           | 89.27                                                                      | 83.49                  | 79.03                                                    | 3.19                                                       |
| BRIGHT · nDCG@10             | 45.91                                               | 39.26                                                           | **47.52**                                                                  | 42.94                  | 38.53                                                    | 19.90                                                      |
| Amazon ESCI · macro-F1       | **57.48**                                           | 57.39                                                           | 55.21                                                                      | 53.37                  | 49.22                                                    | 24.40                                                      |
| PhishNChips · accuracy       | 79.60                                               | 75.05                                                           | 62.55                                                                      | **85.35**              | 50.75                                                    | 50.15                                                      |

We also ran benchmarks across [Typesafe’s own eval suite](https://huggingface.co/collections/typesafe/workflowevals) and our Clef models fared well, beating Jev in 3 out of 4 areas. Notably, our Clef-flash performs exceptionally well, given how much faster it is.

|                           |                                                     |                                                                 |                                                                            |
| ------------------------- | --------------------------------------------------- | --------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Workflow**              | [**Clef**](https://huggingface.co/Cloudflare/clef) | [**Clef-flash**](https://huggingface.co/Cloudflare/clef-flash) | [**Jev**](https://typesafe.ai/blog/introducing-system-one-models-and-jev) |
| Invoice processing        | **64.7**                                            | 57.1                                                            | 61.8                                                                       |
| Customer service          | 76.3                                                | **77**                                                          | 76.0                                                                       |
| Security incidents        | **62.9**                                            | 61.7                                                            | 61.7                                                                       |
| Agent trace observability | 68.5                                                | 69.8                                                            | **71.6**                                                                   |

Across the 43 eval benchmarks that we ran, our Clef models beat the decision models on latency (except for Laya which is very fast but trades off quality in the benchmarks above):

|                      |                                                     |                                                                 |                                                                            |                        |                                                          |                                                            |
| -------------------- | --------------------------------------------------- | --------------------------------------------------------------- | -------------------------------------------------------------------------- | ---------------------- | -------------------------------------------------------- | ---------------------------------------------------------- |
| **Benchmark**        | [**Clef**](https://huggingface.co/Cloudflare/clef) | [**Clef-flash**](https://huggingface.co/Cloudflare/clef-flash) | [**Jev**](https://typesafe.ai/blog/introducing-system-one-models-and-jev) | **DiffusionGemma Jev** | [**Kev-9B**](https://huggingface.co/jaredpalmer/kev-9b) | [**Laya**](https://huggingface.co/convaiinnovations/laya) |
| Median latency  · ms | 209.3                                               | 38.8                                                            | 524.1                                                                      | 84.4                   | 51.4                                                     | **5.8**                                                    |
| p95 latency · ms     | 238.6                                               | **122.4**                                                       | 536.0                                                                      | 211.2                  | 187.9                                                    | 222.5                                                      |

On top of the latency benefits from the model itself, our Clef models are hosted on Workers AI. Because they are hosted on Cloudflare’s infrastructure, we’re able to take advantage of our GPUs at the edge, leading to low network latency and faster decisions. This means that you could put Clef into the hot path for agents to make decisions and combine that with one of our LLMs on Workers AI to take action.

```
curl https://api.cloudflare.com/client/v4/accounts/$CLOUDFLARE_ACCOUNT_ID/ai/run/@cf/cloudflare/clef \\
  -X POST \\
  -H "Authorization: Bearer $CLOUDFLARE_AUTH_TOKEN" \\
  -d '{
    "model": "clef",
    "state": "Checkout has been failing for every customer for the last hour.",
    "questions": {
      "urgent": { "type": "noul", "instructions": "Is this support request urgent?" },
      "team": {
        "type": "choice",
        "instructions": "Which team should handle this request?",
        "criteria": {
          "billing": "Payments, invoices, and refunds",
          "technical": "Outages, errors, and configuration",
          "sales": "Plans and upgrades"
        }
      },
      "severity": {
        "type": "score",
        "instructions": "How severe is the customer impact?",
        "criteria": ["No impact", "Minor", "Major", "Critical"]
      }
    }
  }'
```

Clef also produces strictly typed outputs similar to Jev and is fully API-compatible, so you can make the swap extremely easily. The larger Clef model is your more powerful precision model, while the Clef-Flash model is great for latency-critical decisions. The models are enterprise-ready with our guarantee that we don’t read, store, or train on your requests or responses (unless you want to use our fine-tuning product, which we go into below). You can get started with the Clef models today, starting with our [developer documentation](https://developers.cloudflare.com/workers-ai/models/clef) or play around with the [open-source model on the Hugging Face repo.](https://huggingface.co/Cloudflare/clef)

If you’d like help tuning Clef for a specific workload, we are also offering fine-tuning services — first as a hands-on partner with our forward-deployed engineer (FDE) team, and then later as a self-serve fine-tuning platform for customers to train and redeploy the model onto Cloudflare.

### How we trained Clef

In the same week that Jev came out, we [posted about some experiments](https://x.com/michellechen/status/2101091012559151480) we had with our own homegrown decision model. Our demo goes into how we adapted the DiffusionGemma model to output deterministic probabilities by exposing the logprobs that are generated by a large language model. Our initial approach built upon independent research by [Matt Mastracci](https://x.com/mmastrac), who has been active in the machine learning (ML) community with sharing new ideas and [pull requests to vLLM](https://github.com/vllm-project/vllm/pull/57250) inference engine to make DiffusionGemma support stronger.

Clef builds upon this concept, but uses a different base model as the backbone. We currently use Qwen as the base model and post-trained it to suit decision model use cases. During inference, Clef uses Qwen for a prefill-only pass, then scores the valid schema choices in parallel. The decision step is non-autoregressive, so there’s no intermediate text to generate token by token, making Clef significantly faster than autoregressive LLMs. Rather than generating intermediate text to produce structured answers, Clef and Clef-flash derive schema choices directly from internal backbone representations. This approach relies on a specialized two-stage attention routing process: every valid choice extracts context relevant to the prompt, allowing individual field parameters to cross-attend with other fields and back to the original payload prior to scoring. By leveraging a lexical prior, the model preserves semantic intent across options. Ultimately, the architecture unites option-specific evidence routing, joint cross-field attention, and schema-bound scoring.

By freezing Qwen3.8-27B for Clef and Qwen3.5-9B for Clef-flash, we jointly optimized the routing head alongside rank-256 low-rank adapters. Our post-training utilizes label-smoothed cross-entropy for valid schema outputs paired with a Brier loss to refine probability calibration. This training leverages our own internal synthetic datasets permutating field orders, prompts, and schema structures. We also developed Reinforcement Learning for Calibrated Decisions (RLCD) to serve as a secondary optimization target, granting partial credit to adjacent ordinal choices, rewarding fully precise record outputs, and applying a reference penalty to prevent distribution shift, giving us better accuracy and generalization.

This means that we were able to achieve a few novel things with Clef: we improved accuracy of the model in classification, constrained it to output only probabilities instead of text generation, and made it faster than Jev and the base Qwen models.

### How fine-tuning can extend the capabilities of Clef

We heard a lot of internal use cases that required fine-tuning our Clef model to be built into our agentic workflows at Cloudflare. For example, internal teams want a classifier model to be able to evaluate Trust & Safety submissions, help us triage Cloudflare Support requests, or even to be built-in to our Bot products to decide if a crawler is a good bot or bad bot.

These use cases are incredibly specific and we have had many years of labelled decisions that we could use to train a specific classifier. When you fine-tune a model, you may give up some general purpose performance in exchange for higher accuracy in a specific domain.. Because Cloudflare has more than 15 years of network data across different domains, we can fine-tune a model to fit these specific use cases which is more accurate and faster than our generic Clef model. We’re working with internal teams already to figure out how we can post-train Clef to create powerful ML models that boost our impact and improve workflows across Cloudflare. These internal teams and use cases are the next remit of our new FDE fine-tuning team and basis for our reinforcement learning (RL) product.

### Our new RL service

We are offering a service to help customers fine-tune Clef to suit their workloads with our hands-on FDE team. From that, we’ll learn from our hands-on experiences to build a self-serve platform that customers can use to capture data, fine-tune, and redeploy the model, all on Cloudflare.

This has actually been a long time coming — we’ve been building our AI platform to have the right primitives where we could be building a custom RL product. The interest in Jev shows the need for a fast, small, specific, classifier model, and we chose this to be our niche to start experimenting with RL environments.

To do this, we leverage the primitives that we already have built on our Cloudflare platform:

- Cloudflare AI Gateway – pass all your AI traffic through AI Gateway and automatically create a dataset of requests for your use case
- Cloudflare Workers AI – generate rollouts against the base Clef model
- Cloudflare Containers – RL sandbox for scoring and replaying agent actions
- \[NEW\] Trainer – update weights of fine-tuned Clef model
- Cloudflare Workers AI + BYO Model – redeploy the fine-tuned model on Workers AI

This combines a few work-in-progress pieces of the AI Platform that we’ve been working on, including AI Gateway that captures your AI traffic so you can leverage your own request/response data, Containers for RL Sandboxes, and Workers AI’s Bring Your Own Model (Cog) work that has been progressing since our acquisition of Replicate.

![](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M3TJPQEG983EA9PS20PQZS6R.png&w=1430&h=926&f=webp&fit=cover&position=center)

### Try it out today

We’re excited to launch our first Cloudflare-trained ML model from the Workers AI team today. We’re still early here and have a lot more improvements in store, but it is a wonderful first showcase of the hard work we’ve been doing on the AI Platform team. We believe that Clef has the ability to disrupt the way we use agents, which fits naturally into Cloudflare’s mission of being the agent cloud.

If you have specific use cases and are already customers of these products — [we’d love to chat with you and be design partners as we experiment in this space.](https://www.cloudflare.com/resource/clef-rl-interest)

Try out the Clef models hosted on Workers AI, download the [weights on Hugging Face](https://huggingface.co/Cloudflare/clef) if you’d like to explore for yourself, and reach out if you have fine-tuning use cases you’d like us to help with.

Our ML team has been growing in impact, from model optimizations to model training research. If you’re interested in joining our mission, [check out our open roles](https://www.cloudflare.com/careers/).
