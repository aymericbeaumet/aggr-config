---
title: Anatomize Deep Learning with Information Theory
link: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/
source: lilianweng-github-io
published: 2017-09-28T00:00:00Z
updated: 2017-09-28T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Professor Naftali Tishby passed away in 2021. Hope the post can introduce his cool idea of information bottleneck to more people. Recently I watched the talk “Information Theory in Deep Learning” by Prof Naftali Tishby and found it very interesting. He presented how to apply the information theory to study the growth and transformation of deep neural networks during training. Using the Information Bottleneck (IB) method, he proposed a new learning bound for deep neural networks (DNN), as the traditional learning theory fails due to the exponentially large number of parameters. Another keen observation is that DNN training involves two distinct phases: First, the network is trained to fully represent the input data and minimize the generalization error; then, it learns to forget the irrelevant details by compressing the representation of the input.'
content: extracted
html: 2017-09-28-anatomize-deep-learning-with-information-theory.html
preview:
  file: 2017-09-28-anatomize-deep-learning-with-information-theory.preview-5ea1105465c2.webp
  width: 256
  height: 162
  color: '#ececee'
images:
- source: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-dnn-structure.png
  original:
    file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-b24bcaf30964.png
    width: 1382
    height: 872
  variants:
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-34327973b0dd.webp
    width: 320
    height: 202
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-e1d1e725de8b.webp
    width: 640
    height: 404
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-91b65ef7f593.webp
    width: 960
    height: 606
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-3a333582c4c4.webp
    width: 1280
    height: 808
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-92e327966ab3.webp
    width: 1382
    height: 872
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-information-plane.png
  original:
    file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-970165b6c01d.png
    width: 1390
    height: 580
  variants:
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-c3980fbb6321.webp
    width: 320
    height: 134
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-3bc4fdb301b6.webp
    width: 640
    height: 267
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-a5d273e1be1c.webp
    width: 960
    height: 401
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-09a84c929e8f.webp
    width: 1280
    height: 534
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-7394cbafd4d2.webp
    width: 1390
    height: 580
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-information-plane-merged.png
  original:
    file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-2e0e01604f8c.png
    width: 1278
    height: 1008
  variants:
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-34fa101a79c9.webp
    width: 320
    height: 252
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-bea5a0792737.webp
    width: 640
    height: 505
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-024217e0a6db.webp
    width: 960
    height: 757
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-eedb6fffe2c1.webp
    width: 1278
    height: 1008
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-mean-variation.png
  original:
    file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-9d1b7caf9d72.png
    width: 1370
    height: 924
  variants:
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-680492b93d69.webp
    width: 320
    height: 216
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-54a00c5fc829.webp
    width: 640
    height: 432
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-b4173737aea5.webp
    width: 960
    height: 647
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-b915e5022416.webp
    width: 1280
    height: 863
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-0f3fc1c56a33.webp
    width: 1370
    height: 924
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-bound.png
  original:
    file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-ab1e94c2e0f2.png
    width: 742
    height: 480
  variants:
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-3ec81bee3eb3.webp
    width: 320
    height: 207
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-bc5f171d9e0a.webp
    width: 742
    height: 480
  color: '#e6e5e5'
- source: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-layers.png
  original:
    file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-24afe7b5d113.png
    width: 1324
    height: 772
  variants:
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-563e7d304ff9.webp
    width: 320
    height: 187
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-a6d44253395d.webp
    width: 640
    height: 373
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-f8fc3b12033b.webp
    width: 960
    height: 560
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-0abd4dc00640.webp
    width: 1280
    height: 746
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-12d01b871777.webp
    width: 1324
    height: 772
  color: '#fcfbfc'
- source: https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-training-size.png
  original:
    file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-8e9e09048a8f.png
    width: 994
    height: 820
  variants:
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-164eda015baf.webp
    width: 320
    height: 264
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-f0ec9a27b457.webp
    width: 640
    height: 528
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-8a75cb761ad7.webp
    width: 960
    height: 792
  - file: 2017-09-28-anatomize-deep-learning-with-information-theory.image-793830a929fe.webp
    width: 994
    height: 820
  color: '#fdfdfd'
---

Professor Naftali Tishby passed away in 2021. Hope the post can introduce his cool idea of information bottleneck to more people.

Recently I watched the talk [“Information Theory in Deep Learning”](https://youtu.be/bLqJHjXihK8) by Prof Naftali Tishby and found it very interesting. He presented how to apply the information theory to study the growth and transformation of deep neural networks during training. Using the [Information Bottleneck (IB)](https://arxiv.org/pdf/physics/0004057.pdf) method, he proposed a new learning bound for deep neural networks (DNN), as the traditional learning theory fails due to the exponentially large number of parameters. Another keen observation is that DNN training involves two distinct phases: First, the network is trained to fully represent the input data and minimize the generalization error; then, it learns to forget the irrelevant details by compressing the representation of the input.

Most of the materials in this post are from Prof Tishby’s talk and [related papers](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/#references).

## Basic Concepts

[**Markov Chain**](https://en.wikipedia.org/wiki/Markov_chain)

A Markov process is a [“memoryless”](http://mathworld.wolfram.com/Memoryless.html) (also called “Markov Property”) stochastic process. A Markov chain is a type of Markov process containing multiple discrete states. That is being said, the conditional probability of future states of the process is only determined by the current state and does not depend on the past states.

[**Kullback–Leibler (KL) Divergence**](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)

KL divergence measures how one probability distribution $p$ diverges from a second expected probability distribution $q$. It is asymmetric.

$$ \\begin{aligned} D\_{KL}(p \\| q) &= \\sum\_x p(x) \\log \\frac{p(x)}{q(x)} \\\\ &= - \\sum\_x p(x)\\log q(x) + \\sum\_x p(x)\\log p(x) \\\\ &= H(P, Q) - H(P) \\end{aligned} $$

$D\_{KL}$ achieves the minimum zero when $p(x)$ == $q(x)$ everywhere.

[**Mutual Information**](https://en.wikipedia.org/wiki/Mutual_information)

Mutual information measures the mutual dependence between two variables. It quantifies the “amount of information” obtained about one random variable through the other random variable. Mutual information is symmetric.

$$ \\begin{aligned} I(X;Y) &= D\_{KL}\[p(x,y) \\| p(x)p(y)\] \\\\ &= \\sum\_{x \\in X, y \\in Y} p(x, y) \\log(\\frac{p(x, y)}{p(x)p(y)}) \\\\ &= \\sum\_{x \\in X, y \\in Y} p(x, y) \\log(\\frac{p(x|y)}{p(x)}) \\\\ &= H(X) - H(X|Y) \\\\ \\end{aligned} $$

[**Data Processing Inequality (DPI)**](https://en.wikipedia.org/wiki/Data_processing_inequality)

For any markov chain: $X \\to Y \\to Z$, we would have $I(X; Y) \\geq I(X; Z)$.

A deep neural network can be viewed as a Markov chain, and thus when we are moving down the layers of a DNN, the mutual information between the layer and the input can only decrease.

[**Reparametrization invariance**](https://en.wikipedia.org/wiki/Parametrization#Parametrization_invariance)

For two invertible functions $\\phi$, $\\psi$, the mutual information still holds: $I(X; Y) = I(\\phi(X); \\psi(Y))$.

For example, if we shuffle the weights in one layer of DNN, it would not affect the mutual information between this layer and another.

## Deep Neural Networks as Markov Chains

The training data contains sampled observations from the joint distribution of $X$ and $Y$. The input variable $X$ and weights of hidden layers are all high-dimensional random variable. The ground truth target $Y$ and the predicted value $\\hat{Y}$ are random variables of smaller dimensions in the classification settings.

![](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-dnn-structure.png)\
The structure of a deep neural network, which consists of the target label $Y$, input layer $X$, hidden layers $h\\\_1, \\dots, h\\\_m$ and the final prediction $\\hat{Y}$. (Image source: [Tishby and Zaslavsky, 2015](https://arxiv.org/pdf/1503.02406.pdf))

If we label the hidden layers of a DNN as $h\_1, h\_2, \\dots, h\_m$ as in Fig. 1, we can view each layer as one state of a Markov Chain: $ h\_i \\to h\_{i+1}$. According to DPI, we would have:

$$ \\begin{aligned} H(X) \\geq I(X; h\_1) \\geq I(X; h\_2) \\geq \\dots \\geq I(X; h\_m) \\geq I(X; \\hat{Y}) \\\\ I(X; Y) \\geq I(h\_1; Y) \\geq I(h\_2; Y) \\geq \\dots \\geq I(h\_m; Y) \\geq I(\\hat{Y}; Y) \\end{aligned} $$

A DNN is designed to learn how to describe $X$ to predict $Y$ and eventually, to compress $X$ to only hold the information related to $Y$. Tishby describes this processing as *“successive refinement of relevant information”*.

## Information Plane Theorem

A DNN has successive internal representations of $X$, a set of hidden layers $\\{T\_i\\}$. The *information plane* theorem characterizes each layer by its encoder and decoder information. The encoder is a representation of the input data $X$, while the decoder translates the information in the current layer to the target ouput $Y$.

Precisely, in an information plane plot:

- **X-axis**: The sample complexity of $T\_i$ is determined by the encoder mutual information $I(X; T\_i)$. Sample complexity refers to how many samples you need to achieve certain accuracy and generalization.
- **Y-axis**: The accuracy (generalization error) is determined by the decoder mutual information $I(T\_i; Y)$.

![](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-information-plane.png)\
The encoder vs decoder mutual information of DNN hidden layers of 50 experiments. Different layers are color-coders, with green being the layer right next to the input and the orange being the furthest. There are three snapshots, at the initial epoch, 400 epochs and 9000 epochs respectively. (Image source: [Shwartz-Ziv and Tishby, 2017](https://arxiv.org/pdf/1703.00810.pdf))

Each dot in marks the encoder/ decoder mutual information of one hidden layer of one network simulation (no regularization is applied; no weights decay, no dropout, etc.). They move up as expected because the knowledge about the true labels is increasing (accuracy increases). At the early stage, the hidden layers learn a lot about the input $X$, but later they start to compress to forget some information about the input. Tishby believes that *“the most important part of learning is actually forgetting”*. Check out this [nice video](https://youtu.be/P1A1yNsxMjc) that demonstrates how the mutual information measures of layers are changing in epoch time.

![](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-information-plane-merged.png)\
Here is an aggregated view of Fig 2. The compression happens after the generalization error becomes very small. (Image source: [Tishby’ talk 15:15](https://youtu.be/bLqJHjXihK8?t=15m15s))

## Two Optimization Phases

Tracking the normalized mean and standard deviation of each layer’s weights in time also reveals two optimization phases of the training process.

![](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-mean-variation.png)\
The norm of mean and standard deviation of each layer's weight gradients for each layer as a function of training epochs. Different layers are color-coded. (Image source: [Shwartz-Ziv and Tishby, 2017](https://arxiv.org/pdf/1703.00810.pdf))

Among early epochs, the mean values are three magnitudes larger than the standard deviations. After a sufficient number of epochs, the error saturates and the standard deviations become much noisier afterward. The further a layer is away from the output, the noisier it gets, because the noises can get amplified and accumulated through the back-prop process (not due to the width of the layer).

## “Old” Generalization Bounds

The generalization bounds defined by the classic learning theory is:

$$ \\epsilon^2 < \\frac{\\log|H\_\\epsilon| + \\log{1/\\delta}}{2m} $$

- $\\epsilon$: The difference between the training error and the generalization error. The generalization error measures how accurate the prediction of an algorithm is for previously unseen data.
- $H\_\\epsilon$: $\\epsilon$-cover of the hypothesis class. Typically we assume the size $\\vert H\_\\epsilon \\vert \\sim (1/\\epsilon)^d$.
- $\\delta$: Confidence.
- $m$: The number of training samples.
- $d$: The VC dimension of the hypothesis.

This definition states that the difference between the training error and the generalization error is bounded by a function of the hypothesis space size and the dataset size. The bigger the hypothesis space gets, the bigger the generalization error becomes. I recommend this tutorial on ML theory, [part1](https://mostafa-samir.github.io/ml-theory-pt1/) and [part2](https://mostafa-samir.github.io/ml-theory-pt2/), if you are interested in reading more on generalization bounds.

However, it does not work for deep learning. The larger a network is, the more parameters it needs to learn. With this generalization bounds, larger networks (larger $d$) would have worse bounds. This is contrary to the intuition that larger networks are able to achieve better performance with higher expressivity.

## “New” Input compression bound

To solve this counterintuitive observation, Tishby et al. proposed a new input compression bound for DNN.

First let us have $T\_\\epsilon$ as an $\\epsilon$-partition of the input variable $X$. This partition compresses the input with respect to the homogeneity to the labels into small cells. The cells in total can cover the whole input space. If the prediction outputs binary values, we can replace the cardinality of the hypothesis, $\\vert H\_\\epsilon \\vert$, with $2^{\\vert T\_\\epsilon \\vert}$.

$$ |H\_\\epsilon| \\sim 2^{|X|} \\to 2^{|T\_\\epsilon|} $$

When $X$ is large, the size of $X$ is approximately $2^{H(X)}$. Each cell in the $\\epsilon$-partition is of size $2^{H(X \\vert T\_\\epsilon)}$. Therefore we have $\\vert T\_\\epsilon \\vert \\sim \\frac{2^{H(X)}}{2^{H(X \\vert T\_\\epsilon)}} = 2^{I(T\_\\epsilon; X)}$. Then the input compression bound becomes:

$$ \\epsilon^2 < \\frac{2^{I(T\_\\epsilon; X)} + \\log{1/\\delta}}{2m} $$

![](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-bound.png)\
The black line is the optimal achievable information bottleneck (IB) limit. The red line corresponds to the upper bound on the out-of-sample IB distortion, when trained on a finite sample set. $\\Delta C$ is the complexity gap and $\\Delta G$ is the generalization gap. (Recreated based on [Tishby’ talk 24:50](https://youtu.be/bLqJHjXihK8?t=24m56s))

## Network Size and Training Data Size

Having more layers give us computational benefits and speed up the training process for good generalization.

![](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-layers.png)\
The optimization time is much shorter (fewer epochs) with more hidden layers. (Image source: [Shwartz-Ziv and Tishby, 2017](https://arxiv.org/pdf/1703.00810.pdf))

**Compression through stochastic relaxation**: According to the [diffusion equation](https://en.wikipedia.org/wiki/Fokker%E2%80%93Planck_equation), the relaxation time of layer $k$ is proportional to the exponential of this layer’s compression amount $\\Delta S\_k$: $\\Delta t\_k \\sim \\exp(\\Delta S\_k)$. We can compute the layer compression as $\\Delta S\_k = I(X; T\_k) - I(X; T\_{k-1})$. Because $\\exp(\\sum\_k \\Delta S\_k) \\geq \\sum\_k \\exp(\\Delta S\_k)$, we would expect an exponential decrease in training epochs with more hidden layers (larger $k$).

## The Benefit of More Training Samples

Fitting more training data requires more information captured by the hidden layers. With increased training data size, the decoder mutual information (recall that this is directly related to the generalization error), $I(T; Y)$, is pushed up and gets closer to the theoretical information bottleneck bound. Tishby emphasized that It is the mutual information, not the layer size or the VC dimension, that determines generalization, different from standard theories.

![](https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/ib-training-size.png)\
The training data of different sizes is color-coded. The information plane of multiple converged networks are plotted. More training data leads to better generalization. (Image source: [Shwartz-Ziv and Tishby, 2017](https://arxiv.org/pdf/1703.00810.pdf))

* * *

Cited as:

```
@article{weng2017infotheory,
  title   = "Anatomize Deep Learning with Information Theory",
  author  = "Weng, Lilian",
  journal = "lilianweng.github.io",
  year    = "2017",
  url     = "https://lilianweng.github.io/posts/2017-09-28-information-bottleneck/"
}
```

## References

\[1\] Naftali Tishby. [Information Theory of Deep Learning](https://youtu.be/bLqJHjXihK8)

\[2\] [Machine Learning Theory - Part 1: Introduction](https://mostafa-samir.github.io/ml-theory-pt1/)

\[3\] [Machine Learning Theory - Part 2: Generalization Bounds](https://mostafa-samir.github.io/ml-theory-pt2/)

\[4\] [New Theory Cracks Open the Black Box of Deep Learning](https://www.quantamagazine.org/new-theory-cracks-open-the-black-box-of-deep-learning-20170921/) by Quanta Magazine.

\[5\] Naftali Tishby and Noga Zaslavsky. [“Deep learning and the information bottleneck principle.”](https://arxiv.org/pdf/1503.02406.pdf) IEEE Information Theory Workshop (ITW), 2015.

\[6\] Ravid Shwartz-Ziv and Naftali Tishby. [“Opening the Black Box of Deep Neural Networks via Information.”](https://arxiv.org/pdf/1703.00810.pdf) arXiv preprint arXiv:1703.00810, 2017.
