---
title: Evaluating Agents Beyond the First Prompt
link: https://www.philschmid.de/evocode-bench
source: philschmid-de
published: 2026-07-27T00:00:00Z
updated: 2026-07-27T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: EvoCode-Bench tests coding agents across 227 sequential rounds in a persistent workspace. Single-turn scores overstate reliability — regressions, not missing features, are the real bottleneck.
content: extracted
html: 2026-07-27-evaluating-agents-beyond-the-first-prompt.html
preview:
  file: 2026-07-27-evaluating-agents-beyond-the-first-prompt.preview-727fee6cd55f.webp
  width: 256
  height: 134
  alt: Evaluating Agents Beyond the First Prompt
  color: '#efefef'
images:
- source: https://www.philschmid.de/static/blog/evocode-bench/thumbnail.jpg
  original:
    file: 2026-07-27-evaluating-agents-beyond-the-first-prompt.image-1253bfa929c0.jpg
    width: 1500
    height: 785
  color: '#fdfdfd'
---

Most coding benchmarks work the same. You give the agent a single task, let it work, check the result. The agent might run dozens of tool calls, but there's only one user input and one final evaluation.

That's not how most of us use agents. You build something, you get new ideas, requirements shift, you refactor... EvoCode-Bench is designed to test that cycle.

EvoCode-Bench is a new multi-turn coding benchmark with 26 tasks spanning 227 sequential rounds (5–15 rounds per task). It evaluates agents across five domains: ML/MLOps, build systems, data engineering, cloud/security, and scientific computing. Three things make it interesting:

1. **Persistent workspace:** One container lives across the entire lifecycle of a task. Code, dependencies, and architectural decisions from Round 1 are still there at Round 15.
2. **Evolving specifications:** Each new input brings a new instruction that either *extends* the codebase, *corrects* logic, or *conflicts* with earlier requirements (intentionally breaking previous assumptions).
3. **Cumulative testing:** After each turn, the test suite checks all requirements, not just the new one. Break something from a previous turn, and you fail.

```plaintext
Single-Turn Eval (SWE-bench):
  [Prompt] → [Agent works] → [Pass/Fail] → Container discarded
 
Multi-Turn Eval (EvoCode-Bench):
  [Prompt 1] → [Agent works] → [Test 1] →
  [Prompt 2] → [Agent works] → [Test 1+2] →
  [Prompt 3] → [Agent works] → [Test 1+2+3] → ...
```

## Example: Building a CLI Tool Over 8 Rounds

One task (`d1_w5`) asks the agent to build `buildctl`, a CLI build orchestrator in Go, over 8 rounds. Here's how the first few rounds work:

**Round 1:**

- **User prompt:** *"Build a CLI tool called `buildctl` in Go that orchestrates multi-target build pipelines with dependency resolution, parallel execution, and content-addressable artifact caching."* The full spec defines TOML config parsing, topological sorting, cache key computation (SHA-256 of command + env + input file hashes), 4 CLI commands (`build`, `graph`, `clean`, `status`), JSON report format, and error handling for cycles, missing deps, and timeouts.
- **Agent:** Scaffolds the Go module, implements the pipeline engine, writes the CLI.
- **Verification:** The framework mounts `round-1/tests/test.sh` (24 test cases) and runs it against the CLI. Test scripts are removed after verification so the agent can't read future grading criteria.

**Round 2:**

- **User prompt:** *"Add pipeline composition and variable substitution."* Specifically: a new `imports` field in TOML for composing pipelines across files (with transitive import support, cycle detection, name collision errors), and a `[variables]` section with `${VAR_NAME}` expansion in commands, inputs, and outputs.
- **Agent:** In the same persistent container and codebase, adds import resolution and variable expansion.
- **Verification:** The test suite now checks **both** Round 1 requirements (caching, sorting, error handling) **and** the new import/variable features.

**Round 5:**

- **User prompt:** *"Change build to fail-fast by default. When any target fails, don't start additional targets. Unstarted targets get status `cancelled` (distinct from `skipped`). Add `--keep-going` flag to restore the old behavior."*
- This reverses Round 1, which said independent targets should keep running when a sibling fails. The agent has to refactor the execution engine, add a new status type to the JSON report, and update the summary counts, all without breaking imports, variables, hooks, or cache key computation from earlier rounds.
- **Verification:** The test suite still checks all prior requirements (caching, sorting, imports, variables), but the failure-handling tests from Round 1 are adapted to match the new spec.

## Key Insights

**Single-turn scores dramatically overstate reliability.** The researchers tested two modes: one where each round starts from a clean, human-completed codebase (SR), and one where the agent maintains its own workspace across rounds (MT@4). Agents are good at following instructions on clean code. They're significantly worse when building on their own past work. For lower-tier models the gap is ~4x (8.4% MT → 33.1% SR); for frontier models it's still 1.4–1.8x. Over 57% of multi-turn failures occur on rounds that models solve easily from a clean state.

**Rankings shift under pressure.** Claude Opus 4.6 had the *highest* single-round score (78.9%) but drops to third in multi-turn (44.0%), behind Opus 4.7 (54.0%) and GPT-5.5 (52.4%). Across all models, pass rates go from 46.7% at Round 1 to 21.3% by Round 5 to 7.7% by Round 10. More turns, more accumulated decisions, more failures.

**Failure modes differ by model tier.** Lower-tier agents fail early, they simply don't implement what was asked (87–90% of failures). Mid-tier agents handle the initial rounds but when the spec changes, they add the new logic without fully removing the old behavior it replaced (28.6% of failures, peaking at Round 5). Top-tier agents get further but eventually break something that was already working, regressions account for 35% of failures by Round 2.

**Regressions are the real bottleneck.** Agents rarely fail because they can't build the feature. They fail because they break something that was already working. When the spec changes, the agent edits code to handle the new requirement, but those edits touch shared code paths and accidentally break earlier behavior. The agent doesn't always re-verify that the old stuff still works.

**Structure helps.** Agents that tracked requirements in a persistent document (like a project plan or spec file) more than doubled their success rates.

## Limitations

**Small dataset.** 26 tasks, 227 rounds. A few hard tasks can skew the aggregate scores.

**All-or-nothing scoring.** A single regression zeros out the entire round, even when 98% of test cases pass. The paper also reports per-case scores showing agents often achieve >80% assertion accuracy on rounds scored as binary failures.

**No recovery loops.** Under fail-stop scoring (MT@4), a single failed round terminates the entire trial — all remaining rounds automatically score zero. The benchmark doesn't test whether an agent can recover from a broken state.

**Static interaction.** The "user" is a fixed script. No clarification questions, no "that's not what I meant," no ambiguity.

**Infra-sensitive.** The paper's own results show infrastructure issues (container throttling, kubelet errors) can corrupt scores.

## Bottom Line

EvoCode-Bench tests whether an agent can sustain a codebase over time — with cumulative test suites that grow from ~900 lines of bash at Round 1 to nearly 4,000 by Round 5. Even the best models lose more than half their capability by Round 5. The dominant failure isn't "can't build the feature" — it's regressions, breaking something that was already working. When specs reverse earlier decisions, even frontier models struggle. The clearest signal: agents that maintained a persistent requirements document more than doubled their success rate. Structure beats raw capability.

- Paper: [EvoCode-Bench: Evaluating Coding Agents in Multi-Turn Iterative Interactions](https://arxiv.org/abs/2605.24110)
- Code & Dataset: [UniPat-AI/EvoCodeBench](https://github.com/UniPat-AI/EvoCodeBench)
- Leaderboard: [Results](https://unipat-ai.github.io/EvoCodeBench/)
