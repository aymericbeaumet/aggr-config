---
title: Incident with Grok Copilot AI Model Provider
link: https://www.githubstatus.com/incidents/ktdr5t0xwnhp
source: githubstatus-com
published: 2026-09-03T17:11:47Z
updated: 2026-09-09T17:12:18Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-03-incident-with-grok-copilot-ai-model-provider.html
preview:
  file: 2026-09-03-incident-with-grok-copilot-ai-model-provider.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-03-incident-with-grok-copilot-ai-model-provider.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-03-incident-with-grok-copilot-ai-model-provider.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

Between 13:22 and 17:11 UTC on September 03, 2026, GitHub Copilot experienced degradation affecting several Grok models, including Grok 4.5 and Grok 4.6. Users encountered elevated error rates, but other models were not affected. The degradation was caused by an issue with an upstream model provider. GitHub engineers detected the issue through automated monitoring, displayed in-product warnings for the affected models, and coordinated with the provider. Service returned to normal after the provider implemented a mitigation.

Posted Sep 03, 2026 - 17:11 UTC

## Update

The issues with our upstream model provider have been resolved, and Grok models are once again available in Copilot products and IDE surfaces.\
We will continue monitoring to ensure stability, but mitigation is complete.

Posted Sep 03, 2026 - 17:11 UTC

## Update

The Grok 4.5 model has degraded availability as well. We are working with the upstream provider to resolve the issue.

Posted Sep 03, 2026 - 14:22 UTC

## Update

We are experiencing degraded availability for the Grok 4.6 model in Copilot Chat, VS Code and other Copilot products. This is due to an issue with an upstream model provider. We are working with them to resolve the issue.

Posted Sep 03, 2026 - 14:20 UTC

## Investigating

We are investigating reports of degraded performance for Copilot AI Model Providers

Posted Sep 03, 2026 - 14:17 UTC

This incident affected: Copilot AI Model Providers.
