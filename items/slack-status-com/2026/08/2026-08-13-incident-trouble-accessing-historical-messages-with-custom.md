---
title: 'Incident: Trouble Accessing Historical Messages With Custom Data Retention Policies Enabled'
link: https://slack-status.com/2026-08/e07b9271f0346c3b
source: slack-status-com
published: 2026-08-13T21:21:41Z
updated: 2026-09-14T22:31:02Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: We've completed restoring shared files for all remaining affected workspaces and restored and verified all affected data. If you still can't see previous content, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). Thank you for your patience while we sorted out this issue.
content: extracted
html: 2026-08-13-incident-trouble-accessing-historical-messages-with-custom.html
preview:
  file: 2026-08-13-incident-trouble-accessing-historical-messages-with-custom.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-08-13-incident-trouble-accessing-historical-messages-with-custom.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-08-13-incident-trouble-accessing-historical-messages-with-custom.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-08-13-incident-trouble-accessing-historical-messages-with-custom.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

We've completed restoring shared files for all remaining affected workspaces and restored and verified all affected data. If you still can't see previous content, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). Thank you for your patience while we sorted out this issue.

 3:31 PM PST

 Update

We’ve completed message data restoration across affected teams and channels, and are now focused on restoring file shares which should be the last step towards resolution.

We'll provide another update as soon as new information becomes available.

 8:53 AM PST

 Update

We've made further progress restoring message data across affected teams and channels, with most batches now complete. We've deployed a tool to correct thread reply links affected during the restore process and are running it now. Slack Connect message restoration is resuming production runs, with results being closely monitored.  We'll provide an update as soon as new information becomes available.

 7:37 PM PST

 Update

We’ve made further progress restoring messages, message edits, and shared files affected by this issue. We identified and resolved a technical limit that was slowing recovery for some accounts, and recovery for Slack Connect workspaces is now underway. Message edit restoration is nearing completion for affected workspaces. We’re continuing to validate restored data as recovery progresses.

We’ll provide another update as significant progress is made.

 10:00 AM PST

 Update

We continue to restore batches of messages, message edits, and shared files. Each batch requires careful planning, monitoring, and validation to ensure the affected messages are accurately restored. Custom retention processing remains re-enabled for affected active workspaces. Thank you for bearing with us while we continue to work through this complex and nuanced issue.

More updates will come as restoration and validation progress.

 3:51 PM PST

 Update

We’re continuing our work to restore the remaining message data affected by this issue. We’ve made further progress across the recovery processes for messages, message edits, and shared files. Validation of the message-edit recovery process has identified additional adjustments needed before restoration proceeds, while preparation and validation for message recovery continue. We’ve also completed additional development work supporting shared-file restoration, including handling for shared channels.

Custom retention processing has been re-enabled for affected active workspaces. We’ll continue to provide updates as restoration and validation progress.

 11:58 AM PST

 Update

We’re continuing our work to restore the remaining message data affected by this issue. Our teams have made progress developing and validating the recovery process for messages, message edits, and shared files. We’re addressing additional technical dependencies and completing validation before proceeding with the next stages of restoration.

We’ll continue to provide updates as we make progress.

 1:55 PM PST

 Update

We're continuing to work on restoring messages, message edits, and file shares for a limited number of workspaces that were affected by an issue with custom data retention settings. We've fixed what triggered the issue and are running data recovery using backup restoration methods. Custom retention settings have been restored for impacted channels, and we're gradually re-enabling retention jobs.

We will provide another update as additional information becomes available.

 3:54 PM PST

 Update

We're continuing our restoration efforts regarding this issue affecting a subset of channels with custom message retention settings. While we work to resolve this issue, data retention processing remains paused for affected customers.

We will provide another update as additional information becomes available.

 2:57 PM PST

 Update

We're continuing to investigate an issue affecting a subset of channels with custom message retention settings. Custom channel retention settings have been restored for affected customers. Custom retention jobs remain on hold for some customers as we work to fully resolve the underlying cause regarding the custom message retention issues.

 2:39 PM PST

 Investigating

After further investigation, the custom retention settings have not fully been restored yet. Our teams have identified the technical trigger and are currently developing a mitigation plan to mitigate the impact to the custom retention settings. All custom retention jobs have been put on hold as the team works to address this issue. We apologize for the confusion, and thank you for bearing with us while we work through this issue.

 1:20 PM PST

 Investigating

We're continuing to investigate an issue affecting a subset of channels with custom message retention settings. Our Teams have restored functionality to the affected retention settings for known impacted customers. We are now working to identify and address any additional concerns regarding the message retention issues.

 12:35 PM PST

 Investigating

We're continuing to investigate an issue affecting a subset of channels with custom message retention settings. Our teams are working to identify the technical trigger and an appropriate fix. We'll provide an update as more news becomes available.

 2:49 PM PST

 Investigating

Users with custom data retention policies enabled might be unable to see or load older messages. We apologize for the trouble and we'll share more news as it becomes available.

 2:21 PM PST
