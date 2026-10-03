---
title: A Codebase by an Agent for an Agent
link: https://ampcode.com/by-an-agent-for-an-agent
source: ampcode-com
published: 2025-12-18T00:00:00Z
updated: 2025-12-18T00:00:00Z
first_seen: 2026-10-03T23:09:50.514863041Z
summary: 'When Tim started to use Amp to build the TUI framework for our CLI, he often interrupted the agent. "No, this function should be called that," he''d say, or "this type should go into this file." But then he realized: wait, what I''m telling the agent to do - it''s not statistically likely. It''s not what an agent would do. So he stopped and instead let the agent build the framework and the codebase for itself. And now the agent rips through it.'
content: extracted
html: 2025-12-18-a-codebase-by-an-agent-for-an-agent.html
preview:
  file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.preview-ecfd224e2b36.webp
  width: 256
  height: 134
  color: '#575349'
images:
- source: https://ampcode.com/notes/by-an-agent-for-an-agent?og-image
  original:
    file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-bba7a29d3bb4.png
    width: 1200
    height: 630
  variants:
  - file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-dec983a1b7bd.webp
    width: 320
    height: 168
  - file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-0607067d9a07.webp
    width: 640
    height: 336
  - file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-6e3ace88433c.webp
    width: 960
    height: 504
  - file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-c84714f48567.webp
    width: 1200
    height: 630
  color: '#272925'
- source: https://ampcode.com/by-an-agent-for-an-agent/agent-stealing-feather.jpg
  original:
    file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-8fdb095c979e.jpg
    width: 500
    height: 437
  color: '#fdfdfc'
- source: https://ampcode.com/by-an-agent-for-an-agent/orb-writing-code.jpg
  original:
    file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-2e8b7c8e84aa.jpg
    width: 500
    height: 508
  color: '#fefefe'
- source: https://ampcode.com/by-an-agent-for-an-agent/cathedral-orb-codebase.jpg
  original:
    file: 2025-12-18-a-codebase-by-an-agent-for-an-agent.image-9b0be3801718.jpg
    width: 800
    height: 800
  color: '#fcfcfc'
---

Tim Culverhouse//Dec 18, 2025

When I’m coding there are many decisions I have to make that affect my ability to navigate the codebase: what’s a good name for this function? Should I make a new file for this type or should it be in an existing file? What should the file be called? Should I create a new directory for this?

![Agent stealing feather](https://ampcode.com/by-an-agent-for-an-agent/agent-stealing-feather.jpg)

Not only do I have to decide these things, but I have to remember them. Every time I open Neovim I have to think about what past-Tim would’ve called this. Where would he have put it? I get hit with cognitive overhead on each side of the decision.

The good thing is that now I can offload nearly all of this to an agent.

Yes, *I know*, one shouldn’t, but hear me out. I did exactly that when I created the TUI framework behind the Amp CLI. The framework is a port of [my Zig TUI library](https://github.com/rockorager/libvaxis) into TypeScript and it’s 90% written by Amp. And, [as Thorsten](https://x.com/thorstenball/status/1962926086884401518) and many others on the team, have said: Amp *rips* when working with this framework on the CLI. It plows through most anything you can throw at it. So much so that the question of “why is Amp so good at working on our CLI?” has come up multiple times. I think I have the answer. I think the answer is that I offloaded decisions to the agent.

The thing is — I *do care* about the names of things and how the codebase is structured. Early on in the process of writing the framework I would often stop the agent — irritated by a change it made — and tell it to rename a function, move a file to a different directory, or put some class into its own file. I don’t enjoy making these decisions, but I still wanted to make them. There’s just some satisfaction from having things look nice in your codebase — so I would stop the agent and tell it “I know better”.

But I don’t. I am *not* absolutely right, it turns out.

Let me give you an example. The Amp TUI framework uses a double-buffering approach toward updating the screen. We keep a front and a back screen — one representing the last frame, the other the next frame. Then we diff them, print the updates, and swap the screens. When porting the code from Zig to TypeScript, Amp named the function that swaps the screens `present()`. I didn’t like that and *I know better* so I renamed it to `swapScreens()`.

But as the port progressed, I saw Amp repeatedly trying to find a function called `present()`, not finding it, declaring “let me try something else”, and eventually finding the function under the name that had made sense to *me*.

Amp spun its wheels because *I* had interjected with *my* opinions about code naming, structure, and layout. But apparently my opinions aren’t the most statistically likely. When the agent reached for the name `present()`, its hand was guided by training data and the context I had provided. But I pushed it away from that, against its instincts, so to say, and made the agent’s job harder the next time. Amp could no longer ask “what would past-Amp have called this?” and find the answer in its weights — it had to know what past-*Tim* would call this.

![Orb writing code](https://ampcode.com/by-an-agent-for-an-agent/orb-writing-code.jpg)

I saw this repeat a few times, and realized my mistake. I was interfering with its ability to navigate using its own thoughts. I made it go against its grain. I slowed it down, cost more tokens as it ran more loops, and, ultimately, caused frustration for *myself* when I saw that it couldn’t remember the name.

So I changed my approach. No more telling the agent how to name things or where to put them. The agent gets to decide.

And it worked.

Once I took my hands off the wheel, the agent went faster and for longer. It chose names I wouldn’t have picked and file layouts that didn’t occur to me. It used more OOP patterns and classes than we have anywhere else in the codebase. Its use of generics and TypeScript primitives is *strange* when compared to the rest of our code.

But: it rips.

There was no denying that. Once it had finished the framework, it impressed us by being really, really good at *using* it — our own custom framework, mind you, that’s not documented anywhere and doesn’t fit into a single context window! With the framework it built, Amp can knock out things faster and more correctly than most of us. It knows how to add scrollbars to a new modal (we’re talking about a TUI here, remember!), it knows how the animation subsystem works, it knows how the keyboard shortcuts and click handlers work, and if there’s something missing in the framework, it adds it. “Oh, we don’t listen for this event. Let me fix that real quick.” And real quick it fixes it.

Ultimately I don’t know *why* it’s so good at using it, I can’t look inside its weights while it’s writing code, and I don’t trust its answers when I ask it about it, but here’s what I suspect.

The framework and its predecessor in Zig are inspired by Flutter. They have a Flutter-like API with Widgets and StatefulWidgets and Intents and Bindings and so on. And, I think, the agent recognizes that. It has enough Flutter in its weights. Even though Flutter is originally written in Dart, I think the agent recognized the Flutter in the Zig version and it can now recognize the Flutter in our codebase too and can look through the language and syntax and see the concepts at play.

And the codebase *lets it do that*.

After I gave it free rein over the codebase — letting it use the most *probable* names for functions and types, putting files where its “instincts” told it they should go — it built a framework for itself. A framework and a codebase in which things are exactly where a model that knows Flutter and that knows TypeScript — knows them in a probabilistic way — would first look for them. A codebase in which syntax and concepts strike a balance between what’s statistically likely and what actually compiles. A codebase in which the agent doesn’t have to second-guess what something is called and doesn’t have to wonder why something works the way it does. A codebase in equilibrium between what I want it to do, what the concepts require, and what the agent’s weights judge as the most likely way to achieve it.

A codebase by an agent for an agent.

![Cathedral orb codebase](https://ampcode.com/by-an-agent-for-an-agent/cathedral-orb-codebase.jpg)
