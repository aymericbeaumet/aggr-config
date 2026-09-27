---
title: Actions delays in starting runs
link: https://www.githubstatus.com/incidents/lyppgxbq1nyk
source: githubstatus-com
published: 2026-08-24T14:34:42Z
updated: 2026-08-25T01:36:32Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-24-actions-delays-in-starting-runs.html
preview:
  file: 2026-08-24-actions-delays-in-starting-runs.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-24-actions-delays-in-starting-runs.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-24-actions-delays-in-starting-runs.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On August 24, 2026, between 13:33 UTC and 14:04 UTC, 3.8% of Actions runs experienced start delays over 5 minutes with 1.25% of Actions runs failing outright.

The incident was caused by a disk failure on a node hosting one of many service instances responsible for processing runner assignment events. Typically, pods on unhealthy nodes are removed and replaced automatically without impact. In this case, although the node was severely degraded and unable to perform disk operations, it continued sending healthy signals, preventing the system from immediately moving its work elsewhere. During this period, events assigned to the affected component accumulated until an automatic rebalance redirected processing to healthy components at 13:54 UTC. The queue backlog was cleared at 14:00 UTC, and processing returned to normal by 14:04 UTC.

To prevent a recurrence, we are improving detection and automated remediation for unhealthy nodes that aren’t fully offline. We are also strengthening application-level resiliency, so stalled consumers are automatically removed quickly and their work reassigned without waiting for the affected node to recover.

Posted Aug 24, 2026 - 14:34 UTC

## Monitoring

The degradation affecting Actions has been mitigated. We are monitoring to ensure stability.

Posted Aug 24, 2026 - 14:26 UTC

## Update

Failures while queuing and running Actions jobs for a subset of customers are now resolving. We are monitoring for full recovery.

Posted Aug 24, 2026 - 14:22 UTC

## Investigating

We are investigating reports of degraded performance for Actions

Posted Aug 24, 2026 - 13:56 UTC

This incident affected: Actions.
