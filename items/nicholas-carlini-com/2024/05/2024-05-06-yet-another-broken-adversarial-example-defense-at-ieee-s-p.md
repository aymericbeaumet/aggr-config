---
title: (yet another) Broken Adversarial Example Defense at IEEE S&P 2024
link: https://nicholas.carlini.com/writing/2024/yet-another-broken-defense.html
source: nicholas-carlini-com
published: 2024-05-06T00:00:00Z
updated: 2024-05-06T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: IEEE SP 2024 (one of the top computer security conferences) has, again, accepted an adversarial example defense paper that is broken with simple attacks. It contains claims that are mathematically impossible, does not follow recommended guidance on evaluating adversarial robustness, and its own figures present all the necessary evidence that the evaluation was conducted incorrectly.
content: extracted
html: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.html
preview:
  file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.preview-426d2d7ef8a3.webp
  width: 256
  height: 256
  color: '#d6d6d6'
images:
- source: https://nicholas.carlini.com/writing/2024/mnist_7s.jpg
  original:
    file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-847eccbdbb67.jpg
    width: 964
    height: 964
  variants:
  - file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-9941e54f7bf9.webp
    width: 320
    height: 320
  - file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-fa7fee0a86a3.webp
    width: 640
    height: 640
  - file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-1d17d7552535.webp
    width: 960
    height: 960
  - file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-f91c33fa11ae.webp
    width: 964
    height: 964
  color: '#fdfdfd'
- source: https://nicholas.carlini.com/writing/2024/def2_claim1.png
  original:
    file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-d080ca1814e6.png
    width: 1370
    height: 878
  color: '#fefefe'
- source: https://nicholas.carlini.com/writing/2024/def2_claim2.jpg
  original:
    file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-1dce89abeed5.jpg
    width: 881
    height: 874
  color: '#fbfbfb'
- source: https://nicholas.carlini.com/writing/2024/def1_claim1.png
  original:
    file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-e56cfc25e759.png
    width: 1322
    height: 972
  color: '#fdfdfd'
- source: https://nicholas.carlini.com/writing/2024/def1_claim2.png
  original:
    file: 2024-05-06-yet-another-broken-adversarial-example-defense-at-ieee-s-p.image-c334ca9281c1.png
    width: 1382
    height: 1046
  color: '#fdfdfd'
---

IEEE S&P 2024 (one of the top computer security conferences) has, again, accepted an adversarial example defense paper that is broken with simple attacks. It contains claims that are mathematically impossible, does not follow recommended guidance on evaluating adversarial robustness, and its own figures present all the necessary evidence that the evaluation was conducted incorrectly.

Upon investigation, it turns out that the paper's evaluation code contained a bug. Modifying one line of code breaks the defense and reduces the model accuracy to 0%. After I notified the authors of this bug, they then proposed a new defense component to prevent this attack. But this new component, not described in the original paper, contains a second bug. Again, by changing one line of code, I can break this modified defense.

The authors have indicated they are working on another fix for this second attack. Unfortunately it has been six weeks and they have been unable to describe the necessary fix, provide code, or provide a pointer to where the fix is described in the paper. I then asked the S&P program chairs to help me get this information, but this did not result in any new information. And with the conference coming up in two weeks, I can't wait any longer. When this information is made available, I will update this post with an analysis of any proposed changes.

***Update! (2024-05-27)***: The authors have provided the fix for the second attack. Unfortunately it also doesn't work. Again---for a third time---changing one line of code breaks the defense.

### Overview

I had a hard time deciding on how to write this post, because I think there are two stories here:

1. The first story is the one I started out writing: a paper with impossible claims was accepted at a top conference (again), after the same thing happened at last year's conference. And so the first thing I want to do in this article is to teach you enough about how to evaluate adversarial example defenses that you could have done what the reviewers didn't, and catch these mistake yourself.
2. The second story is more specific to this one defense paper, and harder to write about. It's the story of a paper that was accepted at a top conference, and then when I informed the authors it had a bug, they modified the defense and made it something different. And then when I showed this change had a bug, they again said they had yet another fix---but have been unable to provide details on the fix so that I can evaluate it.

This worries me because neither of these stories are unique to this one paper. And at the same time, adversarial machine learning is quickly becoming an important field of study: we're not going to be able to deploy machine learning models as widely as we'd like if it's trivial to make them do bad things. And if we can't even manage to get *basic* things right in papers accepted at the top conferences, we're going to have a bad time.

## Story #1: flawed evaluations

Let me get started with the first story I had intended to write about: a story on adversarial robustness, and how (not) to evaluate adversarial robustness.

## Setup: What am I even talking about?

An *adversarial example* is an input to a machine learning model that makes it do something wrong. So, for example, people have shown how to make adversarial stop signs that look like normal stop signs to humans but cause a self-driving car to think it's a speed limit sign. Or, recently, we've shown how to construct adversarial text that causes ChatGPT to output hateful text it would previously have refused to generate.

Whereas adversarial examples used to be of only academic concern, because no one actually used machine learning models in practice, ML models now drive a significant fraction of the largest companies. And the more models are deployed, the more likely an adversary is to be motivated to try and break one of them. So it becomes important to fix this problem.

A *defense* to adversarial examples is a way to make a model robust to these attacks. Defenses have, in the past, been very hard to design. I [\[1\]](https://arxiv.org/abs/1607.04311) have [\[2\]](https://arxiv.org/abs/1608.04644) written [\[3\]](https://arxiv.org/abs/1705.07263) a [\[4\]](https://www.usenix.org/system/files/conference/woot17/woot17-paper-he.pdf) bunch [\[5\]](https://proceedings.mlr.press/v80/athalye18a/athalye18a.pdf) of [\[6\]](https://arxiv.org/abs/1804.03286) papers [\[7\]](https://arxiv.org/abs/1902.02322) on [\[8\]](https://arxiv.org/abs/1902.06705) how [\[9\]](https://arxiv.org/abs/2002.08347) to [\[10\]](https://arxiv.org/abs/2009.10975) break [\[11\]](https://arxiv.org/abs/2106.15023) defenses. In total I've broken over 30 published defenses, and another dozen or so unpublished ones. (And I'm not the only one who breaks these defenses.) So suffice to say: it's very hard.

A defense starts out by making a specific *robustness claim*. The type of claim I will talk about in this section is called *max-norm* robustness, or *L∞* robustness.

Such a claim says: if an adversary can modify every pixel in an image by at most some specific amount, then the model will still be accurate. For example, on the MNIST dataset (a dataset of handwritten digits), it's typical to claim robustness against an adversary who can modify each pixel by at most 0.3. That is, if a pixel is completely black at 0.0, the adversary can make it as bright as 0.3, and if a pixel is completely white at 1.0, the adversary can make it as dark as 0.7.

![](https://nicholas.carlini.com/writing/2024/mnist_7s.jpg)

At right I've shown a few examples of the same un-modified image of a 7 (shown in the upper left) with different max-norm perturbations of 0.3, the most common bound for adversarial example defenses on MNIST. I've modified it three different ways, all max-norm valid: first, I've brightened all the pixels, then, I've darkened all the pixels, and finally, I've brightened some pixels and darkened others. In each case the max-norm perturbation is 0.3, and in each case the image still looks like a 7 to us as humans.

The problem of adversarial examples is that it's often possible to find images that a human will say are still the same, but a machine learning model will incorrectly classify as something else. (As it happens, the “random” perturbation I've put in the lower right isn't actually random---I've chosen that noise because it makes a simple MNIST classifier think this 7 is a 0.)

The strongest papers to date will make claims of the form “our defense is 95% accurate on MNIST under a max-norm attack with perturbation size 0.3”. They'll usually also argue robustness on other datasets, and against other types of perturbations, but this dataset is all I'll talk about here because it's the simplest and both of the papers I'll break make claims on MNIST.

## The First Defense

The defense I'll spend the most time discussing will be presented at this years's IEEE S&P 2024. Exactly how the defense works doesn't matter for the purpose of this post. Instead, all I'm going to do is show you what the paper says the defense can do, and then show you why that's mathematically impossible.

![](https://nicholas.carlini.com/writing/2024/def2_claim1.png)

Let's get started with the first impossible claim: this paper claims an accuracy of over 60% on the MNIST dataset under a strong max-norm attack with a perturbation size of 0.5. I've reproduced the figure from the paper at right, which compares their model to adversarial training (AT) and the baseline non-robust model. As you move to the right, the size of the perturbation increases, and the accuracy of the model decreases.

But this 60% accuracy number is impossible: any correct defense evaluation **must** report at most 10% accuracy under a max-norm attack with perturbation size of 0.5. (If you'd like, take a few minutes to think about why this is true before I tell you why that is.)

Had your think? Okay, here's why.

What's the largest max-norm distance between *any* MNIST image and the solid-grey image that has a color value of 0.5 at every pixel? Well the smallest a pixel can be is 0.0, and the largest it can be is 1.0, so the largest max-norm distance any image can be from solid-grey is 0.5. Therefore, with a max-norm perturbation size of 0.5, *every* MNIST image is within 0.5 of the solid-grey image. And so an attack that just returned this solid-grey image every single time is a “valid” attack at a distortion of 0.5. And because this is a 10-class classification problem, the highest accuracy you can get on this attack is 10%.

And therefore, the highest robust accuracy (when under attack) you can reach on the 10-class dataset is 10%. This is true no matter what model you use, no matter what defense you use, no matter what training method you use. It's just a fact of the world. Claiming anything greater than 10% is outright impossible: the only possible cause is a mistake in the evaluation.

This isn't a new observation. We've written this exact observation down in two papers: [here](https://arxiv.org/abs/1802.00420) and [here](https://arxiv.org/abs/1902.06705): We even say it exactly this way: *"Regardless of the dataset, there are some accuracy-vs-distortion numbers that are theoretically impossible. For example, it is not possible to do better than random guessing with a max-norm distortion of 0.5: any image can be converted into a solid gray picture."*

This paper, though, is claiming 60%+ accuracy at a perturbation budget of 0.5. This is impossible.

![](https://nicholas.carlini.com/writing/2024/def2_claim2.jpg)

But this is not the only sign the evaluation has flaws from the results presented. The paper also claims that the defense is more accurate when it is being attacked than when it is not being attacked. At right I show an (abbreviated) figure from the paper that makes this claim on the final row: the defense claims 83% accuracy when not under attack, but 94% accuracy when under attack.

In a response to my paper, the authors argue that this is because the defense has removed noise from the inputs which has increased its accuracy. But if this was true then the function attack(x) = x would be a stronger attack than what they are using. And clearly this is a weak attack, and so the attacks the paper is using are even weaker.

This paper has other significant flaws as well, but I won't discuss these here because they are of a lesser concern.

## The Second Defense

The second defense I'm going to more briefly talk about is a paper published last year at IEEE S&P 2023. Again, I will skip over how the defense works, and focus on the evaluation which makes very similar impossible claims.

![](https://nicholas.carlini.com/writing/2024/def1_claim1.png)

At right I've again shown the same type of accuracy-versus-distortion plot. This time the y axis shows attack success rate; lower is better. As you can see from this figure, the claim here is that an attacker can do is achieve a ~30% attack success rate when you can perturb each pixel by 2.5 ... out of a maximum of 1.0.

This is even *more* impossible than the last impossible claim. With a perturbation budget of 1.0, an adversary can swap any image out for any other image at all. With a perturbation budget of 2.0 you literally can't do anything more than 1, because pixels are bounded between 0 and 1. And then 2.5 ... to misquote Babbage: I honestly don't understand the type of confusion of ideas that would lead to a paper being accepted with a max-norm distortion of 2.5 out of 1. I have never seen something this obviously wrong in any paper before, and yet somehow, this paper was accepted to IEEE S&P.

![](https://nicholas.carlini.com/writing/2024/def1_claim2.png)

But this is not the only impossible claim made in this paper. At left I show another figure from this paper that claims an adversary who can completely overwrite the center 90% of the pixels in an image with anything they want, can't succeed at fooling the classifier. This is, again, impossible: almost all of the digits in the MNIST dataset shown above have their entire content in the center 90% of the image.

And so claiming robustness to adversaries who can control the center 90% of an image just doesn't even make sense.

## Story #2: a buggy paper

Let's now return back to the defense that was published this year. Here we have a completely separate story of a hydra of a paper: every time I break the defense as currently described, it grows a new defense component that has to be evaluated.

## Breaking the defense

Whenever a defense signs of serious flaws in the evaluation, it's usually pretty easy to break. That's at least one nice thing about security: in most fields, when another researcher does something you think is wrong, the only remedy you have is to just complain bitterly and hope that someone listens. But in security, when someone designs a defense you think is wrong, you can just break it.

In this case, the attack is so simple so as to be entirely uninteresting. Here is the complete diff of my “attack” (that breaks the defense) by modifying the paper's original attack code:

**diff --git a/model.py b/model.py**

**index b8ae9f8..243edd6 100644**

**--- a/model.py**

**+++ b/model.py**

@@ -7,7 +7,7 @@ class DefenseWrapper(nn.Module):

         model = Defense(eps=eps, wave=wave, use\_rand=use\_rand, n\_variants=n\_variants)

         self.core = model

         self.base\_model = base\_model

-        self.transform = BPDAWrapper(lambda x, lambda\_r: model.transform(x, lambda\_r).float())

+        self.transform =            (lambda x, lambda\_r: model.transform(x, lambda\_r).float())

A single line change is all that's needed to break the defense. What's happening here is beyond the scope of this article. If you're interested in why this is happening you can go find my attack paper on arXiv and read it for more details. But suffice to say: it should not be this easy to break defenses published at top conferences.

## Changing the defense

I pointed out this bug to the authors of the defense, and they responded by saying that the initial release of their code was incomplete, and released a new version of the code. And that's okay: code is allowed to be buggy, especially after cleaning it up for a public release. But the problem is that the fix wasn't just to correct some mistakes, but to introduce new components to the defense that were never described in the original paper.

This is concerning. Science papers should contain the details necessary to reproduce their results, and especially the details that are necessary to ensure the defense work at all.

The authors state that it's okay they didn't include these details because they weren't fundamental to the defense methodology. And so the omission of these details from the paper isn't a problem; it's just due to the space constraints of a conference paper.

But both of these can't be true at the same time. Either the defense is insecure because new defense component isn't a necessary component of the defense, or the new defense component is necessary and the original paper should have described this component.

But, fortunately, we don't have to actually resolve this issue. Because the proposed modification is also buggy. I won't go into details about the bug here, but will just show you the break.

## Breaking the defense again

Whereas the previous evaluation was flawed because it (incorrectly) included a BPDAWrapper in the evaluation, this evaluation is flawed because it (incorrectly) omits a BPDAWrapper from the evaluation.

And so, again by changing a single line of code, it's possible to break the defense.

**diff --git a/model.py b/model.py**

**index 319aebd..69fec01 100644**

**--- a/model.py**

**+++ b/model.py**

@@ -61,7 +61,7 @@ class Defense(nn.Module):

        def precision\_blend(self, x):

        if self.eps > 0:

        precision = max(min(-int(math.floor(math.log10(abs(1.25 \* self.eps)))) - 1, 1), 0)

-       x = self.diff\_round(x, decimals=precision)

+       x = BPDAWrapper(lambda x: self.diff\_round(x, decimals=precision))(x)

Again, breaking defenses should not be this easy.

## The hydra grows another head

I shared the above (second) attack with the authors six weeks ago. They responded by saying that their prior fix is also incomplete, and needs to be adjusted again for it to work on CIFAR-10 (another dataset their paper claims they can solve robustly).

But the authors are unable to point to where the paper describes what this second change is, have been unable to provide an implementation of this change, and have been unable to provide a description of what the change is. So, for over the last month and a half, I've been stuck in a situation where the authors say they have a way to prevent my attack, but have not provided anything that can be analyzed.

And so here I am, writing about this on the internet.

This wasn't my first choice. I emailed the authors six times over six weeks asking for anything that could help me evaluate this new change to the defense, and got nothing.

It also wasn't my second choice. I went to the S&P program chairs to try and see if they could help me get the necessary information to evaluate the defense; but after several back and forths this did not lead to any progress.

And so I'm basically left with no choice but to do what people do when researchers do bad science in other fields: complain bitterly. If/when the authors provide a description of the next (and final?) version of the defense, I will update this section with the corresponding details and re-evaluation.

## Update (2024-05-28): New Code

Earlier today the authors provided me with the updated code that they say fixes the issues I've raised, and should prevent the attack I've described above. As it turns out, this new code makes even more sweeping changes to the defense (the patch makes a 300 diff to the repository). These changes are, again, not documented in the paper.

Breaking the defense with this new code is again trivial. This time instead of replacing a line of code, I just comment out one line of code. I'm not going to go into any more detail here because I'm tired of this whole saga and there's nothing new to learn from this third break. If you're interested go read my attack paper. I added a description there. I'm only updating this post because I promissed to do so.

## Final thoughts

Why is it that, in 2024, we are *still* fighting this fight, and breaking defenses to adversarial examples that had flawed evaluations? Honestly, I have no idea.

But if you forced me to guess, it would be because people don't realize just how hard it is to defend against adversarial examples. If I were to write a paper that claims P = NP, people would be (rightly) skeptical. For one I've never worked on CS theory ever before in my life. For another, I probably can't even form a valid coherent sentence with the word EXP-TIME or POLYLOG.

But even me, who doesn't know anything about theory, if asked to review a paper that claimed to prove P = NP would know to say “this seems suspicious; let's get an expert.” (And even me, who doesn't know anything about theory, would at least know that if a paper claimed P = NP when N = 1, then that's not even wrong.)

This doesn't seem to be the case for broken adversarial example defenses. They keep getting accepted. And then I have to spend an inordinate amount of time correcting their mistakes.

Which brings me to another point: I've spent nearly a quarter of my life breaking defenses to adversarial examples. If you had asked me back in 2016 if I thought I would still, in 2024, be breaking defenses to adversarial examples with obviously incorrect claims ... I would have laughed at you. But here we are.

Adversarial machine learning used to be a small field. And so I understood why, when a bad defense got published, I'd have to be the one to break it: there just weren't many people who *could* do it, and fewer still who would *want* to. But adversarial machine learning is no longer a small field. There are hundreds, if not thousands, of people who are completely capable of breaking these defenses.

And so it's wild to me that there aren't dozens of people racing to break papers accepted at literally *the most prestigious conferences in the field*.

So here's a standing offer. The next time one of the top security (not ML; too many of those) conferences accepts an adversarial example defense, if you break the defense and send me the attack before I get my break up on arXiv, I'll buy you dinner at the next conference we're both at. Title your email “YOU OWE ME DINNER”. [\[a\]](https://nicholas.carlini.com/writing/2024/yet-another-broken-defense.html#footnote1) Fine print: The paper should be at IEEE S&P, Usenix Security, CCS, or NDSS. The primary claim of the paper must be a defense to adversarial examples. Defenses in other areas that use machine learning (e.g., malware classifiers, deepfake/watermark detection) don't apply. Even if you break a paper that doesn't meet the terms please still do email me I really like to see these things. But make sure you do your evaluation properly and don't rush things; this isn't a race, I'll probably still be happy to buy you dinner even if you break it after me as long as it's a good paper and it's not like a year later.

## Conclusion

Congratulations. You now know enough about evaluating adversarial example defenses that you could have done a better job than the IEEE S&P 2023 and 2024 program committees and rejected these papers.

I feel pretty bad calling out the (volunteer) reviewers and the (volunteer) program chairs for getting this wrong. I know what it's like to have just spent your last two months pushing your own paper for the conference submission deadline, only to be rewarded by then being given a stack of papers to review when you still have your own work to be doing. Reviewing is 100% a thankless job.

But. If you're going to be a reviewer, you have to be able to do the work correctly. And here, the reviewers were either incapable of doing the work correctly, or overworked to the point that they couldn't do the work correctly. Given how completely trivial it would have been to catch these papers as being obviously wrong, I have made the decision to write this article in order to draw attention to this problem.

I also know I'm not going to be making any friends by talking about this. Several people I respect have tried to talk me out of it. And I see their point: maybe there would have been a more productive way to address this.

But I tried everything I could think of. On the side of evaluating defenses, I've written paper after paper about how to perform these kinds of evaluations. We even wrote [a paper whose primary goal was to help reviewers](https://arxiv.org/abs/1902.06705) know when a defense evaluation was good enough. And on the side of calling out this paper for not releasing a version of the code that is deemed to be final---I spent a bunch of time trying to get this code, waited six weeks, and even went to the program chairs, but this didn't make any progress. And so here we are.

Maybe slow and gradual progress over decades is the best we can hope for. But I don't think so. My hope in writing this down is that the next time someone is reading an adversarial example defense, they think critically. Because, to be clear, I'm not asking for perfection. Most defenses in security get broken; that's just the way things work in security. But usually they require some interesting novel attacks. All I'm asking is for papers that don't say the equivalent “P=NP when N=1. QED.”

Taking a step back from the reviewing process, I'm kind of worried for the future of the field of adversarial machine learning. This field is in a mid-life crisis of sorts. In 2013-2016 we set out ourselves a few problems that looked like they'd be tractable to solve, but then we discovered they were actually really, realy hard. And while we were doing that, the world moved forward, and we got machine learning models that actually worked. And now it's no longer enough to solve the toy problems we still haven't solved after a decade of work. We also have to solve the (harder) problems that are actually impacting the real world.

And so I'm concerned whenever I see conferences accept papers that try to solve the same adversarial example problem we've been studying for a decade, but make trivial-to-identify errors.

Because if we can't get *this* right, how are we going to get anything else right?
