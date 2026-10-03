---
title: Who Cares About the Model?
link: https://ampcode.com/news/who-cares-about-the-model
source: ampcode-com
published: 2026-07-29T00:00:00Z
updated: 2026-07-29T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Two weeks ago we shipped the Dial and quietly did something that''s supposed to be traumatic: we changed the default model. Before the Dial, Amp''s default mode was smart, running Claude Opus 4.8 and carrying more than half of all new threads. The Dial made medium the default, and medium runs GPT-5.6 Sol. Most Amp users switched from Anthropic to OpenAI overnight. We braced for the outcry. Every model swap in every coding tool comes with one. We prepared migration docs, packaged the old modes as installable plugins, and waited. Nothing happened. Not a single complaint. Here''s what the switch looked like in production: The day before the Dial shipped, smart carried 55% of new threads. A week later: zero. Last week, the four Dial modes carried 93% of all new threads, and medium alone carried two-thirds. Of the users on the Dial, 69% never set it to anything but medium. And the Amp plugins we shipped to bring the old modes back — exact prompts, exact models, one command to get smart again? Almost nobody installed them. What This Tells Us The differences between frontier models are now small. Small enough that for one engineer on one task, switching models won''t visibly change the result. But they still matter to us, here at Amp: across every thread on every tier, small differences compound, so we keep benchmarking and swapping models. That''s the trade. The default is good because someone is paid to care about it, and it doesn''t have to be you. What does change your output are three things, none of which is a model: how hard the task is, what context you put in, and how closely you review what comes out. All three have more impact on the outcome than whether you use this or that latest frontier model. The Dial covers the first one: how hard the task is. You tell it how hard the task is and the corresponding setting on the Dial uses whichever model wins it right now, re-tested constantly. You know your tasks, we know the models. And for the other two — the context and the review — the rest of Amp lets you do the best job with that.'
content: extracted
html: 2026-07-29-who-cares-about-the-model.html
preview:
  file: 2026-07-29-who-cares-about-the-model.preview-aa799f191437.webp
  width: 256
  height: 134
  color: '#5c554b'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Who+Cares+About+the+Model%3F&date=July+29%2C+2026&tagline=We+swapped+the+default+model+overnight.+Nobody+complained.&sig=554c9ab6497c13e3c4bc7fa3dc58df3875b9bf4ca7a37eb26782f29010127d46
  original:
    file: 2026-07-29-who-cares-about-the-model.image-bee3ca247240.png
    width: 1200
    height: 630
  variants:
  - file: 2026-07-29-who-cares-about-the-model.image-68d762bd8cf9.webp
    width: 320
    height: 168
  - file: 2026-07-29-who-cares-about-the-model.image-9083a3ab6b6c.webp
    width: 640
    height: 336
  - file: 2026-07-29-who-cares-about-the-model.image-b1972a918740.webp
    width: 960
    height: 504
  - file: 2026-07-29-who-cares-about-the-model.image-72c03cd26fb6.webp
    width: 1200
    height: 630
  color: '#221b16'
- source: https://static.ampcode.com/news/who-cares-about-the-model-mode-share.svg
  original:
    file: 2026-07-29-who-cares-about-the-model.image-504f529700f5.png
    width: 940
    height: 500
  variants:
  - file: 2026-07-29-who-cares-about-the-model.image-a9fa0c747da2.webp
    width: 320
    height: 170
  - file: 2026-07-29-who-cares-about-the-model.image-d0387694f1a7.webp
    width: 640
    height: 340
  - file: 2026-07-29-who-cares-about-the-model.image-49c4776e8051.webp
    width: 940
    height: 500
  color: '#161615'
---

Two weeks ago [we shipped the Dial](https://ampcode.com/news/the-dial) and quietly did something that's supposed to be traumatic: we changed the default model.

Before the Dial, Amp's default mode was `smart`, running Claude Opus 4.8 and carrying more than half of all new threads. The Dial made `medium` the default, and `medium` runs GPT-5.6 Sol.

Most Amp users switched from Anthropic to OpenAI overnight.

We braced for the outcry. Every model swap in every coding tool comes with one. We prepared migration docs, packaged the old modes as [installable plugins](https://ampcode.com/modes), and waited.

Nothing happened. Not a single complaint.

Here's what the switch looked like in production:

![Stacked area chart of new threads by agent mode, June 29 to July 26. Smart (Opus 4.8) holds around 55% until July 9, when the Dial ships, then collapses to near zero within days as medium (GPT-5.6) takes over.](https://static.ampcode.com/news/who-cares-about-the-model-mode-share.svg)

The day before the Dial shipped, `smart` carried 55% of new threads. A week later: zero. Last week, the four Dial modes carried 93% of all new threads, and `medium` alone carried two-thirds. Of the users on the Dial, 69% never set it to anything but `medium`.

And the Amp plugins we shipped to bring the old modes back — exact prompts, exact models, one command to get `smart` again? Almost nobody installed them.

## What This Tells Us

The differences between frontier models are now small. Small enough that for one engineer on one task, switching models won't visibly change the result.

But they still matter to us, here at Amp: across every thread on every tier, small differences compound, so we keep benchmarking and swapping models. That's the trade. The default is good because someone is paid to care about it, and it doesn't have to be you.

What does change your output are three things, none of which is a model: how hard the task is, what context you put in, and how closely you review what comes out. All three have more impact on the outcome than whether you use this or that latest frontier model.

The Dial covers the first one: how hard the task is. You tell it how hard the task is and the corresponding setting on the Dial uses whichever model wins it right now, re-tested constantly. You know your tasks, we know the models.

And for the other two — the context and the review — the rest of Amp lets you do the best job with that.
