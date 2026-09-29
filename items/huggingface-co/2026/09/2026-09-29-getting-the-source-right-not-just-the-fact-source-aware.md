---
title: 'Getting the Source Right, Not Just the Fact: Source-Aware Verification for MCP Agents'
link: https://huggingface.co/blog/MultiverseComputingCAI/getting-the-source-right-not-just-the-fact-source
source: huggingface-co
published: 2026-09-29T13:07:00Z
updated: 2026-09-29T13:07:00Z
first_seen: 2026-09-29T14:21:22.166029648Z
content: extracted
html: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.html
preview:
  file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.preview-5c8fbf1f4b1f.webp
  width: 256
  height: 144
  color: '#e4ecf2'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/phhhj_BLOJbvnTFhVaZ5y.png
  original:
    file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-f233f220a685.png
    width: 1672
    height: 941
  color: '#fafcfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/svnUpzhFgTpLqOF_bRDHl.png
  original:
    file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-8bb1981c03d2.png
    width: 1096
    height: 388
  variants:
  - file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-49a3db88623a.webp
    width: 320
    height: 113
  - file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-655cbca4ea44.webp
    width: 640
    height: 227
  - file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-6a0cfa958b26.webp
    width: 1096
    height: 388
  color: '#fbfbfb'
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/nDchWzRhBSmscjaa8w5G5.png
  original:
    file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-63481777ee32.png
    width: 1148
    height: 388
  variants:
  - file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-d9775b71464c.webp
    width: 320
    height: 108
  - file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-368dbd6b586c.webp
    width: 640
    height: 216
  - file: 2026-09-29-getting-the-source-right-not-just-the-fact-source-aware.image-3e53aae9f906.webp
    width: 1148
    height: 388
  color: '#fdfdfd'
---

Tool-using LLM agents no longer read from a single retrieved passage. Through the [Model Context Protocol (MCP)](https://modelcontextprotocol.io), an agent can call a search tool, inspect a structured patient or account record, query a database, and pull metadata, then weave all of it into one answer. That m aakes the usual question of factuality more subtle than it looks. Most of the systems built to check LLM answers, from [RAGAS faithfulness](https://github.com/explodinggradients/ragas) to fine-grained checkers like MiniCheck, AlignScore, and SummaC, ask whether a claim is supported by the available evidence once that evidence has been pooled together. In their usual form, they do not tell us which MCP tool output supports each claim, or whether that is the source the answer names.

Our latest paper, *ProvenanceGuard: Source-Aware Factuality Verification for MCP-Based LLM Agents* (read it on [Hugging Face](https://huggingface.co/blog/MultiverseComputingCAI/%5BHF-PAPER-LINK%5D), or on [arXiv](https://arxiv.org/abs/2606.18037) in the meantime), targets that gap. The failure mode we care about is one we call cross-source conflation: a claim that is true somewhere in the evidence, but attributed to the wrong source. A source-blind verifier may pass it, because the fact does exist in the pool. A source-aware verifier should not.

## The problem: supported somewhere is not the same as supported by the right source

Consider a customer support agent that answers, "According to the account record, this plan includes a 30-day refund window." The refund window may be perfectly real, but stated in a policy document, not in the account record the answer points to. Pool the two together and the claim looks supported. Keep them separate and the attribution is wrong, and in a data-sensitive setting a wrong attribution can be as damaging as a wrong fact. The same pattern shows up in a clinical agent, where a patient-specific medication detail taken from a patient-history tool becomes misleading the moment the answer presents it as a finding from the medical literature.

[![Diagram contrasting source-blind pooled support with source-aware verification, using a refund-window claim that is supported by a policy document but attributed to the account record.](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/svnUpzhFgTpLqOF_bRDHl.png)](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/svnUpzhFgTpLqOF_bRDHl.png)

*A claim can be supported by one MCP source while the answer attributes it to another. Source-blind scoring sees support in the pooled evidence and passes it; ProvenanceGuard separately checks whether the supporting source matches the one the answer states or implies. Source: paper Figure 1.*

This is why faithfulness scores, useful as they are, are not enough for MCP agents. An answer carries provenance, sometimes explicitly ("according to the account record") and sometimes implicitly. ProvenanceGuard keeps that connection between claim and source available for inspection.

## What ProvenanceGuard does

ProvenanceGuard is a post-generation verification layer that sits on top of a black-box MCP agent. It runs after an agent produces an answer, and never collapses the evidence into one anonymous context. Instead it carries the source identity all the way through the pipeline. It reads the captured MCP trace, including the tool outputs and their source IDs, without retraining the agent. Then it does five things in sequence: it breaks the answer into specific claims, finds the source most relevant to each one, checks whether that source actually supports it, compares the source with the one the answer names or implies, and finally emits both a per-claim source verdict and a global, answer-level allow or block decision.

[![Pipeline diagram showing the agent producing a draft answer and trace, then ProvenanceGuard decomposing claims, routing to sources, checking support with NLI, calibrating, checking attributions, and either allowing the answer or sending it to repair.](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/nDchWzRhBSmscjaa8w5G5.png)](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/nDchWzRhBSmscjaa8w5G5.png)

*The verification flow. Source identity is preserved through decomposition, routing, support scoring, attribution checking, and repair, rather than being pooled. Blocked answers can go through RARR-style repair and be re-verified. Source: paper Figure 2.*

A few of the design choices are worth calling out. For the experiments in our paper, we used local models so the captured traces could be processed in a controlled, offline setup: [MiniLM](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) helps find the relevant source, a [DeBERTa NLI verifier model](https://huggingface.co/MoritzLaurer/DeBERTa-v3-base-mnli-fever-anli) checks whether that source supports the claim, and a local language model helps break answers into claims. The verifier also checks literal values closely: a number, date, or identifier absent from the source cannot pass merely because the sentence sounds plausible. A calibrated decision step combines these signals. If an answer is blocked, a [RARR](https://arxiv.org/abs/2210.08726)-style repair step can try a source-grounded revision or a safe fallback, which the verifier then checks again.

Those named models are the setup we evaluated, not a requirement of ProvenanceGuard. The same claim, source, and decision steps can be adapted to hosted models where a team prefers cloud services; a new setup would need its own testing and calibration. Our reported results come from the local configuration. Its conservative decision policy suits data-sensitive review, where getting the source right matters more than producing the fastest possible answer.

## Results

We tested ProvenanceGuard on answers from a medical agent that had used patient records, research articles, and other tools. This gave us 281 real traces to study. Medicine is a useful test because a fact from a patient's record and a fact from general research cannot be treated as the same source. The method can also be used in other fields when an agent keeps a record of its tool outputs and source IDs. For the main test, human experts checked 361 claims from 40 answers set aside from the data used to develop the system.

The most direct result is this: experts said 139 claims should not pass, and ProvenanceGuard caught 138 of them. It let one through. It also held 67 claims that the experts considered supported, sending them for review or repair. This reflects the cautious setting we tested: it favors a second look at some supported claims over letting unsupported ones through. For claims with an identifiable source, it also picked the right source about 86% of the time in this test.

We ran four other support checkers on the same claims. ProvenanceGuard scored highest on the paper's measure of how well a system catches claims that should be blocked while avoiding unnecessary blocks. The other checkers in this comparison did not tell us which tool output supported each claim. ProvenanceGuard records that connection, so a reviewer can see the source checked for each claim and the decision it produced.

| Verifier               | Reject/block F1 | Emits claim-to-source ID |
| ---------------------- | --------------- | ------------------------ |
| ProvenanceGuard (ours) | **0.802**       | Yes                      |
| MiniCheck              | 0.783           | No                       |
| RAGAS Faithfulness     | 0.758           | No                       |
| AlignScore             | 0.662           | No                       |
| SummaC-ZS              | 0.436           | No                       |

*Binary support metrics on the same held-out claim packet. ProvenanceGuard matches or beats the source-blind baselines on blocking while also producing per-claim source verdicts. Source: paper abstract and Table III.*

## Checking claims when sources look similar

In a separate, harder test with several similar sources, ProvenanceGuard scored 0.846 F1 for deciding which claims to block, but identified the exact source correctly in 50.3% of claims. Telling similar sources apart remains an important area for improvement.

We also ran a controlled test focused on wrong attribution: we changed the named source in 50 cases while leaving the supporting evidence intact. ProvenanceGuard caught all 50 swaps. This shows it can detect a clear source error, while the harder test shows the challenge of choosing among many plausible sources.

## Repairing blocked answers

Blocking is only useful if there is something to do with a blocked answer. Wired to the RARR-style repair loop, the full-trace run resolved all 173 blocked answers, though 144 of them ended in fallback text rather than a substantive rewrite, which is the system choosing to avoid an unverifiable answer rather than manufacture one. On reconstructed multi-source test traces, a fresh repair run resolved all 59 initially blocked answers with only two terminal fallbacks. As an offline gate the overhead is modest, roughly half a second per answer on the reported local configuration, with the NLI and routing calls themselves in the tens of milliseconds.

## Why this fits Multiverse Computing

As agents move from single-passage RAG to multi-tool MCP setups, the question of which source a fact actually came from stops being a footnote and becomes part of what factuality means. ProvenanceGuard makes that source connection visible claim by claim. For Multiverse Computing, that means a way to check existing agents while keeping sensitive traces in a controlled environment when needed. The medical study is one use case; the same approach can be adapted wherever an agent's trace preserves its tools and sources.

That adaptation is already visible in [NVIDIA NVFlow](https://github.com/NVIDIA/nvflow/pull/9), which merged an optional grounding-verification stage for its finance agent. It checks completed answers against the SEC excerpts the agent retrieved and saves separate decisions without changing the original rollout or training data. The NVFlow contribution uses ProvenanceGuard's source-aware verification approach; the repair loop discussed above belongs to the broader research system.

ProvenanceGuard was also [presented as a poster at the Agentic AI Summit 2026 at UC Berkeley](https://github.com/aalvsz/provenanceguard/blob/main/poster/ProvenanceGuard_Agentic_AI_Summit_2026_Berkeley.pdf).

Want the full technical details, including the routing and NLI derivations, the calibration ablations, the multi-source stress slices, and the complete results tables? Read the full paper on [Hugging Face](https://huggingface.co/papers/2606.18037), or get in touch with our team to talk about applying source-aware verification to your own agents.
