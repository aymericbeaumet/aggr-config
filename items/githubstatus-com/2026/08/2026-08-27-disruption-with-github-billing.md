---
title: Disruption with GitHub Billing
link: https://www.githubstatus.com/incidents/5bn0vk444m1w
source: githubstatus-com
published: 2026-08-27T19:44:08Z
updated: 2026-08-31T22:46:13Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-27-disruption-with-github-billing.html
preview:
  file: 2026-08-27-disruption-with-github-billing.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-27-disruption-with-github-billing.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-27-disruption-with-github-billing.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On August 26, 2026, between 20:40 UTC and 00:51 UTC on August 27, GitHub Billing experienced degraded performance affecting billing budget pages and GitHub Copilot CLI sessions. Affected customers encountered failed budget page loads or failures when starting or continuing CLI sessions. We confirmed this impact for a small number of customers (

This was caused by a concentrated workload that created processing delays in our data storage layer. Automated retries increased the load and prolonged the degradation. We mitigated the incident by rebalancing traffic within our infrastructure.

We are improving workload isolation, retry behavior, and detection of concentrated load to reduce the likelihood of recurrence and shorten our time to detect and mitigate similar incidents.

Posted Aug 27, 2026 - 19:44 UTC

## Update

No material change since the previous update. Service conditions remain stable following the mitigation, and we have not observed any further customer impact. We are actively monitoring the service while implementing targeted fixes to address the underlying root cause.

Posted Aug 27, 2026 - 17:58 UTC

## Update

Our mitigation continues to hold, and service conditions remain stable. We are continuing to investigate the concentrated workload responsible for the issue and are preparing additional preventative improvements. We have not identified a material change in customer impact since the previous update. We will provide another update as the investigation progresses.

Posted Aug 27, 2026 - 16:20 UTC

## Update

Our mitigation is still holding as we continue to investigate to find the root cause.

Posted Aug 27, 2026 - 14:49 UTC

## Update

We are continuing to monitor the mitigation that we have applied for the billing page disruption.

Posted Aug 27, 2026 - 01:35 UTC

## Update

We've applied a mitigation to unblock Copilot usage and have observed recovery for this particular impact. We're continuing to investigate and apply mitigations for the billing page disruption while monitoring to ensure Copilot remains recovered.

Posted Aug 27, 2026 - 00:31 UTC

## Update

We are currently investigating increased errors with billing services. Customers may observe failed billing budget page loads, and users of the Copilot CLI may observe failures starting or continuing sessions.

Posted Aug 26, 2026 - 23:42 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Aug 26, 2026 - 23:37 UTC
