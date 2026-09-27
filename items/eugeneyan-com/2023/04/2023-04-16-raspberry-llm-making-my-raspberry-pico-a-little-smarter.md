---
title: Raspberry-LLM - Making My Raspberry Pico a Little Smarter
link: https://eugeneyan.com//writing/raspberry-llm/
source: eugeneyan-com
published: 2023-04-16T00:00:00Z
updated: 2023-04-16T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- llm
- 🛠
summary: Generating Dr. Seuss headlines, fake WSJ quotes, HackerNews troll comments, and more.
content: extracted
html: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.html
preview:
  file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.preview-ff295d8a5788.webp
  width: 256
  height: 134
  color: '#71515e'
images:
- source: https://eugeneyan.com/assets/og_image/raspberry-llm.jpg
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-109a8095ef14.jpg
    width: 1200
    height: 630
  color: '#080807'
- source: https://eugeneyan.com/assets/headline-seuss.webp
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-52092b69feee.webp
    width: 1200
    height: 675
  color: '#b5b3ac'
- source: https://eugeneyan.com/assets/headline-quote.webp
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-eff7ed567834.webp
    width: 1200
    height: 900
  color: '#bab3ab'
- source: https://eugeneyan.com/assets/hackernews.webp
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-5202dd39cdf4.webp
    width: 1200
    height: 900
  color: '#b8b4ab'
- source: https://eugeneyan.com/assets/clock.webp
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-9344e5b34548.webp
    width: 1200
    height: 900
  color: '#aca9a2'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-04-16-raspberry-llm-making-my-raspberry-pico-a-little-smarter.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

As I continue my exploration with large language models, I wondered how they might be used in a low-resource setting, such as a household appliance or a Raspberry Pico. I was also curious about how good they were with generating content in a particular style (e.g., Dr. Seuss), humour (e.g., fake WSJ quotes), and toxic (e.g., HackerNews troll comments).

To satisfy my curiosity, I hacked on `raspberry-llm` ([Github](https://github.com/eugeneyan/raspberry-llm)). It’s a simple Raspberry Pico with an e-ink screen that calls [WSJ](https://feeds.a.dj.com/rss/RSSWSJD.xml) and [HackerNews](https://hnrss.org/frontpage?points=100) RSS feeds, 3rd-party LLM APIs, and generates some content. It started with a rhyming clock, and then… well you’ll see.

While some use LLMs to disrupt industries and more,

Others build ChatGPT plugins, pushing boundaries galore.

Yet here I am with my Raspberry Pi loose,

Using LLMs to explain headlines via Dr. Seuss.

![Image](https://eugeneyan.com/assets/headline-seuss.webp "Image")

You'll find that it can be quite witty,

Making up fake quotes from celebrity.

![Image](https://eugeneyan.com/assets/headline-quote.webp "Image")

It can also pose as a hackernews troll,

Slinging mean comments, and being quite an a$$hole.

![Image](https://eugeneyan.com/assets/hackernews.webp "Image")

It all started with getting it to tell the time,

With a little quirk, with a little rhyme.

![Image](https://eugeneyan.com/assets/clock.webp "Image")

Overall, it was a fun experience learning how to work with only 8kb of memory(!) and [Micro Python](https://github.com/micropython/micropython). Given the memory constraints, common libraries (e.g., json, xmltodict) were unavailable on the Pico. And even if they were, I couldn’t load the entire RSS feed into memory before parsing it via `xmltodict.parse()`.

As a result, I had to [parse RSS feeds character by character](https://github.com/eugeneyan/raspberry-llm/blob/main/rss.py#L26), learn how much [memory was consumed](https://github.com/eugeneyan/raspberry-llm/blob/main/check_mem.py) at each step, and do lots of `gc.collect()`. It was also fun learning to draw on an e-ink screen (the helper functions made it easier than expected).

To try it, clone this [GitHub repo](https://github.com/eugeneyan/raspberry-llm/tree/main) and update your wifi and OpenAI credentials in [secrets.py](https://github.com/eugeneyan/raspberry-llm/blob/main/secrets.py).

OG image prompt on MidJourney: “An e-ink display connected to a raspberry pi displaying some text –ar 2:1”

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Apr 2023). Raspberry-LLM - Making My Raspberry Pico a Little Smarter. eugeneyan.com. https://eugeneyan.com/writing/raspberry-llm/.

or

```
@article{yan2023raspberry,
  title   = {Raspberry-LLM - Making My Raspberry Pico a Little Smarter},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Apr},
  url     = {https://eugeneyan.com/writing/raspberry-llm/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
