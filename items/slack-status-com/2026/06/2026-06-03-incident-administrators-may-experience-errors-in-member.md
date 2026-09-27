---
title: 'Incident: Administrators may experience errors in member management.'
link: https://slack-status.com/2026-06/898527c2a0f5c9be
source: slack-status-com
published: 2026-06-03T16:33:16Z
updated: 2026-06-03T17:35:07Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The Slack engineering team identified the cause as a recent update that introduced a code change which caused the invite modal to fail when accessed from the Admin page. We rolled back the change and have confirmed the fix is fully deployed and the issue is resolved.Some workspace Admins were unable to add new members to their workspaces via the Manage Members Admin page. In addition, some users attempting to invite people from this page may have encountered an error or been unable to run macros on their workspaces.Workspace Admins should now be able to add new members via the Manage Members admin page. A page refresh may be required.We apologize for the disruption to your work.
content: extracted
html: 2026-06-03-incident-administrators-may-experience-errors-in-member.html
preview:
  file: 2026-06-03-incident-administrators-may-experience-errors-in-member.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-06-03-incident-administrators-may-experience-errors-in-member.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-06-03-incident-administrators-may-experience-errors-in-member.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-06-03-incident-administrators-may-experience-errors-in-member.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

The Slack engineering team identified the cause as a recent update that introduced a code change which caused the invite modal to fail when accessed from the Admin page. We rolled back the change and have confirmed the fix is fully deployed and the issue is resolved.

Some workspace Admins were unable to add new members to their workspaces via the Manage Members Admin page. In addition, some users attempting to invite people from this page may have encountered an error or been unable to run macros on their workspaces.

Workspace Admins should now be able to add new members via the Manage Members admin page. A page refresh may be required.

We apologize for the disruption to your work.

 10:35 AM PST

 Cause Identified

We have identified the cause of the issue preventing some workspace admins from adding new members via the Admin page as a recent change and are in the process of rolling back this change.

**Current impact to end users**: Some users may encounter errors when attempting to add or manage members, send invites, or run macros on their workspace.

**Known workaround:** Inviting members via the Slack client workspace menu (workspace name → Invite people to workspace) is currently working as expected.

We'll provide an update within 30 minutes or sooner if additional information becomes available.

 10:12 AM PST

 Investigating

We're looking into an issue where some users may be unable to add new members, send invites, or run macros on their workspaces. We're on the case and will provide another update as soon as we have more details to share. We apologize for any disruption to your work.

**Current impact to end users:** Some users may encounter errors when attempting to add or manage members, send invites, or run macros on their workspace.

We'll provide an update within 30 minutes or sooner if additional information becomes available.

 9:49 AM PST

 Investigating

We're investigating a new incident.

- **Current impact to end users:** Some administrators may be experiencing errors managing members.

We'll provide an update within 30 minutes including further details on current actions being taken.

 9:33 AM PST
