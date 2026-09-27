---
title: Recommendations for Evaluating Adversarial Example Defenses
link: https://nicholas.carlini.com/writing/2018/evaluating-adversarial-example-defenses.html
source: nicholas-carlini-com
published: 2018-05-26T00:00:00Z
updated: 2018-05-26T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Abstract: This document contains a collection of advice for performing adversarial example defense evaluations.'
content: extracted
html: 2018-05-26-recommendations-for-evaluating-adversarial-example-defenses.html
---

**THIS ADVICE IS NOW OUT OF DATE.** I ended up working with many others to write a full paper with 20 pages of advice on evaluating adversarial robustness. **READ OUR PAPER INSTEAD:** [On Evaluating Adversarial Robustness](https://github.com/evaluating-adversarial-robustness/adv-eval-paper). It contains strictly more information.

Original content of this page follows below:

At ICML 2018, because our paper [Obfuscated Gradients Give a False Sense of Security: Circumventing Defenses to Adversarial Examples](https://nicholas.carlini.com/papers/2018_icml_obfuscatedgradients.pdf) won best paper, we got to give two talks. While the first was a standard paper talk, in the second talk, I focused exclusively on advice for performing adversarial example defense evaluations. These recommendations are informed by the analysis we performed in our paper where we showed that over half of the defense papers accepted at ICLR 2018 were incorrect [\[a\]](https://nicholas.carlini.com/writing/2018/evaluating-adversarial-example-defenses.html#) That is, the results they claimed were significantly higher than actually delivered. See our paper for details. and performed weak evaluations. [\[b\]](https://nicholas.carlini.com/writing/2018/evaluating-adversarial-example-defenses.html#) More generally, it is not these papers, and these papers only, that were performing weak evaluations. The field as a whole is. These recommendations come from approaches I notice that we were taking that others were (are) not.

I hope to codify some of this advice I gave in the talk here, and provide some more detail and justification behind these pieces of advice. Much of the content here should be useful not only for those performing an evaluation of an adversarial example defense, but also for those who are reading an adversarial example defense paper and would like to understand if the evaluation performed by the authors is sufficient.

The first, and by far most important, component of a defense evaluation is to attempt to evade the specific defense. After the defense has been defined, ask: what attack could possibly defeat this defense? All attacks that might work must be shown to be ineffective. An evaluation that does not attempt to do this, but does everything else listed below, is fundamentally flawed. This is true for at least two reasons.

First, a defense is only interesting from the perspective of a security evaluation if it withstands future attack. Showing that a defense can defeat previous attacks is a necessary, *but not sufficient,* component of an evaluation. Even if we show that a defense can stop state-of-the-art attacks not targeting this specific defense, this is not sufficient. It is crucial to actively attempt to defeat the specific defense being proposed.

Why is this the case? Given any fixed attack algorithm (however powerful), there are often thousands of ways to “defend” against it, and make the attack always fail, by subtly breaking the specific attack. For example, imagine I was trying to design a secure encryption algorithm and settled on [rot-13](https://en.wikipedia.org/wiki/ROT13) (rotate each caracter forward by 13 letters, so that *a* becomes *n* and *b* becomes *o*). This algorithm is obviously insecure. The attack that defeats it---rotating the characters back by 13 characters---is trivial.

If that were the state of the world, I might claim that I can fix the problem. Instead of rotating by 13 characters, I am now going to rotate by 14. (So that, now, *a* goes to *o* instead). So, is this algorithm effective?

Well, if we consider something secure if it defeats previous attacks, this defense does defeat the prior attack. Rotating backwards by 13 characters will no longer decrypt the messages. However, and here's the important part, this algorithm is no more secure than the prior one. Defending against prior attacks is insufficient. We now have to ask: can the attacker adapt? In this case, the answer is yes. The attacker will just rotate by 14 characters.

We could play this game forever, [\[c\]](https://nicholas.carlini.com/writing/2018/evaluating-adversarial-example-defenses.html#) Well, not forever. There are 26 ways to do a rotation. But after exhausting all 26 rotations, we could start with other permutations, which are still not secure, and there are 26! of those. with the defender constantly “preventing” the attack by making changes that do not actually change anything fundamentally, but stop prior attacks from working.

This is why it is exceptionally important to actively attempt to evade a defense *after* completely specifying it.

I said there were two reasons why it is necessary to attempt an adaptive attack: the second reason is that, even if none of the above is convincing, we have *already solved the problem* of non-adaptive attacks. Of the over 200 defense papers on arXiv, well over half of them have already introduced defenses that are effective when the attacker is constrained to apply only existing attack algorithms.

So, in order to make a claim that a defense improves on the state of the art, it is insufficient to only evaluate on previous attacks. That problem has already been solved a hundred times over. In order to demonstrate a defense has done more than prior defenses, it must demonstrate it is effective against future attacks, too.

(Finally: if none of the above is convincing and the new defense is only evaluated against prior fixed attacks, it is important *not* to compare to the numbers obtained by evaluating these prior defenses on adaptive attacks. That is, if new defense A has 90% efficacy against some set of fixed attacks, but defense B has only 10% efficacy on adaptive attacks, it is no more fair to say “defense A is 80 percentage points more effective than B.” than it would be fair to say “My MLPs are better than ResNets because my MLP achieves 99% accuracy on MNIST compared to the 95% accuracy of a ResNet on CIFAR.” Unfortunately, this is the most common comparison made in the literature.)

How should we perform an adaptive attack? Here are some specific recommendations for types of attacks to attempt. Many of these recommendations come directly from various papers written previously. [\[c\]](https://nicholas.carlini.com/writing/2018/evaluating-adversarial-example-defenses.html#) Among other papers, see [Towards Evaluating the Robustness of Neural Networks](https://arxiv.org/abs/1608.04644), [Adversarial Examples Are Not Easily Detected: Bypassing Ten Detection Methods](https://arxiv.org/abs/1705.07263), and [Obfuscated Gradients Give a False Sense of Security: Circumventing Defenses to Adversarial Examples](https://arxiv.org/abs/1802.00420).

**Do not use Fast Gradient Sign (exclusively).** The [Fast Gradient Sign](https://arxiv.org/abs/1412.6572) (FGS) attack is a simple approach to generating adversarial examples that was proposed to demonstrate the linearity of neural networks. It was never intended to be a strong attack for evaluating the robustness of a neural network, and should not be used as one.

Even if it were intended to be a strong attack worth defending against, there are *many* defenses which achieve near-perfect success at defending against this attack. For example, a weak version of [adversarial training](https://arxiv.org/abs/1705.07204) [\[d\]](https://nicholas.carlini.com/writing/2018/evaluating-adversarial-example-defenses.html#) See Figure 1. The network just learns to make the gradient point the wrong direction locally, but changes nothing globally. can defend against this attack by causing [gradient masking](https://arxiv.org/abs/1602.02697), where locally the gradient around a given image may point in a direction that is not useful for generating an adversarial example.

While evaluating against FGS can be a component of an attack evaluation, it should never be used as the only attack in an evaluation.

**Perform an iterative attack with 1000+ iterations.** While it is possible to perform an attack correctly with only 10 or 100 iterations of gradient descent, it requires much more thought and care. I would recommend 1000 iterations as a safe starting point, but more may be necessary in some situations.

There is no reasonable threat model under which an attacker can compute 10 (or 100) iterations of gradient descent, but not 1000. Because the threat model must not constrain the approach an attacker takes, it is always worth evaluating against a strong attack with many iterations of gradient descent.

**Start search from random offsets.** When performing iterative attacks, it is useful to begin the search for adversarial examples at random offsets away from the initial, source input. This can [reduce the distortion](https://arxiv.org/abs/1608.04644) required to generate adversarial examples by 10%, and can make training on adversarial examples [overfit less often](https://arxiv.org/abs/1706.06083). Doing this every time is not necessary, but doing it at least once to verify it does not significantly change results is worthwhile. I would recommend at least 10 random starting points.

**Perform a transferability analysis.** Adversarial examples often [transfer](https://arxiv.org/abs/1605.07277) between models. That is, an adversarial example generated against one model trained on one dataset with one architecture will often *also* be adversarial on another model, even if it is trained on a different dataset with a different architecture.

An adversarial example defense evaluation should attempt a transferability analysis to validate validate that the proposed defense breaks transferability. If it does not, then it is likely to not actually be a strong defense, but just appear like one.

To attempt a transferability analysis, take some source model and and generate adversarial examples on this source model. These adversarial examples should be generated to be classified strongly as the adversarial class. Then, take those adversarial examples and test them on the target (defended) model, and check if any of them remain adversarial.

What should be the source model for a transferability analysis? A good starting point is a model as similar to the defended model as possible, trained on the same training data. If the defended model adds some new layers to a baseline model, it would be good to try the baseline. I would also recommend also trying a strong [adversarially trained model](https://arxiv.org/abs/1706.06083), which has been shown to be a strong source model for adversarial examples. It is also [more effective](https://arxiv.org/abs/1611.02770) to generate adversarial examples that fool an ensemble of source models, and then use those to transfer to a target model.

**Apply gradient-free attacks.** To ensure that the model is not causing various forms of gradient masking, it is worth attempting gradient-free attacks. There are several such proposals. I would recommend these four. [ZOO](https://arxiv.org/abs/1708.03999) numerically estimates gradients and then performs gradient descent, making it powerful but potentially ineffective when the loss surface is very noisy. [SPSA](https://arxiv.org/abs/1802.05666) was proposed specifically to evaluate adversarial example defenses, and has broken many. [NES](https://arxiv.org/abs/1804.08598) is effective at generating adversarial examples with a limited number of queries, and so generates adversarial examples of higher distortion. [Decision-only](https://arxiv.org/abs/1712.04248) adversarial example generation looks only at the top most-likely label and so is more robust to a noisy loss surface, but requires many more queries to generate high quality adversarial examples.

**Try brute-force (random search) attacks.** By attempting to generating adversarial examples with purely random search, by sampling completely random perturbations (with, e.g., Gaussian or uniform random noise) we can confirm that we aren't missing anything obvious. If brute-force random sampling identifies adversarial examples that other methods haven't found, this indicates that other attacks can be improved.

Start by sampling random points of a large distance away. Every time an adversarial example is found, limit the search for adversarial examples of strictly smaller distortion. I recommend verifying a hundred instances or so with a few hundred thousand random samples.

After having performed an evaluation (consisting of at least the above steps), there are several several simple checks that will help identify potential flaws in the attacks that should be corrected.

**Evaluate against the worst attack.** It is important to compare defense against the worst attacks. If Defense A does better than Defense B on weaker attacks (e.g., FGS) but does worse against a stronger attack (e.g., PGD), then Defense B is the stronger defense.

**Verify iterative attacks are better than single-step attacks.** Iterative attacks are strictly more powerful than single-step attacks, and so their results should be strictly superior. If an iterative attack performs worse than a single-step attack, this often indicates that the iterative attack is not correct.

If this is the case, one useful diagnostic test is to plot attack success rate versus number of attack iterations averaged over many attempts, to try and identify if there is a pattern. Another diagnostic is to plot the model *loss* versus the number of attack iterations for a single input. The model loss should be (mostly) decreasing at each iteration. If this is not the case, trying a smaller step size may be helpful. The model may also be [obfuscating gradients](https://nicholas.carlini.com/writing/2018/evaluating-adversarial-example-defenses.html).

**Verify that attack success is monotonically increasing with distortion.** Attacks that allow more distortion are strictly stronger than attacks that allow less distortion. If the attack success rate ever decreases as the amount of distortion allowed increases, the attack is likely flawed.

**Verify iterative attacks converge.** On at least a few inputs, confirm that the number of iterations of gradient descent being performed is sufficient by plotting the attack success versus the number of iterations. This plot should eventually plateau; if not, the attack has not converged and more iterations should be used. The number of iterations necessary is inversely proportional to step size and proportional to distortion allowed. Dataset complexity is often related. For example, on CIFAR-10 with a maximum distortion of 8/255 (as is often done), I have always observed convergence in under 1000 iterations with a step size of 1.

**Verify that unbounded attacks always succeed.** With unbounded distortion, any attack should *eventually* reach 100% success, if only by switching the input to actually be part of the other class. If unbounded attacks do not succeed, this indicates that the attack is being applied incorrectly.

**For unbounded attacks measure distortion, not success rate.** The correct metric for evaluating unbounded attacks is the distortion required to generate an adversarial example, not the success rate (which should always be 100%). The most useful plot is to show success rate vs. distortion, and the most useful single number is either the mean or median distance to adversarial exapmles, when using unbounded attacks. To make measuring success rate meaningful, another option is to artifically bound the attack and report any adversarial example of distance greater than some threshold a failure (as long as this threshold is stated).

**Verify white-box attacks perform better than black-box attacks.** Because white-box attacks are a strict superset of black-box attacks, they should perform strictly better. Black-box attacks doing better often indicates that the defense is somehow masking gradient information. This often indicates that the white-box attack could be improved to succeed more often.

When this happens, I suggest looking at which instances adversarial examples can be found with black-box but not white-box attacks. Check if these instances are at all related, and investigate why white-box attacks are not finding these adversarial examples.
