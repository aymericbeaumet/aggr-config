---
title: The Revenge of the Data Scientist
link: https://hamel.dev/blog/posts/revenge/
source: hamel-dev
published: 2026-03-26T07:00:00Z
updated: 2026-03-26T07:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Hamel Husain
labels:
- evals
- llms
summary: 'Is the heyday of the data scientist over? The Harvard Business Review once called it “The Sexiest Job of the 21st Century.”1 In tech, data scientist roles were often among the best paid.2 The job also demanded an unusual mix of skills: Data Scientist (n.): Person who is better at statistics than any software engineer and better at software engineering than any statistician. — JosH100 (@josh_wills) May 3, 2012 In addition to creating a high-barrier to entry, these skills enabled data scientists to build predicitive models, measure casuality and find patterns in data. Of these, predicitive modeling paid best. Companies later peeled that work off into a new title: Machine Learning Engineer (“MLE”).3 For years, shipping AI meant keeping data scientists and MLEs on the critical path. With LLMs, this stopped being the default. Foundation-model APIs now allow teams to integrate AI independently. Getting cut out of the loop rattled data scientists and MLEs I know. If the company no longer needs you to ship AI, it is fair to wonder whether the job still has the same upside. The harsher story people tell themselves: unless you are pretraining at a foundation-model lab, you are not where the action is. In my opinion, training models was never most of the job. The bulk of the work is setting up experiments to test how well the AI generalizes to unseen data, debugging stochastic systems, and designing good metrics. Calling an LLM over an API does not make this work go away. I recently gave a talk titled “The Revenge of the Data Scientist” at PyAI Conf to make that case with examples rather than assertion alone. Below is an annotated version of that presentation. The Harness Is Data Science OpenAI published a blog post on harness engineering that I recommend reading. They describe how Codex worked on a software project for months, autonomously, with agents developing code bounded by a harness of tests and specifications. One detail in that blog post’s description of the harness is easy to miss. It includes an observability stack: logs, metrics, and traces exposed to the agent so it can tell when it is going off track. In addition to tests and specifications, there are metrics. That is a key component of the system. Andrej Karpathy’s auto-research project shows the same pattern: models iteratively optimize against a validation loss metric. Same idea, different harness. What I want to convince you of is that a large portion of the harness is data science. Let’s take a step back and take stock of where we are. Years ago, practitioners spent hours examining data, checking label alignment, and designing metrics. Today, we build on “vibes,” ask the model if it did a good job, and grab off-the-shelf metric libraries without looking at the data. This shows up most around retrieval and evals. Without a data background, engineers fear what they don’t understand. They claim “RAG is dead” or “evals are dead,” yet build systems that depend on those concepts. The rest of this post walks through five eval pitfalls I see repeatedly, and what a data scientist would do differently in each case. Generic Metrics The first pitfall is generic metrics. It is tempting to reach for an eval framework and use its metrics off the shelf. The problem: you have no idea what is actually broken. Most teams put up a dashboard with helpfulness scores, coherence scores, hallucination scores. These sound reasonable. They are also generic enough to be useless for diagnosing your application’s failures. A data scientist would not adopt metrics off the shelf. They would explore the data, explore the traces, ask “what is actually breaking here?”, and figure out the highest-value thing to start measuring. There are infinite things to measure. You have to form hypotheses and iterate. The best medicine for this pitfall is looking at the data. What does “looking at the data” mean in practice? It means reading traces. Code your own custom trace viewer so you can remove friction and customize the display for your domain’s quirks. Take notes on problems you find. Do error analysis: categorize failures, figure out what to prioritize, decide what to work on. When you look at your data, you end up driving toward application-specific metrics. Off-the-shelf similarity metrics like ROUGE or BLEU rarely fit LLM outputs. The metrics that matter look like “Calendar Scheduling Failure” or “Failure to Escalate To Human.” If there is one thing to take away from this post: look at the data. How to look at it is a separate question and takes practice. This is the higest ROI activity you can engage in and is often skipped. Unverified Judges The second pitfall is unverified judges. A lot of teams use an LLM as a judge to figure out whether their AI is working. Most of the time, nobody has a good answer to “how do you trust the judge?” The default: ask an LLM to rate outputs on a scale and use the numbers. A data scientist would treat the judge like a classifier. You have a black box giving you a prediction. How do you trust it? Get human labels, partition the data into train/dev/test, and measure whether the classifier is trustworthy. Source few-shot examples from your training set. Hill-climb your judge’s prompt against a dev set. Keep a test set aside to confirm you haven’t overfit. If you have done machine learning before, this is boring. But people are not doing it. Verifying classifiers has become a lost art in modern AI. Treat your judge like a classifier in how you report results, too. Everywhere I go I see accuracy reported. If a failure mode occurs 5% of the time, accuracy hides the system’s true performance. Use precision and recall. Bad Experimental Design The third pitfall is experimental design. There are many dimensions to this. Here are two that come up most. The first is constructing test sets. Most teams generate synthetic data by prompting an LLM: “Give me 50 test queries.” They get generic, unrepresentative data. A data scientist would look at real production data first, use hypotheses to determine which dimensions matter, then generate synthetic examples along those dimensions. Ground synthetic data in real logs or traces. Figure out what dimensions to vary. Inject edge cases. Base the synthetic data off real data. The second is metric design. Teams bundle entire rubrics into a single LLM call and default to 1-5 Likert scales. A data scientist would reduce complexity, make each metric actionable, and tie it to a business outcome. Replace subjective scales with binary pass/fail on scoped criteria. Likert scales hide ambiguity and kick the can down the road on hard decisions about system performance. Bad Data and Labels The fourth pitfall is bad data and labels. Data scientists don’t trust the data. They don’t trust the labels. They don’t trust anything. They are skeptical by training. AI engineers at large have not built this muscle yet. When it comes to labeling, most teams make it someone else’s problem. Labeling seems unglamorous, so it gets delegated to the dev team or outsourced. A data scientist would insist that domain experts label the data, stay skeptical of the labels, and look at the data. But labeling matters for a deeper reason than label quality. It is impossible to know what you want unless you look at the data. There is a concept called “criteria drift,” validated in a paper by Shreya Shankar and colleagues: users need criteria to grade outputs, but grading outputs helps users define their criteria. People don’t know what they want until they see the LLM’s outputs. The labeling process itself surfaces what matters. Data scientists champion this: get domain experts and product managers in front of raw data, not summary scores. Automating Too Much The fifth pitfall is automating too much. All of this is human work. The temptation is to automate it away. LLMs can help wire things up, write the plumbing, generate boilerplate for evaluations. They cannot look at the data for you, for the exact reason we just discussed: you don’t know what you want until you see the outputs. Other Pitfalls We did not have time to cover every pitfall. Here is a speed run through the rest. Misusing similarity scores. Asking the judge vague questions like “is it helpful?” Making annotators read raw JSON. Reporting uncalibrated scores without confidence intervals. Data drift, overfitting, not sampling correctly, dashboards that don’t make sense. The Mapping If you zoom out, every pitfall above has the same root cause: missing a data science fundamental. Reading traces and categorizing failures is Exploratory Data Analysis. Validating an LLM judge against human labels is Model Evaluation. Building representative test sets from production data is Experimental Design. Getting domain experts to label outputs is Data Collection. Monitoring whether your product works in production is Production ML. None of this is new! This is a Python conference, so: Python remains the best toolset for looking at your data and dealing with data. I built an evals skills plugin that goes into more depth. Point it at your eval pipeline and it will tell you what you are doing wrong (or try its best to). Always look at the data. If you enjoyed the memes in this talk, there are many more on my website. If you want to go deeper on any of these topics, the slides and video are below. Thanks to Shreya Shankar and Bryan Bischof for many conversations that shaped this talk. Video & Slides Link to the slides Footnotes https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century↩︎ https://www.forbes.com/sites/louiscolumbus/2018/01/29/data-scientist-is-the-best-job-in-america-according-glassdoors-2018-rankings/↩︎ https://www.mckinsey.com/about-us/new-at-mckinsey-blog/ai-reinvents-tech-talent-opportunities↩︎'
content: extracted
html: 2026-03-26-the-revenge-of-the-data-scientist.html
preview:
  file: 2026-03-26-the-revenge-of-the-data-scientist.preview-97e60c0ce058.webp
  width: 256
  height: 146
  color: '#342e2c'
images:
- source: https://hamel.dev/blog/posts/revenge/images/ABL.jpeg
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-c46d55c88f37.png
    width: 1344
    height: 768
  color: '#292824'
- source: https://hamel.dev/blog/posts/revenge/images/slide_1.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-688b6a0e9055.png
    width: 2000
    height: 1125
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-9fd7570c228f.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-66078a563dd0.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-1dddf6df02ff.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-193722b254d7.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-f70c2c68006f.webp
    width: 2000
    height: 1125
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_2.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-8faa3004b95b.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-1b15389f12c8.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-7c2059bf4d5b.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b3625c9e9f31.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b945717ded11.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-31e34eca377e.webp
    width: 1500
    height: 844
  color: '#010101'
- source: https://hamel.dev/blog/posts/revenge/images/slide_3.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-f05fe027ea36.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-beb7d88bb45f.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-da71a29784df.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-e62d25a472fc.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-635ed6ca49cb.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-7643a1058c00.webp
    width: 1500
    height: 844
  color: '#121219'
- source: https://hamel.dev/blog/posts/revenge/images/slide_4.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-8fdba4691832.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-af0d8f16c3bb.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-f81d07034446.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-4bc0b8498e50.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-fff316a94a6d.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b34c093d981a.webp
    width: 1500
    height: 844
  color: '#13131a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_5.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-084daa29a747.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-a9e9355663f9.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-16f172a89c38.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-7173d7cea2e0.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-3c8d6d34e369.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_6.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-5d55f7acc666.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-fb616e8cf7dd.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-ad7f7f9f2f26.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-be31eeb039bd.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-aafc2947c009.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-3077b34da54c.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_7.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-b9334ef3bf5e.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-e1f790c98684.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-ac0856331eb6.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-77ddda84d3cd.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-37fdb0045a23.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-ba3417d13d35.webp
    width: 1500
    height: 844
  color: '#121219'
- source: https://hamel.dev/blog/posts/revenge/images/slide_9.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-ba8e78e319f4.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-bc9a68481fa0.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-96d471ffd28e.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-33ce4f684a23.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-34355e65a499.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-af4b770224a4.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_12.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-5090b4c10d63.png
    width: 1500
    height: 844
  color: '#121218'
- source: https://hamel.dev/blog/posts/revenge/images/slide_13.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-40891d7a8464.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-314d77375fb8.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-c683570cf2a3.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-131a17accd4a.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-f1e6a856f304.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0e9be2139320.webp
    width: 1500
    height: 844
  color: '#fcfcfc'
- source: https://hamel.dev/blog/posts/revenge/images/slide_14.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-2a85a31978fc.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-6d579db82be9.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b60dcc7c5189.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-56d1019ab7fc.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-5221e15ca6cd.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-e121d25b7c02.webp
    width: 1500
    height: 844
  color: '#121219'
- source: https://hamel.dev/blog/posts/revenge/images/slide_15.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-8270b4ac48d5.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-aee5f6ae3f4e.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-6ba4a8a51ca7.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-5fbb55c879fc.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-4341752ae72f.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-c3013fa2d602.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_18.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-ec09ff5af58e.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0095059449b8.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-12657482b266.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-ce25b329f07b.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0d67b9375d59.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-69e3ed505d0f.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_19.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-0a1c10a798b0.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-049f5d480f37.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-c7e8a89afa1d.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-1b73488059c5.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-70017d43b9cc.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-fd25c63f1ee5.webp
    width: 1500
    height: 844
  color: '#121219'
- source: https://hamel.dev/blog/posts/revenge/images/slide_20.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-b01eaaf3f0ad.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-2f7a52d87417.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-79f90efa981a.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-d826762b7062.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-42cbca269ca4.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-f77d583afd18.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_23.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-4ffa21485cc6.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-55f438c2ba4a.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-cda269966f2b.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-a72cb5a9dc0e.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-af89aa6eea09.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-7984f6545d3d.webp
    width: 1500
    height: 844
  color: '#fcfcfc'
- source: https://hamel.dev/blog/posts/revenge/images/slide_26.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-6ea0d04d39f0.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-f0c3ce0a3381.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-d925fd45ba59.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0da3fda53732.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-6fda49d991dc.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-2bd11b46ec9a.webp
    width: 1500
    height: 844
  color: '#fbfbfb'
- source: https://hamel.dev/blog/posts/revenge/images/slide_27.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-53aff110c4d2.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-1c53da17bd0d.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-a3915a670f12.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-a2f2488b61d5.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-a2ea87d61761.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-c941e1648259.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_30.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-11c6346d56f2.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-9785fbb7ff6d.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-5458528ddca2.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-83dc64f7fe14.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b10a32feb690.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-56472014abed.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_31.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-0f3723cef54f.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-960f0aa28164.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0177c810da34.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-9a946ea4f879.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-3aa609206c6d.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-fdf31503e411.webp
    width: 1500
    height: 844
  color: '#121219'
- source: https://hamel.dev/blog/posts/revenge/images/slide_32.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-6358418e6285.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-8214297fead2.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-7a2ac4f42b6b.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b6934b585167.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-4386185829d9.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-2a2f1b3fcd42.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_33.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-3df9d6c42582.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-96afc8e7a586.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-6b4322a95c9c.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-2ff2fc57e300.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-4f5a45d25872.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0a27c65acaf6.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_34.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-8db76cb3dcd0.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-dd0c24b03d7e.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-e19dbdacac6d.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-38dd7194e4ee.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0dd5c812ff3a.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-e6e3ba07c772.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_35.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-5261f02e5c1d.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-a379e27fb5df.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0e9ed7ef96a8.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-23ab3c4dc4f2.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_36.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-d831a638560d.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-bf20951ed5bc.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b02099661ba8.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-ffd4f9859615.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-7529c9ce454e.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_37.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-2ab328c1acc2.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-eb29082ce92b.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-bd852139a907.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-e1fd3d7e84e7.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-0ffc76e35b95.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-65e33e18da5f.webp
    width: 1500
    height: 844
  color: '#12121a'
- source: https://hamel.dev/blog/posts/revenge/images/slide_38.png
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-5e54e958873c.png
    width: 1500
    height: 844
  variants:
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-b568fb0ff0eb.webp
    width: 320
    height: 180
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-f830e7cadbe6.webp
    width: 640
    height: 360
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-db85443edb94.webp
    width: 960
    height: 540
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-d34488b2fbd3.webp
    width: 1280
    height: 720
  - file: 2026-03-26-the-revenge-of-the-data-scientist.image-f3f6fd73fc75.webp
    width: 1500
    height: 844
  color: '#121219'
- source: https://i.ytimg.com/vi/lA4MfpgF91Y/hqdefault.jpg
  original:
    file: 2026-03-26-the-revenge-of-the-data-scientist.image-f3d077399a0f.jpg
    width: 480
    height: 360
  color: '#030303'
---

Is the heyday of the data scientist over? The Harvard Business Review once called it “The Sexiest Job of the 21st Century.”[^1] In tech, data scientist roles were often among the best paid.[^2] The job also demanded an unusual mix of skills:

> Data Scientist (n.): Person who is better at statistics than any software engineer and better at software engineering than any statistician.
>
> — JosH100 (@josh\_wills) [May 3, 2012](https://twitter.com/josh_wills/status/198093512149958656?ref_src=twsrc%5Etfw)

In addition to creating a high-barrier to entry, these skills enabled data scientists to build predicitive models, measure casuality and find patterns in data. Of these, predicitive modeling paid best. Companies later peeled that work off into a new title: Machine Learning Engineer (“MLE”).[^3]

For years, shipping AI meant keeping data scientists and MLEs on the critical path. With LLMs, this stopped being the default. Foundation-model APIs now allow teams to integrate AI independently.

Getting cut out of the loop rattled data scientists and MLEs I know. If the company no longer needs you to ship AI, it is fair to wonder whether the job still has the same upside. The harsher story people tell themselves: unless you are pretraining at a foundation-model lab, you are not where the action is.

In my opinion, training models was never most of the job. The bulk of the work is setting up experiments to test how well the AI generalizes to unseen data, debugging stochastic systems, and designing good metrics. Calling an LLM over an API does not make this work go away.

I recently gave a talk titled “The Revenge of the Data Scientist” at [PyAI Conf](https://pyai.events/) to make that case with examples rather than assertion alone. Below is an annotated version of that presentation.

![](https://hamel.dev/blog/posts/revenge/images/slide_1.png)

## The Harness Is Data Science

![](https://hamel.dev/blog/posts/revenge/images/slide_2.png)

OpenAI published a blog post on [harness engineering](https://openai.com/index/harness-engineering/) that I recommend reading. They describe how Codex worked on a software project for months, autonomously, with agents developing code bounded by a harness of tests and specifications.

![](https://hamel.dev/blog/posts/revenge/images/slide_3.png)

One detail in that blog post’s description of the harness is easy to miss. It includes an observability stack: logs, metrics, and traces exposed to the agent so it can tell when it is going off track. In addition to tests and specifications, there are metrics. That is a key component of the system.

![](https://hamel.dev/blog/posts/revenge/images/slide_4.png)

Andrej Karpathy’s [auto-research project](https://github.com/karpathy/autoresearch) shows the same pattern: models iteratively optimize against a validation loss metric. Same idea, different harness.

![](https://hamel.dev/blog/posts/revenge/images/slide_5.png)

What I want to convince you of is that a large portion of the harness is data science.

Let’s take a step back and take stock of where we are.

![](https://hamel.dev/blog/posts/revenge/images/slide_6.png)

Years ago, practitioners spent hours examining data, checking label alignment, and designing metrics. Today, we build on “vibes,” ask the model if it did a good job, and grab off-the-shelf metric libraries without looking at the data.

![](https://hamel.dev/blog/posts/revenge/images/slide_7.png)

This shows up most around retrieval and evals. Without a data background, engineers fear what they don’t understand. They claim “RAG is dead” or “evals are dead,” yet build systems that depend on those concepts.

The rest of this post walks through five eval pitfalls I see repeatedly, and what a data scientist would do differently in each case.

* * *

## Generic Metrics

The first pitfall is generic metrics.

![](https://hamel.dev/blog/posts/revenge/images/slide_9.png)

It is tempting to reach for an eval framework and use its metrics off the shelf. The problem: you have no idea what is actually broken. Most teams put up a dashboard with helpfulness scores, coherence scores, hallucination scores. These sound reasonable. They are also generic enough to be useless for diagnosing your application’s failures.

A data scientist would not adopt metrics off the shelf. They would explore the data, explore the traces, ask “what is actually breaking here?”, and figure out the highest-value thing to start measuring. There are infinite things to measure. You have to form hypotheses and iterate.

The best medicine for this pitfall is looking at the data.

![](https://hamel.dev/blog/posts/revenge/images/slide_12.png)

What does “looking at the data” mean in practice? It means reading traces. Code your own custom trace viewer so you can remove friction and customize the display for your domain’s quirks. Take notes on problems you find. Do error analysis: categorize failures, figure out what to prioritize, decide what to work on.

![](https://hamel.dev/blog/posts/revenge/images/slide_13.png)

When you look at your data, you end up driving toward application-specific metrics. Off-the-shelf similarity metrics like ROUGE or BLEU rarely fit LLM outputs. The metrics that matter look like “Calendar Scheduling Failure” or “Failure to Escalate To Human.”

![](https://hamel.dev/blog/posts/revenge/images/slide_14.png)

If there is one thing to take away from this post: look at the data. How to look at it is a separate question and takes practice. This is the higest ROI activity you can engage in and is often skipped.

* * *

## Unverified Judges

The second pitfall is unverified judges. A lot of teams use an LLM as a judge to figure out whether their AI is working. Most of the time, nobody has a good answer to “how do you trust the judge?”

![](https://hamel.dev/blog/posts/revenge/images/slide_15.png)

The default: ask an LLM to rate outputs on a scale and use the numbers. A data scientist would treat the judge like a classifier. You have a black box giving you a prediction. How do you trust it? Get human labels, partition the data into train/dev/test, and measure whether the classifier is trustworthy.

![](https://hamel.dev/blog/posts/revenge/images/slide_18.png)

Source few-shot examples from your training set. Hill-climb your judge’s prompt against a dev set. Keep a test set aside to confirm you haven’t overfit. If you have done machine learning before, this is boring. But people are not doing it. Verifying classifiers has become a lost art in modern AI.

![](https://hamel.dev/blog/posts/revenge/images/slide_19.png)

Treat your judge like a classifier in how you report results, too. Everywhere I go I see accuracy reported. If a failure mode occurs 5% of the time, accuracy hides the system’s true performance. Use precision and recall.

* * *

## Bad Experimental Design

The third pitfall is experimental design. There are many dimensions to this. Here are two that come up most.

![](https://hamel.dev/blog/posts/revenge/images/slide_20.png)

The first is constructing test sets. Most teams generate synthetic data by prompting an LLM: “Give me 50 test queries.” They get generic, unrepresentative data. A data scientist would look at real production data first, use hypotheses to determine which dimensions matter, then generate synthetic examples along those dimensions.

![](https://hamel.dev/blog/posts/revenge/images/slide_23.png)

Ground synthetic data in real logs or traces. Figure out what dimensions to vary. Inject edge cases. Base the synthetic data off real data.

![](https://hamel.dev/blog/posts/revenge/images/slide_26.png)

The second is metric design. Teams bundle entire rubrics into a single LLM call and default to 1-5 Likert scales. A data scientist would reduce complexity, make each metric actionable, and tie it to a business outcome. Replace subjective scales with binary pass/fail on scoped criteria. Likert scales hide ambiguity and kick the can down the road on hard decisions about system performance.

* * *

## Bad Data and Labels

The fourth pitfall is bad data and labels. Data scientists don’t trust the data. They don’t trust the labels. They don’t trust anything. They are skeptical by training. AI engineers at large have not built this muscle yet.

![](https://hamel.dev/blog/posts/revenge/images/slide_27.png)

When it comes to labeling, most teams make it someone else’s problem. Labeling seems unglamorous, so it gets delegated to the dev team or outsourced. A data scientist would insist that domain experts label the data, stay skeptical of the labels, and look at the data.

![](https://hamel.dev/blog/posts/revenge/images/slide_30.png)

But labeling matters for a deeper reason than label quality. It is impossible to know what you want unless you look at the data. There is a concept called “criteria drift,” validated in a [paper by Shreya Shankar and colleagues](https://arxiv.org/abs/2404.12272): users need criteria to grade outputs, but grading outputs helps users define their criteria. People don’t know what they want until they see the LLM’s outputs. The labeling process itself surfaces what matters.

![](https://hamel.dev/blog/posts/revenge/images/slide_31.png)

Data scientists champion this: get domain experts and product managers in front of raw data, not summary scores.

* * *

## Automating Too Much

The fifth pitfall is automating too much. All of this is human work. The temptation is to automate it away.

![](https://hamel.dev/blog/posts/revenge/images/slide_32.png)

![](https://hamel.dev/blog/posts/revenge/images/slide_33.png)

LLMs can help wire things up, write the plumbing, generate boilerplate for evaluations. They cannot look at the data for you, for the exact reason we just discussed: you don’t know what you want until you see the outputs.

* * *

## Other Pitfalls

We did not have time to cover every pitfall. Here is a speed run through the rest.

![](https://hamel.dev/blog/posts/revenge/images/slide_34.png)

Misusing similarity scores. Asking the judge vague questions like “is it helpful?” Making annotators read raw JSON. Reporting uncalibrated scores without confidence intervals. Data drift, overfitting, not sampling correctly, dashboards that don’t make sense.

* * *

## The Mapping

If you zoom out, every pitfall above has the same root cause: missing a data science fundamental.

![](https://hamel.dev/blog/posts/revenge/images/slide_35.png)

Reading traces and categorizing failures is Exploratory Data Analysis. Validating an LLM judge against human labels is Model Evaluation. Building representative test sets from production data is Experimental Design. Getting domain experts to label outputs is Data Collection. Monitoring whether your product works in production is Production ML. None of this is new!

![](https://hamel.dev/blog/posts/revenge/images/slide_36.png)

This is a Python conference, so: Python remains the best toolset for looking at your data and dealing with data.

![](https://hamel.dev/blog/posts/revenge/images/slide_37.png)

I built an [evals skills plugin](https://github.com/ai-evals-course/evals-skills) that goes into more depth. Point it at your eval pipeline and it will tell you what you are doing wrong (or try its best to).

![](https://hamel.dev/blog/posts/revenge/images/slide_38.png)

Always look at the data.

If you enjoyed the memes in this talk, there are [many more on my website](https://hamel.dev/notes/llm/evals/memes/index.html#meme-images).

If you want to go deeper on any of these topics, the [slides](https://hamel.dev/blog/posts/revenge/#video-slides) and [video](https://hamel.dev/blog/posts/revenge/#video-slides) are below.

*Thanks to [Shreya Shankar](https://www.sh-reya.com/) and [Bryan Bischof](https://x.com/BEBischof) for many conversations that shaped this talk.*

* * *

## Video & Slides

[www.youtube.com](https://www.youtube.com/watch?v=lA4MfpgF91Y)

[Link to the slides](https://docs.google.com/presentation/d/1Q7F7cr5PthTmsl6RQCofyBBlH23fh4FlaO_5tbWxbTs/edit?usp=sharing)

[^1]: https://hbr.org/2012/10/data-scientist-the-sexiest-job-of-the-21st-century

[^2]: https://www.forbes.com/sites/louiscolumbus/2018/01/29/data-scientist-is-the-best-job-in-america-according-glassdoors-2018-rankings/

[^3]: https://www.mckinsey.com/about-us/new-at-mckinsey-blog/ai-reinvents-tech-talent-opportunities
