---
title: Bluesky reply bot checker
link: https://simonwillison.net/2026/Sep/27/bluesky-bot-check/
source: simonwillison-net
published: 2026-09-27T18:41:44Z
updated: 2026-09-27T18:41:44Z
first_seen: 2026-09-28T06:49:16.850449770Z
labels:
- ai-misuse
- bluesky
- twitter
- vibe-coding
summary: 'Tool: Bluesky reply bot checker Automated reply bots on Twitter are a scourge - as someone with a decent number of followers I attract a swarm of these, such that anything I post there attracts dozens of mindless automated replies. They''ve started manifesting on Bluesky as well. Unlike Twitter, Bluesky still has a freely available and useful API. The lack of such a thing doesn''t slow down the bots, but it does make investigating them a lot more frustrating. So I had Opus 5.5 vibe code this tool, which examines any Bluesky profile for evidence of a likely reply bot. It looks for signals like replies posted within seconds of other posts from the same account, or accounts that never post their own content (or images or links) but instead consistently reply to messages from other, higher-follower users. It also looks for question marks, because I''m extra infuriated by reply bots that I no tie me to waste my time answering a question that no human ever posed. Tags: twitter, bluesky, vibe-coding, ai-misuse'
content: extracted
html: 2026-09-27-bluesky-reply-bot-checker.html
---

Automated reply bots on Twitter are a *scourge* - as someone with a decent number of followers I attract a swarm of these, such that anything I post there attracts dozens of mindless automated replies.

They've started manifesting on Bluesky as well.

Unlike Twitter, Bluesky still has a freely available and useful API. The lack of such a thing doesn't slow down the bots, but it does make investigating them a lot more frustrating.

So I had Opus 5.5 [vibe code this tool](https://github.com/simonw/tools/pull/348), which examines any Bluesky profile for evidence of a likely reply bot.

It looks for signals like replies posted within seconds of other posts from the same account, or accounts that never post their own content (or images or links) but instead consistently reply to messages from other, higher-follower users.

It also looks for question marks, because I'm *extra* infuriated by reply bots that I no tie me to waste my time answering a question that no human ever posed.
