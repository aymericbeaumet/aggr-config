---
title: Incident with Pull Requests
link: https://www.githubstatus.com/incidents/f6yrxnz5f7bs
source: githubstatus-com
published: 2026-09-20T23:22:16Z
updated: 2026-09-24T21:29:12Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-20-incident-with-pull-requests.html
preview:
  file: 2026-09-20-incident-with-pull-requests.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-20-incident-with-pull-requests.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-20-incident-with-pull-requests.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On September 20, 2026, between 21:46 and 22:24 UTC the Pull Requests service was degraded and pull request merge and test-merge commits were created late, with delays reaching approximately four minutes at peak. Merge commits were delayed rather than lost. Because some Actions workflow runs start only after a pull request's merge commit is created, a subset of workflow runs for pull request events were also delayed.

This was due to a routine repository maintenance job for an unusually large repository consuming nearly all of the memory on a single Git storage server, which left that server unable to serve the Git operations used to create merge commits.

We mitigated the incident by removing the affected server from service at 22:18 UTC, after which the queued merge commits were created within six minutes.

We have capped the memory a single repository maintenance job may consume so that one repository cannot exhaust a server, and we have improved monitoring and alerting on storage server health to reduce our time to detection and mitigation of issues like this one in the future.

Posted Sep 20, 2026 - 23:22 UTC

## Update

A git fileserver issue caused a brief delay in creating some merge commits - we've isolated the underlying server and already observed recovery.

Posted Sep 20, 2026 - 22:32 UTC

## Monitoring

The degradation affecting Pull Requests has been mitigated. We are monitoring to ensure stability.

Posted Sep 20, 2026 - 22:27 UTC

## Investigating

We are investigating reports of degraded performance for Pull Requests

Posted Sep 20, 2026 - 22:13 UTC

This incident affected: Pull Requests.
