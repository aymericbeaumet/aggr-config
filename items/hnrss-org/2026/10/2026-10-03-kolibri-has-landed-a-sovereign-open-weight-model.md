---
title: 'Kolibri Has Landed: A Sovereign Open-Weight Model'
link: https://aleph-alpha.com/en/blog/kolibri-has-landed-a-sovereign-open-weight-model/
source: hnrss-org
published: 2026-10-03T09:36:04Z
updated: 2026-10-03T09:36:04Z
first_seen: 2026-10-03T15:15:59.697027369Z
authors:
- bastitx
content: extracted
html: 2026-10-03-kolibri-has-landed-a-sovereign-open-weight-model.html
preview:
  file: 2026-10-03-kolibri-has-landed-a-sovereign-open-weight-model.preview-31590c6c684f.webp
  width: 256
  height: 88
  color: '#34a97a'
images:
- source: https://aleph-alpha.com/_astro/00-cover.Du35XCGh_zJqw.jpeg
  original:
    file: 2026-10-03-kolibri-has-landed-a-sovereign-open-weight-model.image-cb6c1e291e58.jpg
    width: 1200
    height: 413
  color: '#3bb484'
- source: https://aleph-alpha.com/_astro/00-cover.Du35XCGh_ZBjCLW.webp
  original:
    file: 2026-10-03-kolibri-has-landed-a-sovereign-open-weight-model.image-6ff15c29aaad.webp
    width: 1920
    height: 660
  variants:
  - file: 2026-10-03-kolibri-has-landed-a-sovereign-open-weight-model.image-92d2008e4826.webp
    width: 320
    height: 110
  color: '#3bb484'
---

![Green gradient with the white Kolibri hummingbird logo and wordmark in the centre, framed by thin stepped outlines on the left and right](https://aleph-alpha.com/_astro/00-cover.Du35XCGh_ZBjCLW.webp)

 Research

Aleph Alpha

 03/10/2026

On the Day of German Reunification, we are releasing our new model: Kolibri.

Kolibri is an English-German Mixture-of-Experts Transformer with 78B total parameters, 3B active. It supports context lengths of up to 1M tokens. The model can be downloaded with the full weights on [Hugging Face](https://huggingface.co/Aleph-Alpha/Kolibri-1) and used under the Apache 2.0 license terms.

Kolibri is the result of continuous iteration of our model training effort. We first built [a model training pipeline](https://aleph-alpha.com/en/blog/model-training-as-code/) and validated it by building Kolibri Origin, a 30B total, 3B active model with a much shorter 65k token context window. Kolibri ran through the same pipeline: from data ingestion and curation, through ablations, pre-training, and post-training, to the final evals. It enabled running hundreds of ablation experiments and stable pre-training that ran without a person having to step in when hardware failed or a data connection dropped. We continuously monitored training metrics and standardized monitoring for custom benchmarks. The time we put into building and iterating on this pipeline was a valuable investment. We see it in how much better Kolibri is than Kolibri Origin, and in how little time separates their releases.

Kolibri is a specialized language model built for sovereign mission-critical work in regulated areas including public administration, industrials and aerospace. We specialized Kolibri for German, reasoning, math, agentic behavior, and further capabilities our customers need in production. The aim of this specialization was to optimize performance in our customers' specific use cases. Through specialization, customers achieve contextualized performance in their AI operations and they can monitor its economic impact, so that ROI stays measurable and grows over time.

Specialization alone is not enough. Sovereignty is just as important. Sovereignty, for us, combines two dimensions: how we built the model, and how it transfers to our customers. We offer full supply-chain integrity and account for every decision, from data ingestion, through pre- and post-training, to the final evaluations. We provide transparency. Customers have full freedom of deployment and intellectual-property safety, so compliance comes as an inherited property of the model.

Read our [tech report](https://aleph-alpha.com/downloads/tech-report.pdf) for full details.

## What Kolibri Delivers

We optimized Kolibri for performance across a wide range of sectors considering their particular domain-specific language, regulatory, and procedural realities. Its small and efficient size provides our customers with flexibility to run it efficiently on-premise, without sending internal data to third-party inference services. The spotlight in this section introduces the model's capabilities, before we describe them in section [How we built Kolibri at high velocity](https://aleph-alpha.com/en/blog/kolibri-has-landed-a-sovereign-open-weight-model/#how-we-built-kolibri).

### Foundational capabilities for enterprise and government

With Kolibri we optimize the trade-off between model capability and deployment costs, using 3B active parameters out of 78B total. Kolibri sits on the Pareto frontier for quality versus serving cost, for both English and German. The Pareto frontier is a concept from economics, marking the best achievable combinations of two objectives, where improving on one means giving up some of the other. None of the compared models delivers more quality at the same serving cost, or the same quality at lower cost.

Average benchmark score \[%\]

English

German

- Kolibri
- Kolibri Origin
- Other post-trained models
- Pareto frontier

Performance vs. throughput for post-trained models in English (left) and German (right). Metrics show unweighted average benchmark scores against decoded text per second and GPU. Higher and further right is better.

Across math, coding, grounding, and long-context tasks, Kolibri matches models with up to four times its active parameter count, such as Nemotron 3 Super.

Show the numbers

| Benchmark            | Kolibri | Kolibri Origin | Qwen3.6-35B-A3B | Nemotron 3 Super 120B-A12B | Mistral Small 4 119B-A6B |
| -------------------- | ------- | -------------- | --------------- | -------------------------- | ------------------------ |
| AIME 2025            | 96.9    | 81.9           | 84.6            | 91.7                       | 79.8                     |
| AIME 2025 (DE)       | 87.5    | 73.5           | 82.9            | 85.6                       | 72.3                     |
| AIME 2026            | 96.0    | 81.5           | 91.0            | 90.4                       | 83.1                     |
| AIME 2026 (DE)       | 90.0    | 75.2           | 84.4            | 87.5                       | 78.5                     |
| GPQA (diamond)       | 84.3    | 68.1           | 83.4            | 78.0                       | 74.7                     |
| GPQA (diamond, DE)   | 81.3    | 58.5           | 80.6            | 76.6                       | 72.9                     |
| AA-Omniscience Index | -32.8   | -64.0          | -15.3           | -36.5                      | -24.0                    |
| BrowseComp           | 29.4    | 4.4            | 26.9            | 29.1                       | –                        |
| τ³-bench banking     | 38.1    | 5.7            | 10.6            | 15.5                       | 5.7                      |
| τ²-bench retail      | 69.9    | 58.5           | 71.6            | 67.5                       | 62.9                     |
| τ²-bench airline     | 76.7    | 58.7           | 70.7            | 72.7                       | 40.0                     |
| τ²-bench telecom     | 94.7    | 67.5           | 99.1            | 68.1                       | 41.5                     |
| BFCL v4 overall      | 61.4    | 36.4           | 67.2            | 61.0                       | 58.0                     |
| LiveCodeBench v6     | 85.9    | 59.2           | 82.5            | 82.0                       | 71.2                     |
| HumanEval+           | 92.7    | 76.8           | 92.8            | 94.7                       | 92.8                     |
| LongBench Pro        | 64.5    | –              | 70.8            | 62.9                       | 56.4                     |
| AA-LCR               | 68.3    | –              | 69.7            | 67.0                       | 52.3                     |

**Foundational capabilities of Kolibri.** Kolibri is a balanced generalist model with competitive performance across math, code, long context, agentic capabilities, and knowledge. Benchmark scores are shown on a shared 0–100 scale, higher is better.

### Contextualized performance for real-world applications

Public benchmarks fail to capture specialized sector needs, so we developed our own internal evaluation suites for the verticals that matter to our customers, such as the German public sector, aviation, manufacturing and the automotive industry. Each suite mirrors the skills, workflows, and edge cases required in these sectors, and paired synthetic training environments let us improve Kolibri against these evaluations without ever training on customer data. Read more in section [Contextualized performance](https://aleph-alpha.com/en/blog/kolibri-has-landed-a-sovereign-open-weight-model/#contextualized-performance).

Score on internal customer-proxy benchmark

- Automotive supplier 0.72 → 0.99
- Semiconductors 0.35 → 0.80
- German public sector 0.54 → 0.75
- Industrial drive technology 0.31 → 0.60
- Aerospace 0.14 → 0.59
- one checkpoint, one eval
- mean of that day
- Kolibri Origin
- Kolibri

Contextualized performance across successive post-training runs on internal customer-proxy benchmarks. Dots are single evaluations of training checkpoints, lines are the mean of each day's evaluations. Performance climbs across all five verticals, higher is better.

**Answers that are grounded in your documents.** We trained Kolibri with abstention data and with our Merlin-Arthur protocol. As a result, it is trained to say "I don't know" when the answer isn't in the context. Our customers value and request this feature, so we continuously track and validate abstention accuracy. We provide more details in the [Grounding](https://aleph-alpha.com/en/blog/kolibri-has-landed-a-sovereign-open-weight-model/#grounding) section.

**Native German and English model.** We developed a bilingual German/English tokenizer and focused on [including organic German data](https://aleph-alpha.com/en/blog/sauerkraut-not-burgers-why-german-llms-need-german-data/) throughout the training process of the model, so that 21.3% of the pre-training tokens are German. We used translation sparingly (6% overall), since translated text tends to carry the cultural fingerprint of its source language. The result is a model that is bilingual by design, not an English model that has read some German. Read more in sections [German pre-training data](https://aleph-alpha.com/en/blog/kolibri-has-landed-a-sovereign-open-weight-model/#german-pre-training-data) and [Specialized tokenizers](https://aleph-alpha.com/en/blog/kolibri-has-landed-a-sovereign-open-weight-model/#specialized-tokenizers).

### Control and compliance by design: upholding our customers' sovereignty

We built Kolibri with the EU AI Act, the [General-Purpose AI Code of Practice](https://aleph-alpha.com/en/blog/a-signature-and-a-stance-why-we-signed-the-code-of-practice/) and the GDPR in mind from the ground up, with copyright law being a focus of our work to trustworthy technology.

We are **transparent** about our [model weights](https://huggingface.co/Aleph-Alpha/Kolibri-1) and about [curation of our training data](https://aleph-alpha.com/en/blog/sauerkraut-not-burgers-why-german-llms-need-german-data/), so that decisions behind development are visible. Through its reasoning traces, it becomes **explainable** how the model came to a particular answer. With our [Merlin-Arthur protocol](https://aleph-alpha.com/en/blog/bounding-hallucinations-merlin-arthur-protocols-for-mutual-information-bounds-in-language-models/) the model grounds **trustworthiness**: Kolibri refrains from answering when the context doesn't support an answer.

Our teams built the model in Germany, trained it on infrastructure in Germany and Finland, under European and German law, with no foreign **control**. We own the entire pipeline, from data curation, through pre- and post-training, to optimization in our Model Factory. This control of our end to end pipeline underpins the model's sovereignty. Control also passes on to our customers and Kolibri's small size gives them full freedom of deployment and supports controllable reasoning effort to trade cost and latency against answer quality.

## How We Built Kolibri at High Velocity

### Our Model Factory: two models, three months apart

Our Model Factory is our answer to iteration speed, minimizing the time it takes to go from knowing what a model gets wrong to training one that does better. We implemented the training pipeline as code, so that the learnings of our team landed in one versioned training recipe instead of scattered scripts and notes. Here is what that looked like between Kolibri Origin and Kolibri.

Work on the pipeline began in January. Five months and hundreds of ablation runs later, Kolibri Origin finished pre-training at target scale on 11 June. Kolibri finished on 11 September. In the three months between those two dates we went from 30B parameters to 78B, from a 65k-token context window to up to 1M, and from 7.5T training tokens to 20T. To get those 20T, the pipeline processed over 200T tokens of raw data, filtering, deduplicating, and curating it down to what we actually trained on. We changed the attention design, tripled the number of experts, increased sparsity, replaced the routing algorithm, improved our post-training data, more than doubled the number of environment tasks, and taught the model to reason at four different effort levels. Both models kept a similar number of parameters active per token, yet Kolibri trained faster per token than Kolibri Origin thanks to the work of our efficiency team.

|                                        | Kolibri Origin                                        | Kolibri |
| -------------------------------------- | ----------------------------------------------------- | ------- |
| 11 June 2026                           | 11 September 2026                                     |         |
| no public release                      | 3 October 2026                                        |         |
| Yes (one mode only)                    | Yes (none, low, medium, high)                         |         |
| 30.6B                                  | 78.1B                                                 |         |
| 3.27B                                  | 3.46B                                                 |         |
| 7.51T                                  | 20T                                                   |         |
| 50 (2 dense + 48 MoE, 1 shared expert) | 50 (all MoE, 1 shared expert)                         |         |
| 8,192 (8k)                             | 16,384 (16k)                                          |         |
| 65,536 (64k)                           | 262,144 (256k)                                        |         |
| 96,000                                 | 128,000                                               |         |
| 2,048                                  | 2,560                                                 |         |
| 32 / 4                                 | 48 / 4                                                |         |
| 128 / 8                                | 384 / 6                                               |         |
| 768                                    | 512                                                   |         |
| full attention, all layers             | sliding window (512) + full attention every 5th layer |         |
| EN: 1 Sept 2024, DE: 1 Aug 2025        | EN/DE: 18 Jun 2026                                    |         |

What made this possible is our training pipeline, an effort to convert research-grade model development into a fully automated production-ready infrastructure to design and train large language models. [Our pipeline codebase was shared by all](https://aleph-alpha.com/en/blog/model-training-as-code/). Every proposed code change triggers a small end-to-end model run; training, evaluation, to find out if something broke within minutes. Our runs are GitHub Actions workflows, and reproducing one means checking out a commit. We used the pipeline for the main training and the hundreds of ablations that ran to make our architectures and data-mix decisions.

Training checkpoints land roughly every hour and the pipeline automatically evaluates them on English and German knowledge, maths, code, instruction following, tool use, long context, safety, abstention to hallucination and grounding. We can watch capabilities appear every day rather than finding out how the model turned out at the end. The run itself held up better than we expected. Over 21 days of pre-training of Kolibri, we hit 38 unplanned interruptions, roughly one per 10,000 GPU-hours, caused by hardware faults or a connection timing out. Those were automatically handled by the pipeline without manual intervention. The cluster automatically restarted the training job(s) on a different set of nodes, and training picked up from a checkpoint at most 250 steps back.

Having access to the whole pipeline, with checkpoints available to everyone, means that any team can own a capability end to end rather than a stage of an assembly line. One team trained and evaluated the grounding capability of Kolibri as a single piece of work, seamlessly integrating into the overall model.

However, getting here was not a linear walk. We stumbled. We stopped the Kolibri Origin pre-training after a few trillion tokens and restarted it from scratch, as we identified a data-shuffling bug that escaped our tests. We ran ablations, took decisions, only to find a bug or an error in the configuration afterwards, forcing us to rerun some experiments. Alongside our own experience, we tracked state-of-the-art architectures, best practices, and the latest research advances in LLMs. Those accumulated learnings led to improvements in our processes and guardrails in our pipeline.

Looking at the delta between Kolibri Origin and Kolibri, the improvement is notable, but this is also the easy direction: more parameters, more data, a well-understood architecture family, and still at small scale. As we are contemplating scaling up, we are happy to face challenges on our foundation. But the most durable thing we built this year is not the pipeline, it is a team with the proven capability to build, post-train, and ship LLMs from raw data at high velocity.

### Architecture and pre-training

Kolibri has 78B total parameters, which is 2.5 times more than Kolibri Origin, with ~3B active parameters. In our experiments, increasing the size of our model from 32B to 123B led to ever-improved performance. Yet the larger size came with larger training and serving costs. The latter drove the decision: 123B can handle only 3 long-context 256k-token user queries on two H100s, while 78B handles 18 concurrent requests and decodes 28% faster. We used 384 smaller experts rather than fewer wide ones, as they performed better in our tests. We applied the same efficiency-first reasoning to attention. Out of 50 total layers, only 10 process full context, while the remaining 40 use a tight 512-token focused window. This keeps decode computation and memory bounded in those layers regardless of context length. The efficiency gains benefit both serving the trained model and training during post-training RL, which relies on huge amounts of inference.

We trained Kolibri on [768 B200 GPUs](https://aleph-alpha.com/en/blog/scaling-pre-training-in-practice-a-hierarchical-approach/) in three stages: 20T tokens of pre-training at a 16k sequence length over 21 days, 3.44T tokens of mid-training at 64k, and 200B tokens of long-context adaptation at 256k. This is nearly 24T tokens in total, roughly three times what Kolibri Origin consumed. German accounts for more than a fifth of the pre-training mix, about 4.3T tokens, against roughly 62% English and 14% code. Compared to pre-training, mid-training data is a much more selective pool of curated datasets weighted toward reasoning, problem-solving, code and agentic data. For long-context, rather than train on long documents alone, which tends to erode the skills acquired earlier, we interleaved long documents with the high-quality mid-training data of the previous stage. For the long-context mix, we removed synthetic long documents to avoid artificially inflating benchmarks such as RULER.

We optimized with Muon, as we did for Kolibri Origin. We put special focus on training stability, resulting in a robust training run without any loss spikes for either model. With Kolibri, for routing between experts during training, we introduce exact quantile balancing. Quantile balancing was introduced in Kimi K3, where the global quantile is estimated from histograms because an exact computation was considered too expensive to communicate; we show that it can be computed exactly at fixed cost independent of batch size, and that the exactness improves both load balance and model quality.

### Post-training at scale

Post-training happened in two stages; the first step is supervised fine-tuning to teach the model core reasoning ability and how to interact in a chat, followed by large-scale reinforcement learning to train the model to reason across a diverse suite of long-horizon tasks. We built a pipeline for both of these steps, which allowed us to exercise fine-grained control over the behavior of the model.

For SFT we generated a total of 174B tokens worth of synthetic data, which we filtered for quality and combined with filtered versions of permissively licensed open-source datasets to obtain a high-quality training mix of 268B tokens. For reinforcement learning we trained on a broad set of environments that contained more than 1.2 million curated tasks across diverse domains such as code & math reasoning, agentic tasks, instruction following, question answering, tool calling, and more. We optimized our in-house training codebase for high-performance and use asynchronous training, where we generate training data on our environments using the current model in parallel to training the model on already generated data.

Across both stages, we taught the model to reason at different effort levels (none, low, medium, high), which means that the user can exercise control over how much compute the model should invest to find a solution to the task at hand. This allows our customers to trade-off cost and inference speed against the quality of the final answer.

### German pre-training data

From our experiments on small proxy models, we found that training with around 20% German data leads to optimal results, which meant we needed to find 4T German tokens to train our model at a 20T horizon. Open German datasets help, but are far from enough: after deduplication and filtering, we were left with 390B German tokens, well short of our goal. As we argued in [Sauerkraut, Not Burgers](https://aleph-alpha.com/en/blog/sauerkraut-not-burgers-why-german-llms-need-german-data/), German capability has to come from high-quality German texts; relying heavily on machine-translations would lead to poor results due to subtle translation errors and a lack of authentic German cultural context. We closed the token gap in three different ways.

The first was to curate German from Common Crawl ourselves. We built a pipeline specialized for German data. German is not English, and German data cannot be filtered like English data: we had to retune filtering parameters for the German language. One typical filter in a language data pipeline is to remove documents with too many long words, but German administrative prose routinely exceeds the English bound on mean word length, so the standard settings quietly remove the register that public administration writes in. After retuning, our German pipeline gave us 1.3T unique tokens of organic German web.

The second was to rephrase German documents we already had. An LLM rewrites an organic German document in the style of an encyclopedia entry, a Q&A dialogue or a text passage, preserving its content. This teaches the model the same facts in several surface forms and multiplies the information contained in scarce data, and it is a different operation from translation: the source is German, so the subject matter and the cultural affinity stay German: chancellor, not president. It does not add much new knowledge, rather new phrasings of knowledge that was already in the corpus. Rephrasing gave us about 1T unique tokens, making it the single largest source of German in the model.

The third was translation, and this we only used in Kolibri Origin. Translating English into German works when the model, the prompts and the chunking are chosen carefully, but it carries two problems. Output can still show translationese, the literal rendering of idioms: "Drive safe!" becomes "Fahre sicher!" rather than "Komm gut an!". More importantly, cultural context does not translate. A corpus translated from English inherits the geographic, demographic and institutional distribution of the English web, so a model trained on it speaks German about a world that looks American.

In the end German entered Kolibri as a 2.4T-token unique pool, 80% of it curated or generated by us and 20% from open datasets, and the model saw it at 21.3% of pre-training tokens, roughly 4.3T over the 20T run through upsampling. Each German token was seen 1.8 times on average, well inside the four-epoch limit past which repetition stops paying off. Around 85% of the German the model read is web text, either organic or rephrased from organic. The rest is curated documents – parliamentary proceedings, legal texts and other data in the public domain – and that small translated share.

### Specialized tokenizers for English-German

We built a bilingual English-German tokenizer, trained and specialized on each model's pre-training dataset. Because of the 21% German share in our data, it compresses German language better than other SOTA models. This led to more efficient inference (fewer tokens) minimizing costs and shortening response times. We introduce a new way to train tokenizers, UniBPE, that respects the morphology of languages better than existing approaches, especially the compound structure of German, without sacrificing English token efficiency.

German web (FineWeb-2)

- Kolibri 128,000 vocab 4.90
- Kolibri Origin 96,000 vocab 4.69
- Plain BPE 128k 128,000 vocab 4.89
- GPT-5 200,019 vocab 4.35
- DeepSeek V4 129,280 vocab 3.72
- Kimi K3 163,586 vocab 3.28
- GLM 5.3 154,856 vocab 3.93
- Qwen3-Next 151,669 vocab 3.59
- Qwen3.5-3.8 248,077 vocab 4.17
- Gemini 262,144 vocab 4.13
- EuroLLM 128,000 vocab 4.08
- Tekken (Mistral, Nemotron, Apertus) 131,072 vocab 4.03

English web (FineWeb)

- Kolibri 128,000 vocab 4.58
- Kolibri Origin 96,000 vocab 4.50
- Plain BPE 128k 128,000 vocab 4.59
- GPT-5 200,019 vocab 4.67
- DeepSeek V4 129,280 vocab 4.59
- Kimi K3 163,586 vocab 4.62
- GLM 5.3 154,856 vocab 4.61
- Qwen3-Next 151,669 vocab 4.52
- Qwen3.5-3.8 248,077 vocab 4.47
- Gemini 262,144 vocab 4.49
- EuroLLM 128,000 vocab 4.16
- Tekken (Mistral, Nemotron, Apertus) 131,072 vocab 4.45

Tokenizer compression in average bytes per token on German and English web text. Kolibri achieves the best German compression in this comparison. Plain BPE 128k is standard BPE trained on the same data with the same settings as Kolibri, which isolates the effect of the training method. More text per token means fewer tokens per task – higher is better.

The two leading approaches to train tokenizers are [BPE](https://arxiv.org/abs/1508.07909) and [Unigram](https://arxiv.org/abs/1804.10959). We combine those two, keeping the bottom-up approach of BPE and using the Unigram training objective for selecting which merge to add to the vocabulary. On a 128k vocabulary trained on our English/German dataset, this substantially improves tokenization.

Bundessozialgerichtes Federal Social Court (genitive)

- Kolibri Bundes sozial gericht es
- GPT-5 Bund ess oz ial gericht es
- Qwen3.8 Bund ess oz ial gericht es
- Gemini Bund ess oz ial gericht es
- Mistral Medium 3.5 · Nemotron 3 Nano Bund ess oz ial gericht es

silkworm

- Kolibri silk worm
- GPT-5 sil kw orm
- Qwen3.8 sil kw orm
- Gemini sil kw orm
- Mistral Medium 3.5 · Nemotron 3 Nano sil kw orm

Protokolldaten log data

- Kolibri Protokoll daten
- GPT-5 Pro tok ol ld aten
- Qwen3.8 Protokol ld aten
- Gemini Protok ol ld aten
- Mistral Medium 3.5 · Nemotron 3 Nano Pro tok ol ld aten

coprocessors

- Kolibri co processors
- GPT-5 cop rocess ors
- Qwen3.8 cop rocess ors
- Gemini cop rocess ors
- Mistral Medium 3.5 · Nemotron 3 Nano cop rocess ors

How different tokenizers split the same words. The Kolibri tokenizer follows the morphology of the language. English and German words split into meaningful units, while competitors cut across morpheme boundaries. Lower is better: fewer, cleaner splits per word mean fewer tokens.

### Grounding: reducing hallucinations

Common LLM training and evaluation rewards guessing: a guess has some chance of landing the correct answer, while abstention has none. Therefore, models learn to answer with whatever they have, even if their input does not provide enough information to warrant a response. Asking a model to provide citations often backfires for the same reason: models can simply hallucinate plausible-looking sources to justify an ungrounded answer.

For a regulated customer, a model that knows to abstain is the difference between a pilot and a deployment. We treat saying "I don't know" as an important model capability and (1) develop and track dedicated grounding and anti-hallucination measures and (2) use and develop dedicated training procedures to improve this abstention capability.

During training, we use training data samples where the correct answer is "I don't know". While fine-tuning for abstention is gaining traction industry-wide, high-quality negative examples remain hard to come by, especially where our customers need them, in narrow verticals where all data is scarce. Therefore, we also train with our [Merlin-Arthur](https://arxiv.org/abs/2512.11614) procedure, developed in-house and explained in detail in [our blog post](https://aleph-alpha.com/en/blog/bounding-hallucinations-merlin-arthur-protocols-for-mutual-information-bounds-in-language-models/), which solves data scarcity by automatically exploiting the model's weaknesses at each training step and generates synthetic negative examples from available documents. This training becomes a game with three players. Arthur is the model we train and ship. Merlin takes an existing document context, generates a new one that increases Arthur's probability of answering correctly. Morgana generates a new datapoint by stripping out the relevant evidence from the document and tries to lure Arthur into a hallucination. Arthur does not know which one he's facing, so his valid strategy becomes to carefully read the question and the context, to be able to answer on Merlin's context, while recognizing that abstention is the strictly required answer for Morgana's redacted context – making any guess, even a lucky one, incorrect.

For measuring hallucination abstention, we use established benchmarks alongside our own proxies derived from customer use cases. We've also developed our own "M/A grounding score" which falls out of our Merlin-Arthur setup representing a lower bound on how much of the answer provably came from the document.

Kolibri hallucinates far less than Kolibri Origin: it abstains instead of answering wrong on 44% of [AA-Omniscience](https://huggingface.co/datasets/ArtificialAnalysis/AA-Omniscience-Public) items (Origin: 15%), and on [RGB](https://arxiv.org/abs/2309.01431) it holds back more often (86% vs 74%) and invents fewer falsehoods (87% vs 76%). On our own [M/A grounding score](https://aleph-alpha.com/en/blog/bounding-hallucinations-merlin-arthur-protocols-for-mutual-information-bounds-in-language-models/), which certifies rather than estimates how much of an answer came from the document, Kolibri reaches 0.23 where Kolibri Origin, along some other models, reach 0.

Show the numbers

| Benchmark                             | Kolibri | Kolibri Origin | Qwen3.6-35B-A3B | Qwen3-Next 80B-A3B | Nemotron 3 Super 120B-A12B | Mistral Small 4 119B-A6B |
| ------------------------------------- | ------- | -------------- | --------------- | ------------------ | -------------------------- | ------------------------ |
| AA-Omniscience Non-Hallucination Rate | 44.0    | 14.8           | 56.7            | 12.3               | 13.9                       | 34.7                     |
| RGB: holds back                       | 85.6    | 73.9           | 79.6            | 81.3               | 74.6                       | 82.3                     |
| RGB: invents nothing                  | 87.3    | 75.6           | 84.3            | 83.9               | 86.0                       | 87.0                     |
| FRAMES                                | 71.2    | 65.7           | 74.7            | 68.9               | 74.9                       | 71.9                     |
| M/A grounding score                   | 0.23    | 0.00           | 0.12            | 0.00               | 0.00                       | 0.06                     |

### Contextualized performance

Most public benchmarks fail to capture specialized sector needs. To measure contextualized performance – meaning how well a model handles the unique workflows and domain knowledge of specific industries – we need measurements of performance in specialized verticals which are important to our customers, such as the German public sector, legal, hardware, consumer electronics, and automotive.

For this, we first built domain-specific evaluation suites that mirror the skills, tool-calls, workflows, and edge cases required in key sectors and thus capture contextualized performance. Once this was done, we created training environments by generating high-quality specialized synthetic data capturing those required skills. To increase the real-world robustness of the model on those tasks, we additionally randomized the environments around concrete details (tools, configurations, harnesses).

Together, these contributed to an iterative engine that allowed us to sharpen the model's agentic RAG capabilities, domain-specific logic and capabilities, and to hill-climb the contextualized evaluations without ever training on customer data.

These evaluation suites run automatically as part of our pipeline: every checkpoint is scored on them as it lands. Kolibri compares well against competing open-weight models. On the agentic-RAG benchmark Honeypot, Kolibri outperforms all compared models, including the much larger Nemotron 3 Super and Mistral Small 4. On a cleaned version of the agentic-RAG benchmark MuSiQue, it is second only to the more massive Nemotron 3 Super and well ahead of the rest. On the five customer applications, Kolibri leads or is on par with the best model in four of five. These findings speak to our ability to handle a range of data and harness setups, across use-cases, whereas the compared models are sensitive to these details.

These evaluations for measuring contextualized performance can exist because customers tell us how their applications fall short. Each one encodes what we learned from those conversations: what kind of documents matter, which tools the model should get, which questions are hard, where the deployed system frustrates its users today. That makes the exchange concrete in both directions. A customer who shows us a failure case gets it turned into a benchmark that every future checkpoint is measured against, and we can hill-climb the target without ever training on customer data or risking over-fitting.

Show the numbers

| Benchmark                   | Kolibri | Kolibri Origin | Qwen3-Next 80B-A3B | Qwen3.6-35B-A3B | Nemotron 3 Super 120B-A12B | Mistral Small 4 119B-A6B |
| --------------------------- | ------- | -------------- | ------------------ | --------------- | -------------------------- | ------------------------ |
| MuSiQue (cleaned)           | 77.3    | 42.7           | 50.5               | 61.2            | 79.1                       | 66.8                     |
| Honeypot                    | 80.8    | 25.3           | 13.5               | 74.3            | 68.8                       | 68.1                     |
| Semiconductors              | 80.4    | 35.3           | 41.2               | 79.4            | 69.6                       | 62.7                     |
| German public sector        | 75.0    | 54.0           | 29.5               | 72.0            | 78.0                       | 50.0                     |
| Aerospace                   | 58.9    | 14.1           | 48.1               | 59.0            | 54.9                       | 47.0                     |
| Automotive supplier         | 99.0    | 72.4           | 84.2               | 92.6            | 91.0                       | 87.1                     |
| Industrial drive technology | 60.0    | 31.4           | 32.7               | 59.5            | 37.3                       | 56.8                     |

## Benchmarks

Benchmarks were run using our [own harnesses](https://github.com/Aleph-Alpha-Research/eval-framework) and, where applicable, all models used the highest respective reasoning effort.

| Type  | MoE   | MoE   | MoE   | Dense |       |       |       |       |       |       |       |      |      |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ---- | ---- |
| 75.5  | 54.1  | 64.7  | 65.6  | 74.7  | 71.4  | 62.4  | 71.9  | 72.3  | 63.1  | 64.4  | 73.0  | 80.2 | –    |
| 70.8  | 46.4  | 50.4  | 59.3  | 69.8  | 67.3  | 58.0  | 66.3  | 70.2  | 61.4  | 64.8  | 67.9  | 79.9 | –    |
| 50.1  | 39.7  | 45.5  | 46.0  | 52.7  | 52.1  | 48.4  | 51.4  | 50.0  | 47.5  | 45.7  | 52.0  | 56.8 | –    |
| 57.6  | 44.9  | 46.5  | 41.2  | 61.3  | 61.0  | 55.7  | 61.5  | 58.0  | 51.4  | 52.2  | 59.5  | 69.2 | –    |
| 84.3  | 68.1  | 73.1  | 73.9  | 83.8  | 83.4  | 76.1  | 81.1  | 76.4  | 74.7  | 73.2  | 78.0  | 89.2 | 29.5 |
| 81.3  | 58.5  | 59.8  | 49.6  | 84.2  | 80.6  | 72.2  | 80.1  | 76.0  | 72.9  | 71.1  | 76.6  | 88.1 | 31.4 |
| 21.5  | 9.4   | 15.4  | 12.1  | 20.4  | 21.1  | 11.6  | 19.2  | 19.4  | 9.7   | 8.7   | 20.6  | 35.6 | 5.2  |
| 15.9  | 10.4  | 9.1   | 13.1  | 18.1  | 20.5  | 15.6  | 23.4  | 20.7  | 10.5  | 10.5  | 22.3  | 37.2 | 5.7  |
| 14.8  | 11.3  | 17.0  | 19.5  | 22.0  | 19.5  | 24.2  | 20.7  | 23.3  | 25.0  | 20.0  | 26.7  | 17.5 | 13.5 |
| −32.8 | −64.0 | −62.8 | −45.7 | −47.3 | −15.3 | −42.3 | −47.3 | −35.2 | −24.0 | −28.8 | −36.5 | −9.5 | –    |
| 80.0  | 70.1  | 76.5  | 78.3  | 84.6  | 84.3  | 81.7  | 84.5  | 80.8  | 80.4  | 80.9  | 82.7  | 85.0 | 43.0 |
| 75.5  | 65.7  | 70.7  | 61.0  | 81.7  | 81.9  | 79.4  | 81.1  | 77.2  | 70.7  | 74.9  | 79.7  | 82.4 | 37.3 |
| 96.5  | 81.7  | 88.8  | 88.8  | 90.1  | 87.8  | 86.3  | 87.4  | 90.7  | 81.4  | 82.8  | 91.1  | 97.8 | –    |
| 88.8  | 74.3  | 45.2  | 84.3  | 79.6  | 83.7  | 83.7  | 88.1  | 90.8  | 75.4  | 80.6  | 86.5  | 96.7 | –    |
| 96.9  | 81.9  | 89.4  | 89.6  | 88.1  | 84.6  | 84.2  | 87.3  | 90.6  | 79.8  | 81.9  | 91.7  | 97.9 | 0.6  |
| 87.5  | 73.5  | 43.8  | 84.4  | 76.7  | 82.9  | 80.6  | 88.1  | 90.6  | 72.3  | 80.6  | 85.6  | 96.5 | 0.2  |
| 96.0  | 81.5  | 88.3  | 87.9  | 92.1  | 91.0  | 88.5  | 87.5  | 90.8  | 83.1  | 83.8  | 90.4  | 97.7 | 0.6  |
| 90.0  | 75.2  | 46.7  | 84.2  | 82.5  | 84.4  | 86.7  | 88.1  | 91.0  | 78.5  | 80.6  | 87.5  | 96.9 | 0.0  |
| 63.4  | 41.6  | 58.9  | 46.4  | 63.4  | 62.1  | 46.3  | 54.6  | 54.0  | 40.7  | 53.5  | 54.9  | 66.7 | –    |
| 27.7  | –     | 20.2  | 9.7   | 39.7  | –     | 8.6   | –     | 29.2  | 21.0  | –     | 39.7  | 76.8 | –    |
| 94.7  | 67.5  | 95.9  | 45.9  | 97.7  | 99.1  | 43.9  | 45.3  | 73.1  | 41.5  | 53.8  | 68.1  | 82.5 | 10.8 |
| 69.9  | 58.5  | 57.9  | 64.9  | 70.8  | 71.6  | 60.8  | 71.3  | 60.5  | 62.9  | 61.4  | 67.5  | 68.7 | 9.6  |
| 76.7  | 58.7  | 68.7  | 52.7  | 76.0  | 70.7  | 65.3  | 73.3  | 72.7  | 40.0  | 70.7  | 72.7  | 83.3 | 40.0 |
| 38.1  | 5.7   | 7.2   | 5.7   | 11.3  | 10.6  | 5.4   | 16.0  | 14.7  | 5.7   | 6.4   | 15.5  | 50.0 | 2.1  |
| 39.8  | 22.8  | 58.2  | 47.9  | 54.0  | 53.5  | 51.4  | 53.4  | 45.6  | 36.2  | 61.6  | 44.6  | 42.5 | 0.6  |
| 61.4  | 36.4  | 65.4  | 61.5  | 70.5  | 67.2  | 51.0  | 68.2  | 57.3  | 58.0  | 67.2  | 61.0  | 73.2 | –    |
| 79.1  | 78.1  | 83.3  | 85.0  | 85.8  | 88.2  | 83.6  | 83.7  | 35.8  | 83.6  | 85.5  | 45.0  | 85.3 | –    |
| 78.9  | 73.7  | 78.3  | 78.8  | 80.2  | 81.4  | 82.5  | 80.2  | 70.4  | 78.4  | 78.2  | 77.6  | 79.9 | –    |
| 47.5  | 27.5  | 62.7  | 53.5  | 59.9  | 58.1  | 56.0  | 61.4  | 55.4  | 40.4  | 65.2  | 51.7  | 55.5 | –    |
| 62.8  | 19.4  | 41.5  | 39.1  | 62.6  | 53.8  | 35.3  | 52.9  | 50.7  | 39.1  | 43.4  | 59.6  | 79.6 | –    |
| 62.5  | 10.5  | 69.0  | 66.0  | 75.0  | 68.5  | 12.5  | 75.0  | 57.0  | 69.0  | 71.0  | 71.5  | 82.0 | –    |
| 29.4  | 4.4   | –     | 14.5  | 36.5  | 26.9  | 2.8   | 25.5  | 31.2  | –     | –     | 29.1  | 46.4 | –    |
| 89.3  | 68.0  | 67.8  | 81.8  | 85.0  | 87.7  | 83.6  | 89.0  | 90.8  | 82.0  | 79.8  | 88.3  | 94.2 | –    |
| 85.9  | 59.2  | 46.5  | 71.3  | 77.8  | 82.5  | 73.9  | 82.3  | 87.5  | 71.2  | 67.8  | 82.0  | 93.8 | 8.7  |
| 92.7  | 76.8  | 89.0  | 92.4  | 92.2  | 92.8  | 93.3  | 95.7  | 94.1  | 92.8  | 91.8  | 94.7  | 94.7 | 41.6 |
| 66.4  | –     | 51.0  | 38.6  | 71.6  | 73.8  | –     | 57.8  | –     | 60.8  | 11.6  | 60.2  | 72.6 | –    |
| 78.1  | 62.5  | 64.5  | 73.2  | 72.7  | 66.1  | 60.7  | 79.9  | 71.1  | 49.8  | 38.2  | 73.7  | 81.9 | –    |
| 78.1  | 62.5  | 64.5  | 73.2  | 72.7  | 66.1  | 60.7  | 79.9  | 71.1  | 49.8  | 38.2  | 73.7  | 81.9 | 25.5 |

## Get Started

Our model is available openly on [Hugging Face](https://huggingface.co/Aleph-Alpha/Kolibri-1) under an Apache 2.0 License.

Kolibri requires the **aleph-alpha-inference** package that provides the Kolibri vLLM plugin. You can either use the provided container image `ghcr.io/aleph-alpha/aleph-alpha-inference`, or install the package from [Aleph-Alpha/aleph-alpha-inference](https://github.com/Aleph-Alpha/aleph-alpha-inference), which also installs the vLLM version it supports:

```bash
pip install "aleph-alpha-inference>=1.0"
```

Serve the model with reasoning and tool-calling enabled.

```bash
vllm serve Aleph-Alpha/Kolibri-1 --kv-cache-dtype fp8 \
  --reasoning-parser kolibri1 \
  --tool-call-parser kolibri1 \
  --enable-auto-tool-choice
```

To serve contexts beyond 262,144 tokens, add `--max-model-len 1048576 --hf-overrides '{"max_position_embeddings": 1048576}'`. The recommended sampling parameters for the model are `temperature=1.0`, `top_p=0.97` and `top_k=128`.

## Contact for Deployment and Specialization

Contact our team who will be happy to support you through our enterprise deployment and specialization options: [contact sales](https://aleph-alpha.com/en/sales/).
