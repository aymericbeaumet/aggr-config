---
title: '[AINews] AMD buys World Labs for $8.2B, as Atlas solves sparse reconstruction problem for robotics, design and more'
link: https://www.latent.space/p/ainews-amd-buys-world-labs-for-82b
source: latent-space
published: 2026-09-29T02:55:27Z
updated: 2026-09-29T02:55:27Z
first_seen: 2026-09-29T07:30:54.171276122Z
summary: Congrats team!
content: extracted
html: 2026-09-29-ainews-amd-buys-world-labs-for-8-2b-as-atlas-solves-sparse.html
preview:
  file: 2026-09-29-ainews-amd-buys-world-labs-for-8-2b-as-atlas-solves-sparse.preview-49a66886565d.webp
  width: 256
  height: 144
  color: '#4c2c31'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/60iW8FZ7MJU
  original:
    file: 2026-09-29-ainews-amd-buys-world-labs-for-8-2b-as-atlas-solves-sparse.image-40b866059e0c.jpg
    width: 728
    height: 410
  color: '#080415'
---

The [official post](https://x.com/theworldlabs/status/2104665621120311465) is shy, but since AMD is public, we know [the purchase price](https://x.com/PaulBonnet/status/2104684279057773052). We [covered them](https://www.latent.space/p/after-llms-spatial-intelligence-and) less than a year ago:

[www.youtube.com](https://www.youtube.com/watch?v=60iW8FZ7MJU)

Fei Fei has a [lovely reflection blogpost](https://drfeifei.substack.com/p/worldlabs-joining-amd) that hints at the main reasons:

> *Since our founding in 2024, World Labs has built **leading AI spatial intelligence capabilities for everything from creative work to design**. We built the world leading **model training team for images, video and spatial reconstruction**. And, with the acquisition of [SceniX](https://www.worldlabs.ai/blog/real-to-sim-to-real), we’re building towards an industry leading capability for **robotics simulation**.*
>
> *Recently we released [Atlas](https://www.worldlabs.ai/blog/atlas), a first of its kind omni model architecture that solves a key outstanding problem in spatial intelligence: **new camera view prediction**. Like LLMs can predict the next token from a line of text, Atlas, trained from scratch, can predict the next view from an input of 2D images, outperforming state of the art results even by specialized models. It has essentially solved a long standing problem in computer vision called **sparse reconstruction, by combining generative models with multiview geometry**. This has direct and far reaching consequences: from **design and engineering to science and robotics**.*
>
> *We’ve seen incredible interest in Atlas across many domains: RL environments for robotics; scene generation for therapy and entertainment; and real world reconstruction for real estate, design and construction, and so much more to come*.

See also [our Claude Code pod](https://www.youtube.com/watch?v=IZAlq-V19U8) out today:

> AI News for 9/26/2026-9/28/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**Top Story: Claude Sonnet 5.5 launch and reactions**

**Anthropic shipped Claude Sonnet 5.5, the second model in the Claude 5.5 family, one week after Opus 5.5 and the day before OpenAI DevDay. Early independent evals place it at or near Opus 5.5 on several leaderboards.**

- **Launch timing:** Pre-launch chatter came first. [@kimmonismus](https://x.com/kimmonismus/status/2104581635412828550) reported it already routing to his account, and [@scaling01](https://x.com/scaling01/status/2104631080372285480) spotted it in the Anthropic API before the official post.

- **Official announcement:** [@claudeai](https://x.com/claudeai/status/2104633115620823187) (53K engagement) and [@AnthropicAI](https://x.com/AnthropicAI/status/2104633259925630995) called it “a clear upgrade over Sonnet 5.” They claim it runs more than 30% faster and costs up to 30% less for most work.

- **Positioning:** [@ClaudeDevs](https://x.com/ClaudeDevs/status/2104641318555353400) positions it for “well-scoped everyday tasks like fixing bugs and quickly iterating on features.” Anthropic also published a [build guide](https://x.com/ClaudeDevs/status/2104687805876367793) covering when to pick Sonnet vs. Opus 5.5, migrating from Sonnet 5, and tuning effort.

- **Free tier:** [@simonw](https://x.com/simonw/status/2104682232522944909) points out Sonnet 5.5 now powers the free tier on claude.ai. ChatGPT’s free tier is still GPT-5.6 Luna, which he calls “a lot less capable.”

- **Anti-distillation change:** [@ClaudeDevs](https://x.com/ClaudeDevs/status/2104641322137293209) extended “preserved thinking” to counter distillation via account-switching. Reasoning traces stay in the org that generated them. If a session moves to another account, Claude rereads it and regenerates thinking.

- **Roadmap:** [@mikeyk](https://x.com/mikeyk/status/2104643949440962946) said Haiku 5.5 will “round out the family in the coming weeks.”

- **Availability:** It shipped day-one on the Claude Platform and Claude Code, along with a usage reset valid until Oct 22 ( [@ClaudeDevs](https://x.com/ClaudeDevs/status/2104641323198472430)). Third-party availability:

  - GitHub Copilot in VS Code ( [@code](https://x.com/code/status/2104645688663343514))

  - > Cursor
    >
    > [@cursor\_ai on X](https://x.com/cursor_ai/status/2104666044220821594)

  - > Factory
    >
    > [@FactoryAI on X](https://x.com/FactoryAI/status/2104640328611639731)

  - > Devin Desktop/CLI
    >
    > [@cognition on X](https://x.com/cognition/status/2104670026770919586)

  - > Cline
    >
    > [@cline on X](https://x.com/cline/status/2104654757508129113)

  - Arena Agent/Battle modes for WebDev, Text, Vision and Document ( [@arena](https://x.com/arena/status/2104640009232117859))

  - T3 Code, after [@theo](https://x.com/theo/status/2104684406900490264) admitted it hadn’t been added to the catalog yet
