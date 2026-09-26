---
title: How to keep enjoying programming in a world of LLMs
link: https://discourse.haskell.org/t/how-to-keep-enjoying-programming-in-a-world-of-llms/14705
source: hnrss-org-frontpage
published: 2026-09-26T09:41:58Z
updated: 2026-09-26T09:41:58Z
first_seen: 2026-09-26T21:28:42.415865881Z
authors:
- signa11
summary: 'Article URL: https://discourse.haskell.org/t/how-to-keep-enjoying-programming-in-a-world-of-llms/14705 Comments URL: https://news.ycombinator.com/item?id=49854875 Points: 112 # Comments: 162'
content: extracted
html: 2026-09-26-how-to-keep-enjoying-programming-in-a-world-of-llms.html
preview:
  file: 2026-09-26-how-to-keep-enjoying-programming-in-a-world-of-llms.preview-17b718c222f0.webp
  width: 256
  height: 256
  color: '#271e32'
images:
- source: https://us1.discourse-cdn.com/flex002/uploads/haskell/original/1X/89166504e40f4869ea825dd70048017861ec8578.png
  original:
    file: 2026-09-26-how-to-keep-enjoying-programming-in-a-world-of-llms.image-89166504e40f.png
    width: 512
    height: 512
  color: '#5e5085'
---

Are you steering towards AI burnout? Afraid of loosing your job to someone with little programming skills, no aspirations to quality, and a huge Claude account? Disappointed about the code quality in your projects, or worse in “your” own code? This is for you.

There are significant and legitimate ethical concerns about frontier LLMs run by big tech companies, these have been discussed at length, I’m aware and agree, this post is not about them. Please don’t mistake me for a pro-LLM techbro.

Also: Since people have mistaken my texts for LLM-generated before, I’ll tell you that it is 100% human written without any AI-assistance.

## Souls in the Great Machine

There is a [great book of this title by Sean Mcmullen](https://seanmcmullen.net.au/souls-in-the-great-machine/) that I enjoyed reading as a teenager, and at the first glance it describes the opposite of our situation: A big computer where the individual components are human, and work together to form a calculating unit. On the other hand, LLMs are themselves running on actual computers and pretending to be (super-)humans. At a second glance, story and reality are not so far apart though: Our role in the process of producing software is being degraded slowly from being actors to cogs in a machine. The spec-driven-dystopia is that we just get handed down some spec, hammer it into the LLM, and weep when our tokens run out because a [technofeudal lord](https://en.wikipedia.org/wiki/Technofeudalism) decided to hand out fewer of them.

**As a Haskell programmer, I enjoy writing Haskell**. Yes, I like the product we make at work a lot, I like what you can do with my open source libraries, but I really enjoy just the process of expressing my thoughts in this language. I’m assuming this to be true for most of you, and also it not to be true in many other languages, which explains to some amount why enthusiasts of different programming languages have different opinions on how bright or dark the LLM-assisted future is.

**When I generate code, a lot of that enjoyment is at risk**. So, don’t, maybe. I want to keep writing (at least the enjoyable parts of) Haskell programs, and not having to read and review (too much) generated code. At the same time, I want to put those tokens to some good use that doesn’t slowly burn my brain away.

*I want to show you a way to keep enjoying programming, and at the same time becoming moderately more productive with LLM, instead of appearing to be much more productive and losing all the joy.*

If you want to just be LLM-abstinent, that’s great as well, and you already know what you’re doing. But there might be reasons you don’t want to be, e.g. you actually need to bring some real productiveness gain to the table, or you don’t want to be left behind while the rest of your team, your company, your industry, is moving towards heavy LLM adoption.

## Keep writing code

If you want to keep owning your codebase, you need to keep writing some code. If you let it all be generated, it will turn into an LLM wasteland that only your coding agents can thrive on. So you should keep writing code, otherwise your codebase will eventually be lost.

Another reason to keep writing code is to keep being a good programmer. Skills can be lost by not practising, and here the risk is particularly high because there is a really low threshold to giving up your coding work and hand it to an agent. After just a few weeks of not coding and handing everything to agents you’ll notice that you have a hard time returning to coding yourself.

LLMs are way worse at producing *good, human-readable* code than advertised. They are somewhat ok at producing code they themselves later work on exclusively. But I’m sure you’ve already experienced the despair of looking at a completely generated file that must contain a bug somewhere, and your perceived inability as a human to find it yourself, because the landscape is just so alien.

So how to get to those productivity gains if not by letting agents do the coding? By making them do nearly everything else. Especially the boring work that is a nuisance to you. Ideally those tasks that are not too hard to get right, and easy to check.

## Planning

Since the earliest history of computers, they’ve been always used as bookkeeping tool. Use LLMs that way. Amongst other things, it’s a bookkeeping tool that you can address in natural language instead of a formal interface. Convert a huge written conversation between domain experts into actionable todos. Test something, write down the test results, let it organise them into a plan how to fix the defects.

Use tools like a todo tool, or better even markdown files with frontmatter to make it track planning items properly. LLMs can have a huge context, but still if it is too full it can silently lose information.

**But don’t let it make any crucial decisions. Make it ask you.** If you don’t understand the question, it’s the LLMs fault not to give you the relevant context (or you might be exhausted and need a break). If you return to the same issues again and again, take a step back from the screen and think about it yourself, and return when you have a clear picture of what you want.

## Researching

When you give a researching task to an agent, it’s tempting to watch it querying and “thinking”, to kick off some other agent in another project, or to make a coffee. Of these three, making the coffee is the best option. The one better option is: Research yourself in parallel using a good ol’ search engine. At least roughly know everything the agent will know.

Don’t just let it research something, accept its results as facts and plan from there. This will lead to embarrassing technical dept.

**The point of making an agent research is not for it to present all the relevant knowledge to you, or make a better decision than you could have made. The point is that you don’t have to go “Let Me Google That For You” on it.** You should understand the domain you’re modelling as good as the agent does, ideally even better.

Make your agents write down their research results somewhere, with links to their used resources. When it comes back to you later and presents you with a weird proposal, ask it about what the research says and which resource says so. 50% chance says it will discover its own mistake. In the other 50%, read it, now you’re in a position to make a good decision yourself.

## You are the coder

*This is the game changer.*

The typical coding harnesses lure you into “plan first, then let the agent code”. Refuse. **Plan together, but then you code.** Tell the LLM to research your code base, let it tell you the current todo and bring up all the places you need to edit, make it mention potential pitfalls, let it remind you of relevant background research.

**I work with this workflow, and it is a lot of fun.** I’m enjoying my work. Sometimes even more than before LLMs. I always have a clear todo, I don’t need to worry about the overall plan, I can focus, I’m done with the todo quickly because it is well-planned. It’s like agile but without all the annoying processes.

**Make the agents group around your way to work, not the other way around.** Maybe you’re an experienced programmer who already knows how you work best. Let the agents do all the incidental work around you that you enjoy less.

There are multiple advantages to this workflow:

1. *You keep doing what you enjoy.* If you like programming, do it.
2. *You always know what state your code base is in.* Been surprised by some weird generated stuff coming from your own vibe coding sessions? Needed to rewrite LLM slop? Lost track of where you are in a session? This way you never have to again.
3. *You discover a bad plan early.* An agent coder might just go on forever with something that you’ll recognize very quickly as a bad idea.
4. *You keep honing your skills.* Obviously. You’ll stay a good programmer, or even go on improving.

### Rare cases when a coding agent is useful

Ideally for cleanup, small tasks, routine work, low-risk refactorings. You left FIXMEs in your code (maybe on purpose to save time and energy)? You wrote the 3 interesting cases and left the 7 similar boring ones? You have a module reorg in mind and want it benchmarked? Need to swap out an unmaintained library for a better one? Those are valid use cases. Designing something complicated from the ground up is probably not.

Sometimes you run out of time but want something finished, and maybe the rest of the todos in your plan are obvious low-risk tasks. It’s ok to say “I’m afk, finish this” to your supervisor agent, with a bit of luck you come back to a finished feature next morning. But it’s important to do most of the coding work yourself.

There are some slight pitfalls here:

- So you wrote those 3 interesting cases and tell the agent to finish the remaining 7 ones because they are just obvious adaptations of those you wrote. *Chances are you should abstract instead.* Maybe what you’re really doing here is applying a lens or some other optic? Maybe this really is an instance to a popular type class like `Traversable`? LLMs are famously prone not to recognise this and instead copy huge swathes of code. You as a human are striving for better readable and reasonable code.
- Same goes for “make it mention potential pitfalls”. Yes, LLMs may be really good at walking through the whole callchain and making sure that all places you should touch are listed in the todo you get handed. But instead of relying on it to find all these couplings you should consider whether your codebase is organised poorly, forcing you to use an LLM for code research in the first place.

## The review cycle

You might remember how AI generated images suddenly became much more realistic with the advent of [generative adversarial networks](https://en.wikipedia.org/wiki/Generative_adversarial_network). In short, you have one model (the “generator”) that generates an image, and another (the “discriminator”) that tells it how well it has performed. These together can produce much better results than the generator alone. Applying the idea (which, in its generalized form, is not new at all) to LLM-assisted coding, you get an automated review cycle.

**Don’t accept, don’t even read any artefact produced by an LLM without an automated review cycle.** This obviously applies directly to code (in those cases where you still let it be generated), but in particular to planning as well. When an agent codes something, the job is not done when it hands over, the job is done (i.e. fit for human eyes) when a reviewer agent has no more findings on it. The same goes for plans. It’s really tiring to go through logical holes in a plan (refactoring a function in todo 2 that is planned to be written in todo 7) and spot them, so add a review agent that does that.

I’ve found it surprisingly helpful to have my own code reviewed. Sometimes it will just point out some nits, or insist on bloating up the Haddocks, but often enough it finds a genuine bug or an omission, and keeps me focussed on the actual todo. I recommend adding review cycles to your own work, but I absolutely understand if you don’t want to read an LLMs review of your code. One way to get around this to tell it to fix the remaining findings itself if they’re minor.

## The frontier

> You’re completely underutilising what frontier models are capable of.
>
> *Some vibe coder on the internet*

Yes. That’s a logical consequence of my point.

But there are multiple reasons why relying on frontier model features is a bad idea.

1. Environmental cost. (Even though this post was not supposed to touch this topic.) Frontier models just use a huge amount of energy. Although this is a guess to some extent since LLM companies aren’t very transparent on how their gadgets work.
2. It’s hard to build up trust in something that pretends to be much cleverer than yourself. At the end, you’re responsible for the code you produce. Not your machine. Blaming someone else for your code is something that bad managers and colleagues do with their employees and coworkers, it’s ridiculous to do with a machine. So use LLMs in a way that you can take responsibility for the results. This only works if you make yourself an integral part of the process.
3. The fanciest models use the most tokens, so there is no telling whether you’ll be able to complete your tasks with them in a given session. Everything that you can do with a smaller model is a safer bet.
4. When your workflow doesn’t need frontier models, you have a chance of eventually being able to replace them by open weight or open source models, and not be dependent on technofeudal lords at all anymore.

In the end, you’re doing a complex job. In some aspects of it, LLMs may perform the same or maybe even a bit better. But that’s by far not enough to bow down to them, because of all their downsides. LLMs would have to be many times better than a human developer, use less resources, be more reliable in complex real-world situations, be at least as well-aligned, and in some way accountable for their results, to replace human developers at their core activity.

## No tokens left?

Maybe you’ve experienced your work coming to a standstill because you’ve run out of tokens. It’s annoying. Your workflow is now built around a tool that you suddenly have no access to any more. In the extreme case, you can’t go on doing anything. [Take this xkcd and imagine “no tokens” instead of “compiling” for a healthy way to process that situation.](https://xkcd.com/303/)

If this happens a few times, you might have had the feeling you’ve been betrayed. And you’re right. You have been. How many tokens you have in a given session on a particular plan is an intransparent number at the whim of some big techno feudal lord. It’s not like a commodity you buy on a fair market and then use in a plannable way.

**You need to stop perceiving the depletion of your tokens as “having bought too few”, and start treating it as what it is: A service outage.** Your LLM company has sold you the promise that you could use the LLM, and they don’t keep it up. The number of maximum tokens in your session may change without you knowing or being able to influence it, so you can’t really plan for it either.

Of course saving on tokens is imperative. Re-evaluate your agent setups, your usage, your context sizes, your skills, and so on, to save on tokens. But even if you do, and even if you’ve bought a larger seat, it may not be enough.

And instead of seeing it as “your fault” when you run out after you’ve done everything to use tokens sparingly, see it as a technical fault that you need to be prepared for. When you’ve ever have worked a lot on a train or plane, you know that you have to be prepared for not having internet all the time. Download and cache big assets beforehand. Always have some work to do that you can do offline.

For LLM assisted work, this means that you have to make it produce tangible artefacts for everything that you need to work on. Most importantly, if you adapt to the human coder workflow I’ve outlined, make sure that you always have a planned list of todos you can work on. Have fun racing through them, and when your agent is back, tell them to do the cleaning.

## LLM gibberish

LLM produced text is unlike human text. It gets bad in particular when the LLM actually doesn’t really know what it’s talking about. **Treat LLM gibberish as potentially harmful for your psychic health.** Don’t consume to much of it. Keep talking to people about your code, especially about greater visions and interesting aspects. Only read LLM artefacts after automated review agents took the edges off.

When your head is spinning, take a break. Yes, even if no agent is currently running in the background and “producing value”. Your mental health is more important than your work output.

## Your fellow humans

Programming is, by large, a social endeavour. For example, in a company or an open source project, we send each other pull requests, write issues and commit messages. This is a form of communication. Even if you’re the only one on your project, your past self writes issues, commit messages and PRs for you future self. Communication between humans is the pillar of software development.

**Make sure you always meet people human-first.** Don’t send a completely generated PR to someone. I’ve done this by accident, the other end was rightfully fed up. I’m making sure not to repeat this mistake.

Agents will readily offer you to write a complete PR body in grammatically correct language, with all the details in there. **This is not communication. It’s a tool output.** Treat such texts the same way like benchmarking numbers or debug traces: Append them to your handwritten PR body, possibly in a `<details>`, so people can decide themselves whether they want to read it or not. You’ll find that more often they will not.

## My journey

With all this, I’m more productive than without LLM assistance. It’s difficult to tell exactly, maybe twice as fast? This is less than many full vibe coders will boast with, but that’s ok. I picture myself as a gardener adopting some gentle organic fertilisers, while vibe coders are more like drowning their fields in industrial chemicals. I believe that my way is the more sustainable for now.

I love programming Haskell, and doing it for a living is my dream job. In the last month I was worried that this dream was now a thing of the past. But all what I wrote about here is what I learned in order to keep this activity enjoyable. It seems to work.
