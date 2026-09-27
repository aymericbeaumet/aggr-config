---
title: 'Incident: Issues affecting file uploads, message edits, channel renames, and channel creations'
link: https://slack-status.com/2026-05/fe557ca05fdb64fa
source: slack-status-com
published: 2026-05-14T14:37:41Z
updated: 2026-05-14T15:34:48Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The issue affecting multiple features, including file uploads, message edits, and channel management, has been resolved. A fix has been fully deployed, and all systems are functioning normally. If you are still experiencing any trouble, please perform a hard reload of Slack by pressing Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). Mobile users should fully close and relaunch the app. We sincerely apologize for the disruption to your day and appreciate your patience while we worked to sort this out.
content: extracted
html: 2026-05-14-incident-issues-affecting-file-uploads-message-edits.html
preview:
  file: 2026-05-14-incident-issues-affecting-file-uploads-message-edits.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-05-14-incident-issues-affecting-file-uploads-message-edits.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/TableIncident@2x.png
  original:
    file: 2026-05-14-incident-issues-affecting-file-uploads-message-edits.image-4d3ed48ec52c.png
    width: 36
    height: 36
  variants:
  - file: 2026-05-14-incident-issues-affecting-file-uploads-message-edits.image-1b833f24fef5.webp
    width: 36
    height: 36
  color: '#e2ac37'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-05-14-incident-issues-affecting-file-uploads-message-edits.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-05-14-incident-issues-affecting-file-uploads-message-edits.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![](https://slack-status.com/img/v2/TableIncident@2x.png)

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

The issue affecting multiple features, including file uploads, message edits, and channel management, has been resolved. A fix has been fully deployed, and all systems are functioning normally.

If you are still experiencing any trouble, please perform a hard reload of Slack by pressing Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). Mobile users should fully close and relaunch the app.  We sincerely apologize for the disruption to your day and appreciate your patience while we worked to sort this out.

 8:34 AM PST

 Cause Identified

Our work on this issue is still ongoing.

- **Current status and actions being taken:** We've identified a potential cause related to an internal infrastructure component responsible for processing background tasks. Several nodes running this component were experiencing resource exhaustion and CrashLoopBackOff errors, causing connectivity failures to our message queue brokers. A revert of a recent configuration change is currently rolling out and we're seeing signs of improvement — 503 errors are trending down and some impacted pods are stabilizing.

We'll continue monitoring the progress. We'll be back with another update once we have news to share. Thank you for your patience.

 8:19 AM PST

 Investigating

Our work on this issue is still ongoing.

- **Current status and actions being taken**: We're investigating reports of slow loading times, errors when updating workflows, editing messages, creating or renaming channels, reactions, and issues with file uploads and third-party integrations. Some users may encounter failures when posting images or using add-ins.

We'll be back with another update once we have news to share.

 7:50 AM PST

 Investigating

We're investigating a new incident.

- **Current impact to end users:**  We’re aware of an issue affecting file uploads, message edits, channel renames, and channel creations.

We'll provide an update within 30 minutes including further details on current actions being taken.

 7:37 AM PST

Features affected

Messaging

Apps/Integrations/APIs

Files

Workflows

Status

Incident
