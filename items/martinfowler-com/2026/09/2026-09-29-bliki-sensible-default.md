---
title: 'Bliki: Sensible Default'
link: https://martinfowler.com/bliki/SensibleDefault.html
source: martinfowler-com
published: 2026-09-29T13:43:00Z
updated: 2026-09-29T13:43:00Z
first_seen: 2026-09-29T14:21:22.166029648Z
authors:
- Martin Fowler
labels:
- bliki
content: extracted
html: 2026-09-29-bliki-sensible-default.html
preview:
  file: 2026-09-29-bliki-sensible-default.preview-4cb4da40dc7d.webp
  width: 144
  height: 144
  color: '#5f4679'
images:
- source: https://martinfowler.com/logo-sq.png
  original:
    file: 2026-09-29-bliki-sensible-default.image-271f64133582.png
    width: 144
    height: 144
  variants:
  - file: 2026-09-29-bliki-sensible-default.image-2151866ee9f5.webp
    width: 144
    height: 144
  color: '#070d49'
---

A Sensible Default is a practice that, absent some overriding context, should be used when carrying out a certain kind of task. In software development such sensible defaults might include things like “use version control”, “separate UI logic from domain logic”, “automate deployment pipelines”.

The term “sensible default” is a deliberate contrast to “best practice”. Folks dislike calling things “best practice” because that term implies a general presumption that the best practice is something we should always expect to do. A “sensible default”, however, is something that should be reassessed in a new context, something that can (and should) be overridden when circumstances change.

I first heard the term when it was popularized within Thoughtworks by Evan Bottcher. He got the name from talking to James Ross, and found the phrasing appealing as carried the nuance he was seeking, a known-good starting point.

> A sensible default is what we'd expect you to do, the practices to apply, if there are no hard constraints in the environment. Do these practices, or do better, and be prepared to explain why you've chosen some other way.
>
> -- Evan Bottcher

Thoughtworks has since made much of this concept, including [publishing a playbook](https://www.thoughtworks.com/insights/topic/sensible-defaults) of the ones we use. We expect people to be familiar with these defaults, ready to use them when starting any new piece of work. They are our defaults because we've used them in many situations and found them to be effective. But teams should also be familiar with their limitations, and able to judge whether they should be changed depending on the particular circumstances. As the [twelfth agile principle](https://agilemanifesto.org/principles.html) says: “At regular intervals, the team reflects on how to become more effective, then tunes and adjusts its behavior accordingly.” We also reassess these defaults regularly - this is the heart of the [Thoughtworks Technology Radar](https://www.thoughtworks.com/radar).

Searching on the web led me to [a post from Steve Bennett on Sensible Defaults](https://stevebennett.co/posts/the-power-of-sensible-defaults). The post was written at about the time Evan talked to James. I do not know whether it was where James got the name from, or whether it appeared in parallel.
