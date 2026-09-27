---
title: llm weights vs the papercuts of corporate
link: https://ghuntley.com/papercuts/
source: ghuntley-com
published: 2025-12-08T15:55:28Z
updated: 2025-12-08T15:55:28Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Geoffrey Huntley
labels:
- ai
- software development
summary: In woodworking, there's a saying that you should work with the grain, not against the grain and I've been thinking about how this concept may apply to large language models. These large language models are built by training on existing data. This data forms the backbone
content: extracted
html: 2025-12-08-llm-weights-vs-the-papercuts-of-corporate.html
preview:
  file: 2025-12-08-llm-weights-vs-the-papercuts-of-corporate.preview-485d9ea638c7.webp
  width: 256
  height: 256
  color: '#d2c5ce'
images:
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/size/w1200/2025/12/universal_upscale_0_6a2d76dd-41e0-42a4-8529-bf43d0280256_0.jpg
  original:
    file: 2025-12-08-llm-weights-vs-the-papercuts-of-corporate.image-2bb62f83cf68.jpg
    width: 1200
    height: 1200
  color: '#fefefe'
---

In woodworking, there's a saying that you should work with the grain, not against the grain and I've been thinking about how this concept may apply to large language models.

These large language models are built by training on existing data. This data forms the backbone which creates output based upon the preferences of the underlying model weights.

We are now one year in where a new category of companies has been founded whereby the majority of the software behind that company was code-generated.

From here on out I’m going to call to these companies as model weight first. This category of companies can be defined as any company that is building with the data (“grain”) that has been baked into the large language models.

Model weight first companies do not require as much context engineering. They’re not stuffing the context window with rules to try attempt to override and change the base models to fit a pre-existing corporate standard and conceptualisation of how software should be.

The large language model has decided on what to call a method name or class name because that method or classs name is what the large language model prefers thus, when code is adapted, modified, and re-read into the context window, it is consuming its preferred choice of tokens.

Model-weight-first companies do not have the dogma of snake\_case vs PascalCase vs kebab-case policies that many corporate companies have. Such policies were created for humans to create consistency so humans can comprehend the codebase. Something that is of a lesser concern now that AI is here.

Now variable naming is a contrived example, but I suspect in the years to come if a study was done to compare the velocity/productivity/success rates with AI of a model weight first company vs. a corporate company, I suspect a model weight company have vastly better outcomes because they're not trying to do context engineering to force the LLM to follow some pre-existing dogma. There is one universal truth with LLMs as they are now: the less that you use, the better the outcomes you get.

The less that you allocate (i.e., cursor rules or what else have you), then you'll have more context window available for actually implementing requirements of the software that needs to be built.

So if we take this thought experiment about the models having preferences for tokens and expand it out to another use case, let's say that you needed to build a Docker container at a model weight first company.

You could just ask an LLM to build a Docker container, and it knows how to build a Docker container for say Postgres, and it just works. But in the corporate setting, if you ask it to build a Docker container, and in that corporate you have to configure HTTPS, squid proxy, or some sort of artifactory and outbound internet access is restricted, that same simple thing becomes very comical.

You'll see an agent fill up with lots of failed tool calls unless you do context engineering to say "no, if you want to build a docker container, you got to follow these particular allocations of company conventions” in a crude attempt to override the preferences of the inbuilt model weights.

> Building at large companies is hard right now due to the legacy infrastructure. 10-15 years ago, some of these companies were building in house to be frontier. Today, what's available internally is too disconnected from what's available outside and is often years behind.
>
> — Jaana Dogan ヤナ ドガン (@rakyll) [January 3, 2026](https://twitter.com/rakyll/status/2007279234822324598?ref_src=twsrc%5Etfw&ref=ghuntley.com)

At a model weight first company, building a docker image is easy but at a corporate the agent will have one hell of a time and end up with a suboptimal/disappointing outcome.

So, perhaps this is going to be a factor that needs to be considered when talking and comparing the success rates of AI at one company versus another company, or across industries.

If a company is having problems with AI and getting outcomes from AI, are they a model weight first company or are they trying to bend AI to their whims?

Perhaps the corporates who succeed the most with the adoption of AI will be those who shed their dogma that no longer applies and start leaning into transforming to become model-weight-first companies.

ps. socials.

> 🗞️ llm weights vs the papercuts of corporate
>
> (link in next post)
>
> “If a company is having problems with AI and getting outcomes from AI, are they a model weight first company or are they trying to bend AI to their whims?” [pic.twitter.com/WLMGF4HYa1](https://t.co/WLMGF4HYa1?ref=ghuntley.com)
>
> — geoff (@GeoffreyHuntley) [December 8, 2025](https://twitter.com/GeoffreyHuntley/status/1998059600780865979?ref_src=twsrc%5Etfw&ref=ghuntley.com)
