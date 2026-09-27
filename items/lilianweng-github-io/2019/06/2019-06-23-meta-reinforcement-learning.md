---
title: Meta Reinforcement Learning
link: https://lilianweng.github.io/posts/2019-06-23-meta-rl/
source: lilianweng-github-io
published: 2019-06-23T00:00:00Z
updated: 2019-06-23T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In my earlier post on meta-learning, the problem is mainly defined in the context of few-shot classification. Here I would like to explore more into cases when we try to “meta-learn” Reinforcement Learning (RL) tasks by developing an agent that can solve unseen tasks fast and efficiently.
content: extracted
html: 2019-06-23-meta-reinforcement-learning.html
preview:
  file: 2019-06-23-meta-reinforcement-learning.preview-543619e6539e.webp
  width: 256
  height: 79
  color: '#d3d2d1'
images:
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/Hochreiter-meta-learning.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-feb3b4bab914.png
    width: 1999
    height: 614
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-565a0ecea07d.webp
    width: 320
    height: 98
  - file: 2019-06-23-meta-reinforcement-learning.image-f92e7b581518.webp
    width: 640
    height: 197
  - file: 2019-06-23-meta-reinforcement-learning.image-44e92a9b9f5a.webp
    width: 960
    height: 295
  - file: 2019-06-23-meta-reinforcement-learning.image-6ba81b76b993.webp
    width: 1280
    height: 393
  - file: 2019-06-23-meta-reinforcement-learning.image-12fc7818e0c2.webp
    width: 1600
    height: 491
  - file: 2019-06-23-meta-reinforcement-learning.image-688206921ce0.webp
    width: 1999
    height: 614
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/meta-RL-illustration.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-6b8ec6577767.png
    width: 1999
    height: 828
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-fc89a3f3488f.webp
    width: 320
    height: 133
  - file: 2019-06-23-meta-reinforcement-learning.image-533e105d7be5.webp
    width: 640
    height: 265
  - file: 2019-06-23-meta-reinforcement-learning.image-3c84437c66ce.webp
    width: 960
    height: 398
  - file: 2019-06-23-meta-reinforcement-learning.image-3ef8b3e50fae.webp
    width: 1280
    height: 530
  - file: 2019-06-23-meta-reinforcement-learning.image-621fdbf67b1f.webp
    width: 1600
    height: 663
  - file: 2019-06-23-meta-reinforcement-learning.image-7998fb48a471.webp
    width: 1999
    height: 828
  color: '#fdfdfc'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/L2RL.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-7e280a73d390.png
    width: 950
    height: 594
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-56b5718a2a3b.webp
    width: 320
    height: 200
  - file: 2019-06-23-meta-reinforcement-learning.image-7fb282d86fdc.webp
    width: 640
    height: 400
  - file: 2019-06-23-meta-reinforcement-learning.image-aa43dc204630.webp
    width: 950
    height: 594
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/RL_2.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-8ec937a3b7ab.png
    width: 1592
    height: 542
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-fe5a29c62ee0.webp
    width: 320
    height: 109
  - file: 2019-06-23-meta-reinforcement-learning.image-a8ac5befab44.webp
    width: 640
    height: 218
  - file: 2019-06-23-meta-reinforcement-learning.image-835d1882e731.webp
    width: 960
    height: 327
  - file: 2019-06-23-meta-reinforcement-learning.image-96dc19356d3f.webp
    width: 1280
    height: 436
  - file: 2019-06-23-meta-reinforcement-learning.image-4cc9c12dd2cb.webp
    width: 1592
    height: 542
  color: '#fbfafa'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/MAESN.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-840cd6aacfe6.png
    width: 2794
    height: 1484
  color: '#fcfdfd'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/neural-episodic-control.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-67506e88d070.png
    width: 1999
    height: 1087
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-7078e3a3f856.webp
    width: 320
    height: 174
  - file: 2019-06-23-meta-reinforcement-learning.image-9474378572b0.webp
    width: 640
    height: 348
  - file: 2019-06-23-meta-reinforcement-learning.image-b7ceb51cff44.webp
    width: 960
    height: 522
  - file: 2019-06-23-meta-reinforcement-learning.image-5da010c1186e.webp
    width: 1280
    height: 696
  - file: 2019-06-23-meta-reinforcement-learning.image-da6211c8daea.webp
    width: 1600
    height: 870
  - file: 2019-06-23-meta-reinforcement-learning.image-5b5b30623716.webp
    width: 1999
    height: 1087
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/episodic-LSTM.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-eceff413ee80.png
    width: 1544
    height: 898
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-b297182fae1e.webp
    width: 320
    height: 186
  - file: 2019-06-23-meta-reinforcement-learning.image-48dbf5240bd4.webp
    width: 640
    height: 372
  - file: 2019-06-23-meta-reinforcement-learning.image-a5e5dbe39ce9.webp
    width: 960
    height: 558
  - file: 2019-06-23-meta-reinforcement-learning.image-b9fec141096c.webp
    width: 1280
    height: 744
  - file: 2019-06-23-meta-reinforcement-learning.image-87e48a8b45d4.webp
    width: 1544
    height: 898
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/POET.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-593e128022ac.png
    width: 1999
    height: 1205
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-05c4ad66feeb.webp
    width: 320
    height: 193
  - file: 2019-06-23-meta-reinforcement-learning.image-6691a7f6793b.webp
    width: 640
    height: 386
  - file: 2019-06-23-meta-reinforcement-learning.image-f89fda2cd60d.webp
    width: 960
    height: 579
  - file: 2019-06-23-meta-reinforcement-learning.image-f9a6d5f63070.webp
    width: 1280
    height: 772
  - file: 2019-06-23-meta-reinforcement-learning.image-c062ac56721c.webp
    width: 1600
    height: 964
  - file: 2019-06-23-meta-reinforcement-learning.image-83ed9974f369.webp
    width: 1999
    height: 1205
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2019-06-23-meta-rl/DIAYN.png
  original:
    file: 2019-06-23-meta-reinforcement-learning.image-778dd746b70d.png
    width: 1999
    height: 589
  variants:
  - file: 2019-06-23-meta-reinforcement-learning.image-4456c1324668.webp
    width: 320
    height: 94
  - file: 2019-06-23-meta-reinforcement-learning.image-8aa4a69cb4b3.webp
    width: 640
    height: 189
  - file: 2019-06-23-meta-reinforcement-learning.image-cb306d4d985c.webp
    width: 960
    height: 283
  - file: 2019-06-23-meta-reinforcement-learning.image-72eb415fe55e.webp
    width: 1280
    height: 377
  - file: 2019-06-23-meta-reinforcement-learning.image-9280ce73fbac.webp
    width: 1600
    height: 471
  - file: 2019-06-23-meta-reinforcement-learning.image-9453875a1012.webp
    width: 1999
    height: 589
  color: '#fafafa'
---

In my earlier post on [meta-learning](https://lilianweng.github.io/posts/2018-11-30-meta-learning/), the problem is mainly defined in the context of few-shot classification. Here I would like to explore more into cases when we try to “meta-learn” [Reinforcement Learning (RL)](https://lilianweng.github.io/posts/2018-02-19-rl-overview/) tasks by developing an agent that can solve unseen tasks fast and efficiently.

To recap, a good meta-learning model is expected to generalize to new tasks or new environments that have never been encountered during training. The adaptation process, essentially a *mini learning session*, happens at test with limited exposure to the new configurations. Even without any explicit fine-tuning (no gradient backpropagation on trainable variables), the meta-learning model autonomously adjusts internal hidden states to learn.

Training RL algorithms can be notoriously difficult sometimes. If the meta-learning agent could become so smart that the distribution of solvable unseen tasks grows extremely broad, we are on track towards [general purpose methods](http://incompleteideas.net/IncIdeas/BitterLesson.html) — essentially building a “brain” which would solve all kinds of RL problems without much human interference or manual feature engineering. Sounds amazing, right? 💖

## Back in 2001

I encountered a paper written in 2001 by [Hochreiter et al.](http://snowedin.net/tmp/Hochreiter2001.pdf) when reading [Wang et al., 2016](https://arxiv.org/pdf/1611.05763.pdf). Although the idea was proposed for supervised learning, there are so many resemblances to the current approach to meta-RL.

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/Hochreiter-meta-learning.png)\
The meta-learning system consists of the supervisory and the subordinate systems. The subordinate system is a recurrent neural network that takes as input both the observation at the current time step, $x\\\_t$ and the label at the last time step, $y\\\_{t-1}$. (Image source: [Hochreiter et al., 2001](http://snowedin.net/tmp/Hochreiter2001.pdf))

Hochreiter’s meta-learning model is a recurrent network with LSTM cell. LSTM is a good choice because it can internalize a history of inputs and tune its own weights effectively through [BPTT](https://en.wikipedia.org/wiki/Backpropagation_through_time). The training data contains $K$ sequences and each sequence is consist of $N$ samples generated by a target function $f\_k(.), k=1, \\dots, K$,

$$ \\{\\text{input: }(\\mathbf{x}^k\_i, \\mathbf{y}^k\_{i-1}) \\to \\text{label: }\\mathbf{y}^k\_i\\}\_{i=1}^N \\text{ where }\\mathbf{y}^k\_i = f\_k(\\mathbf{x}^k\_i) $$

Noted that *the last label* $\\mathbf{y}^k\_{i-1}$ is also provided as an auxiliary input so that the function can learn the presented mapping.

In the experiment of decoding two-dimensional quadratic functions, $a x\_1^2 + b x\_2^2 + c x\_1 x\_2 + d x\_1 + e x\_2 + f$, with coefficients $a$-$f$ are randomly sampled from \[-1, 1\], this meta-learning system was able to approximate the function after seeing only ~35 examples.

## Proposal in 2016

In the modern days of DL, [Wang et al.](https://arxiv.org/abs/1611.05763) (2016) and [Duan et al.](https://arxiv.org/abs/1611.02779) (2017) simultaneously proposed the very similar idea of **Meta-RL** (it is called **RL^2** in the second paper). A meta-RL model is trained over a distribution of MDPs, and at test time, it is able to learn to solve a new task quickly. The goal of meta-RL is ambitious, taking one step further towards general algorithms.

*Meta Reinforcement Learning*, in short, is to do [meta-learning](https://lilianweng.github.io/posts/2018-11-30-meta-learning/) in the field of [reinforcement learning](https://lilianweng.github.io/posts/2018-02-19-rl-overview/). Usually the train and test tasks are different but drawn from the same family of problems; i.e., experiments in the papers included multi-armed bandit with different reward probabilities, mazes with different layouts, same robots but with different physical parameters in simulator, and many others.

## Formulation

Let’s say we have a distribution of tasks, each formularized as an [MDP](https://lilianweng.github.io/posts/2018-02-19-rl-overview/#markov-decision-processes) (Markov Decision Process), $M\_i \\in \\mathcal{M}$. An MDP is determined by a 4-tuple, $M\_i= \\langle \\mathcal{S}, \\mathcal{A}, P\_i, R\_i \\rangle$:

| Symbol                                                                               | Meaning                          |
| ------------------------------------------------------------------------------------ | -------------------------------- |
| $\\mathcal{S}$                                                                       | A set of states.                 |
| $\\mathcal{A}$                                                                       | A set of actions.                |
| $P\_i: \\mathcal{S} \\times \\mathcal{A} \\times \\mathcal{S} \\to \\mathbb{R}\_{+}$ | Transition probability function. |
| $R\_i: \\mathcal{S} \\times \\mathcal{A} \\to \\mathbb{R}$                           | Reward function.                 |

(RL^2 paper adds an extra parameter, horizon $T$, into the MDP tuple to emphasize that each MDP should have a finite horizon.)

Note that common state $\\mathcal{S}$ and action space $\\mathcal{A}$ are used above, so that a (stochastic) policy: $\\pi\_\\theta: \\mathcal{S} \\times \\mathcal{A} \\to \\mathbb{R}\_{+}$ would get inputs compatible across different tasks. The test tasks are sampled from the same distribution $\\mathcal{M}$ or slightly modified version.

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/meta-RL-illustration.png)\
Illustration of meta-RL, containing two optimization loops. The outer loop samples a new environment in every iteration and adjusts parameters that determine the agent's behavior. In the inner loop, the agent interacts with the environment and optimizes for the maximal reward. (Image source: [Botvinick, et al. 2019](https://www.cell.com/action/showPdf?pii=S1364-6613%2819%2930061-0))

## Main Differences from RL

The overall configure of meta-RL is very similar to an ordinary RL algorithm, except that **the last reward** $r\_{t-1}$ and **the last action** $a\_{t-1}$ are also incorporated into the policy observation in addition to the current state $s\_t$.

- In RL: $\\pi\_\\theta(s\_t) \\to$ a distribution over $\\mathcal{A}$
- In meta-RL: $\\pi\_\\theta(a\_{t-1}, r\_{t-1}, s\_t) \\to$ a distribution over $\\mathcal{A}$

The intention of this design is to feed a history into the model so that the policy can internalize the dynamics between states, rewards, and actions in the current MDP and adjust its strategy accordingly. This is well aligned with the setup in [Hochreiter’s system](https://lilianweng.github.io/posts/2019-06-23-meta-rl/#back-in-2001). Both meta-RL and RL^2 implemented an LSTM policy and the LSTM’s hidden states serve as a *memory* for tracking characteristics of the trajectories. Because the policy is recurrent, there is no need to feed the last state as inputs explicitly.

The training procedure works as follows:

1. Sample a new MDP, $M\_i \\sim \\mathcal{M}$;
2. **Reset the hidden state** of the model;
3. Collect multiple trajectories and update the model weights;
4. Repeat from step 1.

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/L2RL.png)\
In the meta-RL paper, different actor-critic architectures all use a recurrent model. Last reward and last action are additional inputs. The observation is fed into the LSTM either as a one-hot vector or as an embedding vector after passed through an encoder model. (Image source: [Wang et al., 2016](https://arxiv.org/abs/1611.05763))

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/RL_2.png)\
As described in the RL^2 paper, illustration of the procedure of the model interacting with a series of MDPs in training time . (Image source: [Duan et al., 2017](https://arxiv.org/abs/1611.02779))

## Key Components

There are three key components in Meta-RL:

> ⭐ **A Model with Memory** \
>  A recurrent neural network maintains a hidden state. Thus, it could acquire and memorize the knowledge about the current task by updating the hidden state during rollouts. Without memory, meta-RL would not work.

> ⭐ **Meta-learning Algorithm** \
>  A meta-learning algorithm refers to how we can update the model weights to optimize for the purpose of solving an unseen task fast at test time. In both Meta-RL and RL^2 papers, the meta-learning algorithm is the ordinary gradient descent update of LSTM with hidden state reset between a switch of MDPs.

> ⭐ **A Distribution of MDPs** \
>  While the agent is exposed to a variety of environments and tasks during training, it has to learn how to adapt to different MDPs.

According to [Botvinick et al.](https://www.cell.com/action/showPdf?pii=S1364-6613%2819%2930061-0) (2019), one source of slowness in RL training is *weak [inductive bias](https://en.wikipedia.org/wiki/Inductive_bias)* ( = “a set of assumptions that the learner uses to predict outputs given inputs that it has not encountered”). As a general ML rule, a learning algorithm with weak inductive bias will be able to master a wider range of variance, but usually, will be less sample-efficient. Therefore, to narrow down the hypotheses with stronger inductive biases help improve the learning speed.

In meta-RL, we impose certain types of inductive biases from the *task distribution* and store them in *memory*. Which inductive bias to adopt at test time depends on the *algorithm*. Together, these three key components depict a compelling view of meta-RL: Adjusting the weights of a recurrent network is slow but it allows the model to work out a new task fast with its own RL algorithm implemented in its internal activity dynamics.

Meta-RL interestingly and not very surprisingly matches the ideas in the [AI-GAs](https://arxiv.org/abs/1905.10985) (“AI-Generating Algorithms”) paper by Jeff Clune (2019). He proposed that one efficient way towards building general AI is to make learning as automatic as possible. The AI-GAs approach involves three pillars: (1) meta-learning architectures, (2) meta-learning algorithms, and (3) automatically generated environments for effective learning.

* * *

The topic of designing good recurrent network architectures is a bit too broad to be discussed here, so I will skip it. Next, let’s look further into another two components: meta-learning algorithms in the context of meta-RL and how to acquire a variety of training MDPs.

My previous [post](https://lilianweng.github.io/posts/2018-11-30-meta-learning/) on meta-learning has covered several classic meta-learning algorithms. Here I’m gonna include more related to RL.

Both MAML ([Finn, et al. 2017](https://arxiv.org/abs/1703.03400)) and Reptile ([Nichol et al., 2018](https://arxiv.org/abs/1803.02999)) are methods on updating model parameters in order to achieve good generalization performance on new tasks. See an earlier post [section](https://lilianweng.github.io/posts/2018-11-30-meta-learning/#optimization-based) on MAML and Reptile.

The [return](https://lilianweng.github.io/posts/2018-02-19-rl-overview/#value-function) function in an RL problem, $G\_t^{(n)}$ or $G\_t^\\lambda$, involves a few hyperparameters that are often set heuristically, like the discount factor [$\\gamma$](https://lilianweng.github.io/posts/2018-02-19-rl-overview/#value-function) and the bootstrapping parameter [$\\lambda$](https://lilianweng.github.io/posts/2018-02-19-rl-overview/#combining-td-and-mc-learning). Meta-gradient RL ([Xu et al., 2018](http://papers.nips.cc/paper/7507-meta-gradient-reinforcement-learning.pdf)) considers them as *meta-parameters*, $\\eta=\\{\\gamma, \\lambda \\}$, that can be tuned and learned *online* while an agent is interacting with the environment. Therefore, the return becomes a function of $\\eta$ and dynamically adapts itself to a specific task over time.

$$ \\begin{aligned} G\_\\eta^{(n)}(\\tau\_t) &= R\_{t+1} + \\gamma R\_{t+2} + \\dots + \\gamma^{n-1}R\_{t+n} + \\gamma^n v\_\\theta(s\_{t+n}) & \\scriptstyle{\\text{; n-step return}} \\\\ G\_\\eta^{\\lambda}(\\tau\_t) &= (1-\\lambda) \\sum\_{n=1}^\\infty \\lambda^{n-1} G\_\\eta^{(n)} & \\scriptstyle{\\text{; λ-return, mixture of n-step returns}} \\end{aligned} $$

During training, we would like to update the policy parameters with gradients as a function of all the information in hand, $\\theta’ = \\theta + f(\\tau, \\theta, \\eta)$, where $\\theta$ are the current model weights, $\\tau$ is a sequence of trajectories, and $\\eta$ are the meta-parameters.

Meanwhile, let’s say we have a meta-objective function $J(\\tau, \\theta, \\eta)$ as a performance measure. The training process follows the principle of online cross-validation, using a sequence of consecutive experiences:

1. Starting with parameter $\\theta$, the policy $\\pi\_\\theta$ is updated on the first batch of samples $\\tau$, resulting in $\\theta’$.
2. Then we continue running the policy $\\pi\_{\\theta’}$ to collect a new set of experiences $\\tau’$, just following $\\tau$ consecutively in time. The performance is measured as $J(\\tau’, \\theta’, \\bar{\\eta})$ with a fixed meta-parameter $\\bar{\\eta}$.
3. The gradient of meta-objective $J(\\tau’, \\theta’, \\bar{\\eta})$ w.r.t. $\\eta$ is used to update $\\eta$:

$$ \\begin{aligned} \\Delta \\eta &= -\\beta \\frac{\\partial J(\\tau', \\theta', \\bar{\\eta})}{\\partial \\eta} \\\\ &= -\\beta \\frac{\\partial J(\\tau', \\theta', \\bar{\\eta})}{\\partial \\theta'} \\frac{d\\theta'}{d\\eta} & \\scriptstyle{\\text{ ; single variable chain rule.}} \\\\ &= -\\beta \\frac{\\partial J(\\tau', \\theta', \\bar{\\eta})}{\\partial \\theta'} \\frac{\\partial (\\theta + f(\\tau, \\theta, \\eta))}{\\partial\\eta} \\\\ &= -\\beta \\frac{\\partial J(\\tau', \\theta', \\bar{\\eta})}{\\partial \\theta'} \\Big(\\frac{d\\theta}{d\\eta} + \\frac{\\partial f(\\tau, \\theta, \\eta)}{\\partial\\theta}\\frac{d\\theta}{d\\eta} + \\frac{\\partial f(\\tau, \\theta, \\eta)}{\\partial\\eta}\\frac{d\\eta}{d\\eta} \\Big) & \\scriptstyle{\\text{; multivariable chain rule.}}\\\\ &= -\\beta \\frac{\\partial J(\\tau', \\theta', \\bar{\\eta})}{\\partial \\theta'} \\Big( \\color{red}{\\big(\\mathbf{I} + \\frac{\\partial f(\\tau, \\theta, \\eta)}{\\partial\\theta}\\big)}\\frac{d\\theta}{d\\eta} + \\frac{\\partial f(\\tau, \\theta, \\eta)}{\\partial\\eta}\\Big) & \\scriptstyle{\\text{; secondary gradient term in red.}} \\end{aligned} $$

where $\\beta$ is the learning rate for $\\eta$.

The meta-gradient RL algorithm simplifies the computation by setting the secondary gradient term to zero, $\\mathbf{I} + \\partial g(\\tau, \\theta, \\eta)/\\partial\\theta = 0$ — this choice prefers the immediate effect of the meta-parameters $\\eta$ on the parameters $\\theta$. Eventually we get:

$$ \\Delta \\eta = -\\beta \\frac{\\partial J(\\tau', \\theta', \\bar{\\eta})}{\\partial \\theta'} \\frac{\\partial f(\\tau, \\theta, \\eta)}{\\partial\\eta} $$

Experiments in the paper adopted the meta-objective function same as $TD(\\lambda)$ algorithm, minimizing the error between the approximated value function $v\_\\theta(s)$ and the $\\lambda$-return:

$$ \\begin{aligned} J(\\tau, \\theta, \\eta) &= (G^\\lambda\_\\eta(\\tau) - v\_\\theta(s))^2 \\\\ J(\\tau', \\theta', \\bar{\\eta}) &= (G^\\lambda\_{\\bar{\\eta}}(\\tau') - v\_{\\theta'}(s'))^2 \\end{aligned} $$

In policy gradient algorithms, the expected total reward is maximized by updating the policy parameters $\\theta$ in the direction of estimated gradient ([Schulman et al., 2016](https://arxiv.org/abs/1506.02438)),

$$ g = \\mathbb{E}\[\\sum\_{t=0}^\\infty \\Psi\_t \\nabla\_\\theta \\log \\pi\_\\theta (a\_t \\mid s\_t)\] $$

where the candidates for $\\Psi\_t$ include the trajectory return $G\_t$, the Q value $Q(s\_t, a\_t)$, or the advantage value $A(s\_t, a\_t)$. The corresponding surrogate loss function for the policy gradient can be reverse-engineered:

$$ L\_\\text{pg} = \\mathbb{E}\[\\sum\_{t=0}^\\infty \\Psi\_t \\log \\pi\_\\theta (a\_t \\mid s\_t)\] $$

This loss function is a measure over a history of trajectories, $(s\_0, a\_0, r\_0, \\dots, s\_t, a\_t, r\_t, \\dots)$. **Evolved Policy Gradient** (**EPG**; [Houthooft, et al, 2018](https://papers.nips.cc/paper/7785-evolved-policy-gradients.pdf)) takes a step further by defining the policy gradient loss function as a temporal convolution (1-D convolution) over the agent’s past experience, $L\_\\phi$. The parameters $\\phi$ of the loss function network are evolved in a way that an agent can achieve higher returns.

Similar to many meta-learning algorithms, EPG has two optimization loops:

- In the internal loop, an agent learns to improve its policy $\\pi\_\\theta$.
- In the outer loop, the model updates the parameters $\\phi$ of the loss function $L\_\\phi$. Because there is no explicit way to write down a differentiable equation between the return and the loss, EPG turned to [*Evolutionary Strategies*](https://en.wikipedia.org/wiki/Evolution_strategy) (ES).

A general idea is to train a population of $N$ agents, each of them is trained with the loss function $L\_{\\phi + \\sigma \\epsilon\_i}$ parameterized with $\\phi$ added with a small Gaussian noise $\\epsilon\_i \\sim \\mathcal{N}(0, \\mathbf{I})$ of standard deviation $\\sigma$. During the inner loop’s training, EPG tracks a history of experience and updates the policy parameters according to the loss function $L\_{\\phi + \\sigma\\epsilon\_i}$ for each agent:

$$ \\theta\_i \\leftarrow \\theta - \\alpha\_\\text{in} \\nabla\_\\theta L\_{\\phi + \\sigma \\epsilon\_i} (\\pi\_\\theta, \\tau\_{t-K, \\dots, t}) $$

where $\\alpha\_\\text{in}$ is the learning rate of the inner loop and $\\tau\_{t-K, \\dots, t}$ is a sequence of $M$ transitions up to the current time step $t$.

Once the inner loop policy is mature enough, the policy is evaluated by the mean return $\\bar{G}\_{\\phi+\\sigma\\epsilon\_i}$ over multiple randomly sampled trajectories. Eventually, we are able to estimate the gradient of $\\phi$ according to [NES](http://blog.otoro.net/2017/10/29/visual-evolution-strategies/) numerically ([Salimans et al, 2017](https://arxiv.org/abs/1703.03864)). While repeating this process, both the policy parameters $\\theta$ and the loss function weights $\\phi$ are being updated simultaneously to achieve higher returns.

$$ \\phi \\leftarrow \\phi + \\alpha\_\\text{out} \\frac{1}{\\sigma N} \\sum\_{i=1}^N \\epsilon\_i G\_{\\phi+\\sigma\\epsilon\_i} $$

where $\\alpha\_\\text{out}$ is the learning rate of the outer loop.

In practice, the loss $L\_\\phi$ is bootstrapped with an ordinary policy gradient (such as REINFORCE or PPO) surrogate loss $L\_\\text{pg}$, $\\hat{L} = (1-\\alpha) L\_\\phi + \\alpha L\_\\text{pg}$. The weight $\\alpha$ is annealing from 1 to 0 gradually during training. At test time, the loss function parameter $\\phi$ stays fixed and the loss value is computed over a history of experience to update the policy parameters $\\theta$.

The [exploitation vs exploration](https://lilianweng.github.io/posts/2018-01-23-multi-armed-bandit/#exploitation-vs-exploration) dilemma is a critical problem in RL. Common ways to do exploration include $\\epsilon$-greedy, random noise on actions, or stochastic policy with built-in randomness on the action space.

**MAESN** ([Gupta et al, 2018](http://papers.nips.cc/paper/7776-meta-reinforcement-learning-of-structured-exploration-strategies.pdf)) is an algorithm to learn structured action noise from prior experience for better and more effective exploration. Simply adding random noise on actions cannot capture task-dependent or time-correlated exploration strategies. MAESN changes the policy to condition on a per-task random variable $z\_i \\sim \\mathcal{N}(\\mu\_i, \\sigma\_i)$, for $i$-th task $M\_i$, so we would have a policy $a \\sim \\pi\_\\theta(a\\mid s, z\_i)$. The latent variable $z\_i$ is sampled once and fixed during one episode. Intuitively, the latent variable determines one type of behavior (or skills) that should be explored more at the beginning of a rollout and the agent would adjust its actions accordingly. Both the policy parameters and latent space are optimized to maximize the total task rewards. In the meantime, the policy learns to make use of the latent variables for exploration.

In addition, the loss function includes a KL divergence between the learned latent variable and a unit Gaussian prior, $D\_\\text{KL}(\\mathcal{N}(\\mu\_i, \\sigma\_i)|\\mathcal{N}(0, \\mathbf{I}))$. On one hand, it restricts the learned latent space not too far from a common prior. On the other hand, it creates the variational evidence lower bound ([ELBO](http://users.umiacs.umd.edu/~xyang35/files/understanding-variational-lower.pdf)) for the reward function. Interestingly the paper found that $(\\mu\_i, \\sigma\_i)$ for each task are usually close to the prior at convergence.

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/MAESN.png)\
The policy is conditioned on a latent variable variable $z\\\_i \\sim \\mathcal{N}(\\mu, \\sigma)$ that is sampled once every episode. Each task has different hyperparameters for the latent variable distribution, $(\\mu\\\_i, \\sigma\\\_i)$ and they are optimized in the outer loop. (Image source: [Gupta et al, 2018](http://papers.nips.cc/paper/7776-meta-reinforcement-learning-of-structured-exploration-strategies.pdf))

## Episodic Control

A major criticism of RL is on its sample inefficiency. A large number of samples and small learning steps are required for incremental parameter adjustment in RL in order to maximize generalization and avoid catastrophic forgetting of earlier learning ([Botvinick et al., 2019](https://www.cell.com/trends/cognitive-sciences/fulltext/S1364-6613\(19\)30061-0)).

**Episodic control** ([Lengyel & Dayan, 2008](http://papers.nips.cc/paper/3311-hippocampal-contributions-to-control-the-third-way.pdf)) is proposed as a solution to avoid forgetting and improve generalization while training at a faster speed. It is partially inspired by hypotheses on instance-based [hippocampal](https://en.wikipedia.org/wiki/Hippocampus) learning.

An *episodic memory* keeps explicit records of past events and uses these records directly as point of reference for making new decisions (i.e. just like [metric-based](https://lilianweng.github.io/posts/2018-11-30-meta-learning/#metric-based) meta-learning). In **MFEC** (Model-Free Episodic Control; [Blundell et al., 2016](https://arxiv.org/abs/1606.04460)), the memory is modeled as a big table, storing the state-action pair $(s, a)$ as key and the corresponding Q-value $Q\_\\text{EC}(s, a)$ as value. When receiving a new observation $s$, the Q value is estimated in an non-parametric way as the average Q-value of top $k$ most similar samples:

$$ \\hat{Q}\_\\text{EC}(s, a) = \\begin{cases} Q\_\\text{EC}(s, a) & \\text{if } (s,a) \\in Q\_\\text{EC}, \\\\ \\frac{1}{k} \\sum\_{i=1}^k Q(s^{(i)}, a) & \\text{otherwise} \\end{cases} $$

where $s^{(i)}, i=1, \\dots, k$ are top $k$ states with smallest distances to the state $s$. Then the action that yields the highest estimated Q value is selected. Then the memory table is updated according to the return received at $s\_t$:

$$ Q\_\\text{EC}(s, a) \\leftarrow \\begin{cases} \\max\\{Q\_\\text{EC}(s\_t, a\_t), G\_t\\} & \\text{if } (s,a) \\in Q\_\\text{EC}, \\\\ G\_t & \\text{otherwise} \\end{cases} $$

As a tabular RL method, MFEC suffers from large memory consumption and a lack of ways to generalize among similar states. The first one can be fixed with an LRU cache. Inspired by [metric-based](https://lilianweng.github.io/posts/2018-11-30-meta-learning/#metric-based) meta-learning, especially Matching Networks ([Vinyals et al., 2016](http://papers.nips.cc/paper/6385-matching-networks-for-one-shot-learning.pdf)), the generalization problem is improved in a follow-up algorithm, **NEC** (Neural Episodic Control; [Pritzel et al., 2016](https://arxiv.org/abs/1703.01988)).

The episodic memory in NEC is a Differentiable Neural Dictionary (**DND**), where the key is a convolutional embedding vector of input image pixels and the value stores estimated Q value. Given an inquiry key, the output is a weighted sum of values of top similar keys, where the weight is a normalized kernel measure between the query key and the selected key in the dictionary. This sounds like a hard [attention](https://lilianweng.github.io/posts/2018-06-24-attention/) machanism.

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/neural-episodic-control.png)\
Fig. 6 Illustrations of episodic memory module in NEC and two operations on a differentiable neural dictionary. (Image source: [Pritzel et al., 2016](https://arxiv.org/abs/1703.01988))

Further, **Episodic LSTM** ([Ritter et al., 2018](https://arxiv.org/abs/1805.09692)) enhances the basic LSTM architecture with a DND episodic memory, which stores task context embeddings as keys and the LSTM cell states as values. The stored hidden states are retrieved and added directly to the current cell state through the same gating mechanism within LSTM:

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/episodic-LSTM.png)\
Illustration of the episodic LSTM architecture. The additional structure of episodic memory is in bold. (Image source: [Ritter et al., 2018](https://arxiv.org/abs/1805.09692))

$$ \\begin{aligned} \\mathbf{c}\_t &= \\mathbf{i}\_t \\circ \\mathbf{c}\_\\text{in} + \\mathbf{f}\_t \\circ \\mathbf{c}\_{t-1} + \\color{green}{\\mathbf{r}\_t \\circ \\mathbf{c}\_\\text{ep}} &\\\\ \\mathbf{i}\_t &= \\sigma(\\mathbf{W}\_{i} \\cdot \[\\mathbf{h}\_{t-1}, \\mathbf{x}\_t\] + \\mathbf{b}\_i) & \\scriptstyle{\\text{; input gate}} \\\\ \\mathbf{f}\_t &= \\sigma(\\mathbf{W}\_{f} \\cdot \[\\mathbf{h}\_{t-1}, \\mathbf{x}\_t\] + \\mathbf{b}\_f) & \\scriptstyle{\\text{; forget gate}} \\\\ \\color{green}{\\mathbf{r}\_t} & \\color{green}{=} \\color{green}{\\sigma(\\mathbf{W}\_{r} \\cdot \[\\mathbf{h}\_{t-1}, \\mathbf{x}\_t\] + \\mathbf{b}\_r)} & \\scriptstyle{\\text{; reinstatement gate}} \\end{aligned} $$

where $\\mathbf{c}\_t$ and $\\mathbf{h}\_t$ are hidden and cell state at time $t$; $\\mathbf{i}\_t$, $\\mathbf{f}\_t$ and $\\mathbf{r}\_t$ are input, forget and reinstatement gates, respectively; $\\mathbf{c}\_\\text{ep}$ is the retrieved cell state from episodic memory. The newly added episodic memory components are marked in green.

This architecture provides a shortcut to the prior experience through context-based retrieval. Meanwhile, explicitly saving the task-dependent experience in an external memory avoids forgetting. In the paper, all the experiments have manually designed context vectors. How to construct an effective and efficient format of task context embeddings for more free-formed tasks would be an interesting topic.

Overall the capacity of episodic control is limited by the complexity of the environment. It is very rare for an agent to repeatedly visit exactly the same states in a real-world task, so properly encoding the states is critical. The learned embedding space compresses the observation data into a lower dimension space and, in the meantime, two states being close in this space are expected to demand similar strategies.

## Training Task Acquisition

Among three key components, how to design a proper distribution of tasks is the less studied and probably the most specific one to meta-RL itself. As described [above](https://lilianweng.github.io/posts/2019-06-23-meta-rl/#formulation), each task is a MDP: $M\_i = \\langle \\mathcal{S}, \\mathcal{A}, P\_i, R\_i \\rangle \\in \\mathcal{M}$. We can build a distribution of MDPs by modifying:

- The *reward configuration*: Among different tasks, same behavior might get rewarded differently according to $R\_i$.
- Or, the *environment*: The transition function $P\_i$ can be reshaped by initializing the environment with varying shifts between states.

## Task Generation by Domain Randomization

Randomizing parameters in a simulator is an easy way to obtain tasks with modified transition functions. If interested in learning further, check my last [post](https://lilianweng.github.io/posts/2019-05-05-domain-randomization/) on **domain randomization**.

## Evolutionary Algorithm on Environment Generation

[Evolutionary algorithm](https://en.wikipedia.org/wiki/Evolutionary_algorithm) is a gradient-free heuristic-based optimization method, inspired by natural selection. A population of solutions follows a loop of evaluation, selection, reproduction, and mutation. Eventually, good solutions survive and thus get selected.

**POET** ([Wang et al, 2019](https://arxiv.org/abs/1901.01753)), a framework based on the evolutionary algorithm, attempts to generate tasks while the problems themselves are being solved. The implementation of POET is only specifically designed for a simple 2D [bipedal walker](https://gym.openai.com/envs/BipedalWalkerHardcore-v2/) environment but points out an interesting direction. It is noteworthy that the evolutionary algorithm has had some compelling applications in Deep Learning like [EPG](https://lilianweng.github.io/posts/2019-06-23-meta-rl/#meta-learning-the-loss-function) and PBT (Population-Based Training; [Jaderberg et al, 2017](https://arxiv.org/abs/1711.09846)).

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/POET.png)\
An example bipedal walking environment (top) and an overview of POET (bottom). (Image source: [POET blog post](https://eng.uber.com/poet-open-ended-deep-learning/))

The 2D bipedal walking environment is evolving: from a simple flat surface to a much more difficult trail with potential gaps, stumps, and rough terrains. POET pairs the generation of environmental challenges and the optimization of agents together so as to (a) select agents that can resolve current challenges and (b) evolve environments to be solvable. The algorithm maintains a list of *environment-agent pairs* and repeats the following:

1. *Mutation*: Generate new environments from currently active environments. Note that here types of mutation operations are created just for bipedal walker and a new environment would demand a new set of configurations.
2. *Optimization*: Train paired agents within their respective environments.
3. *Selection*: Periodically attempt to transfer current agents from one environment to another. Copy and update the best performing agent for every environment. The intuition is that skills learned in one environment might be helpful for a different environment.

The procedure above is quite similar to [PBT](https://arxiv.org/abs/1711.09846), but PBT mutates and evolves hyperparameters instead. To some extent, POET is doing [domain randomization](https://lilianweng.github.io/posts/2019-05-05-domain-randomization/), as all the gaps, stumps and terrain roughness are controlled by some randomization probability parameters. Different from DR, the agents are not exposed to a fully randomized difficult environment all at once, but instead they are learning gradually with a curriculum configured by the evolutionary algorithm.

## Learning with Random Rewards

An MDP without a reward function $R$ is known as a *Controlled Markov process* (CMP). Given a predefined CMP, $\\langle \\mathcal{S}, \\mathcal{A}, P\\rangle$, we can acquire a variety of tasks by generating a collection of reward functions $\\mathcal{R}$ that encourage the training of an effective meta-learning policy.

[Gupta et al. (2018)](https://arxiv.org/abs/1806.04640) proposed two unsupervised approaches for growing the task distribution in the context of CMP. Assuming there is an underlying latent variable $z \\sim p(z)$ associated with every task, it parameterizes/determines a reward function: $r\_z(s) = \\log D(z|s)$, where a “discriminator” function $D(.)$ is used to extract the latent variable from the state. The paper described two ways to construct a discriminator function:

- Sample random weights $\\phi\_\\text{rand}$ of the discriminator, $D\_{\\phi\_\\text{rand}}(z \\mid s)$.
- Learn a discriminator function to encourage diversity-driven exploration. This method is introduced in more details in another sister paper “DIAYN” ([Eysenbach et al., 2018](https://arxiv.org/abs/1802.06070)).

DIAYN, short for “Diversity is all you need”, is a framework to encourage a policy to learn useful skills without a reward function. It explicitly models the latent variable $z$ as a *skill* embedding and makes the policy conditioned on $z$ in addition to state $s$, $\\pi\_\\theta(a \\mid s, z)$. (Ok, this part is same as [MAESN](https://lilianweng.github.io/posts/2019-06-23-meta-rl/#meta-learning-the-exploration-strategies) unsurprisingly, as the papers are from the same group.) The design of DIAYN is motivated by a few hypotheses:

- Skills should be diverse and lead to visitations of different states. → maximize the mutual information between states and skills, $I(S; Z)$
- Skills should be distinguishable by states, not actions. → minimize the mutual information between actions and skills, conditioned on states $I(A; Z \\mid S)$

The objective function to maximize is as follows, where the policy entropy is also added to encourage diversity:

$$ \\begin{aligned} \\mathcal{F}(\\theta) &= I(S; Z) + H\[A \\mid S\] - I(A; Z \\mid S) & \\\\ &= (H(Z) - H(Z \\mid S)) + H\[A \\mid S\] - (H\[A\\mid S\] - H\[A\\mid S, Z\]) & \\\\ &= H\[A\\mid S, Z\] \\color{green}{- H(Z \\mid S) + H(Z)} & \\\\ &= H\[A\\mid S, Z\] + \\mathbb{E}\_{z\\sim p(z), s\\sim\\rho(s)}\[\\log p(z \\mid s)\] - \\mathbb{E}\_{z\\sim p(z)}\[\\log p(z)\] & \\scriptstyle{\\text{; can infer skills from states & p(z) is diverse.}} \\\\ &\\ge H\[A\\mid S, Z\] + \\mathbb{E}\_{z\\sim p(z), s\\sim\\rho(s)}\[\\color{red}{\\log D\_\\phi(z \\mid s) - \\log p(z)}\] & \\scriptstyle{\\text{; according to Jensen's inequality; "pseudo-reward" in red.}} \\end{aligned} $$

where $I(.)$ is mutual information and $H\[.\]$ is entropy measure. We cannot integrate all states to compute $p(z \\mid s)$, so approximate it with $D\_\\phi(z \\mid s)$ — that is the diversity-driven discriminator function.

![](https://lilianweng.github.io/posts/2019-06-23-meta-rl/DIAYN.png)\
DIAYN Algorithm. (Image source: [Eysenbach et al., 2019](https://arxiv.org/abs/1802.06070))

Once the discriminator function is learned, sampling a new MDP for training is strainght-forward: First, sample a latent variable, $z \\sim p(z)$ and construct a reward function $r\_z(s) = \\log(D(z \\vert s))$. Pairing the reward function with a predefined CMP creates a new MDP.

* * *

Cited as:

```
@article{weng2019metaRL,
  title   = "Meta Reinforcement Learning",
  author  = "Weng, Lilian",
  journal = "lilianweng.github.io",
  year    = "2019",
  url     = "https://lilianweng.github.io/posts/2019-06-23-meta-rl/"
}
```

## References

\[1\] Richard S. Sutton. [“The Bitter Lesson.”](http://incompleteideas.net/IncIdeas/BitterLesson.html) March 13, 2019.

\[2\] Sepp Hochreiter, A. Steven Younger, and Peter R. Conwell. [“Learning to learn using gradient descent.”](http://snowedin.net/tmp/Hochreiter2001.pdf) Intl. Conf. on Artificial Neural Networks. 2001.

\[3\] Jane X Wang, et al. [“Learning to reinforcement learn.”](https://arxiv.org/abs/1611.05763) arXiv preprint arXiv:1611.05763 (2016).

\[4\] Yan Duan, et al. [“RL $^ 2$: Fast Reinforcement Learning via Slow Reinforcement Learning.”](https://arxiv.org/abs/1611.02779) ICLR 2017.

\[5\] Matthew Botvinick, et al. [“Reinforcement Learning, Fast and Slow”](https://www.cell.com/trends/cognitive-sciences/fulltext/S1364-6613\(19\)30061-0) Cell Review, Volume 23, Issue 5, P408-422, May 01, 2019.

\[6\] Jeff Clune. [“AI-GAs: AI-generating algorithms, an alternate paradigm for producing general artificial intelligence”](https://arxiv.org/abs/1905.10985) arXiv preprint arXiv:1905.10985 (2019).

\[7\] Zhongwen Xu, et al. [“Meta-Gradient Reinforcement Learning”](http://papers.nips.cc/paper/7507-meta-gradient-reinforcement-learning.pdf) NIPS 2018.

\[8\] Rein Houthooft, et al. [“Evolved Policy Gradients.”](https://papers.nips.cc/paper/7785-evolved-policy-gradients.pdf) NIPS 2018.

\[9\] Tim Salimans, et al. [“Evolution strategies as a scalable alternative to reinforcement learning.”](https://arxiv.org/abs/1703.03864) arXiv preprint arXiv:1703.03864 (2017).

\[10\] Abhishek Gupta, et al. [“Meta-Reinforcement Learning of Structured Exploration Strategies.”](http://papers.nips.cc/paper/7776-meta-reinforcement-learning-of-structured-exploration-strategies.pdf) NIPS 2018.

\[11\] Alexander Pritzel, et al. [“Neural episodic control.”](https://arxiv.org/abs/1703.01988) Proc. Intl. Conf. on Machine Learning, Volume 70, 2017.

\[12\] Charles Blundell, et al. [“Model-free episodic control.”](https://arxiv.org/abs/1606.04460) arXiv preprint arXiv:1606.04460 (2016).

\[13\] Samuel Ritter, et al. [“Been there, done that: Meta-learning with episodic recall.”](https://arxiv.org/abs/1805.09692) ICML, 2018.

\[14\] Rui Wang et al. [“Paired Open-Ended Trailblazer (POET): Endlessly Generating Increasingly Complex and Diverse Learning Environments and Their Solutions”](https://arxiv.org/abs/1901.01753) arXiv preprint arXiv:1901.01753 (2019).

\[15\] Uber Engineering Blog: [“POET: Endlessly Generating Increasingly Complex and Diverse Learning Environments and their Solutions through the Paired Open-Ended Trailblazer.”](https://eng.uber.com/poet-open-ended-deep-learning/) Jan 8, 2019.

\[16\] Abhishek Gupta, et al.[“Unsupervised meta-learning for Reinforcement Learning”](https://arxiv.org/abs/1806.04640) arXiv preprint arXiv:1806.04640 (2018).

\[17\] Eysenbach, Benjamin, et al. [“Diversity is all you need: Learning skills without a reward function.”](https://arxiv.org/abs/1802.06070) ICLR 2019.

\[18\] Max Jaderberg, et al. [“Population Based Training of Neural Networks.”](https://arxiv.org/abs/1711.09846) arXiv preprint arXiv:1711.09846 (2017).
