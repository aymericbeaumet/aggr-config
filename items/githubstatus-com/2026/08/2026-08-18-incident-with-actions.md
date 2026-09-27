---
title: Incident with Actions
link: https://www.githubstatus.com/incidents/gx7js8bd0jpz
source: githubstatus-com
published: 2026-08-18T10:23:23Z
updated: 2026-08-24T05:55:48Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-18-incident-with-actions.html
preview:
  file: 2026-08-18-incident-with-actions.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-18-incident-with-actions.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-18-incident-with-actions.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On August 18, 2026, between 05:02 UTC and 11:30 UTC, customers were unable to run jobs on Actions Larger Runners and were unable to view or manage Actions Runners and Runner Groups through the GitHub UI and API.

These issues were caused by failures in backend requests resolving essential metadata for starting Larger Runner workflow runs and for reading runner and runner group data. The failures were caused by an expired authentication certificate unique to this service. The certificate had been rotated in KeyVault, but a step to enable use at runtime had been paused to prevent recurrence of previous incidents that had been triggered by this operation.

We mitigated the issues by completing the enablement of the new certificate in the backend system. We have added additional monitoring to this and other certificates. The relevant service is also in the process of being replaced as part of our availability and scale work, bringing this authentication path and secret management in line with patterns across all GitHub services.

Posted Aug 18, 2026 - 10:23 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Aug 18, 2026 - 09:36 UTC
