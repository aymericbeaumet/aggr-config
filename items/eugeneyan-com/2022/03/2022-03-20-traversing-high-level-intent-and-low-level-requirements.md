---
title: Traversing High-Level Intent and Low-Level Requirements
link: https://eugeneyan.com//writing/intent-vs-requirements/
source: eugeneyan-com
published: 2022-03-20T00:00:00Z
updated: 2022-03-20T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- misc
- productivity
summary: How they differ and why they work better in different situations.
content: extracted
html: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.html
preview:
  file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.preview-302c9cd67b82.webp
  width: 256
  height: 134
  color: '#4d5a27'
images:
- source: https://eugeneyan.com/assets/og_image/forest.jpg
  original:
    file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-d4827f569af1.jpg
    width: 1200
    height: 630
  color: '#485725'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2022-03-20-traversing-high-level-intent-and-low-level-requirements.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Have you noticed how, when given a nebulous high-level intent, some people run with it and break it down into smaller problems, while others freeze and don’t know how to begin? On the other hand, when given a detailed 10-page product requirements doc, the former grumbles that it’s too specific while the latter checks the boxes off chirpily?

Why does this happen? I try to answer by distinguishing between high-level intent and low-level requirements and discuss a few factors that influence when to use which.

## High-level intent vs. low-level requirements

High-level intent excludes the details and focuses on the “why” and “what” while low-level requirements dive into the “how”. Put another way, intent zooms out on the big-picture forest while requirements zoom into the detail trees.

Consider programming languages. Low-level languages such as assembly provide little abstraction from computer instructions; we work with machine language and primitives. In contrast, high-level languages like python provide a lot of abstraction and let us write instructions that generate machine instructions. Low-level languages are closer to the machine; high-level languages are closer to the user. Similarly, low-level requirements are closer to technical details while high-level intent is closer to business goals.

**Low-level requirements define *how* the product should work.** Functional aspects include the customer experience, input/output, and user interface mocks. Non-functional aspects include expectations on latency (80ms@p99), scalability (8,000 queries per second), and security (AAA certified). Some examples I found online are Ohio State University’s [sample requirements doc](https://web.cse.ohio-state.edu/~bair.41/616/Project/Example_Document/Req_Doc_Example.html) and the Federal Demonstration Partnership’s (FDP) [requirements for an expanded clearinghouse](https://sites.nationalacademies.org/cs/groups/pgasite/documents/webpage/pga_179129.pdf). The latter includes low-level requirements such as:

> - Public has read-only access via website
> - User accounts are password protected
> - Password reminders and resets are handled by the website
> - Authorized users can update the entity’s profile directly on the website
> - The system is intended to be available online 24 hours per day, 365 days per year with the exception of scheduled and pre-notified system maintenance downtimes, if needed.

**High-level intent communicates the expected outcome (*what*) and rationale (*why*).** Specifying only the outcome is sometimes not enough; it must be paired with the rationale for context. For example, the outcome of introducing new products on an e-commerce platform requires different approaches for different rationales. If the reason is to increase novelty from the customer’s perspective, we consider metrics such as diversity and serendipity. If the reason is to increase seller health from the marketplace’s perspective, we might consider metrics such as count of sellers with at least *x* sales in the past week and proportion of sellers making up *y*% of sales.

Intent statements can be as simple as a sentence, but don’t let their lack of verbosity fool you—often, it’s the requests that are concise that suggest the most challenging projects.

What does high-level intent look like in a document? One example is the goal and tenets section in an [Amazon 6-pager](https://writingcooperative.com/the-anatomy-of-an-amazon-6-pager-fc79f31a41c9?gi=ca48fe78dde). Goals define expected outcomes with quantifiable metrics and timelines. Tenets guide decision-making; they get everyone to agree on critical questions that can’t be verified by data. Here are some [examples of Amazon’s businesses and their tenets](https://www.factoftheday1.com/p/tenets-at-amazon-a2bb8a56ae94):

> - Fulfillment by Amazon — *Sellers are our customers*: FBA simplifies Sellers’ lives — whether they sell locally or globally.
> - Information Security — *Security is measurable*: We produce and communicate metrics on the effectiveness of our security programs.
> - AWS Internet-of-Things — *Necessary offline operations*: We help customers build systems in the cloud that work in predictable ways when connectivity is limited.

## Why either works better and when

There are three factors to consider when deciding whether to communicate high-level intent or low-level requirements: (i) the context and experience of the executor, (ii) the uncertainty of the situation, and (iii) the maturity of the profession.

**The context and experience of the executor** are usually—but not always—positively correlated with developer seniority. Junior developers tend to have less context and experience and may struggle to break down high-level intent into smaller problems and project phases. Thus, they may require more guidance via tightly spec-ed requirements lest they go in the wrong direction. [Test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) is an example of this. Passing all test cases ensures all requirements are met.

On the other hand, senior folks have more context of the business and tech environment, and the experience of tackling similar problems previously. They instinctively know the implicit requirements without you having to specify them, and often relish the opportunity to flex their critical thinking and design skills to achieve your intent most effectively. On the other hand, overly specific requirements may seem like following a step-by-step instruction manual and take the fun out of it.

**The uncertainty of the situation** can be attributed to the knowledge gap (the difference between what we want to know and what we actually know) and the effects gap (the difference between what we want our actions to achieve and what they actually achieve).

For example, at the start of the year, we only have a high-level intent to “increase customer engagement by 10%”. We’re uncertain how our engagement metrics—page views, conversion, daily average users, net promoter score—compare with the rest of the industry, and don’t know which are easier to improve or the levers at our disposal. This *knowledge gap* suggests that we shouldn’t be overly specific on requirements given the environmental uncertainty.

We then decide that daily average users (DAU) is an important metric and hypothesize that more push notifications lead to higher DAU. Thus, we build a feature to notify users when items in their wishlist or cart drop in price. But our hypothesis is wrong. What we thought would be helpful is instead perceived by customers as annoying, causing them to stop push notifications or delete the app. This *effects gap* cautions us from dictating how an outcome should be achieved (in our requirements) when the relationship between actions and outcome is uncertain.

**The maturity of the profession** has an impact on the situation’s uncertainty. Compare software engineering and data science. Software engineering is a mature discipline with well-defined problems and design patterns that map well to outcomes. Need a feature store? The standard approach is to use a key-value store like Redis or DynamoDB. Want to improve latency? We have known strategies (e.g., caching, optimizing VPCs) and trade-offs (e.g., vertical vs. horizontal scaling).

In contrast, data science is younger and doesn’t have similar patterns (yet) for analysis and machine learning. How would we write low-level requirements for an analysis on where to build the next ten Apple stores, or how to allocate marketing spend across various offline and online channels? How would we know whether supervised classification, outlier detection, or graph clustering is the right approach for fraud detection? We can’t foresee what analysis we need or what techniques will work until we dive into and learn about the data. Thus, iterating on high-level intent statements tends to work better.

To sum up, if the executor doesn’t have sufficient context and experience, detailed and low-level requirements can help to provide guardrails and ensure success. On the other hand, if they see the big picture and have tackled similar problems before, it’s likely more effective to provide a high-level intent and let them research the problem and define the low-level requirements.

Conversely, if the situation is uncertain, or the profession (still) more art than science, requirements that are too low-level and specific can prevent the team from exercising their initiative and iterating quickly. Thus, we’re better off accepting the uncertainty, communicating the high-level intent, and empowering the team to assess, decide, and act.

Finally, while I’ve been discussing high-level intent and low-level requirements as binary concepts, they’re not. Adjust the level of detail to the situation. One way to help people grow is to start by giving them high-level intent so they can learn to break it down into smaller pieces, and only adding details for guidance as needed.

Thanks to [Alexey](https://twitter.com/Al_Grigor) for initial chats on this, and to Xinyi and [Swyx](https://twitter.com/swyx) for reading early drafts.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Mar 2022). Traversing High-Level Intent and Low-Level Requirements. eugeneyan.com. https://eugeneyan.com/writing/intent-vs-requirements/.

or

```
@article{yan2022requirement,
  title   = {Traversing High-Level Intent and Low-Level Requirements},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2022},
  month   = {Mar},
  url     = {https://eugeneyan.com/writing/intent-vs-requirements/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
