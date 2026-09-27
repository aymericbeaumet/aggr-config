---
title: 'Incident: Scheduled Grid Migrations are experiencing extended downtime'
link: https://slack-status.com/2026-05/02b2d5b45d656512
source: slack-status-com
published: 2026-05-08T12:13:46Z
updated: 2026-05-08T17:44:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'This issue is now resolved.Current status and actions being taken: We identified database write throttling as the cause of the stalled grid migrations. We applied a drop rule for jobs that were stuck in a retry loop, which has restored normal migration processing. Scope: A small number of enterprise users undergoing grid migrations experienced extended delays in accessing Slack. Current impact to end users: Affected users may have experienced an inability to access Slack during their migration window. Known workarounds: No workarounds needed at this time.We apologize for how this incident affected you and your business.'
content: extracted
html: 2026-05-08-incident-scheduled-grid-migrations-are-experiencing.html
preview:
  file: 2026-05-08-incident-scheduled-grid-migrations-are-experiencing.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-05-08-incident-scheduled-grid-migrations-are-experiencing.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/TableIncident@2x.png
  original:
    file: 2026-05-08-incident-scheduled-grid-migrations-are-experiencing.image-4d3ed48ec52c.png
    width: 36
    height: 36
  variants:
  - file: 2026-05-08-incident-scheduled-grid-migrations-are-experiencing.image-1b833f24fef5.webp
    width: 36
    height: 36
  color: '#e2ac37'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-05-08-incident-scheduled-grid-migrations-are-experiencing.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-05-08-incident-scheduled-grid-migrations-are-experiencing.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![](https://slack-status.com/img/v2/TableIncident@2x.png)

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

This issue is now resolved.

- Current status and actions being taken: We identified database write throttling as the cause of the stalled grid migrations. We applied a drop rule for jobs that were stuck in a retry loop, which has restored normal migration processing.
- Scope: A small number of enterprise users undergoing grid migrations experienced extended delays in accessing Slack.
- Current impact to end users: Affected users may have experienced an inability to access Slack during their migration window.
- Known workarounds: No workarounds needed at this time.

We apologize for how this incident affected you and your business.

 10:44 AM PST

 Investigating

Users who are undergoing scheduled Grid Migrations are experiencing extended downtime. Downtime is expected during a Grid Migration however it's taking longer than usual. During the downtime users are unable to access Slack until the migration completes. We've identified an issue with Job Queues, which may be the cause and are investigating this further. The next update will be provided once additional information becomes available.

 5:13 AM PST

Features affected

Connectivity

Status

Incident
