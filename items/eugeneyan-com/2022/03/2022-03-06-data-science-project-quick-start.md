---
title: Data Science Project Quick-Start
link: https://eugeneyan.com//writing/project-quick-start/
source: eugeneyan-com
published: 2022-03-06T00:00:00Z
updated: 2022-03-06T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- datascience
- engineering
- productivity
summary: Hard-won lessons on how to start data science projects effectively.
content: extracted
html: 2022-03-06-data-science-project-quick-start.html
preview:
  file: 2022-03-06-data-science-project-quick-start.preview-ac6e8e197429.webp
  width: 256
  height: 134
  color: '#5e393a'
images:
- source: https://eugeneyan.com/assets/og_image/quick-start.jpg
  original:
    file: 2022-03-06-data-science-project-quick-start.image-ece72a3dfe83.jpg
    width: 1200
    height: 630
  color: '#a54844'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2022-03-06-data-science-project-quick-start.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2022-03-06-data-science-project-quick-start.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2022-03-06-data-science-project-quick-start.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2022-03-06-data-science-project-quick-start.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2022-03-06-data-science-project-quick-start.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2022-03-06-data-science-project-quick-start.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2022-03-06-data-science-project-quick-start.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2022-03-06-data-science-project-quick-start.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2022-03-06-data-science-project-quick-start.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2022-03-06-data-science-project-quick-start.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2022-03-06-data-science-project-quick-start.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2022-03-06-data-science-project-quick-start.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2022-03-06-data-science-project-quick-start.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

When I was a new data scientist, starting a project often meant defaulting to what I was familiar with—training machine learning models. I was overeager and didn’t consider the business goals, context, or success metrics. My process was disorganized; reproducibility and good engineering habits were the least of my concerns. Thus, whatever time saved early on was paid for with exorbitant interest when we went to production.

Several mistakes and mentors later, I’ve gained some hard-won lessons on how to start data science projects effectively. I’ll share them here so you can go from nebulous problem to usable prototype, without repeating my errors.

## Understand the intent and context

Start by understanding the intent, the “why”, behind the project. I’ve found asking these questions helpful:

- What is the expected customer or business benefit?
- What’s wrong with the way things are now?
- Why is solving this important now?

Imagine you work at an e-commerce startup and a product manager asks you to build a recommendation engine.

You might start by asking, “What’s the expected benefit?”

“Customers find products easier and we increase engagement”, she replies.

You then follow-up with, “How do we define engagement? Clicks? Purchases?”

How we define engagement will determine the labels used for training (i.e., clicks or purchase). If purchases is the goal, we want to distinguish between conversion and revenue. Optimizing for conversion is relatively straightforward—we can train on sales and predict a purchase probability. If the goal is revenue, we might want to weigh purchase labels by item price, similar to how [YouTube weighs videos by watched time](https://research.google/pubs/pub45530/).

Also, try to get as much context as you can. Imagine we get two requests to build a feature store, each with its own circumstances. For the first request, what we build will be used by the entire organization. For the second request, it will be used by a single application while the app waits for the feature store by the core engineering team to come online. The first request will need to be extensible, scalable, and well documented. In contrast, the second request can probably do with a hacky solution and a fraction of the effort, or outright declined. Understanding the context enables us to scope the solution appropriately.

## Define requirements, constraints, and metrics

What should be achieved for the project to be a success? Try describing it from the customer’s or business’ point of view—what’s in it for them?

If we’re improving search, we might have requirements to reduce the proportion of reformulated and abandoned queries. If we’re building a recommender, requirements include increasing the number of clicks and/or attributed purchases on the widget. If we’re automating a manual product categorization process, we’ll set targets on the proportion of products automatically categorized with high confidence, and manpower saved.

Requirements can also be framed as constraints. What can our ML system *not* do? If our insurance fraud detector requires manual investigation for each flagged claim, we might constrain the number of false positives, a proxy for wasted effort, to be less than 25%. If we want to introduce new, cold-start products in our recommendations (for explore-exploit), we might set a constraint that overall conversion should not drop by more than 5%.

I’ve also found it helpful to consider production requirements (though we’re just starting the project). If engineering has a requirement on latency (e.g., <80ms@p99) and throughput (e.g., 8,000 transactions per second), we might not consider techniques that are prohibitively costly to deploy at scale. There could also be resource constraints. Not having a real-time feature store would preclude session-based recommendations, while not having the budget for a GPU cluster may mean starting with simpler, shallower models.

While these constraints may be limiting, I believe they help by narrowing our search space and saving us the unnecessary effort of considering solutions that can’t be used. Clearly defined constraints free us to do anything *except* breach those constraints, empowering us to innovate. You’ll be surprised how much the team can do with a frugal mindset.

To measure how we’re doing on the requirements, we need a set of metrics. This may require us to dogfood our own product to understand how the customer experiences it.

Here’s an ML metric example: Assume we’re asked to build a recommender that suggests 100 products for each product detail page. Given that we’re recommending 100 products, we might use a metric such as hit@100 or ndcg@100. However, while browsing the site, we find that the recommendation widget only displays 5 products on the screen; customers have to swipe to view the other recommendations. With this insight, we might update our metric to adopt k = 5. This would reflect the CX more accurately and likely correlated better with online A/B test metrics.

## Dig into the data early

Ideally, we’re able to do this before the requirements are finalized. Exploring the data might reveal the proposed requirements to be too much of a stretch.

Assume we’re asked to build a product classifier that categorizes products based on title and image. While inspecting the data, we find a portion of existing products to have the same image and title, but be in [different categories](https://eugeneyan.com/writing/product-categorization-api-part-2-data-preparation/#measuring-data-purity). For example, multiple seller listings of the same iPhone case could be categorized under “phone -> accessories -> case” and “phone -> iPhone -> accessories -> case”. In this case, instead of directly building the product classifier and achieving poor accuracy on these inconsistent labels, we might start with a phase of label cleaning and refactoring the product category tree.

We might implement a quick ML baseline as part of data exploration. How quick? A day or two. The baseline may suggest potential challenges in achieving target metrics.

For example, stakeholders have an initial requirement for your fraud detection model to achieve >95% recall and precision. However, your baseline is only able to achieve 60% recall and precision. While closing the gap between 60% and 95% isn’t impossible, it could be a challenging, multi-year effort. Thus, we might want to manage expectations and adjust the target metrics, as well as make trade-offs between recall and precision.

## Consult domain experts, papers, and open-source code

One shortcut to getting up to speed on an unfamiliar problem is to observe how others do it.

Trying to automate a manual process (e.g., insurance claims fraud detection)? Sit with the investigators and learn the heuristics that guide their process, and turn those heuristics into features for machine learning. Need to solve an unfamiliar machine learning problem? Read [papers and tech blogs](https://applyingml.com/papers/) on how others have done it. Want to try a new algorithm or model? Search on [GitHub](https://github.com) if there’s an open-source implementation available, or better yet, a pre-trained model available on [HuggingFace](https://huggingface.co/models).

## Standardize and automate your experiment pipeline

Like most data scientists, I do my early experiments and prototyping in Jupyter notebooks. To prevent them from getting too messy, I’ve found it helpful to refactor my notebooks weekly. Code snippets commonly used across notebooks are refactored into `.py` files instead of copy-pasting code cells (e.g., `metrics.py`, `plots.py`, `logger.py`). Manual steps and commented-out cells—which will be forgotten after a few months—are also pruned. The result is a notebook that can run from start to finish, without any manual intervention, with metrics and plots at the end.

I’ve also learned to automate as much as possible (because I’m lazy). For ML techniques that require extensive hyperparameter tuning (e.g., boosted trees, deep learning), I’ve been using [Hyperopt](https://github.com/hyperopt/hyperopt) since my kaggling days. More recent and popular frameworks include [Optuna](https://github.com/optuna/optuna) and [Tune](https://github.com/ray-project/tune-sklearn). For tracking metrics, [MLFlow](https://github.com/mlflow/mlflow) is free and easy to use. You can even parameterize notebooks with [Papermill](https://github.com/nteract/papermill) so each experiment is run in a separate notebook. (I wrote a [short guide](https://eugeneyan.com/writing/experimentation-workflow-with-jupyter-papermill-mlflow/) on using MLFlow and Papermill to simplify experimentation).

If we do the above, we’ll have a good understanding of the business intent, requirements, and constraints. We’ll also have a feel for the data, a set of initial papers and code to explore, and an experiment pipeline to iterate quickly. Now, we can dive deeper into the data and try increasingly sophisticated techniques as we confidently solve the right problem and deploy a usable solution.

Are they any habits you’ve found useful when starting a data science project? Please [reach out](https://twitter.com/eugeneyan) or comment below!

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Mar 2022). Data Science Project Quick-Start. eugeneyan.com. https://eugeneyan.com/writing/project-quick-start/.

or

```
@article{yan2022quick,
  title   = {Data Science Project Quick-Start},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2022},
  month   = {Mar},
  url     = {https://eugeneyan.com/writing/project-quick-start/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
