---
title: 39 Lessons on Building ML Systems, Scaling, Execution, and More
link: https://eugeneyan.com//writing/conf-lessons/
source: eugeneyan-com
published: 2024-11-03T00:00:00Z
updated: 2024-11-03T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- leadership
- machinelearning
- production
summary: ML systems, production & scaling, execution & collaboration, building for users, conference etiquette.
content: extracted
html: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.html
preview:
  file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.preview-a33fdc519e1c.webp
  width: 256
  height: 134
  color: '#819ca8'
images:
- source: https://eugeneyan.com/assets/og_image/conf-lessons.png
  original:
    file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-bb50b7edd0ba.png
    width: 1200
    height: 630
  variants:
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-78f0a9d301e2.webp
    width: 320
    height: 168
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-fdf6a0fee758.webp
    width: 640
    height: 336
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-f80f26782703.webp
    width: 960
    height: 504
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-5cdedc31c31d.webp
    width: 1200
    height: 630
  color: '#88a7b1'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2024-11-03-39-lessons-on-building-ml-systems-scaling-execution-and-more.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Industry ML conferences are intense. There’s so much information, learning, and context switching between talks and posters and hallway conversations that leaves you exhausted each day. Thus, whenever there’s a break, taking a few minutes to reflect and take notes helps to solidify the learning. Here are my notes from ML conferences in 2024.

(I also had the opportunity to share my work at a few of these conferences. Here are the slides for my talks at the [Netflix PRS Workshop](https://eugeneyan.com/speaking/netflix-prs/) and the [AI Engineer World’s Fair](https://eugeneyan.com/speaking/aie-2024/). Unfortunately, my oral presentation at the [Amazon ML Conference](https://x.com/eugeneyalt/status/1846276463584043352) is internal only.)

## Building effective machine learning systems

**1\. The real world is messy.** To build systems that work, we need to define reward functions (that define labels), operationalize the world as data, find levers that make a difference, and measure what matters. Beware of those who tell you ML is a walk in the park.

**2\. Getting the reward function right is half the battle won.** Short-term rewards don’t convey the full picture. Long-term rewards are delayed and tricky to attribute. Proxy rewards are finicky and may not correlate well with business metrics. And most rewards are sparse, with feedback few and far between. Invest early in reward function engineering.

**3\. You don’t always need machine learning.** Heuristics and SQL queries are valuable baselines. Start simple and see if the juice is worth the squeeze. Remember [the first rule of machine learning: Start *without* machine learning](https://eugeneyan.com/writing/first-rule-of-ml/).

**4\. Machine learning involves trade-offs.** Recall vs. precision. Explore vs. exploit. Relevance vs. diversity vs. [serendipity](https://eugeneyan.com/writing/serendipity-and-accuracy-in-recommender-systems/). Accuracy vs. speed vs. cost. The challenge is figuring out the right balance for your user experience.

**5\. Set realistic expectations.** Most problems have a ceiling on what can be achieved, especially those that involve predicting the behavior of unpredictable humans (e.g., search, recommendations, fraud). It may not make sense to aim beyond the ceiling, unless you’re doing core research to push the boundaries on what’s possible.

**6\. Don’t overlook the dimension of time.** User preferences change. Inventory gets drawn down. Content relevance shifts. Daily, seasonally, over months and years. If time is a key factor in your problem (e.g., recommendations, search, news feed), ensure your systems and models are time-aware.

**7\. Evals are a differentiator and moat.** Over the past two years, teams with solid evals have been able to continuously ship reliable, delightful experiences. No one regrets investing in a robust evaluation framework.

**8\. Design with the data flywheel in mind.** Data alone is not the competitive advantage; it is the data flywheel. How will you gather user feedback to enhance your model, system, or product and fuel a better customer experience? (See this [Tesla self-driving example from Andrej Karpathy](https://x.com/karpathy/status/1599852921541128194).) Whoever turns the data flywheel faster, wins.

**9\. Brandolini’s law: The amount of energy needed to refute bullshit is an order of magnitude larger than needed to produce it.** The same applies to using LLMs. Generating slop content is easy relative to evaluating and guardrailing the defects. But the latter is how we earn—and keep—customer trust. Invest your efforts accordingly.

**10\. We probably won’t have one model to rule them all.** Instead, each product will likely have several models supporting it. Maybe a bigger model orchestrating several smaller models. This way, each smaller model can give their undivided attention to their task.

**11\. Altman’s law: When a new model drops, are you nervous, or are you super pumped? Consciously design your product so that you’re in the latter camp.** Be prepared to swap the model anytime and reap the benefits of constantly improving models. [The model isn’t your product—the system around it is.](https://applied-llms.org/#the-model-isnt-the-product-the-system-around-it-is)

**12\. Build with an eye toward the future.** Flexibility beats specialization in the long run. Remember [The Bitter Lesson](https://www.cs.utexas.edu/~eunsol/courses/data/bitter_lesson.pdf). An LLM that’s a bit worse now will likely outperform a custom finetune later, especially as LLMs get cheaper ([two orders of magnitude in 18 months!](https://applied-llms.org/#the-high-level-trend-of-low-cost-cognition)), faster, and more capable. Other examples include using a generative/extractive approach vs. named entity recognition/classification, and explore-exploit/reinforcement learning vs. supervised learning.

## Production and scaling

**13\. Don’t underestimate the effort it takes to go from demo to production.** “There’s a large class of problems that are easy to imagine and build demos for, but extremely hard to make products out of. For example, self-driving. It’s easy to demo a car self-driving around a block but making it into a product takes a decade.” — [Andrej Karpathy](https://x.com/eugeneyan/status/1672692174704766976)

**14\. Scale makes everything harder.** While we may not need to face it head-on from day one, we should be ready for it. Especially if we have to support multiple existing markets, languages, devices, user experiences, etc.

**15\. Each 10x-ing of scale/traffic will uncover new bugs and issues.** Prepare early for the operational challenges that come with scale and just keeping the lights on.

**16\. Depending on your product, LLMs are cheap or expensive.** “Even the most expensive LLMs are not that expensive for B2B scale; even the cheapest LLMs are not that cheap for consumer scale.” — [Will Larson](https://lethain.com/mental-model-for-how-to-use-llms-in-products/). If you’re mostly B2B or running internal workflows, using LLM APIs could be viable long-term. Address scale when you need to, not prematurely.

**17\. \[Corollary\] The economics aren’t in the way; it’s trust, reliability, security, etc.** Costs will keep dropping. But faithfulness, hallucinations, prompt injections, etc. are still open problems. They’re the bottleneck to wider integration of LLMs in user-facing experiences.

**18\. Get the fundamentals right.** Robust data pipelines, instrumentation, evaluation, guardrails, experimentation, monitoring, metrics. These core capabilities are essential—and reusable—across most products. Investing in them early will save you the development and operational cost associated with each new product.

**19\. Start simple, always.** Complexity will creep in as we patch edge cases and extend an existing system for new requirements and features. Thus, a system that starts complex will inevitably buckle under its own weight or become an operational nightmare.

**20\. Not everything needs to be in real-time.** If your user experience allows it, consider batch or asynchronous workflows to simplify the system design and reduce operational costs. When designing a new system, ask yourself: Can this be async?

**21\. Design for fast failure detection and recovery.** No system is perfect; stuff will slip through. Yet, there’s an optimal investment balance between prevention and cure. Monitoring, alerts, rollbacks, Andon cords—these will go a long way.

## Execution and collaboration

**22\. Execution is everything.** Execution is navigating from where we are today (e.g., legacy systems, low velocity, high opex) to our long-term vision. It’s everything from idea to design to implementation to launch to measurement to operations and everything else in between. Executing well is the difference between success and failure.

**23\. Your rate of iteration = your rate of innovation.** Focus on experimenting fast, getting rapid feedback, and updating or pivoting quickly. Velocity matters.

**24\. \[Counterpoint\] Breakthroughs will take longer than you think.** Nine women can’t give birth to a baby in a month, and we can’t rush game-changing research and innovation. Start early, be patient, and keep pushing.

**25\. Not every challenge is technical.** Some challenges are about alignment, culture, and organizations. Working with people is hard. When you zoom out, it turns out that tech is often the easier part.

**26\. It takes a village to raise a machine learning system.** Infra, engineering, data, ML, design, product, business, and more. No role is more important than the others. 1 + 1 =3.

**27\. Genius can come from anywhere.** Not just the ivory towers of research. Some of the best ideas come from people who deeply understand the customer, regardless of whether their role is technical, creative, or operational. Don’t overlook the wisdom of the crowd.

**28\. You don’t have to go it alone.** Whatever you’re working on, there will be others tackling similar problems. Reach out to them to learn and/or collaborate, either within your organization or on the internet. Together, we’re stronger.

**29\. People *want* to help.** Especially if you’ve done your homework, bring data, and keep an open mind. Everyone, and I mean everyone, at these conferences is incredibly kind and generous. I experienced this warmth firsthand interacting with some of the leaders in the field. The strongest are also the kindest.

**30\. What seems obvious to you can be game-changing for others.** Expertise is a ladder. Wherever you’re at, there are people a few rungs below (and a few rungs above) who are eager to learn from you. Thus, share what you know, even if you think it’s too basic and not worth sharing. It could help someone with a challenge they’re facing.

**31\. Tune out the noise; focus on building.** Don’t get nerd-sniped by the daily barrage of shiny new techniques. Most don’t pan out anyway. For real alpha on what actually works, have hallway conversations and DMs with the practitioners who are quietly crushing it.

## Building for users

**32\. Always work backwards from the customer.** Why are we solving this problem? How does it help the customer? What are the tangible and intangible benefits? Unless you’re a researcher, don’t do science for the sake of science. Focus on the customer.

**33\. To create winning products, dream big *and* sweat the small stuff.** Aim for the stars but don’t forget the details. The best leaders have vision and get their hands dirty with the details. You can’t just do one or the other; you need to do both.

**34\. Humans are insatiable.** LLMs will automate some tasks. LLMs will simplify others. But there will always be new problems to solve, and more things to build. AI won’t steal your job—it’ll just make it more interesting.

## Speaking at and attending conferences

**35\. Speaking at industry conferences is a tightrope act.** It’s a delicate balance between sharing valuable insights and protecting the secret sauce. You’ll see this when speakers rely on public data, reference published work and public tech blogs, stick to high-level ideas, and choose their words carefully. (This is also why many of the best practitioners decline to speak.) Nonetheless, everyone understands.

**36\. GOATs are GOATs because they work hard.** I saw many diligently taking notes, pulling up the papers being presented to review the tables and charts, and following up on points they didn’t understand. This is why they’re the best.

**37\. Conference insiders keep it friendly during public Q&A.** The hard questions and debates are reserved for hallway conversations, happy hours, and over dinner.

**38\. No one really cares how good your slides look.** Fancy layouts? Dark mode? Not a priority. Never has been, from what I can tell. Some of the top speakers just use the default font on white backgrounds. Substance over style, always.

**39\. Get enough geeks together and magic happens.** The energy, the inspiration, the ideas. It’s infectious. I leave every conference on a high, fired up by thoughts on what to explore next, motivated by the conversations, and ready to build. Highly recommend attending a good conference at least once a year.

## Similar reading

- [What We’ve Learned From A Year of Building with LLMs](https://applied-llms.org/)
- [The Metagame of Applying Machine Learning](https://eugeneyan.com/writing/machine-learning-metagame/)
- [Stories and Advice from Machine Learning Practitioners](https://applyingml.com/mentors/)

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Nov 2024). 39 Lessons on Building ML Systems, Scaling, Execution, and More. eugeneyan.com. https://eugeneyan.com/writing/conf-lessons/.

or

```
@article{yan2024conflessons,
  title   = {39 Lessons on Building ML Systems, Scaling, Execution, and More},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2024},
  month   = {Nov},
  url     = {https://eugeneyan.com/writing/conf-lessons/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
