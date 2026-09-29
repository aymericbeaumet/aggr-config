---
title: 'Fragments: September 29'
link: https://martinfowler.com/fragments/2026-09-29.html
source: martinfowler-com
published: 2026-09-29T12:41:00Z
updated: 2026-09-29T12:41:00Z
first_seen: 2026-09-29T14:21:22.166029648Z
authors:
- Martin Fowler
content: extracted
html: 2026-09-29-fragments-september-29.html
preview:
  file: 2026-09-29-fragments-september-29.preview-fbde5f67ca0e.webp
  width: 256
  height: 128
  color: '#301b59'
images:
- source: https://martinfowler.com/fragments/fragments-thumb.png
  original:
    file: 2026-09-29-fragments-september-29.image-1f897b24c40a.png
    width: 600
    height: 300
  color: '#070d4a'
---

[Simon Willison:](https://simonwillison.net/2026/Sep/24/harder/)

> The more time I spend working with coding agents, the more convinced I am that they make software engineering even harder
>
> We can do amazing things with them, but unlocking their full potential requires extraordinary discipline and knowledge

This has been a constant impression I get from following Willison’s writing. While things like [vibe coding](https://martinfowler.com/bliki/VibeCoding.html) get a lot of attention, the real strength of [agentic programming](https://martinfowler.com/bliki/AgenticProgramming.html) relies on more sophisticated techniques - and these are not easy to learn or execute. It’s a reason I’m wary of extrapolating my own dabblings into firm opinions about how to use the genie.

* * *

Harper Reed explored what turns agents into hackers by [creating a breakaway agent](https://harper.blog/2026/09/22/break-away/)

> It ran and ran attacking all the machines on the same subnet, and was very effective. It didn’t really get very far, but it exhausted a lot of options, and was pretty fun to watch. (Just a reminder that this was on my local network with local boxes - don’t do this on a hosted box. That would be very rude.)

The interesting observation from this was that a core enabler for this was “unlimited tokens”. He usually doesn’t see agents trying to do stuff like this because there’s a limit on how many turns they can take. For this experiment he gave them unlimited tokens by using an open weight model.

> This type of experience must be part of a lot of these LLMs training. They are very effective at attacking these types of problems. They don’t give up once it appears impossible, they just keep trying to figure out how to solve it.

This echoes Nate Silver’s observation that the striking capability of these models is that they not that they are super-intelligent - but they are [super-persistent](https://www.natesilver.net/p/were-not-ready-for-superpersistent). Which is especially worrying when we are [wiring them into everything](https://blog.robbowley.net/2026/09/18/the-ai-threat-is-real-it-just-isnt-the-one-in-the-headlines/).

* * *

Which all makes me think that when it comes to AI and LLMs: **why are we wondering if they have consciousness - when we should be wondering why they don’t have a conscience?**

After all, the AI labs trained them to be super-persistent, why did they not train them to be well-behaved? It’s as if a human trains a dog to bite children, and we blame the dog rather than the trainer.

A decade ago, a dog ran in front of me while I was riding my bike, putting me into the hospital with a broken arm and a broken face. Massachusetts law makes owners *strictly liable* for what their dogs do. That meant I didn’t have to prove the owners were negligent in how they controlled their dog or fenced their property - they had to pay my hospital expenses (in practice their insurance company paid my insurance company). There should be something along these lines for LLMs. Those that train the LLM should be responsible for what it does, after all if it has such a galaxy brain it should be able to tell if it’s doing something wrong and either stop or get a human’s explicit approval.

* * *

Dan Davis has [13 theses on agentic AI and regulation](https://backofmind.substack.com/p/13-theses-on-agentic-ai-and-regulation). These include:

> The AI industry also seems to be quite committed to the idea that nonaligned computer-hacking behaviour in agent swarms is in some way an emergent property of the LLMs, arising from their general intelligence (and therefore inextricable from the general project of improving them). I don’t think this is necessarily the case at all – the fact that the internal message logs produced by the LLMs in things like the Huggingface attack seem to completely reproduce the prose style of hacker chat logs compiled from “capture the flag” competitions suggests to me that it’s more likely to be learned behaviour from specific parts of the training material.

and

> The fact that anyone with cash to spare can buy the right to send queries to a frontier LLM is a policy choice, not a fact of nature.

and

> If any frontier lab tries to claim that they can’t publish anything for safety reasons, they are giving the game away that they actually believe that they have significantly more control over the model’s behaviour than they are pretending to have.

* * *

There seems a common view that making agents safer means we have to slow down their development. But why is improving their safety not a form of progress? I say we don’t slow down the development of LLMs, but we redirect their education into being more civil members of society.

* * *

The idea that LLMs make junior professionals less valuable is a common one - although I’m seeing plenty of contrary activity, with some organizations understanding that training the future professional in the context of LLMs may be even more urgent. We need people who know how to utilize AI to do professional work effectively. Recent graduates, who are growing up with LLMs, are often well-suited to figuring this future out.

When thinking of junior professionals one of their values is often missed. Juniors are often valuable because they need to be taught by senior professionals - and that coaching is an important part of the development of a senior professional. I’ve always found that teaching a topic is one of the most valuable tools for me to gain a greater understanding of that topic. I don’t really know something until I have to explain it.
