---
title: 'Incident: Issue sending messages to channels via certain Workflows'
link: https://slack-status.com/2026-07/e82ee127d8bed735
source: slack-status-com
published: 2026-07-24T08:22:54Z
updated: 2026-07-24T10:47:02Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: We’ve completed the revert of the recent change and have validated that this has brought customers out of impact and can now successfully send a message to a channel using Workflows that include 'refer to a message link'.We apologize for how this incident may have affected you and your business.
content: extracted
html: 2026-07-24-incident-issue-sending-messages-to-channels-via-certain.html
preview:
  file: 2026-07-24-incident-issue-sending-messages-to-channels-via-certain.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-07-24-incident-issue-sending-messages-to-channels-via-certain.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/TableIncident@2x.png
  original:
    file: 2026-07-24-incident-issue-sending-messages-to-channels-via-certain.image-4d3ed48ec52c.png
    width: 36
    height: 36
  variants:
  - file: 2026-07-24-incident-issue-sending-messages-to-channels-via-certain.image-1b833f24fef5.webp
    width: 36
    height: 36
  color: '#e2ac37'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-07-24-incident-issue-sending-messages-to-channels-via-certain.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-07-24-incident-issue-sending-messages-to-channels-via-certain.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![](https://slack-status.com/img/v2/TableIncident@2x.png)

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

We’ve completed the revert of the recent change and have validated that this has brought customers out of impact and can now successfully send a message to a channel using Workflows that include 'refer to a message link'.

We apologize for how this incident may have affected you and your business.

 3:47 AM PST

 Update

We’ve performed a test of reverting a recent change and this appears to have resolved the issue. We are in the process of performing a production wide revert of this change. This is expected to take some time to complete and we will validate that it is successful.

We’ll provide an update in 2 hours or sooner if more information becomes available.

 2:42 AM PST

 Update

The Workflow team is currently analyzing whether a recent change may have resulted in the conflict between variables that has been observed. It is expected to take some time to validate whether this is the case.

We’ll provide an update in 60 minutes or sooner if more information becomes available.

 2:08 AM PST

 Update

We've engaged our Worklow team to assist in the investigation. Initial analysis appears to show a conflict between specific variables. We’re continuing to work to understand whether this is the trigger of the issue. We’ll provide an update in 30 minutes or sooner if more information becomes available.

 1:44 AM PST

 Investigating

We’re aware of an issue affecting Workflows that include 'refer to a message link'. Users with these workflows are unable to send a message to a channel, and will receive an ‘Input Validation Error’ when trying to perform this step. We’re actively investigating this issue and will provide an update in 30 minutes or sooner if more information becomes available.

 1:22 AM PST

Features affected

Workflows

Status

Incident
