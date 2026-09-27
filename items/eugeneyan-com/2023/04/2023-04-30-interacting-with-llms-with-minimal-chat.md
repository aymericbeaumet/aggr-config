---
title: Interacting with LLMs with Minimal Chat
link: https://eugeneyan.com//writing/llm-ux/
source: eugeneyan-com
published: 2023-04-30T00:00:00Z
updated: 2023-04-30T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- llm
- misc
- 🛠
summary: Should chat be the main UX for LLMs? I don't think so and believe we can do better.
content: extracted
html: 2023-04-30-interacting-with-llms-with-minimal-chat.html
preview:
  file: 2023-04-30-interacting-with-llms-with-minimal-chat.preview-3675c75e4c6d.webp
  width: 256
  height: 134
  color: '#594738'
images:
- source: https://eugeneyan.com/assets/og_image/library-v2.jpg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-4c5ca13d2b78.jpg
    width: 1200
    height: 630
  color: '#16161a'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://i.ytimg.com/vi/6aLOcD7EJRQ/hqdefault.jpg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-da6ba9f1fa89.jpg
    width: 480
    height: 360
  color: '#000000'
- source: https://i.ytimg.com/vi/rd-J3hmycQs/hqdefault.jpg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-6a30044e9499.jpg
    width: 480
    height: 360
  color: '#000001'
- source: https://i.ytimg.com/vi/JdwpVKKrL2o/hqdefault.jpg
  original:
    file: 2023-04-30-interacting-with-llms-with-minimal-chat.image-df1cd9c4c42b.jpg
    width: 480
    height: 360
  color: '#000000'
---

I’ve been thinking about user experiences for LLMs. Currently, most demos interact with LLMs via chat. This is a good start, but I think we can do better. I’m not convinced most users want to interact via text. Also, I think we can provide LLMs with the context of the user, without the user having to type or voice it out.

> Will chat be the UI for most (LLM) apps?
>
> I'm not sure. If I'm shopping for electronics or clothes, I want to look at specs & images.
>
> Also, there's context (clicks, purchases) that shouldn't have to be chatted.
>
> Maybe agents should work on context first and chat input second.
>
> — Eugene Yan (@eugeneyan) [April 18, 2023](https://twitter.com/eugeneyan/status/1648340902010851329?ref_src=twsrc%5Etfw)

Most UIs today are based on clicks: When we surf the web, we’re mostly clicking on images, links, and buttons. And for some apps, the user’s context, such as location (e.g., Google Maps), persona (e.g., Netflix), and past behavior (e.g., Amazon), are taken into account.

Why not do the same for LLMs? Here’s how we can interact with LLMs with minimal chat. We start with clicking books of interest, then filtering them on vibes, before asking an LLM librarian for help. And even though the question entered is simple (e.g., “more books by female authors”), the LLM has our past context and can give us a personalized answer.

[YouTube video player](https://www.youtube.com/watch?v=6aLOcD7EJRQ)

Searching for my next book with an LLM's help; mostly clicking, minimal chatting.

The prototype is a blend of recommendation systems, NLP, and LLMs.

When an item is selected, similar items are retrieved via approximate nearest neighbors on item embeddings ([details](https://eugeneyan.com/writing/real-time-recommendations/#how-to-design-and-implement-an-mvp)). We can [learn item embeddings](https://eugeneyan.com/writing/recommender-systems-graph-and-nlp-pytorch/) by building a product graph from [e-commerce data](https://nijianmo.github.io/amazon/index.html), generating random walk sequences, and applying representation learning. If multiple items are viewed in a session, more recently viewed items are given heavier weight. The retrieved candidates are then ranked via an LTR model or heuristics.

The vibe keywords that help us filter items are pre-cached. (Someone had the impression they were dynamically extracted via an LLM. This is not feasible now given the latency, but perhaps in the future.) We either extract these keywords from book descriptions or smartly find such data from sources such as the [UCSD Book Graph](https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home).

Finally, chatting with the librarian is a call to an LLM, with basic storage of the user’s historical actions and chat. The app is served via FastAPI and [Jinja templates](https://eugeneyan.com/writing/how-to-set-up-html-app-with-fastapi-jinja-forms-templates/). To minimize the amount of time a user had to wait after chat input, I used the streaming API with Python [async](https://docs.python.org/3/library/asyncio.html) and [aiohttp](https://docs.aiohttp.org/en/stable/) in the backend, with a bit of JavaScript in the frontend.

What do you think? Would you prefer to chat more, or less? Do you know of other UXes for interacting with LLMs? Please share! 🙏

OG image prompt on MidJourney: “finding books in a magical digital library with a close up of books, in the style of contrasting tones, artifacts of online culture, innovative page design, complexity theory, bold black and whites, bold color scheme –ar 2:1”

## Also see

- [Generative Interfaces Beyond Chat](https://www.youtube.com/watch?v=rd-J3hmycQs)
- [AI UX: Beyond the Textbox](https://www.youtube.com/watch?v=JdwpVKKrL2o)
- [Why Chatbots Are Not the Future](https://wattenberger.com/thoughts/boo-chatbots)
- [What Types of Questions Require Conversation to Answer? A Case Study of AskReddit Questions](https://arxiv.org/abs/2303.17710)

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Apr 2023). Interacting with LLMs with Minimal Chat. eugeneyan.com. https://eugeneyan.com/writing/llm-ux/.

or

```
@article{yan2023ux,
  title   = {Interacting with LLMs with Minimal Chat},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Apr},
  url     = {https://eugeneyan.com/writing/llm-ux/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
