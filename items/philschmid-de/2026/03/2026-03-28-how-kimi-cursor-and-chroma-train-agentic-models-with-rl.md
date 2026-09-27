---
title: How Kimi, Cursor, and Chroma Train Agentic Models with RL
link: https://www.philschmid.de/kimi-composer-context
source: philschmid-de
published: 2026-03-28T00:00:00Z
updated: 2026-03-28T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn the unique ways how Kimi, Cursor, and Chroma train agentic models with RL.
content: extracted
html: 2026-03-28-how-kimi-cursor-and-chroma-train-agentic-models-with-rl.html
preview:
  file: 2026-03-28-how-kimi-cursor-and-chroma-train-agentic-models-with-rl.preview-e851050e0a91.webp
  width: 256
  height: 131
  alt: How Kimi, Cursor, and Chroma Train Agentic Models with RL
  color: '#262626'
images:
- source: https://www.philschmid.de/static/blog/kimi-composer-context/thumbnail.jpg
  original:
    file: 2026-03-28-how-kimi-cursor-and-chroma-train-agentic-models-with-rl.image-f0fc9b9da06d.jpg
    width: 1500
    height: 768
  color: '#000000'
---

I read three recent technical reports from Moonshot AI's [Kimi K2.5 paper](https://arxiv.org/html/2602.02276v1), Cursor's [Composer 2 report](https://arxiv.org/html/2603.24477v2) and [blog post](https://cursor.com/blog/real-time-rl-for-composer), and Chroma's [Context-1 write-up](https://www.trychroma.com/research/context-1).

Each report introduces something distinct. Kimi K2.5 trains an **Agent Swarm** where the model learns to decompose tasks into parallel sub-agents through RL. Cursor's Composer 2 uses **self-summarization** to handle long coding sessions and runs **real-time RL** from production traffic. Chroma's Context-1 teaches the model **self-editing context**: actively pruning retrieved documents to free up space for further search.

All three use reinforcement learning with similar methodology:

1. **Start from a strong base model.** None of them train from scratch. Moonshot extends Kimi K2 with multimodal pre-training. Cursor starts from Kimi K2.5 (1T parameters/32B active MoE). Chroma from gpt-oss-20B.
2. **Train inside the production harness.** Each team runs RL rollouts through the same tools, prompts, and execution environments that their model encounters in production.
3. **Outcome-based rewards.** All three use verifiable outcome signals and exceptionally Generative Reward Models (GRMs) for open-ended tasks/style/constitutions.
4. **Asynchronous, large-scale rollouts.** Each system generates parallel trajectories per training step. Agent rollouts are expensive, all invested in infrastructure to run them at scale.

## Kimi K2.5: Agent Swarm and Parallel Agent Orchestration Through RL

**Paper:** [Kimi K2.5: Visual Agentic Intelligence](https://arxiv.org/html/2602.02276v1)

Kimi K2.5 is Moonshot AI's multimodal model with a 1T parameter / 32B active MoE architecture. Its most distinctive feature is **Agent Swarm**, a framework where the model learns to dynamically decompose tasks into parallel subtasks and dispatch them to sub-agents. The parallelization strategy is learned through reinforcement learning, not hand-coded.

### PARL: Parallel-Agent Reinforcement Learning

Most agentic systems execute sequentially: think → tool call → observe → think → tool call. Agent Swarm breaks this by training the model to spawn parallel sub-agents.

The architecture has two roles:

- **Orchestrator** (trainable): Decides when to create sub-agents, what tasks to assign them, and how to aggregate their results. Equipped with `create_subagent` and `assign_task` tools.
- **Sub-agents** (frozen): Execute assigned subtasks independently. Their trajectories are excluded from the optimization objective.

This decoupling solves the credit assignment problem. In end-to-end co-optimization, a correct final answer could mean the orchestrator decomposed well, or that a sub-agent got lucky. Freezing sub-agents and treating their outputs as environmental observations means only the orchestrator's coordination logic gets optimized.

Kimi additionally introduces "critical steps" to measure computational cost in a parallel setting, analogous to the critical path in a computation graph. Instead of counting total steps across all agents, critical steps measure the longest execution chain. For each stage, the cost is the max steps among all parallel sub-agents. Total critical steps sum these maxima. This incentivizes the orchestrator to balance work across sub-agents (reducing the longest branch) rather than just maximizing concurrency.

### The PARL Reward

Training a reliable parallel orchestrator requires reward design. The PARL reward has three components:

1. **Performance reward** (`r_perf`): Did the task succeed? This is the primary signal.
2. **Parallelism reward** (`r_parallel`): Incentivizes sub-agent instantiation to prevent "serial collapse", a local optimum where the orchestrator defaults to single-agent execution and never explores parallel strategies.
3. **Finish reward** (`r_finish`): Rewards completed subtasks to prevent "spurious parallelism", a reward-hacking behavior where the orchestrator spawns many sub-agents without meaningful task decomposition, just to collect `r_parallel`.

The auxiliary reward coefficients are annealed to zero over training, so the final policy optimizes solely for performance.

### How It Works at Inference

At inference time, the model receives a task and decides whether and how to parallelize:

1. The orchestrator analyzes the task and identifies subtask structure.
2. It creates sub-agents with specific instructions using `create_subagent`.
3. It assigns tasks to sub-agents using `assign_task` in parallel. Sub-agents execute concurrently with independent context windows.
4. The orchestrator collects results and synthesizes a final answer or repeats the process.

The decision to parallelize is not hard-coded. On simple tasks, the model works sequentially. On complex multi-source research tasks, it spins up many parallel agents. The training distribution encourages this: synthetic prompts emphasize either "wide search" (many independent information sources) or "deep search" (multiple reasoning branches with delayed aggregation). The prompts don't instruct the model to parallelize. They create tasks where parallelism helps.

Agent Swarm reduces inference latency by up to 4.5× while improving accuracy. On BrowseComp, it achieves 78.4% (vs. 60.6% single-agent), surpassing GPT-5.2 Pro (77.9%). On WideSearch, item-level F1 improves from 72.8% to 79.0%. Agent Swarm also functions as proactive context management, decomposing tasks into isolated sub-agent contexts avoids the context overflow that plagues long sequential runs.

### The Broader Training Pipeline

Kimi K2.5's RL recipe has several other components worth noting:

- **Rule-based outcome rewards** for tasks with verifiable solutions (reasoning, agentic tasks).
- **Generative Reward Models (GRMs)** for open-ended tasks, these are not binary pass/fail judges but fine-grained evaluators aligned with internal quality criteria (helpfulness, aesthetic quality, instruction following). Multiple alternative GRM rubrics mitigate reward hacking.
- **Rejection-sampling fine-tuning (RFT)** creates a self-improving data pipeline: successful RL trajectories are extracted and used as SFT data for subsequent training stages, each building on the last.
- **Toggle** for token efficiency: alternates between budget-constrained and standard scaling phases during training, cutting output length by 25-30% with negligible performance loss.

## Cursor Composer 2: RL for Agentic Coding

**Papers:** [Composer 2 Technical Report](https://arxiv.org/html/2603.24477v2), [Improving Composer through real-time RL](https://cursor.com/blog/real-time-rl-for-composer)

Composer 2 is Cursor's in-house model for agentic software engineering. It can read and edit files, run shell commands, search codebases, and browse the web. The goal is to solve real coding tasks autonomously.

### A Fixed Harness, Close to Production

Composer 2 trains inside the exact same Cursor harness that users interact with: same tools, same prompt format, same system message, same file context. They maintain a shadow deployment of the Cursor backend during training so that tool behaviors (like semantic search) work identically to production.

Public benchmarks like SWE-bench use simplified environments and over-specified prompts. Real developer requests are under-specified, messy, and admit multiple valid solutions. Training in the production harness on problems drawn from actual Cursor usage means Composer 2 learns to handle the real distribution.

They also built **CursorBench**, an internal evaluation suite of tasks pulled from actual coding sessions by their engineering team. CursorBench tasks have a median of 181 lines changed (compared to 7-10 on SWE-bench) and much shorter, more ambiguous prompts. This benchmark co-evolves with the product: as users push agents harder, the benchmark grows more complex.

### The RL Recipe: Four Components

Composer 2's RL infrastructure consists of four decoupled services:

- **Training**: A fully asynchronous stack built on Ray and PyTorch.
- **Environments**: Each rollout runs in a dedicated Firecracker VM on their internal platform (Anyrun), capable of running a full development environment with browser and GUI. Anyrun can schedule 500+ pods per second and supports filesystem-level snapshotting and forking of environments, useful for mid-trajectory checkpointing.
- **Inference**: Partnered with Fireworks AI for RL inference. Weight syncs happen every training step via delta-compressed uploads to S3 with sharding across training ranks, enabling world-scale distributed inference. Inference workers can update weights mid-rollout, keeping later tokens more on-policy.
- **Evaluations**: Pinned production backend and Cursor client replicas for evaluation, giving high confidence that eval behavior matches what users see.

The policy gradient algorithm is a variant close to GRPO, applied single-epoch (no prompt is trained on twice), with full-parameter updates. They remove the length standardization term from standard GRPO (it introduces length bias) and skip advantage normalization by standard deviation (it over-amplifies noise when all rollouts in a group have equal correctness).

Cursor trains additional Multi-Token Prediction (MTP) layers for speculative decoding. These layers are self-distilled: they learn to predict the exact logit distribution of the main LM head at each token position. The MTP layers are initialized from scratch, trained on the same data mix, and then jointly fine-tuned during the long-context and SFT phases before RL. This yields 2-3x faster inference with minimal quality degradation.

### Self-Summarization for Long Horizons

Real coding tasks can be long. The agent might make dozens of tool calls, read many files, and iterate over hundreds of turns. To keep the model effective within a limited context window, Composer 2 uses **self-summarization**: each rollout can involve multiple generations chained together by summaries. The final outcome reward applies to all tokens in the chain, so good summaries that preserve critical information get reinforced, while poor summaries that lose key context get downweighted.

The model learns when and how to summarize as a natural part of RL training. For hard tasks, it often summarizes multiple times.

### Real-Time RL: Learning from Production Traffic

Beyond simulated RL, Cursor also runs what they call **real-time RL**, taking actual production inference tokens and extracting training signal from user interactions. The cycle works like this:

1. Collect billions of tokens from user interactions with the current checkpoint.
2. Distill user responses into reward signals (e.g., did the user follow up with changes, was he happy...).
3. Train on these signals and produce an updated checkpoint.
4. Run the checkpoint through CursorBench to catch regressions.
5. If it passes, deploy.

The entire loop takes about five hours, so they can ship an improved checkpoint multiple times per day. Keeping the loop fast keeps data nearly on-policy: the model generating the data is (almost) the model being trained.

## Chroma Context-1: A Self-Editing Search Agent

**Paper:** [Context-1: Training a Self-Editing Search Agent](https://www.trychroma.com/research/context-1)

Context-1 is a 20B parameter agentic search model trained to do one thing well: find documents. It doesn't answer questions, it returns a ranked set of supporting documents to a downstream reasoning model. The core innovation is **self-editing context**. The model learns to selectively discard retrieved documents that are no longer relevant, freeing up context space for further exploration.

### The Synthetic Data Pipeline

Real-world agentic search tasks are hard to get at scale, you need multi-hop queries with known ground-truth document sets. Chroma built a synthetic generation pipeline across four domains: web, finance (SEC filings), legal (USPTO patents), and email (Epstein files + Enron corpus as distractors).

Each task follows the same structure:

1. Gather supporting documents with unique facts.
2. Generate obfuscated clues (indirect references to facts) and a question.
3. Verify: extract verbatim quotes from documents and check they actually appear in the source text.
4. Collect distractors: documents that match some criteria but point to a different answer.
5. Optionally chain tasks to create multi-hop questions.

The verification step matters because "is this document relevant?" is an unreliable question for an LLM. Instead, they use an **extraction-based pipeline**: the LLM extracts matching quotes from both document and clue, then a deterministic check verifies the quotes actually appear in the source. This achieves >80% alignment with human labels across all domains.

### The Agent Harness

Context-1 operates with four tools:

- `search_corpus(query)`: hybrid BM25 + dense retrieval with RRF fusion and reranking.
- `grep_corpus(pattern)`: regex search.
- `read_document(doc_id)`: read specific chunks.
- `prune_chunks(chunk_ids)`: remove irrelevant chunks from context.

The harness enforces a **fixed token budget** (e.g., 32k tokens). After each turn, current usage is included: `[Token usage: 14,203/32,768]`. Past a soft threshold, the harness suggests pruning. Past a hard cutoff, all tools except `prune_chunks` are blocked, the model must prune or conclude.

**Deduplication** is handled at the harness level: every chunk ID seen across all prior searches is tracked and passed as exclusion filters, so subsequent searches always surface new information.

When the model prunes, the harness removes chunks from the model's view but preserves the full unpruned trajectory for reward computation. This lets the reward credit the agent for documents it encountered during search even if they were later pruned.

A concrete search trajectory might look like this:

```plaintext
Search 1 → context at 11k/32k tokens  
Search 2 → context at 24k/32k (past soft limit, ephemeral "consider pruning" note)  
Search 3 → BLOCKED: 10.4k needed, only 8.5k free  
prune(doc_02, doc_05, doc_06) → context drops to 13k/32k  
Search 3 → context at 23k/32k (past soft limit again)  
prune(doc_08) → context at 18k/32k  
Search 4 → final results  
Answer
```

### Training: SFT Warmup + RL

Training proceeds in two stages:

**SFT warmup**: Generate trajectories using Kimi K2.5 as the inference backend, then filter by recall quality. High-recall trajectories are kept in full. Low-recall ones are included at diminishing rates. A small fraction (up to 5%) of zero-recall trajectories serve as negative examples.

**RL with CISPO**: Fully on-policy training using CISPO (Clipped Importance-Sampled Policy Optimization), a variant of GRPO. 128 queries per step, 8 rollouts each, yielding 1,024 trajectories per step. Groups where all 8 rollouts get the same reward are discarded (no gradient signal under within-group normalization).

The reward is carefully constructed:

- **Outcome**: F-beta score with beta set high (recall weighted 16x over precision initially). This reflects Context-1's role: missing a document is worse than including an irrelevant one, because the downstream model can filter but can't recover what was never retrieved.
- **Process**: Trajectory recall. Credits the agent for encountering relevant documents during search, even if they were later pruned. Without this, the agent converges to issuing one broad search and quitting.
- **Final answer bonus**: +1.0 for retrieving a chunk containing the actual answer.
- **Penalties**: Repeated pruning penalty (discourages one-at-a-time pruning streaks), turn count penalty (discourages diminishing-return search loops).

## Consistent Themes

1. **Train where you deploy.** All three teams invest heavily in making training environments match production. Cursor uses a shadow production backend. Kimi runs sub-agents in the same harness. Chroma runs search against real databases. This minimizes the gap between training performance and real-world performance.

2. **Context management is a first-class problem.** Agent contexts grow over time. Cursor uses self-summarization. Kimi shards context across parallel sub-agents. Chroma teaches the model to discard irrelevant chunks. Different solutions, same underlying constraint.

3. **Reward design is iterative.** Every team describes discovering and fixing reward hacking behaviors. Cursor's model learned to emit broken tool calls. Kimi's orchestrator fell into "serial collapse" or "spurious parallelism." Chroma's agent converged to single-search-then-quit. Each time: observe the degenerate behavior, understand the incentive, add a targeted reward or penalty.

4. **Public benchmarks are not enough.** Cursor explicitly argues that SWE-bench scores don't correlate well with real-world utility. They built CursorBench from actual user sessions. Chroma built synthetic benchmarks across four domains. Kimi uses both public and in-house evaluation. If you're building a vertical model, you need vertical evaluation.

5. **Smaller, purpose-trained models compete with frontier models.** Chroma's 20B model matches frontier-scale LLMs on retrieval at a fraction of the cost and 10x the speed. Composer 2 achieves Pareto-optimal cost-accuracy tradeoffs compared to much larger API models. Domain-specific RL training closes the gap that raw parameter count creates.

## Resources

- **Kimi K2.5**: [Paper](https://arxiv.org/html/2602.02276v1), [Model weights](https://huggingface.co/moonshotai/Kimi-K2.5)
- **Cursor Composer 2**: [Paper](https://arxiv.org/html/2603.24477v2), [Real-time RL blog post](https://cursor.com/blog/real-time-rl-for-composer)
- **Chroma Context-1**: [Technical report](https://www.trychroma.com/research/context-1), [Model weights](https://huggingface.co/chromadb/context-1), [Data gen code](https://github.com/chroma-core/context-1-data-gen)

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
