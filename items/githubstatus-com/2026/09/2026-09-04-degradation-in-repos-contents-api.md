---
title: Degradation in repos contents API
link: https://www.githubstatus.com/incidents/r05vk75c594j
source: githubstatus-com
published: 2026-09-04T22:23:34Z
updated: 2026-09-15T23:34:38Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-04-degradation-in-repos-contents-api.html
preview:
  file: 2026-09-04-degradation-in-repos-contents-api.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-04-degradation-in-repos-contents-api.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-04-degradation-in-repos-contents-api.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On September 4, 2026, between approximately 21:45 and 22:07 UTC, some users experienced errors and elevated latency for repository operations. The incident was fully resolved at 22:23 UTC.

The cause was a capacity change that spread one of our clusters across additional availability zones; our zone-aware traffic routing kept sending requests to the original zone for performance, overloading a small set of servers while the new capacity sat idle. We resolved the incident by reverting the change and letting traffic rebalance.

We are improving per-zone capacity guarantees, cross-zone load-shedding, and pre-production testing of multi-zone changes to prevent recurrence.

Posted Sep 04, 2026 - 22:23 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Sep 04, 2026 - 22:02 UTC
