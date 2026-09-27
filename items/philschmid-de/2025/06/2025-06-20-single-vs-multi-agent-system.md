---
title: Single vs Multi-Agent System?
link: https://www.philschmid.de/single-vs-multi-agents
source: philschmid-de
published: 2025-06-20T00:00:00Z
updated: 2025-06-20T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Single vs. multi-agent? The real secret to building AI agents is 'read vs. write'. Learn which to use for your task and build reliable systems.
content: extracted
html: 2025-06-20-single-vs-multi-agent-system.html
preview:
  file: 2025-06-20-single-vs-multi-agent-system.preview-3de6fad0dce5.webp
  width: 256
  height: 144
  alt: Single vs Multi-Agent System?
  color: '#111216'
images:
- source: https://www.philschmid.de/static/blog/single-vs-multi-agents/thumbnail.jpg
  original:
    file: 2025-06-20-single-vs-multi-agent-system.image-bfb5997e20db.jpg
    width: 961
    height: 540
  color: '#040507'
- source: https://www.philschmid.de/static/blog/single-vs-multi-agents/cognition.png
  original:
    file: 2025-06-20-single-vs-multi-agent-system.image-2f734734d273.png
    width: 1406
    height: 1230
  color: '#f8f9fa'
- source: https://www.philschmid.de/static/blog/agentic-pattern/multi-agent.png
  original:
    file: 2025-06-20-single-vs-multi-agent-system.image-0258fa333921.png
    width: 1477
    height: 562
  variants:
  - file: 2025-06-20-single-vs-multi-agent-system.image-644c2aa5d368.webp
    width: 320
    height: 122
  - file: 2025-06-20-single-vs-multi-agent-system.image-c8119f292abc.webp
    width: 640
    height: 244
  - file: 2025-06-20-single-vs-multi-agent-system.image-17937f455f37.webp
    width: 960
    height: 365
  - file: 2025-06-20-single-vs-multi-agent-system.image-211721c2bc9e.webp
    width: 1280
    height: 487
  - file: 2025-06-20-single-vs-multi-agent-system.image-49c01ddf9b8d.webp
    width: 1477
    height: 562
  color: '#fdfdfd'
---

The debate on whats the right way to build Agents in the AI community heated up with Cognition's ["Don't Build Multi-Agents"](https://cognition.ai/blog/dont-build-multi-agents) and Anthropic's ["How we built our multi-agent research system"](https://www.anthropic.com/engineering/built-multi-agent-research-system). Despite their opposing titles both are surprisingly aligned. The choice between a single agent and a multi-agent system isn't about ideology, it is about picking the right tool for the right job.

Before we take a closer look, let's establish a simple definition.

> AI agent is a system that uses a Large Language Model (LLM) as reasoning engine to decide the control flow of an application.

## What Are Single Agents?

A single-agent system operates as a “single process”. Think of it as one highly-focused worker tackling a task from start to finish. It maintains a continuous thread of thought (memory) and action (tools) to ensure that every step is informed by everything that came before it.

![cognition-single-agent](https://www.philschmid.de/static/blog/single-vs-multi-agents/cognition.png)

\[Credits: [Cognition Don’t Build Multi-Agents](https://cognition.ai/blog/dont-build-multi-agents)\]

**Key characteristics:**

- **Sequential:** Actions are performed one after another. The agent completes step A before moving to step B.
- **Unified Contenxt:** Maintains a single, continuous history of the entire conversation. Every new step has access to all previous steps, thoughts, and tool outputs.
- **Stateful:** Decisions made early on directly and fully inform later actions without the need for message passing.

**Pros:**

- **Context Continuity**: No information is lost between steps
- **Simplicity**: Easier to debug, test, and maintain
- **Transparency**: Clear execution path and decision trail

**Cons:**

- **Sequential Bottlenecks:** Slow for tasks where parts could be handled in parallel.
- **Context Window Limitations**: Eventually exceed the (useful) context window, leading to errors and forgotten details.
- **Inefficiency**: May waste tokens on repetitive context and are limited to single model capabilities/instructions

## What Are Multi-Agent Systems?

A multi-agent system is structured like a team. It typically involves an “lead agent" that breaks down a goal into smaller subtasks, which it then delegates to multiple "worker" agents that can operate in parallel.

![multi-agent](https://www.philschmid.de/static/blog/agentic-pattern/multi-agent.png)

**Key characteristics:**

- **Parallel Execution:** Subtasks can be handled simultaneously by multiple agents.
- **Delegation:** A "lead" agent typically decomposes the main goal, delegates subtasks, and synthesizes the results from the worker agents.
- **Distributed Context:** Each agent operates with its own context, which is often a subset of the total information.

**Pros:**

- **Parallelization**: Can explore multiple paths simultaneously, reducing latency.
- **Specialization**: Each agent can be optimized and instructed for specific tasks
- **Breadth**: Can solve complex, multi-faceted problems.

**Cons:**

- **Context**: Sharing the right context between agents is hard
- **Coordination:** Agents may duplicate work or make conflicting decisions
- **Cost:** Can be more token-intensive. (Anthropic: 15x more tokens compared to a standard chat interaction)

***Note:*** It's worth mentioning other multi-agent pattern, e.g. swarm are not using a "lead" agent. Instead they collaborate in a more peer-to-peer fashion to solve a problem, which has both single agent and multi agent characteristics, e.g. Unified Context, but individual instructions but comes with its on challenges.

## Comparison Single Agents vs Multi-Agent Systems

| Aspect                 | Single Agent System                                  | Multi-Agent System                                                              |
| ---------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| **Context Management** | Continuous, no loss                                  | Complex sharing required                                                        |
| **Execution Speed**    | Sequential                                           | Parallel                                                                        |
| **Token usage**        | 4x chat tokens                                       | 15x chat tokens                                                                 |
| **Reliability**        | High, predictable                                    | Lower, emergent behaviors                                                       |
| **Debugging**          | Straightforward                                      | Complex, non-deterministic                                                      |
| **Best For**           | Sequential, state-dependent tasks ("write" tasks).   | Parallelizable, exploratory tasks ("read" tasks).                               |
| **Coordination**       | None needed                                          | Critical success factor                                                         |
| **Example Use Case**   | Refactoring a codebase, writing a detailed document. | Researching a broad market trend, identifying all board members of the S&P 500. |
| **Core Strength**      | Context Continuity & Reliability                     | Parallelism & Scalability                                                       |
| **Primary Challenge**  | Context Window Limits & Sequential Speed             | Context Fragmentation & Coordination Complexity                                 |

## The Universal Truths of Building Agents

Despite their opposing common principles apply for building serious Agentic system.

1. **Context Engineering is everything:** Architecting systems that dynamically maintain the right information at the right time for reliable decision-making is the key to success and reliability. This isn't just prompt engineering.
2. **"Read" vs "Write" Agents:** Are important distinction isn't single vs multi-agent. it is whether your task primarily involves **reading** (research, analysis, information gathering) or **writing** (code generation, content creation, file editing).
   - **Read tasks** are easier to parallelize and suite multi-agent better
   - **Write tasks** create coordination problems when parallelized, favoring single agents
   - **Mixed tasks** should separate read and write phases architecturally
3. **Reliable Agents Need Different Tooling:** Building reliable agents requires more than a good model. It requires a robust infrastructure for durable execution to survive failures, observability to debug behavior, and evaluation to measure what actually matters.
4. **Economic Viability and Model Improvements:** Models themselves are improving at an incredible rate. Don't over-engineer a solution for today that a much simpler can solve for you tomorrow.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
