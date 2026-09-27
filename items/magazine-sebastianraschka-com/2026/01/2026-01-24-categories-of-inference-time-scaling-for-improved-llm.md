---
title: Categories of Inference-Time Scaling for Improved LLM Reasoning
link: https://magazine.sebastianraschka.com/p/categories-of-inference-time-scaling
source: magazine-sebastianraschka-com
published: 2026-01-24T11:23:18Z
updated: 2026-01-24T11:23:18Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Sebastian Raschka, PhD
summary: And an Overview of Recent Inference-Scaling Papers
content: extracted
html: 2026-01-24-categories-of-inference-time-scaling-for-improved-llm.html
preview:
  file: 2026-01-24-categories-of-inference-time-scaling-for-improved-llm.preview-b71e9711f6f1.webp
  width: 256
  height: 171
  color: '#efeeed'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/1f97789d-b769-45fe-b884-ba0f7941da9e_1846x1230.png
  original:
    file: 2026-01-24-categories-of-inference-time-scaling-for-improved-llm.image-bac8c92cdfd9.png
    width: 1846
    height: 1230
  color: '#fefdfd'
- source: https://substackcdn.com/image/fetch/$s_!IOSP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf9e2677-652a-4af1-9f57-dc0c253d2198_1448x1260.png
  original:
    file: 2026-01-24-categories-of-inference-time-scaling-for-improved-llm.image-c78d3b5fcbf5.jpg
    width: 140
    height: 140
  color: '#fdfdfc'
- source: https://substackcdn.com/image/fetch/$s_!oiA2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffff769a2-8324-4fbd-8659-4615e4711ce2_1600x900.png
  original:
    file: 2026-01-24-categories-of-inference-time-scaling-for-improved-llm.image-94fcc97f09c0.jpg
    width: 1456
    height: 819
  color: '#fdfdfd'
---

Inference scaling has become one of the most effective ways to improve answer quality and accuracy in deployed LLMs.

The idea is straightforward. If we are willing to spend a bit more compute, and more time at inference time (when we use the model to generate text), we can get the model to produce better answers.

Every major LLM provider relies on some flavor of inference-time scaling today. And the academic literature around these methods has grown a lot, too.

Back in March, I wrote an overview of the inference scaling landscape and summarized some of the early techniques.

[![The State of LLM Reasoning Model Inference](https://substackcdn.com/image/fetch/$s_!IOSP!,w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Faf9e2677-652a-4af1-9f57-dc0c253d2198_1448x1260.png)](https://magazine.sebastianraschka.com/p/state-of-llm-reasoning-and-inference-scaling)

In this article, I want to take that earlier discussion a step further, group the different approaches into clearer categories, and highlight the newest work that has appeared over the past few months.

As part of drafting a full book chapter on inference scaling for *[Build a Reasoning Model (From Scratch)](https://mng.bz/Nwr7)*, I ended up experimenting with many of the fundamental flavors of these methods myself. With hyperparameter tuning, this quickly turned into thousands of runs and a lot of thought and work to figure out which approaches should be covered in more detail in the chapter itself. (The chapter grew so much that I eventually split it into two, and both are now available in the early access program.)

PS: I am especially happy with how the chapter(s) turned out. It takes the base model from about 15 percent to around 52 percent accuracy, which makes it one of the most rewarding pieces of the book so far.

What follows here is a collection of ideas, notes, and papers that did not quite fit into the final chapter narrative but are still worth sharing.

I also plan to add more code implementations to the [bonus materials on GitHub](https://github.com/rasbt/reasoning-from-scratch) over time.

**Table of Contents (Overview)**

1. Inference-Time Scaling Overview

2. Chain-of-Thought Prompting

3. Self-Consistency

4. Best-of-N Ranking

5. Rejection Sampling with a Verifier

6. Self-Refinement

7. Search Over Solution Paths

8. Conclusions, Categories, and Combinations

9. Bonus: What Do Proprietary LLMs Use?

You can use the left-hand navigation bar in the article’s web view to jump directly to any section.

*Inference-time scaling* (also called *inference-compute scaling*, *test-time scaling*, or just *inference scaling*) is an umbrella term for methods that allocate more compute and time during inference to improve model performance.

This idea has been around for a long time, and one can think of ensemble methods in classic machine learning as an early example of inference-time scaling. I.e., using multiple models requires more compute resources but can give better results.

Even in LLM contexts, this idea has been around for a long time. However, I remember it became particularly popular (again) when OpenAI showed an inference-time scaling and training plot in one of their o1 announcement blog articles last year ( [Learning to Reason with LLMs](https://openai.com/index/learning-to-reason-with-llms/)).

[![](https://substackcdn.com/image/fetch/$s_!oiA2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffff769a2-8324-4fbd-8659-4615e4711ce2_1600x900.png)](https://substackcdn.com/image/fetch/$s_!oiA2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffff769a2-8324-4fbd-8659-4615e4711ce2_1600x900.png)

*Figure 1: Spending additional resources during inference (left) and training (right) generally improves the model’s accuracy.*

I think this figure, adapted from OpenAI’s [blog post](https://openai.com/index/learning-to-reason-with-llms/), nicely captures the idea behind the two knobs we can use to improve LLMs. We can spend more resources during training (more data, bigger models, more or longer training stages) or inference.

Actually, in practice, it’s even better to do both at the same time: train a stronger model and use additional inference scaling to make it even better.

In this article, I only focus on the left part of the figure, inference-time scaling techniques, i.e., those training-free techniques that don’t change the model weights.
