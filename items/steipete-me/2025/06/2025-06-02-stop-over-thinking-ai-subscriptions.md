---
title: Stop Over-thinking AI Subscriptions
link: https://steipete.me/posts/2025/stop-overthinking-ai-subscriptions/
source: steipete-me
published: 2025-06-02T09:00:00Z
updated: 2025-06-02T09:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: After spending heavily on AI tools for two months, here's why the math actually works out—and which subscriptions are worth every penny.
content: extracted
html: 2025-06-02-stop-over-thinking-ai-subscriptions.html
preview:
  file: 2025-06-02-stop-over-thinking-ai-subscriptions.preview-f8f0436b1235.webp
  width: 256
  height: 134
  color: '#ece9e9'
images:
- source: https://steipete.me/posts/2025/stop-overthinking-ai-subscriptions/index.png
  original:
    file: 2025-06-02-stop-over-thinking-ai-subscriptions.image-a2b1299b6e31.png
    width: 1200
    height: 630
  variants:
  - file: 2025-06-02-stop-over-thinking-ai-subscriptions.image-0612b59d394d.webp
    width: 320
    height: 168
  - file: 2025-06-02-stop-over-thinking-ai-subscriptions.image-d179724cdba3.webp
    width: 640
    height: 336
  - file: 2025-06-02-stop-over-thinking-ai-subscriptions.image-5e373a87f4d9.webp
    width: 960
    height: 504
  - file: 2025-06-02-stop-over-thinking-ai-subscriptions.image-4866d186345e.webp
    width: 1200
    height: 630
  color: '#fdfafa'
- source: https://steipete.me/assets/img/2025/stop-overthinking-ai-subscriptions/hero.png
  original:
    file: 2025-06-02-stop-over-thinking-ai-subscriptions.image-21b4b5d3615c.png
    width: 1536
    height: 1024
  color: '#89cbbb'
---

![](https://steipete.me/assets/img/2025/stop-overthinking-ai-subscriptions/hero.png)

I’ve been working heavily with AI for the past few months, and many times when I post something on Twitter, the replies are predictable: “Oh my god, how much did you pay for that?”

The thing is, people don’t understand that you don’t actually have to pay *that* much to get incredible AI productivity. After using the best AI subscription deals 2025 has to offer, here’s the real math (all prices in USD). (And yes, I built [Vibe Meter](https://steipete.me/posts/2025/vibe-meter-monitor-your-ai-costs/) to track exactly how much I’m spending.)

| Plan           | Monthly Fee | Included Usage    | Marginal Cost      |
| -------------- | ----------- | ----------------- | ------------------ |
| Claude Pro     | $20         | \~45 messages/5h  | Rate limited       |
| Claude Max 5×  | $100        | \~225 messages/5h | Rate limited       |
| Claude Max 20× | $200        | \~900 messages/5h | Rate limited       |
| Cursor Pro     | $20         | 500 fast requests | $0.04/fast request |
| OpenAI o3      | Pay-per-use | N/A               | $10-40/1M tokens   |

## The Current Best Deal: Claude Max at $200/month

**[Claude Max](https://claude.ai/settings/billing) gives you almost unlimited use of [Claude Code](https://claude.ai/code)**, which is hands-down the best terminal-based AI tool available (next to Cursor). For $200/month, you get 20× the usage of Claude Pro.

Claude Max works on a rolling 5-hour block system with roughly **900 messages per 5-hour window**. The moment you send your first prompt, the timer starts. When 5 hours are up, everything refreshes with a new block and full quota. The Claude Max subscription uses Opus as the default model and switches to Sonnet when you reach 50% of the 5-hour limit (this is configurable). If you really burn through your whole allowance for that window, you can switch to API-based billing.

If you’re tight on money and you really need this extremely high use case, look at the [development partner program](https://support.anthropic.com/en/articles/11174108-about-the-development-partner-program) where they offer 30% off standard API pricing (this discount applies only to API usage, not the Max sub).

## But What About Future Price Increases?

I get messages all the time from people worried that token prices will go up. Here’s the reality check:

**Token prices have dropped 1000× in the last two years.** We’ve gone from GPT-3’s astronomical pricing to incredibly capable open models. The competitive landscape has completely transformed—companies are building their own models now. Windsurf has SWE-1, Vercel launched v0-1.5lg, and every major player is racing to offer the best value.

What indicators suggest prices will increase? None. If anything, competition is driving costs down to the fundamental floor: [energy costs](https://blog.samaltman.com/the-gentle-singularity). Models are becoming commodities. The real differentiator is the tooling around them, not the raw compute.

So stop worrying about hypothetical price hikes. The trend line points one direction: down.

## Cursor: The Math That Makes Sense

**[Cursor](https://cursor.com/) Pro costs $20/month** and gives you 500 “fast” requests plus unlimited “slow” ones. Beyond that, it’s **$0.04 per fast request** (per Cursor docs).

Cursor offers two pricing modes: **Normal mode** charges a fixed number of requests per message regardless of context size, while **Max mode** uses token-based pricing (API cost + 20% margin) for complex reasoning tasks. Most everyday coding works perfectly fine in Normal mode where each message to Gemini 2.5 Pro (my favorite model) costs exactly 1 request.

[That workshop I did](https://steipete.me/posts/2025/the-future-of-vibe-coding/)? Probably 200+ AI requests across 3 hours. Let’s do the math: 200 requests × $0.04 = **$8**.

For an afternoon of intensive coding with AI assistance, you’re looking at pocket change. Even if you burn through 1,000 requests in a month, that’s $40 on top of your $20 subscription. When you run out of fast requests, Cursor automatically switches to free requests (processed at lower priority) so you never truly hit a wall.

## What Actually Gets Expensive: OpenAI’s o3

Here’s where I’ll be honest: **about half my monthly AI bill goes to o3**.

OpenAI’s o3 model costs **$10 per 1M input tokens and $40 per 1M output tokens**. Cached input drops to $2.50/1M. When you’re debugging complex code and you want the absolute best reasoning, o3 delivers, but it adds up fast.

o3 is my dark knight—silent, pricey, but rescues impossible bugs.

I spent around $400 on o3 last month because I was banging my head against a wall with some really tricky code. When I hit [truly difficult problems](https://github.com/steipete/AXorcist), throwing o3 at them for a few hours beats getting stuck in debugging rabbit holes for days.

## Smart Alternative: Repo Prompt for o3 Access

If you’re tight on money but still want o3’s reasoning power, I highly recommend [Repo Prompt](https://repoprompt.com/) (macOS-only). It lets you use o3 through your ChatGPT subscription instead of paying API rates.

While it’s more manual than integrated tools, it’s incredibly effective at taking code changes and applying them to your project. The workflow is straightforward: export your repo context, get o3’s analysis and suggestions, then apply the changes back to your codebase.

It’s an amazing tool that bridges the gap between premium AI access and budget constraints. Definitely worth experimenting with.

## The Contractor Math

Let’s be conservative and say **$800/day** (though I’d assume many of you charge more). The AI subscription math is a no-brainer.

One afternoon saved per month = $200 in billable time. Claude Max pays for itself in 5 saved hours. Cursor pays for itself in 45 minutes.

The productivity multiplier is so absurd that arguing about the cost is like complaining about the price of coffee while billing $200/hour.

Now you know—don’t be a Mario:

> Hi, my name is Mario, and I spent about $1000 in Anthropic pay-as-you-go before switching to Max and finding out that I stay well within the rate limit...
>
> Don't be a Mario.
>
> — Mario Zechner (@badlogicgames) [November 30, 2024](https://twitter.com/badlogicgames/status/1929665561715204520?ref_src=twsrc%5Etfw)

## TL;DR

Time is the only non-refillable resource. Claude Max at $200 is currently the cheapest way I know to mint extra hours—and those hours compound faster than any SaaS line item ever will.

**What’s your monthly AI spend?** Ping me on Twitter [@steipete](https://twitter.com/steipete) with your breakdown and what you’re buying with the saved time.
