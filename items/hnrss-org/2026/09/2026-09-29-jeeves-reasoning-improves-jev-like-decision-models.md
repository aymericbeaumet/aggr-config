---
title: Jeeves. Reasoning improves Jev-like decision models
link: https://github.com/PostHog/jeeves
source: hnrss-org
published: 2026-09-29T11:13:54Z
updated: 2026-09-29T11:13:54Z
first_seen: 2026-09-29T14:21:22.166029648Z
authors:
- nicowaltz
content: extracted
html: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.html
preview:
  file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.preview-824a0421d34d.webp
  width: 256
  height: 128
  alt: Jeeves – Reasoning improves Jev-like decision models - PostHog/jeeves
  color: '#f1f2f4'
images:
- source: https://opengraph.githubassets.com/8706e1cdd3708c8ca2784d28d6b205ac4ae3a83661a64296d3a5217609370f52/PostHog/jeeves
  original:
    file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-8c2395d39065.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-c41d6e95765a.webp
    width: 320
    height: 160
  - file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-12ab24e101e6.webp
    width: 640
    height: 320
  - file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-a575ef876872.webp
    width: 1200
    height: 600
  color: '#fefefe'
- source: https://github.com/PostHog/jeeves/raw/master/assets/smug.png
  original:
    file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-5b498e91e747.png
    width: 748
    height: 660
  color: '#191613'
- source: https://camo.githubusercontent.com/48c1f021144e4a727c4dc4a4db76058bf8c81cee064867bfc6bc7a2b9eac2190/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f574549474854532d39422d3061306130612e7376673f7374796c653d666f722d7468652d6261646765266c6162656c436f6c6f723d303030303030
  original:
    file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-369af1131b5a.png
    width: 122
    height: 28
  variants:
  - file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-b317a3949d88.webp
    width: 122
    height: 28
  color: '#030303'
- source: https://camo.githubusercontent.com/f84c43d38bb5dbc22cb4db3039bb91f1437293a2caa2888ed0564709e881da2d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4d49542d3061306130612e7376673f7374796c653d666f722d7468652d6261646765266c6162656c436f6c6f723d303030303030
  original:
    file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-5a329064cc44.png
    width: 125
    height: 28
  variants:
  - file: 2026-09-29-jeeves-reasoning-improves-jev-like-decision-models.image-fdfeb156e1f4.webp
    width: 125
    height: 28
  color: '#040404'
---

A reasoning Jev-style classifier with a diffusion drafter, trained with SFT and CISPO.

[![Jeeves](https://github.com/PostHog/jeeves/raw/master/assets/smug.png)](https://github.com/PostHog/jeeves/blob/master/assets/smug.png)

[![Weights: 9B](https://camo.githubusercontent.com/48c1f021144e4a727c4dc4a4db76058bf8c81cee064867bfc6bc7a2b9eac2190/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f574549474854532d39422d3061306130612e7376673f7374796c653d666f722d7468652d6261646765266c6162656c436f6c6f723d303030303030)](https://huggingface.co/PostHog/jeeves) [![License: MIT](https://camo.githubusercontent.com/f84c43d38bb5dbc22cb4db3039bb91f1437293a2caa2888ed0564709e881da2d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4d49542d3061306130612e7376673f7374796c653d666f722d7468652d6261646765266c6162656c436f6c6f723d303030303030)](https://github.com/PostHog/jeeves/blob/master/LICENSE)

## Acknowledgements

Inspired by [Kev](https://github.com/jaredpalmer/kev).

## Highlights

- A 9B Jev-like model (Qwen3.5-9B, LoRA, pointer head) that thinks before it decides, with a block-4 diffusion drafter and the full training code and train/dev/test data.
- Beats Kev-9B and Jev on test data it was never trained on (0.889 vs 0.822 and 0.857) and on JevBench's public tiers (0.935 vs 0.866 for Jev).
- Supports yes/no (`noul`), multiple-choice (`choice`), and rating (`score`) questions in the same request, through a Jev-compatible API.
- About 0.3 s per request without thinking and a 3.3 s median with it on one H100. Can be sped up by truncating chain length.
- Runs on CUDA (Hopper for the FP8 kernel).

## Problem

Jev-like models give calibrated decision probabilities, but at low accuracy. A lot of pipelines therefore rely on a reasoning model as a fallback. Jeeves trains a Jev-like Qwen3.5-9B (LoRA and a pointer head) using CISPO to reason before it decides.

This results in better performance on out of domain tasks, and outperforms Jev in JevBench hard (public).

## Results

Accuracy with thinking, greedy, 2,560-token cap. The Kev-9B and Jev columns are the numbers Kev publishes.

| bench                                                        | Kev-9B    | Jev       | Jeeves    |
| ------------------------------------------------------------ | --------- | --------- | --------- |
| **Test overall** (out-of-domain and held-out, item-weighted) | 0.822     | 0.857     | **0.889** |
| **Transfer overall** (MMLU-Pro and buried state)             | 0.579     | **0.800** | 0.746     |
| **JevBench overall** (231 public items)                      | 0.715\*   | 0.866     | **0.935** |
| QNLI                                                         | **0.925** | **0.925** | 0.913     |
| SciQ                                                         | 0.963     | 0.988     | **0.991** |
| TweetEval offensive                                          | 0.775     | **0.813** | **0.813** |
| PAWS                                                         | 0.763     | 0.788     | **0.875** |
| MMLU                                                         | 0.738     | **0.900** | 0.793     |
| Emotion                                                      | 0.600     | 0.588     | **0.647** |
| Held-out rule structures                                     | 0.896     | 0.885     | **1.000** |
| Contrastive policies                                         | 0.900     | 0.963     | **1.000** |
| MMLU-Pro (10-way)                                            | 0.515     | **0.840** | 0.739     |
| Buried state                                                 | 0.740     | 0.700     | **0.759** |
| Unknowable answered at p ≥ 0.9 (lower is better)             | **0.000** | 0.090     | 0.055     |
| JevBench hard (111 public items)                             | 0.451\*   | 0.730     | **0.865** |
| JevBench ECE (public items)                                  |           | 0.049     | **0.037** |

\* No Kev-9B JevBench result is published. These are Kev-8B (Qwen3).

All JevBench numbers are on the public easy, standard and hard tiers (231 items). The sealed judge tier is not included, and the Jev and Kev numbers are restricted to the same public items.

Without thinking the same checkpoint scores 0.804 on our test split (2,962 items), against 0.840 with it.

## Quickstart

Requirements: Python 3.12 and a CUDA GPU.

```shell
pip install -r requirements.txt
```

Download the released weights and serve them:

```shell
hf download PostHog/jeeves --local-dir jeeves-weights
python -m inference.serve --model jeeves-weights --drafter jeeves-weights/drafter_k4.safetensors --port 8009
```

Or fuse your own trained checkpoint into a standalone model and serve it with a drafter:

```shell
python export.py runs/cispo/final --out runs/fused
python -m inference.serve --model runs/fused --drafter runs/drafter_k4/drafter.safetensors --port 8009
```

Then send a request in Jev's format:

```shell
curl -s localhost:8009/v1/systemone -H 'content-type: application/json' -d '{
  "state": "Shoes arrived two weeks late and in the wrong size. Also I see two charges on my card.",
  "questions": {
    "department":  {"type": "choice", "instructions": "Which team should handle this?",
                    "criteria": {"returns": "Exchanges, refunds, wrong or damaged items",
                                 "shipping": "Delivery status, delays, lost packages",
                                 "billing": "Charges, invoices, payment problems"}},
    "escalate":    {"type": "noul", "instructions": "Does this need urgent human attention?"},
    "frustration": {"type": "score", "instructions": "How frustrated is the customer?",
                    "criteria": ["Calm", "Frustrated", "Very angry"]}
  },
  "options": {"max_think": 512}}'
```

Response on one H100 (FP8), with the three questions thinking in parallel:

```json
{
    "model": "jeeves-latest",
    "answers": {
        "department": {
            "type": "choice",
            "choice": "billing",
            "confidence": 0.19,
            "probabilities": { "returns": 0.4, "shipping": 0.14, "billing": 0.46 }
        },
        "escalate": { "type": "noul", "noul": 0.72 },
        "frustration": {
            "type": "score",
            "score": 1.5,
            "legend": { "0": "Calm", "1": "Frustrated", "2": "Very angry" },
            "probabilities": { "0": 0.04, "1": 0.43, "2": 0.54 },
            "confidence": 0.75
        }
    },
    "usage": { "input_tokens": 129, "output_tokens": 160, "reasoning_tokens": 1536 },
    "latency_ms": 8141.6
}
```

### Python

`sdk/` is a drop-in replacement for Jev's Python SDK (`typesafe-sdk`):

```shell
pip install ./sdk
```

```python
from jeeves_sdk import Choice, Noul, Score, TypeSafeClient

with TypeSafeClient() as client:
    result = client.system_one(
        state="I was charged twice. Please help.",
        questions={
            "billing": Noul(instructions="Is this about billing?"),
            "tone": Choice(instructions="What is the tone?", criteria={"calm": None, "angry": None}),
            "urgency": Score(instructions="How urgent is this?", criteria=["can wait", "this week", "today"]),
        },
        max_think=768,
        return_reasoning=True,
    )
    print(result.nouls["billing"].noul, result.choices["tone"].choice, result.scores["urgency"].score)
    print(result.reasoning["tone"].text)
```

The client connects to `http://127.0.0.1:8009` by default (or `JEEVES_BASE_URL`), needs no API key, and waits up to 120s.

### Options

`options` is optional and ignored by Jev clients that don't send it. Server-wide defaults are set with the matching `serve` flags.

| option              | default | effect                                                                       |
| ------------------- | ------- | ---------------------------------------------------------------------------- |
| `think`             | `true`  | `false` answers from the prompt alone (about 0.3 s)                          |
| `max_think`         | 2560    | truncates each reasoning chain at this many tokens, then answers             |
| `nothink_threshold` | `null`  | answers without thinking when the no-think confidence is at least this value |
| `return_reasoning`  | `false` | adds each question's reasoning text to the response                          |

On 325 dev questions:

| setting                                  | accuracy | mean reasoning tokens | median / p90 latency |
| ---------------------------------------- | -------- | --------------------- | -------------------- |
| full thinking                            | 0.825    | 1,138                 | 3.3 s / 17.1 s       |
| `max_think` 768, `nothink_threshold` 0.9 | 0.806    | 344                   | 2.0 s / 5.6 s        |
| no thinking                              | 0.775    | 0                     | about 0.3 s          |

## How it works

Questions, states and answers are loaded into the Qwen chat template like

```
<state> …state…
<q> instructions <opt> option 1 </opt> <opt> option 2 </opt> …
<think>
```

The model then rolls out its reasoning chain, and after the `</think>` token we append

```
</think>

<q> instructions <opt> option 1 </opt> <opt> option 2 </opt> …
<decide>
```

A pointer head scores each option with a scaled dot product between a query projection of the hidden state at `<decide>` and a key projection of the hidden state at that option's `</opt>`, where

```
<state>, <q>, <opt>, </opt>, <decide> = "<|fim_prefix|>", "<|fim_middle|>", "<|box_start|>", "<|box_end|>", "<|fim_suffix|>"
```

These are rare, largely unused tokens in the Qwen tokenizer. Ablations found that using plain text like "State" in the prompt instead worsened performance. Likewise, not repeating the questions after the reasoning block also decreases performance. The final probabilities are a softmax over the option scores, divided by a temperature fitted on the dev set.

### Training

1. **SFT** (2 epochs, 596 steps on 8 GPUs). LoRA r=16 on all projections of Qwen3.5-9B plus the pointer head, trained on 19,126 questions from 12 public datasets and synthetic policy data. Half the questions carry a reasoning chain sampled from the base model.
2. **CISPO** (a 624-step schedule stopped at step 402). 9,992 RL questions, 8 rollouts each at temperature 1, capped at 2,560 thinking tokens.
3. **Calibration**. A single temperature fitted on dev, stored with the checkpoint.

Stopping at step 402 keeps the best calibration and dev score. Past it, the head over-sharpens on the saturated RL pool.

### Diffusion drafter

A diffusion view of the frozen model (`drafter/`), inspired by [Orthrus](https://arxiv.org/abs/2605.12825).

Unlike Orthrus, which supports attention-only models, it supports Qwen3.5's Gated DeltaNet layers by letting mask tokens cross-attend to those layers' post-convolution keys and values.

|                                             | chain tokens per second |
| ------------------------------------------- | ----------------------- |
| plain graphed greedy decoding, one question | 109                     |
| block 4, one question                       | 176 (1.6×)              |
| block 8, one question                       | 193 (1.76×)             |
| block 4, eight questions batched            | about 960 in total      |

Block 4 is the default because it stays cheap when several questions are batched.

## Reproduce

### Data

You can build the datasets locally using the prep scripts. This downloads the public datasets from Hugging Face at the revisions pinned in `prep/public.py`:

```shell
python -m prep.prep
```

Each public dataset stays under its own license.

### Training

On 8 GPUs, with the data in `data/`, `bash run.sh` runs the whole pipeline:

```shell
torchrun --nproc_per_node 8 train.py sft --run-dir runs/sft
torchrun --nproc_per_node 8 train.py cispo --run-dir runs/cispo --init runs/sft/final
torchrun --nproc_per_node 8 test.py runs/cispo/final
torchrun --nproc_per_node 8 jevbench.py runs/cispo/final
python export.py runs/cispo/final --out runs/fused
torchrun --nproc_per_node 8 -m drafter.gen --model runs/fused
torchrun --nproc_per_node 8 train.py drafter --model runs/fused --block 4 --run-dir runs/drafter_k4
```

## Repository

| path                                     | contents                                                                            |
| ---------------------------------------- | ----------------------------------------------------------------------------------- |
| `model/`                                 | Qwen3.5 (Gated DeltaNet + gated attention), LoRA, pointer head                      |
| `loader/`                                | prompt format, tokenisation and batching                                            |
| `prep/`                                  | dataset construction (`prep.py`) and synthetic generators                           |
| `trainer.py`, `train.py`                 | SFT, CISPO and drafter training                                                     |
| `test.py`, `jevbench.py`, `calibrate.py` | evaluation, JevBench, temperature fitting                                           |
| `export.py`                              | fuses LoRA into a standalone model with the head and temperature                    |
| `drafter/`                               | drafter model, chain sampling, fused speculative decoder                            |
| `inference/`                             | FP8 kernel, batched speculative engine, Jev-compatible server and benchmark         |
| `sdk/`                                   | `jeeves_sdk`, a drop-in replacement for Jev's Python SDK with the reasoning options |

## Limitations

- Knowledge questions trail Jev (MMLU 0.793 vs 0.900, MMLU-Pro 0.739 vs 0.840).
- Thinking is slow at the tail: 17 s at p90 with full chains. Use `max_think` and `nothink_threshold` when latency matters.
- The Kev and Jev comparisons outside JevBench use different items from the same sources.
- No language consistency reward was included so thinking chains are not well interpretable.

## Quote this

If you use Jeeves, its training recipe or its drafter, please cite:

```text-bibtex
@software{waltz2026jeeves,
  author = {Waltz, Nicholas P.},
  title  = {Jeeves: Reasoning Improves Jev-like Decisions},
  year   = {2026},
  url    = {https://github.com/PostHog/jeeves},
  note   = {Qwen3.5-9B decision model trained with SFT and CISPO, with a block-4 diffusion drafter}
}
```

## References

- [Jev's Architecture Unmasked](https://archerhume.com/posts/jevs-architecture-unmasked), the Jev design that Kev and Jeeves follow.
- Qwen Team. [Qwen3.5-9B](https://huggingface.co/Qwen/Qwen3.5-9B), the base model.
- Yang, Kautz, Hatamizadeh. [Gated Delta Networks: Improving Mamba2 with Delta Rule](https://arxiv.org/abs/2412.06464). ICLR 2025.
- Hu et al. [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685). ICLR 2022.
- MiniMax. [MiniMax-M1: Scaling Test-Time Compute Efficiently with Lightning Attention](https://arxiv.org/abs/2506.13585). 2025. Introduces CISPO.
- Guo, Pleiss, Sun, Weinberger. [On Calibration of Modern Neural Networks](https://arxiv.org/abs/1706.04599). ICML 2017. Temperature scaling.
- [Orthrus](https://arxiv.org/abs/2605.12825), arXiv 2605.12825. The diffusion drafter ours adapts to Gated DeltaNet.
- Leviathan, Kalman, Matias. [Fast Inference from Transformers via Speculative Decoding](https://arxiv.org/abs/2211.17192). ICML 2023.
