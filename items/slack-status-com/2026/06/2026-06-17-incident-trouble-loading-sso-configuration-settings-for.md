---
title: 'Incident: Trouble loading SSO configuration settings for some workspace admins'
link: https://slack-status.com/2026-06/66c001cc76d073f8
source: slack-status-com
published: 2026-06-17T22:33:29Z
updated: 2026-06-18T04:14:39Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: We have successfully deployed the fix to production and the issue is resolved. The Admin SSO and Authentication Configuration page for standalone workspaces should now display the correct settings. If you continue to see outdated information, refresh the SSO Admin page to load the latest changes.
content: extracted
html: 2026-06-17-incident-trouble-loading-sso-configuration-settings-for.html
preview:
  file: 2026-06-17-incident-trouble-loading-sso-configuration-settings-for.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-06-17-incident-trouble-loading-sso-configuration-settings-for.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/TableIncident@2x.png
  original:
    file: 2026-06-17-incident-trouble-loading-sso-configuration-settings-for.image-4d3ed48ec52c.png
    width: 36
    height: 36
  variants:
  - file: 2026-06-17-incident-trouble-loading-sso-configuration-settings-for.image-1b833f24fef5.webp
    width: 36
    height: 36
  color: '#e2ac37'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-06-17-incident-trouble-loading-sso-configuration-settings-for.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-06-17-incident-trouble-loading-sso-configuration-settings-for.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![](https://slack-status.com/img/v2/TableIncident@2x.png)

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

We have successfully deployed the fix to production and the issue is resolved. \
The Admin SSO and Authentication Configuration page for standalone workspaces should now display the correct settings. If you continue to see outdated information, refresh the SSO Admin page to load the latest changes.

 9:14 PM PST

 Update

We have identified the trigger and developed a fix. We are currently deploying this fix and expect to resolve the issue shortly. We will provide another update once the deployment is complete. \
We apologize for the inconvenience.

 6:49 PM PST

 Investigating

Some workspace admins on standalone (non-Enterprise Grid) workspaces may be experiencing an issue where the SSO and authentication configuration page does not display current settings as expected. Admins may see a blank value for the "Require SSO authentication for" field on the admin SSO settings page, which does not accurately reflect the actual backend configuration — SSO settings remain intact and in effect.

We are actively investigating this issue and will provide an update as soon as we have more information to share. We apologize for the inconvenience.

 3:33 PM PST

Features affected

Login/SSO

Status

Incident
