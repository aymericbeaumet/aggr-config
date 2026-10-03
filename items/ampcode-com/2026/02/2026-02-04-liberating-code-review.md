---
title: Liberating Code Review
link: https://ampcode.com/news/liberating-code-review
source: ampcode-com
published: 2026-02-04T00:00:00Z
updated: 2026-02-04T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'Code review has traditionally been tied to an interface where a human reads diffs. Now, we''ve fully decoupled the review agent from any UI, making it a composable and extensible subroutine that can be invoked from many different places where it is useful: You can run amp review in the CLI to run the review agent directly You can request a review in any thread using natural language like "review the outstanding changes" or "review changes since diverging from main" This composability also means you can more easily close the loop by asking the main agent to automatically fix the issues found or by piping review comments into another command. Invoking the review agent directly using amp review: Requesting a review from within a thread: Customizing Review with Checks You can also define Checks within your codebase. Checks are user-defined invariants or review criteria scoped to specific parts of your codebase. They are defined in .agents/checks/ directories. Here''s an example performance check, which you could save to .agents/checks/perf.md: --- name: performance description: Flags common performance anti-patterns --- Look for these patterns: - Nested loops over the same collection (O(n²) → O(n) with a Set/Map) - Repeated `array.includes()` in a loop - Sorting inside a loop - String concatenation in a loop (use array + join) Report the line, why it matters, and how to fix it. The code_review tool will kick off a separate agent for each check. This provides a stronger guarantee that each check will actually be checked than if the checks were embedded in a general context file like AGENTS.md. Here are some more examples of useful checks: Performance best practices specific to your stack Common anti-patterns your team has hit before Security best practices or invariants Migration reminders for deprecated APIs Stylistic conventions that aren''t in the linter Compliance requirements Checks are scoped to the directory that contains .agents/, so a root .agents/checks directory would cover the entire codebase while api/.agents/checks would cover files under api/.'
content: extracted
html: 2026-02-04-liberating-code-review.html
preview:
  file: 2026-02-04-liberating-code-review.preview-cd59849c09e8.webp
  width: 256
  height: 134
  color: '#5c554b'
images:
- source: https://ampcode.com/og?_template=news&_version=7&title=Liberating+Code+Review&date=February+4%2C+2026&tagline=Amp%27s+code+review+agent+is+now+composable+and+extensible.&sig=8438088263379e9f63f5c671567629c5bed24619c31ae67997c25fe232f6d39e
  original:
    file: 2026-02-04-liberating-code-review.image-f95634f23407.png
    width: 1200
    height: 630
  variants:
  - file: 2026-02-04-liberating-code-review.image-42256a3adcc7.webp
    width: 320
    height: 168
  - file: 2026-02-04-liberating-code-review.image-ae43a1515d4c.webp
    width: 640
    height: 336
  - file: 2026-02-04-liberating-code-review.image-263d9a78844e.webp
    width: 960
    height: 504
  - file: 2026-02-04-liberating-code-review.image-f4fa3a6f01eb.webp
    width: 1200
    height: 630
  color: '#221b16'
- source: https://static.ampcode.com/news/review-skill/cli_review.png
  original:
    file: 2026-02-04-liberating-code-review.image-d23a005cb502.png
    width: 3276
    height: 1736
  color: '#282c33'
---

Code review has traditionally been tied to an interface where a human reads diffs. Now, we've fully decoupled the review agent from any UI, making it a composable and extensible subroutine that can be invoked from many different places where it is useful:

- You can run `amp review` in the CLI to run the review agent directly
- You can request a review in any thread using natural language like "review the outstanding changes" or "review changes since diverging from main"

This composability also means you can more easily close the loop by asking the main agent to automatically fix the issues found or by piping review comments into another command.

Invoking the review agent directly using `amp review`:

Requesting a review from within a thread: ![Review tool](https://static.ampcode.com/news/review-skill/cli_review.png)

## Customizing Review with Checks

You can also define [*Checks*](https://ampcode.com/manual#checks) within your codebase. Checks are user-defined invariants or review criteria scoped to specific parts of your codebase. They are defined in `.agents/checks/` directories.

Here's an example performance check, which you could save to `.agents/checks/perf.md`:

```markdown
---
name: performance
description: Flags common performance anti-patterns
---

Look for these patterns:

- Nested loops over the same collection (O(n²) → O(n) with a Set/Map)
- Repeated `array.includes()` in a loop
- Sorting inside a loop
- String concatenation in a loop (use array + join)

Report the line, why it matters, and how to fix it.
```

The `code_review` tool will kick off a separate agent for each check. This provides a stronger guarantee that each check will actually be checked than if the checks were embedded in a general context file like `AGENTS.md`.

Here are some more examples of useful checks:

- Performance best practices specific to your stack
- Common anti-patterns your team has hit before
- Security best practices or invariants
- Migration reminders for deprecated APIs
- Stylistic conventions that aren't in the linter
- Compliance requirements

Checks are scoped to the directory that contains `.agents/`, so a root `.agents/checks` directory would cover the entire codebase while `api/.agents/checks` would cover files under `api/`.
