---
title: Evolution Strategies
link: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/
source: lilianweng-github-io
published: 2019-09-05T00:00:00Z
updated: 2019-09-05T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Stochastic gradient descent is a universal choice for optimizing deep learning models. However, it is not the only option. With black-box optimization algorithms, you can evaluate a target function $f(x): \mathbb{R}^n \to \mathbb{R}$, even when you don’t know the precise analytic form of $f(x)$ and thus cannot compute gradients or the Hessian matrix. Examples of black-box optimization methods include Simulated Annealing, Hill Climbing and Nelder-Mead method.'
content: extracted
html: 2019-09-05-evolution-strategies.html
preview:
  file: 2019-09-05-evolution-strategies.preview-f9aada7f9d00.webp
  width: 256
  height: 148
  color: '#3c4041'
images:
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/EA-illustration.png
  original:
    file: 2019-09-05-evolution-strategies.image-d25ba7304fcd.png
    width: 1217
    height: 702
  variants:
  - file: 2019-09-05-evolution-strategies.image-1defd11f5e92.webp
    width: 320
    height: 185
  - file: 2019-09-05-evolution-strategies.image-594bf04b70e2.webp
    width: 640
    height: 369
  - file: 2019-09-05-evolution-strategies.image-e36c68115739.webp
    width: 1217
    height: 702
  color: '#545554'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-step-size-path.png
  original:
    file: 2019-09-05-evolution-strategies.image-0cdf581fbde2.png
    width: 1999
    height: 952
  variants:
  - file: 2019-09-05-evolution-strategies.image-37aa8513b01c.webp
    width: 320
    height: 152
  - file: 2019-09-05-evolution-strategies.image-53fc06948d6e.webp
    width: 640
    height: 305
  - file: 2019-09-05-evolution-strategies.image-4f264971bed9.webp
    width: 960
    height: 457
  - file: 2019-09-05-evolution-strategies.image-399754793ded.webp
    width: 1280
    height: 610
  - file: 2019-09-05-evolution-strategies.image-96c0970c3a02.webp
    width: 1600
    height: 762
  - file: 2019-09-05-evolution-strategies.image-57b8d5f5f0e8.webp
    width: 1999
    height: 952
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-algorithm.png
  original:
    file: 2019-09-05-evolution-strategies.image-d1bd8cf5285c.png
    width: 1982
    height: 1160
  variants:
  - file: 2019-09-05-evolution-strategies.image-ad8451a9b475.webp
    width: 320
    height: 187
  - file: 2019-09-05-evolution-strategies.image-c3f99552ce5a.webp
    width: 640
    height: 375
  - file: 2019-09-05-evolution-strategies.image-efd0a9768769.webp
    width: 960
    height: 562
  - file: 2019-09-05-evolution-strategies.image-8529f126f1cc.webp
    width: 1280
    height: 749
  - file: 2019-09-05-evolution-strategies.image-c9b11495ea66.webp
    width: 1600
    height: 936
  - file: 2019-09-05-evolution-strategies.image-ca052beeae72.webp
    width: 1982
    height: 1160
  color: '#fbfbfb'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-illustration.png
  original:
    file: 2019-09-05-evolution-strategies.image-b3d10cf739d8.png
    width: 814
    height: 556
  variants:
  - file: 2019-09-05-evolution-strategies.image-968cf49bb082.webp
    width: 320
    height: 219
  - file: 2019-09-05-evolution-strategies.image-1f5cdb9aa6f4.webp
    width: 640
    height: 437
  - file: 2019-09-05-evolution-strategies.image-3e0cf721e440.webp
    width: 814
    height: 556
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-coordinates.png
  original:
    file: 2019-09-05-evolution-strategies.image-9039899d389d.png
    width: 1999
    height: 912
  variants:
  - file: 2019-09-05-evolution-strategies.image-a1ba0cdc0716.webp
    width: 320
    height: 146
  - file: 2019-09-05-evolution-strategies.image-8e8e22e69ec7.webp
    width: 640
    height: 292
  - file: 2019-09-05-evolution-strategies.image-d5a46fe3e6e4.webp
    width: 960
    height: 438
  - file: 2019-09-05-evolution-strategies.image-bd83a7f7fa94.webp
    width: 1280
    height: 584
  - file: 2019-09-05-evolution-strategies.image-3f7db1f2516e.webp
    width: 1600
    height: 730
  - file: 2019-09-05-evolution-strategies.image-c91a927f546d.webp
    width: 1999
    height: 912
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/NES-algorithm.png
  original:
    file: 2019-09-05-evolution-strategies.image-d630e35cb72e.png
    width: 1812
    height: 1198
  variants:
  - file: 2019-09-05-evolution-strategies.image-81ef1e8fe7cc.webp
    width: 320
    height: 212
  - file: 2019-09-05-evolution-strategies.image-67c8a2516c3b.webp
    width: 640
    height: 423
  - file: 2019-09-05-evolution-strategies.image-7b8dc61ec45c.webp
    width: 960
    height: 635
  - file: 2019-09-05-evolution-strategies.image-33ecfaee701e.webp
    width: 1280
    height: 846
  - file: 2019-09-05-evolution-strategies.image-494ee0f80169.webp
    width: 1600
    height: 1058
  - file: 2019-09-05-evolution-strategies.image-461ac882e579.webp
    width: 1812
    height: 1198
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/OpenAI-ES-algorithm.png
  original:
    file: 2019-09-05-evolution-strategies.image-e35a19aed429.png
    width: 1420
    height: 616
  variants:
  - file: 2019-09-05-evolution-strategies.image-aa918cff71b8.webp
    width: 320
    height: 139
  - file: 2019-09-05-evolution-strategies.image-dc3758e51465.webp
    width: 640
    height: 278
  - file: 2019-09-05-evolution-strategies.image-4d77b7284b44.webp
    width: 960
    height: 416
  - file: 2019-09-05-evolution-strategies.image-632bbf0beb0e.webp
    width: 1280
    height: 555
  - file: 2019-09-05-evolution-strategies.image-c4f26217c37e.webp
    width: 1420
    height: 616
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/NS-ES-experiments.png
  original:
    file: 2019-09-05-evolution-strategies.image-527713b2e2ab.png
    width: 2188
    height: 804
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CEM-RL.png
  original:
    file: 2019-09-05-evolution-strategies.image-7ff96e4c6381.png
    width: 1999
    height: 913
  variants:
  - file: 2019-09-05-evolution-strategies.image-f26e222a3172.webp
    width: 320
    height: 146
  - file: 2019-09-05-evolution-strategies.image-a84a8cdeb4fc.webp
    width: 640
    height: 292
  - file: 2019-09-05-evolution-strategies.image-dcf2658dbaa4.webp
    width: 960
    height: 438
  - file: 2019-09-05-evolution-strategies.image-560e8c95c84c.webp
    width: 1280
    height: 585
  - file: 2019-09-05-evolution-strategies.image-5b6e2e3da1cd.webp
    width: 1600
    height: 731
  - file: 2019-09-05-evolution-strategies.image-f08f0c055474.webp
    width: 1999
    height: 913
  color: '#fafbfb'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/PBT.png
  original:
    file: 2019-09-05-evolution-strategies.image-5f5c7dc473f8.png
    width: 1999
    height: 990
  variants:
  - file: 2019-09-05-evolution-strategies.image-a94a749c7c57.webp
    width: 320
    height: 158
  - file: 2019-09-05-evolution-strategies.image-9839eadf662e.webp
    width: 640
    height: 317
  - file: 2019-09-05-evolution-strategies.image-f5f2e0ac57a9.webp
    width: 960
    height: 475
  - file: 2019-09-05-evolution-strategies.image-e140223687c3.webp
    width: 1280
    height: 634
  - file: 2019-09-05-evolution-strategies.image-2389fc3e434f.webp
    width: 1600
    height: 792
  - file: 2019-09-05-evolution-strategies.image-c122503c7977.webp
    width: 1999
    height: 990
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/PBT-algorithm.png
  original:
    file: 2019-09-05-evolution-strategies.image-c374577657c5.png
    width: 1999
    height: 962
  variants:
  - file: 2019-09-05-evolution-strategies.image-efc7d3dea733.webp
    width: 320
    height: 154
  - file: 2019-09-05-evolution-strategies.image-0c29a4c0540c.webp
    width: 640
    height: 308
  - file: 2019-09-05-evolution-strategies.image-b516e1c59de9.webp
    width: 960
    height: 462
  - file: 2019-09-05-evolution-strategies.image-3a860ea8cdee.webp
    width: 1280
    height: 616
  - file: 2019-09-05-evolution-strategies.image-e1aab1fb8371.webp
    width: 1600
    height: 770
  - file: 2019-09-05-evolution-strategies.image-afbdedb464b3.webp
    width: 1999
    height: 962
  color: '#fbfbfb'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/WANN-mutations.png
  original:
    file: 2019-09-05-evolution-strategies.image-7fa588535e3e.png
    width: 1422
    height: 320
  variants:
  - file: 2019-09-05-evolution-strategies.image-c8ad72d6dea8.webp
    width: 320
    height: 72
  - file: 2019-09-05-evolution-strategies.image-c8489e6697fe.webp
    width: 640
    height: 144
  - file: 2019-09-05-evolution-strategies.image-b0fc3cfee3f7.webp
    width: 960
    height: 216
  - file: 2019-09-05-evolution-strategies.image-4a4ec36351a1.webp
    width: 1280
    height: 288
  - file: 2019-09-05-evolution-strategies.image-7a248ebe6b6e.webp
    width: 1422
    height: 320
  color: '#fafafa'
- source: https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/WANN-results.png
  original:
    file: 2019-09-05-evolution-strategies.image-7fac7d0b2a67.png
    width: 1999
    height: 544
  variants:
  - file: 2019-09-05-evolution-strategies.image-682c5c4a9aeb.webp
    width: 320
    height: 87
  - file: 2019-09-05-evolution-strategies.image-c4ea69b39560.webp
    width: 640
    height: 174
  - file: 2019-09-05-evolution-strategies.image-7d02e0148b64.webp
    width: 960
    height: 261
  - file: 2019-09-05-evolution-strategies.image-fd0aba05b431.webp
    width: 1280
    height: 348
  - file: 2019-09-05-evolution-strategies.image-7cca25119651.webp
    width: 1600
    height: 435
  - file: 2019-09-05-evolution-strategies.image-5d18b6b2315e.webp
    width: 1999
    height: 544
  color: '#e7e7e7'
---

Stochastic gradient descent is a universal choice for optimizing deep learning models. However, it is not the only option. With black-box optimization algorithms, you can evaluate a target function $f(x): \\mathbb{R}^n \\to \\mathbb{R}$, even when you don’t know the precise analytic form of $f(x)$ and thus cannot compute gradients or the Hessian matrix. Examples of black-box optimization methods include [Simulated Annealing](https://en.wikipedia.org/wiki/Simulated_annealing), [Hill Climbing](https://en.wikipedia.org/wiki/Hill_climbing) and [Nelder-Mead method](https://en.wikipedia.org/wiki/Nelder%E2%80%93Mead_method).

**Evolution Strategies (ES)** is one type of black-box optimization algorithms, born in the family of **Evolutionary Algorithms (EA)**. In this post, I would dive into a couple of classic ES methods and introduce a few applications of how ES can play a role in deep reinforcement learning.

## What are Evolution Strategies?

Evolution strategies (ES) belong to the big family of evolutionary algorithms. The optimization targets of ES are vectors of real numbers, $x \\in \\mathbb{R}^n$.

Evolutionary algorithms refer to a division of population-based optimization algorithms inspired by *natural selection*. Natural selection believes that individuals with traits beneficial to their survival can live through generations and pass down the good characteristics to the next generation. Evolution happens by the selection process gradually and the population grows better adapted to the environment.

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/EA-illustration.png)\
How natural selection works. (Image source: Khan Academy: [Darwin, evolution, & natural selection](https://www.khanacademy.org/science/biology/her/evolution-and-natural-selection/a/darwin-evolution-natural-selection))

Evolutionary algorithms can be summarized in the following [format](https://ipvs.informatik.uni-stuttgart.de/mlr/marc/teaching/13-Optimization/06-blackBoxOpt.pdf) as a general optimization solution:

Let’s say we want to optimize a function $f(x)$ and we are not able to compute gradients directly. But we still can evaluate $f(x)$ given any $x$ and the result is deterministic. Our belief in the probability distribution over $x$ as a good solution to $f(x)$ optimization is $p\_\\theta(x)$, parameterized by $\\theta$. The goal is to find an optimal configuration of $\\theta$.

> Here given a fixed format of distribution (i.e. Gaussian), the parameter $\\theta$ carries the knowledge about the best solutions and is being iteratively updated across generations.

Starting with an initial value of $\\theta$, we can continuously update $\\theta$ by looping three steps as follows:

1. Generate a population of samples $D = \\{(x\_i, f(x\_i)\\}$ where $x\_i \\sim p\_\\theta(x)$.
2. Evaluate the “fitness” of samples in $D$.
3. Select the best subset of individuals and use them to update $\\theta$, generally based on fitness or rank.

In **Genetic Algorithms (GA)**, another popular subcategory of EA, $x$ is a sequence of binary codes, $x \\in \\{0, 1\\}^n$. While in ES, $x$ is just a vector of real numbers, $x \\in \\mathbb{R}^n$.

## Simple Gaussian Evolution Strategies

[This](http://blog.otoro.net/2017/10/29/visual-evolution-strategies/) is the most basic and canonical version of evolution strategies. It models $p\_\\theta(x)$ as a $n$-dimensional isotropic Gaussian distribution, in which $\\theta$ only tracks the mean $\\mu$ and standard deviation $\\sigma$.

$$ \\theta = (\\mu, \\sigma),\\;p\_\\theta(x) \\sim \\mathcal{N}(\\mathbf{\\mu}, \\sigma^2 I) = \\mu + \\sigma \\mathcal{N}(0, I) $$

The process of Simple-Gaussian-ES, given $x \\in \\mathcal{R}^n$:

1. Initialize $\\theta = \\theta^{(0)}$ and the generation counter $t=0$
2. Generate the offspring population of size $\\Lambda$ by sampling from the Gaussian distribution:

   $D^{(t+1)}=\\{ x^{(t+1)}\_i \\mid x^{(t+1)}\_i = \\mu^{(t)} + \\sigma^{(t)} y^{(t+1)}\_i \\text{ where } y^{(t+1)}\_i \\sim \\mathcal{N}(x \\vert 0, \\mathbf{I}),;i = 1, \\dots, \\Lambda\\}$\
   .

3. Select a top subset of $\\lambda$ samples with optimal $f(x\_i)$ and this subset is called **elite** set. Without loss of generality, we may consider the first $k$ samples in $D^{(t+1)}$ to belong to the elite group — Let’s label them as

$$ D^{(t+1)}\\\_\\text{elite} = \\\\{x^{(t+1)}\\\_i \\mid x^{(t+1)}\\\_i \\in D^{(t+1)}, i=1,\\dots, \\lambda, \\lambda\\leq \\Lambda\\\\} $$

4. Then we estimate the new mean and std for the next generation using the elite set:

$$ \\begin{aligned} \\mu^{(t+1)} &= \\text{avg}(D^{(t+1)}\_\\text{elite}) = \\frac{1}{\\lambda}\\sum\_{i=1}^\\lambda x\_i^{(t+1)} \\\\ {\\sigma^{(t+1)}}^2 &= \\text{var}(D^{(t+1)}\_\\text{elite}) = \\frac{1}{\\lambda}\\sum\_{i=1}^\\lambda (x\_i^{(t+1)} -\\mu^{(t)})^2 \\end{aligned} $$

5. Repeat steps (2)-(4) until the result is good enough ✌️

## Covariance Matrix Adaptation Evolution Strategies (CMA-ES)

The standard deviation $\\sigma$ accounts for the level of exploration: the larger $\\sigma$ the bigger search space we can sample our offspring population. In [vanilla ES](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/#simple-gaussian-evolution-strategies), $\\sigma^{(t+1)}$ is highly correlated with $\\sigma^{(t)}$, so the algorithm is not able to rapidly adjust the exploration space when needed (i.e. when the confidence level changes).

[**CMA-ES**](https://en.wikipedia.org/wiki/CMA-ES), short for *“Covariance Matrix Adaptation Evolution Strategy”*, fixes the problem by tracking pairwise dependencies between the samples in the distribution with a covariance matrix $C$. The new distribution parameter becomes:

$$ \\theta = (\\mu, \\sigma, C),\\; p\_\\theta(x) \\sim \\mathcal{N}(\\mu, \\sigma^2 C) \\sim \\mu + \\sigma \\mathcal{N}(0, C) $$

where $\\sigma$ controls for the overall scale of the distribution, often known as *step size*.

Before we dig into how the parameters are updated in CMA-ES, it is better to review how the covariance matrix works in the multivariate Gaussian distribution first. As a real symmetric matrix, the covariance matrix $C$ has the following nice features (See [proof](http://s3.amazonaws.com/mitsloan-php/wp-faculty/sites/30/2016/12/15032137/Symmetric-Matrices-and-Eigendecomposition.pdf) & [proof](http://control.ucsd.edu/mauricio/courses/mae280a/lecture11.pdf)):

- It is always diagonalizable.
- Always positive semi-definite.
- All of its eigenvalues are real non-negative numbers.
- All of its eigenvectors are orthogonal.
- There is an orthonormal basis of $\\mathbb{R}^n$ consisting of its eigenvectors.

Let the matrix $C$ have an *orthonormal* basis of eigenvectors $B = \[b\_1, \\dots, b\_n\]$, with corresponding eigenvalues $\\lambda\_1^2, \\dots, \\lambda\_n^2$. Let $D=\\text{diag}(\\lambda\_1, \\dots, \\lambda\_n)$.

$$ C = B^\\top D^2 B = \\begin{bmatrix} \\mid & \\mid & & \\mid \\\\ b\_1 & b\_2 & \\dots & b\_n\\\\ \\mid & \\mid & & \\mid \\\\ \\end{bmatrix} \\begin{bmatrix} \\lambda\_1^2 & 0 & \\dots & 0 \\\\ 0 & \\lambda\_2^2 & \\dots & 0 \\\\ \\vdots & \\dots & \\ddots & \\vdots \\\\ 0 & \\dots & 0 & \\lambda\_n^2 \\end{bmatrix} \\begin{bmatrix} - & b\_1 & - \\\\ - & b\_2 & - \\\\ & \\dots & \\\\ - & b\_n & - \\\\ \\end{bmatrix} $$

The square root of $C$ is:

$$ C^{\\frac{1}{2}} = B^\\top D B $$

| Symbol                          | Meaning                                                   |
| ------------------------------- | --------------------------------------------------------- |
| $x\_i^{(t)} \\in \\mathbb{R}^n$ | the $i$-th samples at the generation (t)                  |
| $y\_i^{(t)} \\in \\mathbb{R}^n$ | $x\_i^{(t)} = \\mu^{(t-1)} + \\sigma^{(t-1)} y\_i^{(t)} $ |
| $\\mu^{(t)}$                    | mean of the generation (t)                                |
| $\\sigma^{(t)}$                 | step size                                                 |
| $C^{(t)}$                       | covariance matrix                                         |
| $B^{(t)}$                       | a matrix of $C$’s eigenvectors as row vectors             |
| $D^{(t)}$                       | a diagonal matrix with $C$’s eigenvalues on the diagnose. |
| $p\_\\sigma^{(t)}$              | evaluation path for $\\sigma$ at the generation (t)       |
| $p\_c^{(t)}$                    | evaluation path for $C$ at the generation (t)             |
| $\\alpha\_\\mu$                 | learning rate for $\\mu$’s update                         |
| $\\alpha\_\\sigma$              | learning rate for $p\_\\sigma$                            |
| $d\_\\sigma$                    | damping factor for $\\sigma$’s update                     |
| $\\alpha\_{cp}$                 | learning rate for $p\_c$                                  |
| $\\alpha\_{c\\lambda}$          | learning rate for $C$’s rank-min(λ, n) update             |
| $\\alpha\_{c1}$                 | learning rate for $C$’s rank-1 update                     |

## Updating the Mean

$$ \\mu^{(t+1)} = \\mu^{(t)} + \\alpha\_\\mu \\frac{1}{\\lambda}\\sum\_{i=1}^\\lambda (x\_i^{(t+1)} - \\mu^{(t)}) $$

CMA-ES has a learning rate $\\alpha\_\\mu \\leq 1$ to control how fast the mean $\\mu$ should be updated. Usually it is set to 1 and thus the equation becomes the same as in vanilla ES, $\\mu^{(t+1)} = \\frac{1}{\\lambda}\\sum\_{i=1}^\\lambda (x\_i^{(t+1)}$.

## Controlling the Step Size

The sampling process can be decoupled from the mean and standard deviation:

$$ x^{(t+1)}\_i = \\mu^{(t)} + \\sigma^{(t)} y^{(t+1)}\_i \\text{, where } y^{(t+1)}\_i = \\frac{x\_i^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\sim \\mathcal{N}(0, C) $$

The parameter $\\sigma$ controls the overall scale of the distribution. It is separated from the covariance matrix so that we can change steps faster than the full covariance. A larger step size leads to faster parameter update. In order to evaluate whether the current step size is proper, CMA-ES constructs an *evolution path* $p\_\\sigma$ by summing up a consecutive sequence of moving steps, $\\frac{1}{\\lambda}\\sum\_{i}^\\lambda y\_i^{(j)}, j=1, \\dots, t$. By comparing this path length with its expected length under random selection (meaning single steps are uncorrelated), we are able to adjust $\\sigma$ accordingly (See Fig. 2).

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-step-size-path.png)\
Three scenarios of how single steps are correlated in different ways and their impacts on step size update. (Image source: additional annotations on Fig 5 in [CMA-ES tutorial](https://arxiv.org/abs/1604.00772) paper)

Each time the evolution path is updated with the average of moving step $y\_i$ in the same generation.

$$ \\begin{aligned} &\\frac{1}{\\lambda}\\sum\_{i=1}^\\lambda y\_i^{(t+1)} = \\frac{1}{\\lambda} \\frac{\\sum\_{i=1}^\\lambda x\_i^{(t+1)} - \\lambda \\mu^{(t)}}{\\sigma^{(t)}} = \\frac{\\mu^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\\\ &\\frac{1}{\\lambda}\\sum\_{i=1}^\\lambda y\_i^{(t+1)} \\sim \\frac{1}{\\lambda}\\mathcal{N}(0, \\lambda C^{(t)}) \\sim \\frac{1}{\\sqrt{\\lambda}}{C^{(t)}}^{\\frac{1}{2}}\\mathcal{N}(0, I) \\\\ &\\text{Thus } \\sqrt{\\lambda}\\;{C^{(t)}}^{-\\frac{1}{2}} \\frac{\\mu^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\sim \\mathcal{N}(0, I) \\end{aligned} $$

> By multiplying with $C^{-\\frac{1}{2}}$, the evolution path is transformed to be independent of its direction. The term ${C^{(t)}}^{-\\frac{1}{2}} = {B^{(t)}}^\\top {D^{(t)}}^{-\\frac{1}{2}} {B^{(t)}}$ transformation works as follows:

1. ${B^{(t)}}$ contains row vectors of $C$’s eigenvectors. It projects the original space onto the perpendicular principal axes.
2. Then ${D^{(t)}}^{-\\frac{1}{2}} = \\text{diag}(\\frac{1}{\\lambda\_1}, \\dots, \\frac{1}{\\lambda\_n})$ scales the length of principal axes to be equal.
3. ${B^{(t)}}^\\top$ transforms the space back to the original coordinate system.

In order to assign higher weights to recent generations, we use polyak averaging to update the evolution path with learning rate $\\alpha\_\\sigma$. Meanwhile, the weights are balanced so that $p\_\\sigma$ is [conjugate](https://en.wikipedia.org/wiki/Conjugate_prior), $\\sim \\mathcal{N}(0, I)$ both before and after one update.

$$ \\begin{aligned} p\_\\sigma^{(t+1)} & = (1 - \\alpha\_\\sigma) p\_\\sigma^{(t)} + \\sqrt{1 - (1 - \\alpha\_\\sigma)^2}\\;\\sqrt{\\lambda}\\; {C^{(t)}}^{-\\frac{1}{2}} \\frac{\\mu^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\\\ & = (1 - \\alpha\_\\sigma) p\_\\sigma^{(t)} + \\sqrt{c\_\\sigma (2 - \\alpha\_\\sigma)\\lambda}\\;{C^{(t)}}^{-\\frac{1}{2}} \\frac{\\mu^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\end{aligned} $$

The expected length of $p\_\\sigma$ under random selection is $\\mathbb{E}|\\mathcal{N}(0,I)|$, that is the expectation of the L2-norm of a $\\mathcal{N}(0,I)$ random variable. Following the idea in Fig. 2, we adjust the step size according to the ratio of $|p\_\\sigma^{(t+1)}| / \\mathbb{E}|\\mathcal{N}(0,I)|$:

$$ \\begin{aligned} \\ln\\sigma^{(t+1)} &= \\ln\\sigma^{(t)} + \\frac{\\alpha\_\\sigma}{d\_\\sigma} \\Big(\\frac{\\|p\_\\sigma^{(t+1)}\\|}{\\mathbb{E}\\|\\mathcal{N}(0,I)\\|} - 1\\Big) \\\\ \\sigma^{(t+1)} &= \\sigma^{(t)} \\exp\\Big(\\frac{\\alpha\_\\sigma}{d\_\\sigma} \\Big(\\frac{\\|p\_\\sigma^{(t+1)}\\|}{\\mathbb{E}\\|\\mathcal{N}(0,I)\\|} - 1\\Big)\\Big) \\end{aligned} $$

where $d\_\\sigma \\approx 1$ is a damping parameter, scaling how fast $\\ln\\sigma$ should be changed.

## Adapting the Covariance Matrix

For the covariance matrix, it can be estimated from scratch using $y\_i$ of elite samples (recall that $y\_i \\sim \\mathcal{N}(0, C)$):

$$ C\_\\lambda^{(t+1)} = \\frac{1}{\\lambda}\\sum\_{i=1}^\\lambda y^{(t+1)}\_i {y^{(t+1)}\_i}^\\top = \\frac{1}{\\lambda {\\sigma^{(t)}}^2} \\sum\_{i=1}^\\lambda (x\_i^{(t+1)} - \\mu^{(t)})(x\_i^{(t+1)} - \\mu^{(t)})^\\top $$

The above estimation is only reliable when the selected population is large enough. However, we do want to run *fast* iteration with a *small* population of samples in each generation. That’s why CMA-ES invented a more reliable but also more complicated way to update $C$. It involves two independent routes,

- *Rank-min(λ, n) update*: uses the history of $\\{C\_\\lambda\\}$, each estimated from scratch in one generation.
- *Rank-one update*: estimates the moving steps $y\_i$ and the sign information from the history.

The first route considers the estimation of $C$ from the entire history of $\\{C\_\\lambda\\}$. For example, if we have experienced a large number of generations, $C^{(t+1)} \\approx \\text{avg}(C\_\\lambda^{(i)}; i=1,\\dots,t)$ would be a good estimator. Similar to $p\_\\sigma$, we also use polyak averaging with a learning rate to incorporate the history:

$$ C^{(t+1)} = (1 - \\alpha\_{c\\lambda}) C^{(t)} + \\alpha\_{c\\lambda} C\_\\lambda^{(t+1)} = (1 - \\alpha\_{c\\lambda}) C^{(t)} + \\alpha\_{c\\lambda} \\frac{1}{\\lambda} \\sum\_{i=1}^\\lambda y^{(t+1)}\_i {y^{(t+1)}\_i}^\\top $$

A common choice for the learning rate is $\\alpha\_{c\\lambda} \\approx \\min(1, \\lambda/n^2)$.

The second route tries to solve the issue that $y\_i{y\_i}^\\top = (-y\_i)(-y\_i)^\\top$ loses the sign information. Similar to how we adjust the step size $\\sigma$, an evolution path $p\_c$ is used to track the sign information and it is constructed in a way that $p\_c$ is conjugate, $\\sim \\mathcal{N}(0, C)$ both before and after a new generation.

We may consider $p\_c$ as another way to compute $\\text{avg}\_i(y\_i)$ (notice that both $\\sim \\mathcal{N}(0, C)$) while the entire history is used and the sign information is maintained. Note that we’ve known $\\sqrt{k}\\frac{\\mu^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\sim \\mathcal{N}(0, C)$ in the [last section](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/#controlling-the-step-size),

$$ \\begin{aligned} p\_c^{(t+1)} &= (1-\\alpha\_{cp}) p\_c^{(t)} + \\sqrt{1 - (1-\\alpha\_{cp})^2}\\;\\sqrt{\\lambda}\\;\\frac{\\mu^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\\\ &= (1-\\alpha\_{cp}) p\_c^{(t)} + \\sqrt{\\alpha\_{cp}(2 - \\alpha\_{cp})\\lambda}\\;\\frac{\\mu^{(t+1)} - \\mu^{(t)}}{\\sigma^{(t)}} \\end{aligned} $$

Then the covariance matrix is updated according to $p\_c$:

$$ C^{(t+1)} = (1-\\alpha\_{c1}) C^{(t)} + \\alpha\_{c1}\\;p\_c^{(t+1)} {p\_c^{(t+1)}}^\\top $$

The *rank-one update* approach is claimed to generate a significant improvement over the *rank-min(λ, n)-update* when $k$ is small, because the signs of moving steps and correlations between consecutive steps are all utilized and passed down through generations.

Eventually we combine two approaches together,

$$ C^{(t+1)} = (1 - \\alpha\_{c\\lambda} - \\alpha\_{c1}) C^{(t)} + \\alpha\_{c1}\\;\\underbrace{p\_c^{(t+1)} {p\_c^{(t+1)}}^\\top}\_\\textrm{rank-one update} + \\alpha\_{c\\lambda} \\underbrace{\\frac{1}{\\lambda} \\sum\_{i=1}^\\lambda y^{(t+1)}\_i {y^{(t+1)}\_i}^\\top}\_\\textrm{rank-min(lambda, n) update} $$

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-algorithm.png)

In all my examples above, each elite sample is considered to contribute an equal amount of weights, $1/\\lambda$. The process can be easily extended to the case where selected samples are assigned with different weights, $w\_1, \\dots, w\_\\lambda$, according to their performances. See more detail in [tutorial](https://arxiv.org/abs/1604.00772).

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-illustration.png)\
Illustration of how CMA-ES works on a 2D optimization problem (the lighter color the better). Black dots are samples in one generation. The samples are more spread out initially but when the model has higher confidence in finding a good solution in the late stage, the samples become very concentrated over the global optimum. (Image source: [Wikipedia CMA-ES](https://en.wikipedia.org/wiki/CMA-ES))

## Natural Evolution Strategies

Natural Evolution Strategies (**NES**; [Wierstra, et al, 2008](https://arxiv.org/abs/1106.4487)) optimizes in a search distribution of parameters and moves the distribution in the direction of high fitness indicated by the *natural gradient*.

## Natural Gradients

Given an objective function $\\mathcal{J}(\\theta)$ parameterized by $\\theta$, let’s say our goal is to find the optimal $\\theta$ to maximize the objective function value. A *plain gradient* finds the steepest direction within a small Euclidean distance from the current $\\theta$; the distance restriction is applied on the parameter space. In other words, we compute the plain gradient with respect to a small change of the absolute value of $\\theta$. The optimal step is:

$$ d^{\*} = \\operatorname\*{argmax}\_{\\|d\\| = \\epsilon} \\mathcal{J}(\\theta + d)\\text{, where }\\epsilon \\to 0 $$

Differently, *natural gradient* works with a probability [distribution](https://arxiv.org/abs/1301.3584v7) [space](https://wiseodd.github.io/techblog/2018/03/14/natural-gradient/) parameterized by $\\theta$, $p\_\\theta(x)$ (referred to as “search distribution” in NES [paper](https://arxiv.org/abs/1106.4487)). It looks for the steepest direction within a small step in the distribution space where the distance is measured by KL divergence. With this constraint we ensure that each update is moving along the distributional manifold with constant speed, without being slowed down by its curvature.

$$ d^{\*}\_\\text{N} = \\operatorname\*{argmax}\_{\\text{KL}\[p\_\\theta \\| p\_{\\theta+d}\] = \\epsilon} \\mathcal{J}(\\theta + d) $$

## Estimation using Fisher Information Matrix

But, how to compute $\\text{KL}\[p\_\\theta | p\_{\\theta+\\Delta\\theta}\]$ precisely? By running Taylor expansion of $\\log p\_{\\theta + d}$ at $\\theta$, we get:

$$ \\begin{aligned} & \\text{KL}\[p\_\\theta \\| p\_{\\theta+d}\] \\\\ &= \\mathbb{E}\_{x \\sim p\_\\theta} \[\\log p\_\\theta(x) - \\log p\_{\\theta+d}(x)\] & \\\\ &\\approx \\mathbb{E}\_{x \\sim p\_\\theta} \[ \\log p\_\\theta(x) -( \\log p\_{\\theta}(x) + \\nabla\_\\theta \\log p\_{\\theta}(x) d + \\frac{1}{2}d^\\top \\nabla^2\_\\theta \\log p\_{\\theta}(x) d)\] & \\scriptstyle{\\text{; Taylor expand }\\log p\_{\\theta+d}} \\\\ &\\approx - \\mathbb{E}\_x \[\\nabla\_\\theta \\log p\_{\\theta}(x)\] d - \\frac{1}{2}d^\\top \\mathbb{E}\_x \[\\nabla^2\_\\theta \\log p\_{\\theta}(x)\] d & \\end{aligned} $$

where

$$ \\begin{aligned} \\mathbb{E}\_x \[\\nabla\_\\theta \\log p\_{\\theta}\] d &= \\int\_{x\\sim p\_\\theta} p\_\\theta(x) \\nabla\_\\theta \\log p\_\\theta(x) & \\\\ &= \\int\_{x\\sim p\_\\theta} p\_\\theta(x) \\frac{1}{p\_\\theta(x)} \\nabla\_\\theta p\_\\theta(x) & \\\\ &= \\nabla\_\\theta \\Big( \\int\_{x} p\_\\theta(x) \\Big) & \\scriptstyle{\\textrm{; note that }p\_\\theta(x)\\textrm{ is probability distribution.}} \\\\ &= \\nabla\_\\theta (1) = 0 \\end{aligned} $$

Finally we have,

$$ \\text{KL}\[p\_\\theta \\| p\_{\\theta+d}\] = - \\frac{1}{2}d^\\top \\mathbf{F}\_\\theta d \\text{, where }\\mathbf{F}\_\\theta = \\mathbb{E}\_x \[(\\nabla\_\\theta \\log p\_{\\theta}) (\\nabla\_\\theta \\log p\_{\\theta})^\\top\] $$

where $\\mathbf{F}\_\\theta$ is called the **[Fisher Information Matrix](http://mathworld.wolfram.com/FisherInformationMatrix.html)** and [it is](https://wiseodd.github.io/techblog/2018/03/11/fisher-information/) the covariance matrix of $\\nabla\_\\theta \\log p\_\\theta$ since $\\mathbb{E}\[\\nabla\_\\theta \\log p\_\\theta\] = 0$.

The solution to the following optimization problem:

$$ \\max \\mathcal{J}(\\theta + d) \\approx \\max \\big( \\mathcal{J}(\\theta) + {\\nabla\_\\theta\\mathcal{J}(\\theta)}^\\top d \\big)\\;\\text{ s.t. }\\text{KL}\[p\_\\theta \\| p\_{\\theta+d}\] - \\epsilon = 0 $$

can be found using a Lagrangian multiplier,

$$ \\begin{aligned} \\mathcal{L}(\\theta, d, \\beta) &= \\mathcal{J}(\\theta) + \\nabla\_\\theta\\mathcal{J}(\\theta)^\\top d - \\beta (\\frac{1}{2}d^\\top \\mathbf{F}\_\\theta d + \\epsilon) = 0 \\text{ s.t. } \\beta > 0 \\\\ \\nabla\_d \\mathcal{L}(\\theta, d, \\beta) &= \\nabla\_\\theta\\mathcal{J}(\\theta) - \\beta\\mathbf{F}\_\\theta d = 0 \\\\ \\text{Thus } d\_\\text{N}^\* &= \\nabla\_\\theta^\\text{N} \\mathcal{J}(\\theta) = \\mathbf{F}\_\\theta^{-1} \\nabla\_\\theta\\mathcal{J}(\\theta) \\end{aligned} $$

where $d\_\\text{N}^\*$ only extracts the direction of the optimal moving step on $\\theta$, ignoring the scalar $\\beta^{-1}$.

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CMA-ES-coordinates.png)\
The natural gradient samples (black solid arrows) in the right are the plain gradient samples (black solid arrows) in the left multiplied by the inverse of their covariance. In this way, a gradient direction with high uncertainty (indicated by high covariance with other samples) are penalized with a small weight. The aggregated natural gradient (red dash arrow) is therefore more trustworthy than the natural gradient (green solid arrow). (Image source: additional annotations on Fig 2 in [NES](https://arxiv.org/abs/1106.4487) paper)

## NES Algorithm

The fitness associated with one sample is labeled as $f(x)$ and the search distribution over $x$ is parameterized by $\\theta$. NES is expected to optimize the parameter $\\theta$ to achieve maximum expected fitness:

$$ \\mathcal{J}(\\theta) = \\mathbb{E}\_{x\\sim p\_\\theta(x)} \[f(x)\] = \\int\_x f(x) p\_\\theta(x) dx $$

Using the same log-likelihood [trick](http://blog.shakirm.com/2015/11/machine-learning-trick-of-the-day-5-log-derivative-trick/) in [REINFORCE](https://lilianweng.github.io/posts/2018-04-08-policy-gradient/#reinforce):

$$ \\begin{aligned} \\nabla\_\\theta\\mathcal{J}(\\theta) &= \\nabla\_\\theta \\int\_x f(x) p\_\\theta(x) dx \\\\ &= \\int\_x f(x) \\frac{p\_\\theta(x)}{p\_\\theta(x)}\\nabla\_\\theta p\_\\theta(x) dx \\\\ & = \\int\_x f(x) p\_\\theta(x) \\nabla\_\\theta \\log p\_\\theta(x) dx \\\\ & = \\mathbb{E}\_{x \\sim p\_\\theta} \[f(x) \\nabla\_\\theta \\log p\_\\theta(x)\] \\end{aligned} $$

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/NES-algorithm.png)

Besides natural gradients, NES adopts a couple of important heuristics to make the algorithm performance more robust.

- NES applies **rank-based fitness shaping**, that is to use the *rank* under monotonically increasing fitness values instead of using $f(x)$ directly. Or it can be a function of the rank (“utility function”), which is considered as a free parameter of NES.
- NES adopts **adaptation sampling** to adjust hyperparameters at run time. When changing $\\theta \\to \\theta’$, samples drawn from $p\_\\theta$ are compared with samples from $p\_{\\theta’}$ using \[Mann-Whitney U-test(https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney\_U\_test)\]; if there shows a positive or negative sign, the target hyperparameter decreases or increases by a multiplication constant. Note the score of a sample $x’\_i \\sim p\_{\\theta’}(x)$ has importance sampling weights applied $w\_i’ = p\_\\theta(x) / p\_{\\theta’}(x)$.

## Applications: ES in Deep Reinforcement Learning

## OpenAI ES for RL

The concept of using evolutionary algorithms in reinforcement learning can be traced back [long ago](https://arxiv.org/abs/1106.0221), but only constrained to tabular RL due to computational limitations.

Inspired by [NES](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/#natural-evolution-strategies), researchers at OpenAI ([Salimans, et al. 2017](https://arxiv.org/abs/1703.03864)) proposed to use NES as a gradient-free black-box optimizer to find optimal policy parameters $\\theta$ that maximizes the return function $F(\\theta)$. The key is to add Gaussian noise $\\epsilon$ on the model parameter $\\theta$ and then use the log-likelihood trick to write it as the gradient of the Gaussian pdf. Eventually only the noise term is left as a weighting scalar for measured performance.

Let’s say the current parameter value is $\\hat{\\theta}$ (the added hat is to distinguish the value from the random variable $\\theta$). The search distribution of $\\theta$ is designed to be an isotropic multivariate Gaussian with a mean $\\hat{\\theta}$ and a fixed covariance matrix $\\sigma^2 I$,

$$ \\theta \\sim \\mathcal{N}(\\hat{\\theta}, \\sigma^2 I) \\text{ equivalent to } \\theta = \\hat{\\theta} + \\sigma\\epsilon, \\epsilon \\sim \\mathcal{N}(0, I) $$

The gradient for $\\theta$ update is:

$$ \\begin{aligned} & \\nabla\_\\theta \\mathbb{E}\_{\\theta\\sim\\mathcal{N}(\\hat{\\theta}, \\sigma^2 I)} F(\\theta) \\\\ &= \\nabla\_\\theta \\mathbb{E}\_{\\epsilon\\sim\\mathcal{N}(0, I)} F(\\hat{\\theta} + \\sigma\\epsilon) \\\\ &= \\nabla\_\\theta \\int\_{\\epsilon} p(\\epsilon) F(\\hat{\\theta} + \\sigma\\epsilon) d\\epsilon & \\scriptstyle{\\text{; Gaussian }p(\\epsilon)=(2\\pi)^{-\\frac{n}{2}} \\exp(-\\frac{1}{2}\\epsilon^\\top\\epsilon)} \\\\ &= \\int\_{\\epsilon} p(\\epsilon) \\nabla\_\\epsilon \\log p(\\epsilon) \\nabla\_\\theta \\epsilon\\;F(\\hat{\\theta} + \\sigma\\epsilon) d\\epsilon & \\scriptstyle{\\text{; log-likelihood trick}}\\\\ &= \\mathbb{E}\_{\\epsilon\\sim\\mathcal{N}(0, I)} \[ \\nabla\_\\epsilon \\big(-\\frac{1}{2}\\epsilon^\\top\\epsilon\\big) \\nabla\_\\theta \\big(\\frac{\\theta - \\hat{\\theta}}{\\sigma}\\big) F(\\hat{\\theta} + \\sigma\\epsilon) \] & \\\\ &= \\mathbb{E}\_{\\epsilon\\sim\\mathcal{N}(0, I)} \[ (-\\epsilon) (\\frac{1}{\\sigma}) F(\\hat{\\theta} + \\sigma\\epsilon) \] & \\\\ &= \\frac{1}{\\sigma}\\mathbb{E}\_{\\epsilon\\sim\\mathcal{N}(0, I)} \[ \\epsilon F(\\hat{\\theta} + \\sigma\\epsilon) \] & \\scriptstyle{\\text{; negative sign can be absorbed.}} \\end{aligned} $$

In one generation, we can sample many $epsilon\_i, i=1,\\dots,n$ and evaluate the fitness *in parallel*. One beautiful design is that no large model parameter needs to be shared. By only communicating the random seeds between workers, it is enough for the master node to do parameter update. This approach is later extended to adaptively learn a loss function; see my previous post on [Evolved Policy Gradient](https://lilianweng.github.io/posts/2019-06-23-meta-rl/#meta-learning-the-loss-function).

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/OpenAI-ES-algorithm.png)\
The algorithm for training a RL policy using evolution strategies. (Image source: [ES-for-RL](https://arxiv.org/abs/1703.03864) paper)

To make the performance more robust, OpenAI ES adopts virtual batch normalization (BN with mini-batch used for calculating statistics fixed), mirror sampling (sampling a pair of $(-\\epsilon, \\epsilon)$ for evaluation), and [fitness shaping](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/#fitness-shaping).

## Exploration with ES

Exploration ([vs exploitation](https://lilianweng.github.io/posts/2018-01-23-multi-armed-bandit/#exploitation-vs-exploration)) is an important topic in RL. The optimization direction in the ES algorithm [above](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/TBA) is only extracted from the cumulative return $F(\\theta)$. Without explicit exploration, the agent might get trapped in a local optimum.

Novelty-Search ES (**NS-ES**; [Conti et al, 2018](https://arxiv.org/abs/1712.06560)) encourages exploration by updating the parameter in the direction to maximize the *novelty* score. The novelty score depends on a domain-specific behavior characterization function $b(\\pi\_\\theta)$. The choice of $b(\\pi\_\\theta)$ is specific to the task and seems to be a bit arbitrary; for example, in the Humanoid locomotion task in the paper, $b(\\pi\_\\theta)$ is the final $(x,y)$ location of the agent.

1. Every policy’s $b(\\pi\_\\theta)$ is pushed to an archive set $\\mathcal{A}$.
2. Novelty of a policy $\\pi\_\\theta$ is measured as the k-nearest neighbor score between $b(\\pi\_\\theta)$ and all other entries in $\\mathcal{A}$. (The use case of the archive set sounds quite similar to [episodic memory](https://lilianweng.github.io/posts/2019-06-23-meta-rl/#episodic-control).)

$$ N(\\theta, \\mathcal{A}) = \\frac{1}{\\lambda} \\sum\_{i=1}^\\lambda \\| b(\\pi\_\\theta), b^\\text{knn}\_i \\|\_2 \\text{, where }b^\\text{knn}\_i \\in \\text{kNN}(b(\\pi\_\\theta), \\mathcal{A}) $$

The ES optimization step relies on the novelty score instead of fitness:

$$ \\nabla\_\\theta \\mathbb{E}\_{\\theta\\sim\\mathcal{N}(\\hat{\\theta}, \\sigma^2 I)} N(\\theta, \\mathcal{A}) = \\frac{1}{\\sigma}\\mathbb{E}\_{\\epsilon\\sim\\mathcal{N}(0, I)} \[ \\epsilon N(\\hat{\\theta} + \\sigma\\epsilon, \\mathcal{A}) \] $$

NS-ES maintains a group of $M$ independently trained agents (“meta-population”), $\\mathcal{M} = \\{\\theta\_1, \\dots, \\theta\_M \\}$ and picks one to advance proportional to the novelty score. Eventually we select the best policy. This process is equivalent to ensembling; also see the same idea in [SVPG](https://lilianweng.github.io/posts/2018-04-08-policy-gradient/#svpg).

$$ \\begin{aligned} m &\\leftarrow \\text{pick } i=1,\\dots,M\\text{ according to probability}\\frac{N(\\theta\_i, \\mathcal{A})}{\\sum\_{j=1}^M N(\\theta\_j, \\mathcal{A})} \\\\ \\theta\_m^{(t+1)} &\\leftarrow \\theta\_m^{(t)} + \\alpha \\frac{1}{\\sigma}\\sum\_{i=1}^N \\epsilon\_i N(\\theta^{(t)}\_m + \\epsilon\_i, \\mathcal{A}) \\text{ where }\\epsilon\_i \\sim \\mathcal{N}(0, I) \\end{aligned} $$

where $N$ is the number of Gaussian perturbation noise vectors and $\\alpha$ is the learning rate.

NS-ES completely discards the reward function and only optimizes for novelty to avoid deceptive local optima. To incorporate the fitness back into the formula, another two variations are proposed.

**NSR-ES**:

$$ \\theta\_m^{(t+1)} \\leftarrow \\theta\_m^{(t)} + \\alpha \\frac{1}{\\sigma}\\sum\_{i=1}^N \\epsilon\_i \\frac{N(\\theta^{(t)}\_m + \\epsilon\_i, \\mathcal{A}) + F(\\theta^{(t)}\_m + \\epsilon\_i)}{2} $$

**NSRAdapt-ES (NSRA-ES)**: the adaptive weighting parameter $w = 1.0$ initially. We start decreasing $w$ if performance stays flat for a number of generations. Then when the performance starts to increase, we stop decreasing $w$ but increase it instead. In this way, fitness is preferred when the performance stops growing but novelty is preferred otherwise.

$$ \\theta\_m^{(t+1)} \\leftarrow \\theta\_m^{(t)} + \\alpha \\frac{1}{\\sigma}\\sum\_{i=1}^N \\epsilon\_i \\big((1-w) N(\\theta^{(t)}\_m + \\epsilon\_i, \\mathcal{A}) + w F(\\theta^{(t)}\_m + \\epsilon\_i)\\big) $$

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/NS-ES-experiments.png)\
(Left) The environment is Humanoid locomotion with a three-sided wall which plays a role as a deceptive trap to create local optimum. (Right) Experiments compare ES baseline and other variations that encourage exploration. (Image source: [NS-ES](https://arxiv.org/abs/1712.06560) paper)

## CEM-RL

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/CEM-RL.png)\
Architectures of the (a) CEM-RL and (b) [ERL](https://papers.nips.cc/paper/7395-evolution-guided-policy-gradient-in-reinforcement-learning.pdf) algorithms (Image source: [CEM-RL](https://arxiv.org/abs/1810.01222) paper)

The CEM-RL method ([Pourchot & Sigaud, 2019](https://arxiv.org/abs/1810.01222)) combines Cross Entropy Method (CEM) with either [DDPG](https://lilianweng.github.io/posts/2018-04-08-policy-gradient/#ddpg) or [TD3](https://lilianweng.github.io/posts/2018-04-08-policy-gradient/#td3). CEM here works pretty much the same as the simple Gaussian ES described [above](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/#simple-gaussian-evolution-strategies) and therefore the same function can be replaced using CMA-ES. CEM-RL is built on the framework of *Evolutionary Reinforcement Learning* (*ERL*; [Khadka & Tumer, 2018](https://papers.nips.cc/paper/7395-evolution-guided-policy-gradient-in-reinforcement-learning.pdf)) in which the standard EA algorithm selects and evolves a population of actors and the rollout experience generated in the process is then added into reply buffer for training both RL-actor and RL-critic networks.

Workflow:

- 1. The mean actor of the CEM population is $\\pi\_\\mu$ is initialized with a random actor network.
- 2. The critic network $Q$ is initialized too, which will be updated by DDPG/TD3.
- 3. Repeat until happy:

  - a. Sample a population of actors $\\sim \\mathcal{N}(\\pi\_\\mu, \\Sigma)$.
  - b. Half of the population is evaluated. Their fitness scores are used as the cumulative reward $R$ and added into replay buffer.
  - c. The other half are updated together with the critic.
  - d. The new $\\pi\_mu$ and $\\Sigma$ is computed using top performing elite samples. [CMA-ES](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/#covariance-matrix-adaptation-evolution-strategies-cma-es) can be used for parameter update too.

## Extension: EA in Deep Learning

(This section is not on evolution strategies, but still an interesting and relevant reading.)

The *Evolutionary Algorithms* have been applied on many deep learning problems. POET ([Wang et al, 2019](https://arxiv.org/abs/1901.01753)) is a framework based on EA and attempts to generate a variety of different tasks while the problems themselves are being solved. POET has been introduced in my [last post](https://lilianweng.github.io/posts/2019-06-23-meta-rl/#task-generation-by-domain-randomization) on meta-RL. Evolutionary Reinforcement Learning (ERL) is another example; See Fig. 7 (b).

Below I would like to introduce two applications in more detail, *Population-Based Training (PBT)* and *Weight-Agnostic Neural Networks (WANN)*.

## Hyperparameter Tuning: PBT

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/PBT.png)\
Paradigms of comparing different ways of hyperparameter tuning. (Image source: [PBT](https://arxiv.org/abs/1711.09846) paper)

Population-Based Training ([Jaderberg, et al, 2017](https://arxiv.org/abs/1711.09846)), short for **PBT** applies EA on the problem of hyperparameter tuning. It jointly trains a population of models and corresponding hyperparameters for optimal performance.

PBT starts with a set of random candidates, each containing a pair of model weights initialization and hyperparameters, $\\{(\\theta\_i, h\_i)\\mid i=1, \\dots, N\\}$. Every sample is trained in parallel and asynchronously evaluates its own performance periodically. Whenever a member deems ready (i.e. after taking enough gradient update steps, or when the performance is good enough), it has a chance to be updated by comparing with the whole population:

- **`exploit()`**: When this model is under-performing, the weights could be replaced with a better performing model.
- **`explore()`**: If the model weights are overwritten, `explore` step perturbs the hyperparameters with random noise.

In this process, only promising model and hyperparameter pairs can survive and keep on evolving, achieving better utilization of computational resources.

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/PBT-algorithm.png)\
The algorithm of population-based training. (Image source: [PBT](https://arxiv.org/abs/1711.09846) paper)

## Network Topology Optimization: WANN

*Weight Agnostic Neural* Networks (short for **WANN**; [Gaier & Ha 2019](https://arxiv.org/abs/1906.04358)) experiments with searching for the smallest network topologies that can achieve the optimal performance without training the network weights. By not considering the best configuration of network weights, WANN puts much more emphasis on the architecture itself, making the focus different from [NAS](http://openaccess.thecvf.com/content_cvpr_2018/papers/Zoph_Learning_Transferable_Architectures_CVPR_2018_paper.pdf). WANN is heavily inspired by a classic genetic algorithm to evolve network topologies, called *NEAT* (“Neuroevolution of Augmenting Topologies”; [Stanley & Miikkulainen 2002](http://nn.cs.utexas.edu/downloads/papers/stanley.gecco02_1.pdf)).

The workflow of WANN looks pretty much the same as standard GA:

1. Initialize: Create a population of minimal networks.
2. Evaluation: Test with a range of *shared* weight values.
3. Rank and Selection: Rank by performance and complexity.
4. Mutation: Create new population by varying best networks.

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/WANN-mutations.png)\
mutation operations for searching for new network topologies in WANN (Image source: [WANN](https://arxiv.org/abs/1906.04358) paper)

At the “evaluation” stage, all the network weights are set to be the same. In this way, WANN is actually searching for network that can be described with a minimal description length. In the “selection” stage, both the network connection and the model performance are considered.

![](https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/WANN-results.png)\
Performance of WANN found network topologies on different RL tasks are compared with baseline FF networks commonly used in the literature. "Tuned Shared Weight" only requires adjusting one weight value. (Image source: [WANN](https://arxiv.org/abs/1906.04358) paper)

As shown in Fig. 11, WANN results are evaluated with both random weights and shared weights (single weight). It is interesting that even when enforcing weight-sharing on all weights and tuning this single parameter, WANN can discover topologies that achieve non-trivial good performance.

* * *

Cited as:

```
@article{weng2019ES,
  title   = "Evolution Strategies",
  author  = "Weng, Lilian",
  journal = "lilianweng.github.io",
  year    = "2019",
  url     = "https://lilianweng.github.io/posts/2019-09-05-evolution-strategies/"
}
```

## References

\[1\] Nikolaus Hansen. [“The CMA Evolution Strategy: A Tutorial”](https://arxiv.org/abs/1604.00772) arXiv preprint arXiv:1604.00772 (2016).

\[2\] Marc Toussaint. [Slides: “Introduction to Optimization”](https://ipvs.informatik.uni-stuttgart.de/mlr/marc/teaching/13-Optimization/06-blackBoxOpt.pdf)

\[3\] David Ha. [“A Visual Guide to Evolution Strategies”](http://blog.otoro.net/2017/10/29/visual-evolution-strategies/) blog.otoro.net. Oct 2017.

\[4\] Daan Wierstra, et al. [“Natural evolution strategies.”](https://arxiv.org/abs/1106.4487) IEEE World Congress on Computational Intelligence, 2008.

\[5\] Agustinus Kristiadi. [“Natural Gradient Descent”](https://wiseodd.github.io/techblog/2018/03/14/natural-gradient/) Mar 2018.

\[6\] Razvan Pascanu & Yoshua Bengio. [“Revisiting Natural Gradient for Deep Networks.”](https://arxiv.org/abs/1301.3584v7) arXiv preprint arXiv:1301.3584 (2013).

\[7\] Tim Salimans, et al. [“Evolution strategies as a scalable alternative to reinforcement learning.”](https://arxiv.org/abs/1703.03864) arXiv preprint arXiv:1703.03864 (2017).

\[8\] Edoardo Conti, et al. [“Improving exploration in evolution strategies for deep reinforcement learning via a population of novelty-seeking agents.”](https://arxiv.org/abs/1712.06560) NIPS. 2018.

\[9\] Aloïs Pourchot & Olivier Sigaud. [“CEM-RL: Combining evolutionary and gradient-based methods for policy search.”](https://arxiv.org/abs/1810.01222) ICLR 2019.

\[10\] Shauharda Khadka & Kagan Tumer. [“Evolution-guided policy gradient in reinforcement learning.”](https://papers.nips.cc/paper/7395-evolution-guided-policy-gradient-in-reinforcement-learning.pdf) NIPS 2018.

\[11\] Max Jaderberg, et al. [“Population based training of neural networks.”](https://arxiv.org/abs/1711.09846) arXiv preprint arXiv:1711.09846 (2017).

\[12\] Adam Gaier & David Ha. [“Weight Agnostic Neural Networks.”](https://arxiv.org/abs/1906.04358) arXiv preprint arXiv:1906.04358 (2019).
