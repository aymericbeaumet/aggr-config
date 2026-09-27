---
title: ZML/LLMD alpha
link: https://zml.ai/llmd/
source: zml-ai
published: 0001-01-01T00:00:00Z
updated: 0001-01-01T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 0001-01-01-zml-llmd-alpha.html
---

## The universal LLM server.

A self-contained inference server for LLaMa, Gemma, Qwen and Mistral models, running across NVIDIA CUDA, AMD ROCm, Google TPU, Intel oneAPI and Apple Metal.

cuda zmlai/llmd

```
docker run -p 8000:8000 \
  --shm-size=256GB --gpus=all \
  -e HF_TOKEN -it zmlai/llmd:cuda \
  --model=hf://Qwen/Qwen3-8B
                
```

**5** accelerator targets

**10x** DFlash speedup on supported models

**1.7GB** CUDA image size

Serving

## Modern inference primitives.

### Continuous batching

Serve concurrent requests efficiently without hand-building a scheduler around the model.

### Paged attention

Run long-context workloads with the memory behavior expected from a production server.

### Tensor parallel sharding

Shard model execution across multiple devices while LLMD handles communication.

### Prefix caching

Reuse shared prompt prefixes across requests to reduce repeated work.

### Tool calling

Expose modern assistant workflows without leaving the LLMD serving path.

### Prometheus metrics

Monitor the server directly through the built-in `/metrics` endpoint.

### DFlash speculative decoding

Native DFlash support ships for Gemma 4 series models, with Qwen support planned.

Try it

## Run the server and point your harness at it.

### NVIDIA

```
docker run -p 8000:8000 --shm-size=256GB --gpus=all -e HF_TOKEN -it zmlai/llmd:cuda \
  --model=hf://Qwen/Qwen3.6-27B
```

### AMD

```
docker run -p 8000:8000 --device=/dev/kfd --device=/dev/dri -e HF_TOKEN -it zmlai/llmd:rocm \
  --model=hf://Qwen/Qwen3.6-27B
```

### Intel

```
docker run -p 8000:8000 --device=/dev/dri -e HF_TOKEN -it zmlai/llmd:oneapi \
  --model=hf://Qwen/Qwen3.6-27B
```

### Google TPU

```
docker run --net=host --privileged -e HF_TOKEN -it zmlai/llmd:tpu \
  --model=hf://Qwen/Qwen3.6-27B
```

### Apple Metal

```
brew install zml/zml/llmd
llmd --model=hf://Qwen/Qwen3.6-27B
```

Storage

## Load models where they already live.

LLMD uses ZML's VFS subsystem to load from Hugging Face, S3 and GCS without a separate download step. Use `hf://`, `s3://` or `gs://` anywhere a model path is expected.

Platforms

## One server, optimized platform-native artifacts.

| Platform | Image                       | Size   |
| -------- | --------------------------- | ------ |
| CUDA     | `zmlai/llmd:cuda`           | 1.7 GB |
| ROCm     | `zmlai/llmd:rocm`           | 3.9 GB |
| TPU      | `zmlai/llmd:tpu`            | 280 MB |
| OneAPI   | `zmlai/llmd:oneapi`         | 350 MB |
| Metal    | `brew install zml/zml/llmd` | 140 MB |
