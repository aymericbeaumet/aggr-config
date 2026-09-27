---
title: 'Incident: Slow behaviour in Slack'
link: https://slack-status.com/2026-05/bedf971f90be70a6
source: slack-status-com
published: 2026-05-04T10:00:38Z
updated: 2026-05-04T17:37:54Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'This issue is now resolved for all users.Current status and recent actions taken: A fix has been deployed that resolved the underlying cause of this issue. Our health metrics have returned to normal levels and workflows are running as expected.Scope: Users in the EU region — primarily teams based in Central Europe — experienced elevated workflow failure rates.Previous impact to end users: Users may have experienced workflows failing to run, with the first step remaining stuck in an initialized state. Some users in the EU region may also have experienced general slowness when using Slack.Required resolution steps: No steps are required. Workflows should be running normally. If you continue to experience any trouble, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux).We apologize for any disruptions this may have caused to your day.'
content: extracted
html: 2026-05-04-incident-slow-behaviour-in-slack.html
preview:
  file: 2026-05-04-incident-slow-behaviour-in-slack.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-05-04-incident-slow-behaviour-in-slack.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-05-04-incident-slow-behaviour-in-slack.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-05-04-incident-slow-behaviour-in-slack.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

This issue is now resolved for all users.

- **Current status and recent actions taken:** A fix has been deployed that resolved the underlying cause of this issue. Our health metrics have returned to normal levels and workflows are running as expected.
- **Scope**: Users in the EU region — primarily teams based in Central Europe — experienced elevated workflow failure rates.
- **Previous impact to end users**: Users may have experienced workflows failing to run, with the first step remaining stuck in an initialized state. Some users in the EU region may also have experienced general slowness when using Slack.
- **Required resolution steps**: No steps are required. Workflows should be running normally. If you continue to experience any trouble, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux).

We apologize for any disruptions this may have caused to your day.

 10:37 AM PST

 Update

A fix has been deployed to address the elevated latency in EU backend services. The team is monitoring the environment to ensure service stability and full recovery. We'll provide another update as soon as possible.

 6:08 AM PST

 Update

The investigation identified elevated latency in EU backend services as the cause of workflows not completing. A subset of workflow trigger attempts is impacted, primarily, EU users. Pod restarts have stopped, and error rates are improving. A fix is being prepared and will be deployed shortly. We'll provide another update as soon as possible.

 4:43 AM PST

 Investigating

Some users are experiencing issues with workflows. Triggered workflows may fail to complete, with the first step not executing. We'll provide another update as soon as possible.

 3:00 AM PST
