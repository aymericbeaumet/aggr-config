---
title: Building an AI Text Detector From Scratch
link: https://magazine.sebastianraschka.com/p/ai-detector-from-scratch
source: magazine-sebastianraschka-com
published: 2026-08-15T11:54:24Z
updated: 2026-08-15T11:54:24Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Sebastian Raschka, PhD
summary: An End-to-End Project With Dataset Construction, Model Training, Local Deployment, and RLVR
content: extracted
html: 2026-08-15-building-an-ai-text-detector-from-scratch.html
preview:
  file: 2026-08-15-building-an-ai-text-detector-from-scratch.preview-22de4a1380ec.webp
  width: 256
  height: 144
  color: '#edeeee'
images:
- source: https://substackcdn.com/image/fetch/$s_!Om23!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F875b5076-19a9-4ffe-91d4-cafa156f650a_5343x5696.png
  original:
    file: 2026-08-15-building-an-ai-text-detector-from-scratch.image-ff5e48d2fcf9.jpg
    width: 5343
    height: 5696
  color: '#f9f9f8'
- source: https://substackcdn.com/image/fetch/$s_!myfZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93c84f34-788c-466e-8b2c-4695c585fffb_4655x3638.png
  original:
    file: 2026-08-15-building-an-ai-text-detector-from-scratch.image-1110eb6329ea.jpg
    width: 1456
    height: 1138
  color: '#161819'
- source: https://substackcdn.com/image/fetch/$s_!Om23!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F875b5076-19a9-4ffe-91d4-cafa156f650a_5343x5696.png
  original:
    file: 2026-08-15-building-an-ai-text-detector-from-scratch.image-77d13d184022.jpg
    width: 1456
    height: 1552
  color: '#f9f9f8'
---

Substack recently launched its AI detector feature in the UI, which is super interesting.

Separately, lots of people asked me about interesting local do-it-yourself LLM projects as demos to show what small language models (SLMs) are capable of.

Putting one and one together, I thought it would be interesting to show how an AI detector can be implemented. I will also use it as a verifier to train a small language model to produce text that avoids detection. This is a small educational project for studying the limitations of AI detectors and exploring a verifier-based LLM application beyond regular reasoning models trained on math and code.

[![substack-ai](https://substackcdn.com/image/fetch/$s_!myfZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93c84f34-788c-466e-8b2c-4695c585fffb_4655x3638.png "substack-ai")](https://substackcdn.com/image/fetch/$s_!myfZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F93c84f34-788c-466e-8b2c-4695c585fffb_4655x3638.png)

Figure 1: Substack now features a built-in AI detector.

So, as mentioned above, the intended goal of this tutorial is to explain how AI detectors work by building (a simple) one.

In practice, such a detector can be used to filter out spammy content, but also to potentially improve your personal writing without turning it into AI-generated text. For example, if you wrote a lengthy article and want to improve spelling and grammar, it is tempting (and actually useful) to use a grammar checker to polish it and improve readability. There are different services for that, including general-purpose LLMs like ChatGPT. However, this also runs the risk that these tools turn your writing, even though it’s still your own writing, into something that is then overpolished and now sounds like AI and gets flagged as spammy content.

For example, with an AI checker, one could say, “Fix my grammar while ensuring that my text still scores 0% AI-generated.”

Anyway, while we are building a fully functional checker here, the goal is to explain 1) how AI checkers (can) work and 2) use this as a case study for a more general topic on how to build a scorer or verifier that can be used with LLMs.

Disclaimer: AI checkers are essentially a cat-and-mouse game. AI checkers may learn to detect a certain pattern that is indicative of AI-generated content. Then, the next LLM may incidentally or deliberately not exhibit that pattern and avoid detection. The AI checker then has to be updated to detect said LLM, and so forth. Plus, it’s also likely to encounter false positives (human written text flagged as AI-generated), but more on that later.

There are several goals of this project. The overarching goal is, of course, to illustrate how AI detectors work and show an applied end-to-end LLM project including evaluation, training, and local deployment for real-world use.

The outcome of this is an AI-detector API that can be used by humans and agents, and a user-friendly UI.

[![user-ui-preview](https://substackcdn.com/image/fetch/$s_!Om23!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F875b5076-19a9-4ffe-91d4-cafa156f650a_5343x5696.png "user-ui-preview")](https://substackcdn.com/image/fetch/$s_!Om23!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F875b5076-19a9-4ffe-91d4-cafa156f650a_5343x5696.png)

Figure 2: Preview of the local browser interface developed later in this project. It returns a whole-text AI score and can also highlight the scores for individual text chunks.

Here, we are going to develop a method similar to Pangram models, which, as far as I know, are behind Substack AI detection feature.

I wrote a short article about AI-text detection a while back in 2023: [What Are the Different Approaches for Detecting Content Generated by LLMs Such As ChatGPT? And How Do They Work and Differ?](https://sebastianraschka.com/blog/2023/detect-ai.html)

In essence, there are different ways to detect AI-written text, from supervised classifiers and perturbation-based probability tests to perplexity measures and watermarking.

In this tutorial, we will build a model that returns a 0-100 score. It’s essentially a classifier with an estimated probability score. The probability score will denote how likely a text is AI-generated according to the classifier. (Or, to be precise the score is the classifier’s estimated probability for the AI-generated class based on its training distribution. However, we shouldn’t interpreted it as a general probability that the text was written by AI.)

For this, we are going to fine-tune a DistilBERT classifier (similar to what I described in one of my early Substack articles, [Finetuning Large Language Models](https://magazine.sebastianraschka.com/p/finetuning-large-language-models)), but more details on that later when we get to that stage.
