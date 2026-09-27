---
title: Brownfield Agentic Engineering
link: https://addyosmani.com/blog/brownfield-agentic-engineering/
source: addyosmani-com
published: 2026-09-14T00:00:00Z
updated: 2026-09-14T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: What it takes to run agents in a codebase older than the team
content: extracted
html: 2026-09-14-brownfield-agentic-engineering.html
preview:
  file: 2026-09-14-brownfield-agentic-engineering.preview-1527fd9e4514.webp
  width: 256
  height: 143
  color: '#353737'
images:
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/11.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-2be4fbecab63.webp
    width: 1600
    height: 892
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-9678df64b77f.webp
    width: 320
    height: 178
  color: '#030708'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/01.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-c889eba8ef5b.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-82469f61a39e.webp
    width: 320
    height: 180
  color: '#151513'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/02.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-5f4328bc601e.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-56b3be719a33.webp
    width: 320
    height: 180
  color: '#151414'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/03.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-8542efcbc385.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-4ddeaaeb0e6f.webp
    width: 320
    height: 180
  color: '#181615'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/04.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-f7defd916b6d.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-6997cd305a47.webp
    width: 320
    height: 180
  color: '#0c0c0c'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/05.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-4078905a5a94.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-3abc8a8f5c0d.webp
    width: 320
    height: 180
  color: '#171414'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/06.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-dc8004977d94.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-201c44b052ba.webp
    width: 320
    height: 180
  color: '#0c0c0c'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/07.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-38d2cbe6b0f6.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-4ce70b72e026.webp
    width: 320
    height: 180
  color: '#171514'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/08.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-43cbde57303d.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-59806a5359c6.webp
    width: 320
    height: 180
  color: '#151415'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/09.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-1ea586df650b.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-ea0a9eb1e6ce.webp
    width: 320
    height: 180
  color: '#161515'
- source: https://addyosmani.com/assets/images/brownfield-agentic-engineering/10.webp
  original:
    file: 2026-09-14-brownfield-agentic-engineering.image-37cfdcd42afe.webp
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-brownfield-agentic-engineering.image-8b866b39ccbf.webp
    width: 320
    height: 180
  color: '#171413'
---

**Agentic engineering in an old codebase is about making hidden constraints visible and cheap changes trustworthy. Let’s talk what to do in brownfield codebases.**

During my career I’ve worked on teams whose codebases had been around a long time. Those are brownfield systems: the repository is no longer a complete description of how the thing actually behaves. Institutional knowledge, duct tape, legacy services, and expectations other teams depend on live outside the tree. You have to learn those constraints before you write new code, and you have to prove a change didn’t break them. I love coding with agents, but throw them at an older brownfield codebase unsupervised and you may end up with something that “works” but with the wrong system design and brittle tests.

Even in teams that wanted to do modernization efforts pre-AI, you often had to take things very, very piecemeal, with strong testing in place, a strong layer of confidence to make sure that you weren’t breaking things. You kind of knew that on top of actual user journey testing, any migrations you were making had to keep things working as intended via a barrage of repeatable tests. These days some folks may say that as soon as an agent drops code you didn’t author decision-by-decision, you’re already in a brownfield project. Regardless, you want to optimize for cheap changes being made safely.

And these days, especially in the last, I would say, maybe five to ten years, this idea of caring more about testing, caring more about verification, caring more about how you make changes in a way that is not going to break things, I feel has gotten more attention. But that doesn’t change the fact that if you’re doing a lot of work trying to introduce agentic engineering, and then software factories and all of these other kinds of patterns for autonomously working through these large codebases, you have to put quite a bit of additional mindfulness in place otherwise you risk signing up for a world of technical debt.

**Before we dive in, let’s assume that the code should be the source of truth. Anything we add on top to help brownfield is what can’t be easily inferred. I want to talk about this in terms of zones, blast radius and a few other patterns I think will help.**

## Zones

**If I’m going into an older codebase that’s been around for a while, I probably want to get a sense of what code shouldn’t I be touching.** You can consider these zones. E.g. Green zone = safe/good tests/isolated, yellow = mixed quality, red = sensitive/auth/billing/permissions.

![Slide titled Zone the codebase before the agent touches it. A person draws the map; the zone decides what an agent is allowed to do there. Green, low risk, means good test coverage, current conventions and isolation, and the verb is a tight loop where agents iterate on their own. Yellow, mixed, means mixed quality and coverage you cannot fully trust, and the verb is tests first: pin today's behavior before any change. Red, sensitive, covers auth, billing, permissions and payroll that few people know, and the verb is pair or don't, meaning a human on every step or no work. Three rules make the zones a procedure rather than a metaphor: a person draws the map, zones move only when earned, and the zone sets the verbs.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/01.webp)

What are the parts of the codebase that are very, very sensitive, or that not everybody understands well? And maybe you would draw those with different zones. Maybe you have a green area that’s got very good test coverage, and is using modern conventions that are current, and has good isolation. And for those parts of the system, agents can go off and work on that in a tight loop.

There are sites, especially commerce sites that I’ve worked with, where you could easily have five or six departments all with their own microsites, when the entire experience to the end user is going to feel like a single thing. And there’s actually a lot of inherent complexity underneath the surface. One team might have really good test coverage for their stuff; maybe it was built in the last couple of years. Other teams may not. So you have this green zone.

Maybe you have yellow, which is mixed quality, maybe it’s a mix of things, and agents can change code there after characterization tests have been written.

And then you can have red areas, where you’ve got sensitive stuff like authentication, billing, permissions, payroll, anything that you wouldn’t normally touch and make some hasty changes to. For example, if only a small number of people understand how it all works. You don’t want unsupervised rewrites in that kind of system.

Three rules make the zones an operating procedure instead of a metaphor. A person draws the map, not the agent; left to choose, the agent starts in the scariest file, because the scariest file has the most interesting names. Zones only move when it’s earned: yellow becomes green once characterization tests exist and the module’s owner has reviewed the agent’s first changes. And the zone sets the verbs: green is a tight loop, yellow is tests first, red is a human pairing on every step or the work not happening.

## Write down what the code can’t say

**Autonomy should follow blast radius, observability, and recoverability. A model’s confidence is a poor guide.**

So I think it makes sense to have at least a sense of, how do you think about the map of the world, and what can the agent infer itself from the codebase? **Agents can actually infer quite a lot from the code itself.** There was this period of time when people would try to include markdown files for absolutely everything, and then they’d stuff them in their context windows. Agents are actually pretty good at understanding the map of the system. **What you want to give them is the stuff that is not obvious from the code itself.** Are there conventions? Are there patterns? Are there nuances that are not in there? I think that’s important.

Concretely, that means: business or team specific nuance, trade-offs that explain *why* the system is structured a certain way, guidelines that aren’t explicitly enforced by static analysis or tooling, domain-specific domain rules, external constraints and historical context behind counter-intuitive implementations and so on.

**Write down what the code can’t say, and nothing else.**

![Slide titled Write down what the code can't say. Agents infer a lot from the repository itself, so spend your prose on the rest and nothing else. What the agent can already read: module structure and the call graph, the naming and patterns already in use, how the tests are organized, what the types enforce, and what the linter already catches. Worth writing down instead: business and team-specific nuance, why it is built this way and the trade-off, guidelines no tool enforces, domain rules, external constraints, and the history behind code that looks wrong. A footer notes that how much autonomy a change gets should follow blast radius, observability and recoverability, not the model's confidence.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/02.webp)

## Make the research survive the session

**If your agent’s exploration produces no durable artifact, the next agent pays for the same archaeology again.**

One piece I would add to that map is a durable research artifact. For yellow and red work, I like a separate read-only pass that produces a short comprehension memo: entry points, owners, callers, existing abstractions, tests, production signals, relevant history, and open questions. Claims should cite a file, issue, ownership record, or dashboard.

The default loop otherwise wastes its research. The agent works out how the auth flow behaves, completes the task, and loses that model when the session ends. Chat history isn’t a great system of record, especially after compaction.

After research, I would start planning with a clean context. Ask which files the plausible approaches touch, which invariants they preserve, and how you would reverse them. A human picks the path. Implementation should stop if it discovers the map was wrong. Review starts fresh and works backward from the acceptance criteria. A clean reviewer is more likely to notice when a test proves the implementation while missing the requirement.

![Slide titled Make the research survive the session. If exploration leaves no durable artifact, the next agent pays for the same archaeology again. A five-stage flow: research, a read-only pass with no edits allowed; the memo, a durable artifact that outlives the chat; plan, with fresh context, where a human picks the path; implement, which halts if the map turns out to be wrong; and review, with fresh context, working back from the criteria. The comprehension memo holds entry points, owners, callers, existing abstractions, tests, production signals, relevant history and open questions, and every claim cites a file, an issue, an ownership record or a dashboard. Planning asks which files each approach touches, which invariants it preserves, and how you would reverse it. Caption: chat history isn't a system of record, especially after compaction.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/03.webp)

## When instructions become a harness

**Every repeated correction is a missing piece of the harness.**

It is useful to be precise about where the pieces fit. Instructions record unusual facts about a repository. Skills package reusable procedures such as checking blast radius or verifying a schema change. Plugins can provide governed access to the ownership catalog, incident archive, or dashboards.

The harness is the working environment around the agent: context, tools, permissions, tests, logs, and recovery. A factory schedules many dependable loops, keeps durable state, and hands novel cases back to people.

The practical test is what happens when the agent gets something wrong. If you quietly repair the diff, the next session can repeat it. When the same review comment appears again, move it into a lint rule, hook, type, test, or skill. Keep prose for constraints that cannot be enforced mechanically.

A deny rule, scoped credential, or CI check doesn’t have to remember. Over time the harness becomes a record of failures the team has decided not to pay for twice.

![Slide titled Every repeated correction is a missing piece of the harness. A ladder running from someone has to remember to nobody has to remember: you quietly fix the diff, which is forgotten so the next session repeats it; a review comment, which shows up again next week; a line in the instructions, which the model reads but can still lose; a skill, a reusable way to say check blast radius; a lint rule, hook, type or test, which runs every time, unattended; and deny rules, CI and scoped credentials, which don't have to remember anything. Below: instructions record unusual facts about the repository, skills package reusable procedures such as verifying a schema change, plugins give governed access to ownership catalogs, incidents and dashboards, and the harness is context, tools, permissions, tests, logs and recovery. Caption: over time the harness becomes a record of failures the team decided not to pay for twice.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/04.webp)

## Start with zero-risk work

**Lock today’s behavior before you let anything improve it.**

If you’re bringing agents into an existing codebase, it’s very similar to other kinds of modernization efforts. Maybe you begin with zero-risk work. It shouldn’t be like, hey, let’s rewrite this monolith in Rust or something like that. Maybe it’s, first explain how the things work.

**Generate characterization tests that can lock that current behavior.**

> Characterization tests are automated tests used to document a system’s actual current behavior so you can safely refactor or change legacy code

By characterization tests I mean tests that pin down what the module does today, ugly parts included, because in an old system some of that ugly behavior is what the business runs on, and an agent will happily “fix” it behind a green suite. The machinery is old because the problem is old. Netflix used the same idea at production scale in its GraphQL cutover - replay and shadow traffic against the old and new paths, diff the payloads, promote only when they match. That is the promotion path when a homepage-class surface has no honest unit suite: don’t guess; run both and compare.

When an agent is the one making them pass, don’t let that same session be the only author of the tests. Pin the behavior first, in a separate pass or by a person; then let the agent work. Otherwise you get a green suite that encodes the implementation you just invented.

And then you start down the path of doing mechanical transforms. You can do dead code and unused export inventories. You don’t want to start with the trickiest or hairiest parts of the system. And ultimately you want to have that confidence with any of these migrations.

![Slide titled Start with zero-risk work: lock today's behavior before you let anything improve it. A progression from start here to not first: explain it, asking how it works today with no edits, just the map; pin today's behavior with characterization tests, ugly parts included; mechanical transforms such as codemods, renames and moves, diffable and boring; inventories of dead code and unused exports, lists rather than changes; and only later the hairy parts, which is not a Rust rewrite of the monolith on day one. Ugly parts are included because in an old system some of that ugly behavior is what the business actually runs on, and an agent will happily fix it while the suite stays green. One session should not both write the tests and make them pass, or the green suite just encodes the implementation it invented, so pin behavior in a separate pass or by a person. Caption: agents don't remove the dozens-of-departments problem, they make it cheaper to attempt a change against it.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/05.webp)

I remember working on a number of different kinds of migrations over my time on large codebases, and people exercising a great deal of care, even when fixing things that were broken.

One of the older codebases I worked on was at AOL. There was a day when I was supposed to be off, and I was visiting a comic book store near the office, and as it so happened, my boss dropped me a text and asked if there was any way I could swing by. The AOL.com homepage was completely broken, and we didn’t have enough JavaScript experts around to go and figure it out. So I said, okay, sure, I’ll come in and take a look. And you would think these days, oh, a homepage, how complicated can it be? But when you have dozens and dozens of departments of people that can own lots of different components, lots of different criteria, lots of different scripts, A/B tests, all of these things, you want to avoid breaking the world for everybody else, because you’re not necessarily going to have test coverage all over the place in the same way that you would like. In that case I was able to get it fixed, but we basically had to at least user-test the things that didn’t have their own unit tests. How well were things working, without breaking for everybody? So that was kind of important.

That’s still the job. Agents don’t remove the dozens-of-departments problem; they make it cheaper to attempt a change against it. A surface that only production traffic really understands is a red zone by definition, and until you’ve built a stand-in for that traffic, the user-testing I did on my day off is still the gate.

![Slide titled No honest test suite? Run both paths and compare. A request, real or replayed, forks to a control, the old path whose behavior you already trust, and a candidate, the new path under test. The old answer is returned so users see nothing change, while in the background the old result is compared against the new one. Every diff is a finding that goes to a mismatch log to fix before running the traffic again, and the gate is to promote only when the payloads match. Two examples: GitHub's Scientist library sends the call to both paths, returns the old answer and diffs the new one in the background; Netflix's GraphQL cutover replayed and shadowed traffic against both, diffed the payloads, and promoted only when they matched. Caption: a surface only production traffic really understands is a red zone by definition.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/06.webp)

## Migrate in complete units

**A migration is complete when the new path works and the old dependency is demonstrably gone.**

Half-finished migrations are particularly confusing to agents. Search returns the old approach in forty files, the replacement in twelve, and a shim that presents both as current. The agent sees contradictory precedent.

I would rather finish one route end to end, including removing the old path, than convert thirty files and leave both patterns alive. If deletion is a future cleanup ticket, the migration unit is not complete.

Tests can stay green while a replacement still calls the legacy implementation. [SWE Refactor Bench](https://arxiv.org/abs/2608.23564) calls this migration “Blindness.” Across 520 agent runs, only 28 passed its migration audit, behavioral tests, and independent verification.

If a codemod can make the routine change, use the agent to help write and check it. Give agents the exception queue. Stripe’s migration is useful here precisely because no agents were involved: the durable artifact was the migration machine.

![Slide titled Migrate in complete units: complete means the new path works and the old dependency is demonstrably gone. In a half-finished migration the agent finds the old approach in 40 files, the replacement in 12, and a shim presenting both as current, which is contradictory precedent. One complete unit is one route end to end: the new path serves the route, the old path is deleted rather than deprecated, no shim is left behind, and nothing still calls the legacy code. If deletion is a future cleanup ticket, the unit isn't complete. Below, SWE Refactor Bench data on what the paper calls blindness: tests stay green while the replacement still quietly calls the legacy implementation, and of 520 runs, 340 passed the migration audit but only 28 passed all three stages of migration audit, behavioral tests and independent verification, which is 5.4% of runs across 8 frontier models. Caption: if a codemod can make the routine change, use it, and give the agents the exception queue.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/07.webp)

## The lessons from bigger migrations

[Bun’s Zig-to-Rust port](https://bun.com/blog/bun-in-rust) ran [about 50 workflows over 11 days](https://bun.com/blog/bun-in-rust) from a 535,000-line codebase, with two adversarial reviewers on every generated unit and the entire pre-existing test suite as the merge gate; the part worth copying is that hours went into a porting guide mapping Zig idioms to Rust before any agent ran. Anthropic’s own [migration process](https://claude.com/blog/ai-code-migration) stress-tests its rulebook on a disposable mini-migration and throws the trial output away before the broad run begins.

A [controlled VB6-to-C# study](https://arxiv.org/abs/2608.28972) measured 92% behavioral equivalence on simple features and 47% on complex ones: unit size is the lever. The shape predates agents entirely: Stripe moved [3.7 million lines to TypeScript in one PR](https://stripe.dev/blog/migrating-to-typescript) through months of codemod work, with no agents involved, and Google’s [large-scale-changes chapter](https://abseil.io/resources/swe-book/html/ch22.html) explains why atomic changes shrink as codebases grow. Spotify now reports 650-plus agent PRs merged monthly on rails Backstage built years earlier.

[Asana](https://openai.com/index/asana/) cleared a multi-year Enzyme backlog in two calendar weeks for about $12,000 in model and infrastructure cost. That $12,000 is just a token bill but not a substitute for the five-year staffing estimate they had on the books; treat it as a vendor-reported cost of generation, not a controlled savings study. The transferable part is the same as Bun: a narrow mechanical migration, a pre-existing suite, humans still reviewing every change

**What transfers between companies is the structure around the agents.**

![Slide titled The receipts from bigger migrations: what transfers between companies is the structure around the agents, and the headline number rarely does. Bun's Zig to Rust port moved 535,000 lines in 11 days across about 50 dynamic workflows, with two adversarial reviewers per implementer and a merge gate of 100% of the existing suite on every platform; Bun is Anthropic-owned, and Zig's Andrew Kelley disputes what the tests prove. A controlled VB6 to C# study measured 92% behavioral equivalence on simple features and 47% on complex ones, averaging 70% across 12 features, with complex features also burning about six times the tokens, so unit size is the lever. Stripe moved 3.7 million lines to TypeScript in a single pull request with no agents involved, after months of building and iterating on the codemod, and the durable artifact was the migration machine. Spotify merges 650-plus agent pull requests a month on rails Backstage and Fleet Management had already laid down. Asana cleared an Enzyme to React Testing Library backlog in two weeks for about $12,000 against a five-year estimate, a vendor-reported token bill rather than a controlled savings study. The conclusion: copy the structure, not the headline.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/08.webp)

## What’s actually changed

**Agents have changed the price of trying several plausible implementations. They haven’t changed the evidence required to choose one.**

And then I think you’ve probably seen, this year we’re beginning to read more and more cases of well-established companies who are using agents to do big rewrites. I’ve talked to CTOs who are allowing teams to have agents try multiple rewrites in different languages or frameworks because its now feasible to do so more cheaply and evaluate the trade-offs.

Shopify rebuilt the Shop consumer app from React Native to native Swift and Kotlin in twelve weeks with a small team and agent-gated, screen-sized checkpoints. The much larger merchant app is still the brownfield problem: hundreds of screens, deep platform integration, same gates, longer clock.

You’ve seen other examples of rewrites to Rust. You’ve seen people do framework-level migrations. There have been all kinds of migrations that have been done. And in many cases, these are migrations people would have done on a much longer timeframe. These days, if you have enough tokens, you can just actually have agents go and attempt to complete a migration across a range of different stacks or languages.

You can try to have your agents actually implement something in a number of different competing options. Rather than having one team choose a single option that you go all in on, what you do is you have them implement all of them. They can all check against your unit tests. You can performance profile all of them, and then make a decision, which is much, much cheaper in some cases than it otherwise would have been. And that’s a completely different ball game, I think, for teams these days.

## Parallelize last

**More generated code should lead to more selective human review, not less human ownership. Seriously consider what will setup your brownfield project for success before you go down the path of thinking about the loops/goals/parallelization.**

Software factories can run many changes at once. I would copy that part only after one unit has a dependable judge, recovery path, and review format people can absorb.

Parallelism multiplies the bottleneck you already have. Automated verification can handle five checked changes. One senior reading every line gets a queue, fragmented attention, and eventually ceremonial approval.

I prefer automated review to lead with intent, changed invariants, test results, parity mismatches, and the rollback route. The complete diff remains available. Human attention goes first to the largest blast radius and weakest oracle.

Worktrees isolate changes, not behavior. They may share Git metadata, credentials, local services, and network access. Trusted work may accept that tradeoff. Unattended agents consuming untrusted content need stronger sandboxes and scoped credentials.

![Slide titled Parallelize last: parallelism multiplies whatever bottleneck you already have. Before you fan out, one unit needs a dependable judge that can say no reliably, a recovery path that is cheap when it's wrong, and a review format people can absorb rather than a wall of diffs nobody finishes reading. The same five changes meet two different bottlenecks: automated verification handles them, while one senior reading every line produces a queue, fragmented attention, and then ceremonial approval. Automated review should lead with intent, changed invariants, test results, parity mismatches and the rollback route, and only then the full diff, with human attention going first to the largest blast radius and the weakest oracle. A note warns that worktrees isolate changes, not behavior: they can share git metadata, credentials, local services and network, and unattended agents on untrusted content need real sandboxes. Caption: more generated code should lead to more selective human review, not less human ownership.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/09.webp)

## Agents put a price on ambiguity

Lines generated don’t tell you whether the codebase improved. I would track lead time, review minutes, human interventions, escaped defects, rollbacks, oracle mismatches, and suppressions left behind.

For a migration, track remaining old imports, traffic served by the new path, parity mismatches, and legacy dependencies removed. A green suite with all traffic still taking the old path is busywork.

**Agents put a visible price on ambiguity. Tribal conventions become recurring review comments.**

That cost was always there, paid during onboarding, review, and incident recovery. Agents make more of it countable. That gives us a stronger argument for maintenance work teams already knew was valuable.

The next time an agent works on the homepage equivalent, I would want it to leave behind more than the repair: a synthetic user journey, an ownership record, and a regression test.

What the next engineer and agent inherits matters too.

![Slide titled Agents put a visible price on ambiguity: tribal conventions become recurring review comments, the cost was always there, and now you can count it. Lines generated is crossed out because it says nothing about whether the codebase actually improved, and neither does a green suite on its own. For any agent work, track lead time, review minutes, human interventions, escaped defects, rollbacks, oracle mismatches and suppressions left behind. For a migration, track old imports remaining, traffic on the new path, parity mismatches and legacy dependencies removed. The trap is a green suite with all traffic still on the old path, which is busywork. Besides the fix, the agent should leave behind a synthetic user journey, an ownership record and a regression test, because what the next engineer and the next agent inherit matters too.](https://addyosmani.com/assets/images/brownfield-agentic-engineering/10.webp)
