---
title: How to win a best paper award
link: https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html
source: nicholas-carlini-com
published: 2026-03-09T00:00:00Z
updated: 2026-03-09T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: An opinionated perspective on how to do important research that makes a difference (and sometimes win awards).
content: extracted
html: 2026-03-09-how-to-win-a-best-paper-award.html
---

At [EuroCrypt](https://eurocrypt.iacr.org/2025/) last week month year [\[a\]](https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html#footnote1) Yes, it did take me eight months to write this blog post. I am slow at writing. I was honored to receive a best paper award for [a model stealing paper](https://eprint.iacr.org/2024/1580) I wrote with my co-authors Jorge Chávez-Saab, Anna Hambitzer, Francisco Rodríguez-Henríquez, and Adi Shamir. While there, a few people asked what I do differently to win awards like this. I told them what I tell everyone: honestly I don't really know and it's mostly just luck. And while I do think this is true to a large extent (whether any particular paper wins an award is mostly luck), it's clearly not all down to luck, and I thought I actually owe it to others to give a more actionable response. So this is my attempt at that.

More generally, this article covers my process (for my field! yours may differ.), and gives an opinionated perspective on how research should be performed, and how papers should be written. I'll break this post out into roughly four sections:

1. coming up with a good [research idea](https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html#idea)
2. performing good [technical research](https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html#doing)
3. [writing](https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html#writing) understandable and compelling papers
4. ... and then what happens [afterwards](https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html#after)

### Coming up with your best-paper-worthy idea

#### Have good taste for problems

The single most important skill to develop for high-impact research is good taste in what problems are worth solving. If you have good taste, and you keep writing papers, eventually you'll write one with exceptionally high impact. But if you have bad taste, you could write a hundred papers and never do anything of consequence.

Researchers who have developed good taste find clever and elegant approaches, and find themselves pulled toward solutions that feel "right" before they can fully articulate why. At a macro scale, they pick problems that will matter; at a micro scale, they take approaches likely to succeed. And perhaps most importantly, they make these decisions early, before wasting months of effort.

People who haven't refined their taste frequently spend months on problems that don't matter, or take approaches doomed from the start. They write papers that are technically correct but that no one reads, because they never asked whether the question was worth answering.

Unfortunately, teaching taste is hard. What makes an excellent PhD advisor is someone who has learned to teach this---and that's not me. But if I had to summarize it in one sentence, it would be that taste comes from practicing the skill of research, keeping your focus always on identifying what works and what doesn't.

In slightly more detail, continue reading the rest of this section, which will discuss everything that I think is worth doing to refine your sense of taste, and to put yourself in a position where good ideas will find you.

#### Have great collaborators

No one does science in isolation. The only way to get anything done is to build on the ideas of others, and having *others* to work with helps immensely.

Good collaborators do more than give you someone to offload work onto. They catch your mistakes before someone else does. They push back on bad ideas so you don't waste time. They bring skills you don't have. And they give you someone to bounce ideas off, to figure out what you should be doing in the first place.

Saying "have great collaborators" is not very actionable. And that's mostly by necessity: there is no one step that gets you great collaborators. But you have more control over this than most things in life. Research is extremely amenable to collaborations that span the world.

Almost all researchers are trying to write great papers; if you have an idea they could help with, you'd be surprised how often a simple email works: "Hi \[person\], I am working on \[project\] and have done \[steps 1-3\]. I am now working on \[problem\] and believe \[your work\] would help. Would you be open to meeting to discuss?" Or, another format, "Hi \[person\], I saw that in your \[paper\] you proposed \[problem\]. I have solved \[steps 1-3\] and believe that with your help we could address \[step 4,5\]. Would you be open to meeting to discuss?"

For instance, [the EuroCrypt paper that prompted this article](https://eprint.iacr.org/2024/1580) happened because Adi was giving a talk at Stanford on [his 2023 paper](https://eprint.iacr.org/2023/1526), itself an extension of [one I wrote in 2020](https://arxiv.org/abs/2003.04884). I had never worked with him before, but after the talk I asked whether it would be possible to extend the work in a particular direction. He said he didn't know how, but thought it would be interesting. So I spent the next 12 hours working out a partial solution, and sent him an email with what I'd found, asking if he had ideas for how to proceed. He did, and that became our 2025 paper. (Please note, I'm not suggesting "just email a Turing award winner" as a general strategy, but rather, if you email someone a solution to a problem they appear interested in, you're far more likely to get a reply (or even a paper) than if you just send an email "I love your work can we collaborate". The probability someone will respond to you increases dramatically with the amount of time you put into sending it.)

As another instance of this happening in reverse, in 2017 Anish Athalye emailed me asking me to break a paper he was working on for ICLR'18. I broke it, and we realized the attack was general enough that we should write a paper on it. So we broke the other ICLR'18 papers with a generalization of this attack, which became [our ICML'18 paper](https://arxiv.org/abs/1802.00420).

Cold emails aren't the only way to find collaborators. The best use of conferences is to find people who like similar problems and just talk to them. If something comes up, great, you've got a collaborator. Or you'll have worked with X for a while, they'll have worked with Y, and X invites you and Y to work together. Broadly, just being around, available, and having interesting ideas you're willing to discuss is an easy way to find collaborators.

(I say "interesting ideas *you're willing to discuss*", because many people keep their ideas close to their chest as if someone else will steal them. I think this is generally not worth worrying about: ideas are cheap; execution is hard. Most researchers already have far more ideas than time to pursue them, so whoever you're talking to almost certainly won't drop everything to scoop you. And even if they did, they probably wouldn't execute as well as you---you've been thinking about it longer and have enough of a head start they'd be wasting their time. So share your ideas more freely! I do.)

#### Read all the papers

Okay, so you have a team to complement your skills and make up for your deficiencies. The next step is to learn from everyone you're *not* working with. Fortunately, this is easy: the entire reason we publish papers is to teach the scientific community something new. So all you have to do to gather the wisdom of everyone in the world is read as much of the literature as you possibly can. You can't build on the body of scientific knowledge without knowing what that knowledge is.

This can feel daunting, especially when your research area is large. I'm sure entire books have been written on how to read papers efficiently. But in a few words: I try to be very intentional about *why* I'm reading any given paper, and typically have one of three mindsets:

- **To get a sense of what is possible.** For most papers I "read" I'm just keeping up with what's known, and the only thing I hope to take away is (1) is this paper useful, and if so, (2) what's the new thing that makes it useful. In a good paper this is answerable in a sentence: your goal is to find that sentence. (As a writer, your job is to make that sentence easy to find.)
- **To understand what you have to do to achieve some result.** If a paper is related to your work, read it more carefully. You still don't need to understand every last detail, but you should read the pieces you need in enough depth that you understand that one aspect of the paper. Maybe they used a clever proof technique, or their experimental setup is particularly elegant, or they introduce a new algorithm you're going to use. Extract what you need without getting bogged down in the rest.
- **To extend upon the paper.** For a very small fraction of papers---a few each month---I do actually read the full paper top-to-bottom and think critically about every sentence. Here you're looking to understand everything well enough that you could, without referring back to it, reproduce the work. This helps you answer: what implicit assumptions did they make? What did they leave for future work? What errors did they make?

For example, one of the papers I'm most proud of is my [membership inference paper](https://arxiv.org/abs/2112.03570). When I started reading that literature, very little of it made sense to me---the attacks seemed confused and overcomplicated, training neural networks on the output of models in weird ways that made no sense to me. The evaluation metrics were weird too. The paper I eventually wrote argued the field should use a much simpler approach---treating membership inference as the hypothesis test it actually is---and evaluate using a different metric. A few years on, that's what everyone does. But I couldn't have made that correction without reading everything first, so I could target my argument to land. You can't fix what you don't understand.

#### Ignore all the papers

Now here's an apparent contradiction. Once you've read everything, the second step is to forget it all. The reason is simple: everything that's already been done has already been done. If you constrain yourself to thinking only about what's been done, you'll never come up with something clever and new.

It's especially important not to get swayed by bad ideas---it's a bit taboo to say, but every field has them. These are directions where someone (usually someone famous!) published an influential paper that got something critical wrong with the approach, methodology, or evaluation... and then the rest of the field followed along the same wrong direction without thinking critically about the initial choice, because that's just how the field does things.

Another way this can happen is that, when a research area is young, an early paper makes some arbitrary decision that was never well justified (and the author knew it!) in a rush to publish. Then everyone else just goes along with this bad idea for far longer than they should. If you pay too much attention to how the field does things, it's easy to subconsciously accept these bad ideas as good.

Another reason ignoring the literature can be helpful is that sometimes a bunch of work tries to solve some problem, and so everyone assumes it must be hard---just because no one has solved it yet, even though no one has really tried a fundamentally different approach.

The final failure mode is that, once you've read a bad idea, it's hard not to let it poison your thoughts. Once you've seen ten papers use Approach X, you implicitly assume Approach X is the right way, even if it's not.

Again, back to the membership inference paper. When I first saw the early papers, I thought "that's a nice idea; but nothing about these methods makes sense" and so I put out of my mind how they worked and didn't touch the field. Only after I saw [a paper that gave me an idea](https://arxiv.org/abs/2008.03703) for how to do membership inference in a way that made sense did I (1) develop my own attack, and then (2) read what everyone else was doing. The ordering here---doing the work first---helped me not get distracted by the wrong approach. (Of course, you have to be careful when doing this! You don't want to spend months re-inventing what already exists, or worse, re-inventing wrong ideas people have discarded.)

#### Pick your ideas for impact

Some people go into research with the objective of writing a conference paper. Don't do that. You need to go into research with the objective of discovering something interesting, important, and new. The paper follows as a natural consequence.

High impact research does not come from the goal of being adequate. You can't ask “what is the minimal contribution I can write that will technically still count as a science paper?” You're doomed to fail at having high impact if you take this approach. Or, maybe, you're doomed to succeed at your goal---writing mediocre papers with little reason to exist.

(It's a sad truth that marginal and boring papers are more likely to be accepted than ones that do something truly interesting. But you'll never write an important paper that way. Learn early to have a thick skin; accept your rejections.)

[Hamming was famous for asking](https://www.cs.virginia.edu/~robins/YouAndYourResearch.html) "what's the most important problem in your field, and why aren't you working on it?" Approach your research with this mindset. One excellent paper is worth a thousand mediocre ones, and takes less time to write.

This doesn't mean every paper you write has to have grand ambitions. Sometimes you don't know what the best paper to write is. Sadly---at least for me---*sit around and hope for inspiration* isn't a valid approach to coming up with important ideas. At some point the marginal hour spent trying to find a good idea is less valuable than just working on ... anything. So when you don't have a hugely important idea, it's entirely reasonable to just write something to practice your craft. In any year I usually have one or two papers that I really think have the potential to be great. The rest I write because writing good-enough papers is what helps me discover the ideas that lead to important ones.

#### Do something only you can do

Try to do something only you can do. Find a paper that someone else won't be submitting to the same conference. Because if all you're doing is something someone else would have done, have you really contributed anything at all?

To be clear, there's a spectrum here. Some people have ideas no one else would have come up with in a lifetime. They're all famous, have already won their Nobel Prize, and aren't you or me.

Everyone else has to settle for peering a few months further into the future than others could manage. All ideas are waiting to be found, and within a few years, anything you find would have been found by someone else anyway. So the magnitude of your contribution is judged, in a very real sense, by counting the months between when you publish, and when the next person would have. Try to pick something that would have taken at least a few months for someone else to do as well as you did.

Something subtle that's hard to appreciate is that if you write a high-quality paper early, you can have an outsized impact compared to writing that same paper even a year later. People settle on "how things are done" early, and altering the direction of a field is much harder than starting it in the right direction. So especially when fields are young, writing something six months earlier can be exceptionally valuable if those six months help shift the community towards the correct framing of the problem.

How do you know when you have something only you can do? I look for areas where, when I read the work, I want to scream. Areas where you keep asking yourself "why is everyone doing this obviously wrong?"

#### Find your comparative advantage

It's also helpful to discover along which dimensions you have a comparative advantage. In mathematics, Gowers writes that mathematicians [are either "theory-builders" or "problem-solvers"](https://www.dpmms.cam.ac.uk/~wtg10/2cultures.pdf); the former is more interested in developing theories of how math works, while the latter just wants to prove (or disprove) statements. Both are necessary to advance the field, but any individual might be better suited to one than the other.

Fortunately there's not just one direction of variance, and the space of variables differs between research areas. Because this space is high-dimensional, it's highly likely you are the best in the world at some specific corner of it. Find that corner.

As an example: I'm not very good at proposing entirely new research directions. I've never written papers proposing entirely new directions, or developed completely new strategies for approaching a problem. But I am reasonably competent at taking a fuzzy field someone else defined, and bringing clarity and order to how things should be done. I'm not the first to pose the problem of adversarial examples, but I laid the groundwork for [how to properly evaluate adversarial robustness](https://arxiv.org/abs/1902.06705). I'm not the first to show that language models can memorize training data, but I showed it [in a much more visceral way](https://arxiv.org/abs/2012.07805) than anyone else. Once a field has matured, I'm less good at doing the rigorous science necessary to drive things forward, so I move on to something new.

There are plenty of other directions of variance. Another thing I've done a bunch is to take ideas from distant fields and bring them together. Model stealing existed in the machine learning community since 2016; I found a connection to differential cryptanalysis that resulted in a series of papers, including the EuroCrypt'25 paper that inspired this article. Or a few years back I helped write a few papers ([MixMatch](https://arxiv.org/abs/1905.02249), [FixMatch](https://arxiv.org/abs/2001.07685)) that developed improved semi-supervised learning methods. I observed that because semi-supervised methods train on large unlabeled datasets, adversaries might be able to poison that data more easily; and so I turned this idea into a paper that received [a Distinguished Paper Award at USENIX Security 2021](https://arxiv.org/abs/2105.01622).

You should try to figure out where you have your differential advantage. There are probably three or four ways that I've found I can combine my skills that other people seem to find challenging. Not every problem needs to be one you're uniquely positioned to solve, but keep an eye out for when one comes along.

#### Get lucky: you happen upon a good idea

But above all else, the most important thing here is to get lucky.

I'm one of those people who sees research as not unlike being an early explorer. You have some control of the situation: if you see a lush forest in one direction and a barren desert in the other, you probably shouldn't head into the desert without reason. But sometimes, from where you sit, there are two equally good options and you're forced to just pick one. One may lead to wonders; the other to a cliff edge with nothing interesting to be found. You can't know which is which in advance, and you just hope you choose right. (Having good "research taste" is what helps you predict how fruitful a path will be before you explore it.)

Half the papers I've written weren't from some deliberate thought process, but from a spontaneous conversation, or because I was thinking about some problem when I happened to read a paper that introduced a tool I could use. You can do everything right, and for reasons entirely outside your control, not end up with excellent ideas.

This isn't to say it's all luck (the last several sections were dedicated to the ways in which it's not), but no one can deny there's a lot of luck here. Fortunately, as has been said a million times, you still have to take advantage of luck when it finds you, and that takes practice.

As an example, I wrote [a paper a few years ago on poisoning web-scale training datasets](https://arxiv.org/abs/2302.10149): we showed it's trivial in practice to get malicious text into the largest vision- or language-model datasets by modifying a small amount of content on the Internet. I didn't go "looking" for this paper; I wasn't trying to come up with a practical poisoning attack. I had just written two papers on model poisoning in the [semi-](https://arxiv.org/abs/2105.01622) and [un-supervised](https://arxiv.org/abs/2106.09667) setting, came across the [LAION-5b](https://laion.ai/laion-5b-a-new-era-of-open-large-scale-multi-modal-datasets/) dataset, wanted to study it, and noticed an attack I could mount. (Namely: buy expired domain names for images in the dataset.) There was some skill here---I had to notice the attack was possible---but everything really just fell in front of me.

#### Get lucky: you're well suited to solve important problems

Let me talk about another aspect of luck that I think goes under-appreciated.

Some problem areas are just more important than others given the current state of scientific knowledge. Take the world's most capable programmer and drop them in 1600s Europe and their skills would not be very useful. Take the best blacksmith from the 1600s and bring them to today and they're not going to have much impact on science or the world. And this is through no fault of their own---at any moment in time, certain skills are on the critical path of scientific discovery.

This is true not only at the level of entire careers (blacksmithing vs programming) but also of individual problems within a research area. For example: I am probably the world expert on breaking defenses to adversarial examples in the image domain. Through a confluence of lucky decisions, I got pretty good at that. A decade ago when the world needed people who could do this, that skill got me a few best paper awards. But today, despite being a better breaker-of-adversarial-example-defenses than I was a decade ago, I'm not going to win any more awards for it because the world doesn't need any more image adversarial example attack papers. That field has been thoroughly explored, and new directions need exploring.

What does this mean for you? Two things. First, unfortunately, if you're excited about and skilled at a topic other researchers don't see as important, you'll have a harder time getting recognized. But second, fortunately, if you're in an area that's not the "hot" one, you'll have a much easier time standing out.

This isn't to say you shouldn't do work that's not viewed as "important" by the world at this moment. I feel pretty strongly that your overall impact is the product of your skill in some area, multiplied by how important that area is. If you're very good at a less "important" problem, you'll contribute more than working on something you're not very good at.

I got particularly lucky ending up on the critical path for adversarial machine learning: I spent the first few years researching computer security doing fairly mundane things. The papers I wrote were important enough and got a few hundred citations each, but they weren't going to fundamentally change the research landscape. But then machine learning started becoming increasingly important, and I found myself one of the few security researchers now studying machine learning. As machine learning kept getting more important, so did my work.

At some point this field will taper off, and then I'll no longer be lucky in this way. I'll keep working on these problems because I find them fun, but it will be someone else's turn to get lucky that their life decisions led them to where the problems they're uniquely positioned to solve are the ones other researchers consider most important.

(Given this, what makes a truly great researcher is someone who has re-invented themselves and solved important problems in one area and then, after the world changes, became someone who could solve them in another. I have no idea how to do that---I've never had to. But people who have done this impress me far more than one-hit-wonders.)

### While doing the best-paper-worthy research

Great! You've found an idea that's worthy of a best paper award. It's an important problem, you have some critical insight that's novel, and you think you have a path to completion. Now you have to execute on it flawlessly.

#### Get lucky

But first, let's pick up where we left off. There's going to be a lot of luck on this step too. Most ideas that sound really good die when they make contact with reality. You can't predict what will or won't work ahead of time (that's what separates science from engineering), so you have to get lucky that the idea you're trying will actually work.

I can't tell you how many ideas I've had that in any just world would have been amazing, but ultimately didn't work out. The best thing here is to accept that not every idea you come up with will work, and come to terms with the fact that what works and what doesn't is mostly out of your hands. There's something to be said for really going all-out and trying your best to make something possible, but trying harder won't let you prove a statement that's false.

#### Kill papers that are not working

Because of the luck in which ideas work out and which don't, you need to work on many potential ideas to find the ones that work. Almost all good papers have a degree of risk to them: if it was obviously going to succeed someone else would have done it already. So it's important to try many ideas, and you can only do this by *not* working on bad ones.

I start probably five times as many papers as I finish, and each of them I thought was a good idea that had merit, but things just didn't work out as I expected. The best way I've found to minimize wasted time is to start with the sub-problem most likely to fail. It's comforting to work on the parts of a problem you know how to solve, but this teaches you nothing about whether your idea is any good. You want to de-risk the project as quickly as possible.

In this way, writing a paper is a lot like forming a start-up. Fail fast applies just as well here: don't build the polished version of your experiment when a small prototype will tell you whether the core idea works. If the prototype shows promise, clean it up later. If it doesn't, you've saved yourself months.

#### Kill papers that end up having low impact

As Stephen King says: ["kill your darlings."](https://en.wikipedia.org/wiki/On_Writing:_A_Memoir_of_the_Craft) Let's say you've just spent a month developing an idea that's "working" insofar as the technical contributions are there, and the results are trending positive, but it's just not working out as you hoped. You could probably, with some work, turn this into a paper. But it's not going to become a *good* paper. When this happens, just kill the idea and pick something new. Maybe turn it into a workshop paper or a blog post. Still try to get some value out of it. But don't waste your time.

#### Re-prioritize projects ruthlessly

Finally, sometimes you have to kill papers not because there's anything wrong with the one you're working on, but because something else comes along that's even higher impact. When this happens, don't fall into a sunk cost fallacy; pivot immediately to the more important paper. This is *"it's not you, it's me"* applied to research. (Because the impact of papers is distributed exponentially, even if you're 90% of the way through a "just ok" paper, the marginal return on spending the last 10% to finish it versus spending that same 10% on a paper 100x more important makes the latter the clear winner. And some papers truly are 100x more important than others.)

You have to be careful with this advice, though. For one, it requires accurately assessing the impact of a paper before it's done, which is remarkably hard. But even if you have excellent taste in problems, new ideas always sound more exciting than ones you've been working on for a few months, so it's easy to get distracted by some shiny new problem just because it's new.

As an example, I was in the middle of a few other pretty interesting papers when GPT-2 came out, and I realized this was probably something important to study. Instead of continuing with whatever I was doing, I just dropped that work on the ground (I don't even remember what it was anymore) and started looking at these new language models. (I guess that turned out to be the right decision, and in hindsight I probably should have pivoted harder and earlier.)

#### Put in an unreasonable amount of effort

*"Sometimes magic is just someone spending more time on something than anyone else might reasonably expect."* -Teller, of Penn and Teller

The same is true for research papers.

Great papers don't have poorly performed experiments. Part of doing high quality science is not only that you've got a really good idea, but that you went to unreasonable lengths to execute on it better than anyone would reasonably expect.

Every experiment should be as precisely performed as you can manage. Run multiple trials. Control for confounders. If there's a question a skeptical reader might ask about your setup, run the experiment that answers it before they ask. On multiple occasions I've put in several hours of work just to strengthen a "sometimes" to "usually", because this is what it means to care.

Earlier I made an analogy to being an explorer; here's another I like even more. Think of yourself as a wildlife photographer. Obviously you need to be in the right place (you won't get a great picture of anything from your couch) and you need to be skilled at your craft. But once you've met those preconditions, the way to get the best picture is to just spend an unreasonable amount of time waiting for exactly the right circumstances to arise.

#### Have focus

Remember that the goal of a paper is to advance a single idea. This idea can take many forms, and it should be expressible in as few words as possible. I find it useful to actually write down this idea somewhere well before I start writing the paper, so I don't forget what I'm trying to do.

Every experiment should connect to the core idea. Every paragraph and every figure you'll eventually write should be directly attributable to the goal of the paper. (Have I mentioned it's important to have a singular focus? You should.)

Because the objective of the scientific process is to understand what's true about the world, a common trap is trying to run every possible experiment that relates to your new idea. This doesn't work. First, if you actually tried everything, you'd never do anything else. But also this will just distract from your key idea. Readers will forget why you're doing whatever you're doing, and lose track of the core idea. On occasion you have to do this (e.g., because a reviewer requires it), and then you must be careful to bring back the reader's focus at the earliest possible opportunity.

#### The paper should be the "maximal" version of the paper

Counterbalancing the above, make sure the paper you're writing doesn't have obvious improvements you didn't do. That is: your paper should be at a local optimum; the reader must not be left with the thought "I really wish the authors had done X; that would have been an obvious improvement."

Put differently, when the reader is finished, they should feel satisfied as if they'd just finished an expensive meal. Sure, maybe they thought the portions were a bit small, or would have enjoyed a third appetizer. Everyone wants something different; you can't satisfy every preference. But they shouldn't feel like something essential was missing. And they certainly shouldn't feel like something was spoiled.

In your papers, this means that you shouldn't leave obvious questions unanswered. If you propose a method that obviously could be applied to domain X, then you should do that. If your result implies some trivial lemmas, you should discuss those. If there's some critique that you expect many people may have, you should address that.

But don't go too far; to extend the metaphor: it's okay to leave room for dessert. Having small improvements left over is actually a good thing, because it gives someone who's finished reading your paper a way to engage with it further. If you introduce a new tool with various applications, leaving open a few interesting (but not critical) directions makes space for others to pick up your ideas and work on them, expanding the number of people working on this new topic you presumably care about.

### When writing down the best-paper-worthy research

You've finished your research, and all the experiments turned out exactly as you wanted. It's time to write things down. If you're hoping for a best paper award, this is one of the most important steps: most papers are written poorly, so you need to actually write yours well.

But even if you're not hoping for a best paper, most people only read a few papers on any given topic. Yours doesn't have to be the most technically sophisticated---someone else will design a better algorithm in a few years anyway---what really makes a difference in how often people read your paper is how easy it is to read and how approachable it makes your field.

One of the papers I think I did the best job writing was a paper I discussed above on membership inference attacks. My goal with this paper was to try and change the way the field studied the problem, and so I spent much more time writing than I normally do. When we wrote it, our method was state-of-the-art. But so is the method in every paper when it's first written. The reason this paper has been cited so frequently over the last few years is that we did a far better job writing down what a membership inference attack is than anyone had done before. So even though our exact method no longer really matters, the fact that our paper is approachable and readable means it's now one of the canonical papers people point to for this field, and so it's had an exceptionally high impact.

So in this section I'll tell you what I do to make my papers accessible and informative.

This is the part (1) I feel the least qualified to write, and yet (2) feel the need to write the most. As someone who got C's in high school English and had to take remedial writing classes my freshman year of college, writing well does not come naturally to me. But in some sense I'm also better positioned to write this section than the last two. I've never really struggled with programming. I don't know what it means to be someone who isn't good at it and had to teach themselves to be better. But I do know what it means to be bad at writing, and I do know it's possible to become someone who's better at it.

#### Have focus

Earlier I told you to have focus when performing your experiments. Now I'm going to tell you to have focus when writing them down.

Research papers should have exactly one idea. You're allowed to support this idea through multiple experiments, and you're allowed extensions. But you should have One Singular Idea. Everything you write should connect to it. (Yes, even the background! I'll talk about this later.)

Usually this is easy: if you've developed a new algorithm or evaluation method, that's your one idea. But occasionally you have many ideas you'd like to communicate, and having focus means you get to pick only one. If you try to communicate all of them, the reader will remember none.

Yes, this is annoying. I wish it weren't true. But it is. The sooner you come to terms with it the better.

#### Know your reader

It's an unfortunate truth that, once you've written a paper, you can't tailor the text to every person who might want to understand what you've done. You have to pick a particular reader in mind, and write for that person.

This lets you write a useful introduction (what do they need to hear to be motivated to read your paper), background (what do you need to teach them that they don't already know), methods (how does this build on ideas they've seen), and conclusion (what should they take away).

(Incidentally, the purpose of a background section is to broaden the scope of who "the reader" is. Initially the reader might be anyone, but after the background section, you can assume they're someone familiar with everything you've written there. This should help you write a useful background that's not just a block of citations to appease reviewers.)

Personally I've found the best person to write for is the six-month-younger version of myself. What would I have needed to say to explain why this idea was a good one to myself-before-I-had-it? Why does this paper have a chance of succeeding? Why is this design the right one?

It's more challenging when the reader you have in mind is not yourself. Sometimes, for example, I write papers as an argument to convince someone to change their mind. In this case, I need to write towards that person. When doing this, I find the best approach is just to share your paper with someone you imagine is your ideal reader, and get their impression.

#### Don't obsess over your title

Hot take. I know. But I don't think your paper title matters if your paper is good. In a world of clickbait, I think it's refreshing that, in science, we live in a world where as long as your title is accurate you'll be fine. Remember: we're trying to do good science, not accumulate citations we don't deserve because we titled our paper something overly broad. All you need to do is make sure that your work will be seen by the researchers who need to see it.

This doesn't mean you can ignore the title. For every hundred people who read your paper's title, one will read its content, and so you should accurately describe your work, and let people know what to expect. But don't feel like you have to clickbait "TEN THINGS GRADIENT FLOWS CAN DO. NUMBER SEVEN WILL AMAZE YOU!"

If you're struggling to write a good title, it's also a good sign that your paper is trying to do more than just one thing. If this is the case, fix the cause, not the symptom. Then title your paper appropriately.

#### Your abstract does matter

The purpose of the abstract is to, in just a few sentences, simultaneously (1) convey the entirety of your paper, and (2) convince someone to read it if it's related to their work. This is hard. I frequently rewrite abstracts four or five times before finding one that I like, and even more frequently end up without anything I like at all.

After briefly reviewing some of my most important papers, it (empirically) looks like I actually have a pretty formulaic abstract structure: in sentence (1) what topic you're working on, and then in sentence (2) explain what problem you're going to solve in that field; sentence (3) should say something about your results or methods, and sentence (4) should say something about whichever sentence (3) didn't; and finally sentence (5) should say something about why your work matters. This is, I think, generally a good structure. The second most frequent way I write abstracts (again, empirically) is to go (1) claim of some new result, (2) evidence for this claim, e.g., method or data, (3) impact of this result. This is what I do when the topic is already very broad (and so stating it isn't important), or the paper is meant for only a very specific audience.

Your abstract should be specific and explain exactly what you're going to get out of your paper. Of my favorite papers, the abstract for every one has at least one specific number to highlight what I've done: "we consider problem X" is far less useful than actually stating your result. Similarly, don't hedge: just state the clean (but true) version of your result; everyone knows you have only a few sentences here.

Finally, your abstract should convey a sense of importance. This can either be in the problem you've selected, the novel method you've developed, or some big lesson you hope people take away. Make it obvious what you think is important, and focus in on that.

#### Write a good introduction: you're telling a story

An introduction is the beginning of a story. You start by meeting the reader where they are---with what they currently believe to be true. Then you guide them into the world where your paper is set, where your ideas make sense. And finally you explain, in this world, your contribution.

The reason you have to do it this way is that an idea can only land if the reader is in the right frame of mind to receive it. You're communicating with busy, skeptical people who are suspicious of everything new. You can't just state your idea and expect it to work; you have to help the reader get to the point where they're ready to hear what you have to say.

Sometimes this is easy. If you're introducing a new technique in a well-studied area, you just need to say "We solve X unsolved problem" and that's enough. You don't need to tell a story because it's a story everyone already believes. Sometimes it requires a little work. If the problem is one people mostly know but might not be fresh in their mind, it's probably enough to remind them for a paragraph or two, and then dive in.

But sometimes this is hard. If you're working on a problem not yet well studied (which will be the case for most best-paper-worthy research), you'll need to spend enough time introducing the setting, the cast of characters, and the backstory that the reader can appreciate what you're about to do. (At the same time, you can't spend six pages writing a novel before getting to the action. Readers have short attention spans, and you have at most two pages.)

For example, one of the hardest introductions I had to write was for [a paper I submitted to Crypto in 2020](https://arxiv.org/abs/2003.04884) about stealing the weights of a neural network. Crypto, if you're not familiar, is where people submit mostly theoretical ideas in cryptography. It does not usually accept machine learning papers. So I spent three pages easing the reader---a theoretical cryptographer---from their world into mine. Those three pages were the most important pages of the entire paper. If I could convince them that model stealing was actually comparative cryptanalysis in disguise, I'd have done my job. If not, nothing about my methods or the strength of my results would save me.

Another case where introductions are hard is when you're writing about a problem that doesn't quite exist yet but will soon. In [my semi-supervised learning paper](https://arxiv.org/abs/2105.01622), I was arguing that poisoning unlabeled training data was going to become a serious threat---but in 2020, training on huge uncurated unlabeled datasets wasn't yet standard practice. So a good chunk of the introduction was spent arguing not about my attack, but about where machine learning was heading: everyone was about to start training on exactly the kind of data my attack targets. I had to sell the reader on the future before I could sell them on the paper.

And in the most challenging cases, you can't state your idea outright at all, or it'll be dismissed as heresy. Here the reader has to arrive at the conclusion themselves; your job is to lay out the evidence in an order that makes the conclusion unavoidable.

For example, several years ago I drafted a paper ["On Evaluating Adversarial Robustness"](https://arxiv.org/abs/1902.06705). In one sense, a single sentence could describe this paper: "Here is a protocol you can follow to make sure you've evaluated adversarial robustness correctly." But this is not the idea I wanted to convey; it's not *why this paper exists*. The idea I actually wanted to convey was: "evaluating adversarial robustness is hard; almost everyone gets it wrong, and you probably will too." But speaking these fifteen words to someone does not make them enlightened---they have to feel it in their bones. And if I'd just said it outright in the intro, the reader's immediate reaction would have been "well, not me," and I'd have lost them before I started. So the job of the paper was to provide evidence so overwhelming that by the end, the reader arrives at that conclusion on their own.

#### Each figure must stand on its own

Some readers will study your paper top-down. But most will skim for the important details, as I recommend you do when deciding if a paper is worth reading. So you must make sure the reader can quickly tell if your paper is worth reading. And the best way to do this in my area of experimental computer science is to have self-contained figures that explain the main points of your story. For example, Figure 1 may explain the problem; Figure 2 explains your algorithm; Figures 3-4 explain details of your method; Figures 5-8 have your results and analysis. Importantly, each figure can be interpreted on its own without having read the text.

If it's obvious what you're showing then this is easy: just state the interpretation. "Figure 7. Our method performs 3% better than all prior methods" works well for a bar graph showing accuracy for your method versus three others. But other figures will need explaining; you must do this. If your figure can't be explained in a caption it is too complicated. Split it into multiple figures. If it doesn't have a single sentence describing the takeaway, it is also too complicated.

#### Write a good conclusion

Now that you've finished your paper, it's time to wrap up. Which usually means you write a conclusion.[\[b\]](https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html#footnote3) In some fields you don't. In my field you do. In my experience, the conclusion is usually the worst part of most papers I read. Conclusions are not abstracts or introductions in the past tense.

Let me repeat that: CONCLUSIONS ARE NOT ABSTRACTS OR INTRODUCTIONS IN THE PAST TENSE.

Instead, the purpose of a conclusion is to provide a moment of reflection.[\[c\]](https://nicholas.carlini.com/writing/2026/how-to-win-a-best-paper-award.html#footnote4) In a good paper you should be doing this throughout, wherever appropriate. This helps keep the paper interesting, and makes it easier to write a conclusion where you can assume the reader has already done some reflecting on your ideas. Explain to someone who has just spent the last hour immersed in the technical details of your paper what important idea you want them to take away. Part of this will---necessarily---involve reminding them about some of the facts you told them earlier. If you introduced some new fancy method in Section 2 and then spent sections 3, 4, and 5 evaluating it, you'll want to remind the reader you had a new method. After all, that's part of the important lesson of your paper. But your reminder should be brief. As quickly as possible, return to the primary focus: answering the question "so what?"

If your paper is important, the answer to "so what?" should be more than "our method makes the number go up by 2%". If that's all you can say, then I'm sorry, you may have valuable science, but you don't have something that's going to win any awards. (In fact, something I frequently do when deciding if a paper is worth writing is write my best-case conclusion before even performing the research. I ask: if this paper were to succeed wildly---if all the experiments turned out exactly as I want---what could I say in a conclusion? If I have nothing to say beyond repeating the results, then what I've done mustn't have mattered very much. And so I'll just drop the project and move on to something else.)

In the story analogy above, your conclusion is when you break the fourth wall and just straight up tell your reader the moral. Be heavy handed; leave nothing unsaid.

#### On Writing

There are thousands of books on how to write well. Read them, or don't. The only rule that matters is delivering the message to your reader; all other rules come second. Remember, though, that the reader won't receive your message if they're so bored with how you're saying things that they stop reading.

Fortunately, being not-bad at this is enough. As I'm sure you've noticed reading this article, I'm not some mystical word wizard who captures your imagination with every phrase. But research papers aren't graded for their prose; no one has won a Pulitzer for their science journal. As someone who isn't skilled at writing and used to be much worse, I've found the only way to get not-bad is to read more and write more.

If I were to briefly summarize the best writing advice I've received, it would be to listen to how your writing sounds spoken out loud, and try to make it understandable. I used to do this by reading my papers out loud to force myself to hear every word; I still do this sometimes, but now I also use text-to-speech systems to read the words back to me. You'll notice things you'd never have caught yourself.

Think about how what you write might be confusing to a reader. Pay particular attention to sentences that could have dual meanings, especially when one of those meanings is the wrong interpretation. Avoid sentences that lead the reader towards believing you're going to say one thing, and then saying something else. Make sure that the grammatical structure of your sentences places the impact on the words that matter.

Beyond that, remember that every rule can be broken. Long sentences are fine, as long as you give the reader time to breathe with some short ones after. You're allowed to use contractions, but don't overdo it. Think it would be helpful to ask a question? Go ahead. Don't use jargon in general, but do use it when the alternative is to be imprecise. Cut words and only write what's necessary, but don't be so terse your writing is unreadable or sounds jarring. Papers written in the passive voice are usually less understandable---but feel free to use it where appropriate (or funny).

Proofread your work. But your time is finite; every minute spent proofreading is a minute not spent doing something more important. Readers will forgive the occasional speeling mistake, repeated word, or or grammatical error, as long as they're rare and your message is well communicated.

### After writing the best-paper-worthy research

#### Get lucky: it's not "too early"

It's easy for new ideas to be too early to be accepted. If you see the future before others do, frequently your work will be rejected because it assumes a premise others haven't yet accepted as true. So there's a lot of luck in when exactly you write your paper.

Again let me refer back to the paper I've mentioned a few times now on [poisoning semi-supervised learning.](https://arxiv.org/abs/2105.01622) Unfortunately for me, semi-supervised learning had *just* begun to work in the last year. So when I submitted this paper to security conferences, it was summarily rejected several times because reviewers didn't believe it was worth studying. In their mind, semi-supervised learning didn't work and so attacking it didn't matter. After enough time had passed (and after four rolls of the dice) the idea that unlabeled data would play an important role in the future of machine learning became more widely accepted. And so my paper was accepted at USENIX Security and received a best paper award.

There are ways to mitigate being too early. Writing a compelling introduction that explains why your paper will become important in the coming years is helpful. You can also explain how your problem relates, even tangentially, to something real today. (But don't be deceptive about your true motivations.)

But sometimes there's nothing you can do. You write a paper, and it's fantastic. Ten years later it will be recognized as one of the most important ideas in the field. But at the time, no one (but you!) saw that yet. And that's fine. In some sense, this is the most rewarding type of paper to write. The one where no one believed in it or saw the value, but reluctantly came to understand it was right all along.

#### Get lucky: it's still a hot topic

Most papers take a while from when you first come up with the idea to when they're finally published. And a lot can happen in a year---especially in my field of machine learning. You pick topics because they're important at the time and because you think they'll still be important in a year, but you can never be sure. You can try to mitigate this by being on the lookout for what might be important in the future. But this is very much a guessing game.

#### Get lucky: no one else did it first

Related to the above, you also need to get lucky that no one else writes the same paper and gets it published first. You can have done the most important research of the decade, but the nature of research is that whoever does it first gets a disproportionate share of the credit. This isn't how it should be, but it's how it is.

I've been on both ends of this more times than I can count---it's just part of life in research that good ideas are frequently discovered simultaneously. The body of scientific knowledge finally allows for some new idea to be discovered, and suddenly everyone is doing the same thing. There are ways to mitigate this---work on problems others aren't, spend time thinking about the future and not just the present, work quickly and efficiently---but you can only do so much.

#### Get lucky: the committee likes it

Finally, you have to get lucky one more time: the award committee has to like your paper. At any conference, something like 2-10% of papers are truly excellent and deserving of recognition. But only a smaller fraction can receive an award. It's the job of the award committee to separate those that receive an award from those that merely deserve one, and then from this set pick one (or a few).

The way this happens differs in the details but is the same at a high level. First, your paper needs to get accepted. Then papers are usually filtered by the overall score of the reviewers, potentially informed by nominations from the program committee. This shortlist goes to an award committee. The award committee will usually filter this list down pretty aggressively if there are many papers, and deliberate at length about which paper should receive the award: your paper essentially goes through an entirely separate round of peer review. Finally, among all the papers sufficiently high quality that they probably deserve an award, one (or sometimes a few) are selected.

This means the specific people on the committee make a big difference. So you just have to get lucky that the committee likes your topic area, your problem formulation, and the way you've written your paper. Awards are frequently given for "political" reasons (e.g., to encourage more research in a particular direction, or to correct a particular misunderstanding of the community).

#### Don't get discouraged; be persistent

The majority of my papers that have received best paper awards were rejected at least once before they got in. In one case, a paper of mine was rejected four times first.

From what I can tell, this is normal. I think there's actually something going on to explain this: On the first pass, the paper is often rejected because it's saying something a little outside what's normally believed to be true. The reviewers aren't convinced and leave some confused remarks. Then, the authors (knowing they're right!) spend extra time revising their paper to make the arguments clearer and stronger, so even confused reviewers will understand it. The next reviewers (or the ones after that) see this paper a few months later---a stronger paper at a time when the idea is slightly less heretical. And then they give it an award.

### Conclusion

Good research (1) advances the body of scientific knowledge in a way that's (2) accurate and (3) approachable. Your job is to do all three. What I wrote above is, to a first approximation, the best way I know how to describe what I do, and what advice I'd give to others.

While I did title this blog post how to win a best paper award---and I do touch on this throughout---that's not why I wrote this article. As I hope I've made clear, a best paper award is one sample from a distribution. You don't control the sampling process---that's determined by, among others, the program committee, the timing of your work, who else submitted that year. But you *do* control the distribution. The award, if it comes, is just someone noticing where your distribution ended up. Focus on the distribution.

Because of this, you shouldn't go into research with the objective of winning a best paper award. It's unhealthy, counter-productive, and out of your control. But you also shouldn't go into research just trying to accumulate publications that don't matter. That's equally unhealthy, and even if you do achieve your goal you won't have done anything that mattered. Instead, write papers with the goal of having an impact. That's what matters, is entirely under your control, and is lots of fun.
