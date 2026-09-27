---
title: InstaHide Disappointingly Wins Bell Labs Prize, 2nd Place
link: https://nicholas.carlini.com/writing/2020/instahide_disappointingly_wins_bell_labs_prize.html
source: nicholas-carlini-com
published: 2020-12-05T00:00:00Z
updated: 2020-12-05T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: InstaHide (a recent method that claims to give a way to train neural networks while preserving training data privacy) was just awarded the 2nd place Bell Labs Prize (an award for finding solutions to some of the greatest challenges facing the information and telecommunications industry.). This is a grave error.
content: extracted
html: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.html
preview:
  file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.preview-70e868019e90.webp
  width: 192
  height: 128
  color: '#a09f9c'
images:
- source: https://nicholas.carlini.com/writing/2020/instahide/instahide_is_not_private.png
  original:
    file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-9a442b2f88af.png
    width: 192
    height: 128
  variants:
  - file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-b6dcc1e0c524.webp
    width: 192
    height: 128
  color: '#fefefe'
- source: https://nicholas.carlini.com/writing/2020/instahide/cat.png
  original:
    file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-cf55fe1bb91b.png
    width: 512
    height: 512
  variants:
  - file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-c536597b3fc7.webp
    width: 320
    height: 320
  - file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-3c4a082be700.webp
    width: 512
    height: 512
  color: '#f9faf8'
- source: https://nicholas.carlini.com/writing/2020/instahide/cat2.png
  original:
    file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-c712dc36848a.png
    width: 512
    height: 512
  variants:
  - file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-ddc92d06c169.webp
    width: 320
    height: 320
  - file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-67f15509a6d4.webp
    width: 512
    height: 512
  color: '#7b7b82'
- source: https://nicholas.carlini.com/writing/2020/instahide/cat3.png
  original:
    file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-0748cbbd09e9.png
    width: 512
    height: 512
  variants:
  - file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-4ac3e60891a4.webp
    width: 320
    height: 320
  - file: 2020-12-05-instahide-disappointingly-wins-bell-labs-prize-2nd-place.image-08dd820c18cf.webp
    width: 512
    height: 512
  color: '#f9fbf8'
---

\[What follows are my thoughts on some recent research in machine learning privacy. These are my thoughts, and do not represent those of others.\]

[InstaHide](https://arxiv.org/abs/2010.02772) (a recent method that claims to give a way to train neural networks while preserving training data privacy) was just awarded the 2nd place [Bell Labs Prize](https://www.globenewswire.com/news-release/2020/12/03/2139411/0/en/Nokia-announces-2020-Bell-Labs-Prize-winners.html) (an award for “finding solutions to some of the greatest challenges facing the information and telecommunications industry.”). This is a grave error.

Bell Labs brought to the world the very foundations of information theory, the transistor, the C programming language, and the UNIX operating system. The world today would certainly not be what it is without Bell Labs. So when InstaHide was awarded the 2nd place Bell Labs Prize earlier this week, I was deeply disappointed and saddened.

In case you're not deeply embedded in the machine learning privacy research community, InstaHide is a recent proposal to train a neural network while preserving training data privacy. Its goal is to allow someone to train a machine learning model on a bunch of sensitive training data, and then publish that model, without fear that the model leaks anything about the training data itself.

*Our Attack on InstaHide*\
 ![](https://nicholas.carlini.com/writing/2020/instahide/instahide_is_not_private.png)

Unfortunately, it turns out that InstaHide offers no privacy. It is not private for any definition of privacy we can state, and given the output of InstaHide it is possible to completely recover the inputs that went in to it. We showed this in [a recent paper](https://arxiv.org/abs/2011.05315) (and if you're reading this within the next few days, we'll be giving a talk on this paper at the PPML workshop at NeurIPS).

It is an error that InstaHide was awarded this prize because of how fundamentally misguided InstaHide is---both the idea itself and the methodology of the paper. Drawn to the right is what we're able to do: given a set of encoded images that try to preserve some notion of privacy, we recover extremely high fidelity reconstructions.

I was planning on leaving things as they were after we wrote [our attack paper](https://arxiv.org/abs/2011.05315). But after watching the award ceremony, and the response to it, I felt compelled to respond.[\[a\]](https://nicholas.carlini.com/writing/2020/instahide_disappointingly_wins_bell_labs_prize.html#) And no, I promise I'm not bitter because this paper was awarded a prize even though it's broken and offers no security. I'm used to that. In 2013 I [broke](https://nicholas.carlini.com/papers/2014_usenix_ropattacks.pdf) the [kBouncer](http://www.cs.columbia.edu/~vpappas/papers/kbouncer.pdf) defense, which had received the [Microsoft Blue Hat](https://www.csoonline.com/article/2222842/black-hat--microsoft-incorporates-bluehat-prize-finalist-defensive-tech---releases-.html) prize of $200,000. I didn't complain then because at least that attack required some technical novelty and interesting research, and because Microsoft didn't award the prize *after* the attack was published.

The reason this award really gets to me is that InstaHide really is a culmination of all the weaknesses that machine learning papers tend to have, ranging from focusing on having a good story over a good technique, to the fact that the claims aren't refutable.

I would like to commend the first author of this paper, a early-career PhD student who executed on this idea with care and precision. This paper is exceptionally well written, and the experiments are performed carefully and rigorously. Having worked with the InstaHide implementation over the last month, the code is correct, easy to follow, and something that other researchers should strive for when doing their own code releases. The figures in this paper are clear and provide justifications for the claims. Everything a first author should be expected to do has been done flawlessly.

I'll go into great detail below, but to briefly summarize, here's a short explanation of what goes wrong.

- **InstaHide makes no refutable claims.** The key difference between science and pseudoscience is that claims in science are falsifiable. There must exist a way to disprove the claim. InstaHide makes no falsifiable privacy claims, and instead states their algorithm is private, without ever defining what this means.
- **InstaHide moves the privacy goalposts.** Even if the paper itself does not make refutable claims, the authors release a contest that can be solved. So we did just that: and broke the contest (completely). The author's response to this is to say that InstaHide wasn't intended to be a bullet-proof scheme anyway, and so it's not surprising their challenge can be solved.
- **InstaHide is complicated just for show.** Instead of building the simplest possible scheme that might offer privacy, InstaHide builds a scheme that is complicated because it makes the visual pictures look prettier.
- **InstaHide's extra complexity undermines the scheme.** Normally artificial complexity is an effective no-op; in this case, because of an implementation bug, the extra complexity actually allows a (new, separate-from-the-above) attack on the scheme.
- **InstaHide has spurious theorems.** Theorems of privacy are important, however InstaHide gives theorems that while mathematically true, are independent of the security or privacy of the scheme.
- **InstaHide disregards technical rigor.** Statements that are very precisely defined in the literature, such as indistinguishability, are used imprecisely. The paper argues this by running a single statistical test instead of proving (or arguing about it) it formally.
- **The InstaHide authors continue to promote it.** Sometimes defenses get broken. That's part of life in computer security. When this happens, it's necessary to admit it's broken, and try to fix it or move on to a new proposal. Instead, the InstaHide authors continue to promote their work as if it is effective, without regard to reality.

Suppose some hospital wanted to train the world's greatest medical imaging scanner that can take a scan of your body, run a fancy machine learning model over it, and tell you all the problems you have. To do this, they'd certainly need a lot of training data: images from individual people with their scans, and the list of problems they do (or don't) have.

I wouldn't want to just give some random organization all the scans I've ever had of my body. They're my private images. What if they got leaked?

Training data privacy schemes give a way for someone to train a machine learning model on this private data without having to worry that their training data will be leaked. Most schemes published today are **provably correct**: there is a formal argument that, unless there is an error in the proof, states my data can never be leaked from the trained model.

(Yes, cryptosystems like RSA or AES are not provably secure and we use them anyway. However these systems explicitly aim to be as simple as possible to make analysis easy. This is fairly independent from what we're talking about, because privacy schemes do tend to be provably secure.)

The problem is that these schemes are often slow, and usually degrade the accuracy of the final model. This is not ideal, but such is the cost of correctness.

InstaHide is a proposal for how to achieve this result with a fairly simple algorithm that does not increase model training time, and also does not cause huge accuracy drops. This is clearly an important end goal, and we should be excited by any scheme that tries to do this.

The basic idea behind InstaHide is a simple two-step process. To encode any particular private image, combine it together with a bunch of other random images, and then randomly flip the signs of the pixels in the image. (InstaHide normalizes pixels to \[-1,1\] before taking the sign.)

So what's wrong? Here we go.

## InstaHide makes no refutable claims.

One of the core tenents of modern science (for, oh, the past hundred years) is that [claims should be refutable](https://en.wikipedia.org/wiki/Falsifiability): if the claim is false, there must exist an experiment which could disprove it. This is what separates science from pseudoscience. Unfortunately, InstaHide does not make falsifiable claims. It claims to offer privacy without ever defining what this means. This is a common failing in many papers on the security and privacy of machine learning, but no recent paper exemplifies this better than InstaHide.

A typical privacy definition will say something of the form “it is not possible to learn property X about the training dataset if Y holds true”. For example, differential privacy says that it is not possible to distinguish between the case that a particular user is or is not in the dataset. The InstaHide paper has **no** refutable privacy claims. It defines an algorithm, and says it is private, without ever saying what that means.

As a result, it's impossible to ever write a paper that claims to break it, because defining an attack necessarily requires a definition to break. The best one can do (and indeed, what we do in our paper) is to define potential definitions of what InstaHide *may* mean by privacy, and show that it doesn't satisfy *those* definitions. But it's always possible that there exists some definition of privacy that InstaHide does satisfy.

Fortunately, the authors do release the InstaHide Challenge: a contest where they ask researchers to try and break the privacy of InstaHide under the strongest possible settings. Here, breaking the privacy **is** well defined: given the output of the InstaHide algorithm, we are asked to try and reconstruct the original dataset. Even if the algorithm isn't refutable, at least this contest is.

This Challenge (and the source code for the algorithm) was released **four months** after the initial paper was being presented and discussed in public. The fact that there was this huge delay meant that, for the first four months, it was impossible to say InstaHide is insecure with any certainty.

If the authors had released the challenge on time, early on when the paper was still being considered for the Bell Labs prize, would it have gotten an award? I suspect not. (Now it's still inexcusable that it got an award given our attack came out a month before the prize. But I expect if it had been broken a few weeks after being published it wouldn't have received a prize at all.)

So, the week after the authors released the challenge, we solved it. We proposed two attacks, one breaking the fundamental algorithm and another breaking the implementation of the challenge. So at least we know the challenge data is not private. But this brings us to the next issue...

## InstaHide moves the privacy goalposts.

After publishing our attack, Sanjeev Arora (the senior author on the paper) responded in a [blog post](http://www.offconvex.org/2020/11/11/instahide/) that *“InstaHide was never intended to be a mission-critical encryption like RSA”*. First: what?! Second: this was never mentioned in the paper, and was stated only after we broke it. Third: no algorithm should be designed to only offer “some” security in the best case. Algorithms are designed to be able to be (nearly) perfectly secure, and as soon as any weaknesses are discovered they are discarded for stronger approaches or fixed to maintain strong security.

As soon as it was clear that DES, (or MD5, or SHA-1) had any weakness at all, cryptographers started designing completely new algorithms to replace them. These replacement algorithms were, obviously, designed to be completely secure. Even if the defender has only very limited compute, cryptographic algorithms still argue security against adversaries who are orders of magnitude more powerful. Just because the defender can only run on 1 watt of power doesn't meant the adversary can't run on a compute cluster.

Now to get technical for a second, I should really discus security parameters. This is the value that controls “how strong” a scheme is. Many algorithms (like RSA) can be made much less secure if you pick a low value of this parameter. However, importantly, that's easy to avoid---just pick a big one! InstaHide has no security parameters, and so it can not be made more secure by adjusting a few constants.

Arguing that InstaHide is not intended for mission-critical encryption only after it is broken---and even then calling the attack *“not yet a cost-effective attack in the intended settings”*---is nothing short of moving the goalposts. And this is what you can do when you don't make any claims the first time around. InstaHide was intended to be secure. It is not.

(Briefly, on to the claim that the attack is not cost effective. The attack takes roughly 12 hours of P100 GPU time. On AWS or GCP this costs less than 20 USD to rent. Now $20 isn't nothing, it's certainly more expensive than free. But when DES was first broken it cost $250,000 USD to build the machine, and a few days of compute time to break a single key. A P100 GPU is $2500 (exactly 100 times cheaper, not inflation adjusted) and the attack is at least a few times faster. But I digress.)

## InstaHide is complicated for just show.

It is important for papers to have a compelling story behind the technique, not just introduce a technique that advances the state of the art. And often this means that by artificially introducing complexity where none is required, a paper can be made to look more significant than it is.

InstaHide does exactly this. At some point in their algorithm, InstaHide has a list of numbers \[1, 3, -5, 7, -2\]. It needs to increase the privacy of this list of numbers, so it multiplies each value by either 1 or -1 (chosen at random). So for example if we select \[-1, 1, 1, -1, -1\] then we would get \[-1, 3, -5, -7, 2\]. The claim is this somehow now preserves the privacy of these numbers.

Original Image\
 ![](https://nicholas.carlini.com/writing/2020/instahide/cat.png)

Encoded Image\
 ![](https://nicholas.carlini.com/writing/2020/instahide/cat2.png)

In practice InstaHide operates on images. Given an image represented as pixels in the range \[0,255\], InstaHide first converts each pixel to be in the range \[-1,1\]. Then, after merging a few images together, it multiplies each pixel by either 1 or -1 as shown above. So it takes this image here on the left of a cat that we might regard as private, and converts it to an “encoded” version of the image on the right where each pixel has been multiplied by either 1 or -1.

Looks pretty good, right? Really hard to see what's going on.

The astute reader might observe that all we have done is just remove the sign information of the original list. Because we've multiplied by either 1 or -1, uniformly at random, we have information theoretically completely removed the sign. Thus, InstaHide could have (more simply) just released the absolute value of each item in the list: \[1, 3, 5, 7, 2\]. This would reveal no more or less data than doing some weird sign flipping procedure.

Why bother with this sign flipping, then? Well, it turns out there's something called a One Time Pad. This is a (provably secure) encryption algorithm that *also*, in a different setting, relies on masking a random list of numbers by multiplying each entry by 1 or -1.

And so in order to say that InstaHide is like a perfect encryption algorithm, the authors have to introduce this crazy sign flipping procedure. Sure it would be simpler to describe it as taking the absolute value. But that wouldn't have a good story attached to it; it wouldn't be possible to call it something like an encryption algorithm. One time pads give a good story.

Encoded Image\
 ![](https://nicholas.carlini.com/writing/2020/instahide/cat2.png)

Corrected Image\
 ![](https://nicholas.carlini.com/writing/2020/instahide/cat3.png)

What would have happened if InstaHide had instead used the absolute value instead of flipping signs? Well, this. Clearly still a cat. But would the paper have gotten attention if it claimed that taking the absolute value was secure? Probably not.

This is why extra complexity is dangerous. It takes something that's obviously a broken idea (taking the absolute value of a pixel in order to preserve its privacy somehow) and replaces it with something that looks sane. But sane it is not.

Side note: how do I know that this complexity is actually confusing people? In the award ceremony, the Bell Labs researcher presenting the award explicitly said *he doesn't understand how InstaHide is secure*, but, and I quote, *“it works nonetheless”*. No! It does not. But this complexity is working as intended.

## InstaHide's extra complexity undermines the scheme.

Now in an ironic turn of events, it turns out that this extra complexity with sign flipping that InstaHide uses actually happens to introduce a completely new vulnerability that wouldn't be there if the authors had just taken the absolute value of the list of numbers. (To be clear, InstaHide already is broken. Our main attack breaks it in the general setting. It's just also broken as a result of a new attack because of this fake complexity.)

This implementation uses a standard psudorandom number generator (PRNG) to create the random sign flipping mask. It turns out that PRNGs have the property that, given a few outputs, it's possible to learn their *state* and as a result can recover every subsequent output. Learning the state requires a little bit of work, but it's not all that hard (it takes roughly 100 CPU hours, at a cost of about $4 USD). So we did it.

This means that we can actually *undo* the sign flipping procedure, and recover the original images. After doing this, InstaHide no longer offers any meaningful privacy. We can completely break the algorithm.

Now, if InstaHide had just taken the absolute value, then the sign information would *actually* have been destroyed. This attack would not work. But since the authors insisted on adding unnecessary complexity in order to make a show of things, they've actually weakened the scheme. Being complicated is not good. It introduces room for additional errors.

## InstaHide has spurious theorems.

There's this thing that machine learning research papers do where they propose Algorithm A, say in words it works, and then prove Theorem T. The problem is that Theorem T is often completely disjoint from whether or not Algorithm A works at all. [Lipton and Steinhardt](https://arxiv.org/abs/1807.03341) call these *spurious theorems*.

The way InstaHide makes this error is as follows. In order to justify the security of the proposed approach, authors prove that *“For worst-case pixel-vectors, finding the k-image set is hard.”* That is, they say that there exists a dataset that, if encrypted by InstaHide, would be hard to invert.

This claim is true. There does exist a dataset that is hard to invert. But that says nothing about what happens for standard images. It's not helpful to show that there exists a case where things are hard, we must show that the problem is always hard.

Compare this to something like differential privacy, where the story is completely reversed. The proof here says that **for all datasets**---no matter how pathological---privacy will be preserved. InstaHide claims only to be private on some pathological datasets.

## InstaHide disregards technical rigor.

The paper prominently states that *“Distributions of encryptions of different images are indistinguishable”* in the caption of Figure 2 in their paper. This phrase might seem bland, but realize that [indistinguishability](https://en.wikipedia.org/wiki/Ciphertext_indistinguishability) actually means something *very specific* in computer security papers: it means that *no* adversary can distinguish between two distributions with probability better than random guessing plus some exponentially small probability.

This paper asserts the distributions are indistinguishable not with a proof, but because upon visual inspection of a histogram the distributions look like they overlap, more or less, plus a statistical test in an appendix.

It would not be sufficient if a paper proposing an encryption algorithm were to plot the frequency of 1s and 0s in an encrypted message and say “our algorithm outputs data indistinguishible from random, and you can tell because the probability of a 1 looks roughly equal to the probability of a 0” but this paper makes exactly this argument.

## The InstaHide authors continue to promote it.

If everything up until this point were true, but the authors were to come out and say “we tried to introduce a scheme to increase training data privacy. it turns out our scheme doesn't work. we now understand this and are trying to build stronger schemes with privacy definitions.” then I wouldn't have written this post. The problem is that after seeing our break, this hasn't happened.

They still argue that it's secure under some settings, didn't retract their submission from the Bell Labs award \[Update 2020-12-07: The authors have informed me that they explicitly *did* disclose our attack paper to the Bell Labs committee, and that this award was given with full knowledge of our attack.\], and haven't yet (after a month) accepted our break on their Challenge leaderboard. [\[b\]](https://nicholas.carlini.com/writing/2020/instahide_disappointingly_wins_bell_labs_prize.html#) (On November 3rd we sent a draft of our paper to the InstaHide authors. After some discussion, we provided a challenge solution to the authors with the results of our PRNG attack on the 8th, followed by the code to reproduce on the 10th. After a brief back and forth, on the 11th and 12th, the authors posted their blog post and asked us questions on our attack. On the 18th we asked to check the status on this submission and received no reply. Then, on the 30th, we filed a bug report in a last attempt to contact the authors and received no reply. Finally, on December 3rd, we also provided our second (fully-general) solution to the challenge. We received a reply to this the next day.) \[Update 2020-12-06 The authors have uploaded our submission to the challenge website.\]

I really hope that we can achieve the end goals of InstaHide: privacy-preserving training that doesn't sacrifice accuracy or training time. This would be amazing.

InstaHide does not offer this, however. And not only does it not offer privacy, it doesn't even define what it tries to offer. Future papers should not try and follow this path of ambiguity and hand-waving statements without precise claims.
