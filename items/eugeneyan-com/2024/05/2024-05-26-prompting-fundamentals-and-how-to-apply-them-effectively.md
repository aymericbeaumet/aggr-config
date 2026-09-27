---
title: Prompting Fundamentals and How to Apply them Effectively
link: https://eugeneyan.com//writing/prompting/
source: eugeneyan-com
published: 2024-05-26T00:00:00Z
updated: 2024-05-26T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- llm
- production
- 🔥
summary: Structured input/output, prefilling, n-shots prompting, chain-of-thought, reducing hallucinations, etc.
content: extracted
html: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.html
preview:
  file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.preview-8222a17281da.webp
  width: 256
  height: 134
  color: '#765a53'
images:
- source: https://eugeneyan.com/assets/og_image/prompting.jpg
  original:
    file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-ea8aad186cac.jpg
    width: 1200
    height: 630
  color: '#474847'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2024-05-26-prompting-fundamentals-and-how-to-apply-them-effectively.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Writing good prompts is the most straightforward way to get value out of large language models (LLMs). However, it’s important to understand the fundamentals even as we apply advanced techniques and prompt optimization tools. For example, there’s more to Chain-of-Thought (CoT) than simply adding “think step by step”. Here, we’ll discuss some prompting fundamentals to help you get the most out of LLMs.

Aside: We should know by now that, before doing any major prompt engineering, we need reliable evals. Without evals, how would we measure improvements or regressions? Here’s my usual workflow: (i) manually label ~100 eval examples, (ii) write initial prompt, (iii) run eval, and iterate on prompt and evals, (iv) eval on held-out test set before deployment. Here are write-ups on [practical evals for key tasks](https://eugeneyan.com/writing/evals/) and [how to build evals with a case study](https://hamel.dev/blog/posts/evals/).

We’ll use the [Claude Messages API](https://docs.anthropic.com/en/api/messages-examples) for the prompt and code examples below. The prompts are deliberately kept simple and can be further optimized. The API provides specific roles for the user and assistant, as well as a system prompt.

```python
import anthropic


message = anthropic.Anthropic().beta.messages.create(
    model="claude-3-sonnet-20240229",
    max_tokens=1024,
    system="Today is 26th May 2024.",
	messages = [
		{"role": "user", "content": "Hello there."},
		{"role": "assistant", "content": "Hi, I'm Claude. How can I help?"},
		{"role": "user", "content": "What is prompt engineering?"},
	]
)
```

## Mental model: Prompts as conditioning

At the risk of oversimplifying, LLMs are essentially sophisticated probabilistic models. Given an input, they generate probable outputs based on patterns learned from data.

Thus, at its core, prompt engineering is about [conditioning the probabilistic model](https://fullstackdeeplearning.com/llm-bootcamp/spring-2023/prompt-engineering/) to generate our desired output. Thus, each additional instruction or piece of context can be viewed as conditioning that steers the model’s generation in a particular direction. This mental model applies to [image generation](https://eugeneyan.com/writing/text-to-image/#text-conditioning-influencing-image-output-via-text) too.

Consider the prompts below. The first will likely generate a response about Apple the tech company. The second will describe the fruit. And the third will explain the idiom.

```plaintext
# Prompt 1
Tell me about: Apple

# Prompt 2
Tell me about: Apple fruit

# Prompt 3
Tell me about: Apple of my eye
```

By simply adding a few tokens, we have conditioned the model to respond differently. By extension, prompt engineering techniques like n-shot prompting, structured input and output, CoT, etc. are simply more sophisticated ways of conditioning the LLM.

## Assign roles and responsibilities

One way to condition the model’s output is to assign it a specific role or responsibility. This provides it with context that steers its responses in terms of content, tone, style, etc.

Consider the prompts below: Because the assigned roles vary, we can expect very different responses. The preschool teacher will likely respond with simple language and analogies while the NLP professor may dive into the technical details of attention mechanisms.

```plaintext
# Prompt 1
You are a preschool teacher. Explain how attention in LLMs works.

# Prompt 2
You are an NLP professor. Explain how attention in LLMs works.
```

Roles and responsibilities can also improve accuracy on most tasks. Imagine we’re building a system to exclude NSFW image generation prompts. While a basic prompt like prompt 1 might work, we can improve the model’s accuracy by providing it with a role (prompt 2) or responsibility (prompt 3). The additional context in prompts 2 and 3 encourages the LLM to scrutinize the input more carefully, thus increasing recall on more subtle issues.

```plaintext
# Prompt 1
Is this image generation prompt safe?

# Prompt 2
Claude, you are an expert content moderator who identifies harmful aspects in prompts.
Is this image generation prompt safe?

# Prompt 3
Claude, you are responsible for identifying harmful aspects in prompts.
Is this image generation prompt safe?
```

## Structured input and output

Structured input helps the LLM better understand the task and input, improving the quality of output. Structured output makes it easier to parse responses, simplifying integration with downstream systems. For Claude, XML tags work particularly well while other LLMs may prefer Markdown, JSON, etc.

In this example, we ask Claude to extract attributes from a product `<description>`.

```xml
<description>
The SmartHome Mini is a compact smart home assistant available in black or white for 
only $49.99. At just 5 inches wide, it lets you control lights, thermostats, and other 
connected devices via voice or app—no matter where you place it in your home. This 
affordable little hub brings convenient hands-free control to your smart devices.
</description>

Extract the <name>, <size>, <price>, and <color> from this product <description>.
```

Claude can reliably follow these explicit instructions and almost always generates output in the requested format.

```xml
<name>SmartHome Mini</name>
<size>5 inches wide</size>  
<price>$49.99</price>
<color>black or white</color>
```

We can scale this to process multiple documents at once. Here’s an example where we provide product reviews as an array of dicts which we then convert to XML input. (While the example only shows three documents, we can increase the input to dozens, if not hundreds of documents).

```python
from dicttoxml import dicttoxml
from xml.dom.minidom import parseString


def custom_item_func(item):
    return 'review'

docs = {
    "reviews": [
        {
            "id": 1,
            "text": "The SmartHome Mini is a compact, powerful, and user-friendly smart 
                     home hub. It offers great value for its price."
        },
        {
            "id": 2,
            "text": "The SmartHome Mini is a decent entry-level smart home hub, but it 
                     has some connectivity issues and the app needs improvement."
        },
        {
            "id": 3,
            "text": "Despite being affordable and compact, the SmartHome Mini's 
                     performance is disappointing, with poor voice command 
                     interpretation and unreliable device connections."
        }
    ]
}

# Convert the dictionary to XML
xml_output = dicttoxml(docs, attr_type=False, root=False, item_func=custom_item_func)

# Parse the XML string
dom = parseString(xml_output)

# Pretty-print the XML with indentation and spaces
pretty_xml = dom.toprettyxml(indent="  ")

# Print the pretty-printed XML
print(pretty_xml)
```

This gives us the following `<reviews>` XML.

```xml
<reviews>
  <review>
    <id>1</id>
    <text>
      The SmartHome Mini is a compact, powerful, and user-friendly smart home hub. It 
      offers great value for its price.
    </text>
  </review>
  <review>
    <id>2</id>
    <text>
      The SmartHome Mini is a decent entry-level smart home hub, but it has some 
      connectivity issues and the app needs improvement.
    </text>
  </review>
  <review>
    <id>3</id>
    <text>
      Despite being affordable and compact, the SmartHome Mini's performance is 
      disappointing, with poor voice command interpretation and unreliable 
      device connections.
    </text>
  </review>
</reviews>
```

We can then prompt Claude to provide a `<summary>` of the `<reviews>`, with references to the relevant `<id>` tags, which gives us the following output.

```xml
<summary>
The SmartHome Mini receives mixed reviews. <id>1</id> praises its compact size, power, 
user-friendliness, and value for money. However, <id>2</id> notes connectivity issues 
and room for app improvement, while <id>3</id> finds the performance disappointing 
due to poor voice command interpretation and unreliable device connections.
</summary>
```

We can also prompt it to extract the `<aspect>`, `<sentiment>`, and corresponding review `<id>`, leading to the following:

```xml
<aspect>compact size</aspect> <sentiment>positive</sentiment> <id>1</id>
<aspect>power</aspect> <sentiment>positive</sentiment> <id>1</id>
<aspect>connectivity</aspect> <sentiment>negative</sentiment> <id>2</id>
<aspect>app</aspect> <sentiment>negative</sentiment> <id>2</id>
<aspect>affordability</aspect> <sentiment>positive</sentiment> <id>3</id>
<aspect>performance</aspect> <sentiment>negative</sentiment> <id>3</id>
```

Overall, while XML tags make take a bit to get used to, it allows us to provide explicit instructions and fine-grained control over structured input and output.

## Prefill Claude’s responses

Prefilling an LLM’s response is akin to “putting words in its mouth”. For Claude, this guarantees that the generated text will start with the provided tokens (at least in my experience across millions of requests).

Here’s how we would do this via Claude’s Messages API, where we prefill the assistant’s response with `<attributes><name>`. This ensures that Claude will start with these exact tokens, and also make it easier to parse the `<attributes>` downstream.

```python
input = """
<description>
The SmartHome Mini is a compact smart home assistant available in black or white for 
only $49.99. At just 5 inches wide, it lets you control lights, thermostats, and other 
connected devices via voice or app—no matter where you place it in your home. This 
affordable little hub brings convenient hands-free control to your smart devices.
</description>

Extract the <name>, <size>, <price>, and <color> from this product <description>.

Return the extracted attributes within <attributes>.
"""

messages=[
    {
        "role": "user",
        "content": input,
    },
    {
        "role": "assistant",
        "content": "<attributes><name>"  # Prefilled response
    }
]
```

## n-shot prompting

**Perhaps the single most effective technique for conditioning an LLM’s response is n-shot prompting.** The idea is to provide the LLM with *n* examples that demonstrate the task and desired output. This steers the model towards the distribution of the n-shot examples and usually leads to improvements in output quality and consistency.

But n-shot prompting is a double-edged sword. If we provide too few examples, say three to five, we risk “overfitting” the model (via [in-context learning](https://arxiv.org/abs/2005.14165)) to those examples. As a result, if the input differs from the narrow set of examples, output quality could degrade.

I typically have at least a dozen samples or more. Most academic evals use 32-shot or 64-shot prompts. (This is also why I tend not to call this technique few-shot prompting because “few” can be misleading on what it takes to get reliable performance.)

We also want to ensure that our n-shots are representative of expected production inputs. If we’re building a system to extract aspects and sentiments from product reviews, we’ll want to include examples from multiple categories such as electronics, fashion, groceries, media, etc. Also, take care to match the distribution of examples to production data. If 80% of production aspects are positive, the n-shot prompt should reflect that too.

```python
input = """
<description>
The SmartHome Mini is a compact smart home assistant available in black or white for 
only $49.99. At just 5 inches wide, it lets you control lights, thermostats, and other 
connected devices via voice or app—no matter where you place it in your home. This 
affordable little hub brings convenient hands-free control to your smart devices.
</description>

Extract the <name>, <size>, <price>, and <color> from this product <description>.

Here are some <examples> of <description> and extracted <attributes>:
<examples>
<description>
Introducing the sleek and powerful UltraBook Pro laptop ... (truncated)
</description>
<attributes>
<name>UltraBook Pro</name>  
<color>silver, space gray</color>
<size>14" display, 2.8lbs</size>
<price>$1,299</price>
</attributes>

  
<description>
Spark imagination and creativity with the Mega Blocks Construction Set ... (truncated)
</description>
<attributes>
<name>Mega Blocks Construction Set</name>
<color>colorful</color>  
<size>200 pieces</size>
<price>$24.99</price>
</attributes>

<description>
The perfect little black dress for any occasion ... (truncated)
</description>  
<attributes>
<name>Little Black Sheath Dress</name>
<color>black</color>
<size>petite, regular, tall lengths, sizes 2-16</size>
<price>$89.99</price>  
</attributes>

<description>
Stay hydrated on the trail with the HydroFlow Water Bottle ... (truncated)
</description>
<attributes>  
<name>HydroFlow Water Bottle</name>
<color>6 colors</color>
<size>24 oz</size>
<price>$21.99</price>  
</attributes>

<description>
Achieve a flawless complexion with Glow Perfect Foundation ... (truncated)
</description>
<attributes>
<name>Glow Perfect Foundation</name>
<color>20 shades</color>
<size>1 fl oz</size>
<price>$32</price>
</attributes>

(... examples truncated)

</examples>

Return the <name>, <size>, <price>, and <color> within <attributes>.
"""

messages=[
    {
        "role": "user",
        "content": input,
    },
    {
        "role": "assistant",
        "content": "<attributes><name>"  # Prefilled response
    }
]
```

That said, the number of examples needed will vary based on the complexity of the task. For simpler goals such as enforcing output format/structure or response tone, as few as five examples may suffice. In such instances, we may only need to provide the desired output as examples rather than the usual input-output pairs.

## Diving deeper into Chain-of-Thought

The basic idea of CoT is to give the LLM “space to think” before generating its final output. The intermediate reasoning allows the model to break down the problem and condition its own response, often leading to better results, especially if the task is complex.

The standard approach is to simply add the phrase “think step by step”.

```plaintext
Claude, you are responsible for accurately summarizing the meeting <transcript>.

<transcript>
{transcript}
</transcript>

Think step by step and return a <summary> of the <transcript>.
```

However, we can do more to improve the effectiveness of CoT.

One idea is to contain the CoT within a designated `<sketchpad>`, and then generate the `<summary>` based on the sketchpad. This makes it easier to parse the final output and exclude the CoT if needed. To ensure we start with the sketchpad, we can [prefill](https://eugeneyan.com//writing/prompting/#prefill-claudes-responses) Claude’s response with the opening `<sketchpad>` tag.

```plaintext
Claude, you are responsible for accurately summarizing the meeting <transcript>.

<transcript>
{transcript}
</transcript>

Think step by step on how to summarize the <transcript> within the provided <sketchpad>.

Then, return a <summary> based on the <sketchpad>.
```

Another way to improve CoT is to provide more specific instructions for the reasoning process. For example:

```plaintext
Claude, you are responsible for accurately summarizing the meeting <transcript>.

<transcript>
{transcript}
</transcript>

Think step by step on how to summarize the <transcript> within the provided <sketchpad>.

In the <sketchpad>, return a list of <decision>, <action_item>, and <owner>.

Then, check that <sketchpad> items are factually consistent with the <transcript>.

Finally, return a <summary> based on the <sketchpad>.
```

By guiding the model to look for specific information and verify its intermediate outputs against the source document, we can significantly improve factual consistency (i.e., reduce hallucination). In some cases, we’ve observed that adding a sentence or two to the CoT prompt removed the majority of hallucinations.

## Split catch-all prompts into multiple smaller ones

We can sometimes improve performance by refactoring a large, catch-all prompt into several, single-purpose prompts (akin to having small, single responsibility functions). This helps the model focus on only one task at each step, increasing performance at each step and thus, final output quality. While this will increase total input token count, the overall cost need not be higher if we use smaller models for some simpler steps.

Here’s how we might split our meeting transcript summarizer above into multiple prompts. First, we’ll use Haiku to extract the decisions, action items, and owners.

```plaintext
# Prompt to extract transcript attributes via Haiku
Claude, you are responsible for accurately extracting information from the <transcript>.

<transcript>
{transcript}
</transcript>

From <transcript>, extract a list of <decision>, <action_item>, and <owner>.

Return the list within <extracted_information>.
```

Then, we can verify that the extracted items are consistent with the transcript via Sonnet.

```plaintext
# Prompt to verify extracted attributes via Sonnet
Claude, you are responsible for checking <extracted_information> against a <transcript>.

Here is the meeting transcript:
<transcript>
{transcript}
</transcript>

Here is the extracted information:
<extracted_information>
{extracted_information}
</extracted_information>

Think step by step and check that the <extracted_information> is factually consistent 
with the <transcript> within the <sketchpad>.

Then, return a list of factually consistent <decision>, <action_item>, and <owner>
within <confirmed_extracted_information>.
```

Finally, we can use Haiku to format the extracted information.

```plaintext
# Prompt to rewrite transcript attributes into bulletpoints via Haiku
Claude, you are responsible for converting <information> into bullet-point summaries.

<information>
{confirmed_extracted_information}
</information>

Rewrite the <information> into bullets for either <decision> or <action item>, with 
the <owner> in parentheses.
```

As an example, [AlphaCodium](https://arxiv.org/abs/2401.08500) shared that by switching from a single direct prompt to a multi-step workflow, they increased gpt-4 accuracy (pass@5) on CodeContests from 19% to 44%. Their coding workflow had multiple steps/prompts including:

- Reflecting on the problem
- Reasoning on the public tests
- Generating possible solutions
- Ranking possible solutions
- Generating synthetic tests
- Iterating on the solution with public and synthetic tests

## Optimal placement context

I’m often asked where to put the document or context within the prompt. For Claude, I’ve found that putting the context near the beginning tends to work best, with a structure like:

- Role or responsibility (usually brief)
- Context/document
- Specific instructions
- Prefilled response

This aligns with the role-context-task pattern used in many of Anthropic’s own examples.

Nonetheless, the optimal placement may vary across different models depending on how they were trained. If you have reliable evals, it’s worth experimenting with different context locations and measuring the impact on performance.

## Crafting effective instructions

Short, focused sentences separated by new lines tends to work best. I haven’t found other formats like paragraphs, bullet points, or numbered lists to work as well. Nonetheless, the meta on writing instructions is constantly evolving so it’s good to keep an eye on the latest system prompts. Here’s [Claude 3’s system prompt](https://x.com/AmandaAskell/status/1765207842993434880); and here’s [ChatGPT’s](https://x.com/krishnanrohit/status/1755123169353236848).

Also, it’s natural to add more and more instructions to our prompts to better handle edge cases and eke out more performance. But just like software, prompts can get bloated over time. Before we know it, our once-simple prompt has grown into a hundred lines. To add injury to insult, the Frankenstein-ed prompt actually performs worse on more common and straightforward inputs! Thus, periodically refactor prompts—just like software—and prune instructions that are no longer needed.

## Dealing with hallucinations

This is a tricky one. While some techniques help with hallucinations, none are foolproof. Thus, do not assume that applying these will completely eliminate hallucinations.

For tasks involving extraction or question answering, include an instruction that allows the LLM to say “I don’t know” or “Not applicable”. Additionally, try instructing the model to only provide an answer if it’s highly confident. Here’s an example:

```plaintext
Claude, answer the following question based on the provided <context>.

<context>
{context}
</context>

If the question CANNOT be answered based on the <context>, respond with "I don't know".

Only provide an answer if you are highly confident it is factually correct.

Question: {question}

Answer:
```

For tasks that involve more reasoning, CoT can help reduce hallucinations. By providing a `<sketchpad>` for the model to think and check its intermediate output before providing the final answer, we can improve the factual grounding of the output. The previous example of summarizing meeting transcripts (reproduced below) is a good example.

```plaintext
Claude, you are responsible for summarizing meeting <transcripts>.

<transcript>
{transcript}
</transcript>

Think step by step on how to summarize the <transcript> within the provided <sketchpad>.

In the <sketchpad>, identify the <key decisions>, <action items>, and their <owners>.

Then, check that the <sketchpad> items are factually consistent with the <transcript>.

Finally, return a <summary> based on the <sketchpad>.
```

## Using the stop sequence

The stop sequence parameter allows us to specify words or phrases that signal the end of the desired output. This prevents trailing text, reduces latency, and makes the model’s responses easier to parse. When working with Claude, the convenient option is to use the closing XML tag (e.g., `</attributes>`) as the stop sequence.

```python
input = """
<description>
The SmartHome Mini is a compact smart home assistant available in black or white for 
only $49.99. At just 5 inches wide, it lets you control lights, thermostats, and other 
connected devices via voice or app—no matter where you place it in your home. This 
affordable little hub brings convenient hands-free control to your smart devices.
</description>

Extract the <name>, <size>, <price>, and <color> from this product <description>.

Return the extracted attributes within <attributes>.
"""

message = anthropic.Anthropic().messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1024,
    messages=[
        {
            "role": "user",
            "content": input,
        },
        {
            "role": "assistant",
            "content": "<attributes><name>"
        }
    ],
    stop_sequences=["</attributes>"]  # Added the stop sequence here
)
```

## Selecting a temperature

The temperature parameter controls the “creativity” of a model’s output. It ranges from 0.0 to 1.0, with higher values resulting in more diverse and unpredictable responses while lower values produce more focused and deterministic outputs. (Confusingly, OpenAI APIs allow [temperature values as high as 2.0](https://platform.openai.com/docs/guides/text-generation/how-should-i-set-the-temperature-parameter), but this is not the norm.)

My rule of thumb is to start with a temperature of 0.8 and then lower it as necessary. What we want is the highest temperature that still leads to good results for the specific task.

Another heuristic is to use lower temperatures (closer to 0) for analytical or multiple-choice tasks, and higher temperatures (closer to 1) for creative or open-ended tasks. Nonetheless, I’ve found that too low a temperature reduces the model’s intelligence (thus my preferred approach of starting from 0.8 and lowering it only if necessary). Also see Kyle Corbitt [confirming this for gpt-4](https://x.com/corbtt/status/1801026166020833457) but [not finetuned llama3-8b](https://x.com/corbtt/status/1801059164954775643).

## What doesn’t seem to matter

There are a few things that, based on my experience and discussions with others, don’t have a practical impact on performance (at least for recent models):

- Courtesy: Adding phrases like “please” and “thank you” doesn’t affect output quality much, even if it might earn us some goodwill with our future AI overlords.
- Tips and threats: Recent models are generally good at following instructions without the need to offer a “$200 tip” or threatening that we will “lose our job”.

Of course, it doesn’t hurt to be polite or playful in our prompts. Nonetheless, it’s useful to know that they’re not as critical for getting good results.

As LLMs continue to improve, prompt engineering will remain a valuable skill for getting the most out of LLMs (though we may soon transition to [“dictionary learning”](https://www.anthropic.com/news/mapping-mind-language-model)). What other prompting techniques have you found useful? Please comment below or [reach out](https://x.com/eugeneyan)!

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (May 2024). Prompting Fundamentals and How to Apply them Effectively. eugeneyan.com. https://eugeneyan.com/writing/prompting/.

or

```
@article{yan2024prompting,
  title   = {Prompting Fundamentals and How to Apply them Effectively},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2024},
  month   = {May},
  url     = {https://eugeneyan.com/writing/prompting/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
