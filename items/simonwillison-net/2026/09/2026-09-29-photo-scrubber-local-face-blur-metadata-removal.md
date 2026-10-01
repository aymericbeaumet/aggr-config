---
title: Photo Scrubber — local face blur & metadata removal
link: https://simonwillison.net/2026/Sep/29/photo-scrubber/
source: simonwillison-net
published: 2026-09-29T16:45:27Z
updated: 2026-09-29T16:45:27Z
first_seen: 2026-10-01T00:32:59.748537119Z
labels:
- photography
- tools
summary: 'Tool: Photo Scrubber — local face blur & metadata removal I took a photograph of some protesters, then thought about how I don''t like sharing photographs of strangers with identifiable faces. I had GPT-6 Astra build this experimental tool that would identify faces and automatically blur them out. It uses Google''s MediaPipe C++ library, compiled to WebAssembly via @mediapipe/tasks-vision, plus the BlazeFace face detection model. Tags: photography, tools'
content: extracted
html: 2026-09-29-photo-scrubber-local-face-blur-metadata-removal.html
---

[Tool](https://simonwillison.net/elsewhere/tool/) [Photo Scrubber — local face blur & metadata removal](https://tools.simonwillison.net/photo-scrubber)

I took a photograph of some protesters, then thought about how I don't like sharing photographs of strangers with identifiable faces. I [had GPT-6 Astra build](https://github.com/simonw/tools/commit/aa733ecc3458d5703e3b40c84507ae362f5fb294) this experimental tool that would identify faces and automatically blur them out.

It uses Google's [MediaPipe](https://developers.google.com/edge/mediapipe/solutions/guide) C++ library, compiled to WebAssembly via [@mediapipe/tasks-vision](https://www.npmjs.com/package/@mediapipe/tasks-vision), plus the [BlazeFace](https://sites.google.com/view/perception-cv4arvr/blazeface) face detection model.

Posted [29th September 2026](https://simonwillison.net/2026/Sep/29/) at 4:45 pm
