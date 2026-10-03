---
title: Event Driven Orbs
link: https://ampcode.com/news/event-driven-orbs
source: ampcode-com
published: 2026-07-23T00:00:00Z
updated: 2026-07-23T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Amp''s orbs can now receive requests and react to events outside Amp. That means an orb can wake up when CI fails on GitHub, when someone opens a Linear issue, when a monitor raises an alert, or when an event arrives from Discord. If it can send an HTTP request, it can wake an orb. More Ways to Wake an Orb GitHub issues are just one example. You can use the same pattern to: Investigate every CI failure on main and post the findings to Slack. Watch for new releases of your dependencies, then review the changes and open an upgrade PR. Start a fresh thread when someone opens a Linear issue, then comment with a fix or report. Turn a bug report from Discord into a reproduction and pull request. Resume a rollout when a deployment or security scan reports back. The event decides when the orb wakes up. You decide what it does next. From a GitHub Event to an Orb Here is the whole setup. Start a thread in an orb for your repository and tell Amp which events to watch and what to do with them: Amp turns that request into a project-specific plugin. It scopes the listener to the repository and events you asked for, verifies GitHub''s signature, deduplicates deliveries, and starts a read-only orb thread with trusted event metadata. Then Amp loads the plugin and registers its durable endpoint: If the orb''s GitHub token can administer repository webhooks, Amp connects the endpoint for you. In this case it could not, so Amp gave us one manual step without printing the private URL or signing secret into the thread: A Wild Issue Appears Once the webhook is active, someone opens issue #57: GitHub sends the signed event to Amp. Amp verifies it and starts a fresh orb thread with the trusted repository, event, issue, and actor metadata. The issue itself remains untrusted input, not agent instructions: The new thread inspects the current issue and relevant code, then reports what it found: The typo is real, appears once, and only affects secondary menu text. Now the same workflow runs for every issue and pull request event while the original orb sleeps. How It Works Webhooks work through the Amp Plugin API. When you ask Amp to listen for an event, it creates a plugin in the orb and calls amp.createWebhook to register a durable endpoint for that thread. Then it loads the plugin and gives you the URL to connect to GitHub, Linear, Discord, or another service. If the orb already has access, Amp can connect it for you. When a request arrives, Amp stores the event and wakes the orb. The plugin validates and filters the payload, then handles it using the instructions you gave Amp. The URL stays the same across plugin reloads and orb restarts, so the orb does not need to keep running while it waits. React to Events However You Want amp.createWebhook gives the plugin a handler, not a fixed workflow. That handler is ordinary TypeScript with access to the rest of the Plugin API. The handler can: Continue the owning thread with its context intact by appending the event to ctx.thread. Start a fresh thread in an orb with amp.getBuiltinAgent(...).createThread({ executor: ''orb'' }). Keep durable state so it can react every time, or handle one matching event and then stop listening. That is where the flexibility comes from. Tell Amp how you want to handle an event and it writes the handler that way. It can also call external APIs to post results back to Slack, Linear, GitHub, or wherever the work began. Return to the owning thread whenever you want to change the behavior. The webhook URL is a credential. Keep it private, and tell Amp to remove it when you no longer need it.'
content: extracted
html: 2026-07-23-event-driven-orbs.html
preview:
  file: 2026-07-23-event-driven-orbs.preview-9796893711cb.webp
  width: 256
  height: 134
  color: '#5d564c'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Event+Driven+Orbs&date=July+23%2C+2026&tagline=Orbs+can+now+receive+requests+and+react+to+outside+events.&sig=f158e78b159207ad135a833345491454e9f3ec5100875640763ce67bd18910bf
  original:
    file: 2026-07-23-event-driven-orbs.image-71a653170a70.png
    width: 1200
    height: 630
  variants:
  - file: 2026-07-23-event-driven-orbs.image-a7eb14eb1e01.webp
    width: 320
    height: 168
  - file: 2026-07-23-event-driven-orbs.image-e6bc8b4435bc.webp
    width: 640
    height: 336
  - file: 2026-07-23-event-driven-orbs.image-b3717c164c36.webp
    width: 960
    height: 504
  - file: 2026-07-23-event-driven-orbs.image-69c4d363713a.webp
    width: 1200
    height: 630
  color: '#221b16'
- source: https://ampcode.com/news/webhook-automations-github-prompt.png
  original:
    file: 2026-07-23-event-driven-orbs.image-190f5c7b5744.png
    width: 693
    height: 106
  variants:
  - file: 2026-07-23-event-driven-orbs.image-11e1054d32f1.webp
    width: 320
    height: 49
  - file: 2026-07-23-event-driven-orbs.image-15cb069e6ba4.webp
    width: 640
    height: 98
  - file: 2026-07-23-event-driven-orbs.image-fb8e9855ef75.webp
    width: 693
    height: 106
  color: '#f7fbf4'
- source: https://ampcode.com/news/webhook-automations-plugin-plan.png
  original:
    file: 2026-07-23-event-driven-orbs.image-31fc4f14855f.png
    width: 816
    height: 106
  variants:
  - file: 2026-07-23-event-driven-orbs.image-b6ad9299dbef.webp
    width: 320
    height: 42
  - file: 2026-07-23-event-driven-orbs.image-fd1b805ffd13.webp
    width: 640
    height: 83
  - file: 2026-07-23-event-driven-orbs.image-5dae8b32b9df.webp
    width: 816
    height: 106
  color: '#f8fcf4'
- source: https://ampcode.com/news/webhook-automations-plugin-loaded.png
  original:
    file: 2026-07-23-event-driven-orbs.image-b28dd3991d11.png
    width: 806
    height: 88
  variants:
  - file: 2026-07-23-event-driven-orbs.image-e600b4d8fe3b.webp
    width: 320
    height: 35
  - file: 2026-07-23-event-driven-orbs.image-d5748b5b12b8.webp
    width: 640
    height: 70
  - file: 2026-07-23-event-driven-orbs.image-20d50c5bd986.webp
    width: 806
    height: 88
  color: '#f8fcf4'
- source: https://ampcode.com/news/webhook-automations-github-setup.png
  original:
    file: 2026-07-23-event-driven-orbs.image-eba1945138a1.png
    width: 826
    height: 670
  variants:
  - file: 2026-07-23-event-driven-orbs.image-e2e8a9918176.webp
    width: 320
    height: 260
  - file: 2026-07-23-event-driven-orbs.image-c79a0491d34e.webp
    width: 640
    height: 519
  - file: 2026-07-23-event-driven-orbs.image-f58ec528906c.webp
    width: 826
    height: 670
  color: '#f8fcf4'
- source: https://ampcode.com/news/webhook-automations-github-issue.png
  original:
    file: 2026-07-23-event-driven-orbs.image-18a0fbd07ad7.png
    width: 1181
    height: 366
  variants:
  - file: 2026-07-23-event-driven-orbs.image-9d19c892b8eb.webp
    width: 320
    height: 99
  - file: 2026-07-23-event-driven-orbs.image-e5872fa35b2c.webp
    width: 640
    height: 198
  - file: 2026-07-23-event-driven-orbs.image-831c3d8cb62c.webp
    width: 960
    height: 298
  - file: 2026-07-23-event-driven-orbs.image-e4217269bf07.webp
    width: 1181
    height: 366
  color: '#fdfdfe'
- source: https://ampcode.com/news/webhook-automations-spawned-thread.png
  original:
    file: 2026-07-23-event-driven-orbs.image-c02450ce805b.png
    width: 701
    height: 505
  variants:
  - file: 2026-07-23-event-driven-orbs.image-17f6519d9c63.webp
    width: 320
    height: 231
  - file: 2026-07-23-event-driven-orbs.image-bc962fcd6790.webp
    width: 640
    height: 461
  - file: 2026-07-23-event-driven-orbs.image-4f9ba760db7e.webp
    width: 701
    height: 505
  color: '#e2e5de'
- source: https://ampcode.com/news/webhook-automations-investigation-report.png
  original:
    file: 2026-07-23-event-driven-orbs.image-33a8b6a9f4ea.png
    width: 817
    height: 487
  variants:
  - file: 2026-07-23-event-driven-orbs.image-296e9825f545.webp
    width: 320
    height: 191
  - file: 2026-07-23-event-driven-orbs.image-67ddd736888e.webp
    width: 640
    height: 381
  - file: 2026-07-23-event-driven-orbs.image-d20907d17886.webp
    width: 817
    height: 487
  color: '#f8fcf5'
---

Amp's orbs can now receive requests and react to events outside Amp.

That means an orb can wake up when CI fails on GitHub, when someone opens a Linear issue, when a monitor raises an alert, or when an event arrives from Discord. If it can send an HTTP request, it can wake an orb.

## More Ways to Wake an Orb

GitHub issues are just one example. You can use the same pattern to:

- Investigate every CI failure on `main` and post the findings to Slack.
- Watch for new releases of your dependencies, then review the changes and open an upgrade PR.
- Start a fresh thread when someone opens a Linear issue, then comment with a fix or report.
- Turn a bug report from Discord into a reproduction and pull request.
- Resume a rollout when a deployment or security scan reports back.

The event decides when the orb wakes up. You decide what it does next.

## From a GitHub Event to an Orb

Here is the whole setup. Start a thread in an orb for your repository and tell Amp which events to watch and what to do with them:

![Prompt asking Amp to monitor GitHub issues and pull requests with a webhook](https://ampcode.com/news/webhook-automations-github-prompt.png)

Amp turns that request into a project-specific plugin. It scopes the listener to the repository and events you asked for, verifies GitHub's signature, deduplicates deliveries, and starts a read-only orb thread with trusted event metadata.

![Amp describing the GitHub webhook plugin it will build](https://ampcode.com/news/webhook-automations-plugin-plan.png)

Then Amp loads the plugin and registers its durable endpoint:

![Amp registering the plugin's durable webhook endpoint](https://ampcode.com/news/webhook-automations-plugin-loaded.png)

If the orb's GitHub token can administer repository webhooks, Amp connects the endpoint for you. In this case it could not, so Amp gave us one manual step without printing the private URL or signing secret into the thread:

![Amp explaining the manual GitHub webhook configuration step](https://ampcode.com/news/webhook-automations-github-setup.png)

## A Wild Issue Appears

Once the webhook is active, someone opens issue #57:

![GitHub issue 57 reporting a typo in a completion description](https://ampcode.com/news/webhook-automations-github-issue.png)

GitHub sends the signed event to Amp. Amp verifies it and starts a fresh orb thread with the trusted repository, event, issue, and actor metadata. The issue itself remains untrusted input, not agent instructions:

![A new Amp thread created from a verified GitHub issue webhook](https://ampcode.com/news/webhook-automations-spawned-thread.png)

The new thread inspects the current issue and relevant code, then reports what it found:

![Amp report confirming issue 57 as a low-impact UI typo](https://ampcode.com/news/webhook-automations-investigation-report.png)

The typo is real, appears once, and only affects secondary menu text. Now the same workflow runs for every issue and pull request event while the original orb sleeps.

## How It Works

Webhooks work through the [Amp Plugin API](https://ampcode.com/manual/plugin-api). When you ask Amp to listen for an event, it creates a plugin in the orb and calls `amp.createWebhook` to register a durable endpoint for that thread. Then it loads the plugin and gives you the URL to connect to GitHub, Linear, Discord, or another service. If the orb already has access, Amp can connect it for you.

When a request arrives, Amp stores the event and wakes the orb. The plugin validates and filters the payload, then handles it using the instructions you gave Amp. The URL stays the same across plugin reloads and orb restarts, so the orb does not need to keep running while it waits.

## React to Events However You Want

`amp.createWebhook` gives the plugin a handler, not a fixed workflow. That handler is ordinary TypeScript with access to the rest of the Plugin API. The handler can:

- Continue the owning thread with its context intact by appending the event to `ctx.thread`.
- Start a fresh thread in an orb with `amp.getBuiltinAgent(...).createThread({ executor: 'orb' })`.
- Keep durable state so it can react every time, or handle one matching event and then stop listening.

That is where the flexibility comes from. Tell Amp how you want to handle an event and it writes the handler that way. It can also call external APIs to post results back to Slack, Linear, GitHub, or wherever the work began. Return to the owning thread whenever you want to change the behavior.

The webhook URL is a credential. Keep it private, and tell Amp to remove it when you no longer need it.
