---
title: Open challenges in LLM research
link: https://huyenchip.com//2023/08/16/llm-research-open-challenges.html
source: huyenchip-com
published: 2023-08-16T00:00:00Z
updated: 2023-08-16T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: '[LinkedIn discussion, Twitter thread] Never before in my life had I seen so many smart people working on the same goal: making LLMs better. After talking to many people working in both industry and academia, I noticed the 10 major research directions that emerged. The first two directions, hallucinations and context learning, are probably the most talked about today. I’m the most excited about numbers 3 (multimodality), 5 (new architecture), and 6 (GPU alternatives). 1. Reduce and measure hallucinations Hallucination is a heavily discussed topic already so I’ll be quick. Hallucination happens when an AI model makes stuff up. For many creative use cases, hallucination is a feature. However, for most other use cases, hallucination is a bug. I was at a panel on LLM with Dropbox, Langchain, Elastics, and Anthropic recently, and the #1 roadblock they see for companies to adopt LLMs in production is hallucination. Mitigating hallucination and developing metrics to measure hallucination is a blossoming research topic, and I’ve seen many startups focus on this problem. There are also ad-hoc tips to reduce hallucination, such as adding more context to the prompt, chain-of-thought, self-consistency, or asking your model to be concise in its response. To learn more about hallucination: Survey of Hallucination in Natural Language Generation (Ji et al., 2022) How Language Model Hallucinations Can Snowball (Zhang et al., 2023) A Multitask, Multilingual, Multimodal Evaluation of ChatGPT on Reasoning, Hallucination, and Interactivity (Bang et al., 2023) Contrastive Learning Reduces Hallucination in Conversations (Sun et al., 2022) Self-Consistency Improves Chain of Thought Reasoning in Language Models (Wang et al., 2022) SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models (​​Manakul et al., 2023) A simple example of fact-checking and hallucination by NVIDIA’s NeMo-Guardrails 2. Optimize context length and context construction A vast majority of questions require context. For example, if we ask ChatGPT: “What’s the best Vietnamese restaurant?”, the context needed would be “where” because the best Vietnamese restaurant in Vietnam would be different from the best Vietnamese in the US. According to this cool paper SituatedQA (Zhang & Choi, 2021), a significant proportion of information-seeking questions have context-dependent answers, e.g. roughly 16.5% of the Natural Questions NQ-Open dataset. Personally, I suspect that this percentage would be even higher for enterprise use cases. For example, say a company builds a chatbot for customer support, for this chatbot to answer any customer question about any product, the context needed might be that customer’s history or that product’s information. Because the model “learns” from the context provided to it, this process is also called context learning. Context length is especially important for RAG – Retrieval Augmented Generation (Lewis et al., 2020) – which has emerged to be the predominant pattern for LLM industry use cases. For those not yet swept away in the RAG rage, RAG works in two phases: Phase 1: chunking (also known as indexing) Gather all the documents you want your LLM to use Divide these documents into chunks that can be fed into your LLM to generate embeddings and store these embeddings in a vector database. Phase 2: querying When user sends a query, like “Does my insurance policy pay for this drug X”, your LLM converts this query into an embedding, let’s call it QUERY_EMBEDDING Your vector database fetches the chunks whose embeddings are the most similar to QUERY_EMBEDDING Screenshot from Jerry Liu’s talk on LlamaIndex (2023) The longer the context length, the more chunks we can squeeze into the context. The more information the model has access to, the better its response will be, right? Not always. How much context a model can use and how efficiently that model will use it are two different questions. In parallel with the effort to increase model context length is the effort to make the context more efficient. Some people call it “prompt engineering” or “prompt construction”. For example, a paper that has made the rounds recently is about how models are much better at understanding information at the beginning and the end of the index rather than in the middle of it – Lost in the Middle: How Language Models Use Long Contexts (Liu et al., 2023). 3. Incorporate other data modalities Multimodality, IMO, is so powerful and yet so underrated. There are many reasons for multimodality. First, there are many use cases where multimodal data is required, especially in industries that deal with a mixture of data modalities such as healthcare, robotics, e-commerce, retail, gaming, entertainment, etc. Examples: Oftentimes, medical predictions require both text (e.g. doctor’s notes, patients’ questionnaires) and images (e.g. CT, X-ray, MRI scans). Product metadata often contains images, videos, descriptions, and even tabular data (e.g. production date, weight, color). You might want to automatically fill in missing product information based on users’ reviews or product photos. You might want to enable users to search for products using visual information, like shape or color. Second, multimodality promises a big boost in model performance. Shouldn’t a model that can understand both text and images perform better than a model that can only understand text? Text-based models require so much text that there’s a realistic concern that we’ll soon run out of Internet data to train text-based models. Once we run out of text, we’d need to leverage other data modalities. Flamingo architecture (Alayrac et al., 2022) One use case I’m especially excited about is that multimodality can enable visually impaired people to browse the Internet and navigate the real world. Cool multimodal work: [CLIP] Learning Transferable Visual Models From Natural Language Supervision (OpenAI, 2021) Flamingo: a Visual Language Model for Few-Shot Learning (DeepMind, 2022) BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models (Salesforce, 2023) KOSMOS-1: Language Is Not All You Need: Aligning Perception with Language Models (Microsoft, 2023) PaLM-E: An embodied multimodal language model (Google, 2023) LLaVA: Visual Instruction Tuning (Liu et al., 2023) NeVA: NeMo Vision and Language Assistant (NVIDIA, 2023) I’ve been working on a post on multimodality that hopefully I can share soon! 4. Make LLMs faster and cheaper When GPT-3.5 first came out in late November 2022, many people had concerns about latency and cost of using it in production. However, latency/cost analysis has changed rapidly since then. Within half a year, the community found a way to create a model that came pretty close to GPT-3.5 in terms of performance, yet required just under 2% of GPT-3.5’s memory footprint. My takeaway: if you create something good enough, people will figure out a way to make it fast and cheap. Date Model # params Quantization Memory to finetune Can be trained on Nov 2022 GPT-3.5 175B 16-bit 375GB Many, many machines Mar 2023 Alpaca 7B 7B 16-bit 15GB Gaming desktop May 2023 Guanaco 7B 7B 4-bit 6GB Any Macbook Below is Guanaco 7B’s performance compared to ChatGPT GPT-3.5 and GPT-4, as reported in the Guanco paper. Caveat: in general, the performance comparison is far from perfect. LLM evaluation is very, very hard. Four years ago, when I started working on the notes that would later become the section Model Compression for the book Designing Machine Learning Systems, I wrote about four major techniques for model optimization/compression: Quantization: by far the most general model optimization method. Quantization reduces a model’s size by using fewer bits to represent its parameters, e.g. instead of using 32 bits to represent a float, use only 16 bits, or even 4 bits. Knowledge distillation: a method in which a small model (student) is trained to mimic a larger model or ensemble of models (teacher). Low-rank factorization: the key idea here is to replace high-dimensional tensors with lower-dimensional tensors to reduce the number of parameters. For example, you can decompose a 3x3 tensor into the product of a 3x1 and a 1x3 tensor, so that instead of having 9 parameters, you have only 6 parameters. Pruning All these four techniques are still relevant and popular today. Alpaca was trained using knowledge distillation. QLoRA used a combination of low-rank factorization and quantization. 5. Design a new model architecture Since AlexNet in 2012, we’ve seen many architectures go in and out of fashion, including LSTM, seq2seq. Compared to those, Transformer is incredibly sticky. It’s been around since 2017. It’s a big question mark how much longer this architecture will be in vogue. Developing a new architecture to outperform Transformer isn’t easy. Transformer has been so heavily optimized over the last 6 years. This new architecture has to be performing at the scale that people care about today, on the hardware that people care about. Side note: Transformer was originally designed by Google to run fast on TPUs, and only later optimized on GPUs. There was a lot of excitement in 2021 around S4 from Chris Ré’s lab – see Efficiently Modeling Long Sequences with Structured State Spaces (Gu et al., 2021). I’m not quite sure what happened to it. Chris Ré’s lab is still very invested in developing new architecture, most recently with their architecture Monarch Mixer (Fu et al., 2023) in collaboration with the startup Together. Their key idea is that for the existing Transformer architecture, the complexity of attention is quadratic in sequence length and the complexity of an MLP is quadratic in model dimension. An architecture with subquadratic complexity would be more efficient. I’m sure many other labs are working on this idea, though I’m not aware of any attempt that has been made public. If you know of any, please let me know! 6. Develop GPU alternatives GPU has been the dominating hardware for deep learning ever since AlexNet in 2012. In fact, one commonly acknowledged reason for AlexNet’s popularity is that it was the first paper to successfully use GPUs to train neural networks. Before GPUs, if you wanted to train a model at AlexNet’s scale, you’d have to use thousands of CPUs, like the one Google released just a few months before AlexNet. Compared to thousands of CPUs, a couple of GPUs were a lot more accessible to Ph.D. students and researchers, setting off the deep learning research boom. In the last decade, many, many companies, both big corporations, and startups, have attempted to create new hardware for AI. The most notable attempts are Google’s TPUs, Graphcore’s IPUs (what’s happening with IPUs?), and Cerebras. SambaNova raised over a billion dollars to develop new AI chips but seems to have pivoted to being a generative AI platform. For a while, there has been a lot of anticipation around quantum computing, with key players being: IBM’s QPU Google’s Quantum computer reported a major milestone in quantum error reduction earlier this year in Nature. Its quantum virtual machine is publicly accessible via Google Colab Research labs such as MIT Center for Quantum Engineering, Max Planck Institute of Quantum Optics, Chicago Quantum Exchange, Oak Ridge National Laboratory, etc. Another direction that is also super exciting is photonic chips. This is the direciton I know the least about – so please correct me if I’m wrong. Existing chips today use electricity to move data, which consumes a lot of power and also incurs latency. Photonic chips use photons to move data, harnessing the speed of light for faster and more efficient compute. Various startups in this space have raised hundreds of millions of dollars, including Lightmatter ($270M), Ayar Labs ($220M), Lightelligence ($200M+), and Luminous Computing ($115M). Below is the timeline of advances of the three major methods in photonic matrix computation, from the paper Photonic matrix multiplication lights up photonic accelerator and beyond (Zhou et al., Nature 2022). The three different methods are plane light conversion (PLC), Mach–Zehnder interferometer (MZI), and wavelength division multiplexing (WDM). 7. Make agents usable Agents are LLMs that can take actions, like browsing the Internet, sending emails, making reservations, etc. Compared to other research directions in this post, this might be the youngest direction. Because of the novelty and the massive potential, there’s a feverish obsession with agents. Auto-GPT is now the 25th most popular GitHub repo ever by the number of stars. GPT-Engineering is another popular repo. Despite the excitement, there is still doubt about whether LLMs are reliable and performant enough to be entrusted with the power to act. One use case that has emerged though is the use of agents for social studies, like the famous Stanford experiment that shows that a small society of generative agents produces emergent social behaviors: for example, starting with only a single user-specified notion that one agent wants to throw a Valentine’s Day party, the agents autonomously spread invitations to the party over the next two days, make new acquaintances, ask each other out on dates to the party … (Generative Agents: Interactive Simulacra of Human Behavior, Park et al., 2023) The most notable startup in this area is perhaps Adept, founded by two Transformer co-authors (though both already left) and an ex-OpenAI VP, and has raised almost half a billion dollars to date. Last year, they had a demo showing their agent browsing the Internet and adding a new account to Salesforce. I’m looking forward to seeing their new demos 🙂 8. Improve learning from human preference RLHF, Reinforcement Learning from Human Preference, is cool but kinda hacky. I wouldn’t be surprised if people figure out a better way to train LLMs. There are many open questions for RLHF, such as: 1. How to mathematically represent human preference? Currently, human preference is determined by comparison: human labeler determines if response A is better than response B. However, it doesn’t take into account how much better response A is than response B. 2. What’s human preference? Anthropic measured the quality of their model’s responses along the three axes: helpful, honest, and harmless. See Constitutional AI: Harmlessness from AI Feedback (Bai et al., 2022). DeepMind tries to generate responses that please the most people. See Fine-tuning language models to find agreement among humans with diverse preferences, (Bakker et al., 2022). Also, do we want AIs that can take a stand or a vanilla AI that shies away from any potentially controversial topic? 3. Whose preference is “human” preference, taking into account the differences in cultures, religions, political leanings, etc.? There are a lot of challenges in obtaining training data that can be sufficiently representative of all the potential users. For example, for OpenAI’s InstructGPT data, there was no labeler above 65 years old. Labelers are predominantly Filipino and Bangladeshi. See InstructGPT: Training language models to follow instructions with human feedback (Ouyang et al., 2022). Community-led efforts, while admirable in their intention, can lead to biased data. For example, for the OpenAssistant dataset, 201 out of 222 (90.5%) respondents identify as male. Jeremy Howard has a great Twitter thread on this. 9. Improve the efficiency of the chat interface Ever since ChatGPT, there have been multiple discussions on whether chat is a suitable interface for a wide range of tasks. Natural language is the lazy user interface (Austin Z. Henley, 2023) Why Chatbots Are Not the Future (Amelia Wattenberger, 2023) What Types of Questions Require Conversation to Answer? A Case Study of AskReddit Questions (Huang et al., 2023) AI chat interfaces could become the primary user interface to read documentation (Tom Johnson, 2023) Interacting with LLMs with Minimal Chat (Eugene Yan, 2023) However, this is not a new discussion. In many countries, especially in Asia, chat has been used as the interface for super apps for about a decade. Dan Grover had this discussion back in 2014. Chat as a universal interface for Chinese apps (Dan Grover, 2014) The discussion again got tense in 2016, when many people thought apps were dead and chatbots would be the future. On chat as interface (Alistair Croll, 2016) Is the Chatbot Trend One Big Misunderstanding? (Will Knight, 2016) Bots won’t replace apps. Better apps will replace apps (Dan Grover, 2016) Personally, I love the chat interface because of the following reasons: Chat is an interface that everyone, even people without previous exposure to computers or the Internet, can learn to use quickly. When I volunteered at a low-income residential neighborhood (are we allowed to say slum?) in Kenya in the early 2010s, I was blown away by how comfortable everyone there was with doing banking on their phone, via texts. No one in that neighborhood had a computer. Chat interface is accessible. You can use voice instead of text if your hands are busy. Chat is also an incredibly robust interface – you can give it any request and it’ll give back a response, even if the response isn’t good. However, there are certain areas that I think the chat interface can be improved upon. Multiple messages per turn Currently, we pretty much assume one message per turn. This is not how my friends and I text. Often, I need multiple messages to complete my thought, because I need to insert different data (e.g. images, locations, links), I forgot something in the previous messages, or I just don’t feel like putting everything into a massive paragraph. Multimodal input In the realm of multimodal applications, most energy is spent on building better models, and very little on building better interfaces. Take Nvidia’s NeVA chatbot. I’m not a UX expert, but I suspect there might be room for UX improvement here. P.S. Sorry the NeVA team for calling you out. Even with this interface, your work is super cool! Incorporating generative AI into your workflows Linus Lee covered this point well in his talk Generative AI interface beyond chats. For example, if you want to ask a question about a column of a chart you’re working on, you should be able just point to that column and ask a question. Editing and deletion of messages How would editing or deletion of a user input change the conversation flow with the chatbot? 10. Build LLMs for non-English languages We know that current English-first LLMs don’t work well for many other languages, both in terms of performance, latency, and speed. See: ChatGPT Beyond English: Towards a Comprehensive Evaluation of Large Language Models in Multilingual Learning (Lai et al., 2023) All languages are NOT created (tokenized) equal (Yennie Jun, 2023) Here are some initiatives that I’m aware of. If you have pointers to others, I’d be happy to include them here. Aya: An Open Science Initiative to Accelerate Multilingual AI Progress Symato: Vietnamese ChatGPT Cabrita: Finetuning InstructLLaMA with portuguese data Luotuo-Chinese-LLM Chinese-LLaMA-Alpaca Chinese-Vicuna Several early readers of this post told me they don’t think I should include this direction for two reasons. This is less of a research problem and more of a logistics problem. We already know how to do it. Someone just needs to put money and effort into it. This is not entirely true. Most languages are considered low-resource, e.g. they have far fewer high-quality data compared to English or Chinese, and might require different techniques to train a large language model. See: Low-resource Languages: A Review of Past Work and Future Challenges (Magueresse et al., 2020) JW300: A Wide-Coverage Parallel Corpus for Low-Resource Languages (Agić et al., 2019) Those more pessimistic think that in the future, many languages will die out, and the Internet will consist of two universes in two languages: English and Mandarin. This school of thought isn’t new – anyone remembers Esperando? The impact of AI tools, e.g. machine translation and chatbots, on language learning is still unclear. Will they help people learn new languages faster, or will they eliminate the need of learning new languages altogether? Conclusion Phew, that was a lot of papers to reference, and I have no doubt that I still missed a ton. If there’s something you think I missed, please let me know. For another perspective, check out this comprehsive paper Challenges and Applications of Large Language Models (Kaddour et al., 2023). Some of the problems mentioned above are harder than others. For example, I think that number 10, building LLMs for non-English languages, is more straightforward with enough time and resources. Number 1, reducing hallucination, will be much harder, since hallucination is just LLMs doing their probabilistic thing. Number 4, making LLMs faster and cheaper, will never be completely solved. There is already so much progress in this area, and there will be more, but we will never run out of room for improvement. Number 5 and number 6, new architectures and new hardware, are very challenging, but they are inevitable with time. Because of the symbiosis between architecture and hardware – new architecture will need to be optimized for common hardware, and hardware will need to support common architecture – they might be solved by the same company. Some of these problems won’t be solved using only technical knowledge. For example, number 8, improving learning from human preference, might be more of a policy problem than a technical problem. Number 9, improving the efficiency of the chat interface, is more of a UX problem. We need more people with non-technical backgrounds to work with us to solve these problems. What research direction are you most excited about? What are the most promising solutions you see for these problems? I’d love to hear from you.'
content: extracted
html: 2023-08-16-open-challenges-in-llm-research.html
preview:
  file: 2023-08-16-open-challenges-in-llm-research.preview-9ef0ce0c567b.webp
  width: 256
  height: 180
  color: '#eaf0f9'
images:
- source: https://huyenchip.com/assets/pics/llm-research/10-open-challenges-llm-research.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-895c2db24504.png
    width: 844
    height: 594
  color: '#fbfbfc'
- source: https://huyenchip.com/assets/pics/llm-research/2-context.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-3421f013a0a4.png
    width: 1999
    height: 1090
  variants:
  - file: 2023-08-16-open-challenges-in-llm-research.image-8dcce7104f0d.webp
    width: 320
    height: 174
  - file: 2023-08-16-open-challenges-in-llm-research.image-2f87eab90066.webp
    width: 640
    height: 349
  - file: 2023-08-16-open-challenges-in-llm-research.image-8bbb14cb22a9.webp
    width: 960
    height: 523
  - file: 2023-08-16-open-challenges-in-llm-research.image-fa751eaae4ba.webp
    width: 1280
    height: 698
  - file: 2023-08-16-open-challenges-in-llm-research.image-162cc6c9954e.webp
    width: 1600
    height: 872
  - file: 2023-08-16-open-challenges-in-llm-research.image-3451575eb842.webp
    width: 1999
    height: 1090
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/llm-research/2-rag.jpg
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-3e9c778704c8.jpg
    width: 1280
    height: 720
  color: '#fbfcfc'
- source: https://huyenchip.com/assets/pics/llm-research/3-flamingo.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-ccb657848915.png
    width: 1690
    height: 882
  color: '#fcfbfc'
- source: https://huyenchip.com/assets/pics/llm-research/4-llm-optimization.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-b1e17631c5b9.png
    width: 836
    height: 592
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/llm-research/5-monarch-mixer.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-ae92f4314d91.png
    width: 1999
    height: 643
  variants:
  - file: 2023-08-16-open-challenges-in-llm-research.image-9e05b616d3ac.webp
    width: 320
    height: 103
  - file: 2023-08-16-open-challenges-in-llm-research.image-7e5ce27e0e40.webp
    width: 640
    height: 206
  - file: 2023-08-16-open-challenges-in-llm-research.image-e136edf90083.webp
    width: 960
    height: 309
  - file: 2023-08-16-open-challenges-in-llm-research.image-544ac28f23c2.webp
    width: 1280
    height: 412
  - file: 2023-08-16-open-challenges-in-llm-research.image-be38a9603fab.webp
    width: 1600
    height: 515
  - file: 2023-08-16-open-challenges-in-llm-research.image-fccfe8d74ffa.webp
    width: 1999
    height: 643
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/llm-research/6-photonic-matrix-multiplication.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-9c6e0dc4109b.png
    width: 1999
    height: 1762
  variants:
  - file: 2023-08-16-open-challenges-in-llm-research.image-95b4a4d84e21.webp
    width: 320
    height: 282
  - file: 2023-08-16-open-challenges-in-llm-research.image-b138788768b8.webp
    width: 640
    height: 564
  - file: 2023-08-16-open-challenges-in-llm-research.image-22e972f02dd6.webp
    width: 960
    height: 846
  - file: 2023-08-16-open-challenges-in-llm-research.image-a55fc7d8ccb0.webp
    width: 1280
    height: 1128
  - file: 2023-08-16-open-challenges-in-llm-research.image-3269aef54396.webp
    width: 1600
    height: 1410
  - file: 2023-08-16-open-challenges-in-llm-research.image-4f80aef60e2f.webp
    width: 1999
    height: 1762
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/llm-research/8-instructgpt-demographics.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-24f249d37a47.png
    width: 1026
    height: 532
  color: '#fefefe'
- source: https://huyenchip.com/assets/pics/llm-research/8-openassistant-demographics.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-2a88e9810aaf.png
    width: 1506
    height: 1036
  color: '#fbfbfc'
- source: https://huyenchip.com/assets/pics/llm-research/9-superapp-chat-interface.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-199840d01efc.png
    width: 1400
    height: 370
  variants:
  - file: 2023-08-16-open-challenges-in-llm-research.image-22aaf1d672b7.webp
    width: 320
    height: 85
  - file: 2023-08-16-open-challenges-in-llm-research.image-3e91b0f1ca5e.webp
    width: 640
    height: 169
  - file: 2023-08-16-open-challenges-in-llm-research.image-5b8bdae637ef.webp
    width: 960
    height: 254
  - file: 2023-08-16-open-challenges-in-llm-research.image-01d861ded3d8.webp
    width: 1280
    height: 338
  - file: 2023-08-16-open-challenges-in-llm-research.image-1b3506328dc0.webp
    width: 1400
    height: 370
  color: '#f9f9f9'
- source: https://huyenchip.com/assets/pics/llm-research/9-neva.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-e2e46f22a39e.png
    width: 1930
    height: 1616
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/llm-research/10-non-english-tokens.png
  original:
    file: 2023-08-16-open-challenges-in-llm-research.image-49bfc922e78d.png
    width: 1037
    height: 660
  color: '#fcfdfd'
---

\[*[LinkedIn discussion](https://www.linkedin.com/posts/chiphuyen_llm-airesearch-generativeai-activity-7097619722363408385-s5Cp), [Twitter thread](https://twitter.com/chipro/status/1691858084824838427)*\]

Never before in my life had I seen so many smart people working on the same goal: making LLMs better. After talking to many people working in both industry and academia, I noticed the 10 major research directions that emerged. The first two directions, hallucinations and context learning, are probably the most talked about today. I’m the most excited about numbers 3 (multimodality), 5 (new architecture), and 6 (GPU alternatives).

## 1\. Reduce and measure hallucinations

[Hallucination](https://huyenchip.com/2023/05/02/rlhf.html#rlhf_and_hallucination) is a heavily discussed topic already so I’ll be quick. Hallucination happens when an AI model makes stuff up. For many creative use cases, hallucination is a feature. However, for most other use cases, hallucination is a bug. I was at a panel on LLM with Dropbox, Langchain, Elastics, and Anthropic recently, and the #1 roadblock they see for companies to adopt LLMs in production is hallucination.

Mitigating hallucination and developing metrics to measure hallucination is a blossoming research topic, and I’ve seen many startups focus on this problem. There are also ad-hoc tips to reduce hallucination, such as adding more context to the prompt, chain-of-thought, self-consistency, or asking your model to be concise in its response.

To learn more about hallucination:

- [Survey of Hallucination in Natural Language Generation](https://arxiv.org/abs/2202.03629) (Ji et al., 2022)
- [How Language Model Hallucinations Can Snowball](https://arxiv.org/abs/2305.13534) (Zhang et al., 2023)
- [A Multitask, Multilingual, Multimodal Evaluation of ChatGPT on Reasoning, Hallucination, and Interactivity](https://arxiv.org/abs/2302.04023) (Bang et al., 2023)
- [Contrastive Learning Reduces Hallucination in Conversations](https://arxiv.org/abs/2212.10400) (Sun et al., 2022)
- [Self-Consistency Improves Chain of Thought Reasoning in Language Models](https://arxiv.org/abs/2203.11171) (Wang et al., 2022)
- [SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models](https://arxiv.org/abs/2303.08896) (Manakul et al., 2023)
- A simple example of fact-checking and hallucination by [NVIDIA’s NeMo-Guardrails](https://github.com/NVIDIA/NeMo-Guardrails/blob/main/examples/grounding_rail/README.md#grounding-fact-checking-and-hallucination)

## 2\. Optimize context length and context construction

A vast majority of questions require context. For example, if we ask ChatGPT: “What’s the best Vietnamese restaurant?”, the context needed would be “where” because the best Vietnamese restaurant in Vietnam would be different from the best Vietnamese in the US.

According to this cool paper [SituatedQA](https://arxiv.org/pdf/2109.06157.pdf) (Zhang & Choi, 2021), a significant proportion of information-seeking questions have context-dependent answers, e.g. roughly 16.5% of the [Natural Questions NQ-Open dataset](https://ai.google.com/research/NaturalQuestions). Personally, I suspect that this percentage would be even higher for enterprise use cases. For example, say a company builds a chatbot for customer support, for this chatbot to answer any customer question about any product, the context needed might be that customer’s history or that product’s information.

Because the model “learns” from the context provided to it, this process is also called context learning.

![Context needed for a customer support query](https://huyenchip.com/assets/pics/llm-research/2-context.png)

Context length is especially important for RAG – [Retrieval Augmented Generation](https://arxiv.org/abs/2005.11401) (Lewis et al., 2020) – which has emerged to be the predominant pattern for LLM industry use cases. For those not yet swept away in the RAG rage, RAG works in two phases:

Phase 1: chunking (also known as indexing)

1. Gather all the documents you want your LLM to use
2. Divide these documents into chunks that can be fed into your LLM to generate embeddings and store these embeddings in a vector database.

Phase 2: querying

1. When user sends a query, like “*Does my insurance policy pay for this drug X*”, your LLM converts this query into an embedding, let’s call it QUERY\_EMBEDDING
2. Your vector database fetches the chunks whose embeddings are the most similar to QUERY\_EMBEDDING

Screenshot from [Jerry Liu’s talk on LlamaIndex](https://www.youtube.com/watch?v=njzB6fm0U8g) (2023)

![Context needed for a customer support query](https://huyenchip.com/assets/pics/llm-research/2-rag.jpg)

The longer the context length, the more chunks we can squeeze into the context. The more information the model has access to, the better its response will be, right?

Not always. How much context a model can use and how efficiently that model will use it are two different questions. In parallel with the effort to increase model context length is the effort to make the context more efficient. Some people call it “prompt engineering” or “prompt construction”. For example, a paper that has made the rounds recently is about how models are much better at understanding information at the beginning and the end of the index rather than in the middle of it – [Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172) (Liu et al., 2023).

## 3\. Incorporate other data modalities

Multimodality, IMO, is so powerful and yet so underrated. There are many reasons for multimodality.

First, there are many use cases where multimodal data is required, especially in industries that deal with a mixture of data modalities such as healthcare, robotics, e-commerce, retail, gaming, entertainment, etc. Examples:

- Oftentimes, medical predictions require both text (e.g. doctor’s notes, patients’ questionnaires) and images (e.g. CT, X-ray, MRI scans).
- Product metadata often contains images, videos, descriptions, and even tabular data (e.g. production date, weight, color). You might want to automatically fill in missing product information based on users’ reviews or product photos. You might want to enable users to search for products using visual information, like shape or color.

Second, multimodality promises a big boost in model performance. Shouldn’t a model that can understand both text and images perform better than a model that can only understand text? Text-based models require so much text that there’s a realistic concern that [we’ll soon run out of Internet data to train text-based models](https://huyenchip.com/2023/05/02/rlhf.html#data_bottleneck_for_pretraining). Once we run out of text, we’d need to leverage other data modalities.

![Multimodal Flamingo's architecture](https://huyenchip.com/assets/pics/llm-research/3-flamingo.png)

Flamingo architecture (Alayrac et al., 2022)

One use case I’m especially excited about is that multimodality can enable visually impaired people to browse the Internet and navigate the real world.

Cool multimodal work:

- [\[CLIP\] Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020) (OpenAI, 2021)
- [Flamingo: a Visual Language Model for Few-Shot Learning](https://arxiv.org/abs/2204.14198) (DeepMind, 2022)
- [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597) (Salesforce, 2023)
- [KOSMOS-1: Language Is Not All You Need: Aligning Perception with Language Models](https://arxiv.org/abs/2302.14045) (Microsoft, 2023)
- [PaLM-E: An embodied multimodal language model](https://ai.googleblog.com/2023/03/palm-e-embodied-multimodal-language.html) (Google, 2023)
- [LLaVA: Visual Instruction Tuning](https://arxiv.org/abs/2304.08485) (Liu et al., 2023)
- [NeVA: NeMo Vision and Language Assistant](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/playground/models/neva) (NVIDIA, 2023)

I’ve been working on a post on multimodality that hopefully I can share soon!

## 4\. Make LLMs faster and cheaper

When GPT-3.5 first came out in late November 2022, many people had concerns about latency and cost of using it in production. However, latency/cost analysis has changed rapidly since then. Within half a year, the community found a way to create a model that came pretty close to GPT-3.5 in terms of performance, yet required just under 2% of GPT-3.5’s memory footprint.

My takeaway: if you create something good enough, people will figure out a way to make it fast and cheap.

|          |                                                                |               |                  |                        |                       |
| -------- | -------------------------------------------------------------- | ------------- | ---------------- | ---------------------- | --------------------- |
| **Date** | **Model**                                                      | **\# params** | **Quantization** | **Memory to finetune** | **Can be trained on** |
| Nov 2022 | GPT-3.5                                                        | 175B          | 16-bit           | 375GB                  | Many, many machines   |
| Mar 2023 | [Alpaca 7B](https://crfm.stanford.edu/2023/03/13/alpaca.html) | 7B            | 16-bit           | 15GB                   | Gaming desktop        |
| May 2023 | [Guanaco 7B](https://arxiv.org/abs/2305.14314)                | 7B            | 4-bit            | 6GB                    | Any Macbook           |

Below is Guanaco 7B’s performance compared to ChatGPT GPT-3.5 and GPT-4, as reported in the Guanco paper. Caveat: in general, the performance comparison is far from perfect. LLM evaluation is very, very hard.

![Guanaco 7B's performance compared to ChatGPT GPT-3.5 and GPT-4](https://huyenchip.com/assets/pics/llm-research/4-llm-optimization.png)

Four years ago, when I started working on the notes that would later become the section **[Model Compression](https://learning.oreilly.com/library/view/designing-machine-learning/9781098107956/ch07.html#model_compression)** for the book [**Designing Machine Learning Systems**](https://www.amazon.com/Designing-Machine-Learning-Systems-Production-Ready/dp/1098107969), I wrote about four major techniques for model optimization/compression:

1. **Quantization**: by far the most general model optimization method. Quantization reduces a model’s size by using fewer bits to represent its parameters, e.g. instead of using 32 bits to represent a float, use only 16 bits, or even 4 bits.
2. **Knowledge distillation**: a method in which a small model (student) is trained to mimic a larger model or ensemble of models (teacher).
3. **Low-rank factorization**: the key idea here is to replace high-dimensional tensors with lower-dimensional tensors to reduce the number of parameters. For example, you can decompose a 3x3 tensor into the product of a 3x1 and a 1x3 tensor, so that instead of having 9 parameters, you have only 6 parameters.
4. **Pruning**

All these four techniques are still relevant and popular today. Alpaca was trained using knowledge distillation. QLoRA used a combination of low-rank factorization and quantization.

## 5\. Design a new model architecture

Since AlexNet in 2012, we’ve seen many architectures go in and out of fashion, including LSTM, seq2seq. Compared to those, Transformer is incredibly sticky. It’s been around since 2017. It’s a big question mark how much longer this architecture will be in vogue.

Developing a new architecture to outperform Transformer isn’t easy. Transformer has been so heavily optimized over the last 6 years. This new architecture has to be performing at the scale that people care about today, on the hardware that people care about. Side note: [Transformer was originally designed by Google to run fast on TPUs](https://timdettmers.com/2018/10/17/tpus-vs-gpus-for-transformers-bert/), and only later optimized on GPUs.

There was a lot of excitement in 2021 around S4 from Chris Ré’s lab – see [Efficiently Modeling Long Sequences with Structured State Spaces](https://arxiv.org/abs/2111.00396) (Gu et al., 2021). I’m not quite sure what happened to it. Chris Ré’s lab is still very invested in developing new architecture, most recently with their architecture [Monarch Mixer](https://together.ai/blog/monarch-mixer) (Fu et al., 2023) in collaboration with the startup [Together](https://together.ai/blog/monarch-mixer).

Their key idea is that for the existing Transformer architecture, the complexity of attention is quadratic in sequence length and the complexity of an MLP is quadratic in model dimension. An architecture with subquadratic complexity would be more efficient.

![Monarch Mixer architecture](https://huyenchip.com/assets/pics/llm-research/5-monarch-mixer.png)

I’m sure many other labs are working on this idea, though I’m not aware of any attempt that has been made public. If you know of any, please let me know!

## 6\. Develop GPU alternatives

GPU has been the dominating hardware for deep learning ever since AlexNet in 2012. In fact, one commonly acknowledged reason for AlexNet’s popularity is that it was the first paper to successfully use GPUs to train neural networks. Before GPUs, if you wanted to train a model at AlexNet’s scale, you’d have to use thousands of CPUs, like the one [Google released just a few months before AlexNet](https://www.nytimes.com/2012/06/26/technology/in-a-big-network-of-computers-evidence-of-machine-learning.html). Compared to thousands of CPUs, a couple of GPUs were a lot more accessible to Ph.D. students and researchers, setting off the deep learning research boom.

In the last decade, many, many companies, both big corporations, and startups, have attempted to create new hardware for AI. The most notable attempts are Google’s [TPUs](https://cloud.google.com/tpu/docs/intro-to-tpu), Graphcore’s [IPUs](https://www.graphcore.ai/products/ipu) (what’s happening with IPUs?), and [Cerebras](https://www.eetimes.com/cerebras-sells-100-million-ai-supercomputer-plans-8-more/). SambaNova raised over [a billion dollars to develop new AI chips](https://spectrum.ieee.org/sambanova-ceo-ai-interview) but seems to have pivoted to being a generative AI platform.

For a while, there has been a lot of anticipation around quantum computing, with key players being:

- [IBM’s QPU](https://www.ibm.com/quantum)
- Google’s Quantum computer reported [a major milestone in quantum error reduction](https://www.nature.com/articles/d41586-023-00536-w) earlier this year in Nature. Its quantum virtual machine is publicly accessible via [Google Colab](https://quantumai.google/quantum-virtual-machine)
- Research labs such as [MIT Center for Quantum Engineering](https://cqe.mit.edu/), [Max Planck Institute of Quantum Optics](https://www.mpq.mpg.de/en), [Chicago Quantum Exchange](https://chicagoquantum.org/), [Oak Ridge National Laboratory](https://quantum-roadmap.ornl.gov/), etc.

Another direction that is also super exciting is photonic chips. This is the direciton I know the least about – so please correct me if I’m wrong. Existing chips today use electricity to move data, which consumes a lot of power and also incurs latency. Photonic chips use photons to move data, harnessing the speed of light for faster and more efficient compute. Various startups in this space have raised hundreds of millions of dollars, including [Lightmatter](https://lightmatter.co/) ($270M), [Ayar Labs](https://ayarlabs.com/) ($220M), [Lightelligence](https://www.lightelligence.ai/) ($200M+), and [Luminous Computing](https://www.luminous.com/) ($115M).

Below is the timeline of advances of the three major methods in photonic matrix computation, from the paper [Photonic matrix multiplication lights up photonic accelerator and beyond](https://www.nature.com/articles/s41377-022-00717-8) (Zhou et al., Nature 2022). The three different methods are plane light conversion (PLC), Mach–Zehnder interferometer (MZI), and wavelength division multiplexing (WDM).

![Timeline of advances of the three major methods in photonic matrix multiplication](https://huyenchip.com/assets/pics/llm-research/6-photonic-matrix-multiplication.png)

## 7\. Make agents usable

Agents are LLMs that can take actions, like browsing the Internet, sending emails, making reservations, etc. Compared to other research directions in this post, this might be the youngest direction.

Because of the novelty and the massive potential, there’s a feverish obsession with agents. [Auto-GPT](https://github.com/Significant-Gravitas/Auto-GPT) is now the 25th most popular GitHub repo ever by the number of stars. [GPT-Engineering](https://github.com/AntonOsika/gpt-engineer) is another popular repo.

Despite the excitement, there is still doubt about whether LLMs are reliable and performant enough to be entrusted with the power to act.

One use case that has emerged though is the use of agents for social studies, like the famous Stanford experiment that shows that a small society of generative agents produces emergent social behaviors: *for example, starting with only a single user-specified notion that one agent wants to throw a Valentine’s Day party, the agents autonomously spread invitations to the party over the next two days, make new acquaintances, ask each other out on dates to the party …* ([Generative Agents: Interactive Simulacra of Human Behavior](https://arxiv.org/abs/2304.03442), Park et al., 2023)

The most notable startup in this area is perhaps Adept, founded by two Transformer co-authors (though [both already left](https://www.theinformation.com/briefings/two-co-founders-of-adept-an-openai-rival-suddenly-left-to-start-another-company)) and an ex-OpenAI VP, and has raised almost half a billion dollars to date. Last year, they had a demo showing their agent browsing the Internet and adding a new account to Salesforce. I’m looking forward to seeing their new demos 🙂

[YouTube video player](https://www.youtube.com/watch?v=a7CXIE_Gyy8)

## 8\. Improve learning from human preference

[RLHF, Reinforcement Learning from Human Preference](https://huyenchip.com/2023/05/02/rlhf.html), is cool but kinda hacky. I wouldn’t be surprised if people figure out a better way to train LLMs. There are many open questions for RLHF, such as:

**1\. How to mathematically represent human preference?**

Currently, human preference is determined by comparison: human labeler determines if response A is better than response B. However, it doesn’t take into account how much better response A is than response B.

**2\. What’s human preference?**

Anthropic measured the quality of their model’s responses along the three axes: helpful, honest, and harmless. See [Constitutional AI: Harmlessness from AI Feedback](https://arxiv.org/abs/2212.08073) (Bai et al., 2022).

DeepMind tries to generate responses that please the most people. See [Fine-tuning language models to find agreement among humans with diverse preferences](https://www.deepmind.com/publications/fine-tuning-language-models-to-find-agreement-among-humans-with-diverse-preferences), (Bakker et al., 2022).

Also, do we want AIs that can take a stand or a vanilla AI that shies away from any potentially controversial topic?

**3\. Whose preference is “human” preference, taking into account the differences in cultures, religions, political leanings, etc.?**

There are a lot of challenges in obtaining training data that can be sufficiently representative of all the potential users.

For example, for OpenAI’s InstructGPT data, there was no labeler above 65 years old. Labelers are predominantly Filipino and Bangladeshi. See [InstructGPT: Training language models to follow instructions with human feedback](https://arxiv.org/abs/2203.02155) (Ouyang et al., 2022).

![Demographics of labelers for InstructGPT](https://huyenchip.com/assets/pics/llm-research/8-instructgpt-demographics.png)

Community-led efforts, while admirable in their intention, can lead to biased data. For example, for the OpenAssistant dataset, 201 out of 222 (90.5%) respondents identify as male. [Jeremy Howard has a great Twitter thread on this](https://twitter.com/jeremyphoward/status/1647763133665271808/photo/1).

![Self-reported demographics of contributors to OpenAssistant dataset](https://huyenchip.com/assets/pics/llm-research/8-openassistant-demographics.png)

## 9\. Improve the efficiency of the chat interface

Ever since ChatGPT, there have been multiple discussions on whether chat is a suitable interface for a wide range of tasks.

- [Natural language is the lazy user interface](https://austinhenley.com/blog/naturallanguageui.html) (Austin Z. Henley, 2023)
- [Why Chatbots Are Not the Future](https://wattenberger.com/thoughts/boo-chatbots) (Amelia Wattenberger, 2023)
- [What Types of Questions Require Conversation to Answer? A Case Study of AskReddit Questions](https://arxiv.org/abs/2303.17710) (Huang et al., 2023)
- [AI chat interfaces could become the primary user interface to read documentation](https://idratherbewriting.com/blog/ai-chat-interfaces-are-the-new-user-interface-for-docs) (Tom Johnson, 2023)
- [Interacting with LLMs with Minimal Chat](https://eugeneyan.com/writing/llm-ux/) (Eugene Yan, 2023)

However, this is not a new discussion. In many countries, especially in Asia, chat has been used as the interface for super apps for about a decade. [Dan Grover had this discussion back in 2014](http://dangrover.com/blog/2014/12/01/chinese-mobile-app-ui-trends.html).

![Chat has been used as the universal interface for superapps in China for over a decade](https://huyenchip.com/assets/pics/llm-research/9-superapp-chat-interface.png)

Chat as a universal interface for Chinese apps (Dan Grover, 2014)

The discussion again got tense in 2016, when many people thought apps were dead and chatbots would be the future.

- [On chat as interface](https://acroll.medium.com/on-chat-as-interface-92a68d2bf854) (Alistair Croll, 2016)
- [Is the Chatbot Trend One Big Misunderstanding?](https://www.technologyreview.com/2016/04/25/8510/is-the-chatbot-trend-one-big-misunderstanding/) (Will Knight, 2016)
- [Bots won’t replace apps. Better apps will replace apps](http://dangrover.com/blog/2016/04/20/bots-wont-replace-apps.html) (Dan Grover, 2016)

Personally, I love the chat interface because of the following reasons:

1. Chat is an interface that everyone, even people without previous exposure to computers or the Internet, can learn to use quickly. When I volunteered at a low-income residential neighborhood (are we allowed to say slum?) in Kenya in the early 2010s, I was blown away by how comfortable everyone there was with doing banking on their phone, via texts. No one in that neighborhood had a computer.
2. Chat interface is accessible. You can use voice instead of text if your hands are busy.
3. Chat is also an incredibly robust interface – you can give it any request and it’ll give back a response, even if the response isn’t good.

However, there are certain areas that I think the chat interface can be improved upon.

1. Multiple messages per turn

   Currently, we pretty much assume one message per turn. This is not how my friends and I text. Often, I need multiple messages to complete my thought, because I need to insert different data (e.g. images, locations, links), I forgot something in the previous messages, or I just don’t feel like putting everything into a massive paragraph.

2. Multimodal input

   In the realm of multimodal applications, most energy is spent on building better models, and very little on building better interfaces. Take [Nvidia’s NeVA chatbot](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/playground/models/neva). I’m not a UX expert, but I suspect there might be room for UX improvement here.

   P.S. Sorry the NeVA team for calling you out. Even with this interface, your work is super cool!

   ![NVIDIA's NeVA interface](https://huyenchip.com/assets/pics/llm-research/9-neva.png)

3. Incorporating generative AI into your workflows

   Linus Lee covered this point well in his talk [Generative AI interface beyond chats](https://www.youtube.com/watch?v=rd-J3hmycQs). For example, if you want to ask a question about a column of a chart you’re working on, you should be able just point to that column and ask a question.

4. Editing and deletion of messages

   How would editing or deletion of a user input change the conversation flow with the chatbot?

## 10\. Build LLMs for non-English languages

We know that current English-first LLMs don’t work well for many other languages, both in terms of performance, latency, and speed. See:

- [ChatGPT Beyond English: Towards a Comprehensive Evaluation of Large Language Models in Multilingual Learning](https://arxiv.org/abs/2304.05613) (Lai et al., 2023)
- [All languages are NOT created (tokenized) equal](https://blog.yenniejun.com/p/all-languages-are-not-created-tokenized) (Yennie Jun, 2023)

![Tokenization for non-English languages](https://huyenchip.com/assets/pics/llm-research/10-non-english-tokens.png)

Here are some initiatives that I’m aware of. If you have pointers to others, I’d be happy to include them here.

- [Aya](https://aya.for.ai/): An Open Science Initiative to Accelerate Multilingual AI Progress
- [Symato](https://discord.gg/a2PCzB4AdE): Vietnamese ChatGPT
- [Cabrita](https://github.com/22-hours/cabrita): Finetuning InstructLLaMA with portuguese data
- [Luotuo-Chinese-LLM](https://github.com/LC1332/Luotuo-Chinese-LLM)
- [Chinese-LLaMA-Alpaca](https://github.com/ymcui/Chinese-LLaMA-Alpaca)
- [Chinese-Vicuna](https://github.com/Facico/Chinese-Vicuna)

Several early readers of this post told me they don’t think I should include this direction for two reasons.

1. This is less of a research problem and more of a logistics problem. We already know how to do it. Someone just needs to put money and effort into it. This is not entirely true. Most languages are considered low-resource, e.g. they have far fewer high-quality data compared to English or Chinese, and might require different techniques to train a large language model. See:

   - [Low-resource Languages: A Review of Past Work and Future Challenges](https://arxiv.org/abs/2006.07264) (Magueresse et al., 2020)
   - [JW300: A Wide-Coverage Parallel Corpus for Low-Resource Languages](https://aclanthology.org/P19-1310/) (Agić et al., 2019)
2. Those more pessimistic think that in the future, many languages will die out, and the Internet will consist of two universes in two languages: English and Mandarin. This school of thought isn’t new – anyone remembers Esperando?

The impact of AI tools, e.g. machine translation and chatbots, on language learning is still unclear. Will they help people learn new languages faster, or will they eliminate the need of learning new languages altogether?

## Conclusion

Phew, that was a lot of papers to reference, and I have no doubt that I still missed a ton. If there’s something you think I missed, please let me know.

For another perspective, check out this comprehsive paper [Challenges and Applications of Large Language Models](https://arxiv.org/abs/2307.10169) (Kaddour et al., 2023).

Some of the problems mentioned above are harder than others. For example, I think that number 10, building LLMs for non-English languages, is more straightforward with enough time and resources.

Number 1, reducing hallucination, will be much harder, since hallucination is just LLMs doing their probabilistic thing.

Number 4, making LLMs faster and cheaper, will never be completely solved. There is already so much progress in this area, and there will be more, but we will never run out of room for improvement.

Number 5 and number 6, new architectures and new hardware, are very challenging, but they are inevitable with time. Because of the symbiosis between architecture and hardware – new architecture will need to be optimized for common hardware, and hardware will need to support common architecture – they might be solved by the same company.

Some of these problems won’t be solved using only technical knowledge. For example, number 8, improving learning from human preference, might be more of a policy problem than a technical problem. Number 9, improving the efficiency of the chat interface, is more of a UX problem. We need more people with non-technical backgrounds to work with us to solve these problems.

What research direction are you most excited about? What are the most promising solutions you see for these problems? I’d love to hear from you.
