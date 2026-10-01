---
title: '[Retroactive] Actions workflow run failures after deployment gate approvals'
link: https://www.githubstatus.com/incidents/dqn46wtvbdzv
source: githubstatus-com
published: 2026-10-01T02:00:00Z
updated: 2026-10-01T11:34:33Z
first_seen: 2026-10-01T13:53:40.329767223Z
content: extracted
html: 2026-10-01-retroactive-actions-workflow-run-failures-after-deployment.html
preview:
  file: 2026-10-01-retroactive-actions-workflow-run-failures-after-deployment.preview-26f0b61ff844.webp
  width: 120
  height: 120
  color: '#0e0d0e'
images:
- source: https://dka575ofm4ao0.cloudfront.net/pages-twitter_logos/original/36420/GitHub-Mark-120px-plus.png
  original:
    file: 2026-10-01-retroactive-actions-workflow-run-failures-after-deployment.image-8e4af8e0da1d.png
    width: 120
    height: 120
  variants:
  - file: 2026-10-01-retroactive-actions-workflow-run-failures-after-deployment.image-26f0b61ff844.webp
    width: 120
    height: 120
  color: '#161415'
---

## Resolved

On October 1, around 02:00 UTC, an isolated infrastructure failure caused GitHub Actions to lose execution state for a small number of existing workflow runs. Affected runs may remain stuck, fail deployment approvals, or return errors when cancelled. Service connectivity has recovered, but the lost state cannot be restored by retrying an approval.

If you're affected, you can trigger a new run or contact GitHub Support with links to your stuck runs so we can unblock them. Once cleared, select "Re-run all jobs." This preserves the workflow run ID but starts a new attempt, rebuilds artifacts, and requires fresh deployment approvals. Before rerunning, check whether any deployment steps already completed to avoid repeating changes.

Posted Oct 01, 2026 - 02:00 UTC
