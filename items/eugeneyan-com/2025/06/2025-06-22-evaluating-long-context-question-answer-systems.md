---
title: Evaluating Long-Context Question & Answer Systems
link: https://eugeneyan.com//writing/qa-evals/
source: eugeneyan-com
published: 2025-06-22T00:00:00Z
updated: 2025-06-22T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- eval
- llm
- survey
summary: Evaluation metrics, how to build eval datasets, eval methodology, and a review of several benchmarks.
content: extracted
html: 2025-06-22-evaluating-long-context-question-answer-systems.html
preview:
  file: 2025-06-22-evaluating-long-context-question-answer-systems.preview-e4598d37abfe.webp
  width: 256
  height: 134
  alt: Image
  color: '#e7e7e9'
images:
- source: https://eugeneyan.com/assets/og_image/qa-evals-v2.jpg
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-5e399877eacc.jpg
    width: 1200
    height: 630
  color: '#fafafa'
- source: https://eugeneyan.com/assets/narrativeqa.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-3d5856efbc0f.webp
    width: 1195
    height: 504
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-e115f86dddb1.webp
    width: 320
    height: 135
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/novelqa-fig2.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-2fb1d0484ce8.webp
    width: 1200
    height: 575
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-332e0ffdf859.webp
    width: 320
    height: 153
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/novelqa-table5.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-bf90ac7c4f0c.webp
    width: 1200
    height: 993
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-face73d99521.webp
    width: 320
    height: 265
  color: '#fafafa'
- source: https://eugeneyan.com/assets/novelqa-table7.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-5d22169b4f24.webp
    width: 1083
    height: 1247
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-66c5114c9ced.webp
    width: 320
    height: 368
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/novelqa-fig3.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-61fed62f2cf4.webp
    width: 1200
    height: 563
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-4dbdc7c0d395.webp
    width: 320
    height: 150
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/qasper-fig1.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-1ec99ca705fe.webp
    width: 955
    height: 1312
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-7627459a9adb.webp
    width: 320
    height: 440
  color: '#fafafa'
- source: https://eugeneyan.com/assets/leval-table1.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-174ba4235650.webp
    width: 1200
    height: 906
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-8b5b428feea5.webp
    width: 320
    height: 242
  color: '#faf9f9'
- source: https://eugeneyan.com/assets/helmet-table3.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-f881fd24c9bd.webp
    width: 1200
    height: 797
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-7ce636ac3fb0.webp
    width: 320
    height: 213
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/helmet-fig1.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-62ec20fbf63f.webp
    width: 1200
    height: 406
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-8d181a2ed9e9.webp
    width: 320
    height: 108
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/loong-fig1.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-354146e8fbac.webp
    width: 948
    height: 924
  color: '#fcfbfd'
- source: https://eugeneyan.com/assets/loong-fig2.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-c9c17659ec74.webp
    width: 1200
    height: 797
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-04f693413f8c.webp
    width: 320
    height: 213
  color: '#f9f9f8'
- source: https://eugeneyan.com/assets/loong-fig3.webp
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-ca3be20879d8.webp
    width: 1060
    height: 1140
  color: '#fbfbfc'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2025-06-22-evaluating-long-context-question-answer-systems.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2025-06-22-evaluating-long-context-question-answer-systems.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

While evaluating Q&A systems is straightforward with short paragraphs, complexity increases as documents grow larger. For example, technical documentation, novels and movies, as well as multi-document scenarios. Although some of these evaluation challenges also appear in shorter contexts, long-context evaluation amplifies issues such as:

- **Information overload:** Irrelevant details in large documents obscure relevant facts, making it harder for retrievers and models to locate the right evidence for the answer.
- **Positional variance:** Evidence may appear at the beginning, middle, or end of documents, making it a challenge for models with limited effective context or those susceptible to the “lost in the middle” problem.
- **Multi-hop reasoning:** The correct answer depends on synthesizing several distinct pieces of evidence scattered throughout the text(s), challenging the model’s ability to retain and integrate information that is far apart.
- **Hallucinations at scale:** Larger contexts increase the risk of models returning plausible yet incorrect responses due to poor retrieval or limited effective context.
- **Open-ended questions:** Queries on broad themes or interpretative topics rarely have a single definitive answer, especially for large documents or corpora.

In this write-up, we’ll explore [key evaluation metrics](https://eugeneyan.com//writing/qa-evals/#key-evaluation-metrics), how to [build evaluation datasets](https://eugeneyan.com//writing/qa-evals/#building-an-evaluation-dataset), and [methods to assess Q&A performance](https://eugeneyan.com//writing/qa-evals/#methods-to-assess-qa-performance) through human annotations and LLM-evaluators. We’ll also [review several benchmarks](https://eugeneyan.com//writing/qa-evals/#what-we-can-learn-from-existing-benchmarks) across narrative stories, technical and academic texts, and very long-context, multi-document situations. Finally, we’ll wrap up with advice for evaluating long-context Q&A on our specific use cases.

![Image](https://eugeneyan.com/assets/og_image/qa-evals-v2.jpg "Image")

An overview of what we'll cover in this writeup

By the way, if you want to learn more about evals, my friends Hamel and Shreya are hosting their *final* cohort of “AI Evals for Engineers and PMs” in July. Here’s a [35% discount code](https://maven.com/parlance-labs/evals?promoCode=eugene-is-all-you-need).

## **Key Evaluation Metrics**

Evaluating Q&A systems goes beyond just checking for factual accuracy. Specifically, we might want answers to be based solely on the provided text, not the model’s knowledge. But even technically correct answers aren’t necessarily helpful. Thus, to evaluate Q&A systems effectively, we should consider two orthogonal dimensions:

- **Faithfulness:** How strictly the answer relies on only the source document.
- **Helpfulness:** How relevant, comprehensive, and useful the response is for the user.

**Faithfulness measures whether an answer strictly relies *only* on the source document.** This means the model shouldn’t add external information or make things up (aka hallucinate). Faithfulness is especially important for legal agreements, financial contracts, or medical and insurance forms, where answers must be based solely on the given text. Faithfulness is synonymous with groundedness, where answers must be anchored on the original document.

Faithfulness also includes the Q&A system knowing when to say, “I don’t know.” If the source document doesn’t contain the answer, the ideal response is something like, “I don’t have that information in the provided text.” Related to this challenge are two errors by Q&A systems:

- **False positives:** When the system makes up an answer that doesn’t exist in the source document (hallucinations).
- **False negatives:** When the system incorrectly states that the source document doesn’t contain information that actually is present, either due to poor retrieval or attention limitations over large contexts.

We also want to distinguish faithfulness from correctness. An answer might be correct based on general knowledge but still be unfaithful if it contradicts the document. Examples include patient-specific medical instructions that differ from the usual guidelines, definitions in financial or legal agreements that depart from the standard, and historical fiction with alternate timelines. Users depend on Q&A systems to return responses that are faithful to their specific documents, rather than general truths.

For systems that provide citations, we can also assess citation accuracy. This evaluates if the cited text supports the answer. Benchmarks like [QASPER](https://arxiv.org/abs/2105.03011) explicitly evaluate whether models reference the right supporting evidence for the answer. This combined assessment—checking both faithfulness and citation accuracy—provides finer-grained metrics on overall faithfulness and evidence retrieval.

*However, a faithful answer isn’t always a helpful answer.* This is where we also want to evaluate the helpfulness of responses.

**Helpfulness measures whether an answer is relevant, sufficiently detailed, yet concise.** Relevance means the answer directly addresses the user’s question without straying off-topic. Comprehensiveness ensures the answer contains the necessary details. Conciseness balances comprehensiveness by ensuring the answer is succinct, without unnecessary details or fluff.

While a brief, one-sentence response to a complex question might be faithful, it falls short of being helpful if the answer needs more details. Conversely, overly long responses filled with extraneous details can overwhelm users, making it hard for users to find the core answer they need. An ideal response should contain most, if not all, of the relevant information from the source document, in a concise way that meets the user’s needs.

A study by [Xu et al. (2023)](https://arxiv.org/abs/2305.18201) found that domain experts in fields like biology or economics preferred answers that were both comprehensive and faithful, particularly for long-form questions. In contrast, crowd-workers often emphasized surface aspects such as conciseness or detail. Thus, if we’re building our Q&A system for power users and experts, the system should focus on returning faithful and comprehensive answers.

There’s a tension between faithfulness and helpfulness. An answer can be perfectly faithful yet totally unhelpful. For example, if we ask about a legal contract: “What happens if the tenant misses a payment?” A faithful yet unhelpful answer could be, “Clause 4.2 of the lease agreement addresses missed payments.” Although technically accurate, it’s not helpful as it doesn’t tell us what actually happens if a payment is missed. The same goes for Q&A systems that simply copy-paste large sections from documents. A useful system should synthesize the information and return a direct answer that meaningfully addresses the user’s question.

All in all, the best answers achieve both faithfulness and helpfulness by:

- Staying grounded in the source text (faithful)
- Directly addressing the user’s question (relevant)
- Providing sufficient detail and context (comprehensive)
- Presenting information clearly and succinctly (concise)

## **Building an Evaluation Dataset**

Evaluating long-context Q&A begins with creating a robust evaluation dataset. This involves testing how well a Q&A system can navigate book-length documents to answer questions.

First, we’ll start with creating a variety of realistic, context-specific questions. While human annotators excel at crafting great questions, this is time-consuming and impractical at scale, especially for lengthy documents. A more efficient approach is to use language models to draft questions that annotators can then accept or edit—this augments human judgment with machine speed and scale.

However, just scaling with a language model isn’t enough. We also need to guide the model toward generating natural, useful questions. Thus, instead of vague prompts like “Generate questions about this chapter,” we can be more specific, such as: “Summarize the main characters in this chapter. Then, generate one question about each character’s backstory based on what we’ve read so far.” More precise prompting helps steer models toward producing useful questions for our evaluation dataset.

This approach builds on the methodology of existing benchmarks. [NarrativeQA](https://arxiv.org/abs/1712.07040) intentionally generates questions based on summaries rather than full texts. This encourages questions that test narrative comprehension rather than shallow fact recall. For the same reason, [QASPER](https://arxiv.org/abs/2105.03011) creates questions based on abstracts from academic papers that models then answer based on the full paper. By learning from these benchmarks, we can construct evaluation datasets that effectively measure meaningful comprehension of long-context documents.

**We’ll want to ensure question diversity when creating questions.** Having a range of question types helps us evaluate the Q&A system’s capabilities without overfitting to any single type of question. Depending on our use case, an evaluation dataset could include a mix of:

- **Fact recall:** These evaluate basic fact retrieval, like “Who is the protagonist?”, “When was the treaty signed?”, or “What is the legal clause mentioned in Section 2.1?” While simple, they confirm whether our Q&A system can reliably extract information.
- **Definitions:** These assess a model’s ability to explain domain-specific content based on the document. Examples include “What does this acronym mean in the paper?”, “Explain the magic system introduced in Chapter 7,” or “Define the economic theory discussed on page 203.” This is important for technical documents to ensure the system can handle specialized terminology in context.
- **Summarization:** These measure whether the system can identify the core ideas and coherently summarize them. For example, “Summarize the main findings of the paper”, “Recap what has happened in the book so far”, or “What are the key themes discussed in Part 2?”
- **Inference and reasoning:** These evaluate the ability to reason beyond explicitly stated facts by integrating information from different parts of the document to form a coherent answer. For example, “Why did the character make this choice?” or “What can we infer about the society from these laws?”
- **“No-Info”:** Unlike previous categories, these questions cannot be answered from the document. For example, “What did Gandalf do in the final battle at Hogwarts?” or “What is the penalty for trademark infringement in this residential lease agreement?” A faithful Q&A system should recognize that the required information isn’t present and respond accordingly instead of making up an answer.

**Our Q&A evals should also be robust to the position of evidence within the document.** We ensure this by having questions with evidence that appear at the beginning, middle, or end, as well as creating multi-hop questions that require details from several sections or documents. Benchmarks like [HELMET](https://arxiv.org/abs/2410.02694v1) evaluate how model accuracy changes based on the location of supporting information, evaluating the model’s ability to pay attention to and combine information from the entire document instead of relying solely on nearby context.

## **Methods to Assess Q&A Performance**

**Human annotators are crucial for building a high-quality, ground-truth dataset**. This is useful for calibrating automated evaluators, and with enough annotated examples, we can also train evaluation classifiers or reward models. Here’s how this might look for the metrics of faithfulness and helpfulness:

**Faithfulness annotation** involves evaluating whether an answer accurately reflects the source text. Ideally, we’d like simple binary labels—faithful or unfaithful—but reality is rarely that straightforward. Answers typically exist on a spectrum. As a result, a mostly correct answer that misses a critical detail should be graded differently from one that incorrectly represents minor or peripheral information.

Related to faithfulness is the **“no-info” annotation**. This checks whether the model correctly identifies when the provided context doesn’t contain the information to answer the question. The goal here is to identify hallucinations, where the model invents answers instead of acknowledging the gap. As part of this exercise, we could have the following labels:

- **Incorrect answer / hallucination:** The model tries to answer despite missing information, even if the response sounds plausible.
- **Incorrect refusal:** The model mistakenly claims the information isn’t present, perhaps due to retrieval errors or inadequate attention to the long context.
- **Correct refusal:** The model accurately recognizes the absence of necessary details and appropriately declines to answer.

**Helpfulness comparisons** involve annotators judging which of two faithful answers better meets the user’s needs. Rather than asking for absolute ratings, annotators make relative judgments, answering a straightforward question: “Which answer is more helpful?” People find comparing two answers easier than assigning absolute ratings, resulting in greater consistency across annotators. When comparing helpfulness, annotators should consider:

- **Relevance**: Does one answer more directly and precisely address the question?
- **Comprehensiveness**: Does one answer include key information that the other misses?
- **Conciseness**: Is one answer more succinct and easier to understand?

Here are some practical tips for setting up a reliable annotation process:

- Start with clear guidelines: Include examples for each category and clarify how to handle edge cases. Also, be concise—it makes it easier to read the entire guide.
- Iterate on the guidelines: Our initial draft won’t be perfect. Collect annotator feedback on unclear or challenging cases to improve our guidelines.
- Use qualification tasks: Before assigning actual tasks, provide annotators with practice examples with known correct answers. This ensures they understand the guidelines and can apply them consistently.
- Measure inter-annotator agreement: Check for consistency among annotators using metrics like Cohen’s Kappa. Low agreement can indicate unclear guidelines or ambiguous scenarios needing further clarification.
- Consider expert annotators for specialized domains: General annotation tasks can usually be handled by crowd-workers, but domains like medicine or law often require subject-matter experts for accurate and meaningful evaluations.

That said, while human annotation is traditionally considered the gold standard, it’s not always practical or scalable, especially for large documents. **This is where LLM-evaluators (also called “LLM-as-Judge”) can help.** Via this approach, we provide clear criteria—or our annotation guidelines—to a model, and have it evaluate the quality of Q&A responses.

But first, it’s important to recognize why older automated metrics fall short. Historically, the language modeling community relied on n-gram-based metrics like BLEU and ROUGE, which measure word overlap between generated responses and reference answers. Although these metrics work somewhat for tasks like machine translation, they correlate poorly with human judgment on open-ended tasks such as Q&A.

For example, the [L-Eval](https://arxiv.org/abs/2307.11088) benchmark highlighted the poor correlation between token-overlap metrics and human judgment for Q&A responses. A correct answer using words that differ from the reference answer can get unfairly penalized by a low ROUGE score, leading to a misleading negative signal. This is especially noticeable when model responses and reference answers vary in length. Without length normalization, token-overlap metrics can mistakenly reward verbose yet mediocre answers over concise, accurate ones.

This is why model-based evaluation is increasingly popular—it offers more reliable and nuanced evals than traditional metrics. We typically start by calibrating an LLM-evaluator against a high-quality, human-annotated dataset. With ground truth, we can evaluate our LLM-evaluator by measuring its recall and precision on faithfulness annotations, and its correlation with human judgments on the helpfulness comparisons.

**To evaluate faithfulness, we can treat answers as collections of individual claims, each of which can be verified as true or false.** This is similar to approaches used in [NLI-based](https://arxiv.org/abs/2111.09525) and [Q&A-based](https://aclanthology.org/2022.naacl-main.187/) summarization metrics, and [claim generation and verification](https://arxiv.org/abs/2405.14486). Breaking answers down into atomic claims helps us pinpoint where hallucinations occur. Here’s how it works:

- **Extract claims:** Consider this response about a contract dispute: “The tenant breached the lease because they missed three payments, failed to maintain insurance coverage, and sublet the apartment without permission.” This can be split into:
  - Claim 1: The tenant missed three payments.
  - Claim 2: The tenant failed to maintain required insurance coverage.
  - Claim 3: The tenant sublet the apartment without permission.
- **Verify each claim:** Check each statement against the source document (in this case, the lease agreement) to confirm its accuracy.
- **Calculate faithfulness:** The proportion of claims supported by the document provides an overall faithfulness score.

This fine-grained approach, as demonstrated by evaluations like [SummaC](https://arxiv.org/abs/2111.09525), [QAFactEval](https://aclanthology.org/2022.naacl-main.187/), and [RefChecker](https://arxiv.org/abs/2405.14486), offers more interpretability and nuance. Rather than labeling an entire answer as faithful or not, we gain a nuanced understanding of which claims are incorrect. This also allows assigning partial credit to mostly faithful answers with minor inaccuracies.

We can also go a step further by requiring the model to provide citations for each claim. This helps distinguish between two different failure modes: hallucinations (making up answers) and retrieval failures (not retrieving relevant information).

To evaluate our evaluator, we can compare its judgments to human annotations on two key metrics: (i) recall (of all unfaithful claims, how many does the evaluator correctly flag?) and (ii) precision (of all claims the evaluator flags as unfaithful, how many are truly unfaithful?)

**Evaluating helpfulness requires a more nuanced approach because often, there isn’t a definitively “helpful” way to answer.** Different situations might call for varying levels of detail or explanation styles. Here are several strategies we can consider:

- **Reference-based comparison** works well when we have high-quality reference answers. The LLM-evaluator compares generated answers against these references to assess relevance, detail, and clarity. However, as models improve, their answers may surpass existing references, making this method less effective over time.
- **Criteria-based evaluation** assesses answers using a clearly defined rubric. This approach allows us to directly reuse our annotation guidelines, focusing on criteria like relevance, comprehensiveness, and conciseness.
- **Pairwise comparisons** are particularly useful when iteratively improving Q&A systems. By comparing newly generated answers against previously validated ones, we consistently push quality higher. This method is also ideal for A/B testing different configurations of the Q&A system.

To calibrate an LLM-evaluator on helpfulness, pairwise comparisons are especially reliable. By presenting pairs of answers to annotators and LLM-evaluators, we can measure their alignment—how often they agree on the more helpful answer. Correlation metrics, such as Cohen’s Kappa, quantify this alignment effectively. For example, L-Eval found that GPT-4’s pairwise comparisons correlated strongly with human preferences once properly calibrated.

## **What We Can Learn from Existing Benchmarks**

To ground our discussion so far, let’s look at some benchmarks for long-context Q&A. Besides providing a common standard, these benchmarks highlight challenges we might encounter in dataset creation and evaluation. Since these datasets are likely already part of model training data, we shouldn’t rely solely on them to evaluate our Q&A system. Instead, we’ll want to create evaluation datasets tailored to our use case.

We’ll cover six benchmarks spanning (i) narrative documents, (ii) technical and academic documents, and (iii) very long or multi-document contexts.

**The NarrativeQA dataset**, introduced by [Kočiský et al. in 2017](http://arxiv.org/abs/1712.07040), is designed to test genuine narrative comprehension rather than surface-level pattern matching. Unlike earlier datasets that allowed models to answer by extracting single sentences, NarrativeQA requires synthesizing information scattered across novels and movie scripts to generate answers.

First, the authors collected over 1,500 stories from Project Gutenberg and movie script websites, along with their corresponding plot summaries from Wikipedia. Annotators then generated question-answer pairs based only on these summaries, without viewing the full texts. (Conversely, models answered questions based on the full text but not the summaries.) This deliberate approach ensured that answers couldn’t be found by simple text matching, focusing the evaluation on understanding the entire text. The resulting dataset contains 46,765 question-answer pairs focused on narrative comprehension.

![Image](https://eugeneyan.com/assets/narrativeqa.webp "Image")

Statistics of the NarrativeQA dataset

NarrativeQA evaluates whether models can integrate information dispersed throughout long narratives, such as entire books or movies, to produce coherent answers. Answers are evaluated on n-gram matching metrics such as BLEU, METEOR, and ROUGE, comparing machine-generated answers against two reference answers for each question.

NarrativeQA highlights the importance of questions that go beyond simple extraction, requiring models to integrate information across the document. By generating questions from summaries instead of full texts, the authors ensured questions required holistic comprehension of the text, thus reducing superficial, extractive answering strategies.

**NovelQA**, introduced by [Wang et al. in 2024](http://arxiv.org/abs/2403.12766), is a benchmark designed for evaluating reading comprehension on very long texts, often exceeding 200,000 tokens. Similar to NarrativeQA but updated for modern times, NovelQA assesses how well models understand and integrate narratives spanning entire novels. Models were evaluated in two formats: multiple-choice and open-ended generation.

![Image](https://eugeneyan.com/assets/novelqa-fig2.webp "Image")

Two types of responses in NovelQA

To build the dataset, the authors selected a diverse set of 89 English novels and collaborated closely with English literature students familiar with these works. Annotators created 2,305 questions in two phases. First, annotators used a question template and filled in entities from the novel to form valid questions (templates below).

![Image](https://eugeneyan.com/assets/novelqa-table5.webp "Image")

Templates used to generate questions in NovelQA

Then, to enhance question diversity, annotators also freely generated challenging questions. All the questions were then reviewed by the authors, who ultimately accepted 79.4% of the questions. Each question was accompanied by a gold-standard answer and the relevant supporting evidence from the novels to ground evaluations.

NovelQA evaluates a model’s ability to synthesize, integrate, and recall detailed information across extremely long contexts. Questions fall into these categories:

- Detail-oriented (22.2%): Focus on subtle specifics requiring careful recall.
- Single-hop (42.8%): Answerable from adjacent sentences or closely related passages.
- Multi-hop (35%): Requires synthesizing information across multiple chapters.

The questions cover various narrative aspects, such as characters, plot, setting, and deeper thematic meanings. The benchmark supports both multiple-choice and open-ended generative evaluation methods, with GPT-4 serving as evaluator for generative answers (achieving Cohen’s Kappa of 89.25% against human judgments).

![Image](https://eugeneyan.com/assets/novelqa-table7.webp "Image")

Data distribution by complexity and aspect in NovelQA

NovelQA’s findings are a shift from the typical “lost in the middle” problem—it showed that model performance declines when evidence appears beyond the 100,000-token mark. The authors also highlighted the importance of rigorous quality control, manually reviewing all crowd-generated questions and accepting only 79.4% of question-answer pairs. Finally, explicitly linking each answer to specific supporting evidence helps with retrieval evals.

![Image](https://eugeneyan.com/assets/novelqa-fig3.webp "Image")

Performance of models decline when evidence is beyond 100k tokens

While narrative texts present one kind of challenge, comprehending dense, technical documents introduces an entirely different set of difficulties.

**QASPER**, introduced by [Dasigi et al. (2021)](https://arxiv.org/abs/2105.03011), addresses this by testing models on information-seeking questions on academic papers. Specifically, QASPER contains 5,049 questions on 1,585 NLP papers. Similar to NarrativeQA, these questions were crafted by NLP practitioners who had only read paper titles and abstracts. This approach ensures questions often require synthesizing information across the entire paper rather than simple text extraction.

![Image](https://eugeneyan.com/assets/qasper-fig1.webp "Image")

Example question, answer, and supporting evidence in QASPER

First, 25 NLP practitioners selected papers that interested them and created questions based solely on titles and abstracts. Then, another group of 51 NLP experts answered these questions using the full texts. The latter group’s task included determining if questions were answerable, pinpointing specific supporting evidence (such as text passages, figures, or tables), and providing clear, concise answers. (10% of questions were marked unanswerable and thus excluded.) Separating question generation from answer annotation reduced biases, as question authors had no prior knowledge of the detailed answers.

QASPER evaluates models on two main aspects: answer accuracy (Answer-F1) and evidence selection (Evidence-F1). Answer-F1 measures the accuracy of model responses, regardless of whether they extract text directly or create new explanations. Evidence-F1 evaluates the model’s ability to identify supporting details. This is particularly challenging, as more than half of the questions require combining evidence from multiple sections or paragraphs.

The Evidence-F1 results in QASPER highlight a significant gap between answer generation and evidence retrieval—even when models give accurate answers, they often struggle to identify the exact supporting passages. Additionally, limiting question creators to only titles and abstracts naturally encouraged questions—and answers—that required a deep understanding of the entire paper, moving beyond superficial extraction.

**L-Eval** by [An et al. (2023)](https://arxiv.org/abs/2307.11088) covers documents ranging from 3,000 to 200,000 tokens and includes 20 diverse subtasks, 508 extensive documents, and over 2,000 human-annotated question-answer pairs. Unlike previous benchmarks that mainly relied on text-matching metrics, L-Eval also applied LLM-evaluators and measured the difference between both.

To build L-Eval, the authors first created four new datasets: Coursera (educational content), SFiction (science fiction stories), CodeU (Python codebases), and LongFQA (financial earnings). They also improved five existing datasets by adding more challenging synthesis-oriented questions, such as augmenting QuALITY to require deeper comprehension of entire documents. Lastly, they reviewed and corrected 12 tasks from prior benchmarks, using Claude-100k to identify and remove inaccuracies or unanswerable questions.

![Image](https://eugeneyan.com/assets/leval-table1.webp "Image")

Statistics of datasets, question types, and domains in L-Eval

L-Eval evaluates two types of tasks: closed-ended (like multiple-choice, code comprehension, true/false, and math), emphasizing precise reasoning, and open-ended (such as narrative synthesis and summarization), focusing on integrating and summarizing long-form content.

Closed-ended tasks were evaluated via exact-match accuracy while open-ended tasks had human annotators rating responses from 1 (poor) to 5 (excellent). Additionally, L-Eval used language models like GPT-4 and GPT-3.5 as evaluators through pairwise comparisons for open-ended tasks. These had carefully designed prompts to reduce bias toward overly detailed answers. Traditional n-gram metrics, including ROUGE-L and F1 scores, were also used for efficiency, despite their known sensitivity to response length.

L-Eval showed that traditional n-gram metrics often fail to reflect true comprehension in long-context scenarios due to mismatched answer lengths. Additionally, the benchmark demonstrated that using LLMs as evaluators in pairwise comparisons provides superior alignment with human assessments compared to traditional metrics, highlighting clear distinctions in model strengths for closed-ended versus open-ended tasks.

**HELMET** (How to Evaluate Long-context Models Effectively and Thoroughly), introduced by [Yen et al. (2025)](https://arxiv.org/abs/2410.02694), addresses issues in earlier benchmarks, such as unrealistic tasks and inconsistent metrics, providing a framework for evaluating long-context language models.

To start, the authors identified shortcomings in existing evaluations, including limited context lengths, unreliable methods, and inadequate coverage for non-instruction-tuned models. Then, they created a benchmark with seven task categories: Retrieval-Augmented Generation (RAG), generation with citations, passage re-ranking, many-shot in-context learning, long-document question-answering, summarization, and synthetic recall. Each task contains contexts of up to 128,000 tokens, allowing controlled and consistent assessments with carefully crafted few-shot prompts and model-based metrics.

![Image](https://eugeneyan.com/assets/helmet-table3.webp "Image")

Task categories, datasets, and metrics in HELMET

HELMET specifically evaluates these capabilities in long-context models:

- Retrieval and reasoning: Natural Questions, TriviaQA, and HotpotQA test a model’s ability to find relevant information within extensive contexts containing distractors.
- Instruction following: Generation tasks requiring citations assess whether models can follow precise formatting guidelines while staying accurate.
- Comparative reasoning: Passage re-ranking evaluates how well models compare and reason across multiple sections of text.
- In-context learning: Many-shot tasks measure a model’s ability to quickly adapt and learn from multiple examples provided in-context.
- Long-form comprehension: Long-document question-answering and summarization tasks assess a model’s capability to synthesize and understand extensive texts.

HELMET showed that synthetic tasks like Needle In a Haystack aren’t as useful, due to their weak correlation with real-world scenarios. Also, by carefully controlling input lengths, HELMET could evaluate model robustness to increasingly long contexts that approached previous models’ limits (≥128K tokens). Similar to previous benchmarks, HELMET replicated the flaws in traditional n-gram metrics such as ROUGE, which can misrepresent quality in longer outputs. Instead, it recommended using model-based evaluations, using models like GPT-4o, for evaluations that align more closely with human judgment.

![Image](https://eugeneyan.com/assets/helmet-fig1.webp "Image")

Comparison of benchmark results across NIAH, Ruler, InfinityBench, and HELMET

**Loong**, by [Wang et al. (2024)](https://arxiv.org/abs/2406.17419), is a benchmark that evaluates long-context comprehension across *multiple* documents. While most earlier benchmarks focus on single-document scenarios, Loong presents realistic, multi-document tasks where missing any relevant document results in incorrect answers.

![Image](https://eugeneyan.com/assets/loong-fig1.webp "Image")

Loong focuses on multi-document Q&A

Loong consists of 1,600 evals drawn from financial reports, legal cases, and academic papers in English and Chinese, mainly from 2024. Each task includes evidence spread across multiple documents, mimicking real-world complexity. To generate questions, the authors used two methods: template-based generation, where Q&A pairs were constructed through predefined rules, and free annotation, where GPT-4o was prompted to create additional Q&A pairs.

Loong evaluates a model’s ability to locate, compare, cluster, and reason on evidence spread across multiple documents, typically ranging from 10,000 to over 250,000 tokens. The benchmark covers four task types:

- **Spotlight**: Finding relevant evidence from one specific document among several.
- **Comparison**: Comparing and integrating multiple pieces of information from different documents and returning the right answer.
- **Clustering**: Aggregating and grouping relevant information from multiple sources based on specific criteria.
- **Chain of Reasoning**: Integrating evidence across documents to return answers.

![Image](https://eugeneyan.com/assets/loong-fig2.webp "Image")

The four evaluation tasks in Loong

For evaluation, GPT-4 was used as the LLM-evaluator to score model outputs based on accuracy, hallucinations, and completeness, referencing the golden answer and task requirements. Metrics included (i) average scores (the average evaluation across all questions) and (ii) perfect rate (the percentage of questions receiving a perfect score).

Interestingly, their analysis of retrieval-augmented generation (RAG) showed that using RAG *reduced* performance on the Loong benchmark. They hypothesized that this is because Loong’s evidence is dispersed across multiple documents. While RAG helped somewhat on spotlight tasks, it performed poorly on tasks demanding deeper synthesis, such as comparison, clustering, and multi-step reasoning.

![Image](https://eugeneyan.com/assets/loong-fig3.webp "Image")

The use of RAG degrades performance compared to the baseline

Here are some other long-context benchmarks that you may find helpful:

- [A Critical Evaluation of Evaluations for Long-form Question Answering](http://arxiv.org/abs/2305.18201)
- [Ada-LEval: Evaluating long-context LLMs with length-adaptable benchmarks](http://arxiv.org/abs/2404.06480)
- [BABILong: Testing the Limits of LLMs with Long Context Reasoning-in-a-Haystack](http://arxiv.org/abs/2406.10149)
- [BAMBOO: A Comprehensive Benchmark for Evaluating Long Text Modeling Capacities of Large Language Models](http://arxiv.org/abs/2309.13345)
- [BizBench: A Quantitative Reasoning Benchmark for Business and Finance](http://arxiv.org/abs/2311.06602)
- [ELI5: Long Form Question Answering](http://arxiv.org/abs/1907.09190)
- [Frustratingly Hard Evidence Retrieval for QA Over Books](http://arxiv.org/abs/2007.09878)
- [InfinityBench: Extending Long Context Evaluation Beyond 100K Tokens](http://arxiv.org/abs/2402.13718)
- [LFED: A Literary Fiction Evaluation Dataset for Large Language Models](http://arxiv.org/abs/2405.10166)
- [LongBench: A Bilingual, Multitask Benchmark for Long Context Understanding](http://arxiv.org/abs/2308.14508)
- [LongBench v2: Towards Deeper Understanding and Reasoning on Realistic Long-Context Multitasks](http://arxiv.org/abs/2412.15204)
- [LongReason: A Synthetic Long-Context Reasoning Benchmark via Context Expansion](http://arxiv.org/abs/2501.15089)
- [MultiDoc2Dial: Modeling Dialogues Grounded in Multiple Documents](http://arxiv.org/abs/2109.12595)
- [QuALITY: Question Answering with Long Input Texts, Yes!](http://arxiv.org/abs/2112.08608)

Whew, that was a lot! Here are some key takeaways:

- **Faithfulness and helpfulness are orthogonal dimensions.** An answer can be faithful yet unhelpful, or helpful yet contain hallucinated information.
- **Faithfulness also means knowing when to say “I don’t know”.** Models should decline to answer when the context lacks information and respond correctly when it does.
- **Traditional n-gram metrics struggle on Q&A.** Use LLM-evaluators instead. They’re better at evaluating semantic quality and align more closely with human judgment.
- **The location of evidence matters.** Across the benchmarks discussed, some models struggled with the “lost in the middle” effect while others had poor performance when the evidence was beyond the 100,000 token mark.
- **Using RAG can reduce performance**, especially for tasks requiring cohesive reasoning across evidence dispersed across a single or multiple documents.

Did I miss anything important? Any other metrics, methods, or benchmarks you’d suggest I look into? Please [let me know](https://x.com/eugeneyan)!

By the way, if you want to learn more about evals, my friends Hamel and Shreya are hosting their *final* cohort of “AI Evals for Engineers and PMs” in July. Here’s a [35% discount code](https://maven.com/parlance-labs/evals?promoCode=eugene-is-all-you-need).

## References

An, Chenxin, Shansan Gong, Ming Zhong, Xingjian Zhao, Mukai Li, Jun Zhang, Lingpeng Kong, and Xipeng Qiu. 2023. “L-Eval: Instituting Standardized Evaluation for Long Context Language Models.” arXiv. https://doi.org/10.48550/arXiv.2307.11088.

Bai, Yushi, Xin Lv, Jiajie Zhang, Hongchang Lyu, Jiankai Tang, Zhidian Huang, Zhengxiao Du, et al. 2024. “LongBench: A Bilingual, Multitask Benchmark for Long Context Understanding.” arXiv. https://doi.org/10.48550/arXiv.2308.14508.

Bai, Yushi, Shangqing Tu, Jiajie Zhang, Hao Peng, Xiaozhi Wang, Xin Lv, Shulin Cao, et al. 2025. “LongBench v2: Towards Deeper Understanding and Reasoning on Realistic Long-Context Multitasks.” arXiv. https://doi.org/10.48550/arXiv.2412.15204.

Dasigi, Pradeep, Kyle Lo, Iz Beltagy, Arman Cohan, Noah A. Smith, and Matt Gardner. 2021. “A Dataset of Information-Seeking Questions and Answers Anchored in Research Papers.” arXiv. https://doi.org/10.48550/arXiv.2105.03011.

Dong, Zican, Tianyi Tang, Junyi Li, Wayne Xin Zhao, and Ji-Rong Wen. 2024. “BAMBOO: A Comprehensive Benchmark for Evaluating Long Text Modeling Capacities of Large Language Models.” arXiv. https://doi.org/10.48550/arXiv.2309.13345.

Fabbri, Alexander, Chien-Sheng Wu, Wenhao Liu, and Caiming Xiong. 2022. “QAFactEval: Improved QA-Based Factual Consistency Evaluation for Summarization.” In Proceedings of the 2022 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies. Seattle, United States: Association for Computational Linguistics. https://doi.org/10.18653/v1/2022.naacl-main.187.

Fan, Angela, Yacine Jernite, Ethan Perez, David Grangier, Jason Weston, and Michael Auli. 2019. “ELI5: Long Form Question Answering.” arXiv. https://doi.org/10.48550/arXiv.1907.09190.

Feng, Song, Siva Sankalp Patel, Hui Wan, and Sachindra Joshi. 2021. “MultiDoc2Dial: Modeling Dialogues Grounded in Multiple Documents.” In Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing, 6162–76. https://doi.org/10.18653/v1/2021.emnlp-main.498.

Hu, Xiangkun, Dongyu Ru, Lin Qiu, Qipeng Guo, Tianhang Zhang, Yang Xu, Yun Luo, Pengfei Liu, Yue Zhang, and Zheng Zhang. 2024. “RefChecker: Reference-Based Fine-Grained Hallucination Checker and Benchmark for Large Language Models.” arXiv. https://doi.org/10.48550/arXiv.2405.14486.

Kočiský, Tomáš, Jonathan Schwarz, Phil Blunsom, Chris Dyer, Karl Moritz Hermann, Gábor Melis, and Edward Grefenstette. 2017. “The NarrativeQA Reading Comprehension Challenge.” arXiv. https://doi.org/10.48550/arXiv.1712.07040.

Koncel-Kedziorski, Rik, Michael Krumdick, Viet Lai, Varshini Reddy, Charles Lovering, and Chris Tanner. 2024. “BizBench: A Quantitative Reasoning Benchmark for Business and Finance.” arXiv. https://doi.org/10.48550/arXiv.2311.06602.

Kuratov, Yuri, Aydar Bulatov, Petr Anokhin, Ivan Rodkin, Dmitry Sorokin, Artyom Sorokin, and Mikhail Burtsev. 2024. “BABILong: Testing the Limits of LLMs with Long Context Reasoning-in-a-Haystack.” arXiv. https://doi.org/10.48550/arXiv.2406.10149.

Laban, Philippe, Tobias Schnabel, Paul N. Bennett, and Marti A. Hearst. 2021. “SummaC: Re-Visiting NLI-Based Models for Inconsistency Detection in Summarization.” arXiv. https://doi.org/10.48550/arXiv.2111.09525.

Ling, Zhan, Kang Liu, Kai Yan, Yifan Yang, Weijian Lin, Ting-Han Fan, Lingfeng Shen, Zhengyin Du, and Jiecao Chen. 2025. “LongReason: A Synthetic Long-Context Reasoning Benchmark via Context Expansion.” arXiv. https://doi.org/10.48550/arXiv.2501.15089.

Mou, Xiangyang, Mo Yu, Bingsheng Yao, Chenghao Yang, Xiaoxiao Guo, Saloni Potdar, and Hui Su. 2020. “Frustratingly Hard Evidence Retrieval for QA Over Books.” arXiv. https://doi.org/10.48550/arXiv.2007.09878.

Pang, Richard Yuanzhe, Alicia Parrish, Nitish Joshi, Nikita Nangia, Jason Phang, Angelica Chen, Vishakh Padmakumar, et al. 2022. “QuALITY: Question Answering with Long Input Texts, Yes!” arXiv. https://doi.org/10.48550/arXiv.2112.08608.

Wang, Chonghua, Haodong Duan, Songyang Zhang, Dahua Lin, and Kai Chen. 2024. “Ada-LEval: Evaluating Long-Context LLMs with Length-Adaptable Benchmarks.” arXiv. https://doi.org/10.48550/arXiv.2404.06480.

Wang, Cunxiang, Ruoxi Ning, Boqi Pan, Tonghui Wu, Qipeng Guo, Cheng Deng, Guangsheng Bao, et al. 2024. “NovelQA: Benchmarking Question Answering on Documents Exceeding 200K Tokens.” arXiv. https://doi.org/10.48550/arXiv.2403.12766.

Wang, Minzheng, Longze Chen, Cheng Fu, Shengyi Liao, Xinghua Zhang, Bingli Wu, Haiyang Yu, et al. 2024. “Leave No Document Behind: Benchmarking Long-Context LLMs with Extended Multi-Doc QA.” arXiv. https://doi.org/10.48550/arXiv.2406.17419.

Xu, Fangyuan, Yixiao Song, Mohit Iyyer, and Eunsol Choi. 2023. “A Critical Evaluation of Evaluations for Long-Form Question Answering.” arXiv. https://doi.org/10.48550/arXiv.2305.18201.

Yen, Howard, Tianyu Gao, Minmin Hou, Ke Ding, Daniel Fleischer, Peter Izsak, Moshe Wasserblat, and Danqi Chen. 2025. “HELMET: How to Evaluate Long-Context Language Models Effectively and Thoroughly.” arXiv. https://doi.org/10.48550/arXiv.2410.02694.

Yu, Linhao, Qun Liu, and Deyi Xiong. 2024. “LFED: A Literary Fiction Evaluation Dataset for Large Language Models.” arXiv. https://doi.org/10.48550/arXiv.2405.10166.

Zhang, Xinrong, Yingfa Chen, Shengding Hu, Zihang Xu, Junhao Chen, Moo Khai Hao, Xu Han, et al. 2024. “InfinityBench: Extending Long Context Evaluation Beyond 100K Tokens.” arXiv. https://doi.org/10.48550/arXiv.2402.13718.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Jun 2025). Evaluating Long-Context Question & Answer Systems. eugeneyan.com. https://eugeneyan.com/writing/qa-evals/.

or

```
@article{yan2025qa,
  title   = {Evaluating Long-Context Question & Answer Systems},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2025},
  month   = {Jun},
  url     = {https://eugeneyan.com/writing/qa-evals/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
