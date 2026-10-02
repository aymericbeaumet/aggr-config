---
title: software doesn't need to be readable anymore. it needs to be explainable.
link: https://ghuntley.com/readable/
source: ghuntley-com
published: 2026-10-02T06:06:40Z
updated: 2026-10-02T06:06:40Z
first_seen: 2026-10-02T08:58:34.290726633Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: Forty years of computing was designed around humans as the reader, writer and operator. On AI21's YAAP podcast I argued that's no longer a design goal, and what that means for programming languages.
content: extracted
html: 2026-10-02-software-doesn-t-need-to-be-readable-anymore-it-needs-to-be.html
preview:
  file: 2026-10-02-software-doesn-t-need-to-be-readable-anymore-it-needs-to-be.preview-2f0fd1eff474.webp
  width: 256
  height: 144
  color: '#8c8484'
images:
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/yaap-readable.jpg
  original:
    file: 2026-10-02-software-doesn-t-need-to-be-readable-anymore-it-needs-to-be.image-e608d497fb4e.jpg
    width: 1200
    height: 676
  color: '#969997'
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/A-black-and-white--low-angle-digital-illustration-in-a-symbolic-traditional-tattoo-art-style.--A-bald--light-skinned-man-with-a-bushy-beard--prominent-eyebrows--and-a-friendly-expression-wears-denim-overalls--2--c4421c9b-5f3c-40dc-838c-40b5235ce4e8.jpg
  original:
    file: 2026-10-02-software-doesn-t-need-to-be-readable-anymore-it-needs-to-be.image-e9d064eb5c12.jpg
    width: 256
    height: 256
  color: '#fefefe'
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/bafkreiftzmpb7bbkej76rac3kuijg2mcuyjxxhtnqdie5qq6rbrc45v2iu-1-a7544c4e-6d21-416a-898f-997162311d6f.jpg
  original:
    file: 2026-10-02-software-doesn-t-need-to-be-readable-anymore-it-needs-to-be.image-2bf10d6effdc.jpg
    width: 1000
    height: 978
  color: '#c0c0c0'
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/A-black-and-white--low-angle-digital-illustration-in-a-symbolic-traditional-tattoo-art-style.--A-bald--light-skinned-man-with-a-bushy-beard--prominent-eyebrows--and-a-friendly-expression-wears-denim-overalls--2--c23bc14e-360c-4106-9896-e25298235b65.jpg
  original:
    file: 2026-10-02-software-doesn-t-need-to-be-readable-anymore-it-needs-to-be.image-e9d064eb5c12.jpg
    width: 256
    height: 256
  color: '#fefefe'
- source: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/A-symbolic-traditional-tattoo-art-print-of-a-software-library-in-vibrant-retro-colors-with-complex-ornamental-details.-Creative-light-art-is-used-to-achieve-an-abstract-pattern-with-complimentary-colors--balance-36436c4b-e2ea-4613-85eb-539cecd7bc7d.jpg
  original:
    file: 2026-10-02-software-doesn-t-need-to-be-readable-anymore-it-needs-to-be.image-faf55bf68ea1.jpg
    width: 1200
    height: 670
  color: '#fdfdfd'
---

I sat down with the folks at AI21 Labs for their YAAP podcast at the AI:Engineer World Fair, and we went deep on something I've been chewing on for most of this year: almost every decision in computing for the last forty years was made with a human in the loop, as the reader, the writer, or the operator. What happens when that stops being true?

[YAAP | Your Code Doesn't Need to Be Readable Anymore](https://www.youtube.com/watch?v=vrCE5m2MRys)

YAAP by AI21 Labs — "Your Code Doesn't Need to Be Readable Anymore" (21 minutes)

Here's the gist, written up properly, plus chapter links at the bottom if you want to jump into a specific part of the conversation.

> Software doesn't need to be readable by a human. It needs to be explainable to a human.

## the economics of AI are cooked, and I still haven't written code by hand in two years

The opening hot take was that AI economics are absolutely cooked because adoption is nowhere near what is needed to achieve ROI on the amount of capital deployed. This is why all the labs are hiring aggressively to staff up professional services organizations around the world right now. Go read Ed Zitron's [Where's Your Ed At](https://www.wheresyoured.at/?ref=ghuntley.com) if you want the long version.

So far, the only reliable ways anyone has found to make money with AI are reselling tokens with a markup (and competing directly against the labs while you do it) or selling the infrastructure around them: sandboxes and the like.

Both things can be true at once. The economics can be cooked, *and* how I develop software has completely, fundamentally changed. I haven't written code by hand for two years. These models generate code better than most employers can actually hire for.

What changes is how you think about spend. The thing we're all going to discover over the next year is **where we actually need frontier intelligence and where we don't**. Frontier intelligence is expensive, and you don't need it for every single task.

- The models have been good enough for at least a year. There was a real jump from Sonnet 3.5 to the Opus 4-ish range; since then it's been marginal. The hype and hysteria around alignment and security change, but the capability mostly hasn't. These models have always known how to drive Kali and Burp Suite.
- Personally, I run a mix of GPT 6.1 sol on no or low reasoning, and GLM/Kimi. I use programming languages that let me get away with that (more on that below).
- Find the right model for the task, then buy those tokens wherever they're cheapest. Take an open-weights model like GLM, run it on Baseten, and it costs a fraction as much.
- If you're a company, check for zero data retention. At home? Just get your tokens cheap. There was a time when Z.ai's yearly GLM plan was $300 AUD a *year*, compared with $300 a *month* for the big labs' plans. Some of the smartest engineers I know bought twenty of them and ripped with concurrency.

It's not about the model anymore. It's about how you use the model.

## the last forty years of computing were designed around humans

Why does a computer have a console? Because we used to have mainframe operators. A whole lot of what we consider "how computers work" is really "how computers were made legible to people."

Back in March I took a detour, picked OCaml back up because it has an effect system built in, and started playing with unikernels via [MirageOS](https://mirage.io/?ref=ghuntley.com).

Most applications run *on* an operating system. What if the application *was* the operating system — a very thin one, with almost no overhead? Most software gets breached either through an exploit in the OS layer or through the app layer as a foothold into the OS. Delete the OS layer, and you delete that class of problem.

I ended up rebuilding essentially Erlang as a distributed operating system. Is that a dead end? Maybe. Sometimes these experiments are. But knowing what's possible, and where it works and where it doesn't, lets you rethink the last forty years and ask a better question: **can we delete instead of build?**

Humans aren't operating machines anymore either. You hand a Docker container to a Kubernetes cluster and an operator control loop manages it. Once the human is out of the loop, the design decisions that existed for the human are up for renegotiation.

I truly think Unikernels are going to come back into fashion hard over the next couple of years for security reasons. All the friction... that existed when architecting an application with this design no longer exists.

## readable vs explainable

People are still saying, very loudly, that code should be readable so that humans can understand it. I no longer think that's the goal.

Try this for fun: go play with Haskell this weekend. It doesn't matter if you've never touched it. The LLMs know it. There's a lot of dogma out there about things being hard to learn. Well, they're now false. Learning things like Nix and Haskell used to be pushing shit uphill and unachievable in a single sitting. But now the LLM knows it's easy.

Below, you'll find a function that I have deliberately obfuscated....

```haskell
f :: String -> String
f = g . h
  where
    h []     = []
    h (x:xs) = h xs ++ [x]
    g x =
      let (y, z) = p x
      in if y then z else g z
    p []       = (True, [])
    p [x]      = (True, [x])
    p (x:y:xs)
      | x > y     = let (a, b) = p (x:xs) in (False, y:b)
      | otherwise = let (a, b) = p (y:xs) in (a, x:b)
```

Right now you're probably looking at the code and going... What the actual ... is this?

Now copy that function into an LLM and prompt it to *"explain this function to me as if you were explaining it to my son or daughter but in Python as a reference."*

All of a sudden it becomes accessible. It reads like Python.

> This Haskell turns a string into its letters in alphabetical order. It does that in two steps: flip the string backwards, then keep bubbling letters into order until nothing moves.

```python
def flip(letters):
    """Put the string backwards. 'cat' becomes 'tac'."""
    if letters == "":
        return ""
    first = letters[0]
    rest = letters[1:]
    return flip(rest) + first

def one_pass(letters):
    """
    Walk the letters once.
    Whenever a bigger letter is sitting in front of a smaller one,
    slide the smaller one forward.
    Also report whether anything moved.
    """
    if len(letters) <= 1:
        return True, letters  # already tidy

    first = letters[0]
    second = letters[1]
    rest = letters[2:]

    if first > second:
        # smaller one goes in front; keep comparing the bigger one
        moved, after = one_pass(first + rest)
        return False, second + after
    else:
        # these two are fine; check the rest
        moved, after = one_pass(second + rest)
        return moved, first + after

def tidy_up(letters):
    """Keep doing one pass until a whole pass changes nothing."""
    done, letters = one_pass(letters)
    if done:
        return letters
    return tidy_up(letters)

def f(text):
    return tidy_up(flip(text))
```

That's the shift. The artifact doesn't need to be optimized for a human to read cold. It needs to be something a model can explain to a human on demand.

## types are back pressure

If you want a codebase that agents can maintain, you'll notice something quickly: **a Rust codebase is far more maintainable with agents than a Python one**. Types are a form of verification. They provide [back pressure](https://ghuntley.com/pressure/): compiler errors that the LLM picks up and fixes automatically, every loop.

Think of it as a continuum of power. Up one end, Haskell is even stricter than Rust. At the other end, you've got Ruby, Python, PHP, and Perl, where types were never a first-class citizen and are hard to bolt on after the fact.

This is also why I can get away with cheaper models. Pick a language that does the verification for you, and you need less intelligence to stay on the rails.

## programming languages move at the speed of human learning

Languages evolve through committees, and the pace of language development has been deliberately slowed to match the rate at which humans can learn new concepts.

I remember when monads arrived in .NET — LINQ in 3.5, championed by Erik Meijer. We're talking about `SelectMany`, flat map, to be blunt. It took an easy five to eight years before people were comfortable with it. Now it's everywhere in every programming language.

Or look at Python 2 to 3. It's been about fifteen years, and there are still wounds. The lesson the industry took was "you must not break users," so nobody makes breaking changes.

But what happens if the language designer ships a skills pack alongside the breaking change, and the agent just does the migration?

> If humans aren't the target audience, we don't have to dumb down the language for adoption, and the LLMs have the entire corpus of programming language theory — what can we do with that?

There are maybe ten or twelve truly great applied programming language designers in the world, and most of them have **specialized their careers in human accessibility**.

It's going to take people on the fringe of comp sci (the ones who were told, "you can't do that, nobody will learn it") to hear that this is now false. You can take research that's twenty years old, put it together, and the LLM will do it for you.

I've been collecting other nerds who are thinking along the same lines. No results to show yet. We're all just playing with this new substrate.

> If coding agents will write most of our code, what happens with our communities and sense of ergonomics? How does it impact our compilers and tools? [https://t.co/DaYCSHFUBY](https://t.co/DaYCSHFUBY?ref=ghuntley.com)
>
> — José Valim (@josevalim) [September 24, 2026](https://x.com/josevalim/status/2103133294317445290?ref_src=twsrc%5Etfw&ref=ghuntley.com)

Highly recommend reading this article. Programming language authors who build their languages around agent-first rather than human-first will get ahead. Those who don't will fall behind and end up like Solaris.

## languages are fungible now

Consider being a CTO before AI...

> "We need to hire a Ruby person. Over here it's .NET, so that's a separate team. And the company we acquired is Java."

The end business impact is that you are maintaining three teams, or teams of teams, on tribal ideology about programming languages.

Now you can [run loops](https://ghuntley.com/loop/) to port from one language to the next. A founder in the medical space in Australia had an old ASP.NET Web Forms app on IIS. Terrible, terrible, terrible; I've done it, I know. I challenged him to run some loops himself, then go back to his team the next morning without telling them and ask for an estimate on the rewrite. They said years. A week later the migration was done. He did it all himself and completely mogged his engineering team.

Porting is easy when the end result is easy to verify. Stealing an idea from a Go library and wanting it in Rust is easy now, and has been for at least a year.

[Can a LLM convert C, to ASM to specs and then to a working Z/80 Speccy tape? Yes.](https://ghuntley.com/z80)

✨Daniel Joyce used the techniques described in this post to port ls to rust via an objdump. You can see the code here: https://github.com/DanielJoyce/ls-rs. Keen, to see more examples - get in contact if you ship something! Damien Guard nerd sniped me and other folks wanted

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/A-black-and-white--low-angle-digital-illustration-in-a-symbolic-traditional-tattoo-art-style.--A-bald--light-skinned-man-with-a-bushy-beard--prominent-eyebrows--and-a-friendly-expression-wears-denim-overalls--2--c4421c9b-5f3c-40dc-838c-40b5235ce4e8.jpg)Geoffrey Huntley Geoffrey Huntley

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/bafkreiftzmpb7bbkej76rac3kuijg2mcuyjxxhtnqdie5qq6rbrc45v2iu-1-a7544c4e-6d21-416a-898f-997162311d6f.jpg)

It is provably true that languages are fungible, and it makes no business sense to carry so many of them in your business today.

Operating systems went through a convergence event: AIX, Solaris, HP-UX and IRIX all collapsed into Mac, Linux and Windows. I expect programming languages to converge the same way: down to a couple of gilded languages, or something new rises from the dust.

## the V8 hot rod

The usual objection to a new programming language is adoption. But you don't necessarily need adoption now when deciding whether to use a language, now that we have AI. Like, what libraries exist in this ecosystem don't factor into my decision to create a new project these days.

[what is the point of libraries now that you can just generate them?](https://ghuntley.com/libraries)

It’s a meme as accurate as time. The problem is that our digital infrastructure depends upon just some random guy in Nebraska. Open-source, by design, is not financially sustainable. Finding reliable, well-defined funding sources is exceptionally challenging. As projects grow in size, many maintainers burn out and

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/A-black-and-white--low-angle-digital-illustration-in-a-symbolic-traditional-tattoo-art-style.--A-bald--light-skinned-man-with-a-bushy-beard--prominent-eyebrows--and-a-friendly-expression-wears-denim-overalls--2--c23bc14e-360c-4106-9896-e25298235b65.jpg)Geoffrey Huntley Geoffrey Huntley

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/A-symbolic-traditional-tattoo-art-print-of-a-software-library-in-vibrant-retro-colors-with-complex-ornamental-details.-Creative-light-art-is-used-to-achieve-an-abstract-pattern-with-complimentary-colors--balance-36436c4b-e2ea-4613-85eb-539cecd7bc7d.jpg)

If you'd built some sick V8 hot rod and everyone else was on a horse, would you care whether they adopted it? "Yeah, enjoy that horse, mate."

That's why this area is interesting. Not "is there a faster programming language," but: can we incorporate more ideas from verification into our languages, stop optimizing for humans, and start optimizing for agents? Is there a way to go faster? I don't know yet. I guess we'll have to find out.

## what I'm building in the meantime

Since January, I've been talking about Loom and the idea of a software factory, something that optimizes a whole business function. It's on GitHub, it's not really usable, and it's not intended to be; it's research.

Software factories are very, very hard to do right now. For the last six months, my view has been that we need one of: better models, better programming languages, or a revival of some old computer science techniques like simulators.

Ultimately, I want to replace GitHub. I want to bring back something like Phabricator, the engineering system that predates GitHub and came out of Facebook's early days. Google has Piper and Rosie for large-scale refactoring. Meta has EdenFS, Mononoke and Sapling. If GitHub is the only thing you've seen, it seems fine. It's not good enough. I think Git is somewhat end-of-life; I really wish people would stop trying to extend Git's lifespan.

So right now I've got loops running that are rebuilding a source control system where:

- contents are encrypted;
- agents get a claim, which provides a materialized view of the repository;
- that means real ACLs. Share a sub-path with a contractor, much like Windows file permissions, instead of "the Git repo has everything."

It's a distributed system, so I'm building it in Rust and building **the simulator first**, then driving the agent to validate everything through the simulator. That has kept the agent on the rails remarkably well.

## chapters

- [0:55](https://www.youtube.com/watch?v=vrCE5m2MRys&t=55s&ref=ghuntley.com) — the economics of AI are cooked
- [2:30](https://www.youtube.com/watch?v=vrCE5m2MRys&t=150s&ref=ghuntley.com) — where you actually need frontier intelligence
- [4:30](https://www.youtube.com/watch?v=vrCE5m2MRys&t=270s&ref=ghuntley.com) — get your tokens cheap
- [5:31](https://www.youtube.com/watch?v=vrCE5m2MRys&t=331s&ref=ghuntley.com) — Loom and software factories
- [7:01](https://www.youtube.com/watch?v=vrCE5m2MRys&t=421s&ref=ghuntley.com) — rebuilding source control; Git is end of life
- [8:31](https://www.youtube.com/watch?v=vrCE5m2MRys&t=511s&ref=ghuntley.com) — simulator-first development
- [9:00](https://www.youtube.com/watch?v=vrCE5m2MRys&t=540s&ref=ghuntley.com) — OCaml, unikernels and forty years of human-centric computing
- [10:50](https://www.youtube.com/watch?v=vrCE5m2MRys&t=650s&ref=ghuntley.com) — readable vs explainable
- [13:30](https://www.youtube.com/watch?v=vrCE5m2MRys&t=810s&ref=ghuntley.com) — types as back pressure
- [14:30](https://www.youtube.com/watch?v=vrCE5m2MRys&t=870s&ref=ghuntley.com) — languages designed by committee
- [16:00](https://www.youtube.com/watch?v=vrCE5m2MRys&t=960s&ref=ghuntley.com) — languages are fungible
- [19:30](https://www.youtube.com/watch?v=vrCE5m2MRys&t=1170s&ref=ghuntley.com) — the V8 hot rod
- [20:30](https://www.youtube.com/watch?v=vrCE5m2MRys&t=1230s&ref=ghuntley.com) — optimise for agents, not humans

Keep curious.
