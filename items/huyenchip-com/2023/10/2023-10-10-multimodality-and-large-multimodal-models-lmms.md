---
title: Multimodality and Large Multimodal Models (LMMs)
link: https://huyenchip.com//2023/10/10/multimodal.html
source: huyenchip-com
published: 2023-10-10T00:00:00Z
updated: 2023-10-10T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'For a long time, each ML model operated in one data mode – text (translation, language modeling), image (object detection, image classification), or audio (speech recognition). However, natural intelligence is not limited to just a single modality. Humans can read, talk, and see. We listen to music to relax and watch out for strange noises to detect danger. Being able to work with multimodal data is essential for us or any AI to operate in the real world. OpenAI noted in their GPT-4V system card that “incorporating additional modalities (such as image inputs) into LLMs is viewed by some as a key frontier in AI research and development.” Incorporating additional modalities to LLMs (Large Language Models) creates LMMs (Large Multimodal Models). Not all multimodal systems are LMMs. For example, text-to-image models like Midjourney, Stable Diffusion, and Dall-E are multimodal but don’t have a language model component. Multimodal can mean one or more of the following: Input and output are of different modalities (e.g. text-to-image, image-to-text) Inputs are multimodal (e.g. a system that can process both text and images) Outputs are multimodal (e.g. a system that can generate both text and images) This post covers multimodal systems in general, including LMMs. It consists of 3 parts. Part 1 covers the context for multimodality, including why multimodal, different data modalities, and types of multimodal tasks. Part 2 discusses the fundamentals of a multimodal system, using the examples of CLIP, which lays the foundation for many future multimodal systems, and Flamingo, whose impressive performance gave rise to LMMs. Part 3 discusses some active research areas for LMMs, including generating multimodal outputs and adapters for more efficient multimodal training, covering newer multimodal systems such as BLIP-2, LLaVA, LLaMA-Adapter V2, LAVIN, etc. The post is long. Feel free to skip to the sections most interesting to you. ⚠ Ambiguous terminology ⚠ Multimodal data can also refer to multimodal distributions, e.g. bimodal distribution, which is different from multimodal data in this post. Table of contents Part 1. Understanding Multimodal …. Why multimodal …. Data modalities …. Multimodal tasks …….. Generation …….. Vision-language understanding Part 2. Fundamentals of Multimodal Training …. CLIP: Contrastive Language-Image Pre-training …….. CLIP’s high-level architecture …….. Natural language supervision …….. Contrastive learning …….. CLIP applications …. Flamingo: the dawns of LMMs …….. Flamingo’s high-level architecture …….. Data …….. Flamingo’s vision encoder …….. Flamingo’s language model …. TL;DR: CLIP vs. Flamingo Part 3. Research Directions for LMMs …. Incorporating more data modalities …. Multimodal systems for instruction-following …. Adapters for more efficient multimodal training …. Generating multimodal outputs Conclusion Resources Part 1. Understanding Multimodal Why multimodal Many use cases are impossible without multimodality, especially those in industries that deal with a mixture of data modalities such as healthcare, robotics, e-commerce, retail, gaming, etc. An example of how multimodality can be used in healthcare. Image from Multimodal biomedical AI (Acosta et al., Nature Medicine 2022) Not only that, incorporating data from other modalities can help boost model performance. Shouldn’t a model that can learn from both text and images perform better than a model that can learn from only text or only image? Multimodal systems can provide a more flexible interface, allowing you to interact with them in whichever way works best for you at the moment. Imagine you can ask a question by typing, talking, or just pointing your camera at something. One use case that I’m especially excited about, is that multimodality can also enable visually impaired people to browse the Internet and also navigate the real world. Some cool multimodal use cases from GPT-4V Data modalities Different data modes are text, image, audio, tabular data, etc. One data mode can be represented or approximated in another data mode. For example: Audio can be represented as images (mel spectrograms). Speech can be transcribed into text, though its text-only representation loses information such as volume, intonation, pauses, etc. An image can be represented as a vector, which, in turn, can be flattened and represented as a sequence of text tokens. A video is a sequence of images plus audio. ML models today mostly treat videos as sequences of images. This is a severe limitation, as sounds have proved to be just as important as visuals for videos. 88% of TikTok users shared that sound is essential for their TikTok experience. A text can be represented as an image if you simply take a picture of it. A data table can be converted into a chart, which is an image. How about other data modalities? All digital data formats can be represented using bitstrings (strings of 0 and 1) or bytestrings. A model that can effectively learn from bitstrings or bytestrings will be very powerful, and it can learn from any data mode. There are other data modalities we haven’t touched on, such as graphs and 3D assets. We also haven’t touched on the formats used to represent smell and touch (haptics). In ML today, audio is still largely treated as a voice-based alternative to text. The most common use cases for audio are still speech recognition (speech-to-text) and speech synthesis (text-to-speech). Non-speech audio use cases, e.g. music generation, are still pretty niche. See the fake Drake & Weeknd song and MusicGen model on HuggingFace. Image is perhaps the most versatile format for model inputs, as it can be used to represent text, tabular data, audio, and to some extent, videos. There’s also so much more visual data than text data. We have phones/webcams that constantly take pictures and videos today. Text is a much more powerful mode for model outputs. A model that can generate images can only be used for image generation, whereas a model that can generate text can be used for many tasks: summarization, translation, reasoning, question answering, etc. For simplicity, we’ll focus on 2 modalities: images and text. The learnings can be somewhat generalized to other modalities. Multimodal tasks To understand multimodal systems, it’s helpful to look at the tasks they are built to solve. In literature, I commonly see vision-language tasks divided into two groups: generation and vision-language understanding (VLU), which is the umbrella term for all tasks that don’t require generation. The line between these two groups is blurred, as being able to generate answers requires understanding too. Generation For generative tasks, the output can be unimodal (e.g. text, image, 3D rendering) or multimodal. While unimodal outputs are common today, multimodal outputs are still shaping up. We’ll discuss multimodal outputs at the end of this post. Image generation (text-to-image synthesis) This category is straightforward. Examples: Dall-E, Stable Diffusion, and Midjourney. Text generation A common text generation task is visual question answering. Instead of relying only on text for the context, you can give the model both text and images. Imagine you can point your camera to anything and ask questions like: “My car won’t start. What’s wrong with it?”, “How to make this dish?”, or “What is this meme about?”. Another common use case is image captioning, which can be used as part of a text-based image retrieval system. An organization might have millions, if not billions, of images: product images, graphs, designs, team pictures, promotional materials, etc. AI can automatically generate captions and metadata for them, making it easier to find the exact images you want. Vision-language understanding We’ll zoom into two task types: classification and text-based image retrieval (TBIR). Classification Classification models can only generate outputs that belong to a pre-determined list of classes. This works when you only care about a fixed number of outcomes. For example, an OCR system only needs to predict if a visual is one of the known characters (e.g. a digit or a letter). Side note: An OCR system processes data at the character level. When used together with a system that can understand the broader context, it can improve use cases such as allowing you to “talk” to any textbook, contract, assembly instructions, etc. Document processing with GPT-4V. The model''s mistake is highlighted in red. One related task to classification is image-to-text retrieval: given an image and a pool of pre-defined texts, find the text that’s most likely to accompany the image. This can be helpful for product image search, i.e. retrieving product reviews from a picture. Text-based image retrieval (image search) Image search matters not only for search engines but also for enterprises to be able to search through all their internal images and documents. Some people call text-based image retrieval “text-to-image retrieval”. There are several approaches to text-based image retrieval. Two of them are: Generate captions and metadata for each image, either manually or automatically (see image captioning in Text generation). Given a text query, find images whose captions/metadata are closest to this text query. Train a joint embedding space for both images and text. Given a text query, generate an embedding for this query, and find all images whose embeddings are closest to this embedding. The second approach is more flexible, and I believe will be more widely used. This approach requires having a strong joint embedding space for both vision and language, like the one that OpenAI’s CLIP developed. Part 2. Fundamentals of Multimodal Training Given the existence of so many amazing multimodal systems, a challenge of writing this post is choosing which systems to focus on. In the end, I decided to focus on two models: CLIP (2021) and Flamingo (2022) both for their significance as well as availability and clarity of public details. CLIP was the first model that could generalize to multiple image classification tasks with zero- and few-shot learning. Flamingo wasn’t the first large multimodal model that could generate open-ended responses (Salesforce’s BLIP came out 3 months prior). However, Flamingo’s strong performance prompted some to consider it the GPT-3 moment in the multimodal domain. Even though these two models are older, many techniques they use are still relevant today. I hope they serve as the foundation to understanding newer models. The multimodal space is evolving repaidly, with many new ideas being developed. We’ll go over these newer models in Part 3. At a high level, a multimodal system consists of the following components: An encoder for each data modality to generate the embeddings for data of that modality. A way to align embeddings of different modalities into the same multimodal embedding space. [Generative models only] A language model to generate text responses. Since inputs can contain both text and visuals, new techniques need to be developed to allow the language model to condition its responses on not just text, but also visuals. Ideally, as many of these components should be pretrained and reusable as possible. CLIP: Contrastive Language-Image Pre-training CLIP’s key contribution is its ability to map data of different modalities, text and images, into a shared embedding space. This shared multimodal embedding space makes text-to-image and image-to-text tasks so much easier. Training this multimodal embedding space also produced a strong image encoder, which allows CLIP to achieve competitive zero-shot performance on many image classification tasks. This strong image encoder can be used for many other tasks: image generation, visual question answering, and text-based image retrieval. Flamingo and LLaVa use CLIP as their image encoder. DALL-E uses CLIP to rerank generated images. It’s unclear if GPT-4V uses CLIP. Zero-shot image classification with CLIP CLIP leveraged natural language supervision and contrastive learning, which allowed CLIP to both scale up their data and make training more efficient. We’ll go over why/how these two techniques work. CLIP''s high-level architecture CLIP''s architecture. Both encoders and projection matrices are jointly trained together from scratch. The training goal is to maximize the similarity scores of the right (image, text) pairings while minimizing the similarity scores of the wrong pairings (contrastive learning). For the image encoder, the authors experimented with both ResNet and ViT. Their best-performing model is ViT-L/14@336px: Large vision transformer (ViT-L) 14 patches (each image is divided into 14x14 pixel patches/sub-images) on 336x336 pixel input For the text encoder, CLIP uses a Transformer model similar to GPT-2 but smaller. Their base model has only 63M parameters with 8 attention heads. The authors found CLIP’s performance to be less sensitive to the capacity of the text encoder. Embeddings generated by the image encoder and text encoder are projected into the same embedding space using two projection matrices \(W_v\) and \(W_l\). Given an image embedding \(V_i\), the corresponding multimodal embedding is computed as: \(W_vV_i\). Given a text embedding \(L_i\), the corresponding multimodal embedding is computed as: \(W_lL_i\). When people say CLIP embeddings, they either refer to these multimodal embeddings or the embeddings generated by CLIP’s image encoder. Natural language supervision For many years, image models were trained with manually annotated (image, text) datasets (e.g. ImageNet, MS COCO). This isn’t scalable. Manual annotation is time-consuming and expensive. The CLIP paper noted that none of the then-available (image, text) datasets was big and high quality enough. They created their own dataset – 400M (image, text) pairs – as follows. Construct a list of 500,000 queries. Queries are common words, bigrams, and titles of popular Wikipedia articles. Find images matching these queries (string and substring match). The paper mentioned this search did NOT happen on search engines but didn’t specify where. My theory is that since OpenAI already scraped the entire Internet for their GPT models, they probably just queried their internal database. Each image is paired with a text that co-occurs with it (e.g. captions, comments) instead of the query since queries are too short to be descriptive. Because some queries are more popular than others, to avoid data imbalance, they used at most 20K images for a query. Contrastive learning Pre-CLIP, most vision-language models were trained using a classifier or language model objectives. Contrastive objective is a clever technique that allows CLIP to scale and generalize to multiple tasks. We’ll show why the constrastive objective works better for CLIP using an example task of image captioning: given an image, generate a text that describes it. Classifier objective A classifier predicts the correct class among a predetermined list of classes. This works when the output space is finite. Previous models that work with (image, text) pair datasets all had this limitation. For example, models working with ILSVRC-2012 limited themselves to 1,000 classes, and JFT-300M to 18,291 classes. This objective limits not only the model’s capacity to output meaningful responses but also its capacity for zero-shot learning. Say, if the model was trained to predict among 10 classes, it won’t work for a task that has 100 classes. Language model objective If a classifier outputs only one class for each input, a language model outputs a sequence of classes. Each generated class is called a token. Each token is from a predetermined list, the vocabulary, of the language model. Classifier vs. language model objectives Contrastive objective While the language model objective allows for vastly more flexible outputs, CLIP authors noted this objective made the training difficult. They hypothesized that this is because the model tries to generate exactly the text accompanying each image, while many possible texts can accompany an image: alt-text, caption, comments, etc. For example, in the Flickr30K dataset, each image has 5 captions provided by human annotators, and the captions for the same image can be very different. Contrastive learning is to overcome this challenge. Instead of predicting the exact text of each image, CLIP was trained to predict whether a text is more likely to accompany an image than other texts. For each batch of \(N\) (image, text) pairs, the model generates N text embeddings and N image embeddings. Let \(V_1, V_2, ..., V_n\) be the embeddings for the \(N\) images. Let \(L_1, L_2, ..., L_n\) be the embeddings for the \(N\) texts. CLIP computes the cosine similarity scores of the \(N^2\) possible (\(V_i, L_j\)) pairings. The model is trained to maximize the similarity scores of the \(N\) correct pairings while minimizing the scores of the \(N^2 - N\) incorrect pairings. For CLIP, \(N = 32,768\). Another way to look at this is that each training batch of CLIP is two classification tasks. Each image can be paired with N possible texts, and the model tries to predict the correct one. This is the same setup as image-to-text retrieval. \[L_{\text{contrastive:txt2im}} = -\frac{1}{N}\sum_i^N\log(\frac{\exp(L_i^TV_i\beta)}{\sum_j^N\exp(L_i^TV_j\beta)})\] Each text can be paired with N possible images, and the model tries to predict the correct image. This is the same setup as text-to-image retrieval. \[L_{\text{contrastive:im2txt}} = -\frac{1}{N}\sum_i^N\log(\frac{\exp(V_i^TL_i\beta)}{\sum_j^N\exp(V_i^TL_j\beta)})\] The sum of these two losses is minimized. 𝛽 is a trainable inverse temperature parameter. This is what it all looks like in pseudocode. CLIP authors found that the contrastive objective provided a 12x improvement in efficiency compared to the language model objective baseline while producing higher-quality image embeddings. CLIP applications Classification Today, for many image classification tasks, CLIP is still a strong out-of-the-box baseline to be used as-is or fine-tuned. Text-based image retrieval Since CLIP’s training process was conceptually similar to image-to-text retrieval and text-to-image retrieval, CLIP “displays significant promise for widely-applicable tasks like image retrieval or search.” However, “on image retrieval, CLIP’s performance relative to the overall state of the art is noticeably lower.” There are attempts to use CLIP for image retrieval. For example, clip-retrieval package works as follows: Generate CLIP embeddings for all your images and store them in a vector database. For each text query, generate a CLIP embedding for this text. Query in the vector database for all images whose embeddings are close to this text query embedding. Image generation CLIP’s joint image-text embeddings are useful for image generation. Given a text prompt, DALL-E (2021) generates many different visuals and uses CLIP to rerank these visuals before showing the top visuals to users. In 2022, OpenAI introduced unCLIP, a text-to-image synthesis model conditioned on CLIP latents. It consists of two main components: CLIP is trained and frozen. The pretrained CLIP model can generate embeddings for both text and images in the same embedding space. Two things happen at image generation: Use CLIP to generate embedding for this text. Use a diffusion decoder to generate images conditioned on this embedding. Text generation: visual question answering, captioning CLIP authors did attempt to create a model for text generation. One version they experimented with is called LM RN50. Though this model could generate text responses, its performance was consistently around 10% below CLIP’s best-performing model on all the vision-language understanding tasks that CLIP was evaluated on. While today CLIP isn’t used directly for text generation, its image encoder is often the backbone for LMMs that can generate texts. Flamingo: the dawns of LMMs Unlike CLIP, Flamingo can generate text responses. In a reductive view, Flamingo is CLIP + a language model, with added techniques to make it possible for the language model to generate text tokens conditioned on both visual and text inputs. Flamingo can generate text responses conditioned on both text and images Flamingo''s high-level architecture At a high level, Flamingo consists of 2 parts: Vision encoder: a CLIP-like model is trained using contrastive learning. The text encoder of this model is then discarded. The vision encoder is frozen to be used in the main model. Language model: Flamingo finetunes Chinchilla to generate text tokens, conditioned on visuals and text, using language model loss, with two additional components Perceiver Resampler and GATED XATTN-DENSE layers. We’ll discuss them later in this blog. Data Flamingo used 4 datasets: 2 (image, text) pair datasets, 1 (video, text) pair dataset, and 1 interleaved image and text dataset. Dataset Type Size How Training weight M3W Interleaved image and text dataset 43M webpages For each webpage, they sample a random subsequence of 256 tokens and take up to the first 5 images included in the sampled sequence. 1.0 ALIGN (Image, text) pairs 1.8B pairs Texts are alt-texts, averaging 12 tokens/text. 0.2 LTIP (Image, text) pairs 312M pairs Texts are long descriptions, averaging 20.5 tokens/text. 0.2 VTP (Video, text) pairs 27M short videos ~22 seconds/video on average 0.03 Flamingo''s vision encoder Flamingo first trains a CLIP-like model from scratch using contrastive learning. This component only uses the 2 (image, text) pair datasets, ALIGN and LTIP, totaling 2.1B (image, text) pairs. This is 5x larger than the dataset CLIP was trained on. For the text encoder, Flamingo uses BERT instead of GPT-2. For the vision encoder, Flamingo uses a NormalizerFree ResNet (NFNet) F6 model. Text and vision embeddings are meanpooled before being projected to the joint embedding space. Flamingo''s language model Flamingo uses Chinchilla as their language model. More specifically, they freeze the 9 pretrained Chinchilla LM layers. A traditional language model predicts the next text token based on the preceding text tokens. Flamingo predicts the next text token based on both the preceding text and visual tokens. Next token generation is conditioned on both text and visual tokens. Illustration taken from Chunyuan Li''s CVPR 2023 tutorial: Large Multimodal Models. To be able to generate text conditioned on both text and visual inputs, Flamingo relied on Perceiver Resampler and GATED XATTN-DENSE layers. Perceiver Resampler As the visual inputs can be both images and videos, the vision encoder can produce a variable number of image or video features. Perceiver Resampler converts these variable features into a consistent 64 visual outputs. Interestingly enough, while training the vision encoder, the resolution used was 288 x 288. However, at this phase, visual inputs are resized to 320 × 320. It’s been shown that a higher test-time resolution can lead to improved performance when using CNNs. GATED XATTN-DENSE layers GATED XATTN-DENSE layers are inserted between existing and frozen LM layers to allow the language model to attend more efficiently to the visual tokens when generating text tokens. Without these layers, Flamingo authors noted a drop of 4.2% in the overall score. Loss function Flamingo computes the likelihood of text \(y\) conditioned on the interleaved images and videos \(x\). \[p(y|x) = \prod_{l=1}^N p(y_l|y_{<l}, x_{\leq l})\] The training loss function was a weighted sum of expected negative log-likelihoods of generated text across all 4 datasets, with \(\lambda_m\) being the training weight of dataset \(m\). \[\sum_{m=1}^M \lambda_m E_{(x, y)\sim D_m} [ -\sum_{l=1}^L \log p(y|x)]\] Training While the Chinchilla LM layers are finetuned and frozen, the additional components are trained from scratch, using all 4 Flamingo datasets, with different weights. Finding the right per-dataset weights was key to performance. The weight for each dataset is in the Training weight column in the dataset table above. VTP’s weight is much smaller than other datasets (0.03 compared to 0.2 and 1), so its contribution to the training should be minimal. However, the authors noted that removing this dataset negatively affects performance on all video tasks. While Flamingo isn’t open-sourced, there are many open-source replications of Flamingo. IDEFICS (HuggingFace) mlfoundations/open_flamingo TL;DR: CLIP vs. Flamingo Part 3. Research Directions for LMMs CLIP is 3 years old and Flamingo is almost 2. While their architectures serve as a good foundation for us to understand how LMMs are built, there have been many new progresses in the space. Here are a few directions that I’m excited about. This is far from an exhaustive list, both because this post has been long and because I’m still learning about the space too. If you have any pointers or suggestions, please let me know! Incorporating more data modalities Today, most multimodal systems work with text and images. It’s only a matter of time before we need systems that can incorporate other modalities such as videos, music, and 3D. Wouldn’t it be amazing to have one shared embedding space for ALL data modalities? Examples of works in this space: ULIP: Learning a Unified Representation of Language, Images, and Point Clouds for 3D Understanding (Xue et al., Dec 2022) ImageBind: One Embedding Space To Bind Them All (Girdhar et al., May 2023) NExT-GPT: Any-to-Any Multimodal Large Language Model (Wu et al., Sep 2023) Jeff Dean’s ambitious Pathways project (2021): its vision is to “enable multimodal models that encompass vision, auditory, and language understanding simultaneously.” Multimodal systems for instruction-following Flamingo was trained for completion, but not for dialogue or for following instructions. (If you’re not familiar with completion vs. dialogue, check out my post on RLHF). Many people are working on building LMMs that can follow instructions and have conversations, such as: MultiInstruct: Improving Multi-Modal Zero-Shot Learning via Instruction Tuning (Xu et al., Dec 2022) LLaVA: Visual Instruction Tuning (Liu et al., Apr 28, 2023) InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning (Salesforce, May 11, 2023) LaVIN: Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models (Luo et al., May 24, 2023) Examples of LaVIN''s outputs compared to other LMMs, shown in LaVIN''s paper Adapters for more efficient multimodal training While Flamingo used 9 pretrained and frozen layers from Chinchilla, it had to pretrain its vision encoder, Perceiver Resampler, and GATED XATTN-DENSE layers from scratch. These train-from-scratch modules could be compute-intensive. Many works focus on more efficient ways to bootstrap multimodal systems using less training from scratch. Some works are quite promising. BLIP-2, for example, outperformed Flamingo-80B by 8.7% on zero-shot VQA-v2 with 54x fewer trainable parameters. Works in this space include: BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models [LAVIN] Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models LLaMA-Adapter V2: Parameter-Efficient Visual Instruction Model The two images below are from Chunyuan Li’s Large Multimodal Models tutorial at CVPR 2023, which is, btw, an excellent tutorial. Generating multimodal outputs While models that can process multimodal inputs are becoming the norm, multimodal output is still lagging. Many use cases require multimodal outputs. For example, if we ask ChatGPT to explain RLHF, an effective explanation might require graphs, equations, and even simple animations. To generate multimodal outputs, a model would first need to generate a shared intermediate output. One key question is what the intermediate output would look like. One option for intermediate output is text, which will then be used to generate/synthesize other actions. For example, CM3 (Aghajanyan et al., 2022) outputs HTML markup which can be compiled into webpages that contain not only text but also formattings, links, and images. GPT-4V generates Latex code, which can then be reconstructed as data tables. Sampled outputs from CM3 GPT-4V generates Latex code, which can then be reconstructed as a data table Another option for intermediate output would be multimodal tokens. This is the option that Caiming Xiong, whose team at Salesforce has done a lot of awesome work on multimodality, told me. Each token will have a tag to denote whether it’s a text token or an image token. Image tokens will then be input into an image model like Diffusion to generate images. Text tokens will then be input into a language model. Generating Images with Multimodal Language Models (Koh et al., Jun 2023) is an awesome paper that shows how LMMs can generate and retrieve images together with generating texts. See below. Conclusion It’s been a lot of fun going over so many multimodal papers as well as talking to people doing awesome work and trying to summarize the key patterns in one blog post. There’s so much about multimodality that I’m sure there are many things that I’ve missed, but I hope that this post provides the core patterns that will help you develop multimodal systems and apply them to your work. As you see in part 3 of this post, we’re still in the early days of multimodal systems (so early that a friend told me he’s not sure if the LMM abbreviation would catch on). Yes, in most of my conversations, there’s little doubt that multimodal systems in general, and LMMs in particular, will be even more impactful than large language models. However, keep in mind that LMMs do not make LLMs obsolete. As LMMs extend upon LLMs, the performance of an LMM relies on the performance of its base LLM. Many labs that work on multimodal systems work on LLMs in parallel. Early reviewers I’d like to thank the amazing early reviewers who gave me plenty of pointers and suggestions to make this post better: Han-chung Lee, Sam Reiswig, and Luke Metz. Resources Models An incomplete list of multimodal systems by time to give you a sense of how fast the space is moving! Microsoft COCO Captions: Data Collection and Evaluation Server (Apr 2015) VQA: Visual Question Answering (May 2015) VideoBERT: A Joint Model for Video and Language Representation Learning (Google, Apr 3, 2019) LXMERT: Learning Cross-Modality Encoder Representations from Transformers (UNC Chapel Hill, Aug 20, 2019) [CLIP] Learning Transferable Visual Models From Natural Language Supervision (OpenAI, 2021) Unifying Vision-and-Language Tasks via Text Generation (UNC Chapel Hill, May 2021) BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation (Salesforce, Jan 28, 2022) Flamingo: a Visual Language Model for Few-Shot Learning (DeepMind, April 29, 2022) GIT: A Generative Image-to-text Transformer for Vision and Language (Microsoft, May 2, 2022) MultiInstruct: Improving Multi-Modal Zero-Shot Learning via Instruction Tuning (Xu et al., Dec 2022) BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models (Salesforce, Jan 30, 2023) Cross-Modal Fine-Tuning: Align then Refine (Shen et al., Feb 11, 2023) KOSMOS-1: Language Is Not All You Need: Aligning Perception with Language Models (Microsoft, Feb 27, 2023) PaLM-E: An Embodied Multimodal Language Model (Google, Mar 10, 2023) LLaMA-Adapter: Efficient Fine-tuning of Language Models with Zero-init Attention (Zhang et al., Mar 28, 2023) mPLUG-Owl: Modularization Empowers Large Language Models with Multimodality (Ye et al., Apr 2, 2023) LLaMA-Adapter V2: Parameter-Efficient Visual Instruction Model (Gao et al., Apr 28, 2023) LLaVA: Visual Instruction Tuning (Liu et al., Apr 28, 2023) X-LLM: Bootstrapping Advanced Large Language Models by Treating Multi-Modalities as Foreign Languages (Chen et al., May 7, 2023) InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning (Salesforce, May 11, 2023) Towards Expert-Level Medical Question Answering with Large Language Models (Singhal et al., May 16, 2023) Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models (Luo et al., May 24, 2023) Shikra: Unleashing Multimodal LLM’s Referential Dialogue Magic (SenseTime, Jun 3, 2023) Macaw-LLM: Multi-Modal Language Modeling with Image, Audio, Video, and Text Integration (Tencent, Jun 15, 2023) Other resources [CVPR2023 Tutorial Talk] Large Multimodal Models: Towards Building and Surpassing Multimodal GPT-4 Slides: Large Multimodal Models [CMU course] 11-777 MMML [Open source] Salesforce’s LAVIS'
content: extracted
html: 2023-10-10-multimodality-and-large-multimodal-models-lmms.html
preview:
  file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.preview-4e58db57dea6.webp
  width: 256
  height: 153
  alt: Architecture of OpenAI's CLIP
  color: '#f2f2f0'
images:
- source: https://huyenchip.com/assets/pics/multimodal/4-CLIP-architecture.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-fe086017a303.png
    width: 2116
    height: 1264
  color: '#fdfdfc'
- source: https://huyenchip.com/assets/pics/multimodal/26-healthcare.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-3020827a1627.png
    width: 1314
    height: 1284
  color: '#fefefe'
- source: https://huyenchip.com/assets/pics/multimodal/1-gpt-4v-use-cases.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-88876ec63b49.png
    width: 1999
    height: 1490
  variants:
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-25d17937d908.webp
    width: 320
    height: 239
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-319d77d73de1.webp
    width: 640
    height: 477
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-2f05d2463337.webp
    width: 960
    height: 716
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-5c1b64d61b8f.webp
    width: 1280
    height: 954
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-b541a55e77ea.webp
    width: 1600
    height: 1193
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-f8a5fdea245a.webp
    width: 1999
    height: 1490
  color: '#fcf8ec'
- source: https://huyenchip.com/assets/pics/multimodal/2-gpt-4v-ocr.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-8d9499c4bc56.png
    width: 1999
    height: 1083
  variants:
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-cc52dff2a33f.webp
    width: 320
    height: 173
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-56a93b63d13f.webp
    width: 640
    height: 347
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-0556902c6750.webp
    width: 960
    height: 520
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-8cdb5dd677ff.webp
    width: 1280
    height: 693
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-1fb414ae8065.webp
    width: 1600
    height: 867
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-41dfc06b11ab.webp
    width: 1999
    height: 1083
  color: '#fefaee'
- source: https://huyenchip.com/assets/pics/multimodal/3-CLIP-image-classification.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-eb6a2254e647.png
    width: 1604
    height: 1522
  color: '#f1f2f2'
- source: https://huyenchip.com/assets/pics/multimodal/5-classifier-vs-language-model-objectives.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-81a28630faa3.png
    width: 1854
    height: 1058
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/multimodal/6-multiple-captions.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-3a3cf1127b56.png
    width: 1999
    height: 1341
  variants:
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-1d5185cde03d.webp
    width: 320
    height: 215
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-fedb438980a6.webp
    width: 640
    height: 429
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-68449cd0104a.webp
    width: 960
    height: 644
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-2f0f993bd2d4.webp
    width: 1280
    height: 859
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-f49d0f2c015a.webp
    width: 1600
    height: 1073
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-910808d9c625.webp
    width: 1999
    height: 1341
  color: '#fafafa'
- source: https://huyenchip.com/assets/pics/multimodal/7-clip.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-9027021a2e5e.png
    width: 1852
    height: 874
  color: '#fbfbfb'
- source: https://huyenchip.com/assets/pics/multimodal/8-clip-pseudocode.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-274c4ec76a08.png
    width: 1090
    height: 1144
  color: '#fcfcfd'
- source: https://huyenchip.com/assets/pics/multimodal/9-contrastive-learning-efficiency.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-a5a44edb370d.png
    width: 1108
    height: 702
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/multimodal/10-clip-perf.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-aaa90cacac57.png
    width: 812
    height: 1080
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/multimodal/11-unCLIP.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-d5e132272d43.png
    width: 1196
    height: 642
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/multimodal/12-flamingo-chatbots.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-ca5b60b80381.png
    width: 1728
    height: 1318
  color: '#f9f8f8'
- source: https://huyenchip.com/assets/pics/multimodal/13-flamingo-architecture.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-ccb657848915.png
    width: 1690
    height: 882
  color: '#fcfbfc'
- source: https://huyenchip.com/assets/pics/multimodal/14-flamingo-data.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-4c392f40edce.png
    width: 1610
    height: 296
  color: '#f5f5f5'
- source: https://huyenchip.com/assets/pics/multimodal/15-lmm-text-generation.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-792d86a306e8.png
    width: 1026
    height: 552
  color: '#fefefe'
- source: https://huyenchip.com/assets/pics/multimodal/16-flamingo-perceiver-resampler.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-dcd7889d9ef3.png
    width: 1999
    height: 1429
  variants:
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-49969e07eaa2.webp
    width: 320
    height: 229
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-ab9aeea5b88f.webp
    width: 640
    height: 458
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-ba056777b49e.webp
    width: 960
    height: 686
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-75e080ff9b42.webp
    width: 1280
    height: 915
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-8c7f86db8d71.webp
    width: 1600
    height: 1144
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-1e46f367b658.webp
    width: 1999
    height: 1429
  color: '#fbfbfb'
- source: https://huyenchip.com/assets/pics/multimodal/17-gated%20xattn-dense.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-b7fedce76603.png
    width: 1466
    height: 788
  color: '#fbfbfc'
- source: https://huyenchip.com/assets/pics/multimodal/18-clip-flamingo.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-dc9fb481de68.png
    width: 1538
    height: 1328
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/multimodal/19-imagebind.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-82be05f9a252.png
    width: 1999
    height: 875
  variants:
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-7f0c2b450534.webp
    width: 320
    height: 140
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-636e6d3a7f67.webp
    width: 640
    height: 280
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-fee7c9828383.webp
    width: 960
    height: 420
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-58bd3676cb8d.webp
    width: 1280
    height: 560
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-d4d2153b374d.webp
    width: 1600
    height: 700
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-631bef14ddf6.webp
    width: 1999
    height: 875
  color: '#f9fafa'
- source: https://huyenchip.com/assets/pics/multimodal/20-LaVIN.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-1f8298574488.png
    width: 1852
    height: 1682
  color: '#f6f6f6'
- source: https://huyenchip.com/assets/pics/multimodal/21-adapters-1.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-37ded2f5efc3.png
    width: 3062
    height: 1108
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/multimodal/22-adapters-2.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-2df96b2a4357.png
    width: 1999
    height: 930
  variants:
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-ec7dd4c645dc.webp
    width: 320
    height: 149
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-72b91aa1eda3.webp
    width: 640
    height: 298
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-6a211b9fbeaa.webp
    width: 960
    height: 447
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-81b2c831d2f4.webp
    width: 1280
    height: 595
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-a156470ae923.webp
    width: 1600
    height: 744
  - file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-9260ee725636.webp
    width: 1999
    height: 930
  color: '#fcfcfc'
- source: https://huyenchip.com/assets/pics/multimodal/23-cm3.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-2f033f10b593.png
    width: 1358
    height: 894
  color: '#fdfdfd'
- source: https://huyenchip.com/assets/pics/multimodal/24-gpt-4v-latex.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-61d0470240e8.png
    width: 1438
    height: 1026
  color: '#fefaf0'
- source: https://huyenchip.com/assets/pics/multimodal/27-lmms-generating-images.png
  original:
    file: 2023-10-10-multimodality-and-large-multimodal-models-lmms.image-64e9e46117b3.png
    width: 1876
    height: 1280
  color: '#fbfcfc'
---

For a long time, each ML model operated in one data mode – text (translation, language modeling), image (object detection, image classification), or audio (speech recognition).

However, natural intelligence is not limited to just a single modality. Humans can read, talk, and see. We listen to music to relax and watch out for strange noises to detect danger. Being able to work with multimodal data is essential for us or any AI to operate in the real world.

OpenAI noted in their [GPT-4V system card](https://cdn.openai.com/papers/GPTV_System_Card.pdf) that “*incorporating additional modalities (such as image inputs) into LLMs is viewed by some as a key frontier in AI research and development*.”

Incorporating additional modalities to LLMs (Large Language Models) creates LMMs (Large Multimodal Models). Not all multimodal systems are LMMs. For example, text-to-image models like Midjourney, Stable Diffusion, and Dall-E are multimodal but don’t have a language model component. Multimodal can mean one or more of the following:

1. Input and output are of different modalities (e.g. text-to-image, image-to-text)
2. Inputs are multimodal (e.g. a system that can process both text and images)
3. Outputs are multimodal (e.g. a system that can generate both text and images)

This post covers multimodal systems in general, including LMMs. It consists of 3 parts.

- Part 1 covers the context for multimodality, including why multimodal, different data modalities, and types of multimodal tasks.
- Part 2 discusses the fundamentals of a multimodal system, using the examples of CLIP, which lays the foundation for many future multimodal systems, and Flamingo, whose impressive performance gave rise to LMMs.
- Part 3 discusses some active research areas for LMMs, including generating multimodal outputs and adapters for more efficient multimodal training, covering newer multimodal systems such as BLIP-2, LLaVA, LLaMA-Adapter V2, LAVIN, etc.

The post is long. Feel free to skip to the sections most interesting to you.

**⚠ Ambiguous terminology ⚠**\
 Multimodal data can also refer to multimodal distributions, e.g. bimodal distribution, which is different from multimodal data in this post.

* * *

**Table of contents**\
 [Part 1. Understanding Multimodal](https://huyenchip.com//2023/10/10/multimodal.html#part_1_understanding_multimodal)\
 …. [Why multimodal](https://huyenchip.com//2023/10/10/multimodal.html#why_multimodal)\
 …. [Data modalities](https://huyenchip.com//2023/10/10/multimodal.html#data_modalities)\
 …. [Multimodal tasks](https://huyenchip.com//2023/10/10/multimodal.html#multimodal_tasks)\
 …….. [Generation](https://huyenchip.com//2023/10/10/multimodal.html#generation)\
 …….. [Vision-language understanding](https://huyenchip.com//2023/10/10/multimodal.html#vision_language_understanding)\
 [Part 2. Fundamentals of Multimodal Training](https://huyenchip.com//2023/10/10/multimodal.html#part_2_multimodal_training)\
 …. [CLIP: Contrastive Language-Image Pre-training](https://huyenchip.com//2023/10/10/multimodal.html#clip)\
 …….. [CLIP’s high-level architecture](https://huyenchip.com//2023/10/10/multimodal.html#clip_s_high_level_architecture)\
 …….. [Natural language supervision](https://huyenchip.com//2023/10/10/multimodal.html#natural_language_supervision)\
 …….. [Contrastive learning](https://huyenchip.com//2023/10/10/multimodal.html#contrastive_learning)\
 …….. [CLIP applications](https://huyenchip.com//2023/10/10/multimodal.html#clip_applications)\
 …. [Flamingo: the dawns of LMMs](https://huyenchip.com//2023/10/10/multimodal.html#flamingo)\
 …….. [Flamingo’s high-level architecture](https://huyenchip.com//2023/10/10/multimodal.html#flamingo_s_high_level_architecture)\
 …….. [Data](https://huyenchip.com//2023/10/10/multimodal.html#data)\
 …….. [Flamingo’s vision encoder](https://huyenchip.com//2023/10/10/multimodal.html#flamingo_s_vision_encoder)\
 …….. [Flamingo’s language model](https://huyenchip.com//2023/10/10/multimodal.html#flamingo_s_language_model)\
 …. [TL;DR: CLIP vs. Flamingo](https://huyenchip.com//2023/10/10/multimodal.html#clip_vs_flamingo)\
 [Part 3. Research Directions for LMMs](https://huyenchip.com//2023/10/10/multimodal.html#part_3_research_directions_for_lmms)\
 …. [Incorporating more data modalities](https://huyenchip.com//2023/10/10/multimodal.html#incorporating_more_data_modalities)\
 …. [Multimodal systems for instruction-following](https://huyenchip.com//2023/10/10/multimodal.html#multimodal_systems_for_instruction_following)\
 …. [Adapters for more efficient multimodal training](https://huyenchip.com//2023/10/10/multimodal.html#adapters_for_more_efficient_multimodal_training)\
 …. [Generating multimodal outputs](https://huyenchip.com//2023/10/10/multimodal.html#generating_multimodal_outputs)\
 [Conclusion](https://huyenchip.com//2023/10/10/multimodal.html#conclusion)\
 [Resources](https://huyenchip.com//2023/10/10/multimodal.html#resources)

* * *

## Part 1. Understanding Multimodal

Many use cases are impossible without multimodality, especially those in industries that deal with a mixture of data modalities such as healthcare, robotics, e-commerce, retail, gaming, etc.

![Multimodal AI in healthcare](https://huyenchip.com/assets/pics/multimodal/26-healthcare.png)

An example of how multimodality can be used in healthcare. Image from Multimodal biomedical AI (Acosta et al., Nature Medicine 2022)

Not only that, incorporating data from other modalities can help boost model performance. Shouldn’t a model that can learn from both text and images perform better than a model that can learn from only text or only image?

Multimodal systems can provide a more flexible interface, allowing you to interact with them in whichever way works best for you at the moment. Imagine you can ask a question by typing, talking, or just pointing your camera at something.

One use case that I’m especially excited about, is that multimodality can also enable visually impaired people to browse the Internet and also navigate the real world.

![Some cool multimodal use cases from GPT-4V](https://huyenchip.com/assets/pics/multimodal/1-gpt-4v-use-cases.png)

Some cool multimodal use cases from GPT-4V

## Data modalities

Different data modes are text, image, audio, tabular data, etc. One data mode can be represented or *approximated* in another data mode. For example:

- Audio can be represented as images (mel spectrograms).
- Speech can be transcribed into text, though its text-only representation loses information such as volume, intonation, pauses, etc.
- An image can be represented as a vector, which, in turn, can be flattened and represented as a sequence of text tokens.
- A video is a sequence of images plus audio. ML models today mostly treat videos as sequences of images. This is a severe limitation, as sounds have proved to be just as important as visuals for videos. [88% of TikTok users shared that sound is essential for their TikTok experience](https://www.kantar.com/uki/inspiration/advertising-media/the-power-of-tiktok).
- A text can be represented as an image if you simply take a picture of it.
- A data table can be converted into a chart, which is an image.

* * *

**How about other data modalities?**

All digital data formats can be represented using bitstrings (strings of 0 and 1) or bytestrings. A model that can effectively learn from bitstrings or bytestrings will be very powerful, and it can learn from any data mode.

There are other data modalities we haven’t touched on, such as graphs and 3D assets. We also haven’t touched on the formats used to represent smell and touch (haptics).

* * *

 In ML today, audio is still largely treated as a voice-based alternative to text. The most common use cases for audio are still speech recognition (speech-to-text) and speech synthesis (text-to-speech). Non-speech audio use cases, e.g. music generation, are still pretty niche. See the fake Drake & Weeknd song and [MusicGen model on HuggingFace](https://huggingface.co/spaces/facebook/MusicGen).

Image is perhaps the most versatile format for model inputs, as it can be used to represent text, tabular data, audio, and to some extent, videos. There’s also so much more visual data than text data. We have phones/webcams that constantly take pictures and videos today.

Text is a much more powerful mode for model outputs. A model that can generate images can only be used for image generation, whereas a model that can generate text can be used for many tasks: summarization, translation, reasoning, question answering, etc.

For simplicity, we’ll focus on 2 modalities: images and text. The learnings can be somewhat generalized to other modalities.

## Multimodal tasks

To understand multimodal systems, it’s helpful to look at the tasks they are built to solve. In literature, I commonly see vision-language tasks divided into two groups: **generation** and **vision-language understanding** (VLU), which is the umbrella term for all tasks that don’t require generation. The line between these two groups is blurred, as being able to generate answers requires understanding too.

### Generation

For generative tasks, the output can be unimodal (e.g. text, image, 3D rendering) or multimodal. While unimodal outputs are common today, multimodal outputs are still shaping up. We’ll discuss multimodal outputs at the end of this post.

#### Image generation (text-to-image synthesis)

This category is straightforward. Examples: Dall-E, Stable Diffusion, and Midjourney.

#### Text generation

A common text generation task is visual question answering. Instead of relying only on text for the context, you can give the model both text and images. Imagine you can point your camera to anything and ask questions like: “My car won’t start. What’s wrong with it?”, “How to make this dish?”, or “What is this meme about?”.

Another common use case is image captioning, which can be used as part of a text-based image retrieval system. An organization might have millions, if not billions, of images: product images, graphs, designs, team pictures, promotional materials, etc. AI can automatically generate captions and metadata for them, making it easier to find the exact images you want.

### Vision-language understanding

We’ll zoom into two task types: classification and text-based image retrieval (TBIR).

#### Classification

Classification models can only generate outputs that belong to a pre-determined list of classes. This works when you only care about a fixed number of outcomes. For example, an OCR system only needs to predict if a visual is one of the known characters (e.g. a digit or a letter).

**Side note**: An OCR system processes data at the character level. When used together with a system that can understand the broader context, it can improve use cases such as allowing you to “talk” to any textbook, contract, assembly instructions, etc.

![Document processing with GPT-4V](https://huyenchip.com/assets/pics/multimodal/2-gpt-4v-ocr.png)

Document processing with GPT-4V. The model's mistake is highlighted in red.

One related task to classification is **image-to-text retrieval**: given an image and a pool of pre-defined texts, find the text that’s most likely to accompany the image. This can be helpful for product image search, i.e. retrieving product reviews from a picture.

#### Text-based image retrieval (image search)

Image search matters not only for search engines but also for enterprises to be able to search through all their internal images and documents. Some people call text-based image retrieval “text-to-image retrieval”.

There are several approaches to text-based image retrieval. Two of them are:

1. Generate captions and metadata for each image, either manually or automatically (see image captioning in **Text generation**). Given a text query, find images whose captions/metadata are closest to this text query.
2. Train a joint embedding space for both images and text. Given a text query, generate an embedding for this query, and find all images whose embeddings are closest to this embedding.

The second approach is more flexible, and I believe will be more widely used. This approach requires having a strong joint embedding space for both vision and language, like the one that OpenAI’s [CLIP](https://arxiv.org/abs/2103.00020) developed.

## Part 2. Fundamentals of Multimodal Training

Given the existence of so many amazing multimodal systems, a challenge of writing this post is choosing which systems to focus on. In the end, I decided to focus on two models: [CLIP](https://arxiv.org/abs/2103.00020) (2021) and [Flamingo](https://arxiv.org/abs/2204.14198) (2022) both for their significance as well as availability and clarity of public details.

- CLIP was the first model that could generalize to multiple **image classification tasks** with zero- and few-shot learning.
- Flamingo wasn’t the first large multimodal model that could **generate open-ended responses** ([Salesforce’s BLIP](https://arxiv.org/abs/2201.12086) came out 3 months prior). However, Flamingo’s strong performance prompted some to consider it [the GPT-3 moment in the multimodal domain](https://arxiv.org/abs/2304.08485).

Even though these two models are older, many techniques they use are still relevant today. I hope they serve as the foundation to understanding newer models. The multimodal space is evolving repaidly, with many new ideas being developed. We’ll go over these newer models in [Part 3](https://huyenchip.com//2023/10/10/multimodal.html#part_3_research_directions_for_lmms).

At a high level, a multimodal system consists of the following components:

1. An **encoder** for each data modality to generate the embeddings for data of that modality.
2. A way to **align embeddings** of different modalities into the same **multimodal embedding space**.
3. \[Generative models only\] A **language model to generate text responses**. Since inputs can contain both text and visuals, new techniques need to be developed to allow the language model to condition its responses on not just text, but also visuals.

Ideally, as many of these components should be pretrained and reusable as possible.

## CLIP: Contrastive Language-Image Pre-training

CLIP’s key contribution is its ability to map data of different modalities, text and images, into a shared embedding space. This shared multimodal embedding space makes text-to-image and image-to-text tasks so much easier.

Training this multimodal embedding space also produced a strong image encoder, which allows CLIP to achieve **competitive zero-shot performance on many image classification tasks**. This strong image encoder can be used for many other tasks: image generation, visual question answering, and text-based image retrieval. Flamingo and LLaVa use CLIP as their image encoder. DALL-E uses CLIP to rerank generated images. It’s unclear if GPT-4V uses CLIP.

![Zero-shot image classification with CLIP](https://huyenchip.com/assets/pics/multimodal/3-CLIP-image-classification.png)

Zero-shot image classification with CLIP

CLIP leveraged **natural language supervision** and **contrastive learning**, which allowed CLIP to both scale up their data and make training more efficient. We’ll go over why/how these two techniques work.

### CLIP's high-level architecture

![Architecture of OpenAI's CLIP](https://huyenchip.com/assets/pics/multimodal/4-CLIP-architecture.png)

CLIP's architecture. Both encoders and projection matrices are jointly trained together from scratch. The training goal is to maximize the similarity scores of the right (image, text) pairings while minimizing the similarity scores of the wrong pairings (contrastive learning).

For the **image encoder**, the authors experimented with both ResNet and ViT. Their best-performing model is `ViT-L/14@336px`:

- Large vision transformer (ViT-L)
- 14 patches (each image is divided into 14x14 pixel patches/sub-images)
- on 336x336 pixel input

For the **text encoder**, CLIP uses a Transformer model similar to [GPT-2](https://openai.com/research/better-language-models) but smaller. Their base model has only 63M parameters with 8 attention heads. The authors found CLIP’s performance to be less sensitive to the capacity of the text encoder.

Embeddings generated by the image encoder and text encoder are projected into the same embedding space using two projection matrices \\(W\_v\\) and \\(W\_l\\).

- Given an image embedding \\(V\_i\\), the corresponding multimodal embedding is computed as: \\(W\_vV\_i\\).
- Given a text embedding \\(L\_i\\), the corresponding multimodal embedding is computed as: \\(W\_lL\_i\\).

When people say CLIP embeddings, they either refer to these multimodal embeddings or the embeddings generated by CLIP’s image encoder.

### Natural language supervision

For many years, image models were trained with manually annotated (image, text) datasets (e.g. ImageNet, MS COCO). This isn’t scalable. Manual annotation is time-consuming and expensive.

The CLIP paper noted that none of the then-available (image, text) datasets was big and high quality enough. They created their own dataset – 400M (image, text) pairs – as follows.

1. Construct a list of 500,000 queries. Queries are common words, bigrams, and titles of popular Wikipedia articles.
2. Find images matching these queries (string and substring match). The paper mentioned this search did NOT happen on search engines but didn’t specify where. My theory is that since OpenAI already scraped the entire Internet for their GPT models, they probably just queried their internal database.
3. Each image is paired with a text that co-occurs with it (e.g. captions, comments) instead of the query since queries are too short to be descriptive.

Because some queries are more popular than others, to avoid data imbalance, they used at most 20K images for a query.

### Contrastive learning

Pre-CLIP, most vision-language models were trained using a classifier or language model objectives. Contrastive objective is a clever technique that allows CLIP to scale and generalize to multiple tasks.

We’ll show why the constrastive objective works better for CLIP using an example task of image captioning: given an image, generate a text that describes it.

#### Classifier objective

A classifier predicts the correct class among a predetermined list of classes. This works when the output space is finite. Previous models that work with (image, text) pair datasets all had this limitation. For example, models working with [ILSVRC-2012](https://www.image-net.org/challenges/LSVRC/2012/) limited themselves to 1,000 classes, and [JFT-300M](https://arxiv.org/abs/1707.02968) to 18,291 classes.

This objective limits not only the model’s capacity to output meaningful responses but also its capacity for zero-shot learning. Say, if the model was trained to predict among 10 classes, it won’t work for a task that has 100 classes.

#### Language model objective

If a classifier outputs only one class for each input, a language model outputs a sequence of classes. Each generated class is called a token. Each token is from a predetermined list, the vocabulary, of the language model.

![Classifier vs. language model objectives](https://huyenchip.com/assets/pics/multimodal/5-classifier-vs-language-model-objectives.png)

Classifier vs. language model objectives

#### Contrastive objective

While the language model objective allows for vastly more flexible outputs, CLIP authors noted this objective made the training difficult. They hypothesized that this is because the model tries to generate *exactly* the text accompanying each image, while many possible texts can accompany an image: alt-text, caption, comments, etc.

For example, in the [Flickr30K dataset](https://arxiv.org/abs/1509.04942), each image has 5 captions provided by human annotators, and the captions for the same image can be very different.

![Multiple captions for the same image](https://huyenchip.com/assets/pics/multimodal/6-multiple-captions.png)

Contrastive learning is to overcome this challenge. Instead of predicting the exact text of each image, CLIP was trained to predict whether a text is more likely to accompany an image than other texts.

For each batch of \\(N\\) (image, text) pairs, the model generates N text embeddings and N image embeddings.

- Let \\(V\_1, V\_2, ..., V\_n\\) be the embeddings for the \\(N\\) images.
- Let \\(L\_1, L\_2, ..., L\_n\\) be the embeddings for the \\(N\\) texts.

CLIP computes the cosine similarity scores of the \\(N^2\\) possible (\\(V\_i, L\_j\\)) pairings. The model is trained to maximize the similarity scores of the \\(N\\) correct pairings while minimizing the scores of the \\(N^2 - N\\) incorrect pairings. For CLIP, \\(N = 32,768\\).

![How CLIP works](https://huyenchip.com/assets/pics/multimodal/7-clip.png)

Another way to look at this is that each training batch of CLIP is two classification tasks.

1. Each image can be paired with N possible texts, and the model tries to predict the correct one. This is the same setup as image-to-text retrieval.

   \\\[L\_{\\text{contrastive:txt2im}} = -\\frac{1}{N}\\sum\_i^N\\log(\\frac{\\exp(L\_i^TV\_i\\beta)}{\\sum\_j^N\\exp(L\_i^TV\_j\\beta)})\\\]
2. Each text can be paired with N possible images, and the model tries to predict the correct image. This is the same setup as text-to-image retrieval.

   \\\[L\_{\\text{contrastive:im2txt}} = -\\frac{1}{N}\\sum\_i^N\\log(\\frac{\\exp(V\_i^TL\_i\\beta)}{\\sum\_j^N\\exp(V\_i^TL\_j\\beta)})\\\]

The sum of these two losses is minimized. 𝛽 is a trainable inverse temperature parameter.

This is what it all looks like in pseudocode.

![CLIP pseudocode](https://huyenchip.com/assets/pics/multimodal/8-clip-pseudocode.png)

CLIP authors found that the contrastive objective provided a 12x improvement in efficiency compared to the language model objective baseline while producing higher-quality image embeddings.

![CLIP constrastive learning](https://huyenchip.com/assets/pics/multimodal/9-contrastive-learning-efficiency.png)

### CLIP applications

#### Classification

Today, for many image classification tasks, CLIP is still a strong out-of-the-box baseline to be used as-is or fine-tuned.

![CLIP as a strong baseline for image classification](https://huyenchip.com/assets/pics/multimodal/10-clip-perf.png)

#### Text-based image retrieval

Since CLIP’s training process was conceptually similar to image-to-text retrieval and text-to-image retrieval, CLIP “*displays significant promise for widely-applicable tasks like image retrieval or search*.” However, “*on image retrieval, CLIP’s performance relative to the overall state of the art is noticeably lower.*”

There are attempts to use CLIP for image retrieval. For example, [clip-retrieval](https://github.com/rom1504/clip-retrieval) package works as follows:

1. Generate CLIP embeddings for all your images and store them in a vector database.
2. For each text query, generate a CLIP embedding for this text.
3. Query in the vector database for all images whose embeddings are close to this text query embedding.

#### Image generation

CLIP’s joint image-text embeddings are useful for image generation. Given a text prompt, [DALL-E](https://openai.com/research/dall-e) (2021) generates many different visuals and uses CLIP to rerank these visuals before showing the top visuals to users.

In 2022, OpenAI introduced [unCLIP](https://openai.com/research/hierarchical-text-conditional-image-generation-with-clip-latents), a text-to-image synthesis model conditioned on CLIP latents. It consists of two main components:

1. CLIP is trained and frozen. The pretrained CLIP model can generate embeddings for both text and images in the same embedding space.
2. Two things happen at image generation:
   - Use CLIP to generate embedding for this text.
   - Use a diffusion decoder to generate images conditioned on this embedding.

![unCLIP](https://huyenchip.com/assets/pics/multimodal/11-unCLIP.png)

#### Text generation: visual question answering, captioning

CLIP authors did attempt to create a model for text generation. One version they experimented with is called LM RN50. Though this model could generate text responses, its performance was consistently around 10% below CLIP’s best-performing model on all the vision-language understanding tasks that CLIP was evaluated on.

While today CLIP isn’t used directly for text generation, its image encoder is often the backbone for LMMs that can generate texts.

## Flamingo: the dawns of LMMs

Unlike CLIP, Flamingo can generate text responses. In a reductive view, Flamingo is CLIP + a language model, with added techniques to make it possible for the language model to generate text tokens conditioned on both visual and text inputs.

![Conversations with Flamingo LMMs](https://huyenchip.com/assets/pics/multimodal/12-flamingo-chatbots.png)

Flamingo can generate text responses conditioned on both text and images

### Flamingo's high-level architecture

At a high level, Flamingo consists of 2 parts:

1. **Vision encoder**: a CLIP-like model is trained using contrastive learning. The text encoder of this model is then discarded. The vision encoder is frozen to be used in the main model.
2. **Language model**: Flamingo finetunes Chinchilla to generate text tokens, conditioned on visuals and text, using language model loss, with two additional components Perceiver Resampler and GATED XATTN-DENSE layers. We’ll discuss them later in this blog.

![Flamingo high level architecture](https://huyenchip.com/assets/pics/multimodal/13-flamingo-architecture.png)

### Data

Flamingo used 4 datasets: 2 (image, text) pair datasets, 1 (video, text) pair dataset, and 1 interleaved image and text dataset.

![Flamingo's 4 datasets](https://huyenchip.com/assets/pics/multimodal/14-flamingo-data.png)

|             |                                    |                  |                                                                                                                                      |                     |
| ----------- | ---------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------- |
| **Dataset** | **Type**                           | **Size**         | **How**                                                                                                                              | **Training weight** |
| M3W         | Interleaved image and text dataset | 43M webpages     | For each webpage, they sample a random subsequence of 256 tokens and take up to the first 5 images included in the sampled sequence. | 1.0                 |
| ALIGN       | (Image, text) pairs                | 1.8B pairs       | Texts are alt-texts, averaging 12 tokens/text.                                                                                       | 0.2                 |
| LTIP        | (Image, text) pairs                | 312M pairs       | Texts are long descriptions, averaging 20.5 tokens/text.                                                                             | 0.2                 |
| VTP         | (Video, text) pairs                | 27M short videos | \~22 seconds/video on average                                                                                                        | 0.03                |

### Flamingo's vision encoder

Flamingo first trains a CLIP-like model from scratch using contrastive learning. This component only uses the 2 (image, text) pair datasets, ALIGN and LTIP, totaling 2.1B (image, text) pairs. This is 5x larger than the dataset CLIP was trained on.

- For the text encoder, Flamingo uses BERT instead of GPT-2.
- For the vision encoder, Flamingo uses a NormalizerFree ResNet (NFNet) F6 model.
- Text and vision embeddings are meanpooled before being projected to the joint embedding space.

### Flamingo's language model

Flamingo uses Chinchilla as their language model. More specifically, they freeze the 9 pretrained Chinchilla LM layers. A traditional language model predicts the next text token based on the preceding text tokens. Flamingo predicts the next text token based on both the preceding text and visual tokens.

![Flamingo's 4 datasets](https://huyenchip.com/assets/pics/multimodal/15-lmm-text-generation.png)

Next token generation is conditioned on both text and visual tokens. Illustration taken from Chunyuan Li's CVPR 2023 tutorial: Large Multimodal Models.

To be able to generate text conditioned on both text and visual inputs, Flamingo relied on Perceiver Resampler and GATED XATTN-DENSE layers.

#### Perceiver Resampler

As the visual inputs can be both images and videos, the vision encoder can produce a variable number of image or video features. Perceiver Resampler converts these variable features into a consistent 64 visual outputs.

Interestingly enough, while training the vision encoder, the resolution used was 288 x 288. However, at this phase, visual inputs are resized to 320 × 320. It’s been shown that [a higher test-time resolution can lead to improved performance when using CNNs](https://arxiv.org/abs/1906.06423).

![Flamingo's Perceiver Resampler](https://huyenchip.com/assets/pics/multimodal/16-flamingo-perceiver-resampler.png)

#### GATED XATTN-DENSE layers

GATED XATTN-DENSE layers are inserted between existing and frozen LM layers to allow the language model to attend more efficiently to the visual tokens when generating text tokens. Without these layers, Flamingo authors noted a drop of 4.2% in the overall score.

![Flamingo's GATED ATTN-DENSE layers](<https://huyenchip.com/assets/pics/multimodal/17-gated xattn-dense.png>)

#### Loss function

Flamingo computes the likelihood of text \\(y\\) conditioned on the interleaved images and videos \\(x\\).

\\\[p(y|x) = \\prod\_{l=1}^N p(y\_l|y\_{\<l}, x\_{\\leq l})\\\]

The training loss function was a weighted sum of expected negative log-likelihoods of generated text across all 4 datasets, with \\(\\lambda\_m\\) being the training weight of dataset \\(m\\).

\\\[\\sum\_{m=1}^M \\lambda\_m E\_{(x, y)\\sim D\_m} \[ -\\sum\_{l=1}^L \\log p(y|x)\]\\\]

#### Training

While the Chinchilla LM layers are finetuned and frozen, the additional components are trained from scratch, using all 4 Flamingo datasets, with different weights. *Finding the right per-dataset weights was key to performance.* The weight for each dataset is in the **Training weight** column in the dataset table above.

VTP’s weight is much smaller than other datasets (0.03 compared to 0.2 and 1), so its contribution to the training should be minimal. However, the authors noted that removing this dataset negatively affects performance on all video tasks.

While Flamingo isn’t open-sourced, there are many open-source replications of Flamingo.

- [IDEFICS](https://huggingface.co/spaces/HuggingFaceM4/idefics_playground) (HuggingFace)
- [mlfoundations/open\_flamingo](https://github.com/mlfoundations/open_flamingo/issues)

## TL;DR: CLIP vs. Flamingo

![Flamingo's 4 datasets](https://huyenchip.com/assets/pics/multimodal/18-clip-flamingo.png)

## Part 3. Research Directions for LMMs

CLIP is 3 years old and Flamingo is almost 2. While their architectures serve as a good foundation for us to understand how LMMs are built, there have been many new progresses in the space.

Here are a few directions that I’m excited about. This is far from an exhaustive list, both because this post has been long and because I’m still learning about the space too. If you have any pointers or suggestions, please let me know!

## Incorporating more data modalities

Today, most multimodal systems work with text and images. It’s only a matter of time before we need systems that can incorporate other modalities such as videos, music, and 3D. Wouldn’t it be amazing to have one shared embedding space for ALL data modalities?

Examples of works in this space:

- [ULIP: Learning a Unified Representation of Language, Images, and Point Clouds for 3D Understanding](https://arxiv.org/abs/2212.05171) (Xue et al., Dec 2022)
- [ImageBind: One Embedding Space To Bind Them All](https://browse.arxiv.org/abs/2305.05665) (Girdhar et al., May 2023)
- [NExT-GPT: Any-to-Any Multimodal Large Language Model](https://next-gpt.github.io/) (Wu et al., Sep 2023)
- Jeff Dean’s ambitious [Pathways](https://blog.google/technology/ai/introducing-pathways-next-generation-ai-architecture/) project (2021): its vision is to “*enable multimodal models that encompass vision, auditory, and language understanding simultaneously*.”

![Imagebind](https://huyenchip.com/assets/pics/multimodal/19-imagebind.png)

## Multimodal systems for instruction-following

Flamingo was trained for completion, but not for dialogue or for following instructions. (If you’re not familiar with completion vs. dialogue, check out my post on [RLHF](https://huyenchip.com/2023/05/02/rlhf.html)). Many people are working on building LMMs that can follow instructions and have conversations, such as:

- [MultiInstruct: Improving Multi-Modal Zero-Shot Learning via Instruction Tuning](https://arxiv.org/abs/2212.10773) (Xu et al., Dec 2022)
- [LLaVA: Visual Instruction Tuning](https://arxiv.org/abs/2304.08485) (Liu et al., Apr 28, 2023)
- [InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning](https://arxiv.org/abs/2305.06500) (Salesforce, May 11, 2023)
- LaVIN: [Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models](https://arxiv.org/abs/2305.15023) (Luo et al., May 24, 2023)

![LaVIN](https://huyenchip.com/assets/pics/multimodal/20-LaVIN.png)

Examples of LaVIN's outputs compared to other LMMs, shown in LaVIN's paper

## Adapters for more efficient multimodal training

While Flamingo used 9 pretrained and frozen layers from Chinchilla, it had to pretrain its vision encoder, Perceiver Resampler, and GATED XATTN-DENSE layers from scratch. These train-from-scratch modules could be compute-intensive. Many works focus on more efficient ways to bootstrap multimodal systems using less training from scratch.

Some works are quite promising. BLIP-2, for example, outperformed Flamingo-80B by 8.7% on zero-shot VQA-v2 with 54x fewer trainable parameters.

Works in this space include:

- [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597)
- \[LAVIN\] [Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models](https://arxiv.org/abs/2305.15023)
- [LLaMA-Adapter V2: Parameter-Efficient Visual Instruction Model](https://arxiv.org/abs/2304.15010)

The two images below are from Chunyuan Li’s [Large Multimodal Models](https://datarelease.blob.core.windows.net/tutorial/vision_foundation_models_2023/slides/Chunyuan_cvpr2023_tutorial_lmm.pdf) tutorial at CVPR 2023, which is, btw, an excellent tutorial.

![Adapters for LMMs](https://huyenchip.com/assets/pics/multimodal/21-adapters-1.png)

![Adapters for LMMs](https://huyenchip.com/assets/pics/multimodal/22-adapters-2.png)

## Generating multimodal outputs

While models that can process multimodal inputs are becoming the norm, multimodal output is still lagging. Many use cases require multimodal outputs. For example, if we ask ChatGPT to explain RLHF, an effective explanation might require graphs, equations, and even simple animations.

To generate multimodal outputs, a model would first need to generate a shared intermediate output. One key question is what the intermediate output would look like.

One option for intermediate output is text, which will then be used to generate/synthesize other actions.

For example, [CM3](https://arxiv.org/abs/2201.07520) (Aghajanyan et al., 2022) outputs HTML markup which can be compiled into webpages that contain not only text but also formattings, links, and images. GPT-4V generates Latex code, which can then be reconstructed as data tables.

![CM3](https://huyenchip.com/assets/pics/multimodal/23-cm3.png)

Sampled outputs from CM3

![GPT-4V generating LaTeX](https://huyenchip.com/assets/pics/multimodal/24-gpt-4v-latex.png)

GPT-4V generates Latex code, which can then be reconstructed as a data table

Another option for intermediate output would be multimodal tokens. This is the option that [Caiming Xiong](https://www.linkedin.com/in/caiming-xiong-150a1417), whose team at Salesforce has done a lot of awesome work on multimodality, told me. Each token will have a tag to denote whether it’s a text token or an image token. Image tokens will then be input into an image model like Diffusion to generate images. Text tokens will then be input into a language model.

[Generating Images with Multimodal Language Models](https://arxiv.org/abs/2305.17216) (Koh et al., Jun 2023) is an awesome paper that shows how LMMs can generate and retrieve images together with generating texts. See below.

![LMMs generating text and images](https://huyenchip.com/assets/pics/multimodal/27-lmms-generating-images.png)

## Conclusion

It’s been a lot of fun going over so many multimodal papers as well as talking to people doing awesome work and trying to summarize the key patterns in one blog post. There’s so much about multimodality that I’m sure there are many things that I’ve missed, but I hope that this post provides the core patterns that will help you develop multimodal systems and apply them to your work.

As you see in part 3 of this post, we’re still in the early days of multimodal systems (so early that a friend told me he’s not sure if the LMM abbreviation would catch on). Yes, in most of my conversations, there’s little doubt that multimodal systems in general, and LMMs in particular, will be even more impactful than large language models. However, keep in mind that LMMs do not make LLMs obsolete. As LMMs extend upon LLMs, the performance of an LMM relies on the performance of its base LLM. Many labs that work on multimodal systems work on LLMs in parallel.

## Early reviewers

I’d like to thank the amazing early reviewers who gave me plenty of pointers and suggestions to make this post better: [Han-chung Lee](https://www.linkedin.com/in/hanchunglee/), [Sam Reiswig](https://www.linkedin.com/in/samreiswig/), and [Luke Metz](https://twitter.com/Luke_Metz).

## Resources

### Models

An incomplete list of multimodal systems by time to give you a sense of how fast the space is moving!

- [Microsoft COCO Captions: Data Collection and Evaluation Server](https://arxiv.org/abs/1504.00325) (Apr 2015)
- [VQA: Visual Question Answering](https://arxiv.org/abs/1505.00468) (May 2015)
- [VideoBERT: A Joint Model for Video and Language Representation Learning](https://arxiv.org/abs/1904.01766) (Google, Apr 3, 2019)
- [LXMERT: Learning Cross-Modality Encoder Representations from Transformers](https://arxiv.org/abs/1908.07490) (UNC Chapel Hill, Aug 20, 2019)
- [\[CLIP\] Learning Transferable Visual Models From Natural Language Supervision](https://arxiv.org/abs/2103.00020) (OpenAI, 2021)
- [Unifying Vision-and-Language Tasks via Text Generation](https://arxiv.org/abs/2102.02779) (UNC Chapel Hill, May 2021)
- [BLIP: Bootstrapping Language-Image Pre-training for Unified Vision-Language Understanding and Generation](https://arxiv.org/abs/2201.12086) (Salesforce, Jan 28, 2022)
- [Flamingo: a Visual Language Model for Few-Shot Learning](https://arxiv.org/abs/2204.14198) (DeepMind, April 29, 2022)
- [GIT: A Generative Image-to-text Transformer for Vision and Language](https://arxiv.org/abs/2205.14100) (Microsoft, May 2, 2022)
- [MultiInstruct: Improving Multi-Modal Zero-Shot Learning via Instruction Tuning](https://arxiv.org/abs/2212.10773) (Xu et al., Dec 2022)
- [BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models](https://arxiv.org/abs/2301.12597) (Salesforce, Jan 30, 2023)
- [Cross-Modal Fine-Tuning: Align then Refine](https://arxiv.org/abs/2302.05738) (Shen et al., Feb 11, 2023)
- [KOSMOS-1: Language Is Not All You Need: Aligning Perception with Language Models](https://arxiv.org/abs/2302.14045) (Microsoft, Feb 27, 2023)
- [PaLM-E: An Embodied Multimodal Language Model](https://arxiv.org/abs/2303.03378) (Google, Mar 10, 2023)
- [LLaMA-Adapter: Efficient Fine-tuning of Language Models with Zero-init Attention](https://arxiv.org/abs/2303.16199) (Zhang et al., Mar 28, 2023)
- [mPLUG-Owl: Modularization Empowers Large Language Models with Multimodality](https://arxiv.org/abs/2304.14178) (Ye et al., Apr 2, 2023)
- [LLaMA-Adapter V2: Parameter-Efficient Visual Instruction Model](https://arxiv.org/abs/2304.15010) (Gao et al., Apr 28, 2023)
- [LLaVA: Visual Instruction Tuning](https://arxiv.org/abs/2304.08485) (Liu et al., Apr 28, 2023)
- [X-LLM: Bootstrapping Advanced Large Language Models by Treating Multi-Modalities as Foreign Languages](https://arxiv.org/abs/2305.04160) (Chen et al., May 7, 2023)
- [InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning](https://arxiv.org/abs/2305.06500) (Salesforce, May 11, 2023)
- [Towards Expert-Level Medical Question Answering with Large Language Models](https://arxiv.org/abs/2305.09617) (Singhal et al., May 16, 2023)
- [Cheap and Quick: Efficient Vision-Language Instruction Tuning for Large Language Models](https://arxiv.org/abs/2305.15023) (Luo et al., May 24, 2023)
- [Shikra: Unleashing Multimodal LLM’s Referential Dialogue Magic](https://arxiv.org/abs/2306.15195) (SenseTime, Jun 3, 2023)
- [Macaw-LLM: Multi-Modal Language Modeling with Image, Audio, Video, and Text Integration](https://arxiv.org/abs/2306.09093) (Tencent, Jun 15, 2023)

### Other resources

- \[CVPR2023 Tutorial Talk\] [Large Multimodal Models: Towards Building and Surpassing Multimodal GPT-4](https://www.youtube.com/watch?v=mkI7EPD1vp8)
  - Slides: [Large Multimodal Models](https://datarelease.blob.core.windows.net/tutorial/vision_foundation_models_2023/slides/Chunyuan_cvpr2023_tutorial_lmm.pdf)
- \[CMU course\] [11-777 MMML](https://cmu-multicomp-lab.github.io/mmml-course/fall2022/)
- \[Open source\] [Salesforce’s LAVIS](https://github.com/salesforce/LAVIS)
