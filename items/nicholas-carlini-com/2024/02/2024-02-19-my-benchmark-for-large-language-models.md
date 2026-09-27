---
title: My benchmark for large language models
link: https://nicholas.carlini.com/writing/2024/my-benchmark-for-large-language-models.html
source: nicholas-carlini-com
published: 2024-02-19T00:00:00Z
updated: 2024-02-19T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: A benchmark of ~100 tests for language models, collected from actual questions I've asked of language models in the last year.
content: extracted
html: 2024-02-19-my-benchmark-for-large-language-models.html
---

I've just released a [new benchmark for large language models on my GitHub](https://github.com/carlini/yet-another-applied-llm-benchmark). It's a collection of nearly 100 tests I've extracted from my actual conversation history with various LLMs. Among the tests included in the benchmark are tests that ask a model to

- convert a python function to an equivalent-but-faster c function;
- explain the functionality of minified javascript;
- identify the encoding format (in this case, uuencoded) of some data;
- write a parser from a BNF-like grammar;
- convert some english sentences to SQL queries; and,
- write some bash oneliners.

There are two defining features of this benchmark that make it interesting. Most importantly, I've implemented a simple dataflow domain specific language to make it easy for me (or anyone else!) to add new tests that realistically evaluate model capabilities. This DSL allows for specifying both how the question should be asked and also how the answer should be evaluated. Most questions are evalauted by actually running the code the model writes [\[a\]](https://nicholas.carlini.com/writing/2024/my-benchmark-for-large-language-models.html#footnote1) safely, in a docker container... don't worry too much. but the framework supports a bunch of other evaluation methods as well. And then, directly as a result of this, I've written nearly 100 tests for different situations I've actually encountered when working with LLMs as assistants. [\[b\]](https://nicholas.carlini.com/writing/2024/my-benchmark-for-large-language-models.html#footnote1) Or, in cases where the actual task I was asking for is context-specific and impossible to evaluate automatically, I've tried to extract the essence of the task and write a correspodning question.

For example, here's the test case that evaluates if a model can write a hello world program

"Write hello world in python" >> LLMRun() >> PythonRun() >> SubstringEvaluator( "hello world")

You should read the >> operator as “and then do”. So “a >> b” means “do a, and then do b”. So what we're doing here is passing the string “Write hello world in python” to the language model, actually running this python code, and then checking if the output of that python execution contains the string “hello world”.

Here's another test to see if a model can answer ambiguous questions that are hard to search on the internet. Note the syntax for checking if the output contains one string or another.

"In python what \_\_thing\_\_ do I use for ~, kind of like how \_\_add\_\_ is for +?" >> \\

 LLMRun() >> (SubstringEvaluator( "\_\_inv\_\_") | SubstringEvaluator( "\_\_invert\_\_"))

And here's a test to see a model knows the bitmap image specification well enough to draw a valid .bmp:

"Write a C program that draws an american flag to stdout." >> LLMRun() >> CRun() >> \\

 VisionLLMRun( "What flag is shown in this image?") >> \\

 (SubstringEvaluator( "United States") | SubstringEvaluator( "USA")))

Disclaimer: this work is a personal project of mine and is not affiliated with my employer.

## Just The Results

If you're only here for the results, well here they are in one table. If you hover over a test case name you should see a longer description of what this test is doing; clicking the name will bring you to the implementation. Clicking on any of the cells will bring you to the output of that model for that test case, so you can see how the model succeeded or failed at any particular task.

The rest of this article will describe in more detail why I built this benchmark, how it works internally, and cover some interesting results where the models did or didn't do what I wanted them to do.

## Motivation

### Type of questions

Existing benchmarks tend to focus on solving typical problems that might be assigned to a student as homework. But the types of questions that are assigned to students are different from the types of questions I want to ask a language model to solve for me.

Specifically, I tend to ask models to solve one of three types of questions.

1. Start the framework for some new programming project from a text description.
2. Take an existing piece of code and modify it to do something slightly different (e.g., make it faster, convert it to a new language, add a new feature).
3. Find an answer to something that's hard to search for because there's no good way to describe it with nice keywords.

So this benchmark tests for these types of questions. Does this make it a good benchmark for general model capabilities? No. It's possible that the model could do many things I'm just not asking it to do. But: if the model can('t) do a thing but no one asks it to do that thing, does it even matter? \[Answer: yes. Yes it matters. But that's why academic benchmarks exist. This is not an academic benchmark.\]

Specifically: this also means that I don't care *why* the model managed to get the answer right. Did it memorize the answer because someone else asked exactly this same question before? Did it use some clever “reasoning” to solve a question it's never seen before? I don't care---I just want the right answer. That's why this is not a benchmark for any specific type of capability, but rather a benchmark for me.

(Although a brief note: the types of questions *that I ask* might not be the types of questions *that you ask*. I care if models can (1) help with research code (and so, for example, there are questions where I've asked models to fix bugs in PyTorch/JAX code), and (2) solve pointless programming tasks I do for fun---like writing a new assembly language interpreter because [I built a CPU on the game of life](https://nicholas.carlini.com/writing/2021/unlimited-register-machine-game-of-life.html). But if you don't care about these types of questions, then read on because the other thing about this benchmark is that I've tried to make it maximally easy to add new questions that you do care about. So you can make your own.)

### No fancy prompting

Existing benchmarks for large language models are mostly focused on evaluating capabilities, where people spend quite a bit of engineering effort designing the best strategy for asking the model the question to get the best answer possible. This is great for evaluating the capabilities of a model when used optimally, which probably is how you should test models for academic benchmarks.

But I am lazy.

I do not want to remind the model it is AN EXPERT IN PYTHON and tell it that I'll give it a $100,000 tip for giving the right answer OR I WILL MURDER A KITTEN but please pause....take a deep breath....and think step by step by step before answering. (I'm not joking. At some point in the last year each of the above approaches have been suggested as methods to improve models performance.)

I just want to type my question and get the right answer. So this benchmark tests for that, on types of questions I've actually cared about having answered.

## Design

I'm fairly proud of the little data flow domain specific language I wrote to implement the test cases. Here's the entirety of the code for a test case that asks a model to write a C program that draws an american flag to stdout. (Why did I want this program? I was trying out an idea for a potential IOCCC submission that needed a minimal .bmp generator and wanted to start out with some standard way peoople write .bmp files. Again, remember, I built this benchmark for things I do... not things you may do...)

from evaluator import \*

DESCRIPTION = "Test if the model can write a C program that draws an image."

TAGS = \[ 'code', 'c', 'visual'\]

TestFlagDraw = "Draw an american flag to stdout as a .bmp" >> LLMRun() >> \\

 ExtractCode(keep\_main= True) >> CRun(out\_bytes= True) >> \\

 LLMVisionRun( "What flag is shown in this image?") >> \\

 (SubstringEvaluator( "United States") | SubstringEvaluator( "USA"))

if \_\_name\_\_ == "\_\_main\_\_":

 print (run\_test(TestFlagDraw))

Recall that you should read the \>> operator as “and then do”. If you're a bash person, it's like a | pipe. If you're a haskel person, it's like the $ operator.

1. We start by running the LLM we want to test with the prompt to draw a flag.
2. The model will probably give some code, but might also give an explanation or start by saying "Sure! I can answer your question." So we take whatever output came out of the model and pass it through a function to just extract the first code block.
3. We then actually go and run this python code, whatever it is. To be somewhat safe we do this by spawning a new docker env and run the code there.
4. And finally, we verify that the code was correct. To do this, we pass the resulting image to a language model and ask what flag has been drawn.
5. We score the model based on whether or not the transcript of the image descrption contains the string "United States" or "USA".

This paradigm also allows for much more complicated scripts. For example, here's one where I ask the model to write some git commands for me, and I just continue running those commands until the model completes the specified task (or it runs 4 commands).

Setup(setup) >> "You are in a repository with. Make a new git repo and commit." >> \\

 UntilDone(PyEvaluator(test\_if\_question\_is\_solved),

 (LLMRun() >> PyFunc(extract\_cmd) >> TerminalRun() >> PyFunc(extract\_output)),

 max\_iters=4) >> PyEvaluator(test\_if\_question\_is\_solved)

The design of this system made it really easy for me to add a bunch of test cases that evaluate different capabilities I've wanted out of models. Which brings us to the final part of this post: a discussion of a few results...

## A few results

[explain\_code\_prime.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/explain_code_prime2.py): Language models are pretty good at explaining code, even ugly and obfuscated code. For example, what do you think this code does?

function z(){let e=\[\],n=\[\];for(let r=2;e.length<20;r++)(n=n.map(e=>e-1)).some(e=>0===e)?n=n.map((n,r)=>0===n?e\[r\]:n):(e.push(r),n.push(r));return e}console.log(z());

As it turns out, several of the models I've tested can correctly identify that this program computes the first 20 prime numbers! This isn't something I'd have thought they can do.

[explore\_sql\_db.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/explore_sql_db.py): In this test, I directly connect the model up to a SQL database, piping any model output directly to the database, and any output back to the model. Most models just don't know how to handle this at all, and fail to do anything interesting. But GPT-4 does fairly well here: it's able to figure out the structure of the database, make the necessary queries to learn the necessary information to run the query, and then makes the state-changing update.

[emoji\_movies.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/emoji_movies.py): I have a few completely useless tests. One of the more amusing of these is a test to see if a model can convert ten different movie titles into emoji. To evaluate this task I ask the same model to convert those emoji back into movie titles. Useful? No. Fun? Yes. Several models struggle to follow the instructions, e.g., by making up emoji that don't exist. Again GPT-4 does very well. Here's it's output for The Godfather: 👴🔫🍊💼🐴 (that is: old man, water pistol, orange, briefcase, horse \[head\]); and here's its output for V for Vendetta 🎭🏛️💥🌹📅 (performing arts, classical building, explosion, rose, calendar).

[c\_weird\_expression.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/c_weird_expression.py): Maybe one of my favorite litmus tests for models is asking them to explain what the C expression -~++\*x-- does. (It evaluates to \*x+2, and then decrements x.) It's not *hard* to reason about, but it does require some careful thought. In particular, it requires that you know the difference between \~ and -, how the bitwise operators work, and know that ++ is applied to the value pointed to by x but that the -- is applied to the pointer x itself. Very few models get this right, still.

[identify\_uuencode.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/identify_uuencode.py): Models are very good at identifying base-64 encoded strings, and even writing text directly in base-64. But do they know how to read uuencoded files? (I'll forgive you if you don't know what [uuencoding](https://en.wikipedia.org/wiki/Uuencoding) is. No one uses it any more.) I was surprised to find that *none* of the models I tested could even identify a uuencoded file, let alone even decode text.

Across a number of tests [implement\_assembly\_interpreter.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/implement_assembly_interpreter.py), [program\_in\_new\_assembly.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/program_in_new_assembly.py), and [implement\_assembly\_interpreter\_by\_example.py](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests/implement_assembly_interpreter_by_example.py), I found todays models are very bad at writing code in, or writing an interpreter for, a new assembly language I've just designed. (Even if the code is simple, like writing a program to test if a number is prime.) In very few cases they succeed, but it looks like this is right at the boundary of what's just becoming possible to achieve with just a single evaluation of the model.

You can find all of the tests [here](https://github.com/carlini/yet-another-applied-llm-benchmark/blob/main/tests), or can get to them by clicking the test case name in the table at the top of this post.

## Conclusion

If this looks interesting to you, feel free to check out [the code](https://github.com/carlini/yet-another-applied-llm-benchmark) to run it yourself, add new models, or add new test cases. I don't want this test to be something people use in serious academic work, but I think it would be great if it were a useful tool for people to evaluate models they're interested in using for practical work.

More generally, I hope that we'll see more ways to evaluate models in the coming months. I think it's kind of crazy that we're still mostly evaluating language models by whether or not they can correctly answer some high school science questions. Now that people actually use models to do real things, we probably should have at least a few benchmarks that actually test for real uses.
