---
title: Disruption with some GitHub services
link: https://www.githubstatus.com/incidents/qwdwmtqghpk5
source: githubstatus-com
published: 2026-09-15T11:17:22Z
updated: 2026-09-22T12:41:35Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-15-disruption-with-some-github-services.html
preview:
  file: 2026-09-15-disruption-with-some-github-services.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-15-disruption-with-some-github-services.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-15-disruption-with-some-github-services.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On September 15, 2026, between 05:45 and 09:50 UTC, the Claude Fable 5.1 model in GitHub Copilot experienced intermittently degraded availability, with an average error rate of 2.8%. During brief recurring 15 minute periods that recurred every ~45 minutes, availability for Claude Fable 5.1 dropped to a maximum of ~40% before recovering completely. Other Copilot models were not affected. Users could continue to work with a different model or with 'Auto'.

The cause was an issue with an upstream model provider that intermittently rejected requests while overloaded. GitHub worked with the provider, who acknowledged and then resolved the underlying issue at 9:50 UTC, after which the model returned to constant normal availability. Once recovery was guaranteed, we resolved the incident at 11:17 UTC.

To reduce the chance of recurrence and customer impact, GitHub is reviewing per-model availability alerting and automatic in-product fallback so that requests to a degraded model can shift to a healthy alternative more quickly.

Posted Sep 15, 2026 - 11:17 UTC

## Update

The issues with our upstream model provider have been resolved, and Claude Fable 5.1 is once again available in Copilot products and IDE surfaces.

We will continue monitoring to ensure stability, but mitigation is complete.

Posted Sep 15, 2026 - 11:17 UTC

## Update

We continue to monitor intermittent errors affecting Claude Fable 5.1 in some Copilot products and integrated development environments. Customer-facing metrics have recovered, and we are awaiting confirmation from our upstream provider that the issue will not recur. Customers can select another model or Auto in the meantime.

Posted Sep 15, 2026 - 11:09 UTC

## Update

We continue to investigate intermittent errors affecting Claude Fable 5.1 in some Copilot products and integrated development environments. Customers can use another model or select Auto while we monitor the situation.

Posted Sep 15, 2026 - 10:32 UTC

## Investigating

Copilot AI Model Providers is experiencing degraded performance. We are continuing to investigate.

Posted Sep 15, 2026 - 10:20 UTC

## Monitoring

We are investigating degraded availability for Claude Fable 5.1, affecting some Copilot products and integrated development environments. The issue is caused by a problem with our upstream model provider. Customers can use another model or select Auto while we investigate.

Posted Sep 15, 2026 - 09:48 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Sep 15, 2026 - 09:47 UTC

This incident affected: Copilot AI Model Providers.
