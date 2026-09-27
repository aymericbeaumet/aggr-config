---
title: Mechanisms for Effective Technical Teams
link: https://eugeneyan.com//writing/mechanisms-for-teams/
source: eugeneyan-com
published: 2023-02-05T00:00:00Z
updated: 2023-02-05T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- leadership
- mechanism
summary: End of week debrief, weekly business review, monthly learning sessions, and quarter review.
content: extracted
html: 2023-02-05-mechanisms-for-effective-technical-teams.html
preview:
  file: 2023-02-05-mechanisms-for-effective-technical-teams.preview-c4065c345033.webp
  width: 256
  height: 134
  color: '#604249'
images:
- source: https://eugeneyan.com/assets/og_image/scrum-for-data-science.jpg
  original:
    file: 2023-02-05-mechanisms-for-effective-technical-teams.image-dafcc7af74fd.jpg
    width: 1200
    height: 630
  color: '#0b172a'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-02-05-mechanisms-for-effective-technical-teams.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-02-05-mechanisms-for-effective-technical-teams.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-02-05-mechanisms-for-effective-technical-teams.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-02-05-mechanisms-for-effective-technical-teams.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-02-05-mechanisms-for-effective-technical-teams.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-02-05-mechanisms-for-effective-technical-teams.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Previously, we discussed [mechanisms for machine learning projects](https://eugeneyan.com/writing/mechanisms-for-projects/). Here, we’ll discuss mechanisms for technical teams and teams of teams. The goal is to increase productivity and team effectiveness. A team is a group of people organized around a common function such as engineering, business intelligence, or machine learning. Another definition is [Amazon’s 2-pizza teams](https://aws.amazon.com/executive-insights/content/amazon-two-pizza-team/#Taking_Ownership) which are organized around a product or service.

## End of Week Debrief (team)

The end of week debrief (EOWD) provides an opportunity for the team to come together and share the week’s progress. They’re typically informal and last an hour. The only requirement is to *not* spend time preparing beforehand. It’s meant to be a lightweight mechanism for the team to share information, gather feedback, and ask for help.

At EOWDs, team members might present their work through jupyter notebooks, pull requests, or document outlines. Whether someone is sharing a new dataset they found or built, optimizing a data pipeline, or trying a new machine learning technique, it’s a chance for everyone to learn. It’s also a forum to ask for feedback: Are there any blindspots or design flaws? Any suggestions for improvement?

To get the most out of it, I usually spend a few minutes before an EOWD to think about what input I want from the team. With a team of 4 - 6 people, each person only has 10 minutes to share. Thus, it’s important to be concise. Note that EOWDS are not meant to replace [methodology reviews](https://eugeneyan.com/writing/mechanisms-for-projects/#methodology-review). Nonetheless, they work well as mini-reviews that let us iteratively receive feedback and course-correct before the full methodology review.

One benefit of EOWDs is that they give everyone greater context on what everyone else is working on. This is especially helpful for new members to get up to speed quickly. Also, frequent sharing fosters knowledge transfer and stronger collaboration within the team.

## Monthly Learning Sessions (team)

This is for sharing knowledge that goes beyond the ~10 minutes at EOWD. The intent is to scale the expertise of an individual to the team, or from a team to the greater organization. Depending on the topic shared, the audience may expand from the team to include other teams in the organization.

The sessions are typically an hour long. The speaker shares for 30 - 45 minutes and the remaining time is dedicated to Q&A. We usually record these sessions so people who missed it can watch the recording. This also lets people revisit the details in the session.

There’s no fixed format for this mechanism. If the topic is on machine learning techniques or evaluation methodology, it may be beneficial to have slides and visuals. On the other hand, if we’re teaching how to use tools like SageMaker or Airflow, it might be effective to have hands-on demos, look at code, and interact with the UI.

Learning sessions can cover a wide range of topics. Some I’ve attended include:

- Machine learning: If we’re sharing about bandits, we might discuss concepts such as reward, context, feedback, explore-exploit, and [examples from industry](https://eugeneyan.com/writing/bandits/#industry-examples-of-bandits-for-recsys).
- Tools: If we’re teaching about Airflow, we might start by taking a look at a production DAG via the UI before walking through the DAG’s code. Then, we’ll share how our team has set up alarms and ticketing for task failures or delays.
- Skills: If we’re sharing how to navigate CloudWatch, we might first show how to plot metrics and run queries before diving into a case study of how we used CloudWatch to triage the root cause of an outage.

## Quarterly Review (team of teams)

It’s easy for tech teams to get caught up in the day-to-day details of executing and lose sight of the broader business and product priorities. This is where a quarterly review is valuable. It’s an opportunity to take a step back, reflect, and align on the medium-to-long-term goals. The intent is to provide the broader team with the right context so they can make good decisions in their work.

At Netflix, they embody this concept with their culture of being “[highly aligned, loosely coupled](https://jobs.netflix.com/culture#highly-aligned-loosely-coupled)”. In their words: “We spend lots of time debating and writing down strategy and context, and then trust each other to execute on tactics without prior approval.” Quarterly reviews help teams act on this principle.

We typically kick off a review by reflecting on the previous quarter. What did we achieve? This can be measured via features released, operational improvements, or experiments ran. In addition, what did we learn? What went well and didn’t go so well? This is a chance to celebrate the wins and lessons gained.

Then, we focus on next quarter’s priorities. What are the continued priorities that we’ll carry on this quarter? What are the new priorities that were either previously planned or a net new initiative? If it’s a new initiative, why is it important? Finally, what will we stop doing and why? This could be due to negative results, poor customer response, or temporary deprioritization.

During the review, it’s important to share the “Why” behind each priority so the team understands the context. This empowers the team to make informed decisions on “What” to do or not do, letting them execute more effectively and quickly.

The quarterly review is also a great time to build team camaraderie. Whether in-person or virtual, we can set aside time for games and a meal. Some great online games I’ve tried include [Fibbage: Enough About You](https://www.jackboxgames.com/introducing-fibbage-enough-about-you/), [Drawasaurus](https://www.drawasaurus.org), and [Codewords](http://codewordsgame.com). The goal is to have fun, connect, and come away with a shared understanding of next quarter’s priorities.

## Weekly Business Review (team of teams)

The goal of the weekly business review (WBR) is to provide a comprehensive overview of the business or team. At the crux of it are metrics: input metrics and output metrics.

It’s important to understand how the inputs (aka leading indicators) affect the outputs (lagging indicators) of our systems. With this knowledge, we can adjust the controllable inputs to achieve the desired outputs. While output metrics are crucial, they can’t be directly manipulated over the long term. On the other hand, input metrics measure things that—if done right—bring the desired outputs.

In [Working Backwards](https://www.goodreads.com/en/book/show/53138083), we learn that Amazon uses input metrics such as adding items to the catalog, lowering costs to lower prices, and positioning inventory to facilitate faster delivery to customers. Output metrics include orders, revenue, and profit. To learn more about WBRs, I suggest Commoncog’s summary on [controllable input metrics](https://commoncog.com/working-backwards/#controllable-input-metrics). Also, the book “Working Backwards”, authored by two long-time Amazon insiders, has an entire chapter dedicated to metrics.

Let’s take a look at some input and output metrics for a hypothetical machine learning team in an e-commerce startup. For sales, output metrics include conversion, revenue, basket size, and the sales funnel (e.g., clicks, add-to-cart, wishlist). Input metrics include the number of experiments conducted on search and recommendations, coverage of recommendation widgets across the home page, detail page, checkout page, etc., and the delay between customer behavior and updated recommendations.

Another example is delivery forecasts. The output metric could be the proportion of deliveries that arrive before, on, or after the forecasted delivery date. Input metrics could include data integration with third-party logistics providers, the timeliness, granularity, and quality of logistics data, and efforts to improve forecasting accuracy.

Though these mechanisms increase team productivity, they do come with a cost. Some, like the EOWD that only requires an hour a week, are a small investment. Others, like the WBR, require more time and effort. The ROI from these mechanisms will vary depending on the stage of the team. Thus, be deliberate about picking mechanisms to adopt.

Have you tried similar mechanisms and how did they work out? What other mechanisms does your team have? Please share!

**Thanks** to Yang Xinyi for reading drafts of this.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Feb 2023). Mechanisms for Effective Technical Teams. eugeneyan.com. https://eugeneyan.com/writing/mechanisms-for-teams/.

or

```
@article{yan2023teams,
  title   = {Mechanisms for Effective Technical Teams},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Feb},
  url     = {https://eugeneyan.com/writing/mechanisms-for-teams/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
