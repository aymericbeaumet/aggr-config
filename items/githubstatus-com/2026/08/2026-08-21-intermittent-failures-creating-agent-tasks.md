---
title: Intermittent failures creating agent tasks
link: https://www.githubstatus.com/incidents/bhbcjn4n3jzp
source: githubstatus-com
published: 2026-08-21T00:37:20Z
updated: 2026-08-25T16:34:14Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-21-intermittent-failures-creating-agent-tasks.html
preview:
  file: 2026-08-21-intermittent-failures-creating-agent-tasks.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-21-intermittent-failures-creating-agent-tasks.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-21-intermittent-failures-creating-agent-tasks.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

Between 13:57 UTC on August 20 and 00:37 UTC on August 21, 2026, some users of the Copilot Cloud Agent experienced delays of up to 60 to 90 minutes in seeing the status and results of their agent tasks. The agent tasks themselves continued to run and complete during this time; only the visibility of their status was delayed.

The cause was a regional outage in a third-party cloud database service that Copilot uses to store agent task status. We failed over the affected database to a healthy region, added processing capacity to work through the backlog, and restored normal operation once the underlying service recovered. No task data was lost during the incident.

To prevent repetition of similar incidents, we are removing the database configuration that made us vulnerable to this regional outage and improving our database failover procedures.

Posted Aug 21, 2026 - 00:37 UTC

## Update

We are seeing gradual recovery in Copilot Cloud Agent task status visibility as we deploy a fix for the root cause. Session output remains delayed by approximately one hour while remediation continues.

Posted Aug 20, 2026 - 20:37 UTC

## Update

We are continuing to observe gradual recovery for Copilot Cloud Agent task status visibility. Session output continues to be delayed by approximately 1 hour as our remediation steps take effect.

Posted Aug 20, 2026 - 19:35 UTC

## Update

We are continuing to observe gradual recovery for Copilot Cloud Agent task status visibility, with session output delayed by approximately 1 hour. We have taken additional steps to accelerate the recovery and expect this to take effect within the next hour.

Posted Aug 20, 2026 - 18:45 UTC

## Update

We are continuing to observe gradual recovery for Copilot Cloud Agent task status visibility, with session output delayed by approximately 1 hour. We have taken additional steps to accelerate the recovery and are continuing to monitor the impact.

Posted Aug 20, 2026 - 18:04 UTC

## Update

We are observing gradual recovery for Copilot Cloud Agent task status visibility, with session output delayed approximately 1 hour. We have taken additional steps to accelerate the recovery and are continuing to monitor the impact.

Posted Aug 20, 2026 - 17:32 UTC

## Update

We are seeing signs of recovery for Copilot Cloud Agent task status visibility, but this recovery is slower than anticipated. We are pursuing additional mitigating measures to accelerate recovery.

Posted Aug 20, 2026 - 17:05 UTC

## Update

Users are experiencing delays when starting tasks using Copilot Cloud Agent and are not be able to see the status of these tasks. Copilot Cloud Agent tasks are still being completed. We have identified the cause of the issue and are putting mitigations in place to return service to normal levels. We will provide another update about the expected recovery time shortly.

Posted Aug 20, 2026 - 16:14 UTC

## Update

We are experiencing issues with Copilot Cloud Agent tasks, resulting in newly started tasks not properly displaying on-going progress. These Copilot Cloud Agent tasks are still being completed correctly but lack proper visibility. We are actively investigating the issue and will provide updates as we learn more.

Posted Aug 20, 2026 - 15:41 UTC

## Update

We have identified the problematic component and are working to fail over to a healthy instance. Further updates will be provided as we perform mitigations.

Posted Aug 20, 2026 - 15:01 UTC

## Update

Users may experience delays when starting tasks using Copilot Cloud Agent. We are actively investigating the issue and will provide updates as we learn more.

Posted Aug 20, 2026 - 14:51 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Aug 20, 2026 - 14:43 UTC
