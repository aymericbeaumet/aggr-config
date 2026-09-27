---
title: How to Interview and Hire ML/AI Engineers
link: https://eugeneyan.com//writing/how-to-interview/
source: eugeneyan-com
published: 2024-07-07T00:00:00Z
updated: 2024-07-07T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- career
- leadership
- machinelearning
- 🔥
summary: What to interview for, how to structure the phone screen, interview loop, and debrief, and a few tips.
content: extracted
html: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.html
preview:
  file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.preview-6e962143a169.webp
  width: 256
  height: 134
  color: '#707a81'
images:
- source: https://eugeneyan.com/assets/og_image/whiteboard.jpg
  original:
    file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-1d3d25aa68e9.jpg
    width: 1200
    height: 630
  color: '#5a6565'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2024-07-07-how-to-interview-and-hire-ml-ai-engineers.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Hiring well is the highest leverage activity we can do for the mission and organization. And running effective interviews is key to hiring well. We can think of the interview process as a system: Given a candidate, assesses whether they are a good fit for the role and team. Thus, to hire well, the interview system should be reliable and valid, with minimal noise.

In this write-up, [Jason](https://jxnl.co) and I will share a few things we’ve learned about interviewing candidates for machine learning (ML) and AI roles. First, we’ll discuss *what* [technical](https://eugeneyan.com//writing/how-to-interview/#what-technical-skills-to-consider) and [non-technical](https://eugeneyan.com//writing/how-to-interview/#what-non-technical-abilities-to-look-out-for) qualities to assess. Then, we’ll share *how* to [calibrate phone screens](https://eugeneyan.com//writing/how-to-interview/#calibrating-the-phone-screen-for-loop-success), and run the [interview loop and debrief](https://eugeneyan.com//writing/how-to-interview/#running-the-interview-loop-and-debrief). Finally, we’ll wrap up with some tips for [interviewers](https://eugeneyan.com//writing/how-to-interview/#for-interviewers-candidates-are-rough-diamonds) and [hiring managers](https://eugeneyan.com//writing/how-to-interview/#for-hiring-managers-sell-on-mission--talent-density), as well as our opinionated take on some traits of a good hire. (And like all our writing online, opinions our own.)

## What technical skills to consider

**For most ML/AI roles, a basic proficiency in software engineering is expected.** Depending on the role, we’ve seen simple 30-60 minute coding exercises that ask a candidate to:

- Check if 2/3D arrays meet predefined criteria (e.g., validating robot’s simulated route through a warehouse), checking for edge cases, and writing unit tests
- Implement and start an inference endpoint, including input/output validation, logging, monitoring, and command to update endpoint state
- Build a pipeline to process data, first in batch and then adapting it for streaming

Successfully completing the coding exercise is only part of the interview. What’s more important is *how* the candidate solves it: Do they break down the problem logically, write clean, readable, maintainable code, consider edge cases, respond well to feedback, etc? There’ve been cases where a candidate completes the coding exercise but receives a “no hire” decision because *how* they solved it did not meet the bar.

**Next, data literacy is a crucial yet overlooked skill for ML/AI roles.** While it’s nebulous and hard to quantify, I think that at its core, data literacy means understanding and respecting the data, being proficient at data analysis, and having an intuition for when the data or analysis smells fishy.

> One may expect Google engineering to largely consist of implementing PhD level algorithms, and while that’s sometimes true, *much of a search or AI engineer’s job involves looking at examples, spotting patterns, hand labelling data, and other non-scalable, in-the-weeds analysis.* — [shreyans.org](https://shreyans.org/google)

To respect the data is to make an effort to understand it—firsthand—beyond assumptions about how it was collected and stored. For example, data-savvy engineers understand that [server-side and client-side logs will differ](https://eugeneyan.com/writing/testing-pipelines/#adding-new-data-or-logic--updating-pipeline-code). They’ll also look at rows of data, and check for missing values, outliers, and inconsistencies. They know how to clean and preprocess data to make it easier to use downstream, whether for training models or serving features. And they [monitor the data for contamination or drift over time](https://eugeneyan.com/writing/practical-guide-to-maintaining-machine-learning/#monitor-training-and-serving-data-for-contamination).

Proficiency in data analysis goes beyond writing SQL or computing statistics. When joining tables, are we accidentally duplicating or dropping rows by incorrectly assuming that a join key is unique and complete? When computing statistics, do we only focus on aggregate stats like mean and median and thus miss crucial patterns and subgroups (see [Simpson’s paradox](https://en.wikipedia.org/wiki/Simpson%27s_paradox))? Do our data visualizations guide the audience toward accurate interpretations? Ultimately, the goal isn’t analysis per se but better decisions and actions.

Perhaps more important is the intuition (read: skepticism) to question an analysis that seems off. This may require domain knowledge to know when the analysis contradicts our intuition and we need to dig deeper. More specifically to ML, if we look at a precision-recall curve and see precision plunging or recall spiking at a certain threshold, do we have the curiosity and drive to figure out the root cause and fix it? In general, acting on good data intuition leads to healthier data and thus better performing ML systems.

> When the data and the anecdotes disagree, the anecdotes are usually right. It’s usually not that the data is being miscollected. It’s usually that you’re not measuring the right thing. — Jeff Bezos

We can assess for data literacy by asking follow-up questions such as:

- How did you wrangle the data? What issues did you face and how did you fix them?
- What was a misleading summary statistic? Which were more useful than others?
- What was an insightful data viz you created and why? What was the least helpful?

These questions let the candidate share about mistakes they’ve made and lessons learned. This also reveals the candidate’s self-awareness, intellectual honesty, and the humility to share their past errors. Also see Jason’s take on [what data literacy does NOT look like](https://jxnl.co/writing/2024/06/02/10-ways-to-be-data-illiterate-and-how-to-avoid-them/).

**It also helps for ML/AI engineers to be comfortable with the output of opaque models.** Sometimes, when software engineers first start working with ML models, they expect a level of control and predictability similar to databases or conventional APIs. But to their surprise, ML models are completely different beasts. They don’t have perfect accuracy, their predictions may change when the model is retrained on new data, they lack clear interpretability on how they arrive at outputs, and for large language models (LLMs), the output is stochastic where the same input can lead to different outputs.

While most engineers eventually adapt to these quirks, some never get fully comfortable with the inherent uncertainty and opacity of ML. And that’s okay. They’ll probably be happier in roles that don’t involve directly building or interacting with ML models.

Folks who’re comfortable with ML understand we can’t have full control or interpretability over most models, and that all models will reflect biases from their training data. They also know to build validators and policies to align the system’s (instead of the model’s) behavior with the needs of users and the business. We can learn about this by asking:

- What unexpected/biased output have you seen? How did you address it, if needed?
- What guardrails/policies do you have around the model to align it with the user?
- If you spot bias emerging from the model, how would you mitigate it?

**Finally, having a basic understanding of evals is key for anyone building ML-powered products.** Fundamentally, machine learning is an empirical discipline. Even if an ML/AI engineer isn’t directly training models, they have a responsibility to evaluate the model they’re using whether it’s a simple decision tree, a recommender system, or an off-the-shelf LLM API. We can ask about a candidate’s experience with evals by asking:

- How did you measure model performance over time, as it was retrained or updated?
- When model performance breached predefined thresholds, how did you respond?
- How did you collect the initial evaluation data and build the eval harness?

To understand more about running evals for LLM-powered products, you might find these useful: (i) [Your AI Product Needs Evals](https://hamel.dev/blog/posts/evals/), (ii) [Task-Specific LLM Evals that Do & Don’t Work](https://eugeneyan.com/writing/evals/), (iii) [Evaluating the n levels of RAG](https://jxnl.co/writing/2024/02/28/levels-of-complexity-rag-applications/), (iv) [Data Flywheels for LLM Applications (Evaluation)](https://www.sh-reya.com/blog/ai-engineering-flywheel/#1-evaluation-defining-success-metrics)

For the increasing number of [AI engineer](https://www.latent.space/p/ai-engineer) roles that apply pretrained models and APIs to build products, the above is usually sufficient. Nonetheless, for roles closer to research or applied science, we typically also assess for science breadth, depth, and application.

**Science breadth interviews assess a candidate’s familiarity with various ML domains.** They’re usually based on what the team needs. I tend to start with the basics of supervised and unsupervised learning (to build the candidate’s confidence) before progressing to more specialized areas such as recommender systems, language modeling, and evaluation / benchmarking. Other teams might prioritize forecasting, operations optimization, computer vision, or multi-modal models.

While interviewing for breadth, we shouldn’t expect expertise in every area—that would be unrealistic, especially compared to the interviewer who has likely conducted the interview dozens, if not hundreds of times. Instead, the goal is to understand the candidate’s breadth of knowledge and where the gaps are. You can make a point to communicate this upfront, letting candidates know that you’ll continue probing till you reach the boundaries of their knowledge, and encourage them to call out when they’re unfamiliar with a topic so the interview can focus the time on their strengths.

In contrast, **science depth interviews invite the candidate to showcase their expertise on a project of their choice.** The goal is to understand how rigorously they’ve thought about and executed on a problem. What were alternative designs or techniques considered, or what connections were drawn to adjacent fields? How did they navigate constraints and make difficult trade-offs? Can they discuss nitty-gritty details and implementation challenges? And how they they measure impact on customers and the business? Candidates who have depth in past work can likely bring similar rigor and thoughtfulness to future work.

I’ve found depth interviews to be the hardest to conduct, especially when the candidate’s expertise is very different from my own. Candidates could be working on anything from optimizing robot navigation in a warehouse to inserting virtual ad posters into movies and livestreams. In these cases, here are questions to collect data points on the fundamentals:

- How did you evaluate and perform error analysis on the model or system?
- What were some constraints faced, and what hard trade-offs did you have to make?
- How did you implement safeguards and policies around the model or system?
- What was the outcome, and looking back, what would you have done differently?

**Science application interviews challenge a candidate to apply their skills and knowledge to a practical problem**, typically one directly relevant to the hiring team’s work. There’s usually no “right” solution. Instead, the focus is on the candidate’s thought process while they solve the problem, as well as the various perspectives they consider, such as science, engineering, product, and business. Often, the candidate’s questions are more insightful (on their ability) than the eventual solution proposed.

For example, we might have a question on designing a recommendation system for [frequently changing items (e.g., ads)](https://eugeneyan.com/writing/bandits/). Most candidates explore issues like cold-start and sparse data, the need to explore-exploit, optimization metrics (e.g., impressions, clicks, conversions), and balancing between organic engagement and revenue.

Read more about one candidate’s experience on science interviews [here](https://medium.com/@ajinkya.pahinkar98/my-amazon-applied-science-interview-journey-f3eb1ed748a6).

## What non-technical abilities to look out for

Beyond interviewing for technical skills, it helps to also consider the non-technical aspects of a candidate’s experience, especially for senior+ roles. Some dimensions to consider include ambiguity, influence, complexity, and execution (AICE).

**Ambiguity is how nebulous the problem was when the candidate started working on it.** Sometimes, the problem is clearly scoped with predefined success criteria, allowing them to focus on implementation. In other situations, the candidate may have to define the problem and measures of success. Also, does the problem have known solutions and best practices from industry? Or is it previously unsolved with no prior art? The level of guidance is also relevant—did the candidate operate with close supervision, or largely on their own? Finally, some problems are more narrow and focused on the technical, and thus likely less ambiguous, while others span across the business and organization.

**Influence captures the scope of how the candidate collaborates and drives impact through others.** A candidate’s work may primarily involve influence on their immediate team, multiple teams, multiple lines of business, or even the field and community. It also helps to understand their mechanisms of influence, such as roadmap/design reviews, office hours, advising on a project, or being a mentor. Their influence may also extend beyond technical strategy into shaping product and business decisions. Candidates who are a force multiplier elevate the output of those around them, not just their own direct contribution.

**Complexity refers to the intricacy of the problem space.** (Not to be confused with the complexity of the solution! Ideally, we’d solve complex problems with simple, elegant solutions.) Some problems can be isolated to a specific component or system, while others require work across multiple interdependent systems with competing constraints and trade-offs. We want to understand the level of complexity the candidate has worked on and is used to, while getting a sense of the effectiveness of their solutions.

**Execution is the candidate’s ability to deliver within limited resources and timelines.** We don’t only consider solutions or products that did well on an A/B test—after all, the cost of innovation is the occasional failure and we want to know how fast the candidate can fail, learn, and iterate. Also, some focus on tactical, short-term problems, while others deliver strategic solutions that solve multiple endemic problems. As part of execution, some even pioneer new approaches that are adopted across teams or industry. The scope and scale of effort required (e.g., number of teams) is also a consideration.

As we listen to a candidate describe their experiences, we can pick up cues that indicate the level of AICE they operate on. For example, a candidate working on a project to boost subscription rates through signup bonuses may have to frequently collaborate with finance, suggesting cross-team influence. But if their manager handled most of the negotiation and debates, their influence may be more limited than it appears.

Depending on the role and stage of the company, we may prioritize different AICE traits. An early-stage startup may prioritize ambiguity and execution while a mature tech company might need more influence and complexity management.

Finally, it’s worth reflecting which technical and non-technical qualities can be coached, and which can only be hired. IMHO, most technical skills can be coached, while traits such as ownership, resourcefulness, and grit are likely hired. Being clear on the qualities to coach vs. hire will help with identifying candidate gaps that are truly dealbreakers.

So far, we’ve focused on *what* to interview for in ML/AI roles. Next, we’ll discuss *how* to calibrate the phone screen, run an interview loop, and conduct a debrief. (An interview loop typically consists of multiple interviewers who assess for various aspects.) Then, we’ll wrap up with some tips for interviewers and hiring managers.

## Calibrating the phone screen for loop success

Before investing in an interview loop, most hiring managers conduct a phone screen. The goal is to select candidates who’ll have a high likelihood of success in the interview loop. Interview loops are costly, often requiring a game of Tetris across multiple interviewer schedules. A typical loop might involve five interviews of an hour each, plus a 30-minute pre-brief and debrief involving all interviewers. That’s 10 hours of collective time invested in a single candidate, excluding the time taken to prepare and write feedback.

Given how costly a loop is for candidates and interviewers, a rule of thumb from [Working Backwards](https://www.goodreads.com/book/show/53138083-working-backwards) is for the phone screen to be sufficiently selective. The hiring manager should advance candidates that they believe will perform well in the interview loop. Of course, we may need to recalibrate the phone screen if the pipeline is sparse or if the number of candidates making it to the loop is extremely low. But in general, a rigorous phone screen reduces the number of borderline candidates who end up struggling in the interview loop.

> After this detailed phone screen, the hiring manager decides whether they are inclined to hire the candidate based on the data they’ve collected so far. If so, then the candidate will be invited for an in-house interview. **Sometimes, the hiring manager isn’t sure about a candidate but still invites them to go through the interview loop, hoping that this will assist in the hiring decision. This is a mistake.** In most cases, the questionable candidate will not get the job, and a lot of time will have been wasted in the process. **The hiring manager should not bring the candidate in for the time-consuming and expensive interview loop unless they are inclined to hire them after the phone interview.** — [Working Backwards](https://www.goodreads.com/book/show/53138083-working-backwards)

## Running the interview loop and debrief

**It helps to have a pre-brief before the interview loop**, especially if some interviewers are unfamiliar with role requirements or come from different teams. This is where the hiring manager can align the loop on what’s needed for the role, and what to look out for.

For example, the loop would need to know if the role focuses more on financial analysis and A/B tests, or training and serving recsys models, or RAG and building LLM-powered user experiences. It’s also helpful to share about the level of ambiguity, influence, complexity, and execution expected for the role, or if there are any [company values](https://www.scarletink.com/p/interviewing-at-amazon-leadership-principles) that interviewers should collect data points on.

**For the actual interviews, we’ve found the STAR format to be effective at gathering relevant data points** based on past behavior and work:

- Situation/Task: The context and scope of the problem, including the stakes and potential impact if not acted on. Also, the candidates assigned responsibility.
- Action: The role the candidate played, the challenges faced, and skills and knowledge applied. Also, were they the key driver, and how did they contribute?
- Results: The quantifiable outcomes such as customer impact, new capabilities, revenue, or cost savings. Also, how did they decide on, and measure these metrics?

As we guide the candidates through their STAR response, there will be opportunities to dive deeper with follow-up questions around both the technical and non-technical aspects of their work and experience:

- Situation/Task: Data points on the ambiguity and complexity of the initial problem, and how the candidate defined the problem statement and success criteria.
- Action: Data points on how the candidate influenced others, navigated constraints and trade-offs, and the challenges and technical details of their solution.
- Result: Data points on execution with limited resources and under a timeline, as well as results and how they measured them.

As we gather data points, it’s important to clarify the candidate’s direct contributions vs. those of the broader team. Some candidates may overstate their role while others may be overly humble. Asking targeted questions can help tease this out: “What was the hardest decision made on the project and who made that call?” or “Can you share an example of when you disagreed with the team?” Strong candidates will give nuanced responses that acknowledge the role others played while being clear about their contribution.

Watch out for less seasoned candidates rambling or going off-tangent, especially if the question is uncomfortable. As the interviewer, our job is to steer the interview and gather the data points needed for the hiring manager to make a good decision. Thus, if the response is headed in an unproductive direction, we can politely interject and redirect: “In the interest of time, let’s move on to …” or “I’d love to dig into your role in that project …” Don’t be afraid to firmly but gently guide the candidate through the interview.

Sometimes, a candidate’s first STAR example falls flat and doesn’t provide the data points needed for the role and level. If so, consider pivoting and asking the candidate to share another example. For instance, if we have a 15-minute block for a question and can tell that the response doesn’t demonstrate what’s needed by the 10-minute mark, make the call to cut it short and ask for another example. Sometimes, this can elicit a stronger example with solid data points across both technical and non-technical aspects, even within five minutes. Having different questions that assess for the same data point helps.

**After the interview, interviewers should spend 15 - 30 minutes independently writing feedback as well as making an initial “hire” or “no hire” vote.** Writing feedback independently prevents groupthink and biased individual assessments. The feedback should be anchored on data points for the specific technical and non-technical aspects the interviewer was responsible for.

**Once all interviewer feedback has been logged, all interviewers get together for the debrief.** (In some companies, this may just involve the hiring manager, recruiter, and a senior team member.) All interviewers spend some time to review each others’ written feedback and potentially update their initial vote based on the additional data points they now have. Sometimes, if there are conflicting data points from multiple interviewers, discussion is needed to understand the candidate better.

Ultimately, the debrief must conclude with a decision on whether to extend an offer. The vote doesn’t necessarily need to be unanimous, but there shouldn’t be strong objections against hiring a candidate. If an interviewer feels strongly enough to say something like “I’ll quit if we hire this person,” that’s a red flag to discuss—what did the interviewer observe that others might have missed? Overall, while consensus is ideal, the hiring manager owns the final decision.

## For interviewers: Candidates are rough diamonds

Even for the most qualified candidates, the interview process can be daunting. Thus, making the candidate feel comfortable goes a long way in helping them perform their best. It can be as simple as offering a quick bathroom break before starting your interview, or a word of encouragement that they’re nearly done with the loop.

It also helps to approach each interview with the mindset that the candidate is a diamond in the rough. Thus, our job is to uncover their stengths and potential. This doesn’t mean to lower the bar, but to give them the benefit of the doubt and work together to discover their abilities. You can even make this explicit in the interview intro. Let them know up front that your goal is to help the hiring manager gather as many data points as possible. This puts the interview in a collaborative mood and eases some of the interview anxiety.

Finally, think of each candidate as a customer of your business or a user of your product. Regardless of whether we eventually hire the candidate, strive to make the interview a valuable experience that helps them grow. While they may not be the right fit now, they could be a strong hire down the road or refer other great talent your way.

## For hiring managers: Sell on mission & talent density

While compensation is important, it’s rarely the primary factor for in-demand candidates who likely have multiple competitive offers. Instead, consider attracting top talent via the organization’s mission, the talent density of the team, and the manager who will develop them. Thus, sell candidates on the ambitious problem they’ll solve, the opportunity to work with exceptional team members, and their personal growth.

That said, set the right expectations. Don’t sugarcoat the role. If there’s unglamorous work that needs to be done, such as setting up data infra and pipelines, be upfront about it. Overselling the position will likely lead to disappointment and regrettable attrition. It’s better to not hire a candidate who’s not the right fit than to bring them on with incorrect expectations and have them leave within a few months.

Finally, be prepared for hiring to take time, especially for more senior roles. In general, it takes a few months to hire a junior/mid-level IC while senior roles can take up to a year. Also, the best candidates are usually not looking out—they’re too busy learning, building, and having fun in their work. Nonetheless, it doesn’t hurt to start building relationships wherever they graze, be it meetups, conferences, hackathons, or simply online.

Finally, we’ll conclude with an opinionated take on the traits that strong hires have: hunger, judgment, and empathy.

**Hunger** shows up as having bias for action, being able to learn fast, and the grit to push through challenges. In startups, this helps candidates adapt quickly as the startup iterates toward product-market fit. In larger organizations, this helps new hires push through technical and organizational challenges that might discourage others.

IMHO hunger is more hired than coached. While budget and time constraints can simulate urgency, it’s a poor substitute for the intrinsic drive that gets shit done.

**Judgement** is the hard-won intuition to distinguish between what will work and what’s a wild goose chase or dead end. Judgment helps one make pragmatic decisions, think several steps ahead, and be decisive in the face of uncertainty. In the context of ML, this could be making a design decision to focus on techniques that work well with sparse data for cold-start recommendations, or deciding not to focus on chat for LLM-powered products.

While judgment can be cultivated, it takes time. The impact of design decisions may only become apparent after 12 - 18 months. Thus, for roles that need good judgment, it may be easier to hire for a track record of good decisions than to develop it on the job.

**Empathy** is the genuine interest in customers, the organization, and the team. This may show up as passion for the mission/product and how it helps customers. Empathy makes it easier to build trust, empower others, and foster productive relationships. People with empathy are also better communicators because they [“seek first to understand, then to be understood”](https://www.franklincovey.com/the-7-habits/habit-5/). This is especially crucial for senior roles who may have to influence without authority, navigate delicate organization challenges, and moderate hotly debated topics.

Empathy is likely innate and thus mostly hired. While there are techniques to demonstrate empathy, they’re superficial if there isn’t a sincere interest in others and their challenges.

Okay, that’s all we had. Thanks for reading this long write-up! If you’re actively looking for an ML/AI role, check out [Jason’s job board](https://jobs.applied-llms.org/). Also, what other interview practices have served you well? Please comment below or [DM me](https://x.com/eugeneyan)!

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Jul 2024). How to Interview and Hire ML/AI Engineers. eugeneyan.com. https://eugeneyan.com/writing/how-to-interview/.

or

```
@article{yan2024default,
  title   = {How to Interview and Hire ML/AI Engineers},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2024},
  month   = {Jul},
  url     = {https://eugeneyan.com/writing/how-to-interview/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
