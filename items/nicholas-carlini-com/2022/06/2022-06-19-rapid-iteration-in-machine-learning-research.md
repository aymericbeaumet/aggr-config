---
title: Rapid Iteration in Machine Learning Research
link: https://nicholas.carlini.com/writing/2022/rapid-iteration-machine-learning-research.html
source: nicholas-carlini-com
published: 2022-06-19T00:00:00Z
updated: 2022-06-19T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: A brief discussion about a tool I use to make rapid iteration in ML research possible.
content: extracted
html: 2022-06-19-rapid-iteration-in-machine-learning-research.html
---

I can only do good research when I can quickly iterate on ideas. In this post I want to take just one minute talking about how I reduce my iteartion time in research, by introducing a tool I wrote that's helped me out for the past several years.

Briefly, it's a ~100 line script that allows me to snapshot the Python environment at an arbitrary point in time, and then interactively make changes to the code and run different experiments on the current state. Consider the following example, where we have some initial setup script that takes a few seconds to run, after which we want to perform some analysis on the result. By using this script I don't need to wait for the setup each time.

Find it on [GitHub here](https://github.com/carlini/pycallcc).

.

## “(Machine learning) research does not support rapid iteration”

Before I did ML research, I did system security research where if something took longer than a second to start up something was wrong. Then I started doing ML, and I would complain about something taking a minute to initialize and get laughed at because this was perfectly normal. So I took a few days out of my PhD research time and wrote a tool to make my workflow quite a bit faster.

Research is fundamentally easier for me when it's possible to interactively explore ideas. Any time where it takes longer than ~a second for a script to start giving useful output, I find that my ability to be productive starts to drop off. What makes this especially difficult is that I find anything that gets in between my ideas and implementing them incredibly frustrating.

## What problem does this tool solve?

Often I find myself in a situation where I am trying to quickly iterate on a function that can only be tested after some (slow to construct) state exists. Something like this:

```
                initialize_boring_state()  # takes forever
                do_something_interesting() # fast
              
```

This can be something as complicated as building a nasty in-memory datastructure that takes forever to construct, or as simple as loading a large amount of data off disk or just importing the TensorFlow library (which takes inordinately long). You can't actually go and do the interesting work until the slow thing has loaded. So what do you do?

Lots of people will tell you to go and just set up a jupyter notebook, put the first slow thing in the first cell, and then iterate on the second interesting function after having run the first cell once.

But what if you don't want to do that? This project gives another solution.

I've been using (various incarnations of) this tool for about six years now. I initially built it when I was working with a big malware dataset as a PhD student. It took about a minute to load off disk and I wanted to have a way to interactively query the data without waiting for it to load every time. Since then it's been a critical part of my workflow in the development of most of my research papers. You can actually [watch me livecode](https://nicholas.carlini.com/writing/2020/screen-recording-breaking-adversarial-example-defense.html) breaking a defense to adversarial examples using a version of this tool.
