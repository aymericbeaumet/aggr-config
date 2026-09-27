---
title: Realtime Screen Recording of Breaking a Defense to Adversarial Examples
link: https://nicholas.carlini.com/writing/2020/screen-recording-breaking-adversarial-example-defense.html
source: nicholas-carlini-com
published: 2020-09-15T00:00:00Z
updated: 2020-09-15T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: I recently broke a defense to be published at CCS 2020, and this time I recorded my screen the entire time---all two hours of it. Typically when I break defenses, I'll write a short paper, stick it on arXiv, and then move on. Pedagogically, this isn't very useful. [a] (Don't you worry, I did that again this time, too.) So for this defense I thought I'd try something different.
content: extracted
html: 2020-09-15-realtime-screen-recording-of-breaking-a-defense-to.html
---

I recently broke [a defense](https://arxiv.org/abs/1904.08554) to be published at CCS 2020, and this time I recorded my screen the entire time---all two hours of it. Typically when I break defenses, I'll write a short paper, stick it on arXiv, and then move on. Pedagogically, this isn't very useful. [\[a\]](https://nicholas.carlini.com/writing/2020/screen-recording-breaking-adversarial-example-defense.html#) (Don't you worry, [I did that again this time](http://arxiv.org/abs/2009.10975), too.) So for this defense I thought I'd try something different.

Below is the entire 2.5 hour session, keystroke by keystroke, that I went through to break this defense. The authors were kind enough to share the source code with me, and before opening up their code I started [a terminal screen recording program](https://asciinema.org/) to capture my entire terminal session. What's shown is the entire attack process, from when I looked at the code for the very first time, to a complete successful break of the defense.

I added a voiceover a few days later, where I discuss some of my thoughts in breaking the defense and the process I typically follow.

.

## Timestamps

00:00 Introducing the defense; initial code setup.\
 I spent the first ~20 minutes setting up some infrastructure to make developing attacks easier. There's not much interesting that happens here with an attack, but I talk through the idea of the defense and describe my code setup.

26:28 Implement baseline attack..\
 I next write a straightforward implementation of gradient descent. This is just standard infinity-norm regularized PGD on the cross entropy loss. Nothing interesting yet.

43:38 Baseline attack matches results from paper.\
 I confirm the results I get match what the paper reports for its attack. It's always good to make sure that I've got everything working as it should to make I've made no errors--otherwise my attack results are untrustworthy.

59:02 Begin creating stronger attack.\
 I build a better loss function by tuning some parameters, and make a slight modification to the loss function.

72:17 Break defense to AUC of 0.40.\
 I confirm that what I have so far constitutes the first break of the defense, with the detection accuracy now below random guessing. To do this, I verify adversarial examples I generated on a clean setup of the defense code.

73:40 Design even stronger attack.\
 I next start actually modifying the attack to make perform much better. The approach here splits the loss function into one of two modes, depending on if the input is already adversarial or already fools the detector.

116:15 Break defense to AUC of 0.25.\
 I'm able now to bring the accuracy to far below random guessing; the prior attack is much more successful and makes better use of the available distortion.

124:54 Further improvements on the attack.\
 I finally make a few modifications that adjust more attack parameters. I also try some additioanl improvements that don't end up increasing the attack success rate, and settle on just running the attack for longer.

141:55 Final attack, AUC down to 0.017.\
 I give some parting thoughts on what it takes to break this defense, and defenses generally.

## Concluding Thoughts

I get the feeling that people vastly over-estimate the difficulty of breaking published defenses. Breaking defenses is often a purely mechanical process without much deep thought required, and this time is no different. Applying standard attack techniques is more than sufficient to bring accuracy to (below) chance. Turning a moderately successful attack into a complete break takes a little bit of work, but it's not really all that much.

I hope that this demonstration will have at least some effect on how breaking defenses is perceived. Specifically, I hope to dispel the apparent myth that breaking defenses to adversarial examples is some kind of black magic. In most cases, defenses to advrsarial examples fail in predictable ways, with fairly straightforward attacks.
