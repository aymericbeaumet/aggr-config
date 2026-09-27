---
title: Intermittent failures in runner group and runner-related permissions pages
link: https://www.githubstatus.com/incidents/bmpybhnrky3x
source: githubstatus-com
published: 2026-08-18T11:42:59Z
updated: 2026-08-24T05:56:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-08-18-intermittent-failures-in-runner-group-and-runner-related.html
preview:
  file: 2026-08-18-intermittent-failures-in-runner-group-and-runner-related.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-08-18-intermittent-failures-in-runner-group-and-runner-related.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-08-18-intermittent-failures-in-runner-group-and-runner-related.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On August 18, 2026, between 05:02 UTC and 11:30 UTC, customers were unable to view or manage Actions Runners and Runner Groups through the GitHub UI and API.

The issue was caused by failures in backend requests reading runner and runner group data. The failures were caused by an expired authentication certificate unique to this service. The certificate had been rotated in KeyVault, but a step to enable use at runtime had been paused to prevent recurrence of previous incidents triggered by this operation.

The impact was mitigated by completing the enablement of the new certificate in the backend system. We have added additional monitoring to this and other certificates. This service is also in the process of being replaced as part of our availability and scale work, bringing this authentication path and secret management in line with patterns across all GitHub services.

Posted Aug 18, 2026 - 11:42 UTC

## Update

We have applied a mitigation and are seeing recovery signals. We will continue monitoring recovery and providing updates.

Posted Aug 18, 2026 - 11:24 UTC

## Update

We have identified the source of a communication issue between Actions services and are working toward mitigation. Customers may experience failure to load runner groups and runner-related permissions issues when using Larger Runners.

Posted Aug 18, 2026 - 10:41 UTC

## Monitoring

We are investigating reports of failure to load runner groups and runner-related permissions for customers using larger runners.

Posted Aug 18, 2026 - 07:40 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Aug 18, 2026 - 07:40 UTC
