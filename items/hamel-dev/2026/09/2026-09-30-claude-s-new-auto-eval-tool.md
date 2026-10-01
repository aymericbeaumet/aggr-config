---
title: Claude’s new auto eval tool
link: https://hamel.dev/blog/posts/claude-auto-evals/
source: hamel-dev
published: 2026-09-30T07:00:00Z
updated: 2026-09-30T07:00:00Z
first_seen: 2026-10-01T00:32:59.748537119Z
authors:
- Hamel Husain
labels:
- ai
- evals
summary: 'Anthropic released new eval tooling for Claude Code. Their claude-api plugin now includes a new build_eval and hill-climb command that helps you build evals, check the graders, and improve your application against them. I usually don’t review eval tools. Software changes so often that a review has a short shelf life. But a first-party tool from Anthropic is likely to influence how people approach evals, so I wanted to try it. Isaac Flath and I livestreamed ourselves using it on conversation traces from an apartment leasing assistant. Here’s what we found: The bad 1. It pushes you to create an eval before looking at data Claude started by suggesting several potential failures, then asked us to pick one straight away to turn into an eval. It gave us the below menu of options, with call-transfer rules as the recommended choice. We hadn’t yet reviewed the conversations ourselves, so it was hard to know if this was a real failure or worth prioritizing. Despite this, we went ahead with the recommended choice, because we figured that’s what typical users would do. I believe you should be looking at data first to inform your understanding and prioritize which evals to write. An agent can help you find issues, but you still should do error analysis to decide which failures deserve attention before proceeding. 2. It asks you to validate judgments without enough context Next, Claude created Markdown files for looking at data associated with the call-transfer failure. In the screenshot below, Claude asks us to skim inputs.md and “tell it” which labels are wrong. That meant reading long conversations in an editor and reporting corrections separately in a chat. We found this very silly as we were using a coding agent, so it should have built an annotation app that made the conversations easy to read and let us leave feedback in-situ. We eventually asked Claude to build a web app for us and used that instead. Later in the workflow, Claude made an initial attempt at creating an evaluator for the call-transfer failure. It presented aggregate label counts and asked us, “Would you have scored any case differently?” without giving us enough information to know if the labels were correct. A recurring theme of the workflow was to jump too fast into creating artifacts or asking us for approval without helping us understand the data. 3. The evaluator’s scope was too broad Next, the tool created a call-transfer evaluator that checked four different failures at once: Asking for confirmation more than once, or transferring without asking for confirmation. (LLM as a Judge) Saying something between the caller’s consent and the transfer. (Code-based eval) Saying something during or after the transfer. (Code-based eval) Saying tool mechanics aloud, such as “triggering” a transfer. (Code-based eval) There were too many things bundled into this evaluator. I would prefer to scope the eval to focus on one error at a time, or at the very least separate the evals into those that needed a code-based eval vs a LLM as a Judge. Claude’s description of the evaluator was also confusing: protocol_ok is the headline. A case passes only if all four checks pass. On “should not transfer” calls, protocol_ok is 1 if no transfer happened. This AI slop is hard to read. I’d much rather see the code or the judge prompt so I can understand whats being created. I’ve found that it always pays to read the prompt, especially for something as important as an eval. Below is a screenshot of what this part of the workflow looked like: The good I was impressed by this plugin’s out-of-the-box ability to discover issues that other auto-eval approaches haven’t been able to find! It found issues with human handoff, formatting, voice agents, and more. It’s still better to look at your data iteratively with an agent, but this was the strongest performance I’ve seen with a more “one-shot” issue discovery approach. Anthropic’s blog post introducing this tool broadly conveys thinking that I agree with, such as the importance of looking at data, sampling intelligently, not saturating your own evals, etc. I’m really happy more people are thinking about evals this way. Would I use it? I’d hold off for now. I’d want the workflow to help me explore the data before committing to an evaluator, with a better review interface from the start. Additionally, I’m already quite happy with what coding agents can do using these Eval skills Shreya and I put together which is less opinionated (but more flexible). I’ve since spoken with the author of the Claude eval plugin. He was appreciative of the feedback and said he’d update the plugin accordingly, so I expect it to change soon. It could be worth revisiting in the future. Even as this plugin changes, I hope this walkthrough helps you assess other eval tools. Make sure the tool helps you understand your data before choosing evals and inspect its judgments carefully. I believe much of the eval workflow should happen in a web application rather than chat to remove friction from data exploration and annotation. Remember, if an eval tool doesn’t put looking at data at the center of your workflow, it’s not worth using. Find all the eval memes on my memes page. Thanks to Isaac Flath for reviewing this article and joining me for the livestream.'
content: extracted
html: 2026-09-30-claude-s-new-auto-eval-tool.html
preview:
  file: 2026-09-30-claude-s-new-auto-eval-tool.preview-f340fac6f4f3.webp
  width: 256
  height: 192
  color: '#2f333a'
images:
- source: https://hamel.dev/blog/posts/claude-auto-evals/cover.png
  original:
    file: 2026-09-30-claude-s-new-auto-eval-tool.image-323af95b58a9.png
    width: 1448
    height: 1086
  variants:
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-8e9b658600bb.webp
    width: 320
    height: 240
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-931578137376.webp
    width: 640
    height: 480
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-b8ee6736dc6b.webp
    width: 960
    height: 720
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-a09265cd7e0c.webp
    width: 1280
    height: 960
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-7806365d731b.webp
    width: 1448
    height: 1086
  color: '#252a32'
- source: https://hamel.dev/blog/posts/claude-auto-evals/images/choose-eval-annotated.png
  original:
    file: 2026-09-30-claude-s-new-auto-eval-tool.image-a66a1aba9b77.png
    width: 1391
    height: 1131
  variants:
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-efddad56f14b.webp
    width: 320
    height: 260
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-114acce3f0bd.webp
    width: 640
    height: 520
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-a7bff0345248.webp
    width: 960
    height: 781
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-c4bad39ba453.webp
    width: 1280
    height: 1041
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-a45ca2c0934f.webp
    width: 1391
    height: 1131
  color: '#252a32'
- source: https://hamel.dev/blog/posts/claude-auto-evals/images/markdown-review-annotated-v3.png
  original:
    file: 2026-09-30-claude-s-new-auto-eval-tool.image-1405e1f05fa4.png
    width: 1391
    height: 1131
  variants:
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-e8799530ea1e.webp
    width: 320
    height: 260
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-0a951be6d8f0.webp
    width: 640
    height: 520
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-010da5200a39.webp
    width: 960
    height: 781
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-9758cca40287.webp
    width: 1280
    height: 1041
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-1ab25e8747ab.webp
    width: 1391
    height: 1131
  color: '#272b32'
- source: https://hamel.dev/blog/posts/claude-auto-evals/images/score-review-question-annotated-v3.png
  original:
    file: 2026-09-30-claude-s-new-auto-eval-tool.image-9128bc3fb2e8.png
    width: 1448
    height: 1086
  variants:
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-f2acfe4ed9d4.webp
    width: 320
    height: 240
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-2b26f38be01a.webp
    width: 640
    height: 480
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-b00d56ef62db.webp
    width: 960
    height: 720
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-48e0ad0785bc.webp
    width: 1280
    height: 960
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-715672dacac5.webp
    width: 1448
    height: 1086
  color: '#242a31'
- source: https://hamel.dev/blog/posts/claude-auto-evals/images/bundled-grader.png
  original:
    file: 2026-09-30-claude-s-new-auto-eval-tool.image-45bead509767.png
    width: 700
    height: 525
  variants:
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-e84aef6ace1c.webp
    width: 320
    height: 240
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-2a4755b0dee0.webp
    width: 640
    height: 480
  - file: 2026-09-30-claude-s-new-auto-eval-tool.image-72a0125a3334.webp
    width: 700
    height: 525
  color: '#272b33'
---

Anthropic released [new eval tooling for Claude Code](https://claude.dev/blog/automating-eval-design-and-hillclimbing/). Their `claude-api` plugin now includes a new `build_eval` and `hill-climb` command that helps you build evals, check the graders, and improve your application against them.

I usually don’t review eval tools. Software changes so often that a review has a short shelf life. But a first-party tool from Anthropic is likely to influence how people approach evals, so I wanted to try it.

[Isaac Flath](https://isaacflath.com/) and I [livestreamed ourselves using it](https://x.com/i/broadcasts/1nKOLQOwQEEGR) on conversation traces from an apartment leasing assistant. Here’s what we found:

## The bad

### 1\. It pushes you to create an eval before looking at data

Claude started by suggesting several potential failures, then asked us to pick one straight away to turn into an eval. It gave us the below menu of options, with call-transfer rules as the recommended choice. We hadn’t yet reviewed the conversations ourselves, so it was hard to know if this was a real failure or worth prioritizing. Despite this, we went ahead with the recommended choice, because we figured that’s what typical users would do.

![A red box highlights the recommended call-transfer evaluator. An arrow points to it beneath the question: How are we supposed to know this is a good eval to invest in at this point?](https://hamel.dev/blog/posts/claude-auto-evals/images/choose-eval-annotated.png)

I believe you should be looking at data first to inform your understanding and prioritize which evals to write. An agent can help you find issues, but you still should do [error analysis](https://hamel.dev/blog/posts/evals-faq/why-is-error-analysis-so-important-in-llm-evals-and-how-is-it-performed.html) to decide which failures deserve attention before proceeding.

### 2\. It asks you to validate judgments without enough context

Next, Claude created Markdown files for looking at data associated with the call-transfer failure. In the screenshot below, Claude asks us to skim `inputs.md` and “tell it” which labels are wrong. That meant reading long conversations in an editor and reporting corrections separately in a chat.

![Red text reads: This is the most painful way you can think of to label data. Create a web app instead! A question mark, arrow, and underline highlight the full sentence Please skim inputs.md and tell me in Claude's request to review labels.](https://hamel.dev/blog/posts/claude-auto-evals/images/markdown-review-annotated-v3.png)

We found this very silly as we were using a coding agent, so it should have built an annotation app that made the conversations easy to read and let us leave feedback in-situ. We eventually asked Claude to build a web app for us and used that instead.

Later in the workflow, Claude made an initial attempt at creating an evaluator for the call-transfer failure. It presented aggregate label counts and asked us, “Would you have scored any case differently?” without giving us enough information to know if the labels were correct. A recurring theme of the workflow was to jump too fast into creating artifacts or asking us for approval without helping us understand the data.

![Red text reads: There is no way to know the answer to this without seeing the data. You need to label this in-situ, not in a separate chat. A question mark, arrow, and underline highlight Claude asking: Would you have scored any case differently?](https://hamel.dev/blog/posts/claude-auto-evals/images/score-review-question-annotated-v3.png)

### 3\. The evaluator’s scope was too broad

Next, the tool created a call-transfer evaluator that checked four different failures at once:

- Asking for confirmation more than once, or transferring without asking for confirmation. (LLM as a Judge)
- Saying something between the caller’s consent and the transfer. (Code-based eval)
- Saying something during or after the transfer. (Code-based eval)
- Saying tool mechanics aloud, such as “triggering” a transfer. (Code-based eval)

There were too many things bundled into this evaluator. I would prefer to scope the eval to focus on one error at a time, or at the very least separate the evals into those that needed a code-based eval vs a LLM as a Judge.

Claude’s description of the evaluator was also confusing:

> protocol\_ok is the headline. A case passes only if all four checks pass. On “should not transfer” calls, protocol\_ok is 1 if no transfer happened.

This AI slop is hard to read. I’d much rather see the code or the judge prompt so I can understand whats being created. I’ve found that it [always pays to read the prompt](https://hamel.dev/blog/posts/prompt/), especially for something as important as an eval. Below is a screenshot of what this part of the workflow looked like:

![Claude defines protocol_ok as a single pass/fail result requiring four checks: confirm once, transfer immediately, remain silent, and avoid saying tool mechanics aloud.](https://hamel.dev/blog/posts/claude-auto-evals/images/bundled-grader.png)

## The good

I was impressed by this plugin’s out-of-the-box ability to discover issues that [other auto-eval approaches](https://parlance-labs.com/blog/posts/auto-evals.html) haven’t been able to find! It found issues with human handoff, formatting, voice agents, and more. It’s still better to look at your data iteratively with an agent, but this was the strongest performance I’ve seen with a more “one-shot” issue discovery approach.

Anthropic’s [blog post introducing this tool](https://claude.dev/blog/automating-eval-design-and-hillclimbing/) broadly conveys thinking that I agree with, such as the importance of looking at data, sampling intelligently, not saturating your own evals, etc. I’m really happy more people are thinking about evals this way.

## Would I use it?

I’d hold off for now. I’d want the workflow to help me explore the data before committing to an evaluator, with a better review interface from the start. Additionally, I’m already quite happy with what coding agents can do using these [Eval skills Shreya and I put together](https://hamel.dev/blog/posts/evals-skills/) which is less opinionated (but more flexible).

I’ve since spoken with the author of the Claude eval plugin. He was appreciative of the feedback and said he’d update the plugin accordingly, so I expect it to change soon. It could be worth revisiting in the future.

Even as this plugin changes, I hope this walkthrough helps you assess other eval tools. Make sure the tool helps you understand your data before choosing evals and inspect its judgments carefully. I believe much of the eval workflow should happen in a web application rather than chat to remove friction from data exploration and annotation.

Remember, if an eval tool doesn’t put looking at data at the center of your workflow, it’s not worth using.

*Thanks to [Isaac Flath](https://isaacflath.com/) for reviewing this article and joining me for [the livestream](https://x.com/i/broadcasts/1nKOLQOwQEEGR).*
