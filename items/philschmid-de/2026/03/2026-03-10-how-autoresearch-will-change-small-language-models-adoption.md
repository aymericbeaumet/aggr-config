---
title: How Autoresearch will change Small Language Models adoption
link: https://www.philschmid.de/autoresearch
source: philschmid-de
published: 2026-03-10T00:00:00Z
updated: 2026-03-10T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Autoresearch lets an AI agent run hundreds of model training experiments overnight. Learn how it works, early results from Karpathy and Shopify, and how to apply it.
content: extracted
html: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.html
preview:
  file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.preview-ba6af9a665df.webp
  width: 256
  height: 136
  alt: How Autoresearch will change Small Language Models adoption
  color: '#121212'
images:
- source: https://www.philschmid.de/static/blog/autoresearch/thumbnail.jpg
  original:
    file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-af601fe56d2f.jpg
    width: 1700
    height: 900
  color: '#000000'
- source: https://www.philschmid.de/static/blog/autoresearch/mermaid.png
  original:
    file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-8886a7218f23.png
    width: 2781
    height: 4934
  variants:
  - file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-48a98e8b34aa.webp
    width: 320
    height: 568
  - file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-abcd5a5994ac.webp
    width: 640
    height: 1135
  - file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-580f206a68ee.webp
    width: 960
    height: 1703
  - file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-cf6040d6e85b.webp
    width: 1280
    height: 2271
  - file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-23900584d318.webp
    width: 1600
    height: 2839
  - file: 2026-03-10-how-autoresearch-will-change-small-language-models-adoption.image-7daa7a65de2e.webp
    width: 2781
    height: 4934
  color: '#f1fef5'
---

What if you could optimize a model overnight without any ML experience? What if an AI agent runs hundreds of training experiments autonomously, keeping only the improvements? That is the idea behind [autoresearch](https://github.com/karpathy/autoresearch). You give an AI agent a training script and a metric. It edits the code, runs a short experiment, checks if the metric improved, keeps or discards, and repeats. [Karpathy](https://x.com/karpathy/status/2031135152349524125) used it to squeeze 11% more speed out of his GPT-2 training. [Tobi Lütke](https://x.com/tobi/status/2030771823151853938), Shopify's CEO, trained a 0.8B model overnight that outscored his previous 1.6B.

## How Autoresearch Works

An LLM agent that can edit training code, runs a short experiment, checks if the metric improved, and repeats, without human involvement.

![mermaid](https://www.philschmid.de/static/blog/autoresearch/mermaid.png)

**Design constraints that make it work:**

- **Fixed 5-min time budget.** Results stay comparable regardless of what the agent changes.
- **Single file scope.** Agent edits `train.py` only. Data prep and evaluation are locked down.
- **Git as memory.** Each experiment is a commit. The agent reads branch history to plan what to try next.
- **Binary keep/discard.** No human judgment needed.

Roughly 12 experiments/hour. Around 100 overnight.

## Two Early Experiments

Both examples below are small-scale and early. The setups are minimal, the models are small, and neither is a controlled study. But they show where this is headed.

### Karpathy: 700 experiments on nanochat

Karpathy pointed autoresearch at [nanochat](https://github.com/karpathy/nanochat), his already well-tuned GPT-2 training codebase. Over two days the agent ran ~700 experiments and found ~20 real improvements. Stacked together, time-to-GPT-2 dropped from 2.02 to 1.80 hours (11% faster). The agent caught things Karpathy had missed:

- Parameterless QKNorm had no scaler multiplier, making attention too diffuse
- Value Embeddings had no regularization applied
- Banded attention window was too conservative
- AdamW betas, weight decay schedule, and initialization were all suboptimal

All improvements transferred from depth-12 to depth-24 models.

### Tobi Lütke query expansion overnight

Tobi adapted the pattern for a query-expansion model for the [QMD Open source project](https://github.com/tobi/qmd):

1. Told an AI agent to read the autoresearch repo and build a version for QMD. Get training data from tobi/qmd github.
2. Went to sleep
3. Woke up to a **0.8B model scoring 19% higher** than the previous **1.6B model** after 37 experiments in 8 hours

A smaller model outperformed one twice its size. He then pointed the same loop at a reranker and beat that baseline too.

## How to apply this

The loop hinges on your eval. If the metric is gamed or leaky, the model looks better on paper and fails in production. Your eval set must be held out completely — the agent never touches it, never trains on it, never sees it during optimization. You need a training script the agent can modify, training data (manually labeled or synthetic), and a metric that reflects what the model will actually do in production.

When experiments run 100x faster than a human can manage, your eval becomes the bottleneck. Static benchmarks get saturated. Build your eval pipeline so it can evolve, refresh from real production data, harder edge cases.

The pattern fits search ranking, product categorization, clinical NER, fraud scoring, contract extraction, intent classification, and similar tasks. Small models work well — training runs finish in minutes and improvements transfer when you scale up. Open models like Gemma are a good starting point: small enough for a single GPU, performant for production tasks, commercially licensed.

### How this differs from GEPA and prompt optimizers

[GEPA](https://arxiv.org/abs/2501.09361) (Genetic-Pareto prompt evolution, ICLR 2026) optimizes *prompts* using genetic evolution and reflection. It works on frozen models and APIs where you cannot change weights.

Autoresearch optimizes *weights* — it modifies training code, architectures, and hyperparameters to produce a better model.

- **GEPA / prompt optimization:** No training. Works on frozen models and APIs.
- **Autoresearch / weight optimization:** Changes the model itself. Requires training infra and data.

For teams building domain SLMs, both layers compound.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
