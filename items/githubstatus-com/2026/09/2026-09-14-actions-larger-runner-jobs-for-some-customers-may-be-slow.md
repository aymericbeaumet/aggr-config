---
title: Actions Larger Runner Jobs for some customers may be slow to start
link: https://www.githubstatus.com/incidents/kzh5j0mt7qwc
source: githubstatus-com
published: 2026-09-14T19:35:48Z
updated: 2026-09-18T23:11:13Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-14-actions-larger-runner-jobs-for-some-customers-may-be-slow.html
preview:
  file: 2026-09-14-actions-larger-runner-jobs-for-some-customers-may-be-slow.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-14-actions-larger-runner-jobs-for-some-customers-may-be-slow.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-14-actions-larger-runner-jobs-for-some-customers-may-be-slow.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On September 14, 2026, between 16:10 and 19:01 UTC, some customers using GitHub Actions larger runners experienced longer-than-normal wait times for jobs to start. During this period, **5.7%** of larger-runner jobs were affected.

A routine expansion of our compute capacity exposed a bug in how our provisioning system handled capacity records when selecting where to create runner virtual machines. This slowed the creation of new runners, leaving insufficient runner capacity to start affected jobs promptly.

We restored normal provisioning by correcting the affected capacity records. We have fixed the underlying capacity-selection bug to prevent this failure from recurring. We have also added alerts for VM-record creation failures associated with this capacity issue.

Posted Sep 14, 2026 - 19:35 UTC

## Monitoring

The degradation has been mitigated. We are monitoring to ensure stability.

Posted Sep 14, 2026 - 19:01 UTC

## Investigating

We are investigating reports of impacted performance for some GitHub services.

Posted Sep 14, 2026 - 18:40 UTC
