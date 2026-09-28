---
title: 'We just shipped support for the ugliest part of HTTP: Vary'
link: https://simonwillison.net/2026/Sep/23/hn-49823961/
source: simonwillison-net
published: 2026-09-23T23:14:57Z
updated: 2026-09-23T23:14:57Z
first_seen: 2026-09-28T06:49:16.850449770Z
labels:
- cloudflare
- http
summary: 'My comment on We just shipped support for the ugliest part of HTTP: Vary — Hacker News. I''ve been wanting this from Cloudflare for years. The classic problem here is if you do that thing where user agents that send "accept: text/html" get HTML, while user agents that don''t get JSON or some other format. This used to be impossible to deploy behind Cloudflare caching, because they ignored the Vary header on anything other than images - so you risked caching the JSON version and then serving it up to someone who was expecting HTML. (Independent of the Cloudflare feature I ended up deciding never to use that pattern, because I prefer having URL that predictably returns HTML or JSON - I add a .json suffix to my apps to serve JSON instead.) Tags: http, cloudflare'
content: extracted
html: 2026-09-23-we-just-shipped-support-for-the-ugliest-part-of-http-vary.html
---

I've been wanting this from Cloudflare *for years*.

The classic problem here is if you do that thing where user agents that send "accept: text/html" get HTML, while user agents that don't get JSON or some other format.

This used to be impossible to deploy behind Cloudflare caching, because they ignored the Vary header on anything other than images - so you risked caching the JSON version and then serving it up to someone who was expecting HTML.

(Independent of the Cloudflare feature I ended up deciding never to use that pattern, because I prefer having URL that predictably returns HTML or JSON - I add a .json suffix to my apps to serve JSON instead.)
