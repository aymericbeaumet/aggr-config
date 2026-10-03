---
title: 'Show HN: Germany''s new sovereign AI model Kolibri'
link: https://tej.as/blog/aleph-alpha-kolibri
source: hnrss-org
published: 2026-10-03T10:43:51Z
updated: 2026-10-03T10:43:51Z
first_seen: 2026-10-03T15:15:59.697027369Z
authors:
- tejaskumar__
content: extracted
html: 2026-10-03-show-hn-germany-s-new-sovereign-ai-model-kolibri.html
preview:
  file: 2026-10-03-show-hn-germany-s-new-sovereign-ai-model-kolibri.preview-c851549801a3.webp
  width: 256
  height: 134
  alt: 'Aleph Alpha Kolibri: How the Sovereign German LLM Works. An article by Tejas Kumar.'
  color: '#373434'
images:
- source: https://tej.as/img/og/blog/aleph-alpha-kolibri.png
  original:
    file: 2026-10-03-show-hn-germany-s-new-sovereign-ai-model-kolibri.image-51ad905563a6.png
    width: 1200
    height: 630
  color: '#110e0f'
---

**Kolibri is an open-weight large language model (LLM) from [Aleph Alpha](https://aleph-alpha.com/) for German and English: a mixture of experts with 78 billion parameters that only uses about 3.5 billion of them for each token it reads or writes.** It came out on 3 October 2026 under the [Apache 2.0 license](https://www.apache.org/licenses/LICENSE-2.0), the weights are [on Hugging Face](https://huggingface.co/Aleph-Alpha/Kolibri-1), and it was trained from scratch on infrastructure in Germany and Finland. (Kolibri is German for hummingbird, which is cute for a model whose whole trick is being light.)

I live in Germany, and at [SmashingConf New York](https://tej.as/talks/smashingconf-new-york-2024) in 2024 I told the room what I’d heard in the US when I said where I’m based: “you regulate, you don’t innovate.” It hurt to hear, and what I wished for on that stage was the middle, “the right balance between innovation and regulation around data privacy, data stewardship, environmental constraints and energy requirements.” Kolibri is a pretty direct answer to that: a German team built it with the [EU AI Act](https://artificialintelligenceact.eu/) in mind “from the ground up”, and in Aleph Alpha’s own evaluation it scores above every compared model of its size in both languages.

Huge congrats to everyone at Aleph Alpha who built it, my good friend [Michael Hofmann](https://www.linkedin.com/in/michaellhofmann/) among them!

This post is about how Kolibri works, where it’s strong, where it isn’t, how to run it, and when it’s the right pick. Everything here comes from Aleph Alpha’s [189 page technical report](https://aleph-alpha.com/downloads/tech-report.pdf), [the model card](https://huggingface.co/Aleph-Alpha/Kolibri-1) and [their launch post](https://aleph-alpha.com/en/blog/kolibri-has-landed-a-sovereign-open-weight-model/), plus one experiment I ran on its tokenizer.

## What is Kolibri?

|                  | Kolibri 1                                                                                                                                                              |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Parameters       | 78.1 billion in total, 3.46 billion per token (4.4%)                                                                                                                   |
| Languages        | German and English                                                                                                                                                     |
| Context          | 262,144 tokens natively, tested up to 1,048,576                                                                                                                        |
| License          | Apache 2.0 for the weights and configuration files (Aleph Alpha keeps the rights to its training code and methods)                                                     |
| Memory           | about 78 GB of weights in 8-bit floating point (FP8)                                                                                                                   |
| Reasoning        | 4 levels: none, low, medium and high                                                                                                                                   |
| Tool calling     | Yes                                                                                                                                                                    |
| Knowledge cutoff | 18 June 2026                                                                                                                                                           |
| Training         | about 24 trillion tokens, more than a fifth of them German, on 768 [NVIDIA B200](https://www.nvidia.com/en-us/data-center/dgx-b200/) graphics processing units (GPUs) |

**Aleph Alpha calls Kolibri sovereign, and in their launch post that means 2 things.** The first is how it was built: “teams built the model in Germany, trained it on infrastructure in Germany and Finland, under European and German law, with no foreign control.” The second is what customers get: “full freedom of deployment and intellectual-property safety, so compliance comes as an inherited property.” In plain words, a ministry or a car supplier can run it on its own servers, with its data never leaving the building, and nobody can change or switch off the model under them. Aleph Alpha has also signed the [European Union](https://european-union.europa.eu/)’s [General-Purpose AI (GPAI) Code of Practice](https://digital-strategy.ec.europa.eu/en/policies/contents-code-gpai).

Sovereign doesn’t mean that nothing from outside Europe went in, and the model card says so itself: English web text was rephrased with Google’s [Gemma 4](https://huggingface.co/google/gemma-4-26b-a4b-it), German with [Mistral-NeMo](https://huggingface.co/mistralai/Mistral-Nemo-Instruct-2407), and [Qwen3-32B](https://huggingface.co/Qwen/Qwen3-32B) labeled data for the quality filters. They then filtered the training data for the political bias such models can have, which they’ve [measured in Chinese open models](https://aleph-alpha.com/en/blog/training-on-the-party-line/) themselves.

## How Kolibri works

Kolibri is 6 ideas stacked on top of each other, and each one is there to make German cheaper, longer or more honest.

### 1\. 384 specialists, and each token sees 6

In a normal (dense) model, every token goes through every parameter. In a [mixture of experts (MoE)](https://huggingface.co/blog/moe), each layer has a crowd of small sub-networks called experts and a router that picks a few of them for each token. Kolibri has 50 layers, each with 384 experts plus 1 shared expert that every token goes through, and its router sends each token to 6 of the 384. That’s how 78.1 billion parameters turn into 3.46 billion of actual work per token.

In 2024 I gave a talk called [Why Small Language Models are the future](https://www.youtube.com/watch?v=uNj3y2pItrA&t=72s), and I argued for “smaller language models with fewer parameters and fewer places things can go wrong that require lesser compute.” My analogy was a doctor who has read every medical book in the world against a specialist in hematology: go to the first one with a blood condition and “they may not get it right cuz they know too much.” A mixture of experts puts a hospital full of specialists inside one model, and the router is the receptionist who sends each token to the right 6.

The analogy breaks in 2 places though. The experts aren’t neat topics like “German law”: when researchers [look inside MoE models](https://huggingface.co/blog/moe#what-does-an-expert-learn) they mostly find experts for patterns of tokens, like punctuation or proper nouns, not subjects a person would pick. And the hospital has to keep all 384 specialists on staff even if you only see 6, so **Kolibri computes like a 3.5 billion parameter model but needs the memory of a 78 billion parameter one.** The model card says it plainly: “the full model must be held in memory even though only part of it is active at any time.”

### 2\. A tokenizer that reads long German words

A model doesn’t read letters or words, it reads tokens: chunks of text from a fixed vocabulary, picked when the tokenizer is trained. German glues words together into long compound words, and a tokenizer that learned mostly from English chops them into pieces. Here’s the German name of the [Federal Constitutional Court](https://www.bundesverfassungsgericht.de/EN/), split by the tokenizer GPT-4o and [GPT-5](https://en.wikipedia.org/wiki/GPT-5) use (`o200k_base`, through OpenAI’s [tiktoken](https://github.com/openai/tiktoken)), and by Kolibri’s:

```text
o200k_base (GPT-5):  Bund | es | ver | fass | ungs | gericht     6 tokens
Kolibri:             Bundes | verfassungsgericht                 2 tokens
```

Kolibri’s tokenizer has 128,000 tokens, trained with a new algorithm Aleph Alpha calls UniBPE: it keeps the bottom-up merging of [byte-pair encoding (BPE)](https://en.wikipedia.org/wiki/Byte-pair_encoding) and picks each merge with a different scoring rule (the Unigram objective), which respects how German builds words. The report says it needs 11.2% fewer tokens for German text than GPT-5’s tokenizer, the best of the 9 others they measured.

I wanted to see that for myself, so I ran 6 tokenizers over all of the [Basic Law for the Federal Republic of Germany](https://www.gesetze-im-internet.de/gg/), the German constitution (185 KB of very German legal text), and over its [official English translation](https://www.gesetze-im-internet.de/englisch_gg/):

| Tokenizer                                                                            | German tokens | More than Kolibri | English tokens | More than Kolibri |
| ------------------------------------------------------------------------------------ | ------------- | ----------------- | -------------- | ----------------- |
| Kolibri 1                                                                            | 35,190        |                   | 39,875         |                   |
| `o200k_base` (GPT-4o, GPT-5, [GPT-OSS](https://huggingface.co/openai/gpt-oss-120b)) | 41,482        | 17.9%             | 39,737         | -0.3%             |
| [Qwen3.5 35B-A3B](https://huggingface.co/Qwen/Qwen3.5-35B-A3B)                      | 42,907        | 21.9%             | 41,650         | 4.5%              |
| [Mistral Small 4](https://huggingface.co/mistralai/Mistral-Small-4-119B-2603)       | 43,478        | 23.6%             | 41,301         | 3.6%              |
| Gemma 4                                                                              | 43,850        | 24.6%             | 41,564         | 4.2%              |

On legal German, Kolibri needed 15% fewer tokens than GPT-5’s tokenizer, even more than Aleph Alpha’s own 11.2%, and in English it tied with it. Wild. Fewer tokens means fewer steps to read or write the same German text, and more German fits in the same context window. (I counted each one with its own `tokenizer.json` through Hugging Face’s `tokenizers` library, except `o200k_base`, which I counted with tiktoken.)

### 3\. Most layers only look nearby

40 of Kolibri’s 50 layers use sliding-window attention: each token only looks at the 512 tokens before it. Every 5th layer looks at everything before it. It’s like reading a long contract while mostly paying attention to the sentence you’re on, and every few pages stopping to think about all of it, and it’s what keeps a 1 million token context affordable.

There’s a clever detail in there too. Only the sliding-window layers know where a token sits (through [rotary position embeddings](https://arxiv.org/abs/2104.09864)), and the full-attention layers don’t, so the context stretches past the 262,144 tokens it was trained on without any extra position tricks. Aleph Alpha validated it up to 1,048,576.

In my talk [Unlocking Value with AI Today](https://www.youtube.com/watch?v=uMksIqpbVoY&t=364s) I called finite context one of “the big three” problems of generative AI, next to hallucination and the knowledge cutoff, and Kolibri goes after all 3. At 1 million tokens, on the [RULER](https://arxiv.org/abs/2404.06654) long-context test, Kolibri’s base model scores 63.2, against 57.5 for Qwen3.5 35B-A3B’s base model.

### 4\. It thinks in German

Reasoning models think before they answer, and even on German prompts they mostly think in English. Aleph Alpha [posted about this](https://x.com/Aleph__Alpha/status/2103139585039511700) on 24 September and wrote it up as [Through the Valley of Tears](https://aleph-alpha.com/en/blog/through-the-valley-of-tears-cold-starting-german-reasoning-in-llms/): they generated about 800,000 German reasoning examples, and found that a little German reasoning data is worse than none. Their model’s German math score dropped from 70.2 to 48.3, because its German thoughts kept going around in circles and never finished, and it only climbed back (to 67.3) with a lot more German data.

Kolibri got the lot more. It reasons in German on German prompts, and its German math scores are the best of the models with about 3 billion active parameters: 87.5 on the [American Invitational Mathematics Examination (AIME)](https://en.wikipedia.org/wiki/American_Invitational_Mathematics_Examination) 2025 in German, against 84.4 for the next best, NVIDIA’s [Nemotron 3 Nano](https://developer.nvidia.com/nemotron).

### 5\. It’s trained to say “I don’t know”

Hallucination was number 1 of my big three, and the fix in that talk was retrieval augmented generation (RAG): you look up good, authoritative information and put it in the prompt. RAG only works if the model admits when the documents don’t have the answer, though, and that’s what Aleph Alpha trained for, with their own method, the [Merlin-Arthur protocol](https://arxiv.org/abs/2512.11614).

It works like a game with 3 players. Arthur is the model, and he gets a question with parts of the supporting document hidden. Merlin hides parts so that the correct answer gets easier to find, and Arthur is trained to answer those. Morgana hides the evidence the answer depends on, and Arthur is trained to say he doesn’t know. Arthur never knows which of the 2 he’s facing, so the only way to win is to actually check whether the evidence in front of him supports an answer.

It shows. On [Artificial Analysis’s Omniscience test](https://artificialanalysis.ai/evaluations/omniscience), when Kolibri didn’t know an answer, it said so (or gave a partial answer) 44% of the time instead of making one up. Qwen3.5 35B-A3B did that 11.1% of the time and GPT-OSS 120B 23.7%. Of all the mixture-of-experts models Aleph Alpha compared, only [Qwen3.6 35B-A3B](https://huggingface.co/Qwen/Qwen3.6-35B-A3B) did better, at 56.7%.

### 6\. You pick how hard it thinks

Every request can set `reasoning_effort` to `none`, `low`, `medium` or `high`, so a quick lookup answers right away and a hard question gets a long think, from the same model on the same server.

## What Kolibri is good at

These are the rows where Kolibri leads the open models of its size in Aleph Alpha’s evaluation, which runs every model through the same setup with the sampling settings its makers recommend:

| What                                             | Kolibri | Closest model with about 3B active parameters |
| ------------------------------------------------ | ------- | --------------------------------------------- |
| Overall score, English                           | 75.5    | 74.7 (Qwen3.5 35B-A3B)                        |
| Overall score, German                            | 70.8    | 69.8 (Qwen3.5 35B-A3B)                        |
| AIME 2025, English                               | 96.9    | 89.6 (Nemotron 3 Nano)                        |
| AIME 2025, German                                | 87.5    | 84.4 (Nemotron 3 Nano)                        |
| Questions over unseen company documents, English | 89.7    | 87.0 (Qwen3.5 35B-A3B)                        |
| Long context at 1 million tokens (base model)    | 63.2    | 58.5 (Nemotron 3 Nano)                        |

The math is the standout: on AIME 2025 and 2026 in English it beats every MoE model in the comparison, including the ones with 12 billion active parameters, and only the dense Qwen3.8 27B scores higher. The company-documents row is 5 tests Aleph Alpha built from customer-like work in semiconductors, the German public sector, aerospace, an automotive supplier and industrial drives, run over documents and questions the model never saw in training.

## What Kolibri is bad at

Aleph Alpha publishes its weakest rows right next to its best ones in the model card, so here they are:

- **It knows less from memory.** It’s last of the 12 models on a closed-book test (the [Retrieval-Augmented Generation Benchmark](https://arxiv.org/abs/2309.01431)’s questions with no documents, 51.0), and it answers only 14.8% of the Omniscience questions correctly, against 22.2% for Qwen3.5 35B-A3B. It knows when it doesn’t know, which is great, but it also knows less: fine with documents in the prompt, bad as a trivia oracle.
- **Multi-turn tool calling is weaker.** On the [Berkeley Function Calling Leaderboard](https://gorilla.cs.berkeley.edu/leaderboard.html)’s multi-turn tests it scores 39.8, against 58.2 for [GLM-4.7 Flash](https://huggingface.co/zai-org/GLM-4.7-Flash) and 54.0 for Qwen3.5 35B-A3B.
- **It’s not the best coding agent.** It scores 27.7 on [Terminal-Bench](https://www.tbench.ai/) 2.1 against 39.7 for Qwen3.5 35B-A3B, and 66.4 on [SWE-bench Verified](https://www.swebench.com/) against 73.8 for Qwen3.6 35B-A3B.
- **Long context dips in the middle.** At 128,000 tokens on RULER, Qwen3.5’s base model scores 89.9 to Kolibri’s 67.9. Kolibri only pulls ahead at the very long end.
- **The memory.** 78 GB of weights means a data-center GPU or two, never a laptop, whatever the 3.5 billion suggests.
- **The plumbing is new.** It needs Aleph Alpha’s [vLLM plugin](https://github.com/Aleph-Alpha/aleph-alpha-inference), which supports one vLLM version at a time (0.29 today), and on launch day no hosted provider serves it yet.
- **2 languages only.** That’s on purpose: the model card calls it “a deliberate choice of depth over breadth.”
- **A bigger dense model beats it.** [Qwen3.8 27B](https://huggingface.co/Qwen/Qwen3.8-27B) scores 80.2 in English and 79.9 in German, but it uses nearly 8 times as many parameters for every token, and that’s the trade Kolibri is built around.

## How to run Kolibri

You need about 78 GB of GPU memory: 2 NVIDIA A100s or [H100s](https://www.nvidia.com/en-us/data-center/h100/) with 80 GB each at the least, or a single [H200](https://www.nvidia.com/en-us/data-center/h200/), B200 or B300. I haven’t run the model itself yet, since it needs a data-center GPU and nobody hosts it so far, so this is straight from the model card. First the plugin, which installs the [vLLM](https://docs.vllm.ai/) version it supports:

```bash
pip install 'aleph-alpha-inference>=1'

vllm serve Aleph-Alpha/Kolibri-1 --kv-cache-dtype fp8 \
  --reasoning-parser kolibri1 \
  --tool-call-parser kolibri1 \
  --enable-auto-tool-choice
```

That gives you an OpenAI-compatible server, so any OpenAI client talks to it, and the reasoning effort goes through the chat template:

```python
# from the Kolibri model card (trimmed)
from openai import OpenAI

client = OpenAI(base_url="http://localhost:8000/v1", api_key="EMPTY")
response = client.chat.completions.create(
    model="Aleph-Alpha/Kolibri-1",
    messages=[{"role": "user", "content": "Erkläre kurz, was ein Mixture-of-Experts-Modell ist."}],
    extra_body={"chat_template_kwargs": {"reasoning_effort": "high", "enable_thinking": True}},
)
print(response.choices[0].message.content)
```

The model card recommends `temperature=1.0`, `top_p=0.97` and `top_k=128`, and contexts of at most 262,144 tokens for anything latency-sensitive. For the full million, serve it with 2 more flags:

```bash
vllm serve Aleph-Alpha/Kolibri-1 --kv-cache-dtype fp8 \
  --max-model-len 1048576 \
  --hf-overrides '{"max_position_embeddings": 1048576}'
```

## When to use Kolibri

**Kolibri is the pick when German text and your own hardware both matter:** a public authority, a bank, a manufacturer or an aerospace supplier that has to keep its documents in house, wants answers in German that reason in German, and would rather hear “I don’t know” than a confident wrong answer. RAG over long German documents (laws, contracts, manuals) plays to every strength above: the tokenizer, the 1 million token context and the abstention.

That’s the kind of project I’m working on right now, with [Prof. Dr. Heinrich Audebert](https://schlaganfallcentrum.charite.de/en/research/research_groups/audebert), who heads [neurology at Campus Benjamin Franklin](https://neurologie.charite.de/en/), one of the [Charité](https://www.charite.de/en/)’s hospitals in Berlin. Today, a patient with a neurological complaint goes to their general practitioner (GP), and the GP has to see them, which is very demanding for a busy practice. In what we’re building, the patient sits down at a computer in the GP’s practice, an AI avatar takes them through a battery of tests, and it grades how urgent their symptoms are: a referral to a specialist right away, or they can wait a bit. The conversations are in German, they’re about people’s health, and the model has to run where we control it, so we’re thinking of using Kolibri for it.

It’s the wrong pick for a coding agent, where Qwen3.6 35B-A3B leads, for questions the model has to answer from memory, for any language besides German and English, and for anyone who can’t spare 78 GB of GPU memory. For that last case, the small specialized model I argued for in 2024 is still the answer: for my podcast search I [fine-tuned Mistral 7B on my Apple silicon laptop](https://www.youtube.com/watch?v=uNj3y2pItrA&t=231s) instead of paying for GPT-4o.

Which is better for your own documents, a hospital of specialists like Kolibri or one small model trained for a single job, only shows when you try both on your data. Swapping the model under an agent without breaking it is what [my workshop on reliable AI agents](https://tej.as/workshops#ai-agents) covers: guardrails and retries that work whichever model sits underneath.
