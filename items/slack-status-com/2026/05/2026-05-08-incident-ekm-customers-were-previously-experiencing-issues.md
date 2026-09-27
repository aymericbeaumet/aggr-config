---
title: 'Incident: EKM customers were previously experiencing issues with channel loading and message delivery'
link: https://slack-status.com/2026-05/d284d808ed66c511
source: slack-status-com
published: 2026-05-08T18:31:22Z
updated: 2026-05-11T21:48:12Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Users who rely on Enterprise Key Management (EKM) may have experienced elevated latency and errors across messaging, channel loading, workflows, and file operations starting around 5:55 AM PDT on May 11, 2026. Impact was mitigated and the incident was brought under control at 9:16 AM PDT on May 11, 2026.We've deployed fixes addressing the root cause of the elevated encryption key request load. If you continue to experience trouble, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). We apologize for the disruption and thank you for your patience.
content: extracted
html: 2026-05-08-incident-ekm-customers-were-previously-experiencing-issues.html
preview:
  file: 2026-05-08-incident-ekm-customers-were-previously-experiencing-issues.preview-15d5325a696f.webp
  width: 256
  height: 256
  color: '#613b5e'
images:
- source: https://status.slack.com/img/v2_rebrand/slack_hash_256.png
  original:
    file: 2026-05-08-incident-ekm-customers-were-previously-experiencing-issues.image-23b0f4da6cd6.png
    width: 256
    height: 256
  color: '#521753'
- source: https://slack-status.com/img/v2/DetailPageCheck@2x.png
  original:
    file: 2026-05-08-incident-ekm-customers-were-previously-experiencing-issues.image-d9f8d6bffd0e.png
    width: 60
    height: 60
  variants:
  - file: 2026-05-08-incident-ekm-customers-were-previously-experiencing-issues.image-3f06aea498e5.webp
    width: 60
    height: 60
  color: '#39c49e'
---

![incident done](https://slack-status.com/img/v2/DetailPageCheck@2x.png) Resolved

Users who rely on Enterprise Key Management (EKM) may have experienced elevated latency and errors across messaging, channel loading, workflows, and file operations starting around 5:55 AM PDT on May 11, 2026. Impact was mitigated and the incident was brought under control at 9:16 AM PDT on May 11, 2026.

We've deployed fixes addressing the root cause of the elevated encryption key request load. If you continue to experience trouble, please reload Slack using Command + Shift + R (Mac) or Ctrl + Shift + R (Windows/Linux). We apologize for the disruption and thank you for your patience.

 2:48 PM PST

 Update

Our work on this issue is still ongoing.

**Current status and actions being taken**: Customer impact effectively stopped around 8:00AM PT and has remained stable. Our monitoring has shown no further instability and we have considered this incident under control. We are continuing to monitor closely and we'll update again when there's another meaningful update to share.

We apologize for the disruption.

 12:02 PM PST

 Monitoring

Our work on this issue is still ongoing.

- **Current status and actions being taken**: Our monitoring has shown no further instability and we have considered this incident now under control. We are continuing to monitor closely.

We'll provide another update should we find any further information of note. We apologize for the disruption.

 10:12 AM PST

 Update

**Current status and actions being taken**: We have identified contributing factors to the issue impacting EKM customers and have taken mitigating actions, including reducing the rate of KMS requests by approximately 50% and increasing cache capacity. We are continuing to monitor closely and are actively investigating the underlying cause in partnership with our third-party provider.

**Estimated time to resolution**: The incident has been stable for over an hour with no new customer-facing impact detected.

**Scope**: Customers using Enterprise Key Management (EKM)

**Current impact to end users:** ; Customers using Enterprise Key Management (EKM) may have experienced issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications, Workflow, DMs, and activity feeds.

**Known workarounds**: No known workarounds at this time.

We'll provide another update by 9:50 AM PT. We apologize for the disruption.

 9:38 AM PST

 Update

**Current status and actions being taken:**  We're continuing to investigate elevated error rates affecting a subset of users. We've identified additional load contributing to the issue and have taken steps to reduce it. We've also engaged our third-party vendor to assist in providing additional troubleshooting.

**Estimated time to resolution**: Under investigation. No ETA available at this time.

**Scope**: Customers using Enterprise Key Management (EKM)

**Current impact to end users**: Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications, Workflow, DMs, and activity feeds.

**Known workarounds**: No known workarounds at this time.

We'll provide another update in 30 minutes or as new information becomes available. We apologize for the disruption.

 9:05 AM PST

 Update

**Current status and actions being taken:** We're continuing to investigate elevated error rates affecting a subset of users. We've identified additional load contributing to the issue and have taken steps to reduce it.

**Estimated time to resolution:** Under investigation. No ETA available at this time.

**Scope**: Customers using Enterprise Key Management (EKM)

**Current impact to end users:** Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications, Workflow, DMs, and activity feeds.

**Known workarounds:** No known workarounds at this time.

We'll provide another update in 30 minutes or as new information becomes available. We apologize for the disruption.

 8:11 AM PST

 Update

**Current status and actions being taken:** We’re investigating a recurrence of a previous incident causing elevated errors and high latency for EKM customers. As a precaution, EKM backfills have been paused to reduce load. Teams are evaluating pod scaling and assessing a high-volume load test workload contributing to EKGen traffic. A bridge is active with EKM and EKGen engineers engaged, and third-party infrastructure teams have been re-engaged as a potential contributing factor.

**Estimated time to resolution:** Under investigation. No ETA available at this time.

**Scope**: Customers using Enterprise Key Management (EKM).

**Current impact to end users:** Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications, Workflow, DMs, and activity feeds.

**Known workarounds:** No known workarounds at this time.

We'll continue to provide updates every 30 minutes until the impact is resolved for all users.

 7:22 AM PST

 Resolved

This issue is now resolved for all users.

- **Current status and recent actions taken:** Our upstream provider has successfully implemented a fix. Following a period of stable monitoring, all services have been restored. Customers should no longer experience issues accessing Slack.
- **Scope:** This issue didn’t affect any specific region, but a percentage of Enterprise Key Management customers may have experienced an impact globally.
- **Previous impact to end users:** Customers using Enterprise Key Management (EKM) may have experienced issues sending messages and loading channels. Additionally, they may have experienced delays with all messaging activity, including notifications and activity feeds.
- **Required resolution steps**: No additional steps are necessary.

We apologize for any disruptions to your day.

 4:27 PM PST

 Update

We’re investigating this issue:

- **Current status and actions being taken:** Our upstream provider has implemented a fix and is continuing their investigation. In the meantime, we continue to observe improved health metrics. Affected customers may already be seeing improvements, and we’ll continue to monitor the situation closely until it is fully resolved
- **Estimated time to resolution**: No ETA at this time.
- **Scope:** This is not affecting any specific region, but a percentage of Enterprise Key Management customers may experience impact globally.
- **Current impact to end users**: Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications and activity feeds.
- **Known workarounds:** No known workarounds at this time.

We'll continue to provide updates every 60 minutes until the impact is resolved for all users.

 2:58 PM PST

 Update

We’re investigating this issue:

- **Current status and actions being taken:** We continue to monitor the issue as our upstream provider continues their work toward a resolution. Affected customers may already be seeing improvements, and we’ll continue to monitor the situation closely until it is fully resolved.
- **Estimated time to resolution**: No ETA at this time.
- **Scope:** This is not affecting any specific region, but a percentage of Enterprise Key Management customers may experience impact globally.
- **Current impact to end users**: Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications and activity feeds.
- **Known workarounds:** No known workarounds at this time.

We'll continue to provide updates every 60 minutes until the impact is resolved for all users.

 1:57 PM PST

 Update

We’re investigating this issue:

- **Current status and actions being taken:** We're seeing consistent improvement in our health metrics while our upstream provider has begun implementing a fix on their side. Affected customers may already be seeing improvements, and we’ll continue to monitor the situation closely until it is fully resolved.
- **Estimated time to resolution**: No ETA at this time.
- **Scope:** This is not affecting any specific region, but a percentage of Enterprise Key Management customers may experience impact globally.
- **Current impact to end users**: Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications and activity feeds.
- **Known workarounds:** No known workarounds at this time.

We'll continue to provide updates every 30 minutes until the impact is resolved for all users.

 1:27 PM PST

 Update

We’re investigating this issue:

- **Current status and actions being taken**: We’ve deployed a fix that has improved our cache hit ratio and are observing recovery in our health metrics. In parallel, we're monitoring for updates from our upstream provider. Affected customers may already be seeing improvements, and we’ll continue to monitor the situation closely until it is fully resolved.
- **Estimated time to resolution**: No ETA at this time.
- **Scope**: This is not affecting any specific region, but a percentage of Enterprise Key Management customers may experience impact globally.
- **Current impact to end users**: Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading channels. Additionally, they may experience delays with all messaging activity, including notifications and activity feeds.
- **Known workarounds**: No known workarounds at this time.

We'll continue to provide updates every 30 minutes until the impact is resolved for all users.

 12:54 PM PST

 Investigating

We’re investigating this issue:

- **Current status and actions being taken:**  We believe an issue with an upstream provider may be impacting some Slack services. We’re partnering with them to help with our investigation and are waiting on additional details from their analysis. In parallel, we’re actively reviewing the cache configuration to alleviate the impact.
- **Estimated time to resolution:** No ETA at this time.
- **Scope**: This is not affecting any specific region, but a percentage of Enterprise Key Management (EKM) customers may experience impact globally.
- **Current impact to end users:** Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading some channels.
- **Known workarounds**: No known workarounds at this time.

We'll continue to provide updates every 30 minutes until the impact is resolved for all users.

 12:20 PM PST

 Investigating

We’re investigating this issue:

- **Current status and actions being taken:**  We’re seeing renewed, elevated error rates and latency affecting customers using Enterprise Key Management (EKM). Affected customers may experience issues sending messages and loading some channels. Our team is actively investigating this issue.
- **Estimated time to resolution**: No ETA at this time.
- **Scope**: This is not affecting any specific region, but a percentage of Enterprise Key Management (EKM) customers may experience impact globally.
- **Current impact to end users**: Customers using Enterprise Key Management (EKM) may experience issues sending messages and loading some channels.
- **Known workarounds**: No known workarounds at this time.

We'll continue to provide updates every 30 minutes until the impact is resolved for all users.

 11:44 AM PST

 Investigating

Between approximately 10:55 AM and 11:10 AM PDT, customers using Enterprise Key Management (EKM) may have experienced issues sending messages and loading some channels. Error rates have returned to normal levels and customers should now see expected performance. We are continuing to investigate the underlying cause and monitoring closely. We apologize for the disruption.

 11:31 AM PST
