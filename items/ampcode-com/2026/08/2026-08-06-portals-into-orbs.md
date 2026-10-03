---
title: Portals into Orbs
link: https://ampcode.com/news/portals
source: ampcode-com
published: 2026-08-06T00:00:00Z
updated: 2026-08-06T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'For remote development in orbs to be better than local dev, you need to be able to easily try out the agent''s changes in your app, with live reloading. No VPN, no port juggling, and no waiting for preview deployments. Today, we''re shipping portals, which let you access anything running in an orb that listens on a port and speaks HTTP. All you need to do is ask Amp: show me in a portal. Or words to that effect. Obviously, you can use portals to try out the features or fixes made by the agent: You can also annotate and comment on anything: But you can also have the agent build ad-hoc web apps for you to debug or understand the system: Portals are accessible to anyone with access to the thread. They go to sleep and wake along with your orb. Use the gear icon in the Portal address bar to change the hostname or check who has access. Make a thread multiplayer with on the web, and your team members can also make changes and see them live in the portal. Services When you say show me in a portal, the agent knows to create or look for a .amp/services.yaml file and then run amp orb services <ensure|start> to run your app and expose it via HTTPS. Your app needs to respect the PORT and PUBLIC_URL env vars it''s given. Amp will handle all of this for you; agents are really good at that kind of stuff. Commit the .amp/services.yaml file and Amp''s changes to your dev server config so it''s faster next time. What about my app''s sign-in flow? What about ...? You don''t want to waste time signing into your dev server each time. We recommend adding a way for humans and agents to bypass sign-in flows (such as username/password or OAuth) in your dev server, by visiting a URL like: https://localhost:2000/__dev/log-me-in/{email}?returnTo={path} We''ve documented this pattern and more in the portals documentation. Also see Putting an Agent in an Orb for more about how we''re using portals. Tell us how else you and your agents are using portals!'
content: extracted
html: 2026-08-06-portals-into-orbs.html
preview:
  file: 2026-08-06-portals-into-orbs.preview-38c8f12ba4d9.webp
  width: 256
  height: 134
  color: '#645d48'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Portals+into+Orbs&date=August+6%2C+2026&tagline=Try+the+agent%27s+changes+to+your+app+in+orbs%2C+with+live+reloading.&backgroundImage=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fportals-background.jpg&sig=0976f9409dbbda5af9a2d420b94e6c7468b692cba3cd7edbd00a18e87254725a
  original:
    file: 2026-08-06-portals-into-orbs.image-cea4be1755d4.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-06-portals-into-orbs.image-968ddfd6dd97.webp
    width: 320
    height: 168
  - file: 2026-08-06-portals-into-orbs.image-0ff87198995a.webp
    width: 640
    height: 336
  - file: 2026-08-06-portals-into-orbs.image-cba120c67546.webp
    width: 960
    height: 504
  - file: 2026-08-06-portals-into-orbs.image-4e6e285e70d2.webp
    width: 1200
    height: 630
  color: '#25281a'
---

For remote development in [orbs](https://ampcode.com/manual/orbs) to be better than local dev, you need to be able to easily try out the agent's changes in your app, with live reloading. No VPN, no port juggling, and no waiting for preview deployments.

Today, we're shipping [portals](https://ampcode.com/manual/orbs#portals), which let you access anything running in an orb that listens on a port and speaks HTTP.

All you need to do is ask Amp: *show me in a portal*. Or words to that effect.

Obviously, you can use portals to try out the features or fixes made by the agent:

You can also annotate and comment on anything:

But you can also have the agent build ad-hoc web apps for you to debug or understand the system:

Portals are accessible to anyone with access to the thread. They go to sleep and wake along with your orb. Use the gear icon in the Portal address bar to change the hostname or check who has access.

Make a thread [multiplayer](https://ampcode.com/news/multiplayer) with on the web, and your team members can *also* make changes and see them live in the portal.

## Services

When you say *show me in a portal*, the agent knows to create or look for a [`.amp/services.yaml`](https://ampcode.com/manual/orbs#portals) file and then run `amp orb services <ensure|start>` to run your app and expose it via HTTPS. Your app needs to respect the `PORT` and `PUBLIC_URL` env vars it's given.

Amp will handle all of this for you; agents are really good at that kind of stuff. Commit the `.amp/services.yaml` file and Amp's changes to your dev server config so it's faster next time.

## What about my app's sign-in flow? What about ...?

You don't want to waste time signing into your dev server each time. We recommend adding a way for humans and agents to bypass sign-in flows (such as username/password or OAuth) in your dev server, by visiting a URL like:

```
https://localhost:2000/__dev/log-me-in/{email}?returnTo={path}
```

We've documented this pattern and more in the [portals documentation](https://ampcode.com/manual/orbs#portals). Also see [Putting an Agent in an Orb](https://ampcode.com/notes/putting-an-agent-in-an-orb) for more about how we're using portals. Tell us how else you and your agents are using portals!
