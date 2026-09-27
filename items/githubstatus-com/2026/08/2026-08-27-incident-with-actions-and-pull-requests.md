---
title: Incident with Actions and Pull Requests
link: https://www.githubstatus.com/incidents/kfspvrz14xr0
source: githubstatus-com
published: 2026-08-27T00:26:44Z
updated: 2026-08-28T22:10:28Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-27-incident-with-actions-and-pull-requests.html
preview:
  file: 2026-08-27-incident-with-actions-and-pull-requests.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-27-incident-with-actions-and-pull-requests.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-27-incident-with-actions-and-pull-requests.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On August 26, 2026, from 21:55 UTC to 23:58 UTC, 2.6% of workflow runs triggered by pull request events were delayed, with the impact rising as high as 25% at its peak. Some users also experienced delays in pull request merge-commit generation, mergeability information, and merge-button availability. Actions and Pull Requests fully recovered by 23:58 UTC; the incident was resolved at 00:26 UTC after normal operation was confirmed.

Background jobs that process pull request updates and generate merge commits were impacted by timeouts reaching a single partition of git data. This resulted in a backlog in pull request merge-commit processing, delaying pull request-triggered GitHub Actions workflows and some mergeability information.

We reduced workload, shifted traffic away from affected infrastructure, and restored the affected service component to a healthy state. Together, these actions helped drain the backlog and restore normal operations.

We are working to improve resource saturation detection and to eliminate customer impact in this scenario by isolating impact, placing better bounds on retries, and strengthening backpressure to make our systems more resilient under load.

Posted Aug 27, 2026 - 00:26 UTC

## Monitoring

The degradation affecting Actions and Pull Requests has been mitigated. We are monitoring to ensure stability.

Posted Aug 27, 2026 - 00:26 UTC

## Update

We confirmed full recovery beginning at 23:58 UTC. Actions workflow runs and pull request merges are operating normally. We will now resolve the incident while continuing to monitor service health.

Posted Aug 27, 2026 - 00:25 UTC

## Update

We've applied mitigations and are seeing recovery in Actions workflow runs and blocked pull request merges. We're continuing to monitor for sustained health of merge commit creates before resolving.

Posted Aug 27, 2026 - 00:01 UTC

## Update

We are investigating elevated delays and timeouts affecting Actions workflow runs triggered by pull request events. 20% of actions runs have delayed starts of more than 5 minutes and up to 4% of runs failed to trigger. We are actively working on mitigation and will provide updates as we learn more.

Posted Aug 26, 2026 - 22:57 UTC

## Investigating

We are investigating reports of degraded performance for Actions and Pull Requests

Posted Aug 26, 2026 - 22:56 UTC

This incident affected: Pull Requests and Actions.
