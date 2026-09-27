---
title: Disruption with some GitHub services
link: https://www.githubstatus.com/incidents/bk7zgdcq7s9t
source: githubstatus-com
published: 2026-09-15T20:00:50Z
updated: 2026-09-18T17:08:48Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-15-disruption-with-some-github-services-7d194cc9.html
preview:
  file: 2026-09-15-disruption-with-some-github-services-7d194cc9.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-15-disruption-with-some-github-services-7d194cc9.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-15-disruption-with-some-github-services-7d194cc9.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On September 15, 2026 between 15:30 and 20:00 UTC, some Copilot code reviews on pull requests failed to complete. The cause was increased latency in an internal caching service that GitHub Copilot Code Review relies on to coordinate its review jobs. This caused a timeout in lock acquisition, which interrupted the job. We reverted the change to the internal caching service and restored normal operation by 20:00 UTC.

We sincerely apologize for the disruption.

Posted Sep 15, 2026 - 20:00 UTC

## Monitoring

The degradation has been mitigated. We are monitoring to ensure stability.

Posted Sep 15, 2026 - 19:48 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Sep 15, 2026 - 19:11 UTC
