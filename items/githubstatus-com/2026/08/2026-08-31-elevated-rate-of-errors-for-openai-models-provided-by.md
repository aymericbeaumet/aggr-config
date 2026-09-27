---
title: Elevated rate of errors for OpenAI models provided by Copilot
link: https://www.githubstatus.com/incidents/7fxts6gmq5gr
source: githubstatus-com
published: 2026-08-31T09:58:14Z
updated: 2026-09-03T21:39:35Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-31-elevated-rate-of-errors-for-openai-models-provided-by.html
preview:
  file: 2026-08-31-elevated-rate-of-errors-for-openai-models-provided-by.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-31-elevated-rate-of-errors-for-openai-models-provided-by.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-31-elevated-rate-of-errors-for-openai-models-provided-by.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

Between 08:37 and 09:41 UTC on August 31, 2026, GitHub Copilot experienced degradation affecting several GPT models, including gpt-5.2, gpt-5.3-codex, gpt-5.4, gpt-5.4-mini, gpt-5.4-nano, and the gpt-5.6 family (Luna, Sol, and Terra). Users encountered elevated error rates and interrupted streaming responses. Other models were not affected.

The degradation was caused by an issue with an upstream model provider. GitHub engineers detected the issue through automated monitoring, displayed in-product warnings for the affected models, and coordinated with the provider. Service returned to normal after the provider implemented a mitigation.

Posted Aug 31, 2026 - 09:58 UTC

## Update

The issues with our upstream model provider have been resolved, and gpt-5.3-codex, gpt-5.4-mini, gpt-5.4-nano, gpt-5.5, and the gpt-5.6 family of models are once again available in Copilot products and IDE surfaces.

We will continue monitoring to ensure stability, but mitigation is complete.

Posted Aug 31, 2026 - 09:58 UTC

## Monitoring

The degradation affecting Copilot AI Model Providers has been mitigated. We are monitoring to ensure stability.

Posted Aug 31, 2026 - 09:51 UTC

## Update

One of our model providers has confirmed an incident on their end. We have provided them details to help identify the issue. We are starting to see recovery.

Posted Aug 31, 2026 - 09:48 UTC

## Update

Copilot is experiencing a higher rate of errors for OpenAI models, including gpt-5.2, gpt-5.3-codex, gpt-5.4, gpt-5.4, and the gpt-5.6 family of models. Other models are not impacted.

Posted Aug 31, 2026 - 09:22 UTC

## Investigating

We are investigating reports of degraded performance for Copilot AI Model Providers

Posted Aug 31, 2026 - 09:15 UTC

This incident affected: Copilot AI Model Providers.
