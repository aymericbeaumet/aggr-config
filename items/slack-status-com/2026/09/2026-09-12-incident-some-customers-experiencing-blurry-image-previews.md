---
title: 'Incident: Some customers experiencing blurry image previews and download issues'
link: https://slack-status.com/2026-09/f171324a097abb9a
source: slack-status-com
published: 2026-09-12T11:04:17Z
updated: 2026-09-12T20:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Some customers experienced blurry images that couldn't be downloaded across all clients, affecting files uploaded before a recent file storage update. We deployed a fix restoring proper file access, and confirmed it resolved the issue for affected customers. We apologize for how this incident affected you and your business. We will undertake a full investigation of the incident, establishing the technical trigger, the underlying cause, and preventive action to avoid a repeat in the future.
content: extracted
html: 2026-09-12-incident-some-customers-experiencing-blurry-image-previews.html
preview:
  file: 2026-09-12-incident-some-customers-experiencing-blurry-image-previews.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-09-12-incident-some-customers-experiencing-blurry-image-previews.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/TableIncident@2x.png
  original:
    file: 2026-09-12-incident-some-customers-experiencing-blurry-image-previews.image-4d3ed48ec52c.png
    width: 36
    height: 36
  variants:
  - file: 2026-09-12-incident-some-customers-experiencing-blurry-image-previews.image-1b833f24fef5.webp
    width: 36
    height: 36
  color: '#e2ac37'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-09-12-incident-some-customers-experiencing-blurry-image-previews.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-09-12-incident-some-customers-experiencing-blurry-image-previews.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![](https://slack-status.com/img/v2/TableIncident@2x.png)

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

Some customers experienced blurry images that couldn't be downloaded across all clients, affecting files uploaded before a recent file storage update. We deployed a fix restoring proper file access, and confirmed it resolved the issue for affected customers. We apologize for how this incident affected you and your business. We will undertake a full investigation of the incident, establishing the technical trigger, the underlying cause, and preventive action to avoid a repeat in the future.

 1:00 PM PST

 Cause Identified

**We have confirmed the technical trigger and are preparing a fix**

- **Current status and actions being taken: We** confirmed the technical trigger — a code change that caused older file records to lose their stored file names, breaking retrieval of existing images. The team is preparing a targeted fix to restore file names correctly when reading older files. The affected files are expected to recover automatically once the fix is deployed.
- **Estimated time to resolution**: We don't have an estimate yet.
- **Scope**: Affects a subset of customers across desktop, web, and mobile clients.
- **Current impact to end users**: Images uploaded before roughly 9:30 PM GMT on Sep 11 appear blurry in previews and fail to download. Newly uploaded images are unaffected. Previously affected images remain blurry and undownloadable.

We appreciate your patience as we sort this out.

 8:24 AM PST

 Investigating

T**he earlier rollback did not resolve the issue, and investigation has** been redirected **to a file-serving problem rather than the thumbnail-generation change.**

- **Current status and actions being taken**: The rollback of the thumbnail-sharpening change did not fix the issue. We have revised the technical trigger assessment to a file-serving problem, where the system returns near-empty successful responses instead of full image data. We are investigating file storage and access-policy changes, and gathering file details from affected customers to pinpoint the cause.
- **Estimated time to resolution:**  No confirmed estimate yet; the team is still narrowing down the cause following the pivot to file serving.
- **Scope**: Affects a subset of customers across desktop, web, and mobile clients.
- **Current impact to end users**: Images uploaded before 9:30 PM GMT on Sep 11 appear blurry in previews and fail to download. Newly uploaded images are unaffected. The earlier rollback did not change this — previously affected images remained blurry and undownloadable.

We appreciate your patience as we sort this out.

 7:10 AM PST

 Cause Identified

**We have rolled back the code change that caused the issue in the production environment. Validation is in progress.**

- **Current status and actions being taken: We** confirmed the technical trigger (a thumbnail-sharpening code change) and completed a rollback in the production environment after successfully testing the fix in the test environment. We are performing additional validations to confirm the fix was successful.
- **Estimated time to resolution**: We do not have and estimated time frame at the moment,.
- **Scope**: Affects a subset of customers across desktop, web, and mobile clients;
- **Current impact to end users**: Images uploaded before roughly 9:30 PM GMT on Sep 11 appear blurry in previews and fail to download. Newly uploaded images are unaffected.

We appreciate your patience as we sort this out.

 5:52 AM PST

 Investigating

We've identified a likely technical trigger connected to a recent image-rendering service change.

- **Current status and actions being taken**: We have identified a likely technical trigger — a recent thumbnail-sharpening change to the image-rendering service. We're working to validate this, and are evaluating the rollback of the change as a possible fix.
- **Estimated time to resolution**: We are still confirming the technical trigger before a fix can be scoped.
- **Scope**: Affects a subset of customers across desktop, web, and mobile clients
- **Current impact to end users**: Images uploaded before roughly 9:30 PM GMT on Sep 11 appear blurry in previews and fail to download. Newly uploaded images are unaffected.
- **Known workarounds**: None identified yet.

We appreciate your patience as we sort this out.

 4:29 AM PST

 Investigating

We're aware there's an issue where a subset of customers are experiencing blurry images which can't be downloaded, this is happening across all clients. We're actively working to understand the cause, scope, fix, or other details.

 4:04 AM PST

Features affected

Files

Status

Incident
