---
title: 'Incident: Trouble adding multiple users to channels'
link: https://slack-status.com/2026-06/767f9891b3838e3f
source: slack-status-com
published: 2026-06-09T15:24:39Z
updated: 2026-06-10T21:43:02Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: We've resolved an issue that could cause incorrect email address tokens to appear when users pasted multiple email addresses into the member invite flow. In some cases, pasted email addresses were incorrectly matched to existing members with similar email addresses, requiring users to manually correct the entries before proceeding.We identified the cause of the issue and deployed a fix to ensure email addresses are now matched correctly when pasted into the invite flow. The issue is resolved for all affected users.If you're still experiencing any trouble, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). We apologize for the disruption and appreciate your patience.
content: extracted
html: 2026-06-09-incident-trouble-adding-multiple-users-to-channels.html
preview:
  file: 2026-06-09-incident-trouble-adding-multiple-users-to-channels.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-06-09-incident-trouble-adding-multiple-users-to-channels.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-06-09-incident-trouble-adding-multiple-users-to-channels.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-06-09-incident-trouble-adding-multiple-users-to-channels.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

We've resolved an issue that could cause incorrect email address tokens to appear when users pasted multiple email addresses into the member invite flow. In some cases, pasted email addresses were incorrectly matched to existing members with similar email addresses, requiring users to manually correct the entries before proceeding.

We identified the cause of the issue and deployed a fix to ensure email addresses are now matched correctly when pasted into the invite flow. The issue is resolved for all affected users.

If you're still experiencing any trouble, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). We apologize for the disruption and appreciate your patience.

 2:43 PM PST

 Update

We've deployed a fix for the issue and are monitoring to confirm that the issue has been resolved.

We'll provide another update as soon as more information becomes available.

 7:35 AM PST

 Update

We’ve identified the cause of the issue and we’re validating a fix to ensure it fully resolves the issue before we deploy it.\
We'll provide further update as soon as more information becomes available.

 4:44 AM PST

 Update

We’re working to reproduce the issue. This will help us identify the underlying cause and provide a resolution.

We'll provide further update as soon as more information becomes available.

 3:35 AM PST

 Investigating

We've received reports that a subset of users may continue to experience issues when attempting to add multiple users to channels using email addresses. These users may encounter a "Multiple matches" error, preventing the intended user from being identified and added to the channel.\
We’re actively investigating these reports to determine whether additional remediation is required. We'll provide further updates as more information becomes available.

 2:47 AM PST

 Resolved

This issue is now resolved for all users.

- **Current status and recent actions taken:** We identified the trigger for this issue and a fix was deployed. After a period of monitoring, we have concluded that there is no further impact to users.
- **Previous impact to end users:** Some users may have experienced issues when attempting to add multiple users to channels and received an error indicating a failure to identify team members. This could have caused subsequent workflows not to trigger.
- **Required resolution steps:** No further action is necessary.

Thanks for sticking with us as we resolved this. We apologize for any disruptions to your day.

 1:18 PM PST

 Update

Our work on this issue is still ongoing.

- **Current status and actions being taken**: We have identified the trigger for this issue and a fix is in the process of deploying. The progress is being monitored and we’ll provide an update as soon as the fix is completely deployed and verified.

We'll be back with another update once we have news to share.

 10:39 AM PST

 Investigating

Some users may experience issues when attempting to add multiple users to channels. Multiple matches error may appear along with a failure to identify team members. This may cause subsequent workflows not to trigger. We're investigating and will let you know as soon as we know more.

 8:24 AM PST
