---
title: 'Incident: Slack Search Feature Disruption - Japan region'
link: https://slack-status.com/2026-04/4bd5196b0c972734
source: slack-status-com
published: 2026-05-01T01:50:42Z
updated: 2026-05-01T01:50:42Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: At 23:20 UTC, April 30, 2026, a feature disruption began affecting the Slack Search feature for a subset of customers in Japan region. As a result, users were unable to use Search functionality within Slack.We identified that infrastructure changes to our Search cluster caused a number of internal search nodes to become unavailable, which prevented Search queries from routing correctly. We implemented a series of remediation steps, including increasing cluster capacity and restoring affected search shards. We also blocked certain Search traffic to contain secondary impact while the cluster was being recovered.As of 00:45 UTC, May 1, 2026, Slack Search functionality is fully operational.We apologize for how this incident affected you and your business. We will undertake a full investigation of the incident, establishing the technical trigger, the underlying cause, and preventive action to avoid a repeat in the future.
content: extracted
html: 2026-05-01-incident-slack-search-feature-disruption-japan-region.html
preview:
  file: 2026-05-01-incident-slack-search-feature-disruption-japan-region.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-05-01-incident-slack-search-feature-disruption-japan-region.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-05-01-incident-slack-search-feature-disruption-japan-region.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-05-01-incident-slack-search-feature-disruption-japan-region.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

At 23:20 UTC, April 30, 2026, a feature disruption began affecting the Slack Search feature for a subset of customers in Japan region. As a result, users were unable to use Search functionality within Slack.

We identified that infrastructure changes to our Search cluster caused a number of internal search nodes to become unavailable, which prevented Search queries from routing correctly. We implemented a series of remediation steps, including increasing cluster capacity and restoring affected search shards. We also blocked certain Search traffic to contain secondary impact while the cluster was being recovered.

As of 00:45 UTC, May 1, 2026, Slack Search functionality is fully operational.

We apologize for how this incident affected you and your business. We will undertake a full investigation of the incident, establishing the technical trigger, the underlying cause, and preventive action to avoid a repeat in the future.

 6:50 PM PST
