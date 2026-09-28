---
title: There is more to code review than (automatable) detection
link: https://www.adaptivecapacitylabs.com/2026/08/24/there-is-more-to-code-review-than-automatable-detection/
source: hnrss-org
published: 2026-09-26T15:06:28Z
updated: 2026-09-26T15:06:28Z
first_seen: 2026-09-28T06:49:16.850449770Z
authors:
- utiiiD
content: extracted
html: 2026-09-26-there-is-more-to-code-review-than-automatable-detection.html
preview:
  file: 2026-09-26-there-is-more-to-code-review-than-automatable-detection.preview-9ecee80470ad.webp
  width: 256
  height: 107
  color: '#7c7463'
images:
- source: https://www.adaptivecapacitylabs.com/wp-content/uploads/2026/08/IMG_6084.png
  original:
    file: 2026-09-26-there-is-more-to-code-review-than-automatable-detection.image-86cd93f75187.png
    width: 1280
    height: 537
  color: '#ebe4ca'
---

The abstract for article “[The End of Code Review: Coding Agents Supersede Human Inspection](https://arxiv.org/abs/2606.13175)” paints this picture for the reader…

> **Abstract** – Code review has been the primary quality gate in software development since Fagan formalised code inspection in 1976. For five decades, having a human examine and comment on a colleague’s changes before merge has been a cornerstone practice at organisations of every size. Coding agents are large language model (LLM)-based autonomous systems capable of reading, writing, testing, and repairing software. We argue that coding agents have crossed a threshold of capability at which traditional human code review is no longer a necessary component of a software quality pipeline. Our argument rests on two claims: every stated goal of code review can be served by agents at lower cost and higher throughput; the naive integration in which agents write code and humans remain the mandatory reviewers is a dead end because it neither provides meaningful assurance nor scales with AI-assisted throughput.

The article is structured well and quite straightforward for engineers who aren’t used to reading research articles very often. However, I do think the argument critically depends on a problematic framing: ***the substitution myth***.

The author decomposes peer code review into four stated functions: defect detection, style enforcement, knowledge transfer, and awareness. It argues an agent can perform each one. The conclusion, of course, is that if an agent can execute each of those functions, then the agent has the capability to replace a human reviewer.

I think this overlooks some important aspects of peer code review that *cannot* be reduced to a function:

**A peer reviewer’s confusion**

When an experienced engineer reads a diff and says “I don’t understand this.”, their confusion *is* the finding. It means the code is either too complex, the abstraction is wrong, or the intent is not clear. An LLM will always ‘understand’ the code in the sense of being able to *process* it. It can’t give you the signal of legitimate human incomprehension. The article treats comprehensibility as something that is more about style than anything else. It’s not. It’s an emergent property and it shows up in the interaction between a person attempting to understand the artifact and the artifact itself.

**Qualified skepticism about whether the change is even necessary**

Questioning the existence of a change, like:

> “Should this actually be *two* PRs?”

or

> “This solves the symptom, not the problem”

These are questions about intent, scope, and appropriateness of the change. All of that comes *before* whether the code is “correct.” The article’s framing assumes that a) the code change being reviewed is necessary, and b) the main purpose of the review is verification.

But anybody who has ever had contact with production understands that code review is *often the last* (or sometimes only) moment when someone can be expected to challenge whether the change is even necessary.

**The ability to see what is *not* there**

A human reviewer can notice that an API contract has changed but the error handling didn’t. They can notice what is *missing*. In other words: being able to recognize what is *expected* to be present, but isn’t. The article doesn’t acknowledge this at all, which is particularly interesting, given that [absence blindness](https://absencebench.github.io/) is exactly the class of failure that LLMs tend to be quite poor at.

The agent reviews what is there; engineers with expertise can easily notice what’s missing.

***Who* wrote the code influences the scrutiny of the review**

Peer code reviewers have a sort of *calibrated attention* that comes from past experience with the code’s author, who is often a colleague. For example: a less-tenured engineer’s first commit to, say, a payments module will likely get different attention than a veteran and ‘grey beard’ engineer’s routine refactoring.

Reviewers typically match the situation’s who, what, when, and where to their own experience of where risk lies. \
The article seems to treat all diffs as equivalent inputs.

**Code review is bidirectional and constructive**

It seems to me that paper reduces knowledge transfer down to just information delivery; the agent simply ‘generates explanations.’ But discussion in a code review is a *joint* *cognitive activity*. The peer reviewer learns about the author’s approach, the author learns via the reviewers’ questions, and the result is a shared understanding that neither party had prior to the discussion.

This is **coactive** work, not simply a transmission. An agent’s summary isn’t a substitute for a conversation that changes both participants’ mental models.

**Operational context that lives outside repos**

> “We just had an incident in this service last Tuesday.”
>
> “The team that owns this downstream consumer is about to deprecate that interface.”
>
> “Legal told us not to log this field anymore.”

Human reviewers possess so much more contextual knowledge than they’re aware of, even though they can recognize connections in the wild. People understand the current state of the organization, recent events, and informal agreements that aren’t captured in tests, docs or version control, and they can recognize how these may influence the code under review. This happens so often that it’s all but invisible.

The article assumes the codebase *is* the complete context. It never is.

**Accountability for the code isn’t just a beuraucratic formality**

The article treats human responsibility as a compliance artifact, a “named human” for legal or other rule-related purposes. But being aware that you are personally responsible for approving a change shapes how you review it. It is the “skin in the game.” An agent that “signs off” on a pull request bears no consequences and certainly has no incentive structure that fuels an earnest evaluation. While the paper does include ethics concerns in its discussion section, it ends up redirecting it to “requirements engineering and post-deployment monitoring” which seems to me as hand-waving way of kicking the can down the road.

The most fundamental issue I have with the article is that it assumes code review is a first and foremost a **detection** process: you find defects, style violations, security issues, etc., and the assumption is that detecting these faster and cheaper is universally better.

But code review is also a *coordination* process, a *sensemaking* process, and a *governance* process.

The ***substitution myth*** often plays out in this same way:

1. First, decompose the human contribution of work into measurable functions.
2. Show that the machine can replicate this human contribution into measurable functions of its own.
3. Declare the human redundant.

This approach often falls apart at the same point: the human contribution that mattered most was the *integration* across functions. People’s ability to adapt to unplanned circumstances and contexts and serve the social accountability expected.

This ability to adapt in those situations aren’t accounted for in the original decomposition step #1, above.\
I don’t think they were accounted for in the original article, either.
