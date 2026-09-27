---
title: An LLM-as-Judge Won't Save The Product—Fixing Your Process Will
link: https://eugeneyan.com//writing/eval-process/
source: eugeneyan-com
published: 2025-04-20T00:00:00Z
updated: 2025-04-20T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- eval
- llm
summary: Applying the scientific method, building via eval-driven development, and monitoring AI output.
content: extracted
html: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.html
preview:
  file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.preview-d7707444c1f9.webp
  width: 256
  height: 135
  color: '#f4f4f4'
images:
- source: https://eugeneyan.com/assets/og_image/eval-method.jpg
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-a4a4fe7b900a.jpg
    width: 1200
    height: 631
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/eval-method.webp
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-af8f7330c899.webp
    width: 1200
    height: 631
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-34f3ae4582da.webp
    width: 320
    height: 168
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/edd.webp
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-4b433d5a9077.webp
    width: 1200
    height: 630
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-8281258a9b82.webp
    width: 320
    height: 168
  color: '#fefefe'
- source: https://eugeneyan.com/assets/ai-monitoring.webp
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-a4328cc428dd.webp
    width: 1200
    height: 486
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-584cb064fa15.webp
    width: 320
    height: 130
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2025-04-20-an-llm-as-judge-won-t-save-the-product-fixing-your-process.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Product [evals](https://eugeneyan.com/tag/eval/) are misunderstood. Some folks think that adding another tool, metric, or LLM-as-judge will solve the problems and save the product. But this sidesteps the core problem and avoids the real work. Evals aren’t static artifacts or quick fixes; they’re practices that apply the scientific method, eval-driven development, and AI output monitoring.

**Building product evals is simply the scientific method in disguise.** That’s the secret sauce. It’s a cycle of inquiry, experimentation, and analysis.

It starts with *observation* aka Look at The Data™. This means examining our inputs, AI outputs, and how users interact with our systems. By looking at the data, we learn where the system works well, and perhaps more crucially, where it fails. Identifying these failure modes is the starting point for meaningful improvement.

Then, we *annotate* some data, prioritizing problematic outputs. This means labeling samples of successes and failures to build a balanced and representative dataset. Ideally, we should have a 50:50 split of passes and fails that spans the distribution of inputs. This dataset forms the foundation for targeted evals that track performance on the issues we’ve identified.

Next, we *hypothesize* why specific failures occur. Perhaps our RAG’s retrieval isn’t returning the relevant context, or maybe the model struggles to follow the complex—and sometimes conflicting—instructions. By looking at data such as retrieved documents, reasoning traces, and erroneous outputs, we can prioritize failures to fix and hypotheses to test.

Then, we *design and run experiments* to test our hypotheses. Experiments could involve rewriting prompts, updating retrieval components, or switching to a different model. A good experiment defines the outcomes that confirm or invalidate the hypotheses. Ideally, it should also include a baseline or control condition against which to compare.

*Measuring outcomes and analyzing errors* is often the most challenging step. Unlike casual vibe checks, this requires quantifying whether an experiment’s updates actually improved outcomes: Did accuracy increase? Were fewer defects generated? Does the new version do better in pairwise comparisons? We can’t improve outcomes if we can’t measure it.

If an experiment succeeds, we apply the update; if it fails, we dig into the error analysis, refine our hypotheses, and try again. Through this iterative loop, product evals becomes the data flywheel that improves our product, reduces defects, and earns user trust.

![Image](https://eugeneyan.com/assets/eval-method.webp "Image")

Applying the scientific method to building AI products.

**Eval-driven development (EDD) helps us build better AI products.** It’s similar to [test-driven development](https://en.wikipedia.org/wiki/Test-driven_development) where we write tests before implementing software that passes those tests. EDD follows the same philosophy: Before developing an AI feature, we define success criteria, via product evals, to ensure alignment and measurability from day one. Here’s a secret: Machine learning teams have practiced this for decades, where we build models and systems against [validation and test sets](https://en.wikipedia.org/wiki/Training,_validation,_and_test_data_sets). The ideas are similar though they have different names.

In EDD, evals guide our development. We start by evaluating a baseline, perhaps a simple prompt, to get an initial benchmark. From then on, every prompt tweak, every system update, every iteration, is evaluated. Did simplifying the prompt improve faithfulness? Did updating retrieval increase the recall of relevant documents? Or did the update worsen performance?

Because EDD provides immediate, objective feedback, we can see what’s improving and what’s not. This cycle—write evals, make changes, run evals, integrate improvements—ensures measurable progress. Instead of relying on vague, intuition-based perceptions, we build a feedback loop grounded in software engineering practices.

![Image](https://eugeneyan.com/assets/edd.webp "Image")

First, write some evals; then, build systems that pass those evals.

**Human oversight is still needed even with automated evaluators (aka LLM-as-judge).** While automated evals help scale monitoring, they can’t compensate for neglect. If we’re not actively reviewing AI outputs and customer feedback, automated evaluators won’t save our product.

To evaluate and monitor AI products, we typically sample outputs and annotate them for quality and defects. With enough high-quality annotations, we can [calibrate automated evaluators](https://eugeneyan.com/writing/aligneval/) to align with human judgment. This could mean measuring recall or precision on binary labels, or correlation when deciding between outputs via pairwise comparisons. Once properly aligned, these evaluators help scale the continuous monitoring of AI systems.

But having automated evaluators doesn’t remove the need for human oversight. We still need to periodically sample and annotate data, and analyze user feedback. Ideally, we should design products that capture implicit feedback through user interactions. Nonetheless, explicit feedback, while less frequent and occasionally biased, can also be valuable. (Read [more](https://eugeneyan.com/writing/llm-patterns/#collect-user-feedback-to-build-our-data-flywheel).)

Also, while automated evaluators scale well, they aren’t perfect. But neither are human annotators. Nonetheless, by collecting more and higher-quality annotations, we can better align these evaluators. Organizational discipline is crucial to maintain this feedback loop of sampling data, annotating outputs, and improving automated evaluators.

![Image](https://eugeneyan.com/assets/ai-monitoring.webp "Image")

Automated evaluators amplify our existing annotation and feedback processes.

While building with AI can feel like magic, building AI products still takes elbow grease. If teams don’t apply the scientific method, practice eval-driven development, and monitor the system’s output, buying or building yet another evaluation tool won’t save the product.

By the way, if you want to learn more about evals, my friends Hamel and Shreya are hosting their *final* cohort of “AI Evals for Engineers and PMs” in July. Here’s a [35% discount code](https://maven.com/parlance-labs/evals?promoCode=eugene-is-all-you-need).

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Apr 2025). An LLM-as-Judge Won't Save The Product—Fixing Your Process Will. eugeneyan.com. https://eugeneyan.com/writing/eval-process/.

or

```
@article{yan2025eval-process,
  title   = {An LLM-as-Judge Won't Save The Product—Fixing Your Process Will},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2025},
  month   = {Apr},
  url     = {https://eugeneyan.com/writing/eval-process/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
