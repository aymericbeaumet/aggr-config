---
title: Disruption with Copilot Code Review
link: https://www.githubstatus.com/incidents/zw1hbx2yyhvr
source: githubstatus-com
published: 2026-09-04T22:26:46Z
updated: 2026-09-09T17:07:58Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-04-disruption-with-copilot-code-review.html
preview:
  file: 2026-09-04-disruption-with-copilot-code-review.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-04-disruption-with-copilot-code-review.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-04-disruption-with-copilot-code-review.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On September 4, 2026, between 20:04 and 22:26 UTC, GitHub Copilot code review experienced an increased failure rate. Affected pull request reviews failed to complete or post review comments.

The incident was caused by a change to the service’s authentication permissions that prevented it from submitting affected reviews to the GitHub API. We reverted the change and restored normal operation by 22:26 UTC.

We apologize for the disruption.

Posted Sep 04, 2026 - 22:26 UTC

## Monitoring

The degradation has been mitigated. We are monitoring to ensure stability.

Posted Sep 04, 2026 - 22:25 UTC

## Update

We are applying the mitigation and expect recovery within approximately 30 minutes.

Posted Sep 04, 2026 - 21:54 UTC

## Update

Some users may be experiencing failures when using Copilot code review. We have identified the root cause and are working on a mitigation.

Posted Sep 04, 2026 - 20:57 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Sep 04, 2026 - 20:39 UTC
