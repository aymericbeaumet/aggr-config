---
title: Degraded Git Operations over SSH
link: https://www.githubstatus.com/incidents/wms44hv62t3p
source: githubstatus-com
published: 2026-08-21T14:00:00Z
updated: 2026-08-24T07:19:23Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-21-degraded-git-operations-over-ssh.html
preview:
  file: 2026-08-21-degraded-git-operations-over-ssh.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-21-degraded-git-operations-over-ssh.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-21-degraded-git-operations-over-ssh.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On August 21, 2026, between 14:00 and 14:07 UTC, dotcom Git operations over SSH were degraded. Successful Git operations over SSH fell by more than 95% for during the peak impact window, making clone, fetch, or push over SSH effectively unavailable to most users for approximately four minutes. Git operations over HTTPS were not affected.

The incident was caused by a software defect in our load-balancing infrastructure that was triggered by a configuration change. The defect only occurred when connections passed through multiple layers of load balancers running the new configuration, which meant it was not detected during canary testing.

We mitigated the incident by rolling back the configuration change.

We are adding regression coverage for multi-layer load-balancer configurations and improving monitoring and alerting for Git operations over SSH to reduce our time to detection and mitigation of similar issues in the future.

Posted Aug 21, 2026 - 14:00 UTC
