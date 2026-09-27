---
title: Incident with Actions
link: https://www.githubstatus.com/incidents/y1t7p9fzrlj2
source: githubstatus-com
published: 2026-08-26T18:01:30Z
updated: 2026-08-27T02:24:56Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-26-incident-with-actions.html
preview:
  file: 2026-08-26-incident-with-actions.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-26-incident-with-actions.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-26-incident-with-actions.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On August 26, 2026 from 15:02 to 15:45 UTC, Actions jobs failed to start. The following 2 hours until 17:40 UTC, Actions runs were delayed starting by more than 5 minutes as the system caught up with delayed load. This impact was triggered by saturation of writes to the database primary used by the service processing triggers for Actions workflows. The primary was failed over, but the system did not fully recover. The saturation was caused by growing daily peak load combined with an upstream issue in GitHub’s event processing infrastructure, [https://www.githubstatus.com/incidents/hcbtzksccj2f](https://www.githubstatus.com/incidents/hcbtzksccj2f), which caused burst amplification of already-high load. Downstream throttles that were later used to recover were set ~10% too high to protect the system.

At 15:45 UTC, throttling combined with service restarts recovered the service’s core health. Those throttles were gradually raised between 15:54 and 17:22 to restore full webhook processing for Actions runs. This ramp was deliberately slow to ensure we did not re-overwhelm the system given our original throttling was now known to be incorrectly set. The queue of webhook events was fully burned down at 17:40 UTC.

3.7% of larger-runner jobs, along with some scale-set self-hosted jobs, remained stuck in queued or “waiting for runner” state. We deployed a change to force-revoke jobs in this state, and they transitioned to failed at 18:40 UTC, about 50 minutes after incident mitigation. Releasing these jobs also freed hosted concurrency for larger-runner jobs.

Customers using concurrency groups saw longer impact due to a separate issue where runners assigned to a subset of jobs disconnected before the force-revoke mitigation was deployed, which prevented runner acquisition from progressing and left jobs in a waiting-for-runner state. This was resolved at 01:00 UTC on August 27.

Some runs triggered during the 15:02-15:45 UTC incident window encountered a bug that left them showing as queued even after service recovery. In the backend, these runs had already failed and will automatically move to canceled state 24 hours after creation. As follow-up, we are fixing the root cause of this queued state and improving our ability to bulk-cancel affected runs.

Several changes to improve the general scalability of this part of Actions were already complete and deploying to production. Rollout of those changes will be complete within the next 24 hours. Further work to improve scale, resiliency, and more graceful degradation of Actions workflows are in flight. We are also taking a repair item to accelerate clearing of stuck queued or waiting jobs in similar future cases.

Posted Aug 26, 2026 - 18:01 UTC

## Update

All inbound queues have recovered and Actions is operating as expected. 3.7% of jobs assigned to larger runners during the early stage of this incident are stuck waiting for runner assignment. Those will be canceled within the hour. Other runners are successfully processing all new jobs.

Posted Aug 26, 2026 - 18:00 UTC

## Monitoring

The degradation affecting Actions has been mitigated. We are monitoring to ensure stability.

Posted Aug 26, 2026 - 17:54 UTC

## Update

We are continuing to observe recovery and expect actions inbound queues to be back to normal in <30min. Work will continue to flow through the system subject to per-customer concurrency limits.

Posted Aug 26, 2026 - 17:32 UTC

## Update

We are continuing to observe recovery and delayed queues are burning down. Some customers will continue to see increased delays until all throttled work has been completed - we expect this within the next hour.

Posted Aug 26, 2026 - 16:50 UTC

## Update

Pages is operating normally.

Posted Aug 26, 2026 - 16:49 UTC

## Update

We believe we've identified and addressed the issue and are ramping traffic back up slowly to ensure it doesn't recur. Some customers will continue to see delays as we ramp up.

Posted Aug 26, 2026 - 16:14 UTC

## Update

primary failover briefly improved performance but did not fully mitigate, we've throttled inbound traffic and are investigating upstream Vitess issues

Posted Aug 26, 2026 - 15:48 UTC

## Update

We've identified an issue with a database primary and are failing over to a replica immediately

Posted Aug 26, 2026 - 15:23 UTC

## Update

Pages is experiencing degraded performance. We are continuing to investigate.

Posted Aug 26, 2026 - 15:12 UTC

## Investigating

We are investigating reports of degraded availability for Actions

Posted Aug 26, 2026 - 15:11 UTC

This incident affected: Actions and Pages.
