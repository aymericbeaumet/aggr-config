---
title: Prompt Design
link: https://cursor.com/blog/prompt-design
source: cursor-com
published: 2023-06-11T00:00:00Z
updated: 2023-06-11T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Prompting is like web design. Let's call it prompt design, and build better tools for it.
content: extracted
html: 2023-06-11-prompt-design.html
preview:
  file: 2023-06-11-prompt-design.preview-18fdd54608f0.webp
  width: 256
  height: 134
  color: '#1d1b15'
images:
- source: https://ptht05hbb1ssoooe.public.blob.vercel-storage.com/assets/og/opengraph-default.png
  original:
    file: 2023-06-11-prompt-design.image-581af892e88b.png
    width: 2401
    height: 1260
  color: '#14120b'
- source: https://cursor.com/marketing-static/_next/image?url=https%3A%2F%2Fptht05hbb1ssoooe.public.blob.vercel-storage.com%2Fassets%2Fblog%2Fprompt-design-0.png&w=1920&q=70
  original:
    file: 2023-06-11-prompt-design.image-2737a423e8a7.png
    width: 1838
    height: 1124
  color: '#181818'
- source: https://cursor.com/marketing-static/_next/image?url=https%3A%2F%2Fptht05hbb1ssoooe.public.blob.vercel-storage.com%2Fassets%2Fblog%2Fprompt-design-1.png&w=1920&q=70
  original:
    file: 2023-06-11-prompt-design.image-33bb36d20a84.png
    width: 1578
    height: 1894
  color: '#f9f9f9'
---

I’m usually averse to the common habit of trying to find old-world analogues to new-world phenomena. So please bear with me as I commit that exact sin: **let me present the case for why prompting should be called** ***prompt design*** **and be likened to web design**.

I view prompting as communicating with a time-constrained human. While LLM-specific techniques are certainly helpful (most notably chain-of-thought), I’ve found that one of the best ways to improve performance is to just have extremely clear and high-quality instructions, similarly to how clarity and conciseness helps real humans understand better too.

*Prompting-as-clear-communication* makes prompting sound like writing. Most of the prompting I am doing, however, is parametric: I have a number of input variables, and need to dynamically adapt my prompt to those.

Hence, *prompting-as-clear-communication-with-dynamic-input* feels like the most accurate characterization.

What other field is about communicating clearly with dynamic input? Web design.

Let’s list out all the similarities. Prompting and web design both:

1. require clarity, and have communication as the primary goal;

2. need to respond to dynamic content, unlike writing or magazine layout; and

3. need to adapt their content to different sizes — screen sizes for web design, context windows for prompting.

In my experience from doing both prompting and web design, I’ve also found that I have similar developer preferences in the two areas:

1. *Looking at the actual prompt* is super important, just like *looking at the rendered website* is super important. I cannot design a website if I have to simulate the HTML and CSS rendering process in my mind. Similarly, it is really hard to write good and clear prompts without looking at the rendered output of a prompt with all input variables filled in.

2. For example, the prompt `"Hi ${username} ${message}"` may look reasonable, until you render it and realize that the user name blends in with the message.

3. Composable components are useful in both prompting and web design.

4. Declarative is better than imperative for both. It is really hard to change a website where all HTML elements are created with `document.createElement` calls. Similarly, reading and changing a prompt that consists of a long sequence of `str += "..."` easily becomes unmanageable.

5. In both, I sometimes want to achieve “pixel perfection”. When prompting less capable models (GPT-3.5 and worse), I want to make sure that I have no extraneous newlines or other types of imperfect formatting, and when designing a website, sometimes every pixel matters.

For LLM agents, it is possible to take the analogy even further: agent prompting can be seen as building an interactive website for the agents, where they can “click buttons” by calling functions, and where the prompt re-renders in response to a function call, just like a website re-renders in response to a button click.

Of course, there are differences between prompt design and web design:

1. Prompting deals with text only (for now!).

2. Caching is different: for agents in particular, you want to make sure that your re-renders are cheap by only changing the later parts of the prompt. There’s a contrived parallel here to the web (you want to cache-optimize your website), but I think it is fundamentally quite a different challenge.

Still, the similarities have convinced me that prompting should be called *prompt design*, not *prompt engineering*. Writing a prompt *feels just like designing a website*, and should thus be named the same way too.

The prompt design perspective inspired me to create [Priompt](https://github.com/anysphere/priompt), a React-like, JSX-based prompt design library.

## Priompt v0.1: a first attempt at a prompt design library

[Priompt](https://github.com/anysphere/priompt) is a first attempt at creating a prompt design library inspired by modern web design principles. We are using it internally at Cursor, and we like it a lot.

I think it is probably not exactly correct in all of its abstractions, but I am at least convinced that JSX is a far more ergonomic way to write prompts than string templates. Even the simple thing of trivially being able to comment out parts of your prompt makes the iteration loop much faster.

![](https://cursor.com/marketing-static/_next/image?url=https%3A%2F%2Fptht05hbb1ssoooe.public.blob.vercel-storage.com%2Fassets%2Fblog%2Fprompt-design-0.png&w=1920&q=70)

Priompt also comes with a (very hastily put together) preview website, where you can preview your prompt on real data. When developing your application, you can log the serialized `props` coming into a component on every request. Then, when you see unexpected behavior, you can go to the Priompt preview, look at the exact prompt, and change the source code and have the prompt update with the same `props` as the real request. We’ve found that this makes it easier to iterate on prompts

![](https://cursor.com/marketing-static/_next/image?url=https%3A%2F%2Fptht05hbb1ssoooe.public.blob.vercel-storage.com%2Fassets%2Fblog%2Fprompt-design-1.png&w=1920&q=70)

If you try it out, please let me know your thoughts! I’d love to see more ideas in the same vein, or just be told that I’m completely wrong and prompt design is stupid :).

## Caveats

Models change quickly, and so too do prompting techniques have to. With that, I think there are a few caveats with the *prompt design* characterization:

1. Pixel-perfect designs are unimportant for GPT-4, and will probably be obsolete for GPT-4.5 or better models.

2. The context window constraint may disappear, if one extrapolates the recent trend of long-context models. I am not convinced of this, though.

3. OpenAI appears to be moving in the direction of offering less and less control over the prompt to developers; it is possible that in a year there will be no such thing as a prompt, with the API call just asking us for the raw inputs plus an instruction. This trend of less control started with the chat format and has continued with the recently announced function calling

4. It’s possible that caching is one of the most important aspects of prompting, in which case it starts sounding a bit more like engineering than design.

5. Perhaps prompt design is too low-level, and should be left to a higher-level framework or compiler (e.g. [langchain](https://github.com/langchain-ai/langchain)). I think this may be true, but given the quickly changing nature of the LLMs, I personally prefer to be as close to the raw model as possible.

## Obligatory last note

*...because I’d love to work with you*: At Cursor, we’re building [Cursor](https://cursor.com/), an AI-first code editor. If you’re excited about prompt design, LLMs for coding, or building beautiful products, please email me at [arvid@cursor.com](mailto:arvid@cursor.com). We’re 5 people in San Francisco, all of my coworkers are exceptional, and we want a few more exceptional people — coders and designers — to join us in building the future of coding.
