---
title: Large Transformer Model Inference Optimization
link: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/
source: lilianweng-github-io
published: 2023-01-10T17:00:00Z
updated: 2023-01-10T17:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: '[Updated on 2023-01-24: add a small section on Distillation.] Large transformer models are mainstream nowadays, creating SoTA results for a variety of tasks. They are powerful but very expensive to train and use. The extremely high inference cost, in both time and memory, is a big bottleneck for adopting a powerful transformer for solving real-world tasks at scale. Why is it hard to run inference for large transformer models? Besides the increasing size of SoTA models, there are two main factors contributing to the inference challenge (Pope et al. 2022):'
content: extracted
html: 2023-01-10-large-transformer-model-inference-optimization.html
preview:
  file: 2023-01-10-large-transformer-model-inference-optimization.preview-01dc7df74cc2.webp
  width: 256
  height: 106
  color: '#e9e7e8'
images:
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/distillation.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-fce4d23d79d4.png
    width: 1612
    height: 668
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/quantization-experiment-table.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-3851e0cc8961.png
    width: 1846
    height: 430
  color: '#e7e7e7'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/OPT-models-outlier.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-89686d7c797d.png
    width: 1810
    height: 1504
  color: '#eaeaf2'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/LLM-int8.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-e6c516b9a9e9.png
    width: 2430
    height: 984
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/quantization-granularity.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-197415b1bb1a.png
    width: 1970
    height: 498
  color: '#fbfbfb'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/SmoothQuant.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-5a40f60138c6.png
    width: 1182
    height: 1026
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/2-to-4-sparsity.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-3c51477efbc5.png
    width: 1258
    height: 668
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/permutation-QK.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-0dfd6ae9a59c.png
    width: 2420
    height: 998
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/permutation-FFN.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-48d3c4f4e57a.png
    width: 2890
    height: 1974
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/N-to-M-sparsity-permutation-algo.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-c66eea745915.png
    width: 1858
    height: 866
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/SR-STE.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-b7015f83644b.png
    width: 2622
    height: 892
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/Top-KAST-stabilize.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-688192588a8f.png
    width: 3560
    height: 804
  color: '#fafafa'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/scaling-transformer-speedup-table.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-49adaaf5eacd.png
    width: 1416
    height: 860
  color: '#fbfbfb'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/sparse-FFN.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-c49ac4fb6f7c.png
    width: 1888
    height: 800
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/sparse-QKV.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-7dcb23fa36fb.png
    width: 1942
    height: 720
  color: '#fdfdfc'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/BPR.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-baefd82d7824.png
    width: 1838
    height: 378
  color: '#fafafa'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/PR-MoE.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-7be8641b6e60.png
    width: 1660
    height: 986
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2023-01-10-inference-optimization/efficient-transformer.png
  original:
    file: 2023-01-10-large-transformer-model-inference-optimization.image-29ac320bd86b.png
    width: 2182
    height: 1814
  color: '#f9f9f9'
---

\[Updated on 2023-01-24: add a small section on [Distillation](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/#distillation).\]

Large transformer models are mainstream nowadays, creating SoTA results for a variety of tasks. They are powerful but very expensive to train and use. The extremely high inference cost, in both time and memory, is a big bottleneck for adopting a powerful transformer for solving real-world tasks at scale.

**Why is it hard to run inference for large transformer models?** Besides the increasing size of SoTA models, there are two main factors contributing to the inference challenge ([Pope et al. 2022](https://arxiv.org/abs/2211.05102)):

1. *Large memory footprint*. Both model parameters and intermediate states are needed in memory at inference time. For example,
   - The KV cache should be stored in memory during decoding time; E.g. For a batch size of 512 and context length of 2048, the KV cache totals 3TB, that is 3x the model size (!).
   - Inference cost from the attention mechanism scales quadratically with input sequence length.
2. *Low parallelizability.* Inference generation is executed in an autoregressive fashion, making the decoding process hard to parallel.

In this post, we will look into several approaches for making transformer inference more efficient. Some are general network compression methods, while others are specific to transformer architecture.

## Methods Overview

We in general consider the following as goals for model inference optimization:

- Reduce the memory footprint of the model by using fewer GPU devices and less GPU memory;
- Reduce the desired computation complexity by lowering the number of FLOPs needed;
- Reduce the inference latency and make things run faster.

Several methods can be used to make inference cheaper in memory or/and faster in time.

1. Apply various *parallelism* to scale up the model across a large number of GPUs. Smart parallelism of model components and data makes it possible to run a model of trillions of parameters.
2. Memory *offloading* to offload temporarily unused data to the CPU and read them back when needed later. This helps with memory usage but causes higher latency.
3. Smart batching strategy; E.g. [EffectiveTransformer](https://github.com/bytedance/effective_transformer) packs consecutive sequences together to remove padding within one batch.
4. Network *compression* techniques, such as *pruning, quantization, distillation*. A model of smaller size, in terms of parameter count or bitwidth, should demand less memory and run faster.
5. Improvement specific to a target model architecture. Many *architectural changes*, especially those for attention layers, help with transformer decoding speed.

Check [the previous post on large model training](https://lilianweng.github.io/posts/2021-09-25-train-large/) on different types of training parallelism and memory saving designs including CPU memory offloading. This post focuses on network compression techniques and architecture-specific improvement for transformer models.

## Distillation

**Knowledge Distillation** (**KD**; [Hinton et al. 2015](https://arxiv.org/abs/1503.02531), [Gou et al. 2020](https://arxiv.org/abs/2006.05525)) is a straightforward way to build a smaller, cheaper model (*“student model”*) to speed up inference by transferring skills from a pre-trained expensive model (*“teacher model”*) into the student. There is no much restriction on how the student architecture should be constructed, except for a matched output space with the teacher in order to construct a proper learning objective.

![](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/distillation.png)\
The generic framework of teacher-student knowledge distillation training. (Image source: [Gou et al. 2020](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/”https://arxiv.org/abs/2006.05525”))

Given a dataset, a student model is trained to mimic outputs of a teacher via distillation loss. Usually a neural network has a softmax layer; For example, a LLM outputs a probability distribution over tokens. Let’s denote the logits layer right before softmax as $\\mathbf{z}\_t$ and $\\mathbf{z}\_s$ for teacher and student models, respectively. The *distillation loss* minimizes the difference between two softmax outputs with a high temperature $T$. When ground truth labels $\\mathbf{y}$ are known, we can combine it with a *supervised* learning objective between ground truth and the student’s soft logits using e.g. cross-entropy.

$$ \\mathcal{L}\_\\text{KD} = \\mathcal{L}\_\\text{distll}(\\text{softmax}(\\mathbf{z}\_t, T), \\text{softmax}(\\mathbf{z}\_s, T)) + \\lambda\\mathcal{L}\_\\text{CE}(\\mathbf{y}, \\mathbf{z}\_s) $$

where $\\lambda$ is a hyperparameter to balance between soft and hard learning objectives. A common choice for $\\mathcal{L}\_\\text{distll}$ is KL divergence / cross entropy.

A successful early trial is **DistilBERT** ([Sanh et al. 2019](https://arxiv.org/abs/1910.01108)) that is able to reduce the parameters of a BERT by 40% while maintaining 97% performance of BERT on fine-tuned downstream tasks and running 71% faster. The loss of pre-training DistilBERT is a combination of soft distillation loss, supervised training loss (i.e. [Masked language modeling loss](https://lilianweng.github.io/posts/2019-01-31-lm/#MLM) $\\mathcal{L}\_\\text{MLM}$ in the case of BERT) and a special *cosine embedding loss* to align the hidden state vectors between teacher and student.

Distillation can be easily combined with [quantization](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/#quantization), [pruning](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/#pruning) or [sparsification](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/#sparsity) techniques, where the teacher model is the original full-precision, dense model and the student is quantized, pruned, or trimmed to have higher sparsity level.

## Quantization

There are two common approaches for applying quantization on a deep neural network:

1. *Post-Training Quantization (PTQ)*: A model is first trained to convergence and then we convert its weights to lower precision without more training. It is usually quite cheap to implement, in comparison to training.
2. *Quantization-Aware Training (QAT)*: Quantization is applied during pre-training or further fine-tuning. QAT is able to attain better performance but requires extra computation resources and access to representative training data.

We should be aware of the gap between theoretical optimal quantization strategy and the hardware kernel support. Due to the lack of GPU kernel support for certain types of matrix multiplication (e.g. INT4 x FP16), not all the methods below result in speedup for the actual inference.

## Challenges for Transformer Quantization

Many studies on Transformer model quantization have the same observation: A simple low-precision (e.g. 8-bit) post-training quantization leads to significant performance drop mainly due to the high dynamic ranges of activation and a naive activation quantization strategy fails to maintain the capacity.

![](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/quantization-experiment-table.png)\
Only quantizing model weights to 8-bit while keeping activation at full precision (\`W8A32\`) achieves much better results when activations are quantized to 8-bit irrespective of whether weights are in lower precision (\`W8A8\` and \`W32A8\`). (Image source: [Bondarenko et al. 2021](https://arxiv.org/abs/2109.12948))

[Bondarenko et al. (2021)](https://arxiv.org/abs/2109.12948) observed in a small BERT model that FFN’s input and output have very different dynamic ranges due to strong outliers in the output tensor. Therefore per-tensor quantization for the FFN’s residual sum is likely to cause a notable error.

As the model size continues to grow to billions of parameters, outlier features of high magnitude start to emerge in *all* transformer layers, causing failure of simple low-bit quantization. [Dettmers et al. (2022)](https://arxiv.org/abs/2208.07339) observed such a phenomenon for [OPT](https://arxiv.org/abs/2205.01068) models larger than 6.7B parameters. Larger models have more layers with extreme outliers and these outlier features have a significant impact on the model performance. The scale of activation outliers in a few dimensions can be ~100× larger than most of the other values.

![](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/OPT-models-outlier.png)\
The mean zero-shot accuracy over a set of language tasks (WinoGrande, HellaSwag, PIQA, LAMBADA) of OPT models of increasing sizes. (Image source: [Dettmers et al. 2022](https://arxiv.org/abs/2208.07339))

## Post-training quantization (PTQ)

### Mixed-precision quantization

The most straightforward approach for resolving the above quantization challenge is to implement quantization at different precision for weights vs activation.

GOBO ([Zadeh et al. 2020](https://arxiv.org/abs/2005.03842)) is one of the first models to apply post-training quantization on transformers (i.e. a small BERT model). It assumes that model weights of each layer follow a Gaussian distribution and therefore detects outliers by tracking mean and standard deviation per layer. Outlier features remain in original form, while other values are split into multiple bins and only corresponding bin indices of weights and the centroid values are stored.

Based on the observation that only certain activation layers (e.g. residual connections after FFN) in BERT cause big performance drop, [Bondarenko et al. (2021)](https://arxiv.org/abs/2109.12948) adopted mixed-precision quantization by using 16-bit quantization on problematic activations but 8-bit on others.

Mixed-precision quantization in `LLM.int8()` ([Dettmers et al. 2022](https://arxiv.org/abs/2208.07339)) is implemented via two mixed-precision decompositions:

1. Because matrix multiplication contains a set of independent inner products between row and column vectors, we can impose independent quantization per inner product: Each row and column are scaled by the absolution maximum values and then quantized to INT8.
2. Outlier activation features (e.g. 20x larger than other dimensions) remain in FP16 but they represent only a tiny fraction of total weights. How to identify outliers is empirical.

![](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/LLM-int8.png)\
Two mixed-precision decompositions of \`LLM.int8()\`. (Image source: [Dettmers et al. 2022](https://arxiv.org/abs/2208.07339))

### Quantization at fine-grained granularity

![](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/quantization-granularity.png)\
Comparison of quantization at different granularity. $d$ is the model size / hidden state dimension and $h$ is the number of heads in one MHSA (multi-head self-attention) component.

Naively quantizing the entire weight matrix in one layer (“per-tensor” or “per-layer” quantization) is easiest to implement but does not lead to good granularity of quantization.

**Q-BERT** ([Shen, Dong & Ye, et al. 2020](https://arxiv.org/abs/1909.05840)) applied *group-wise quantization* to a fine-tuned BERT model, treating an individual matrix $W$ with respect to *each head* in MHSA (multi-head self-attention) as one group and then applies Hessian based mixed precision quantization.

*Per-embedding group (PEG)* activation quantization was motivated by the observation that outlier values only appear in a few out of $d$ (hidden state / model size) dimensions ([Bondarenko et al. 2021](https://arxiv.org/abs/2109.12948)). Per-embedding is pretty computationally expensive. In comparison, PEG quantization splits the activation tensor into several evenly sized groups along the embedding dimension where elements in the same group share quantization parameters. To ensure all outliers are grouped together, they apply a deterministic range-based permutation of embedding dimensions, where dimensions are sorted by their value ranges.

**ZeroQuant** ([Yao et al. 2022](https://arxiv.org/abs/2206.01861)) uses *group-wise quantization* for weights, same as in Q-BERT, and *token-wise quantization* for activation. To avoid expensive quantization and de-quantization computation, ZeroQuant built customized *kernel* to *fuse* quantization operation with its previous operator.

### Second order information for quantization

Q-BERT ([Shen, Dong & Ye, et al. 2020](https://arxiv.org/abs/1909.05840)) developed Hessian AWare Quantization (HAWQ) for its mixed-precision quantization. The motivation is that parameters with higher Hessian spectrum (i.e., larger top eigenvalues) are more sensitive to quantization and thus require higher precision. It is essentially a way to identify outliers.

In another viewpoint, the problem of quantization is an optimization problem. Given a weight matrix $\\mathbf{W}$ and an input matrix $\\mathbf{X}$ , we want to find a quantized weight matrix $\\hat{\\mathbf{W}}$ to minimize the MSE:

$$ \\hat{\\mathbf{W}}^\* = {\\arg\\min}\_{\\hat{\\mathbf{W}}} | \\mathbf{W}\\mathbf{X} - \\hat{\\mathbf{W}}\\mathbf{X}| $$

**GPTQ** ([Frantar et al. 2022](https://arxiv.org/abs/2210.17323)) treats the weight matrix $\\mathbf{W}$ as a collection of row vectors ${\\mathbf{w}}$ and applies quantization to each row independently. GPTQ iteratively quantizes more weights that are selected greedily to minimize the quantization error. The update on selected weights has a closed-form formula, utilizing Hessian matrices. Read more details in the paper and the OBQ (Optimal Brain Quantization; [Frantar & Alistarh 2022](https://arxiv.org/abs/2208.11580)) method if interested. GPTQ can reduce the bitwidth of weights in OPT-175B down to 3 or 4 bits without much performance loss, but it only applies to model weights not activation.

### Outlier smoothing

It is known that activations are harder to quantize than weights in transformer models. **SmoothQuant** ([Xiao & Lin 2022](https://arxiv.org/abs/2211.10438)) proposed a smart solution to smooth outlier features from activations to weights via mathematically equivalent transformation and then enable quantization on both weights and activations (`W8A8`). Because of this, SmoothQuant has better hardware efficiency than mixed-precision quantization.

![](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/SmoothQuant.png)

SmoothQuant migrates the scale variance from activations to weights offline to reduce the difficulty of activation quantization. Both the resulting new weight and activation matrices are easy to quantize. (Image source:
