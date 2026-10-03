---
title: One month coding with GLM 5.3 Flash
link: https://wagtail.org/blog/one-month-on-glm-53-flash/
source: hnrss-org
published: 2026-10-02T15:29:15Z
updated: 2026-10-02T15:29:15Z
first_seen: 2026-10-03T05:41:51.909771426Z
authors:
- ThibWeb
content: extracted
html: 2026-10-02-one-month-coding-with-glm-5-3-flash.html
preview:
  file: 2026-10-02-one-month-coding-with-glm-5-3-flash.preview-e1751f4a6455.webp
  width: 256
  height: 147
  alt: Tree map of agents token usage over September 2026, with half going to GLM 5.3 Flash
  color: '#9ab1d1'
images:
- source: https://media.wagtail.org/images/agentsview_septembermodels_split.min-1200x630.png
  original:
    file: 2026-10-02-one-month-coding-with-glm-5-3-flash.image-19f92c7c43df.png
    width: 1013
    height: 580
  color: '#f9fafa'
- source: https://media.wagtail.org/images/agentsview_tokens_september_2026.width-950.png
  original:
    file: 2026-10-02-one-month-coding-with-glm-5-3-flash.image-ea148a065760.png
    width: 950
    height: 618
  color: '#fafbfc'
- source: https://media.wagtail.org/images/agentsview_septembermodels_split.width-800.png
  original:
    file: 2026-10-02-one-month-coding-with-glm-5-3-flash.image-c49a35336fe9.png
    width: 800
    height: 458
  color: '#f9fafa'
- source: https://media.wagtail.org/images/pareto_frontier_September_2026.width-800.png
  original:
    file: 2026-10-02-one-month-coding-with-glm-5-3-flash.image-026705390feb.png
    width: 800
    height: 810
  color: '#141a23'
- source: https://media.wagtail.org/images/benchmark_wagtail_tasks_energy_use.width-800.png
  original:
    file: 2026-10-02-one-month-coding-with-glm-5-3-flash.image-6512f429cdc5.png
    width: 800
    height: 466
  color: '#151b27'
---

![Screenshot of AgentsView tokens usage for September 2026 over 2B tokens](https://media.wagtail.org/images/agentsview_tokens_september_2026.width-950.png)

Zooming in on the models split specifically:

![Tree map of agents token usage over September 2026, with half going to GLM 5.3 Flash](https://media.wagtail.org/images/agentsview_septembermodels_split.width-800.png)

The goal was to spend the whole month on GLM 5.3 Flash pictured in teal. Here’s what went well:

- Successfully spent the first half of the month on just that model.
- That model’s usage was well within our budget ($68, about 4kWh of energy use / 365 grams of carbon emissions).

The second half of the month didn’t go so well, with 1B tokens going to other models.

## Unexpected hurdles

### The cost of vibe coding

We’re pretty transparent that our [experimental Wagtail MCP server is a vibe-coded prototype](https://wagtail.org/blog/experiments-with-mcp-in-wagtail/). Vibe coding isn’t quite what we normally aspire to, but for a prototype it’s spot on. Unfortunately there are still consequences to it. I chose the 'wrong' model for the prototype, and we spent 450M tokens / $150 / 5kWh of energy use almost overnight. The MCP server itself works well and we now have a great demo of the capabilities, so it’s not for nothing:

[MCP server in Wagtail - experiments demo](https://www.youtube.com/watch?v=TOtoB0RMPvg)

Nonetheless, it’s a good reminder to be careful with model selection and with agentic patterns. We could have achieved similar results for most likely 5x less cost with not that much more effort. Lessons learned! We need to budget for this, and be more careful. Could have seen it coming, but now we know.

### Infrastructure woes

Another unexpected hurdle was infrastructure availability issues. We’ve written extensively about [comparing inference providers](https://wagtail.org/blog/comparing-open-weight-ai-models-and-providers/). Our choices work really most of the times, but it turns out they’re very popular, and do not have the same capacity as the big labs who hoard all the GPUs. We noted degradation with the performance of GLM 5.3 Flash in particular, most likely because of it being [so high up](https://wagtail.org/blog/open-models-only-adoption-challenge/) the Pareto frontier of relevant models for our work.

![Scatter plot of AI models, with the drawn pareto frontier, GLM 5.3 Flash in the top left](https://media.wagtail.org/images/pareto_frontier_September_2026.width-800.png)

This meant having to switch to other similar models (DeepSeek V4.1 Flash, Qwen 3.8 Flash). Which is very simple to do, but nonetheless unexpected!

### The cost of experimentation and R&D

Last but not least, beyond using one model for day-to-day engineering, it felt essential to keep experimenting with a wide range of models, keeping up with what providers are releasing. This is particularly essential as we start to benchmark models’ performance on Wagtail tasks, where we need data across a wide range of models. Sneak peek of our benchmark:

![Data table of 14 AI models reporting their accuracy as a percentage, energy use in Wh, Cost in $. Top of the table is DeepSeek V4.1 Flash with 95% accuracy and 14.9Wh energy use, $0.09 per task](https://media.wagtail.org/images/benchmark_wagtail_tasks_energy_use.width-800.png)

It’s much easier to guide people towards leaner options with this kind of concrete data. And for us to make those options even more viable with agent skills, or [our new CLI prototype](https://wagtail.org/blog/prototyping-a-cli-for-wagtail/), which is intended to work well with agents.

## Takeways and what to do next

So technically this challenge was a failure. Only 50% usage on the target model, 1B out of 2B tokens. About 35 kWh of energy use instead of 10. But we did learn a lot, which is crucial for the current moment. Reflecting on this for October, here’s what will make it work:

1. **Constant, local usage measurement and reporting**. Looking not just at tokens but also energy use and spend, and ideally how well this all leads to concrete positive outcomes.
2. **Budgeting for experimentation, not just day-to-day tasks**. Making more concerted decisions about which prototypes are worth building, and how.
3. **Better prompt selection and multi-agent techniques**. Orchestrator vs. scout vs. implementer vs. reviewer agents. Bounded goals. Not rocket science but certainly one more thing to learn.
4. **Keep pushing for more efficient techniques and models**. The Jev-style decision diffusion models look very promising if they can run so efficiently. Latest flagship models also look like a step in the right direction on that front.

For day-to-day developer work, it’s totally viable to focus on one or two flash-tier cheap models. A viable target is probably that the *majority* of AI inference work should be done with such efficient models, measured in cost or energy use rather than meaningless tokens. That’s the goal for October! You should try it too, you’ll learn a lot in the process.

* * *

And come say hi at [Wagtail Space 2026](https://wagtail.org/wagtail-space-2026/) in November to hear how that all pans out!
