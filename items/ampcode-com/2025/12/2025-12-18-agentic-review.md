---
title: Agentic Review
link: https://ampcode.com/news/agentic-code-review
source: ampcode-com
published: 2025-12-18T00:00:00Z
updated: 2025-12-18T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Amp has a new agent and it specializes in code review. In the VS Code extension, you can use this agent by going to the review panel. Start by dragging the selection of changes you want to review. Sometimes, you want to review a single commit; other times you want to review all outstanding changes on your branch: Amp will pre-scan the diff and recommend an order in which to review the files. It will also provide a summary of the changes in each file and the changeset overall: This addresses a key difficulty in reviewing large changesets, as it''s often difficult to know where to start. Clicking on a file will open the full file diff, which is editable and has code navigation if your diff includes the current working changes: Review Agent The review agent lives in a separate panel below. It analyzes the changes and posts a list of actionable improvements, which can then be fed back into the main Amp agent to close the feedback loop: There''s a big improvement in review quality over the first version of the review panel, which used a single-shot LLM request. The new review agent uses Gemini 3 Pro and a review-oriented toolset to perform a much deeper analysis that surfaces more bugs and actionable feedback while filtering out noise. To get to the review panel, click the button in the navbar. We''ve also added a ⌘ ; keybinding to make it easy to toggle in and out of review mode. An Opinionated Read-Write Loop If you''re wondering how best to incorporate this into your day-to-day workflow, here''s our opinionated loop for agentic coding in the editor: 1 Write code with the agent 2 Open the review panel ⌘ ; 3 Drag your target diff range 4 Request agentic review and read summaries + diffs while waiting 5 Feed comments back into the agent Open Questions We think the review agent and UI help substantially with the bottleneck of reviewing code written by agent. But we''re still pondering some more open questions: How do reviews map to threads? It''s not 1-1, since you can review the output of multiple threads at once. How do we incorporate review feedback into long-term memory? When you accept or reject review comments, should Amp learn from that? Should it incorporate feedback into AGENTS.md? What does the TUI version of this review interface look like? Should there exist an editable review interface in the TUI? Or should we integrate with existing terminal-based editors and diff viewers?'
content: extracted
html: 2025-12-18-agentic-review.html
preview:
  file: 2025-12-18-agentic-review.preview-5f264fa51ff9.webp
  width: 256
  height: 134
  color: '#61564a'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Agentic+Review&date=December+18%2C+2025&tagline=A+new+agent+for+reviewing+agent-generated+code+in+your+editor&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fagentic-review%2Ffile-diff.png%3Fv%3D2&sig=203ca78b735c25c402d5ffdbd832a77a5e8a1759c01dd3ba2b5e07b27eb01613
  original:
    file: 2025-12-18-agentic-review.image-41e8f319b777.png
    width: 1200
    height: 630
  variants:
  - file: 2025-12-18-agentic-review.image-35ea52ce0f1f.webp
    width: 320
    height: 168
  - file: 2025-12-18-agentic-review.image-4c030af1b280.webp
    width: 640
    height: 336
  - file: 2025-12-18-agentic-review.image-e2d12aa821ee.webp
    width: 960
    height: 504
  - file: 2025-12-18-agentic-review.image-b9c23f82fc42.webp
    width: 1200
    height: 630
  color: '#221d19'
- source: https://static.ampcode.com/news/agentic-review/range-finder.png
  original:
    file: 2025-12-18-agentic-review.image-7394661a4140.png
    width: 1206
    height: 504
  color: '#252727'
- source: https://static.ampcode.com/news/agentic-review/changes.png?v=2
  original:
    file: 2025-12-18-agentic-review.image-346832584bc2.png
    width: 1206
    height: 824
  color: '#242625'
- source: https://static.ampcode.com/news/agentic-review/file-diff.png?v=2
  original:
    file: 2025-12-18-agentic-review.image-e3b4c86d3ab3.png
    width: 1517
    height: 1092
  color: '#252625'
- source: https://static.ampcode.com/news/agentic-review/agentic-review.png?v=2
  original:
    file: 2025-12-18-agentic-review.image-8aabc1d53eb8.png
    width: 1270
    height: 964
  color: '#1e2021'
---

Amp has a new agent and it specializes in code review.

In the VS Code extension, you can use this agent by going to the review panel. Start by dragging the selection of changes you want to review. Sometimes, you want to review a single commit; other times you want to review all outstanding changes on your branch:

![](https://static.ampcode.com/news/agentic-review/range-finder.png)

Amp will pre-scan the diff and recommend an order in which to review the files. It will also provide a summary of the changes in each file and the changeset overall:

![](https://static.ampcode.com/news/agentic-review/changes.png?v=2)

This addresses a key difficulty in reviewing large changesets, as it's often difficult to know where to start.

Clicking on a file will open the full file diff, which is editable and has code navigation if your diff includes the current working changes:

![](https://static.ampcode.com/news/agentic-review/file-diff.png?v=2)

## Review Agent

The review agent lives in a separate panel below. It analyzes the changes and posts a list of actionable improvements, which can then be fed back into the main Amp agent to close the feedback loop:

![](https://static.ampcode.com/news/agentic-review/agentic-review.png?v=2)

There's a big improvement in review quality over the [first version](https://ampcode.com/news/review) of the review panel, which used a single-shot LLM request. The new review agent uses Gemini 3 Pro and a review-oriented toolset to perform a much deeper analysis that surfaces more bugs and actionable feedback while filtering out noise.

To get to the review panel, click the button in the navbar. We've also added a ⌘ ; keybinding to make it easy to toggle in and out of review mode.

## An Opinionated Read-Write Loop

If you're wondering how best to incorporate this into your day-to-day workflow, here's our opinionated loop for agentic coding in the editor:

1

Write code with the agent

2

Open the review panel ⌘ ;

3

Drag your target diff range

4

Request agentic review and\
read summaries + diffs while waiting

5

Feed comments back into the agent

## Open Questions

We think the review agent and UI help substantially with the bottleneck of reviewing code written by agent. But we're still pondering some more open questions:

- **How do reviews map to threads?** It's not 1-1, since you can review the output of multiple threads at once.
- **How do we incorporate review feedback into long-term memory?** When you accept or reject review comments, should Amp learn from that? Should it incorporate feedback into AGENTS.md?
- **What does the TUI version of this review interface look like?** Should there exist an editable review interface in the TUI? Or should we integrate with existing terminal-based editors and diff viewers?
