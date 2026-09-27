---
title: Setup OpenClaw 2.0 with Gemini in Under 60 Seconds
link: https://www.philschmid.de/openclaw-v2-gemini
source: philschmid-de
published: 2026-08-31T00:00:00Z
updated: 2026-08-31T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Five commands, zero fluff: install OpenClaw 2.0, connect Gemini 3.8 Flash, and start chatting with live Google Search grounding from your terminal and web dashboard in under a minute.'
content: extracted
html: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.html
preview:
  file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.preview-ce6b27122df7.webp
  width: 256
  height: 134
  alt: Setup OpenClaw 2.0 with Gemini in Under 60 Seconds
  color: '#18191f'
images:
- source: https://www.philschmid.de/static/blog/openclaw-v2-gemini/thumbnail.jpg
  original:
    file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-46d15bbc29c4.jpg
    width: 1500
    height: 785
  color: '#0a0c10'
- source: https://www.philschmid.de/static/blog/openclaw-v2-gemini/cli-setup.png
  original:
    file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-71cb349d3e6e.png
    width: 2060
    height: 1108
  color: '#11141b'
- source: https://www.philschmid.de/static/blog/openclaw-v2-gemini/openclaw-control-ui.png
  original:
    file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-cc1bb9889bcb.png
    width: 1420
    height: 1030
  variants:
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-6de0eaacca75.webp
    width: 320
    height: 232
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-42ad6071805a.webp
    width: 640
    height: 464
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-d68a87000fd8.webp
    width: 960
    height: 696
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-d70b8b7f9b2d.webp
    width: 1280
    height: 928
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-0d37667a6cd8.webp
    width: 1420
    height: 1030
  color: '#0e1015'
- source: https://www.philschmid.de/static/blog/openclaw-v2-gemini/openclaw-websearch.png
  original:
    file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-8cc8bef52940.png
    width: 1420
    height: 1030
  variants:
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-3844f1cdcc9c.webp
    width: 320
    height: 232
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-4970892a3cf4.webp
    width: 640
    height: 464
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-4991c6011f0c.webp
    width: 960
    height: 696
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-35d43bb21211.webp
    width: 1280
    height: 928
  - file: 2026-08-31-setup-openclaw-2-0-with-gemini-in-under-60-seconds.image-1c4f025ea5e1.webp
    width: 1420
    height: 1030
  color: '#0e1015'
---

Five commands, zero fluff: install OpenClaw 2.0 (`v2026.8.1`), link [Gemini 3.8 Flash](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-3-7-flash/), our fastest (up to [+300 token/sec](https://artificialanalysis.ai/#speed)) agentic model for coding and agents, and start chatting with live Google search grounding in under a minute.

## What is OpenClaw 2.0?

[OpenClaw](https://openclaw.ai) is a personal AI assistant that actually does things. It runs on your devices, in your chats, with your rules.

- **Chat from anywhere:** Chat from the browser Control UI, Telegram, Discord, WhatsApp, or Slack.
- **Integrated tooling:** Browser automation, terminal execution, filesystem access, and web search grounding.
- **Remember:** Keeps memory across sessions so it knows your preferences, projects, and people.

[OpenClaw 2.0](https://openclaw.ai/blog/openclaw-2-accidentally) (`v2026.8.1`) is the largest update yet: simpler install, a rebuilt browser app that opens straight into a conversation, and setup you can finish by talking to your Claw. Full notes: [v2026.8.1](https://docs.openclaw.ai/releases/2026.8.1).

## Prerequisites

- **Node.js:** `Node 26` (recommended) or `Node 22.22.3+` / `24.15+` (`node --version`).
- **Gemini API Key:** Grab a free key from [Google AI Studio](https://aistudio.google.com/apikey).

## 1\. Install OpenClaw 2.0

Install OpenClaw globally via npm:

Bash

```bash
npm install -g openclaw@2026.8.1
```

Verify your installation:

Bash

```bash
openclaw --version
# Output: OpenClaw 2026.8.1 (ea80657)
```

## 2\. Configure Gemini Authentication

Export your Gemini API key in your current shell:

Bash

```bash
export GEMINI_API_KEY="your-gemini-api-key-here"
```

To persist it into OpenClaw's local profile store:

Bash

```bash
openclaw onboard --non-interactive --accept-risk --skip-health \
  --mode local \
  --auth-choice gemini-api-key \
  --gemini-api-key "$GEMINI_API_KEY"
```

*(Note: You can safely ignore any onboarding tip about configuring a Brave API key—Gemini's built-in Google Search grounding is used automatically.)*

## 3\. Set Gemini 3.8 Flash as Default

Set **Gemini 3.8 Flash** as the default agent model:

Bash

```bash
openclaw models set google/gemini-3.8-flash
```

*(Note: If OpenClaw displays a warning that the model is not in the local catalog, it is safe to ignore. The model is dynamically resolved and validated against Google AI Studio in the next step.)*

Verify model discovery and authentication status:

Bash

```bash
openclaw models list --provider google
```

## 4\. Test Inference via CLI

Run a standalone agent turn in your terminal:

Bash

```bash
openclaw agent --local --agent main --message "Hello! What model are you running?"
```

Output:

```text
I am currently running on google/gemini-3.8-flash.
What would you like to call me?
```

![OpenClaw CLI Setup](https://www.philschmid.de/static/blog/openclaw-v2-gemini/cli-setup.png)

## 5\. Start the Gateway & Launch Control UI

Set the gateway to local mode and start it:

Bash

```bash
openclaw config set gateway.mode local
openclaw gateway run
```

*(To run as a persistent background daemon instead: `openclaw gateway install && openclaw gateway start`. To stop it: `openclaw gateway stop --force`)*

Open the Control UI in your browser:

Bash

```bash
openclaw dashboard
```

The browser will open to `http://127.0.0.1:18789/` authenticated with your local session token:

![OpenClaw Control UI with Gemini 3.8 Flash](https://www.philschmid.de/static/blog/openclaw-v2-gemini/openclaw-control-ui.png)

## Next Steps

### Google Search Grounding (Enabled by Default)

Google Search grounding is **enabled by default** when `GEMINI_API_KEY` is present. No extra configuration or search keys are required.

- Test it directly via CLI:

  Bash

  ```bash
  openclaw capability web search --query "Latest gemini models"
  ```

- In chat sessions, Gemini autonomously runs web searches with citations whenever fresh context is needed.

![Google Search Grounding in OpenClaw Control UI](https://www.philschmid.de/static/blog/openclaw-v2-gemini/openclaw-websearch.png)

- See: [Google Search Grounding in OpenClaw](https://docs.openclaw.ai/tools/gemini-search)

### Leveraging Other Gemini Capabilities

- **[Image Generation](https://docs.openclaw.ai/tools/image-generation):** Set Google's image model using `openclaw models set-image google/gemini-3.1-flash-image`.
- **[Video Generation](https://docs.openclaw.ai/tools/video-generation):** Generate 4–8s videos with `google/veo-3.1-fast-generate-preview`.
- **[Music Generation](https://docs.openclaw.ai/tools/music-generation):** Prompt-driven music via `google/lyria-3-clip-preview`.
- **[Text-to-Speech](https://docs.openclaw.ai/providers/google#text-to-speech):** Gemini batch TTS with natural language style prompting (`gemini-3.1-flash-tts-preview`).
- **[Realtime Voice](https://docs.openclaw.ai/providers/google#realtime-voice):** Low-latency bidirectional voice streams via Gemini Live API (`gemini-3.1-flash-live-preview`).

Full [Google Documentation](https://docs.openclaw.ai/providers/google).

### Connecting Chat Channels

Connect your Gemini assistant to your favorite messaging apps using simple CLI commands:

- **Browse all channels:** `openclaw channels list --all`
- **Interactive setup:** `openclaw channels add`
- **Telegram:** `openclaw channels add --channel telegram --token "<BOT_TOKEN>"`
- **WhatsApp (QR code):** `openclaw channels login --channel whatsapp`
- **Discord:** `openclaw channels add --channel discord --token "<BOT_TOKEN>"`
- **Verify channel status:** `openclaw channels status --probe`

Full [Chat Channels Documentation](https://docs.openclaw.ai/channels).
