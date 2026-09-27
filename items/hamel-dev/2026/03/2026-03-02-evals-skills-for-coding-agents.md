---
title: Evals Skills for Coding Agents
link: https://hamel.dev/blog/posts/evals-skills/
source: hamel-dev
published: 2026-03-02T08:00:00Z
updated: 2026-03-02T08:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Hamel Husain
- Shreya Shankar
labels:
- ai
- evals
summary: 'Today, Shreya Shankar and I are publishing evals skills, a set of skills for AI product evals1. Eval tools often get in the way. They nudge you toward generic off-the-shelf metrics and fully automated evals before you’ve looked at your data. These skills help you avoid common mistakes we’ve seen helping 50+ companies and teaching students in our AI Evals course. Why skills for evals There are many easily avoidable footguns in evals. These skills help you avoid them. evals-start is the entry point. It looks at your situation and routes you to the right skill. Most of the time it will send you to one of these two: eval-audit, if you already have an eval pipeline. It inspects your setup and recommends next steps. 2 error-discovery, if you have traces but haven’t analyzed them yet. It builds a customized annotation interface and helps you sample traces intelligently. Shreya does a live walkthrough of using this skill here. The skills Install the skills: npx skills add https://github.com/ai-evals-course/evals-skills Then give your agent this prompt: Run the evals-start skill from the evals plugin and follow the skill it picks. If it picks eval-audit, investigate each diagnostic area using a separate subagent in parallel, then synthesize the findings into a single report. If you’re experienced with evals, skip the router and pick the skill you need: Skill What it does evals-start Entry point. Routes to the skill that matches your situation eval-audit Audit an eval pipeline and surface problems with prioritized severity error-discovery Build a review app, select diverse samples, and organize your notes into failure modes generate-synthetic-data Create diverse synthetic test inputs using dimension-based tuple generation write-judge-prompt Design LLM-as-Judge evaluators for subjective quality criteria validate-evaluator Calibrate LLM judges against human labels using data splits, TPR/TNR, and bias correction evaluate-rag Evaluate retrieval and generation quality in RAG pipelines build-review-interface Build custom annotation interfaces for human trace review These skills are only a starting point. To make them better, tune them to be more specific to your data and domain. The repo is ai-evals-course/evals-skills. You can find me on X or email me through my newsletter. Footnotes Not foundation model benchmarks like MMLU or HELM that measure general LLM capabilities. Product evals measure whether your pipeline works on your task with your data. If you aren’t familiar with product-specific AI evals, check out my AI Evals FAQ.↩︎ The audit isn’t a complete solution, but it will catch common problems.↩︎'
content: extracted
html: 2026-03-02-evals-skills-for-coding-agents.html
preview:
  file: 2026-03-02-evals-skills-for-coding-agents.preview-55884f864bba.webp
  width: 256
  height: 144
  color: '#171a20'
images:
- source: https://hamel.dev/blog/posts/evals-skills/cover-gradient.png
  original:
    file: 2026-03-02-evals-skills-for-coding-agents.image-7a198524c13b.png
    width: 1280
    height: 720
  color: '#0d1117'
- source: https://hamel.dev/blog/posts/evals-skills/cover-original.png
  original:
    file: 2026-03-02-evals-skills-for-coding-agents.image-57d7efdbc20f.png
    width: 1280
    height: 720
  variants:
  - file: 2026-03-02-evals-skills-for-coding-agents.image-35475f8d957c.webp
    width: 320
    height: 180
  - file: 2026-03-02-evals-skills-for-coding-agents.image-14cf6c78d02b.webp
    width: 640
    height: 360
  - file: 2026-03-02-evals-skills-for-coding-agents.image-09bfc078c338.webp
    width: 1280
    height: 720
  color: '#0d1117'
---

![](https://hamel.dev/blog/posts/evals-skills/cover-original.png)

Today, Shreya Shankar and I are publishing [evals skills](https://github.com/ai-evals-course/evals-skills), a set of skills for AI product evals[^1].

Eval tools often get in the way. They nudge you toward [generic off-the-shelf metrics](https://hamel.dev/blog/posts/evals-faq/index.html#q-should-i-use-ready-to-use-evaluation-metrics) and [fully automated evals](https://parlance-labs.com/blog/posts/auto-evals.html) before you’ve looked at your data. These skills help you avoid common mistakes we’ve seen helping 50+ companies and teaching students in our [AI Evals course](https://maven.com/parlance-labs/evals).

There are many [easily avoidable footguns](https://hamel.dev/blog/posts/revenge/index.html) in evals. These skills help you avoid them.

**evals-start** is the entry point. It looks at your situation and routes you to the right skill. Most of the time it will send you to one of these two:

- **eval-audit**, if you already have an eval pipeline. It inspects your setup and recommends next steps. [^2]
- **error-discovery**, if you have traces but haven’t analyzed them yet. It builds a customized annotation interface and helps you sample traces intelligently. Shreya does a live walkthrough of using this skill [here](https://youtu.be/tqUDjc1HzO4).

## The skills

Install the skills:

```
npx skills add https://github.com/ai-evals-course/evals-skills
```

Then give your agent this prompt:

> Run the evals-start skill from the evals plugin and follow the skill it picks. If it picks eval-audit, investigate each diagnostic area using a separate subagent in parallel, then synthesize the findings into a single report.

If you’re experienced with evals, skip the router and pick the skill you need:

| Skill                   | What it does                                                                              |
| ----------------------- | ----------------------------------------------------------------------------------------- |
| evals-start             | Entry point. Routes to the skill that matches your situation                              |
| eval-audit              | Audit an eval pipeline and surface problems with prioritized severity                     |
| error-discovery         | Build a review app, select diverse samples, and organize your notes into failure modes    |
| generate-synthetic-data | Create diverse synthetic test inputs using dimension-based tuple generation               |
| write-judge-prompt      | Design LLM-as-Judge evaluators for subjective quality criteria                            |
| validate-evaluator      | Calibrate LLM judges against human labels using data splits, TPR/TNR, and bias correction |
| evaluate-rag            | Evaluate retrieval and generation quality in RAG pipelines                                |
| build-review-interface  | Build custom annotation interfaces for human trace review                                 |

These skills are only a starting point. To make them better, tune them to be more specific to your data and domain.

The repo is [ai-evals-course/evals-skills](https://github.com/ai-evals-course/evals-skills).

You can find me on [X](https://x.com/hamelhusain) or email me through my [newsletter](https://ai.hamel.dev).

[^1]: Not foundation model benchmarks like MMLU or HELM that measure general LLM capabilities. Product evals measure whether *your* pipeline works on *your* task with *your* data. If you aren’t familiar with product-specific AI evals, check out my [AI Evals FAQ](https://hamel.dev/blog/posts/evals-faq/index.html).

[^2]: The audit isn’t a complete solution, but it will catch common problems.
