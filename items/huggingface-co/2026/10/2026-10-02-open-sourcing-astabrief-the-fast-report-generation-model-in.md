---
title: Open-sourcing AstaBrief, the fast report-generation model in Asta
link: https://huggingface.co/blog/allenai/astabrief
source: huggingface-co
published: 2026-10-02T15:19:50Z
updated: 2026-10-02T15:19:50Z
first_seen: 2026-10-02T15:44:26.851147349Z
content: extracted
html: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.html
preview:
  file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.preview-2eb2a80406e7.webp
  width: 256
  height: 183
  alt: AstaBrief — scientific report generation
  color: '#072e2f'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/AdqLEBgtQnaDXKLdylC3l.png
  original:
    file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-585c8eab9b16.png
    width: 1512
    height: 1080
  color: '#04272a'
- source: https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/nBB2ourke1eQYpjq2mGD5.png
  original:
    file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-614420fce35e.png
    width: 859
    height: 810
  variants:
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-1a1bd9c72ee4.webp
    width: 320
    height: 302
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-b308ee211ace.webp
    width: 640
    height: 603
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-380a09acd8b9.webp
    width: 859
    height: 810
  color: '#0a3235'
- source: https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/3dI-e2nnf6FDQw6ay-x9N.png
  original:
    file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-75482fdf34a1.png
    width: 1535
    height: 810
  variants:
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-463b88c65e59.webp
    width: 320
    height: 169
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-5135843e5486.webp
    width: 640
    height: 338
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-97fd4ed7a1bf.webp
    width: 960
    height: 507
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-ca49daf11094.webp
    width: 1280
    height: 675
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-6a46df85ada7.webp
    width: 1535
    height: 810
  color: '#0a3235'
- source: https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/Ek19fGImhqROo_qSUt5LV.png
  original:
    file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-cfa84142f384.png
    width: 1550
    height: 660
  variants:
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-8eae82f5cee0.webp
    width: 320
    height: 136
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-9e7e14a18b4d.webp
    width: 640
    height: 273
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-dba8531df038.webp
    width: 960
    height: 409
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-b9c974661027.webp
    width: 1280
    height: 545
  - file: 2026-10-02-open-sourcing-astabrief-the-fast-report-generation-model-in.image-c202b4406c33.webp
    width: 1550
    height: 660
  color: '#0a3235'
---

🤗 [Model](https://huggingface.co/allenai/AstaBrief_8B) | 📊 [Data](https://huggingface.co/collections/allenai/astabrief)

[![AstaBrief — scientific report generation](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/AdqLEBgtQnaDXKLdylC3l.png)](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/AdqLEBgtQnaDXKLdylC3l.png)

Language models can already help researchers search the literature, synthesize evidence, and work through complex questions. But scientific work places particular demands on these models—answers need to stay grounded in evidence, the models need to preserve what the evidence actually supports rather than quietly broadening a study’s conclusions, and researchers need to be able to verify the final outputs.

We see that in how scientists use [Asta](https://asta.allen.ai/), our agentic platform for scientific work. Instead of simple keyword searches, users often bring substantial context and many constraints—for example, asking Asta to compare approaches across a body of literature while accounting for a particular method, population, or setting. Many also return to generated reports later, treating them as working research artifacts rather than one-off answers.

We wanted to help scientists generate cited reports faster, with a model they could download and run themselves. To do that, we tested whether a small, open model trained specifically for scientific report generation could match the report quality of the proprietary models we were using, while reducing generation time and serving costs.

We built [**AstaBrief 8B**](https://huggingface.co/allenai/AstaBrief_8B), a model that turns a research question and retrieved literature excerpts into a cited report. AstaBrief is available in Asta’s Generate a report feature today as [Fast mode](https://asta.allen.ai/) alongside Claude-powered Thinking mode, and we’re also open-sourcing it and the training data so others can study, reproduce, and build on our approach.

Developing AstaBrief required tens of thousands of real research queries, citation-focused filtering, preference data, and a redesigned report-generation pipeline that writes the full report in one pass rather than section by section. The result is nearly an order-of-magnitude reduction in report generation time compared to the proprietary models we tracked—across the full Asta pipeline, Fast mode averages 51.1 seconds per report compared with 178.5 seconds for Thinking mode, about 3.5× faster.

Together, those efficiency gains made AstaBrief a useful test case for a broader goal: building open language models that can be adapted to the specific demands of scientific work.

Open weights will also let institutions run AstaBrief on their own infrastructure, which is necessary when research questions reveal sensitive or unpublished work. Alongside the model weights, we’re releasing [an example workflow that researchers can adapt to create reports from their own PDFs](https://github.com/allenai/ai2-scholarqa-lib/tree/main/api/scholarqa/lite), providing a starting point for local report generation

This post covers how we trained AstaBrief, what we learned about grounding it in scientific evidence, and which parts of our approach we think can carry forward to future models for science. Most of the training and evaluation described was completed in 2025, so the proprietary models used to generate training data and as comparison points reflect the frontier at the time. We haven’t rerun the full evaluation against today’s frontier models; the results below are best read as evidence about the particular training and system design choices we tested.

## **Training the model**

Our goal with AstaBrief was to build an open-weights model with all the qualities that matter most for long-form scientific synthesis: answer quality, relevance, structure, and citation grounding. We started from Qwen3-8B and focused most of our effort on the post-training data, evaluation, and surrounding report-generation scaffolding.

Adapting general-purpose models for scientific work – and training new scientific models from scratch – is something we're exploring broadly across Ai2. Through [NSF OMAI](https://allenai.org/omai), a U.S. national initiative led by Ai2 to build fully open AI infrastructure and models for scientific discovery, our researchers are working directly with scientific communities to understand what they need from future open models and where today's general-purpose models fall short. That includes studying how needs differ across scientific fields and workflows, with more findings from that research to share in the future.

Recent work, including our [DR Tulu](https://allenai.org/blog/dr-tulu), has shown that reinforcement-learning-based (RL) methods can improve long-form report generation for open-weights models, especially when judge models are involved in the training loop. We considered that path for AstaBrief, but ultimately focused on a simpler recipe built around supervised fine-tuning (SFT) and direct preference optimization (DPO).

RL-based training can be unstable and expensive. We wanted to see how far we could push report generation quality with a cheaper, more operationally manageable setup—one that's also easier to debug and iterate on.

That made the quality of the training data especially important. Rather than relying on a more complex optimization method to compensate for noisy examples, we spent much of the project figuring out how to generate, select, and filter examples that actually demonstrated the report-writing behavior we wanted.

We also wanted AstaBrief to be faster so that users could get preliminary reports quickly that they could then iterate over in subsequent turns. For speed improvements, we decided to train AstaBrief to directly generate the final report in one pass given a user query and relevant retrieved snippets, bypassing the expensive snippet summarization and clustering stages our Claude-based Thinking mode uses and not writing out the answer section-by-section. Interestingly, we found it was possible to do so without sacrificing performance.

## **Collecting SFT training data**

The training pipeline began with real user queries submitted through the system described in our paper “[Synthesizing scientific literature with retrieval-augmented LMs](https://allenai.org/blog/nature-openscilm)” and [ScholarQA](https://aclanthology.org/2025.acl-demo.49.pdf), the framework that now underpins Asta’s Generate a report feature. Rather than training only on synthetic prompts or benchmark-style tasks, we wanted AstaBrief to learn from real queries from real scientists.

Our research suggests that scientists often ask different things of language models than users do of general-purpose chatbots or traditional search tools. In our [analysis of hundreds of thousands of Asta queries](https://allenai.org/blog/asta-interaction-dataset), expert researchers frequently supplied substantial context, multiple constraints, and relationships between concepts rather than relying on short, keyword-style prompts.

More recent Asta user studies have also surfaced differences in how researchers want AI involved in their work—some are comfortable using models for ideation or experimentation, while others prefer a narrower role in synthesis, literature surveillance, or pattern-finding. Across those differences, participants want clearer source traceability, more visibility into what a model is doing, and greater control over the context it uses.

We filtered the user logs we collected for quality, relevance, and privacy, stripping out beta-tester and bot traffic, dropping queries that were too short to be meaningful, and using an LLM-based filtering pass to catch non-English queries, non-scientific requests, and prompts containing personal information. That left a pool of 90K research-focused queries.

For SFT, we generated full-report target outputs from the filtered queries using the multi-step ScholarQA pipeline behind Asta's report generation. The pipeline retrieved relevant literature, organized the material into sections, and used a backing report-generating model to synthesize the evidence into a cited report. We drew on a mix of proprietary systems: Claude 3.5 Sonnet, Claude 3.7 Sonnet, o3, o4-mini, and GPT-4.1. After quality filtering, this yielded 47K usable training examples.

## **Creating DPO pairs**

DPO required a different kind of training data. Instead of a single target report per query, we needed pairs of reports with one preferred over the other.

We built those pairs from a separate subset of queries not used during SFT data generation. One report per query came from the existing ScholarQA pipeline, typically backed by Claude 3.5 Sonnet or 3.7 Sonnet. The competing report was generated by feeding ScholarQA's retrieved literature excerpts to a different model: o3, o4-mini, DeepSeek-V3, or DeepSeek-R1, depending on the example.

Two judge models – GPT-4.1 and DeepSeek-R1 – compared each pair and picked a winner. We ensured that LLM judges were aligned with human preferences (95% agreement) and only kept pairs where both judges agreed, which gave us a cleaner preference set and cut much of the noise that typically shows up in preference data generated at scale.

After quality filtering, the final DPO dataset came to about 6K examples.

Using multiple generators and requiring agreement between two judges gave us a relatively simple way to construct preference data without treating any single model’s output or judgment as ground truth.

## **Filtering data for better attribution**

Our main evaluation target was [SQABench-CS2](https://github.com/allenai/ai2-scholarqa-eval), a set of 200 user-written computer science research questions. We tracked four metrics throughout the development of AstaBrief:

- **Rubric score**, which measures how much necessary content is covered by the report.
- **Answer precision**, which measures whether each paragraph is relevant to the question.
- **Citation precision**, which measures whether each citation supports the claim it's attached to.
- **Citation recall**, which measures whether the report's claims are fully supported by the citations provided.

For our final model, we also ran secondary evaluations: [DeepScholarBench](https://arxiv.org/abs/2508.20033), a 63-query benchmark for long-form research synthesis built from recent ArXiv papers, and two separate pairwise evaluations against reports generated by the Claude-powered pipeline—an LLM-judged comparison on SQABench-CS2 and a small human study.

A report can sound polished and complete while meandering from the question or attaching citations to claims from which the underlying evidence doesn't follow. For scientific synthesis, we needed to measure those behaviors separately. But citation support is only part of scientific faithfulness—a model can cite the right study and still make a stronger claim than the study itself supports. [This can happen in subtle ways](https://royalsocietypublishing.org/rsos/article/12/4/241776/235656/Generalization-bias-in-large-language-model), for example, turning a finding about a particular sample into a generic claim about an entire population, shifting a result reported in the past tense into a present-tense statement that sounds more universally true, or turning a descriptive finding into a recommendation for what clinicians, policymakers, or researchers should do.

Those kinds of generalizations are especially important for scientific report generation because each step can broaden the apparent scope of the evidence without introducing an obviously false statement. A cited sentence may therefore be technically related to its source while still overstating what researchers actually established. Our development metrics focused primarily on relevance, coverage, and citation grounding; a richer evaluation of scientific report writers should also test whether they preserve the scope and strength of the claims in their sources.

Our first SFT runs improved overall content quality, but they still lagged behind our Claude-powered report generation pipeline on answer precision and citation quality. In other words, the model got better at writing reports, but it still wasn’t grounded in evidence as consistently as we needed for scientific synthesis.

That pushed us to spend more time on data quality. We tested four statistics-based filters to identify weaker synthetic training examples:

- **Output-to-input token ratio**. Answers with very high ratios were often noisy because they were generating a lot of text from too little evidence.
- **Citation relevance**. For each synthetic report in the training set, we averaged the retrieval relevance scores of its cited papers. Low averages suggested the report was relying too heavily on lower-ranked evidence.
- **Citation density**. We measured the share of statements that had at least one citation. Low-density reports often had large stretches of unsupported text.
- **Citation diversity:** We measured the share of papers cited in the answer, given the set returned by the Claude-powered report retrieval pipeline. Low scores suggested the report was overly reliant on a few papers.

The strongest gains came from filtering out synthetic reports with low citation density; more aggressive filtering, filter combinations, and learning-rate sweeps didn't add meaningful gains.

That was one of the clearest lessons from the project: more elaborate filtering wasn’t necessarily better. A relatively simple signal – whether the synthetic reports consistently cited their claims – was more useful than several more complicated combinations we tried. Scientific specialization, in other words, isn't necessarily a matter of adding more scientific text to pretraining; the composition and quality of post-training data and whether it demonstrates behaviors like grounding and attribution can materially change how the resulting model performs.

That focus on grounded, useful output also lines up with what we’ve heard in Asta user research. Participants note that generating more text isn't necessarily more helpful; they want concise synthesis and enough source traceability to review and verify results without wading through unnecessary outputs.

Once we had a stronger SFT checkpoint, we ran DPO training on top of it. That stage pushed performance further, bringing AstaBrief within range of the Claude-powered report pipeline in Asta and DR Tulu on report generation.

## **Validating the approach**

Because this model was intended to work as part of our agentic Asta report generation framework (not necessarily as a standalone model), our main question was whether AstaBrief could preserve the report qualities we cared about while enabling a substantially faster and cheaper report-generation pipeline. In other words, we weren’t only asking whether the model could match a stronger proprietary model on individual benchmarks; we wanted to know how much of that quality we could retain with a much simpler system.

[![image](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/nBB2ourke1eQYpjq2mGD5.png)](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/nBB2ourke1eQYpjq2mGD5.png)

[![image](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/3dI-e2nnf6FDQw6ay-x9N.png)](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/3dI-e2nnf6FDQw6ay-x9N.png)

*Each row is ordered best first; higher is better on every metric. Qwen3-8B was evaluated on SQABench-CS2 test only. SQABench-CS2 is a set of user-written computer science research questions; DeepScholarBench scores long-form research synthesis with its own metrics, which are not comparable with SQABench-CS2's.*

In the evaluations we used during development, AstaBrief was competitive with the Claude-powered pipeline and DR Tulu across several measures of answer and citation quality. The chart below shows the LLM-judged comparison—in a separate 14-question human study, three scientific researchers each contributed 4-5 questions and ranked reports from the three systems on overall preference, completeness, relevance, organization, and citation accuracy (with ties allowed). On overall preference, DR-Tulu wins, but two of the three researchers prefer AstaBrief over other systems on citation accuracy metrics, demonstrating the utility of our SFT data quality filters.

[![image](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/Ek19fGImhqROo_qSUt5LV.png)](https://cdn-uploads.huggingface.co/production/uploads/638e39b249de7ae552d977b5/Ek19fGImhqROo_qSUt5LV.png)

*Bars show the share of LLM-judged report comparisons each system won against Thinking mode on the same questions. Human judgments were evaluated separately and are not included. Thinking mode is the comparison reference and has no bar. Unlike DR-Tulu, Asta Brief was optimized for this pairwise report ranking during the DPO stage.*

These numbers are best read as validation of the engineering approach at the time we developed it, rather than as a claim about where this particular base model sits relative to today’s frontier. The model ecosystem moves quickly—the data construction, attribution filtering, and serving lessons are the pieces we expect to generalize.

Validating the usefulness of AstaBrief in Asta, Fast mode has shown encouraging early usage. Among 374 Asta users who’ve tried it, 29.1% have used it for two or more days, and users on average generate 3.67 report threads with it. Twenty-three percent of users who tried Fast mode continued using it and never switched back to Thinking mode for future threads. An additional 18% switched between Fast and Thinking modes depending on their goals, using Fast mode for ~40% of their threads.

While feedback is generally too sparse to draw strong conclusions, we see that Fast mode receives positive feedback at a similar rate as Thinking mode (84.2% versus 85.2%).

## **Where this goes next**

Asta's report generation is the first production use of AstaBrief, giving researchers an open-weights Fast mode alongside the existing Thinking mode. Because the model is open weights, institutions can deploy it on their own hardware, including behind their own firewall, without relying on a proprietary model API for report generation.

In Asta, that also means we can study and improve this part of the report generation pipeline directly while preserving Thinking mode as an option for more compute-intensive tasks.

There's more to do. We're exploring more fine-grained preference learning, stronger RAG-plus-RL approaches, multi-turn and multi-tool capabilities, additional scientific data sources, and query decomposition. We're also interested in evaluations that go beyond whether a claim has a supporting citation to ask whether a model preserves the evidentiary—both to better capture the quality of the report as a research artifact and to ask whether a model preserves the evidentiary scope of its sources. That includes qualities such as concision and organization, as well as whether the model turns sample-specific findings into broad generalizations or descriptive results into recommendations.

AstaBrief is one experiment in a longer line of work on language models for science, from ScholarQA and DR Tulu to future versions of Olmo beginning to take shape now. The lessons here – especially around training data, filtering, and evaluation- can help inform what we build next.

[Try Fast model today in Asta](https://asta.allen.ai/), or [download AstaBrief from Hugging Face](https://huggingface.co/allenai/AstaBrief_8B).
