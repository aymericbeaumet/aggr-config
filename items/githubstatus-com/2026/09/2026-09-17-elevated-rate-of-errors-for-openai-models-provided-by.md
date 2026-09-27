---
title: Elevated rate of errors for OpenAI models provided by Copilot
link: https://www.githubstatus.com/incidents/nvy2q96mcmyg
source: githubstatus-com
published: 2026-09-17T21:49:01Z
updated: 2026-09-18T17:46:20Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-17-elevated-rate-of-errors-for-openai-models-provided-by.html
preview:
  file: 2026-09-17-elevated-rate-of-errors-for-openai-models-provided-by.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-17-elevated-rate-of-errors-for-openai-models-provided-by.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-17-elevated-rate-of-errors-for-openai-models-provided-by.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

Between 20:26 and 21:17 UTC on September 17, 2026, GitHub Copilot experienced degradation affecting several GPT models, including GPT-5.6 Luna, GPT-5.6 Terra, GPT-5.6 Sol, GPT-5.3-Codex, and GPT-6 Astra. Users encountered elevated error rates when using these models.

The degradation was caused by an issue with an upstream model provider. GitHub engineers detected the issue through automated monitoring and coordinated with the provider. Our automated model-warning system activated in-product warnings for affected models during the incident. Service returned to normal after the provider implemented a mitigation.

Posted Sep 17, 2026 - 21:49 UTC

## Monitoring

We are experiencing degraded availability for GPT-5.6 Luna, GPT-5.6 Terra, GPT-5.6 Sol, GPT-6 Astra, GPT-5.3-Codex in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. The provider is working to mitigate the problem and we are monitoring recovery. We recommend choosing another model or selecting 'Auto' to continue using Copilot.

Posted Sep 17, 2026 - 21:39 UTC

## Investigating

We are investigating reports of degraded performance for Copilot AI Model Providers

Posted Sep 17, 2026 - 20:59 UTC

This incident affected: Copilot AI Model Providers.
