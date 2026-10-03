---
title: From the creator of Redis; run LLM locally with ds4
link: https://dwarfstar.sh/
source: hnrss-org
published: 2026-10-02T18:01:16Z
updated: 2026-10-02T18:01:16Z
first_seen: 2026-10-03T00:25:47.283928759Z
authors:
- fibo
content: extracted
html: 2026-10-02-from-the-creator-of-redis-run-llm-locally-with-ds4.html
preview:
  file: 2026-10-02-from-the-creator-of-redis-run-llm-locally-with-ds4.preview-caaffb0b052f.webp
  width: 256
  height: 134
  alt: DwarfStar 4 logo and local inference project card
  color: '#171f30'
images:
- source: https://dwarfstar.sh/og-preview-v2.png
  original:
    file: 2026-10-02-from-the-creator-of-redis-run-llm-locally-with-ds4.image-5e4ce5041485.png
    width: 1200
    height: 630
  variants:
  - file: 2026-10-02-from-the-creator-of-redis-run-llm-locally-with-ds4.image-be5161fb557e.webp
    width: 320
    height: 168
  - file: 2026-10-02-from-the-creator-of-redis-run-llm-locally-with-ds4.image-8467420b7cc3.webp
    width: 640
    height: 336
  - file: 2026-10-02-from-the-creator-of-redis-run-llm-locally-with-ds4.image-8520df165435.webp
    width: 1200
    height: 630
  color: '#0a1121'
---

## Run frontier open weights locally with ds4.

DwarfStar 4 is a narrow C inference engine for high-memory Mac, CUDA and ROCm machines. It supports DeepSeek V4 and V4.1 Flash, GLM 5.x and Qwen3.8 Flash Next, with text and vision models, local APIs, a CLI and a native agent in one stack.

SUPPORTED: DEEPSEEK V4 / V4.1 + GLM 5.x + QWEN3.8 · MIT LICENSE · C / METAL / CUDA / ROCM · [QWEN ON 64GB](https://dwarfstar.sh/docs/qwen-3-8-flash-next/)

ds4 · local session

```
$ ./ds4
DwarfStar 4 · DeepSeek V4 Flash · ds4f-q2
local backend ready · model loaded
> /read src/kvcache.c
1,412 lines loaded into context
> why can a prefix survive a server restart?
The KV cache is keyed by the SHA1 of the rendered
prompt prefix and persisted to disk, so a matching
prefix is reloaded instead of recomputed.
```

PRINCIPLE · LOCAL MODEL STACK

PHASE 1 · THE GIANT

### A 284-billion-parameter star

DeepSeek V4 Flash is a large mixture-of-experts model. The usual path is remote serving; ds4 starts from the opposite constraint.

PHASE 2 · THE COLLAPSE

### Compressed, not lobotomized

Asymmetric quantization targets the routed experts while preserving critical paths. The model becomes practical on high-memory machines.

PHASE 3 · THE DWARF STAR

### Dense, resident, yours

The local engine exposes a CLI, HTTP APIs and a native agent, all sharing the same model state and cache.

[How the collapse works →](https://dwarfstar.sh/docs/architecture/)

SCROLL ▾

DESIGN CHOICES

## Local frontier inference, narrow on purpose

Not a generic GGUF runner. ds4 follows a small, opportunistic set of model families and validates each supported layout end to end.

CORE 01

### Asymmetric 2-bit quantization

Compress the routed experts, keep critical shared paths precise. That is how the supported routed-MoE builds fit their target machines.

CORE 02

### KV cache as a disk citizen

Save long prefixes to SSD and resume by prompt hash. Restarts do not have to mean full re-prefill.

CORE 03

### One engine, three interfaces

Use `./ds4` for chat, `./ds4-server` for local APIs and `./ds4-agent` for persistent coding sessions.

- SSD STREAMING
- TENSOR PARALLELISM
- SESSION BATCHING
- DSPARK + MTP
- VISION INPUT

ARCHITECTURE

## How the ds4 stack fits together

Project GGUFs, a self-contained engine and agent-facing interfaces, checked against official model outputs.

RUNTIME MAP · SIMPLIFIED. SEE [ARCHITECTURE NOTES](https://dwarfstar.sh/docs/architecture/) FOR THE FULL DRAWING.

RUN IT

## Run ds4 in three steps

Download the project GGUF, build for your backend, then start the CLI or server. Generic GGUF files are not the target.

STEP 1 · FETCH THE WEIGHTS

ds4 · zsh

$ git clone https://github.com/antirez/ds4\
 $ cd ds4 && ./download\_model.sh ds4f-q2

STEP 2 · BUILD FOR YOUR BACKEND

ds4 · zsh

$ make \
 $ make cuda-spark

STEP 3 · TALK TO IT

ds4 · zsh

$ ./ds4\
 $ ./ds4-server --ctx 100000

FIT CHECK

## ds4 hardware fit: local, streamed and distributed

Pick your platform and memory: get a conservative starting path and understand which execution modes apply.

PLATFORM MEMORY

✓ Runs well

V4 Flash Q2 is the baseline. At 128 GB, GLM 5.3 Q2 and Qwen Q4 also fit; V4.1 Q2 streams from SSD.

```
./download_model.sh ds4f-q2 && make
```

REF · M5 MAX 128GB · 32K CTX: 34.4 T/S GEN · 557 T/S PREFILL

Estimates from the [ds4 benchmark table](https://dwarfstar.sh/benchmarks/). Full guide in [Hardware](https://dwarfstar.sh/hardware/) and [Installation](https://dwarfstar.sh/docs/installation/).

BENCHMARKS

## ds4 benchmarks: prefill and generation

Reference rows from upstream. Read prefill and generation separately, especially for long-context agent workloads.

| Machine           | Context         | Prefill t/s | Generation t/s |
| ----------------- | --------------- | ----------- | -------------- |
| M5 Max, 128 GB    | q2 · 2,048 tok  | 790.2       | 39.4           |
| M5 Max, 128 GB    | q2 · 65,536 tok | 398.5       | 27.6           |
| DGX Spark, 128 GB | q2 · 2,048 tok  | 825.8       | 18.1           |
| DGX Spark, 128 GB | q2 · 65,536 tok | 823.0       | 13.8           |

[All benchmarks →](https://dwarfstar.sh/benchmarks/)

API & AGENTS

## Use ds4 from Codex, Claude Code and OpenCode

ds4-server speaks OpenAI and Anthropic-style APIs, so local coding agents can connect to your own machine with a base URL.

## Own your local AI inference.

Start with the quickstart, check the hardware matrix, then connect your editor, agent or API client to the local server.
