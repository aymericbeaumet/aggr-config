---
title: My research idea logfile, 2016-2019
link: https://nicholas.carlini.com/writing/2024/my-research-logfile.html
source: nicholas-carlini-com
published: 2024-01-21T00:00:00Z
updated: 2024-01-21T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: How do I pick what research problems I want to solve? I get asked this question often, most recently in December at NeurIPS, and so on my flight back I decided to describe the only piece of my incredibly rudimentary system that's at all a process. I maintain a single file called ideas.txt, where I just append a new entry every time I think of something that might be an interesting topic for a paper. When it's time to pick my next project, I skim through the list, and pick whichever I think is most interesting. or exciting. or important. or whatever I'm looking for at the moment. (Or find something new entirely if nothing looks compelling.)
content: extracted
html: 2024-01-21-my-research-idea-logfile-2016-2019.html
---

How do I pick what research problems I want to solve? I get asked this question often, most recently in December at NeurIPS, and so on my flight back I decided to describe the only piece of my incredibly rudimentary system that's at all a process. I maintain a single file called **ideas.txt**, where I just append a new entry every time I think of something that might be an interesting topic for a paper. When it's time to pick my next project, I skim through the list, and pick whichever I think is most interesting. or exciting. or important. or whatever I'm looking for at the moment. (Or find something new entirely if nothing looks compelling.)

But first: this is the system that works best for me; but that means it may not be best for you. In particular, there are a two key factors that make this system uniquely suited to my usecase:

1. I'm an “independent contributor”: I don't run a lab that has a bunch of people that I can pass projects off to. This means I can only work on a few things at a time, and so picking something *has to be* an intentional decision. Each thing I do means I'm not doing something else.
2. I work in the field of adversarial machine learning, which is a is a sufficiently new field that there are millions of ideas no one has tried yet, many of which are good. This may not be the case for areas of research that have long histories, where proposing new ideas is hard, and solving the unsolved ideas is even harder. My problem is how to filter down the reasonable ideas, not how to come up with them in the first place.

But with that said, here's my process.

## A bit more detail

Each time I come up with an idea that I think is sufficiently interesting that I could imagine it being a research paper, I add it to my ideas.txt file.

Importantly, only ideas that I think (at the time) are good go in this file. Steven King has a quote about writing that I think applies here: *“a writer's notebook is the best way in the world to immortalize bad ideas”*.

I think this applies to research too. And so I try to counter this effect by only writing down ideas that I really believe are good, or might become good, at the time.

But there is one key aspect where research is different than writing books: bad research ideas can quickly become good research ideas as progress is made. Something that would be an insane idea five years ago might be a great idea today because new techniques have been developed that make the idea possible.

Writing down ideas really helps me, personally, because I can only work on a small number of things at a time. And when I'm in the middle of working on one project, if I think of something that's really exciting, I can just write it down and get back to the task at hand. This helps me avoid the problem of getting distracted by new ideas, because I know that I won't forget it and will always be able to come back to it later.

I view this file as an append-only write-only log of ideas that I only read through a few times a year. This means I don't structure it in any interesting way, I just add an entry each time I think of something that might be worth doing.

Every once and a while, when I've finished whatever my current projects are, I skim through the last few entries and see if anything that I wrote down at the time is still something I think is interesting. This filtering of ideas is useful because it means I've thought the idea was good at least twice: once when I wrote it down, and once when I read it. And the separation in time lets me evaluate it more objectively. Often times, the idea will no longer be something that's important or interesting. Other times someone will have already solved it. But on more than a few occasions, I've found good ideas that I've turned into fairly nice papers.

## ideas.txt, 2016-2019

Initially I thought about trying to write at a high level about how I go about doing this. But then I couldn't come up with any generic advice that was worth saying. (That's why I'm writing this in January, not December when I was most recently asked how I come up with ideas.)

So below I've decided to just dump my entire ideas.txt file, from when I started it in March of 2016 up through to the end of December 2019. (Stopping at 2019 gives me enough time to reflect back on these ideas with some hindsight. Who knwos, if people find this interesting maybe in a few more years I'll write a followup.)

The rest of this post won't make any sense if you're not someone who is at least somewhat familiar with the field of adversarial machine learning. These were notes to myself, not intended to be somthing I use to explain ideas to others. Sorry about that, but I don't have the time to explain the background for each of these ideas. Where possible I've tried to include links.

If you're not someone who's deeply familiar with this field, let me summarize for you a few of the main takeaways:

1. Only about one in five papers I think are good enough to write down in this list are something I end up actualy doing.
2. With reasonable frequency, I'd write down an idea for something that ended up becoming imporant, but didn't work on it it because (at the time) I thought it wouldn't matter. It just goes to show how hard it can be to judge when something is a good idea.
3. Despite both of the above, it's clearly been helpful for me to write these ideas down: I managed to get nearly ten papers out of the ideas I wrote down here.

**Original comments to myself**

**Notes on the idea as of January 2024**

Adversarial training:

1. Given classifier, construct generator which makes minimum changes to numbers in order to fool it, and then does so.

Expanding 1:

- taking an image as input, generate an output image such that:
  - l2(img,gen) < C
  - class(img) != class(gen)
- There are many ways to make the class different, with loss function equal:
  - model\[given\] ;; penalize it looking like the original
  - -model\[target\] ;; try to make it look like something else specifically
  - -max(model != given) ;; make it look like anything not this given
  - model\[given\]-model\[target\] ;; try to make it look not like this, and like something else specific
- Train 10\*9 models, one for each

Well I started off with a great idea! Adversarial training is, to date, the best defense we have against adversarial examples. Unfortunately, I didn't actually think of the "right" idea here: I was thinking of using a neural network to generate adversarail examples, but the best way to do this is to use a gradient-based attack.

Adversarial co-training:

- Train attack model and recognizer simultaneously.
- Problem: it's much easier to attack this than the original model.
- Why is this the case?
  - Is it at a nash equilibrium?
  - Or is the attacker finding something it can do better and the model instantly learning this new thing too?
- Possible solutions:
  - Train multiple attackers and require they generate different outputs.
  - Possibly let the recognizer also see the original input.
  - Let the recognizer see old attackers.
  - Remember every adversarial instance the model generates and train the model on all of them.

This idea basically doesn't work. I tried hard but nothing worked. Such is the way of research.

average distance required to fool for different classes of models, for different distance metrics:

- l0, l1, l2 as seen before
- rectangular metric
- gaussian-weighted metric for "bounding boxes"
- "unimportant regions" metric
  - can only modify pixels that aren't part of the actual object

This question of how to evalaute attacks beyond just L\_p distances is still mostly open.

How does regularization change the behavior of models adversarially?

- Does adding L2 regularization make the model more resistant to attacks?
- How much impact is there for regularization?
- Yes, attacker distance L2 goes from 4.4 mean to 3.0 from regularization weighted by 1e-5 to 1e-1.
- Does L2 regularization actually help with attacks that are adversarially generated with a model? That is, is the model which we train actually learning how to attack the fine-tuned details of the model? Or is it relying on the underlying structure of it?
- Why does L2 regularization help? Does it decrease the gradient? How much different is the network weight when we apply L2 regularization? Does this at all correlate with the difference in how easy it is to attack?
- Train more networks here with more data points in between 1e-5 and 1e-1. Also record the training accuracy with each of these.
- Does dropout help defend against adversarial instances?
- Does batch normalization?

These were some good ideas at the time. The answer turns out to be "no, regularization does not help with adversarial robustness" but I don't think it was at all obvious that this would be the case a-priori.

You can see how early on when I wrote things down here I tried to write down *how* to solve a problem, which I later realized was probably not smart, and instead started shifting to writing down *what* problems were worth solving.

Problem: We train on only actual digits, and not on non-digits.

- Solution: Use auto-encoders to determine if this thing is actually a thing of the class?
  - autoencode the thing and see if we can do it well. if we can it is?
  - project on to latent space and compute entropy of the thing?
    - require latent space follow some gaussian distribution
  - Train with other pieces of random data and have an additional "attack" class?

Also something that doesn't help much.

Train a network where the attacker can only run it forward and not backward cryptographically? That is, can we trap-door a network so that it can be evaluated forwards by anyone but backwards only by someone who knows the secret? This would prevent gradient descent in the idea of [https://arxiv.org/pdf/1610.01934v2.pdf](https://arxiv.org/pdf/1610.01934v2.pdf)

This was just a bad idea. It won't work.

Lots of systems to detect adversarial examples, can we break them?

- ICLR 2017: [https://openreview.net/pdf?id=SJzCSf9xg](https://openreview.net/pdf?id=SJzCSf9xg)
- Unpublished:
  - [https://arxiv.org/pdf/1702.06280.pdf](https://arxiv.org/pdf/1702.06280.pdf)
  - [https://arxiv.org/abs/1612.07767](https://arxiv.org/abs/1612.07767)

This ended up becoming my [5th most cited paper](https://arxiv.org/abs/1705.07263) (as of the time of writing of this post).

Try to evaluate how malicious training data could mess up your scheme.

- Take training data, take an adversary model. With probability p pick from adversary, 1-p from training data. Train model.
- Evaluate model accuracy by \[???\]
- If accuracy is lower substantially then this is bad.
- Other idea here: use this to reduce the effect of an actual adversarial training data, because we know at least some of it is.

Well this was also a really good idea! If only I actually had done this I would have been able to claim to have the first first poisoning attacks on deep learning models.

Can we train a network with a "backdoor" where we take some given values we want it to learn to misclassify and make it memorize those, without impacting the accuracy of other labels?

Also would have been a great idea! [BadNets](https://arxiv.org/abs/1708.06733) did this later in the year. I remember considering working on this paper in the summer of 2017 and deciding that it probably wasn't actually something that would be interesting ... I guess I should have listened to myself the first time around.

L0 Attack appears to produce better-than-required attacks with negative score?

I honestly have no idea what this meant.

Classify spam/ham.

- If an adversary can inject spam, can they make it easier to inject new spam? Probably!
- Measure how many poisoned inputs can cause classifier to be wrong.
  - what if poison data is correctly classified initially?
  - what if it is actually on the right side of the decision boundary?

I guess we would call this a "[clean label](https://arxiv.org/abs/1804.00792)" poisoning attack today.

Monotone classifiers:

- Learn a neural network that is monotone: for all x \\in R^n, y \\in R^n, such that x\_i <= y\_i, we have F(x) <= F(y)

This ended up becoming the final chapter of my thesis.

Is it true that more accurate classifiers are harder to fool? Looks like the answer is definitely "no"

I still don't think we know the answer to this well. Lots of papers have been written on the topic though; it's hard to really answer.

Run L0 attack, find optimal pixels to change. Use those. Now say "you can't touch those pixels". Repeat.

- How does the number of pixels you have to change increase?
- Are there some pixels thare are more important than others?

I never did this. I still think it would be an interesting question to answer.

Attacks have two different philosophies:

1. Minimize distance such that adversarial
2. Maximize "adversarialness" such that distance is within d

Should write a brief note comparing these two. Usually it's easy to convert from one to the other. (e.g., can do 2 => 1 with log(n) rounds). Converting 1 => 2 can be tricky, maybe?

People I think understand this now. It didn't need a paper.

Current black-box attacks are awful.

- Now: just use transferability. But this is really-really-black box, we don't even query it.
- Better:
  1. Build a surrogate model with your own training data.
  2. Iteratively improve the surrogate model to match the actual model.
  How? By querying on the points you're unsure about. Measure uncertainty by training a bunch of models, and finding the points that they differ most often on.

This is before the [ZOO](https://arxiv.org/abs/1708.03999) (or any of the other query-based black-box attacks) existed. The question here was, I think, correct: the black-box attacks from 2017 were in fact terrible. But my idea my idea here was wrong on how to fix it. It's kind of surprising how "hard" of an idea it was to just run a white-box attack by making gradient queries. ZOO doesn't get much credit for this anymore today, but it really did change the game of black-box attacks.

Treat as a crypto problem. We have a function F\_k(x)=y and want to learn what F is by asking questions with different x.

- How many queries do we have to make?
- Can we do better model stealing with this type of formulation?

This question is the foundation for two papers of mine that study [model stealing](https://arxiv.org/abs/2003.04884) from a cryptographic perspective. Actually, if you read this paper, we motivate it exactly this way!

Current untargeted attacks are just targeted on the nearest other class

- First: compare to see how often it's the wrong target chosen
- Second: if often, how to pick a better other target?

Not really important.

RL adversarial examples using the real RL environment, not making perturbations at the image level.

- Play against a good agent that we know is good, show our thing is at least as good at that "in the standard setting", and then show we can defeat the better thing by doing something dumb

The first papers that finally did a good job on this was only recently published.

Improving the L2 attack:

- We use binary search to choose a constant c
- Better: initially set c ~ |∇ loss| and then increase c as we go according to some schedule (exponential decay?)
- Also, reduce learning rate over time?
- Confidence is somewhat weird over logits. Better to do it over the probs and then we can set the confidence-gap to some threshold.

Still something no one has really done, but not that important.

Proofs of non-existence for adversarial examples:

- Given a sample x, network F, Lipschitz constant c
- Let gap(x) = F(x)\_C(x)-max\_{i != C(x)} F(x)\_i
- We know that \\forall x' . (x-x')\*c < gap(x) => C(x)=C(x')
- Main questions:
  1. What is the constant c
  2. What is the gap
  3. How many queries do we need to make to cover a large region?

I tried to do this. It's computationally hard and nothing works.

Detector defense:

- Make a GAN conditional on each class
- Given an input x:
  1. Compute the label C(x) = y
  2. Condition the GAN on the label y, try to generate x:
     1. Perform gradient descent on latent space to find G(z,y)~x
     2. \[Penalize the entropy of z to find a low-entropy z?\]
  3. Reject as adversarial if |G(z,y)-x|>c, or if entropy(z)>d
- Another option for a cgan: Condition on each class, then look at how good the reconstruction is. Label it with the one that has the best reconstruction.

I spent a good amount of time on this. I got it working better than the best defenses of the time but never published it becaues I didn't really think it was good enough to be worth doing. In retrospect I was probably wrong here.

Method to double the security radius:

- Given a classifier F. Accuracy on clean data is 99%
- Accuracy on adversarial within radius eps is 95%
- If we are willing to make clean accuracy 95%, can get 2eps robustness. How? Before classifying, mount an attack, find "worst" point. If it's closer than eps, reject this point as uncertain. This slows down classification.
- Also use the discriminator? We know it works well on the training data.

I have recently seen a paper that does this, but I don't remember its name.

Break captcha:

- Various captcha URLs
  - https://cmt.research.microsoft.com/ICML2018/Public/ForgotIDOrPassword.aspx
  - https://scgi.ebay.com/ws/eBayISAPI.dll?LoadBotImage&tokenString=wVb5yBIAAAA%3D
  - https://en.wikipedia.org/w/index.php?title=Special:Captcha/image&wpCaptchaId=1442326666
  - https://webapp.ftb.ca.gov/MyFTBAccess/Captcha.ashx?get=image&c=c_iusername_captcha&t=1eb3439a22e64e6d8d940543ad9a408a
  - Zoom
  - AWS amazon
  - https://images-na.ssl-images-amazon.com/captcha/lqbiackd/Captcha_urlfalkghq.jpg
  - NuCaptcha
- Construct a representation encoder f trained so that f(t1(x)) = f(t2(x)) and f(merge(x, y, r)) = merge(f(x), f(y), r) where merge(a,b,r) takes the left r% of a and the right 100-r% of b. Doesn't quite work if the text can be in varying positions \[sdf \], \[ sdf\] will be \[sdfsdf\]. Also learn a function that predicts the midpoint? Or character locations?
- Another option. Define flip(x,r) be \[abcdefgh\] -> \[efghabcd\] (for r=50%). Then train the model to predict midpoint so that f(flip(x,r(x))) = flip(f(x),r(x))

There are still a lot of papers on this topic, and still I don't think many of them are good. If you're reading this: someone who understands ML really should solve the captcha problem once and for all.

Break document summarization and make it pick a completely wrong summary of something. (e.g., make it take a document about how good something is and make it extract how bad it is instead.)

- Reference: [https://arxiv.org/pdf/1801.10198.pdf](https://arxiv.org/pdf/1801.10198.pdf)

This would also still be an interesting research problem!

Minimum bounding box adversarial example. Do it in a greedy manner the same way that we do L0 attacks. Initially find an attack using all pixels, then see which border we use less and then re-solve without those border pixels.

I remember writing this down because I still didn't like Lp perturbation attacks, and wanted to come up with something better. And at the time the only better idea I had was bounding box, which ended up becoming [a nice paper](https://arxiv.org/abs/1712.09665).

For limited-dimension subspace attacks (e.g., just cropping/rescaling) try to do "provable" minimal distortion by alternating between finding the point with highest

Uh, I seem to have forgot to complete the sentence.

Train a GAN by removing the G function and making it just be the adversary. That is, the goal of D is to discriminate between real and fake and instead of using G as a generator, just use optimization attacks. For what purpose? Maybe this will get a stronger discriminator.

- Improvement: maybe also train G simultaneously. This might act as a way to improve the quality of generated images

Crazy idea, probably would never work.

Train a network that makes it easy to find the best adversarial example.

- Idea 1: try and shift the gradient so FGM direction = true adversarial direction. Penalize cosine distance between FGM direction and true adversarial direction. Actually take the step and penalize the differences in loss values. Compute the distance between the adversarial point and the ray extending in the FGSM direction.
- Idea 2: make the classifier output one more label for "robustness bound". Penalize (robustness bound - actual robustness bound)^2. Or maybe something that's not symmetric, max(bound-actual,0)?

I don't think anyone has done this -- I wonder if it would still work. I'd guess the answer is "no" (and that's why I never tried it, I got tired of breaking all my ideas and having nothing to show for it).

1. Train a robust neural network that can predict class and "is adversarial" 2. Use this to help GAN training by using this as part of the D network.

Another crazy idea I kept thinking about and wrote down mainly so that I could feel like I could move on without it bothering me.

Can we increase robustness on CIFAR-10 by adding CIFAR-100 images to add more data? Need a way to label the CIFAR-100 images: maybe use the unified embedding paper?

Another problem that [someone else did](https://proceedings.neurips.cc/paper_files/paper/2018/file/f708f064faaf32a43e4d3c784e6af9ea-Paper.pdf) better than I was thinking when I wrote this down.

Can we detect if we're being attacked? That is, we don't want to know if this one image is adversarial, but we want to know if some sequence of queries we've received is indicative of an attack.

Another idea that ended up becoming a paper I wrote! Specifically, it became [this paper](https://arxiv.org/abs/1907.05587).

Numerical estimate of the difficultiness of finding adversarial examples. Maybe measure the effectiveness of finding an adversarial example after K iterations of gradient descent? Look at the slope of the curve? We want something small that's easy to find adversarial examples for, because it means we're probably not masking gradients. Another way: Find the nearest adversarial example of distance d Use K steps with step size a so that K\*a = d Compare d=|x-x'| and the length of the path \\sum\_{i=0}^K |x'\_i-x'\_{i+1}|

I have no idea what I wanted to do here.

Computing adversarial examples by taking the mean over random perturbations.

This is now something people do when generating black-box attacks.

Estimate dimensionality of adversarial subspace with random perturbations

I was specifically thinking that [this paper](https://arxiv.org/abs/1704.03453) was not the best and wated to improve on it. But I didn't have ideas for how.

Blackbox attack: try to learn the values that's held secret (e.g., classifying x+δ, classifying T(x), etc)

- Reference: [https://arxiv.org/pdf/1809.01715.pdf](https://arxiv.org/pdf/1809.01715.pdf)

The second time I wrote this idea down; I guess I didn't notice the first time. Well it was good idea that ended up with a nice paper.

Talk idea: how not to be seen as if we're playing with toys. We are solving hard problems, but we need to motivate it. Safety, Stop the L\_P nonsense, Actually do science and perform evaluations properly

I still haven't done this.

Understand loss in robust training by freezing weights of a pre-trained network and try to see what's different.

- Experiments to do:
  - Take a clean classifier, compare clean training with fine-tuning some layers or dropping some layers and retraining from scratch
  - Then do the same going adv-clean and clean-adv
- Result: just with clean training, all the layers seem useful.

This would be very interesting! I haven't done it though; it's probably not the most important thing.

Quantifying linearity of a neural network For some normal point x and then some other point x' close to x, compute mean\[ sim(∇(f,x\_i), ∇(f,x\_{i+1})) \] for i,i+1 interpolating from x to x'

- Compute this around images, and then around non-images to see what the space is like?
- Look at the ReLU crossings again maybe? --> highly specific though

I have no idea why I'd want to do this.

Attack wikipedia comment spam detector or google chrome extension for comment spam "Tune"

- Reference: [This for example.](https://www.theverge.com/2019/11/20/20973830/robot-lawyer-donotpay-ai-startup-license-agreements-sign-arbitration-clauses)

Would still be fund to do this. I haven't had the time but I'd like to know the answer

Training poisoning DoS. Insert some training points that make models diverge. This should be easy if the input domain is unconstrained.

Still no one knows how to do this. It's a surprisingly hard problem...

Re-visit Same-class poisoning. This is now called a clean-label back-door I believe.

I haven't done this still.

What is the volume of the region that can be reached with a path of high confidence followed by a jump of size epsilon? Start with an image X classified with F(x)\_y > t. Find a path p so that p(0) = x, p(1) = x' and so that ∀ alpha ∈ \[0,1\] F(p(alpha)) > t. What fraction of images x' can be reached in such a way? "Mode Connectivity" Freeman and Bruna 16, Garipov 18, Draxler 18

I thought this was interesting at the time, but don't any more. I don't know why I would care about this really.

A stricter adversarial example problem: find a pair of images x, x' so that |x-x'| is small but f(x)-f(x') is big. Necessary to be "correct" in general that this isn't easy.

Adversarial examples are hard enough as is. Let's not make the problem any easier for the attacker....

Try to see why gradient descent is hard for adversarial examples. Find a provably minimally distorted input with some technique. Then ask: why can't we find this one with gradient descent? Interpolate between x and x' and look at loss function. Is there no decreasing path from x to x'? Is there a decreasing path but other paths are "better"?

This became [a paper one of my interns wrote](https://arxiv.org/abs/2206.13991).

Looking at transferability. Don't just judge based on "success / fail" but, for each example, quantitatively measure "how successful" it was. Candidates: probability gap (p\_t - max\_{t' ≠ t} p\_{t'}), logit gap \[same as above but with logits\]. Or maybe a more interesting: how much "work" is required to fix it. x: original, A: attack algorithm, x'=A(x,F): adversarial on F now compute x\_1 = A(x, G) and x\_2 = A(x', G) And compare ratio |x'-x\_2| / |x-x\_1|

I think other people have wrote a bit about this, but not in the way I'd like.

Train a NN on reviewers to de-identify reviewers.

This is one of those things that I'd like to know is possible, but seems annoying to do, and also has some ethical consideratinos. Stylometry is a thing and I feel like we could do even better but I don't really want to be someone who does this.

Backdoor in a way that can't be detected. Might be hard to do "can't detect if it was backdoored or not" But should be easy to do "can't detect what the backdoor key is"

[Getting scooped](https://arxiv.org/abs/2204.06974) by a Turing award winner doesn't feel so bad.

Re run the features vs bugs experiment. For image X\_i generate perturbation P\_i. Now encode X\_i + P\_j for i != j. Does it work?

I don't know the answer to this!

Attack the L0 detectors: [https://arxiv.org/pdf/1812.09638.pdf](https://arxiv.org/pdf/1812.09638.pdf) the NeurIPS paper last year

I did end up breaking this in [this paper](https://arxiv.org/abs/2002.08347).

Attack the speech defense papers: Bo's paper \[I think I saw an attack on this one recently\], "Adversarial Regularization for Attention Based End-to-End Robust Speech Recognition"

Another paper we broke in [this paper](https://arxiv.org/abs/2002.08347).

Does having access to intermediate models leak more data? Assume adversary has checkpoints from every N batches. Compare this to just having M total models.

Ah! My first privacy question that I wrote down here. For a long time I spent most of my time thinking about adversarial examples and so only wrote those questions here, but I guess you can start to see the broading out of my interests here. This is something people have been thinking about quite a lot in the last year or so. It's been interesting seeing how much progress there is on this topic.

Can we get a much better estimate of the adversarial dimensionality? Find the worst-case direction, then the next-worst direction that is orthogonal to the first. Then repeat. Plot \[dimension number\] versus \[distortion required\]. This has problems. Maybe optimize pairs (a,b,c) so that all are mutually orthogonal and we maximize mean distortion of a,b,c. Then pick the one that's most-adversarial and then repeat for b,c,d with them all being orthogonal to a. What happens for adversarially robust classifiers? What if we just generate K all at the same time orthogonal and measure the average loss compared to each other?

Back to adversarial examples. I don't know why this is important though.

Poison semi-supervised learning. What if can only introduce an example, but NOT A LABEL? How to do this? First: understand training dynamics. Then: try to grow the region of points in the desired direction.

I came up with this idea while working on [MixMatch](https://proceedings.neurips.cc/paper_files/paper/2019/hash/1cd138d0499a68f4bb72bee04bbec2d7-Abstract.html), and then wrote a paper exactly on this topic [at USENIX Security](https://arxiv.org/abs/2105.01622).

Attack deepfake classifier [Adobe Photoshop AI Detector](https://www.fxguide.com/fxfeatured/adobe-photoshop-ai-detector/)

I never actually managed to to get around to doing this, but it did turn into a different paper. Still on deepfake detection, but attacking different defenses instead of this one.

Attackb the c0 discontinuous adversarial example defense at iclr

Yet another paper we broke in [this paper](https://arxiv.org/abs/2002.08347).

Membership inference attacks on GPT-2 to determine if articles were present.

I wrote this idea down after a discussion with Dawn Song at NeurIPS 2019. We never ended up writing a paper exactly on this, but ... let's go to the next row.

Secret-sharer attack on transformer models.

This second idea was much more interesting: trying to actually extract data from the GPT-2 model. And this paper actually ended up becoming [Extracting Training Data from Large Language Models](https://arxiv.org/abs/2012.07805), a paper that's since become very important in the field.

Experiment: take MNIST and add an all-black/white image to the training set. Train a model. What is the confidence of the new image? What was the confidence before? Problem => maybe it's high always. Solution => find an image that's consistently (over many model) low-confidence. Insert that image, and see what happens?

I had the opportunity to host Milad Nasr as an intern in 2020 and we wrote [a paper on this topic](https://arxiv.org/abs/2101.04535).

New adversarial example failure mode analysis: train with fewer samples.

I haven't tried this.

Given a loss function for a defense, how do we tell if it's a good loss function? We have tried dumb things: eventually we succeed with enough distortion. Are there better methods? If for input x we had advex x', we could lerp(x,x') and look at the value over time. => not guaranteed to be correct in high dimensions, there might be a low dimensional path. Given x and adversarial example x', compute minimum number of linear segments \[x, x1, x2, x3, ..., x'\] such that loss function is monotonically decreasing. => there exists \*A\* path that is easy to find. Maximize the loss function instead of minimize. Check if \*that\* is possible.

This is the same research question as I posed in one of the ideas earlier. I guess writing things down twice happens when you like the idea. This became [a paper one of my interns wrote](https://arxiv.org/abs/2206.13991).

New product to attack: apple's photo clustering? Photo's search?

Not sure why I'd want to attack this, but you can see a shift a bit in some of my research interest in how to make attacks practical.

Attack these papers: Training DNNs with improved adversarial robustness. NeurIPS'18. Robust detection of adversarial attacks by modeling the intrinsic properties of deep neural networks. NeurIPS'18. A simple unified framework for detecting out-of-distribution samples and adversarial attacks. NeurIPS'18. HyperGAN? Adversarial defense by restricting the hidden space of deep ICCV. Interpret neural networks by identifying critical data routing paths. Adversarial defense through network profiling based path extraction.

These mostly turned into [this paper](https://arxiv.org/abs/2002.08347)

Perceptual adversarial examples with LGN model. See: [YouTube Video](https://www.youtube.com/watch?v=NWws_mk1d6o)

I haven't thought about this at all. Maybe it would be interesting?

## Conclusion

I've always found it fascinating to learn how other people approach their ideas. If you found this interesting, Leslie Lamport writes [a short retrospecitve paragraph](https://lamport.azurewebsites.net/pubs/pubs.html) for each of his papers which I found fun to read; Ian Goodfellow [published the git repository](https://github.com/goodfeli/galatea) he used throughout his PhD; and maybe most famously John Carmack published his [.plan files](https://github.com/ESWAT/john-carmack-plan-archive/tree/master/by_year) for what work he was doing at any moment in time.

If you have an interesting process I'd encourage you to share yours. And I hope you found mine interesting.
