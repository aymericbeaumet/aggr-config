---
title: What To Do If Dependency Teams Can’t Help
link: https://eugeneyan.com//writing/getting-help/
source: eugeneyan-com
published: 2023-01-08T00:00:00Z
updated: 2023-01-08T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- leadership
- misc
summary: Seeking first to understand, earning trust, and preparing for away team work.
content: extracted
html: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.html
preview:
  file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.preview-4323dc122e90.webp
  width: 256
  height: 134
  color: '#b58387'
images:
- source: https://eugeneyan.com/assets/og_image/dependency.jpeg
  original:
    file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-aff611d4a070.jpg
    width: 1200
    height: 630
  color: '#b8b7b6'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-01-08-what-to-do-if-dependency-teams-can-t-help.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Other than in small organizations, most teams have interdependencies with other teams. For example, a machine learning team may rely on upstream data teams to provide feature data during offline training and online serving. The same ML team may then depend on downstream infra teams to support serving their models at scale.

What can we do if dependency teams won’t or can’t help? This is a common question during chats with peers and mentees. The typical response is to escalate. Alternatively, let’s write a lengthy JIRA ticket explaining why the request is urgent and important to meet the team’s goal. Besides these, here are some approaches I’ve found long-term success with.

**Why won’t or can’t they help?**—do we know? Often, we don’t because we didn’t [seek first to understand before trying to convince them](https://www.franklincovey.com/habit-5/). We can do this by trying to understand their constraints and priorities. Are they unable to help because of resource or technical bottlenecks? Or does our request conflict with their priority of migrating off a tech stack? Also, what does it mean for them to be successful? If we understand their constraints, we can help work around, or even remove, those constraints. If we understand their priorities, we can consciously collaborate to achieve them.

**Achieving mutual understanding hinges on earning trust.** It’s easier to be frank about constraints and priorities if we trust each other. We won’t go into a detailed discussion on how to earn trust but a few simple ways include understanding their needs and acting in their interests, listening and adapting to their feedback, and being dependable and delivering when others need your help.

**“But I’m offering to write the code, why do they refuse my help?”** Depending on the situation, your *help* can cost more effort than them doing it themselves. Imagine having a new developer help with implementing a feature in an existing system: An experienced mentor has to onboard them, provide context and documentation, and likely help with environment setup, debugging error messages, and code reviews. In the short term, this increases work instead of reducing it.

**For another team to readily accept code contributions, they need to be set up for away team work.** [Away](https://www.sledgeworx.io/away-team-work/) [team](https://pedrodelgallego.github.io/blog/amazon/operating-model/away-team-model/) work is where an *away* team implements a feature, pipeline, or integration in the *host* team’s systems. This allows away teams to unblock themselves, thus reducing inter-team dependency. Host teams may need mechanisms such as office hours for consultation, self-service documentation and onboarding, and a review process for architecture design and code. Setting up for away team work is a significant investment and thus not many teams will have that mechanism in place.

**Finally, perhaps we can frame the work as an investment**, where the dependency team invests in mentoring you for the current code contribution so you can mentor your team for future code changes. For them, it reduces the effort of supporting future requests from your team and is a trial for away team work. For you, it’s a solution to implementing what you need and an opportunity to learn and teach about the host team’s code base. Win-win.

Have you come across similar situations in the past? What helped with navigating them?

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Jan 2023). What To Do If Dependency Teams Can’t Help. eugeneyan.com. https://eugeneyan.com/writing/getting-help/.

or

```
@article{yan2023dependency,
  title   = {What To Do If Dependency Teams Can’t Help},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Jan},
  url     = {https://eugeneyan.com/writing/getting-help/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
