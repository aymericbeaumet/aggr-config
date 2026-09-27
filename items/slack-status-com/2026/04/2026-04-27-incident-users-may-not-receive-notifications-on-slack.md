---
title: 'Incident: Users May Not Receive Notifications on Slack Thread Replies'
link: https://slack-status.com/2026-04/22195f9f2ca47237
source: slack-status-com
published: 2026-04-27T17:11:53Z
updated: 2026-04-27T20:36:02Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: We’ve successfully performed a rollback in the production environment, and all systems are now functioning as expected. Users should no longer experience any issues with thread replies not generating a notification banner.We apologize for any inconvenience and appreciate your patience.
content: extracted
html: 2026-04-27-incident-users-may-not-receive-notifications-on-slack.html
preview:
  file: 2026-04-27-incident-users-may-not-receive-notifications-on-slack.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-04-27-incident-users-may-not-receive-notifications-on-slack.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/TableIncident@2x.png
  original:
    file: 2026-04-27-incident-users-may-not-receive-notifications-on-slack.image-4d3ed48ec52c.png
    width: 36
    height: 36
  variants:
  - file: 2026-04-27-incident-users-may-not-receive-notifications-on-slack.image-1b833f24fef5.webp
    width: 36
    height: 36
  color: '#e2ac37'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-04-27-incident-users-may-not-receive-notifications-on-slack.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-04-27-incident-users-may-not-receive-notifications-on-slack.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![](https://slack-status.com/img/v2/TableIncident@2x.png)

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

We’ve successfully performed a rollback in the production environment, and all systems are now functioning as expected. Users should no longer experience any issues with thread replies not generating a notification banner.

We apologize for any inconvenience and appreciate your patience.

 1:36 PM PST

 Cause Identified

We’re observing positive signals after rolling back the deployment in test environments. We will complete some final validations before rolling back the deployment in the production environment.

Thank you for your patience as we work through this one. We’ll report back once production rollback is completed or if there are substantial updates to share.

 11:07 AM PST

 Investigating

Some users have reported issues with thread replies not generating a notification banner. We’re investigating the issue, and have identified a recent deployment as a possible trigger.

We apologize for the inconvenience and will report back as more information becomes available.

 10:11 AM PST

Features affected

Notifications

Status

Incident
