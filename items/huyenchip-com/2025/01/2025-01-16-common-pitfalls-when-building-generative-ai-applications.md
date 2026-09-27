---
title: Common pitfalls when building generative AI applications
link: https://huyenchip.com//2025/01/16/ai-engineering-pitfalls.html
source: huyenchip-com
published: 2025-01-16T00:00:00Z
updated: 2025-01-16T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'As we’re still in the early days of building applications with foundation models, it’s normal to make mistakes. This is a quick note with examples of some of the most common pitfalls that I’ve seen, both from public case studies and from my personal experience. Because these pitfalls are common, if you’ve worked on any AI product, you’ve probably seen them before. 1. Use generative AI when you don''t need generative AI Every time there’s a new technology, I can hear the collective sigh of senior engineers everywhere: “Not everything is a nail.” Generative AI isn’t an exception — its seemingly limitless capabilities only exacerbate the tendency to use generative AI for everything. A team pitched me the idea of using generative AI to optimize energy consumption. They fed a household’s list of energy-intensive activities and hourly electricity prices into an LLM, then asked it to create a schedule to minimize energy costs. Their experiments showed that this could help reduce a household’s electricity bill by 30%. Free money. Why wouldn’t anyone want to use their app? I asked: “How does it compare to simply scheduling the most energy-intensive activities when electricity is cheapest? Say, doing your laundry and charging your car after 10pm?” They said they would try it later and let me know. They never followed up, but they abandoned this app soon after. I suspect that this greedy scheduling can be quite effective. Even if it’s not, there are other much cheaper and more reliable optimization solutions than generative AI, like linear programming. I’ve seen this scenario over and over again. A big company wants to use generative AI to detect anomalies in network traffic. Another wants to predict upcoming customer call volume. A hospital wants to detect whether a patient is malnourished (really not recommended). It can often be beneficial to explore a new approach to get a sense of what’s possible, as long as you’re aware that your goal isn’t to solve a problem but to test a solution. “We solve the problem” and “We use generative AI” are two very different headlines, and unfortunately, so many people would rather have the latter. 2. Confuse ''bad product'' with ''bad AI'' At the other end of the spectrum, many teams dismiss gen AI as a valid solution for their problems because they tried it out and their users hated it. However, other teams successfully used gen AI for similar use cases. I could only look into two of these teams. In both cases, the issue wasn’t with AI, but with product. Many people have told me that the technical aspects of their AI applications are straightforward. The hard part is user experience (UX). What should the product interface look like? How to seamlessly integrate the product into the user workflow? How to incorporate human-in-the-loop? UX has always been challenging, but it’s even more so with generative AI. While we know that generative AI is changing how we read, write, learn, teach, work, entertain, etc., we don’t quite know how yet. What will the future of reading/learning/working be like? Here are some simple examples to show how what users want can be counter-intuitive and need rigorous user study. My friend works on an application that summarizes meeting transcripts. Initially, her team focused on getting the right summary length. Would users prefer 3-sentence summaries or 5-sentence summaries? However, it turned out that their users didn’t care about the actual summary. They only wanted action items specific to them from each meeting. When LinkedIn developed a chatbot for skill fit assessment, they discovered that users didn’t want correct responses. Users wanted helpful responses. For example, if a user asks a bot whether they’re a fit for a job and the bot responds with: “You’re a terrible fit,” this response might be correct but not very helpful to the user. Users want tips on what the gaps are and what they can do to close the gaps. Intuit built a chatbot to help users answer tax questions. Initially, they got lukewarm feedback — users didn’t find the bot useful. After investigation, they found out that users actually hated typing. Facing a blank chatbot, users didn’t know what the bot could do and what to type. So, for each interaction, Intuit added a few suggested questions for users to click on. This reduced the friction for users to use the bot and gradually built users’ trust. The feedback from users then became much more positive. (Shared by Nhung Ho, VP of AI at Intuit, during our panel at Grace Hopper.) Because everyone uses the same models nowadays, the AI components of AI products are similar, and the differentiation is product. 3. Start too complex Examples of this pitfall: Use an agentic framework when direct API calls work. Agonize over what vector database to use when a simple term-based retrieval solution (that doesn’t require a vectordb) works. Insist on finetuning when prompting works. Use semantic caching. Given so many shiny new technologies, it’s tempting to jump straight into using them. However, incorporating external tools too early can cause 2 problems: Abstract away critical details, making it hard to understand and debug your system. Introduce unnecessary bugs. Tool developers can make mistakes. For example, I often find typos in default prompts when reviewing a framework’s codebase. If the framework you use updates its prompt without telling you, your application’s behaviors might change and you might not know why. Eventually, abstractions are good. But abstractions need to incorporate best practices and be tested overtime. As we’re still in the early days of AI engineering, best practices are still evolving, we should be more vigilant when adopting any abstraction. 4. Over-index on early success It took LinkedIn 1 month to achieve 80% of the experience they wanted, and an additional 4 months to surpass 95%. The initial success made them grossly underestimate how challenging it is to improve the product, especially around hallucinations. They found it discouraging how difficult it was to achieve each subsequent 1% gain. A startup that develops AI sales assistants for ecommerce told me that getting from 0 to 80% took as long as from 80% to 90%. The challenges they faced: Accuracy/latency tradeoff: more planning/self-correction = more nodes = higher latency Tool calling: hard for agents to differentiate similar tools Hard for tonal requests (e.g. "talk like a luxury brand concierge") in the system prompt to be perfectly obeyed Hard for the agent to completely understand customers’ intent Hard to create a specific set of unit tests because the combination of queries is basically infinite Thanks Jason Tjahjono for sharing this. In the paper UltraChat, Ding et al. (2023) shared that “the journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging.” This is perhaps one of the first painful lessons anyone who has built an AI product quickly learns. It’s easy to build a demo, but hard to build a product. Other than the issues of hallucinations, latency, latency/accuracy tradeoff, tool use, prompting, testing, … as mentioned, teams also run into issues, such as: Reliability from the API providers. A team told me that 10% of their API calls timed out. Or product’s behaviors change because the underlying model changes. Compliance, e.g. around AI output copyrights, data access/sharing, user privacy, security risks from retrieval/caching systems, and ambiguity around training data lineage. Safety, e.g. bad actors abusing your product, your product generates insensitive or offensive responses. When planning a product’s milestones and resources, make sure to take into account these potential roadblocks. A friend calls this “being cautiously optimistic”. However, remember that many cool demos don’t lead to wonderful products. 5. Forgo human evaluation To automatically evaluate AI applications, many teams opt for the AI-as-a-judge (also called LLM-as-a-judge) approach — using AI models to evaluate AI outputs. A common pitfall is forgoing human evaluation to rely entirely on AI judges. While AI judges can be very useful, they aren’t deterministic. The quality of a judge depends on the underlying judge’s model, the judge’s prompt, and the use case. If the AI judge isn’t developed properly, it can give misleading evaluations about your application’s performance. AI judges must be evaluated and iterated over time, just like all other AI applications. The teams with the best products I’ve seen all have human evaluation to supplement their automated evaluation. Every day, they have human experts evaluate a subset of their application’s outputs, which can be anywhere from 30 - 1000 examples. Daily manual evaluation serves 3 purposes: Correlate human judgments with AI judgments. If the score by human evaluators is decreasing but the score by AI judges is increasing, you might want to investigate your AI judges. Gain a better understanding of how users use your application, which can give you ideas to improve your application. Detect patterns and changes in users’ behaviors, using your knowledge of current events, that automated data exploration might miss. The reliability of human evaluation also depends on well-crafted annotation guidelines. These annotation guidelines can help improve the model’s instructions (if a human has a hard time following the instructions, the model will, too). It can also be reused to create finetuning data later if you choose to finetune. In every project I’ve worked on, staring at data for just 15 minutes usually gives me some insight that could save me hours of headaches. Greg Brockman tweeted: “Manual inspection of data has probably the highest value-to-prestige ratio of any activity in machine learning.” 6. Crowdsource use cases This is a mistake I saw in the early days when enterprises were in a frenzy to adopt generative AI. Unable to come up with a strategy for what use cases to focus on, many tech executives crowdsourced ideas from the whole company. “We hire smart people. Let them tell us what to do.” They then try to implement these ideas one by one. And that’s how we ended up with a million text-to-SQL models, a million Slack bots, and a billion code plugins. While it’s indeed a good idea to listen to the smart people that you hire, individuals might be biased toward the problems that immediately affect their day-to-day work instead of problems that might bring the highest returns on investment. Without an overall strategy that considers the big picture, it’s easy to get sidetracked into a series of small, low-impact applications and come to the wrong conclusion that gen AI has no ROI. Summary In short, here are the common AI engineering pitfalls: Use generative AI when you don’t need generative AI Gen AI isn’t a one-size-fits-all solution to all problems. Many problems don’t even need AI. Confuse ‘bad product’ with ‘bad AI’ For many AI product, AI is the easy part, product is the hard part. Start too complex While fancy new frameworks and finetuning can be useful for many projects, they shouldn’t be your first course of action. Over-index on early success Initial success can be misleading. Going from demo-ready to production-ready can take much longer than getting to the first demo. Forgo human evaluation AI judges should be validated and correlated with systematic human evaluation. Crowdsource use cases Have a big-picture strategy to maximize return on investment.'
content: extracted
html: 2025-01-16-common-pitfalls-when-building-generative-ai-applications.html
preview:
  file: 2025-01-16-common-pitfalls-when-building-generative-ai-applications.preview-fa5608ca1844.webp
  width: 256
  height: 94
  color: '#edeff6'
images:
- source: https://huyenchip.com/assets/pics/aie-pitfalls/aie-pitfalls.png
  original:
    file: 2025-01-16-common-pitfalls-when-building-generative-ai-applications.image-cd399b0b8020.png
    width: 868
    height: 318
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/aie-pitfalls/ai-judge.png
  original:
    file: 2025-01-16-common-pitfalls-when-building-generative-ai-applications.image-7d2382655e14.png
    width: 2350
    height: 700
  color: '#fcfcfc'
---

As we’re still in the early days of building applications with foundation models, it’s normal to make mistakes. This is a quick note with examples of some of the most common pitfalls that I’ve seen, both from public case studies and from my personal experience.

Because these pitfalls are common, if you’ve worked on any AI product, you’ve probably seen them before.

## 1\. Use generative AI when you don't need generative AI

Every time there’s a new technology, I can hear the collective sigh of senior engineers everywhere: “Not everything is a nail.” Generative AI isn’t an exception — its seemingly limitless capabilities only exacerbate the tendency to use generative AI for everything.

A team pitched me the idea of using generative AI to optimize energy consumption. They fed a household’s list of energy-intensive activities and hourly electricity prices into an LLM, then asked it to create a schedule to minimize energy costs. Their experiments showed that this could help reduce a household’s electricity bill by 30%. Free money. Why wouldn’t anyone want to use their app?

I asked: “How does it compare to simply scheduling the most energy-intensive activities when electricity is cheapest? Say, doing your laundry and charging your car after 10pm?”

They said they would try it later and let me know. They never followed up, but they abandoned this app soon after. I suspect that this greedy scheduling can be quite effective. Even if it’s not, there are other much cheaper and more reliable optimization solutions than generative AI, like linear programming.

I’ve seen this scenario over and over again. A big company wants to use generative AI to detect anomalies in network traffic. Another wants to predict upcoming customer call volume. A hospital wants to detect whether a patient is malnourished (really not recommended).

It can often be beneficial to explore a new approach to get a sense of what’s possible, as long as you’re aware that your goal isn’t to solve a problem but to test a solution. “We solve the problem” and “We use generative AI” are two very different headlines, and unfortunately, so many people would rather have the latter.

## 2\. Confuse 'bad product' with 'bad AI'

At the other end of the spectrum, many teams dismiss gen AI as a valid solution for their problems because they tried it out and their users hated it. However, other teams successfully used gen AI for similar use cases. I could only look into two of these teams. In both cases, the issue wasn’t with AI, but with product.

Many people have told me that the technical aspects of their AI applications are straightforward. The hard part is user experience (UX). What should the product interface look like? How to seamlessly integrate the product into the user workflow? How to incorporate human-in-the-loop?

UX has always been challenging, but it’s even more so with generative AI. While we know that generative AI is changing how we read, write, learn, teach, work, entertain, etc., we don’t quite know how yet. What will the future of reading/learning/working be like?

Here are some simple examples to show how what users want can be counter-intuitive and need rigorous user study.

1. My friend works on an application that summarizes meeting transcripts. Initially, her team focused on getting the right summary length. Would users prefer 3-sentence summaries or 5-sentence summaries?

   However, it turned out that their users didn’t care about the actual summary. They only wanted action items specific to them from each meeting.

2. When [LinkedIn](https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product?_l=en_US) developed a chatbot for skill fit assessment, they discovered that users didn’t want correct responses. Users wanted helpful responses.

   For example, if a user asks a bot whether they’re a fit for a job and the bot responds with: “You’re a terrible fit,” this response might be correct but not very helpful to the user. Users want tips on what the gaps are and what they can do to close the gaps.

3. Intuit built a chatbot to help users answer tax questions. Initially, they got lukewarm feedback — users didn’t find the bot useful. After investigation, they found out that users actually hated typing. Facing a blank chatbot, users didn’t know what the bot could do and what to type.

   So, for each interaction, Intuit added a few suggested questions for users to click on. This reduced the friction for users to use the bot and gradually built users’ trust. The feedback from users then became much more positive. \
    *(Shared by [Nhung Ho](https://www.linkedin.com/in/nhungho/), VP of AI at Intuit, during our panel at Grace Hopper.)*

Because everyone uses the same models nowadays, the AI components of AI products are similar, and the differentiation is product.

## 3\. Start too complex

Examples of this pitfall:

1. Use an agentic framework when direct API calls work.
2. Agonize over what vector database to use when a simple term-based retrieval solution (that doesn’t require a vectordb) works.
3. Insist on finetuning when prompting works.
4. Use semantic caching.

Given so many shiny new technologies, it’s tempting to jump straight into using them. However, incorporating external tools too early can cause 2 problems:

1. Abstract away critical details, making it hard to understand and debug your system.
2. Introduce unnecessary bugs.

Tool developers can make mistakes. For example, I often find typos in default prompts when reviewing a framework’s codebase. If the framework you use updates its prompt without telling you, your application’s behaviors might change and you might not know why.

Eventually, abstractions are good. But abstractions need to incorporate best practices and be tested overtime. As we’re still in the early days of AI engineering, best practices are still evolving, we should be more vigilant when adopting any abstraction.

## 4\. Over-index on early success

1. It took [LinkedIn](https://www.linkedin.com/blog/engineering/generative-ai/musings-on-building-a-generative-ai-product) *1 month to achieve 80% of the experience they wanted, and an additional 4 months to surpass 95%*. The initial success made them grossly underestimate how challenging it is to improve the product, especially around hallucinations. They found it discouraging how difficult it was to achieve each subsequent 1% gain.

2. A startup that develops AI sales assistants for ecommerce told me that *getting from 0 to 80% took as long as from 80% to 90%*. The challenges they faced:
   - Accuracy/latency tradeoff: more planning/self-correction = more nodes = higher latency
   - Tool calling: hard for agents to differentiate similar tools
   - Hard for tonal requests (e.g. `"talk like a luxury brand concierge"`) in the system prompt to be perfectly obeyed
   - Hard for the agent to completely understand customers’ intent
   - Hard to create a specific set of unit tests because the combination of queries is basically infinite

   *Thanks [Jason Tjahjono](https://www.linkedin.com/in/jasontjahjono/) for sharing this.*

3. In the paper UltraChat, [Ding et al. (2023)](https://arxiv.org/abs/2305.14233) shared that “*the journey from 0 to 60 is easy, whereas progressing from 60 to 100 becomes exceedingly challenging*.”

This is perhaps one of the first painful lessons anyone who has built an AI product quickly learns. It’s easy to build a demo, but hard to build a product. Other than the issues of hallucinations, latency, latency/accuracy tradeoff, tool use, prompting, testing, … as mentioned, teams also run into issues, such as:

1. **Reliability** from the API providers. A team told me that 10% of their API calls timed out. Or product’s behaviors change because the underlying model changes.
2. **Compliance**, e.g. around AI output copyrights, data access/sharing, user privacy, security risks from retrieval/caching systems, and ambiguity around training data lineage.
3. **Safety**, e.g. bad actors abusing your product, your product generates insensitive or offensive responses.

When planning a product’s milestones and resources, make sure to take into account these potential roadblocks. A friend calls this “being cautiously optimistic”. However, remember that many cool demos don’t lead to wonderful products.

## 5\. Forgo human evaluation

To automatically evaluate AI applications, many teams opt for the AI-as-a-judge (also called LLM-as-a-judge) approach — using AI models to evaluate AI outputs. A common pitfall is forgoing human evaluation to rely entirely on AI judges.

While AI judges can be very useful, they aren’t deterministic. The quality of a judge depends on the underlying judge’s model, the judge’s prompt, and the use case. If the AI judge isn’t developed properly, it can give misleading evaluations about your application’s performance. AI judges must be evaluated and iterated over time, just like all other AI applications.

![Agent pattern](https://huyenchip.com/assets/pics/aie-pitfalls/ai-judge.png)

The teams with the best products I’ve seen all have human evaluation to supplement their automated evaluation. Every day, they have human experts evaluate a subset of their application’s outputs, which can be anywhere from 30 - 1000 examples.

Daily manual evaluation serves 3 purposes:

1. Correlate human judgments with AI judgments. If the score by human evaluators is decreasing but the score by AI judges is increasing, you might want to investigate your AI judges.
2. Gain a better understanding of how users use your application, which can give you ideas to improve your application.
3. Detect patterns and changes in users’ behaviors, using your knowledge of current events, that automated data exploration might miss.

The reliability of human evaluation also depends on well-crafted annotation guidelines. These annotation guidelines can help improve the model’s instructions (if a human has a hard time following the instructions, the model will, too). It can also be reused to create finetuning data later if you choose to finetune.

In every project I’ve worked on, *staring at data for just 15 minutes usually gives me some insight that could save me hours of headaches*. [Greg Brockman](https://x.com/gdb/status/1622683988736479232) tweeted: “*Manual inspection of data has probably the highest value-to-prestige ratio of any activity in machine learning.*”

## 6\. Crowdsource use cases

This is a mistake I saw in the early days when enterprises were in a frenzy to adopt generative AI. Unable to come up with a strategy for what use cases to focus on, many tech executives crowdsourced ideas from the whole company. “We hire smart people. Let them tell us what to do.” They then try to implement these ideas one by one.

And that’s how we ended up with a million text-to-SQL models, a million Slack bots, and a billion code plugins.

While it’s indeed a good idea to listen to the smart people that you hire, individuals might be biased toward the problems that immediately affect their day-to-day work instead of problems that might bring the highest returns on investment. Without an overall strategy that considers the big picture, it’s easy to get sidetracked into a series of small, low-impact applications and come to the wrong conclusion that gen AI has no ROI.

## Summary

In short, here are the common AI engineering pitfalls:

1. **Use generative AI when you don’t need generative AI**\
    Gen AI isn’t a one-size-fits-all solution to all problems. Many problems don’t even need AI.

2. **Confuse ‘bad product’ with ‘bad AI’**\
    For many AI product, AI is the easy part, product is the hard part.

3. **Start too complex**\
    While fancy new frameworks and finetuning can be useful for many projects, they shouldn’t be your first course of action.

4. **Over-index on early success**\
    Initial success can be misleading. Going from demo-ready to production-ready can take much longer than getting to the first demo.

5. **Forgo human evaluation**\
    AI judges should be validated and correlated with systematic human evaluation.

6. **Crowdsource use cases**\
    Have a big-picture strategy to maximize return on investment.
