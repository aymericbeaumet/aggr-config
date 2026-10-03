---
title: GPT-5.4, The New Oracle
link: https://ampcode.com/news/gpt-5.4-the-new-oracle
source: ampcode-com
published: 2026-03-05T00:00:00Z
updated: 2026-03-05T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: Habemus oraculum! We have a new oracle in Amp and it's GPT-5.4. It's a great model. In our internal evals response quality went from 60.8% (GPT-5.2) to 68.2% (GPT-5.4). Mean latency is down from ~6.7min to ~4.9min. In Amp's smart mode GPT-5.4 works really well with Opus 4.6, which is smart mode's current main model. They complement each other with the oracle bringing sage advice on architecture, code reviews, and tricky bugs to the context window, just as we're used to from previous incantations. On top of that, we also decided to add the oracle subagent to deep mode. Now you might wonder, since deep mode currently uses GPT-5.3-Codex as the main model, why add another GPT model in the same mode? Does that even make sense? We think it does. GPT-5.3-Codex is fantastic at coding (as Codex models tend to be), which is exactly why it is the main model in deep, but the oracle is plain GPT-5.4, a non-Codex model. Less a code specialist, more an all-rounder. That gives us two models from the same family, but trained for different goals, with different system prompts, in the same mode — two distinct voices in the same conversation. We're still learning what GPT-5.4 can do in practice. There are very likely hidden smarts and treasures we haven't found yet. Let us know once you do.
content: extracted
html: 2026-03-05-gpt-5-4-the-new-oracle.html
preview:
  file: 2026-03-05-gpt-5-4-the-new-oracle.preview-fbb37bf08b5f.webp
  width: 256
  height: 134
  color: '#5f5a4f'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=GPT-5.4%2C+The+New+Oracle&date=March+5%2C+2026&tagline=GPT-5.4+is+now+Amp%27s+oracle%2C+and+deep+mode+now+has+one+too.&backgroundImage=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fgpt-5.4-oracle-4.jpg&sig=1257176c10cc8b5137992e156e11321ebc85605770c1891df025b734965ba5fd
  original:
    file: 2026-03-05-gpt-5-4-the-new-oracle.image-66a25c7141ba.png
    width: 1200
    height: 630
  variants:
  - file: 2026-03-05-gpt-5-4-the-new-oracle.image-edd425cebd0a.webp
    width: 320
    height: 168
  - file: 2026-03-05-gpt-5-4-the-new-oracle.image-492a2de2de60.webp
    width: 640
    height: 336
  - file: 2026-03-05-gpt-5-4-the-new-oracle.image-2ac3e834ca1e.webp
    width: 960
    height: 504
  - file: 2026-03-05-gpt-5-4-the-new-oracle.image-8d64f0e1e8c4.webp
    width: 1200
    height: 630
  color: '#292925'
---

Habemus oraculum! We have a new [oracle](https://ampcode.com/manual#oracle) in Amp and it's GPT-5.4.

It's a great model. In our internal evals response quality went from **60.8% (GPT-5.2)** to **68.2% (GPT-5.4)**. Mean latency is down from **\~6.7min** to **\~4.9min**.

In Amp's `smart` mode GPT-5.4 works really well with Opus 4.6, which is `smart` mode's current main model. They complement each other with the oracle bringing sage advice on architecture, code reviews, and tricky bugs to the context window, just as we're used to from previous incantations.

On top of that, we also decided to add the oracle subagent to `deep` mode. Now you might wonder, since `deep` mode currently uses GPT-5.3-Codex as the main model, why add another GPT model in the same mode? Does that even make sense?

We think it does. GPT-5.3-Codex is fantastic at coding (as Codex models tend to be), which is exactly why it is the main model in `deep`, but the oracle is plain GPT-5.4, a non-Codex model. Less a code specialist, more an all-rounder.

That gives us two models from the same family, but trained for different goals, with different system prompts, in the same mode — two distinct voices in the same conversation.

We're still learning what GPT-5.4 can do in practice. There are very likely hidden smarts and treasures we haven't found yet. Let us know once you do.
