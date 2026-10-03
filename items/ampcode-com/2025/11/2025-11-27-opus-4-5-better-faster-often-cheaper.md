---
title: 'Opus 4.5: Better, Faster, Often Cheaper'
link: https://ampcode.com/news/opus-4.5
source: ampcode-com
published: 2025-11-27T00:00:00Z
updated: 2025-11-27T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Claude Opus 4.5 is the new main model in Amp''s smart mode, two days after we shipped it for you to try out. Only a week ago, we changed Amp''s main model to Gemini 3 — a historic change, we said. It was the first time since Amp''s creation that we switched away from Claude. Now we''re switching again and you may ask: why? Why follow a historic change with another one, in a historically short amount of time? We love Gemini 3, but, once rolled out, its impressive highs came with lows. What we internally experienced as rough edges turned into some very frustrating behaviors for our users. Frustrating and costly. Then, not even a week later, Opus 4.5 comes out. Opus 4.5, on the other hand, seems as capable as Gemini 3. Its highs might not be as brilliant as Gemini 3''s, but it also seems to do away with the lows. It seems more polished. It''s faster, even. We''re also pleasantly surprised by Opus''s cost-efficiency. Yes, Opus tokens are more expensive, but it needs fewer tokens to do the job, makes fewer token-wasting mistakes, and needs less human intervention (which results in a higher cache hit rate, which means lower costs and latency). Sonnet 4.5 Gemini 3 Pro Opus 4.5 Internal Evals 37.1% 53.7% 57.3% Avg. Thread Cost $2.75 $2.04 $2.05 0-200k Tokens Only[^1] $1.48 $1.19 $2.05 Off-the-Rails Cost[^2] 8.4% 17.8% 2.4% Speed (p50, preliminary) 2.4 min 4.3 min 3.5 min In words: If you use long threads (200k+ tokens): Opus will be a lot cheaper. It’s currently limited to 200k tokens of context, which forces you to use small threads—our strong recommendation anyway, for both quality and cost. If you need long context, use large mode. If Sonnet or Gemini frequently struggles for you or has hit a capability ceiling: Opus will be far more capable and accurate, and often cheaper too (by avoiding wasted tokens). If you loved Gemini 3 Pro: Opus will be ~40% more expensive but faster and more tolerant of ambiguous prompts. (This describes most of the Amp team, and we still find Opus worth it.) If you were perfectly satisfied with Sonnet 4.5: Opus will be ~35% more expensive for the same task. The real win comes from getting outside your comfort zone and giving it harder tasks where Sonnet would struggle. Staying on the frontier means sometimes shipping despite issues — and sometimes shipping something better a week later.'
content: extracted
html: 2025-11-27-opus-4-5-better-faster-often-cheaper.html
preview:
  file: 2025-11-27-opus-4-5-better-faster-often-cheaper.preview-9b96878f5ec9.webp
  width: 256
  height: 134
  color: '#5d564a'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Opus+4.5%3A+Better%2C+Faster%2C+Often+Cheaper&date=November+27%2C+2025&tagline=Claude+Opus+4.5+is+the+new+main+agent+model+in+Amp&backgroundImage=https%3A%2F%2Fampcode.com%2F%28marketing%29%2Fnews%2Fopus-4.5-bg.jpg&sig=30ae5f3b662f31588ab122031a15ad2213e21ba10e0a46c84007c3a933b80289
  original:
    file: 2025-11-27-opus-4-5-better-faster-often-cheaper.image-3044d3835d88.png
    width: 1200
    height: 630
  variants:
  - file: 2025-11-27-opus-4-5-better-faster-often-cheaper.image-904a6770f5ee.webp
    width: 320
    height: 168
  - file: 2025-11-27-opus-4-5-better-faster-often-cheaper.image-6b6b03f549ce.webp
    width: 640
    height: 336
  - file: 2025-11-27-opus-4-5-better-faster-often-cheaper.image-72162f8cba35.webp
    width: 960
    height: 504
  - file: 2025-11-27-opus-4-5-better-faster-often-cheaper.image-a95fc4bf125e.webp
    width: 1200
    height: 630
  color: '#27221d'
---

Claude Opus 4.5 is the new main model in Amp's `smart` mode, two days after we shipped [it](https://ampcode.com/news/try-opus) for you to try out.

Only a week ago, we changed Amp's main model to Gemini 3 — [a historic change](https://ampcode.com/news/gemini-3), we said. It was the first time since Amp's creation that we switched away from Claude. Now we're switching again and you may ask: why? Why follow a historic change with another one, in a historically short amount of time?

We love Gemini 3, but, once rolled out, its impressive highs came with lows. What we internally experienced as [rough](https://ampcode.com/news/gemini-3#not-perfect) [edges](https://x.com/sqs/status/1991563112290607422) turned into some very frustrating behaviors for our users. Frustrating and costly.

Then, not even a week later, Opus 4.5 comes out. Opus 4.5, on the other hand, seems as capable as Gemini 3. Its highs might not be as brilliant as Gemini 3's, but it also seems to do away with the lows. It seems more polished. It's faster, even.

We're also pleasantly surprised by Opus's cost-efficiency. Yes, Opus tokens are more expensive, but it needs fewer tokens to do the job, makes fewer token-wasting mistakes, and needs less human intervention (which results in a higher cache hit rate, which means lower costs and latency).

|                              | Sonnet 4.5 | Gemini 3 Pro | Opus 4.5 |
| ---------------------------- | ---------- | ------------ | -------- |
| **Internal Evals**           | 37.1%      | 53.7%        | 57.3%    |
| **Avg. Thread Cost**         | $2.75      | $2.04        | $2.05    |
|    0-200k Tokens Only        | $1.48      | $1.19        | $2.05    |
| **Off-the-Rails Cost**       | 8.4%       | 17.8%        | 2.4%     |
| **Speed (p50, preliminary)** | 2.4 min    | 4.3 min      | 3.5 min  |

In words:

- *If you use long threads (200k+ tokens):* Opus will be a lot cheaper. It’s currently limited to 200k tokens of context, which forces you to use [small threads](https://ampcode.com/guides/context-management)—our strong recommendation anyway, for both quality and cost. If you need long context, use [`large` mode](https://ampcode.com/modes).
- *If Sonnet or Gemini frequently struggles for you or has hit a capability ceiling:* Opus will be far more capable and accurate, and often cheaper too (by avoiding wasted tokens).
- *If you loved Gemini 3 Pro:* Opus will be ~40% more expensive but faster and more tolerant of ambiguous prompts. (This describes most of the Amp team, and we still find Opus worth it.)
- *If you were perfectly satisfied with Sonnet 4.5:* Opus will be ~35% more expensive for the same task. The real win comes from getting outside your comfort zone and giving it harder tasks where Sonnet would struggle.

Staying on the frontier means sometimes shipping despite [issues](https://x.com/sqs/status/1991563112290607422) — and sometimes shipping something better a week later.
