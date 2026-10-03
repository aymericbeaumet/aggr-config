---
title: 'I got targeted: Trying to get your credentials via a git post-checkout hook'
link: https://frankwiles.com/posts/i-got-targeted/
source: lobste-rs
published: 2026-10-02T22:19:02Z
updated: 2026-10-02T22:19:02Z
first_seen: 2026-10-03T11:34:56.593287151Z
authors:
- frankwiles.com by frankwiles
labels:
- django
- opensource
- security
- vcs
summary: Comments
content: extracted
html: 2026-10-02-i-got-targeted-trying-to-get-your-credentials-via-a-git.html
preview:
  file: 2026-10-02-i-got-targeted-trying-to-get-your-credentials-via-a-git.preview-35e71ea8f7a3.webp
  width: 256
  height: 134
  alt: I got targeted — an essay by Frank Wiles
  color: '#1d1d20'
images:
- source: https://frankwiles.com/og/posts/i-got-targeted.jpg?v=1tsueg2
  original:
    file: 2026-10-02-i-got-targeted-trying-to-get-your-credentials-via-a-git.image-33b00706fae1.jpg
    width: 1200
    height: 630
  color: '#131417'
- source: https://frankwiles.com/_astro/mhrezaa-GRYHwCxL9wQ-unsplash.BR2RnPe1_pEAdW.webp
  original:
    file: 2026-10-02-i-got-targeted-trying-to-get-your-credentials-via-a-git.image-43b7cd0df888.webp
    width: 3180
    height: 1989
  variants:
  - file: 2026-10-02-i-got-targeted-trying-to-get-your-credentials-via-a-git.image-02a418ac73bc.webp
    width: 320
    height: 200
  color: '#67a7d3'
- source: https://frankwiles.com/images/headshot-300.jpg
  original:
    file: 2026-10-02-i-got-targeted-trying-to-get-your-credentials-via-a-git.image-6d70fd8dc19c.jpg
    width: 300
    height: 300
  color: '#090909'
---

> TL;DR Someone targeted me in an attempt to run arbitrary code on my laptop. I suspect in an attempt to gain access to my Github account and/or other [REVSYS client](https://www.revsys.com/clients/) related access since I have a metric fuck ton of it.

Be careful out there folks. They’re coming and they’re fucking sneaky!

I received a pretty normal project inquiry looking to see if we might be interested and available to work on a web app project in the Ed Tech space. We do a fair bit of that work and while we’re pretty booked up, I usually follow up on these sorts of projects in case the client is able to delay the project start until we have availablity.

I offered to setup a call with them and gave them a Calendly link. They asked that I read over the project overview and details prior to the meeting and sign an NDA.

Pretty normal stuff so far.

They then shared a Dropbox folder that had several folders of Markdown files. The project spec was pretty handwavey and light on details, but fleshed out enough for the MVP they supposedly wanted.

I initially missed the `.git` folder in that Dropbox link.

When I couldn’t find a NDA or NDA template in the folders I asked for them to email it to me.

They told me:

> We keep it in the NDA branch and just to switch to it, fill it out and return it to them before our meeting

![Red flag on a pole under a blue sky](https://frankwiles.com/_astro/mhrezaa-GRYHwCxL9wQ-unsplash.BR2RnPe1_pEAdW.webp)

This is where I realized what was going on and that this wasn’t a real project. I cruised on over to the `.git/hooks` folder and sure enough they had all of the `*.example` hooks in there and a single real `post-checkout` hook.

## post-checkout hook, seriously?!?!?!

No one really uses those in practice so I carefully opened it up to see what it was doing.

It was using a Vercel app for [command and control](https://en.wikipedia.org/wiki/Botnet#Command_and_control) where it would download an OS specific binary, make it executuable, run it, and then delete itself.

I immediately alerted Dropbox and Vercel’s security teams so they can hopefully take these accounts down before they snag someone. Sadly, they also were impersonating an unspecting development shop owner as part of the ruse.

These assholes didn’t get me today, but I can easily see someone falling for this. Git is such a common workflow for us.

Be extra vigilant and watch your credentials like a hawk. They’re coming for you on some level.

Posted 02 October 2026

[![Headshot of Frank Wiles](https://frankwiles.com/images/headshot-300.jpg)](https://frankwiles.com/ "Frank Wiles Homepage")

Infrequent Insights

### Join my newsletter!

Get the occasional email from me when I write something new.

Ask Frank Anything

Struggling with architecture decisions or team dynamics? Ask me any tech, business process, or entrepreneurial question, and I'll do my best to help!

[Submit a Question](https://forms.frankwiles.com/ask-frank)

[frankwiles.com](https://frankwiles.com/)©2026Frank Wiles. All Rights Reserved.
