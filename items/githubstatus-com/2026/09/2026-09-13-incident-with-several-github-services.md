---
title: Incident with several GitHub Services
link: https://www.githubstatus.com/incidents/0rn90wk115q9
source: githubstatus-com
published: 2026-09-13T10:44:55Z
updated: 2026-09-15T21:47:16Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-13-incident-with-several-github-services.html
preview:
  file: 2026-09-13-incident-with-several-github-services.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-09-13-incident-with-several-github-services.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-13-incident-with-several-github-services.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

On September 13, 2026, between 08:43 and 10:44 UTC, GitHub experienced degraded availability across approximately 28 services, including Issues, Pull Requests, Actions, Codespaces, Pages, Notifications, Code Scanning, Git LFS, and new account signup. At peak, 8.8% of requests to create GitHub App installation access tokens failed. Token issuance for Actions workflows was also affected, impacting approximately 4% of workflows during the incident time frame. Creating issues through the web interface failed for about 96% of attempts, and signup failures were above 90%.

The cause was an internal data-cleanup job that began writing to a shared database cluster at 07:33 UTC. That cluster stores permission data read on nearly every authenticated request. The safeguard that was pacing the background job watched only one health signal — how far the database replicas were lagging — and that signal stayed low the whole time. It did not account for the load building on the primary itself, so the job kept writing while the primary quietly ran toward its limit.

When the primary ran out of available connections, requests that needed it could not complete. First, there was no quick timeout on these database calls, so request handlers waited on the stalled database instead of failing fast, and the shared request-handling capacity degraded into site-wide errors. Second, a retry loop around token creation kept re-sending the writes that were already failing, which held the database saturated rather than letting it recover.

Monitoring declared the incident at 08:50 UTC, but due to the broad impact and amplification from token creation, it took time to identify the source of the load. First responders mitigated by shedding internal load and pausing the job, and all services recovered by 10:44 UTC.

To prevent recurrence, we are rate-limiting background jobs against shared, customer-serving databases by default, and adding automatic pausing and paging on primary-server load rather than replication lag alone. We are also surfacing running background work directly alongside database health signals so responders can see and pause it without leaving those dashboards, bounding retries in the token-issuing path, and adding request-level timeouts so one unhealthy database cannot consume shared web server capacity. In addition, we are breaking apart this database cluster to remove the single point of failure. We will be moving various service-specific data, including the authorization data, out of this shared cluster in the next two weeks.

Posted Sep 13, 2026 - 10:44 UTC
