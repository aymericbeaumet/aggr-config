---
title: 'Launch HN: Magnitude (YC S25) – Self-optimizing inference engine for agents'
link: https://github.com/magnitudedev/magnitude
source: hnrss-org
published: 2026-09-30T17:37:40Z
updated: 2026-09-30T17:37:40Z
first_seen: 2026-10-01T06:33:51.242437607Z
authors:
- anerli
summary: 'Hey HN, Anders and Tom here. We''re building Magnitude, an inference engine for agents that optimizes itself to run as fast as possible on your hardware. It works on Mac, Linux, and Windows on any hardware and is up to 2x faster than llama.cpp. We''re both software engineers and previously built an open source browser agent to 4k+ GH stars and 100k+ downloads. We increasingly wanted to run it on local models, but found that no inference engine worked for our use case. Inference engines today all make a performance tradeoff. They are either: - Built for batched inference on datacenter hardware at the cost of single-session performance (vLLM, SGLang) - Designed for broad compatibility instead of optimizing for specific hardware (llama.cpp, Ollama) - Specialized for specific hardware or models but lacking engine completeness (oMLX, ds4) Plus none of them are designed for running agents locally. Sessions are long, several often run at once, and you still want to use your computer for other things. Magnitude is built for maximum performance on your hardware and running local agents: - On-device compilation and tuning: Kernels are written with flexible parameters that are tuned on your actual device before the model runs. This gives you broad hardware compatibility with the same performance ceiling as hardware-specific kernels. - Focus on best architectures: We write our tunable, highly efficient kernels for the most popular open-weights families. This allows us to achieve and surpass the performance of hardware or model specialized engines, without forcing ourselves to over-generalize at the cost of performance. - Dynamic memory allocation: Magnitude reserves only enough memory up front to hold model weights. As your agent sessions grow, the memory heap dynamically increases, and frees itself when agents stop. Your hardware can still be used for other stuff while agents run. - Hybrid paged attention: We borrow the best ideas from engines like SGLang to allow concurrent sessions to share prefix caches, but optimize placement for memory-adjacency so single-session performance doesn''t suffer. Magnitude is fully open source (Apache 2.0). We built it in Rust, including a custom GPU kernel runtime and autotuner. We take inspiration from the best innovations in inference from academics (e.g. FlashAttention, FlashInfer, TurboQuant) as well as other engines (e.g. SGLang radix attention) to reach the performance ceiling. Benchmarked against llama.cpp with Qwen 3.6 35B A3B (4 bit), 64k context, no speculative decoding: Metal (Mac M4 Pro 48 GB) - 92% faster decode (30 tok/s → 57 tok/s) - 9% faster prefill (466 tok/s → 507 tok/s) - 28% less per-agent memory usage CUDA (DGX Spark) - 19% faster decode (49 tok/s → 58 tok/s) - 23% faster prefill (2,033 tok/s → 2,507 tok/s) - 27% less per-agent memory usage Magnitude ships as a desktop app that you can easily connect with whatever agents you already use (Pi, OpenCode, Hermes, Codex, and more). It automatically runs models on demand when these agents actually need them, and shuts them down after inactivity. Here''s what it looks like: https://www.youtube.com/watch?v=0qE8BWEZu7o We''re excited to push Magnitude further to let you run bigger models on the same hardware while continuing to improve performance. Our plans include: - Expert streaming: store experts on RAM or disk and load them just-in-time. This lets you run models bigger than what otherwise would fit on your GPU. - Kernel compiler: our current kernels tune a few parameters to fit your hardware. We can take this further with a fully custom compiler that automatically chooses how to fuse kernels and which implementations to use, to make it fit to your hardware even better. - Multi-device utilization: Make the best possible use of all hardware on a system (CPU, GPUs, RAM, disk) by detecting these and automatically solving for the best model layout. We''d love for more people to try it out and give us feedback. Feel free to comment here, we''ll be around all day! Comments URL: https://news.ycombinator.com/item?id=49911995 Points: 144 # Comments: 71'
content: extracted
html: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.html
preview:
  file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.preview-acc734a79ed4.webp
  width: 256
  height: 128
  alt: Open source inference engine for agents that optimizes itself for your exact hardware. Compiles and tunes its kernels on your device, so open models run up to 2x faster than llama.cpp. Works on App...
  color: '#e7eaed'
images:
- source: https://opengraph.githubassets.com/7226103401330956a16419edd35f929244579f247e9c14944747b8d5fbf8ed32/magnitudedev/magnitude
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-afe5a0984b75.png
    width: 1200
    height: 600
  color: '#fdfdfd'
- source: https://github.com/magnitudedev/magnitude/raw/main/assets/brand/icon-light.svg
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-5632a5e10495.png
    width: 134
    height: 128
  variants:
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-ed3600631628.webp
    width: 134
    height: 128
  color: '#000000'
- source: https://camo.githubusercontent.com/bc9fac0ea9b676cff0219c5feb3b40fcb4fd20ce2b74ec3cea7b7baff609b000/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d446f776e6c6f61642d677261793f7374796c653d666c61742d737175617265266c6162656c436f6c6f723d303336396131266c6f676f3d64617461253341696d616765253246737667253242786d6c25334262617365363425324350484e325a79423462577875637a30696148523063446f764c336433647935334d793576636d63764d6a41774d43397a646d636949485a705a58644362336739496a41674d4341794e4341794e4349675a6d6c7362443069626d39755a534967633352796232746c5053496a5a6d5a6d5a6d5a6d4969427a64484a766132557464326c6b64476739496a49754d6a556949484e30636d39725a5331736157356c5932467750534a79623356755a434967633352796232746c4c577870626d567162326c7550534a79623356755a434925324250484268644767675a4430695454457949444e324d5449694c7a3438634746306143426b50534a744e7941784d434131494455674e53303149693825324250484268644767675a443069545451674d5464324d6d4579494449674d434177494441674d69417961444579595449674d694177494441674d4341794c544a324c5449694c7a34384c334e325a7a34253344
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-77f56acf055a.png
    width: 89
    height: 20
  variants:
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-54e449e01b58.webp
    width: 89
    height: 20
  color: '#555555'
- source: https://camo.githubusercontent.com/755fdc537ec9c427889e3dc9dffec8273c41c519983a770ecdcb062fc4f39de2/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539332539352d446f63732d3033363961313f7374796c653d666c61742d737175617265266c6162656c436f6c6f723d30333639613126636f6c6f723d67726179
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-202a5007f8b5.png
    width: 58
    height: 20
  variants:
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-b127d9bca51d.webp
    width: 58
    height: 20
  color: '#555555'
- source: https://camo.githubusercontent.com/cf68671d7bff8402a3e7712d95ddf977e8cc9b8ddd68f9f4eb7fe4fdc39f37f9/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d446973636f72642d677261793f7374796c653d666c61742d737175617265266c6f676f3d646973636f7264266c6f676f436f6c6f723d7768697465266c6162656c436f6c6f723d353836354632
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-f5f52575f1d5.png
    width: 75
    height: 20
  variants:
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-7c208b14421c.webp
    width: 75
    height: 20
  color: '#555555'
- source: https://camo.githubusercontent.com/5b55d22bf180ad37b77d6fad8a9a68f3f4ea889bcbe8b59f7869c6103b59cf00/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d547769747465722d677261793f7374796c653d666c61742d737175617265266c6f676f3d78266c6f676f436f6c6f723d7768697465266c6162656c436f6c6f723d303030303030
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-4fbdc78f4f4d.png
    width: 73
    height: 20
  variants:
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-b3809c4227e1.webp
    width: 73
    height: 20
  color: '#555555'
- source: https://camo.githubusercontent.com/f46074392f1d3f080ae7ffa7427dca2654348aa06e3691b16d81dbc87270edb9/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f73746172732f6d61676e69747564656465762f6d61676e6974756465
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-a37b2d1bf42c.png
    width: 90
    height: 20
  variants:
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-7bcfeceddc55.webp
    width: 90
    height: 20
  color: '#f8f8f8'
- source: https://github.com/magnitudedev/magnitude/raw/main/assets/benchmarks/llama-cpp-light.svg
  original:
    file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-f772d1f0ce84.png
    width: 800
    height: 240
  variants:
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-dce78cb57d78.webp
    width: 320
    height: 96
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-aa888b42307e.webp
    width: 640
    height: 192
  - file: 2026-09-30-launch-hn-magnitude-yc-s25-self-optimizing-inference-engine.image-f8dc0b7fd78d.webp
    width: 800
    height: 240
  color: '#fdfdfd'
---

![Magnitude icon](https://github.com/magnitudedev/magnitude/raw/main/assets/brand/icon-light.svg)

**Run open models as fast as your hardware allows**

[![Download Magnitude](https://camo.githubusercontent.com/bc9fac0ea9b676cff0219c5feb3b40fcb4fd20ce2b74ec3cea7b7baff609b000/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d446f776e6c6f61642d677261793f7374796c653d666c61742d737175617265266c6162656c436f6c6f723d303336396131266c6f676f3d64617461253341696d616765253246737667253242786d6c25334262617365363425324350484e325a79423462577875637a30696148523063446f764c336433647935334d793576636d63764d6a41774d43397a646d636949485a705a58644362336739496a41674d4341794e4341794e4349675a6d6c7362443069626d39755a534967633352796232746c5053496a5a6d5a6d5a6d5a6d4969427a64484a766132557464326c6b64476739496a49754d6a556949484e30636d39725a5331736157356c5932467750534a79623356755a434967633352796232746c4c577870626d567162326c7550534a79623356755a434925324250484268644767675a4430695454457949444e324d5449694c7a3438634746306143426b50534a744e7941784d434131494455674e53303149693825324250484268644767675a443069545451674d5464324d6d4579494449674d434177494441674d69417961444579595449674d694177494441674d4341794c544a324c5449694c7a34384c334e325a7a34253344)](https://magnitude.dev/download) [![Documentation](https://camo.githubusercontent.com/755fdc537ec9c427889e3dc9dffec8273c41c519983a770ecdcb062fc4f39de2/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2546302539462539332539352d446f63732d3033363961313f7374796c653d666c61742d737175617265266c6162656c436f6c6f723d30333639613126636f6c6f723d67726179)](https://docs.magnitude.dev) [![Discord](https://camo.githubusercontent.com/cf68671d7bff8402a3e7712d95ddf977e8cc9b8ddd68f9f4eb7fe4fdc39f37f9/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d446973636f72642d677261793f7374796c653d666c61742d737175617265266c6f676f3d646973636f7264266c6f676f436f6c6f723d7768697465266c6162656c436f6c6f723d353836354632)](https://discord.gg/EHt48pPWdC) [![Follow Magnitude on Twitter](https://camo.githubusercontent.com/5b55d22bf180ad37b77d6fad8a9a68f3f4ea889bcbe8b59f7869c6103b59cf00/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f2d547769747465722d677261793f7374796c653d666c61742d737175617265266c6f676f3d78266c6f676f436f6c6f723d7768697465266c6162656c436f6c6f723d303030303030)](https://x.com/usemagnitude) [![GitHub Repo stars](https://camo.githubusercontent.com/f46074392f1d3f080ae7ffa7427dca2654348aa06e3691b16d81dbc87270edb9/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f73746172732f6d61676e69747564656465762f6d61676e6974756465)](https://github.com/magnitudedev/magnitude/stargazers)

Magnitude is an open source inference engine for agents that optimizes itself for your exact hardware. It compiles and tunes its kernels on your device, so open models run up to 2x faster than llama.cpp. One click connects the agent you already use (Pi, OpenCode, Hermes, Codex, and more). Works on Apple Silicon, NVIDIA, AMD, or nothing but a CPU.

**[Download Magnitude for macOS, Windows, or Linux](https://magnitude.dev/download)**

⭐ Help us reach more developers and grow the Magnitude community. Star this repo!

demo-9-29.mp4

## Get started

1. [Download Magnitude](https://magnitude.dev/download), install it, and open the app.
2. Choose a recommended model in **Discover** and download it.
3. Connect your agent in **Connections** and start using it.

The desktop app includes the `magnitude` CLI. No separate installation is needed.

## Why Magnitude?

- **Up to 2x faster than llama.cpp:** 92% faster decode on Metal, 19% on CUDA
- **Tuned on your device:** kernels are tuned on your hardware before a model runs
- **Built for the best models:** hand-optimized kernels for popular open-weight families
- **Memory that flexes:** 27% less memory per agent, freed when agents stop
- **Fast concurrent sessions:** sessions share prefix caches to prevent slowdown
- **Works with your agent:** one click to connect Pi, OpenCode, Hermes, Codex, and more
- **Free, private, open source:** no token costs, nothing leaves your machine, Apache 2.0

## Up to 2x faster than llama.cpp

![Magnitude vs llama.cpp: 9% faster prefill and 92% faster decode on Metal, 23% faster prefill and 19% faster decode on CUDA](https://github.com/magnitudedev/magnitude/raw/main/assets/benchmarks/llama-cpp-light.svg)

## FAQ

### What is Magnitude?

An open source inference engine that optimizes itself for your hardware. It ships as a desktop app that runs open models and connects them to the agent you already use.

### How is it faster than llama.cpp, Ollama, or LM Studio?

They ship kernels precompiled for broad classes of hardware. Magnitude compiles and tunes its kernels on your actual device before a model runs, so they fit your exact chip. [See the benchmarks against llama.cpp.](https://github.com/magnitudedev/magnitude#up-to-2x-faster-than-llamacpp)

### What hardware do I need?

Any Apple Silicon, NVIDIA, or AMD GPU, or nothing but a CPU. There is no fixed minimum. Smaller machines run smaller models, and more memory lets you run larger ones.

### What operating systems does it support?

macOS, Linux, and Windows.

### Which models does it support?

See the full list at [magnitude.dev/models](https://magnitude.dev/models). We write optimized kernels for the most popular open-weight families, which is how we beat generalist engines.

### Which agents work with it?

One click connects Pi, OpenCode, Hermes, OpenClaw, Codex, Claude Code, Oh My Pi, and Cline. Anything else works through the OpenAI-compatible API.

### Is it private?

Yes. Prompts, files, and models stay on your machine. No internet needed once a model is downloaded.
