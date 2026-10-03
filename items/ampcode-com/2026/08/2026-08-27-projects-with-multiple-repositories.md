---
title: Projects with Multiple Repositories
link: https://ampcode.com/news/multi-repo-projects
source: ampcode-com
published: 2026-08-27T00:00:00Z
updated: 2026-08-27T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: Projects in Amp can now include more than one repository. Additional repositories are checked out to an adjacent directory in the orb and the agent is made aware of them. The changes panel will show the diff across all repositories in the project. To add additional repositories, specify them when creating the project or later in project settings. You can add up to 20 additional repositories per project. Only the primary repository's .agents/setup script runs automatically on orb setup. If an additional repository requires initialization, you should update the primary's setup script to include this, or ask an Amp agent to make this update for you.
content: extracted
html: 2026-08-27-projects-with-multiple-repositories.html
preview:
  file: 2026-08-27-projects-with-multiple-repositories.preview-e1e58cd13afb.webp
  width: 256
  height: 134
  color: '#81796e'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Projects+with+Multiple+Repositories&date=August+27%2C+2026&tagline=Give+Amp+every+repository+it+needs+to+finish+the+job.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fmulti-repo-project-picker.png&sig=498e0e6c9065950dd4bc686c5c68105d8353cba42748329a90c99d9e3077ff16
  original:
    file: 2026-08-27-projects-with-multiple-repositories.image-aa720458255d.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-27-projects-with-multiple-repositories.image-964e4ad36d9a.webp
    width: 320
    height: 168
  - file: 2026-08-27-projects-with-multiple-repositories.image-89df275bc7d1.webp
    width: 640
    height: 336
  - file: 2026-08-27-projects-with-multiple-repositories.image-83f4cc4693e5.webp
    width: 960
    height: 504
  - file: 2026-08-27-projects-with-multiple-repositories.image-5486208c90b4.webp
    width: 1200
    height: 630
  color: '#66605c'
- source: https://static.ampcode.com/news/multi-repo-diff.png
  original:
    file: 2026-08-27-projects-with-multiple-repositories.image-ab2e7ebb21ba.png
    width: 1580
    height: 1078
  color: '#f6f8f4'
- source: https://static.ampcode.com/news/multi-repo-project-picker.png
  original:
    file: 2026-08-27-projects-with-multiple-repositories.image-498598020f7b.png
    width: 1280
    height: 1568
  color: '#f9f9f7'
---

Projects in Amp can now include more than one repository.

Additional repositories are checked out to an adjacent directory in the orb and the agent is made aware of them. The changes panel will show the diff across all repositories in the project.

![The Changes panel showing changes in primary and dependency repositories](https://static.ampcode.com/news/multi-repo-diff.png)

To add additional repositories, specify them when creating the project or later in project settings. You can add up to 20 additional repositories per project.

![The New Project dialog with an arrow pointing to Add Additional Repositories](https://static.ampcode.com/news/multi-repo-project-picker.png)

Only the primary repository's `.agents/setup` script runs automatically on orb setup. If an additional repository requires initialization, you should update the primary's setup script to include this, or ask an Amp agent to make this update for you.
