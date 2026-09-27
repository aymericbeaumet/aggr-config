---
title: Delays in credit purchases
link: https://status.claude.com/incidents/620swtqyn24k
source: status-claude-com
published: 2026-09-02T01:24:22Z
updated: 2026-09-02T01:24:22Z
first_seen: 2026-09-27T19:29:15.293681927Z
content: extracted
html: 2026-09-02-delays-in-credit-purchases.html
preview:
  file: 2026-09-02-delays-in-credit-purchases.preview-d71bfc908b8e.webp
  width: 96
  height: 96
  color: '#385d09'
images:
- source: https://dka575ofm4ao0.cloudfront.net/assets/logos/favicon-2b86ed00cfa6258307d4a3d0c482fd733c7973f82de213143b24fc062c540367.png
  original:
    file: 2026-09-02-delays-in-credit-purchases.image-a82d857ea7fa.png
    width: 96
    height: 96
  variants:
  - file: 2026-09-02-delays-in-credit-purchases.image-d71bfc908b8e.webp
    width: 96
    height: 96
  color: '#73ba00'
---

## Resolved

This issue has been resolved.

Posted Sep 02, 2026 - 01:24 UTC

## Monitoring

We have identified and resolved an issue where users who reached a balance of zero usage credits saw delays in the availability of newly purchased credits, resulting in some requests to the Claude API receiving 'credit balance is too low' errors erroneously. This affected credits purchased from 05:10am PT / 12:10 UTC to 2:35pm PT / 21:35 UTC. At this time, newly purchased credits should be available as expected and we are working to resolve any remaining impact.

Posted Sep 01, 2026 - 23:26 UTC

This incident affected: Claude Console (platform.claude.com).
