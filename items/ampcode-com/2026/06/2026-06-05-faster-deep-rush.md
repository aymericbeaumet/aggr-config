---
title: Faster Deep & Rush
link: https://ampcode.com/news/faster-deep-rush
source: ampcode-com
published: 2026-06-05T00:00:00Z
updated: 2026-06-05T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'The first token now arrives 87% faster and entire responses are 32% faster, p50, in Amp''s deep and rush modes. How? Mostly using websockets for communication with OpenAI, partly because we rebuilt Amp to be much faster last month. These gains matter most on long-horizon tasks, where we''re seeing up to a 40% end-to-end speedup from user prompt submission to completion. You can see the difference:'
content: extracted
html: 2026-06-05-faster-deep-rush.html
preview:
  file: 2026-06-05-faster-deep-rush.preview-06413e3ea26f.webp
  width: 256
  height: 134
  color: '#5d564c'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Faster+Deep+%26+Rush&date=June+5%2C+2026&tagline=Deep+and+rush+now+start+87%25+faster+and+finish+32%25+faster&sig=0499086e3be38c8885baeab014d4013d7c3eb074c7bc5fcbfda1c61e045af020
  original:
    file: 2026-06-05-faster-deep-rush.image-8057bc54e931.png
    width: 1200
    height: 630
  variants:
  - file: 2026-06-05-faster-deep-rush.image-8578ac41ac91.webp
    width: 320
    height: 168
  - file: 2026-06-05-faster-deep-rush.image-319d289992e6.webp
    width: 640
    height: 336
  - file: 2026-06-05-faster-deep-rush.image-1eeacafc73f0.webp
    width: 960
    height: 504
  - file: 2026-06-05-faster-deep-rush.image-6500a3566113.webp
    width: 1200
    height: 630
  color: '#221b16'
- source: https://static.ampcode.com/news/openai-ws-elapsed-by-turn.svg?v=2
  original:
    file: 2026-06-05-faster-deep-rush.image-73d9a566cb55.png
    width: 940
    height: 500
  variants:
  - file: 2026-06-05-faster-deep-rush.image-5d252ee20e01.webp
    width: 320
    height: 170
  - file: 2026-06-05-faster-deep-rush.image-4f010c4fc6f7.webp
    width: 640
    height: 340
  - file: 2026-06-05-faster-deep-rush.image-16b895e66079.webp
    width: 940
    height: 500
  color: '#161615'
---

The first token now arrives 87% faster and entire responses are 32% faster, p50, in Amp's `deep` and `rush` modes.

How? Mostly using websockets for communication with OpenAI, partly because we [rebuilt Amp](https://ampcode.com/news/neo) to be much faster last month.

These gains matter most on long-horizon tasks, where we're seeing up to a 40% end-to-end speedup from user prompt submission to completion.

![Chart showing OpenAI WebSocket responses finishing faster than HTTP SSE over a long Amp session](https://static.ampcode.com/news/openai-ws-elapsed-by-turn.svg?v=2)

You can see the difference:
