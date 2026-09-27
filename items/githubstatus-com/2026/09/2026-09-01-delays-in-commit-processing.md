---
title: Delays in commit processing
link: https://www.githubstatus.com/incidents/jk2dy5h9yp3m
source: githubstatus-com
published: 2026-09-01T16:01:21Z
updated: 2026-09-09T19:29:52Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-01-delays-in-commit-processing.html
preview:
  file: 2026-09-01-delays-in-commit-processing.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-01-delays-in-commit-processing.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-01-delays-in-commit-processing.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On September 1, 2026, between approximately 14:01 and 16:01 UTC, updates in response to pushes were delayed, temporarily showing stale diffs. The median time to refresh a diff after a push rose from the normal level of about 3 seconds to over 2 minutes at the peak, and more than 140,000 customer accounts had at least one delayed refresh during the most affected 75 minutes. Pushing commits and opening pull requests continued to work normally. The incident was caused by a sharp, concentrated surge in push volume that saturated worker pools and job queueing infrastructure. Autoscaling did not increase capacity as intended, so the backlog did not clear on its own.

The incident was mitigated by manually scaling the affected worker pools and increasing push-processing capacity. This allowed the system to process the backlog, after which refresh times returned to normal. To reduce the likelihood and impact of similar incidents, we are adding quotas and throttling earlier in the push path so a single concentrated source of load cannot saturate shared capacity, improving worker-pool autoscaling so capacity is added automatically, and improving monitors for background job processing so on-call is paged before customers experience delayed pull request updates.

Posted Sep 01, 2026 - 16:01 UTC

## Update

Time to update pull request diffs have improved to normal thresholds.

Posted Sep 01, 2026 - 16:00 UTC

## Update

Diffs in the PR view may be stale for several minutes. We are investigating and scaling up resources.

Posted Sep 01, 2026 - 15:00 UTC

## Investigating

We are investigating reports of degraded performance for Pull Requests

Posted Sep 01, 2026 - 15:00 UTC

This incident affected: Pull Requests.
