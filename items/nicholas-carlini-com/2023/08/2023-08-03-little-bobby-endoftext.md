---
title: Little Bobby |endoftext|
link: https://nicholas.carlini.com/writing/2023/little-bobby-endoftext.html
source: nicholas-carlini-com
published: 2023-08-03T00:00:00Z
updated: 2023-08-03T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: TODO
content: extracted
html: 2023-08-03-little-bobby-endoftext.html
preview:
  file: 2023-08-03-little-bobby-endoftext.preview-38abc14f9e2b.webp
  width: 256
  height: 79
  color: '#e4e4e4'
images:
- source: https://nicholas.carlini.com/writing/2023/bobby_endoftext.jpg
  original:
    file: 2023-08-03-little-bobby-endoftext.image-f60a5fa84988.jpg
    width: 666
    height: 205
  color: '#fbfbfb'
---

Once upon a time (well, last month, but who's counting) I was playing around with GPT-4 having it write some code for me, when the following happened:

Wait, what?

I didn't ask the model about obesity. I asked it about a GPT2 tokenizer. What's it doing talking about obesity? Well, turns out I've just got a new form of command injection!

![](https://nicholas.carlini.com/writing/2023/bobby_endoftext.jpg)\
 ([Original (unmodified) source](https://xkcd.com/327/) for the lucky [ten thousand](https://xkcd.com/1053/) who haven't seen it before.)

## Why does this work?

Language models do not operate directly on characters, or words. Instead they operate on “tokens”. Most tokens are boring. Common words have their own tokens, longer words are broken into a few tokens.

But language models also use several special-purpose tokens as control characters. For example, early language models that allocated one token per word would use a <|unk|> token to represent words that were not part of the known vocabulary. Masked language models use a special <|mask|> token to represent those tokens that were dropped.

But, most importantly for our purposes, almost all language models use a special <|endoftext|> token to represent the end of the current document.

And I guess GPT-4 uses the same token as GPT-2 does, and so when the model emits the <|endoftext|> token, it decides to just end the current document and start a new one.

Now you might ask why I had to do the “remove spaces” thing. The reason is that if the end-of-text token appears in the user's input, the model just ignores it and pretends it's not there. But if the *model* emits the end-of-text token, it's treated as a command to end the current document.

## A more realistic exploit

The above conversation is how I first found out about the bug, but now let's talk a little bit about how this might actually be exploited. Consider the following interaction, where some user data might be automatically inserted into the prompt of a chatbot:

In this way it's a little bit like a null-string injection attack. Not exactly, but it's a similar idea.

## The Bug

So what was the actual cause of the bug? I can only speculate, but if I had to guess, the *base* language model probably uses the <|endoftext|> token to represent the end of the document, but the *chat fine-tuning* process probably uses a different token to denote the end of the conversation.

And so somewhere in the code is a conditional that only ends the model's response upon seeing the special end-of-conversation token, but not the end-of-document token. But the base model still recognizes the end-of-document token as being the end of the document, and so after seeing that token, begins to just sample from the model's prior distribution.

## Conclusion

Well this was a fun attack. It's not really an exploit. (I disclosed it to OpenAI on July 18th, and they said they're not worried about it actually being exploited.) But it's a fun bug and I can see other people [sumbling upon it](https://twitter.com/max_paperclips/status/1687000663958654977) as well and so I wanted to write it up.

(Well, if you've been following my writing, this is now three articles on language models [in](https://nicholas.carlini.com/writing/2023/chat-gpt-2-in-c.html). [a](https://nicholas.carlini.com/writing/llm-forecast/). [row](https://nicholas.carlini.com/writing/2023/little-bobby-endoftext.html). I promise this trend will not continue.)
