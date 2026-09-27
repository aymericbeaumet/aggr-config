---
title: Some Intuition on Attention and the Transformer
link: https://eugeneyan.com//writing/attention/
source: eugeneyan-com
published: 2023-05-21T00:00:00Z
updated: 2023-05-21T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- deeplearning
- llm
summary: What's the big deal, intuition on query-key-value vectors, multiple heads, multiple layers, and more.
content: extracted
html: 2023-05-21-some-intuition-on-attention-and-the-transformer.html
preview:
  file: 2023-05-21-some-intuition-on-attention-and-the-transformer.preview-45f01cd8b6ee.webp
  width: 256
  height: 134
  color: '#5e4032'
images:
- source: https://eugeneyan.com/assets/og_image/attention.jpg
  original:
    file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-90541689829c.jpg
    width: 1200
    height: 630
  color: '#02010b'
- source: https://eugeneyan.com/assets/encoder-decoder.webp
  original:
    file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-444887806a87.webp
    width: 720
    height: 337
  color: '#fefefe'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-05-21-some-intuition-on-attention-and-the-transformer.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

ChatGPT and other chatbots (e.g., Bard, Claude) have thrust LLMs into the mainstream. As a result, more and more people outside ML and NLP circles are trying to grasp the concept of attention and the Transformer model. Here, we’ll address some questions and try to provide intuition on the Transformer architecture. **The intended audience is people who *have read the [paper](https://arxiv.org/abs/1706.03762) and have a basic understanding of how attention works.***

To keep it simple, I’ll mostly refer to “words” in a “sentence”. Nonetheless, attention can apply to any generic set of items in a sequence. For example, instead of words, we could have tokens, events, or products. And instead of a sentence, we could have a paragraph, in-session behavior, or purchase history.

## What’s the big deal about attention?

Consider machine translation as an example. Before attention, most translation was done via an encoder-decoder network. The encoder encodes the input sentence (“I love you”) via a recurrent model and the decoder decodes it into another language (“我爱你”).

![Encoding an input sentence into a fixed-size vector for the decoder](https://eugeneyan.com/assets/encoder-decoder.webp "Encoding an input sentence into a fixed-size vector for the decoder")

Encoding an input sentence into a fixed-size vector for the decoder ([source](https://towardsdatascience.com/understanding-encoder-decoder-sequence-to-sequence-model-679e04af4346))

Via this approach, the encoder had to cram the entire input into a fixed-size vector which is then passed to the decoder—this single vector had to convey everything about the input sentence! Naturally, this led to an informational bottleneck. **With attention, we no longer have to encode input sentences into a single vector.** Instead, we let the decoder *attend* to different words in the input sentence at each step of output generation. This increases the informational capacity, from a single fixed-size vector to the entire sentence (of vectors).

Furthermore, previous recurrent models had long paths between input and output words. If you had a 50-word sentence, the decoder had to recall information from 50 steps ago for the first word (and that data had to be squeezed into a single vector). As a result, recurrent models had difficulty dealing with long-range dependencies. Attention addressed this by **letting each step of the decoder see the entire input sentence and decide what words to *attend* to.** This cut down path length and made it consistent across all steps in the decoder.

Finally, prior language models leaned heavily on a recurrent approach: To encode a sentence, we start with the first word (`w1`) and process it to get the first hidden state (`h1`). Then, we input the second word (`w2`) with the previous hidden state (`h1`) to derive the next hidden state (`h2`). And so on. Unfortunately, this process was sequential and prevented parallelization. Attention tackled this by **reading the entire sentence in one go and computing the representation of each word, based on the sentence, in parallel**.

## What are query, key, and value vectors?

Imagine yourself in a library. You have a specific question (**query**). Books on the shelves have titles on their spines (**keys**) that suggest their content. You compare your question to these titles to decide how relevant each book is, and how much **attention** to give each book. Then, you get the information (**value**) from the relevant books to answer your question.

In attention, **the *query* refers to the word we’re computing attention for.** In the case of an encoder, the query vector points to the current input word (aka context). For example, if the context was the first word in the input sentence, it would have a query vector `q1`.

**The *keys* represent the words in the input sentence.** The first word has key vector `k1`, the second word has vector `k2`, and so on. The key vectors help the model understand how each word relates to the context word. If the first word is the context, we compare the keys to `q1`.

**Attention is how much weight the query word** (e.g., `q1`) **should give each word in the sentence** (e.g., `k1`, `k2`, etc). This is computed via a dot product between the query vector and all the key vectors. (A dot product tells us [how similar two vectors are](https://en.wikipedia.org/wiki/Cosine_similarity#:~:text=Cosine%20similarity%20is%20the%20cosine,but%20only%20on%20their%20angle.).) If the dot product between a query-key pair is high, we pay more attention to it. These dot products then go through a [softmax](https://en.wikipedia.org/wiki/Softmax_function) which makes the attention scores (across all keys) sum to 1.

**Each word is also represented by a *value* which contains the information of that word.** These value vectors are weighed by the attention scores that sum to 1. As a result, each context word is now represented by an attention-based weightage of all the words in the sentence, where the most relevant words have higher weight.

## What does the encoder and decoder do?

The encoder takes a text input, such as a sentence, and returns a sequence of embeddings. These output embeddings can then be used for classification, translation, semantic similarity, etc. Self-attention enables the encoder to weigh the importance of each word and capture both short and long-range dependencies.

In contrast, the decoder takes inputs such as a start-of-sentence token and (optional) embeddings from the encoder, and returns probabilities to select the next word. Self-attention enables the decoder to focus on different parts of the output generated so far; cross-attention (aka encoder-decoder attention) helps it attend to the encoder’s output.

## How does the decoder generate words?

The decoder outputs the probability of the next word (i.e., every possible word has an associated probability). Thus, we can generate the next word by greedily picking the word with the highest probability. Alternatively, we can apply [beam search](https://en.wikipedia.org/wiki/Beam_search) and keep the top *n* predictions, generate the word after next for each of these top *n* predictions, and select whichever combination had less error.

## Why have multiple attention heads?

**Multiple heads lets the model consider multiple words simultaneously.** Because we use the softmax function in attention, it amplifies the highest value while squashing the lower ones. As a result, each head tends to focus on a single element.

Consider the sentence: “The chicken crossed the road carelessly”. The following words are relevant to “crossed” and should be attended to:

- The “chicken” is the subject doing the crossing.
- The “road” is the object being crossed.
- The crossing is done “carelessly”.

**If we had a single attention head, we might only focus on a single word**, either “chicken”, “road”, or “crossed”. Multiple heads let us attend to several words. **It also provides redundancy**, where if any single head fails, we have the other attention heads to rely on.

## Why have multiple attention layers?

**Multiple attention layers builds in redundancy** (on top of having multiple attention heads). If we only had a single attention layer, that attention layer would have to do a flawless job—this design could be brittle and lead to suboptimal outcomes. We can address this via multiple attention layers, where each one uses the output of the previous layer [*with the safety net of skip connections*](https://eugeneyan.com//writing/attention/#why-have-skip-connections). Thus, if any single attention layer messed up, the skip connections and downstream layers can mitigate the issue.

**Stacking attention layers also broadens the model’s receptive field.** The first attention layer produces context vectors by attending to interactions between pairs of words in the input sentence. Then, the second layer produces context vectors based on pairs of pairs, and so on. With more attention layers, the Transformer gains a wider perspective and can attend to multiple interaction levels within the input sentence.

## Why have skip connections?

Because attention acts as a filter, it blocks most information from passing through. As a result, a small change to the inputs of the attention layer may not change the outputs, if the attention score is tiny or zero. This can lead to flat gradients or local optima.

**[Skip connections](https://en.wikipedia.org/wiki/Residual_neural_network#:~:text=The%20identity%20skip%20connections,%20Transformer%20models) help dampen the impact of poor attention filtering**. Even if an input’s attention weight is zero and the input is blocked, skip connections add a copy of that input to the output. This ensures that even small changes to the input can still have noticeable impact on the output. **Furthermore, skip connections preserve the input sentence:** There’s no guarantee that a context word will attend to itself in a transformer. Skip connections ensure this by taking the context word vector and adding it to the output.

Finally, here’s what Andrej Karpathy had to say about Transformers (and Attention).

> The Transformer is a magnificient neural network architecture because it is a general-purpose differentiable computer. It is simultaneously:\
> 1) expressive (in the forward pass)\
> 2) optimizable (via backpropagation+gradient descent)\
> 3) efficient (high parallelism compute graph)
>
> — Andrej Karpathy (@karpathy) [October 19, 2022](https://twitter.com/karpathy/status/1582807367988654081?ref_src=twsrc%5Etfw)

## References

- [Attention Is All You Need](https://arxiv.org/abs/1706.03762)
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
- [Transformers From Scratch](https://peterbloem.nl/blog/transformers)
- [Transformers From Scratch](https://e2eml.school/transformers.html) (yeap, same title, not an error)
- [Understanding the Attention Mechanism in Sequence Models](https://www.jeremyjordan.me/attention/)

OG image prompt: “words in a sentence, emphasis on the words, with a pair of glasses, in the style of contrasting tones, artifacts of online culture, innovative page design, complexity theory, bold black and whites, bold color scheme –ar 2:1 –version 5.1”

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (May 2023). Some Intuition on Attention and the Transformer. eugeneyan.com. https://eugeneyan.com/writing/attention/.

or

```
@article{yan2023attention,
  title   = {Some Intuition on Attention and the Transformer},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {May},
  url     = {https://eugeneyan.com/writing/attention/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
