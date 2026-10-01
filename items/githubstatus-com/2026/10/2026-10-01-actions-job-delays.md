---
title: Actions Job Delays
link: https://www.githubstatus.com/incidents/2dpbcq5j165n
source: githubstatus-com
published: 2026-10-01T17:56:54Z
updated: 2026-10-01T17:56:54Z
first_seen: 2026-10-01T19:33:44.897168732Z
content: extracted
html: 2026-10-01-actions-job-delays.html
preview:
  file: 2026-10-01-actions-job-delays.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-10-01-actions-job-delays.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-10-01-actions-job-delays.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

This incident has been resolved. Thank you for your patience and understanding as we addressed this issue. A detailed root cause analysis will be shared as soon as it is available.

Posted Oct 01, 2026 - 17:56 UTC

## Update

GitHub Actions experienced degraded performance for some hosted runners due to throttling within an upstream Azure dependency. Service capacity has recovered, and we are continuing to monitor while working with Azure on the underlying condition.

Posted Oct 01, 2026 - 17:50 UTC

## Update

We are currently applying a mitigation and anticipate recovery within thirty minutes.

Posted Oct 01, 2026 - 16:48 UTC

## Update

We have identified an issue with our upstream provider which is causing Actions requests to 429 which is creating the delays. We have escalated to the owning team and are investigating how to mitigate the 429s.

Posted Oct 01, 2026 - 16:10 UTC

## Update

We are seeing a reoccurrence in run-start delays, and are continuing to investigate to issue. Customers will potentially experience delays of up to ten minutes.

Posted Oct 01, 2026 - 15:29 UTC

## Investigating

Actions is experiencing degraded performance. We are continuing to investigate.

Posted Oct 01, 2026 - 15:20 UTC

## Monitoring

The degradation affecting Actions has been mitigated. We are monitoring to ensure stability.

Posted Oct 01, 2026 - 15:10 UTC

## Update

Run-start delays on Ubuntu runners have been resolved. We are investigating run-start delays on Windows runners and will share more information as it becomes available.

Posted Oct 01, 2026 - 15:02 UTC

## Update

We have identified the cause of increased Actions run start delays on Ubuntu runners and are actively deploying a fix. Customers may continue to experience intermittent delays while the mitigation rolls out and service metrics return to normal.

Posted Oct 01, 2026 - 14:54 UTC

## Investigating

We are investigating reports of degraded performance for Actions

Posted Oct 01, 2026 - 14:47 UTC

This incident affected: Actions.
