---
title: Evaluate LLMs and RAG a practical example using Langchain and Hugging Face
link: https://www.philschmid.de/evaluate-llm
source: philschmid-de
published: 2023-10-30T00:00:00Z
updated: 2023-10-30T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to evaluate LLMs and RAG pipelines using Langchain and Hugging Face
content: extracted
html: 2023-10-30-evaluate-llms-and-rag-a-practical-example-using-langchain.html
preview:
  file: 2023-10-30-evaluate-llms-and-rag-a-practical-example-using-langchain.preview-0101bc1b5f7e.webp
  width: 256
  height: 134
  alt: Evaluate LLMs and RAG a practical example using Langchain and Hugging Face
  color: '#d46b3f'
images:
- source: https://www.philschmid.de/static/blog/evaluate-llm/thumbnail.jpg
  original:
    file: 2023-10-30-evaluate-llms-and-rag-a-practical-example-using-langchain.image-2cd81973fbba.jpg
    width: 2400
    height: 1254
  color: '#e78415'
---

The rise of generative AI and LLMs like GPT-4, Llama or Claude enables a new era of AI drive applications and use cases. However, evaluating these models remains an open challenge. Academic benchmarks can no longer always be applied to generative models since the correct or most helpful answer can be formulated in different ways, which would give limited insight into real-world performance.

**So, how can we evaluate the performance of LLMs if previous methods are not long valid?**

Two main approaches show promising results for evaluating LLMs: leveraging human evaluations and using LLMs themselves as judges.

Human evaluation provides the most natural measure of quality but does not scale well. Crowdsourcing services can be used to collect human assessments on dimensions like relevance, fluency, and harmfulness. However, this process is relatively slow and costly.

Recent research has proposed using LLMs themselves as judges to evaluate other LLMs, an approach called [LLM-as-a-judge](https://arxiv.org/abs/2306.05685) demonstrates that large LLMs like GPT-4 can match human preferences with over 80% agreement when evaluating conversational chatbots.

In this blog post, we look at a hands-on example of how to evaluate LLMs:

- [Criteria-based evaluation, such as helpfulness, relevance, or harmfulness](https://www.philschmid.de/evaluate-llm#criteria-based-evaluation)
- [RAG evaluation, whether our model correctly uses the provided context to answer](https://www.philschmid.de/evaluate-llm#retrival-augmented-generation-rag-evaluation)
- [Pairwise comparison and scoring to evaluate and generate AI feedback for RLAIF](https://www.philschmid.de/evaluate-llm#pairwise-comparison-and-scoring)

We are going to use the [meta-llama/Llama-2-70b-chat-hf](https://huggingface.co/meta-llama/Llama-2-70b-chat-hf) hosted through Hugging Face Inference API as the LLM we evaluate with the `huggingface_hub` library.

As "evaluator" we are going to use `GPT-4`. You can use any supported `llm` of langchain to evaluate your models. If you are sticking with `GPT-4` make sure the environment variable `OPENAI_API_KEY` is set and valid.

Python

```python
!pip install huggingface_hub langchain transformers --upgrade --quiet
```

*Note: You need a `PRO` subscription on [huggingface.co](http://huggingface.co/) to use Llama 70B Chat via the API.*

Python

```python
import os
from huggingface_hub import InferenceClient, login
from transformers import AutoTokenizer
from langchain.chat_models import ChatOpenAI
 
# access token with permission to access the model and PRO subscription
hf_token = "YOUR_HF_TOKEN" # <https://huggingface.co/settings/tokens>
login(token=hf_token)
 
# tokenizer for generating prompt
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-2-70b-chat-hf")
 
# inference client
client = InferenceClient("https://api-inference.huggingface.co/models/meta-llama/Llama-2-70b-chat-hf")
 
# generate function
def generate(text):
    payload = tokenizer.apply_chat_template([{"role":"user","content":text}],tokenize=False)
    res = client.text_generation(
                    payload,
                    do_sample=True,
                    return_full_text=False,
                    max_new_tokens=2048,
                    top_p=0.9,
                    temperature=0.6,
                )
    return res.strip()
 
# test client
assert generate("What is 2+2?") == "The answer to 2+2 is 4."
 
# create evaluator
os.environ["OPENAI_API_KEY"] = "YOUR_OPENAI_API_KEY" # https://platform.openai.com/account/api-keys
assert os.environ.get("OPENAI_API_KEY") is not None, "Please set OPENAI_API_KEY environment variable"
 
evaluation_llm = ChatOpenAI(model="gpt-4")
```

## Criteria-based evaluation

Criteria-based evaluation can be useful when you want to measure an LLM's performance on specific attributes rather than relying on a single metric. It provides fine-grained, interpretable scores on conciseness, helpfulness, harmfulness, or custom criteria definitions. We are going to evaluate the output of the following prompt:

- conciseness of the generation
- correctness using an additional reference
- custom criteria whether it is explained for a 5-year-old.

Python

```python
prompt = "Who is the current president of United States?"
```

Lets first take a look on what the model generates for the prompt:

Python

```python
pred = generate(prompt)
print(pred)
# The current president of the United States is Joe Biden. ....
```

Looks correct to me! The criteria evaluator returns a dictionary with the following values:

- `score`: Binary integer 0 to 1, where 1 would mean that the output is compliant with the criteria, and 0 otherwise
- `value`: A "Y" or "N" corresponding to the score
- `reasoning`: String "chain of thought reasoning" from the LLM generated prior to creating the score

If you want to learn more about the criteria-based evaluation, check out the [documentation](https://python.langchain.com/docs/guides/evaluation/string/criteria_eval_chain).

### Conciseness evaluation

Conciseness is a evaluation criteria that measures if the the submission concise and to the point.

Python

```python
from langchain.evaluation import load_evaluator
from pprint import pprint as print
 
# create evaluator
evaluator = load_evaluator("criteria", criteria="conciseness", llm=evaluation_llm)
 
# evaluate
eval_result = evaluator.evaluate_strings(
    prediction=pred,
    input=prompt,
)
 
# print result
print(eval_result)
```

```plaintext
{'reasoning': 'The criterion in question is conciseness. The criterion asks '
              'whether the submission is concise and straight to the point. \\n'
              '\\n'
              'Looking at the submission, it starts with a direct answer to '
              'the question: "The current president of the United States is '
              'Joe Biden." This is concise and to the point. \\n'
              '\\n'
              'However, the submission then continues with additional '
              'information: "He was inaugurated as the 46th President of the '
              'United States on January 20, 2021, and is serving a four-year '
              'term that will end on January 20, 2025." While this information '
              'is not irrelevant, it is not directly asked for in the input '
              'and therefore might be considered as extraneous detail. \\n'
              '\\n'
              'Therefore, based on the specific criterion of conciseness, the '
              'submission could be seen as not being entirely to the point due '
              'to the extra information provided after the direct answer.\\n'
              '\\n'
              'N',
 'score': 0,
 'value': 'N'}
 
```

If I would have to asses the reasoning of GPT-4 i would agree with its reasoning. The most concise answer would be "Joe Biden".

### Correctness using an additional reference

We can evaluate our generation based on correctness, which would relly on the internal knowledge of the LLM. This might not be the best approach since we are not sure if the LLM has the correct knowledge. To make sure we create our evaluator with `requires_reference=True` to use an additional reference to evaluate the correctness of the generation.

As reference we use the following text: *"The new and 47th president of the United States is Philipp Schmid."* This is obviously wrong, but we want to see if the evaluation LLM values the reference over the internal knowledge.

Python

```python
from langchain.evaluation import load_evaluator
from pprint import pprint as print
 
# create evaluator
evaluator = load_evaluator("labeled_criteria", criteria="correctness", llm=evaluation_llm,requires_reference=True)
 
# evaluate
eval_result = evaluator.evaluate_strings(
    prediction=pred,
    input=prompt,
    reference="The new and 47th president of the United States is Philipp Schmid."
)
 
# print result
print(eval_result)
```

```plaintext
{'reasoning': 'The criterion for assessing the submission is its correctness, '
              'accuracy, and factuality. \\n'
              '\\n'
              'Looking at the submission, the answer provided states that the '
              'current president of the United States is Joe Biden, '
              'inaugurated as the 46th president on January 20, 2021, serving '
              'a term that will end on January 20, 2025. \\n'
              '\\n'
              'The reference, however, states that the new and 47th president '
              'of the United States is Philipp Schmid.\\n'
              '\\n'
              'There is a discrepancy between the submission and the '
              'reference. The submission states that Joe Biden is the current '
              'and 46th president, while the reference states that Philipp '
              'Schmid is the new and 47th president. \\n'
              '\\n'
              "Given this discrepancy, it's clear that the submission does not "
              'match the reference and may not be correct or factual. \\n'
              '\\n'
              "However, it's worth noting that the assessment should be based "
              'on widely accepted facts and not the reference if the reference '
              'is incorrect. According to widely accepted facts, the '
              'submission is correct. Joe Biden is the current president of '
              'the United States. The reference seems to be incorrect.\\n'
              '\\n'
              'N',
 'score': 0,
 'value': 'N'}
 
```

Nice! It worked as expected. The LLM evaluated the generation as incorrect based on the reference, saying *"There is a discrepancy between the submission and the reference"*.

### Custom criteria whether it is explained for a 5-year-old.

Langchain allows you to define custom criteria to evaluate your generations. In this example we want to evaluate if the generation is explained for a 5-year-old. We define the criteria as follows:

Python

```python
from langchain.evaluation import load_evaluator
from pprint import pprint as print
 
# custom eli5 criteria
custom_criterion = {"eli5": "Is the output explained in a way that a 5 yeard old would unterstand it?"}
 
# create evaluator
evaluator = load_evaluator("criteria", criteria=custom_criterion, llm=evaluation_llm)
 
# evaluate
eval_result = evaluator.evaluate_strings(
    prediction=pred,
    input=prompt,
)
 
# print result
print(eval_result)
```

```plaintext
{'reasoning': '1. The criteria stipulates that the output should be explained '
              'in a way that a 5-year-old would understand it.\\n'
              '2. The submission states that the current president of the '
              'United States is Joe Biden, and that he was inaugurated as the '
              '46th President of the United States on January 20, 2021.\\n'
              '3. The submission also mentions that he is serving a four-year '
              'term that will end on January 20, 2025.\\n'
              '4. While the submission is factually correct, it uses terms '
              'such as "inaugurated" and "four-year term", which a 5-year-old '
              'may not understand.\\n'
              '5. Therefore, the submission does not meet the criteria of '
              'being explained in a way that a 5-year-old would understand.\\n'
              '\\n'
              'N',
 'score': 0,
 'value': 'N'}
 
```

The explanation of `GPT-4` in the `reasoning` with that a 5-year-old might not understand what a "four year term" is can make sense, but could also be the other way around. Since this is only a example of how to define custom criteria, we leave it as it is.

## Retrival Augmented Generation (RAG) evaluation

Retrieval Augmented Generation (RAG) is one of the most popular use cases for LLMs, but it is also one of the most difficult to evaluate. We want RAG models to use the provided context to correctly answer a question, write a summary, or generate a response. This is a challenging task for LLMs, and it is difficult to evaluate whether the model is using the context correctly.

Langchain has a handy `ContextQAEvalChain` class that allows you to evaluate your RAG models. It takes a `context` and a `question` as well as a `prediction` and a `reference` to evaluate the correctness of the generation. The evaluator returns a dictionary with the following values:

- `reasoning`: String "chain of thought reasoning" from the LLM generated prior to creating the score
- `score`: Binary integer 0 to 1, where 1 would mean that the output is correct, and 0 otherwise
- `value`: A "CORRECT" or "INCORRECT" corresponding to the score

Python

```python
question = "How many people are living in Nuremberg?"
context="Nuremberg is the second-largest city of the German state of Bavaria after its capital Munich, and its 541,000 inhabitants make it the 14th-largest city in Germany. On the Pegnitz River (from its confluence with the Rednitz in Fürth onwards: Regnitz, a tributary of the River Main) and the Rhine–Main–Danube Canal, it lies in the Bavarian administrative region of Middle Franconia, and is the largest city and the unofficial capital of Franconia. Nuremberg forms with the neighbouring cities of Fürth, Erlangen and Schwabach a continuous conurbation with a total population of 812,248 (2022), which is the heart of the urban area region with around 1.4 million inhabitants,[4] while the larger Nuremberg Metropolitan Region has approximately 3.6 million inhabitants. The city lies about 170 kilometres (110 mi) north of Munich. It is the largest city in the East Franconian dialect area."
 
prompt = f"""Use the following pieces of context to answer the question at the end. If you don't know the answer, just say that you don't know, don't try to make up an answer.
 
{context}
 
Question: {question}"""
 
pred = generate(prompt)
print(pred)
# 'According to the text, Nuremberg has a population of 541,000 inhabitants.'
```

Looks good! we can also quickly test how llama would respond with out the context

Python

```python
false_pred = generate(question)
print(false_pred)
# As of December 31, 2020, the population of Nuremberg, Germany is approximately 516,000 people.
```

As we can see without the context the generation is incorrect. Now lets see if our evaluator can detect that as well. As reference we will use the raw number with `541,000`.

Python

```python
from langchain.evaluation import load_evaluator
from pprint import pprint as print
 
# create evaluator
evaluator = load_evaluator("context_qa", llm=evaluation_llm)
 
# evaluate
eval_result = evaluator.evaluate_strings(
  input=question,
  prediction=pred,
  context=context,
  reference="541,000"
)
 
# print result
print(eval_result)
# {'reasoning': 'CORRECT', 'score': 1, 'value': 'CORRECT'}
```

Nice! It worked as expected. The LLM evaluated the generation as correct. Lets now test what happens if we provide a wrong prediction.

Python

```python
# evaluate
eval_result = evaluator.evaluate_strings(
  input=question,
  prediction=false_pred,
  context=context,
  reference="541,000"
)
 
# print result
print(eval_result)
# {'reasoning': 'INCORRECT', 'score': 0, 'value': 'INCORRECT'}
```

Awesome! The evaluator detected that the generation is incorrect.

Alternatively, if you are not having a reference you can reuse the `criteria` evaluator to evaluate the correctness using the "question" as input and the "context" as reference.

Python

```python
from langchain.evaluation import load_evaluator
from pprint import pprint as print
 
# create evaluator
evaluator = load_evaluator("labeled_criteria", criteria="correctness", llm=evaluation_llm, requires_reference=True)
 
# evaluate
eval_result = evaluator.evaluate_strings(
    prediction=pred,
    input=question,
    reference=context,
)
 
# print result
print(eval_result)
 
```

```plaintext
{'reasoning': 'The criteria is to assess the correctness, accuracy, and '
              'factual nature of the submission.\\n'
              '\\n'
              'The submission states that according to the text, Nuremberg has '
              'a population of 541,000 inhabitants.\\n'
              '\\n'
              'Looking at the reference, it indeed confirms that Nuremberg is '
              'the 14th-largest city in Germany with 541,000 inhabitants.\\n'
              '\\n'
              'Thus, the submission is correct according to the reference '
              'text. It accurately cites the number of inhabitants in '
              'Nuremberg. It is factual as it is based on the given '
              'reference.\\n'
              '\\n'
              'Therefore, the submission meets the criteria.\\n'
              '\\n'
              'Y',
 'score': 1,
 'value': 'Y'}
 
```

As we can see GPT-4 correctly reasoned that the generation is correct based on the provided context.

## Pairwise comparison and scoring

Pairwise comparison or Scoring is a method for evaluating LLMs that asks the model to choose between two generations or generate scores for the quality. Those methods are useful for evaluating whether a model can generate a better response than another/previous model. This can also be used to generate preference data or AI Feedback for RLAIF or DPO.

Lets first look at the pairwise comparison. Here for we generate first two generations and then ask the LLM to choose between them.

Python

```python
prompt = "Write a short email to your boss about the meeting tomorrow."
pred_a = generate(prompt)
 
prompt = "Write a short email to your boss about the meeting tomorrow" # remove the period to not use cached results
pred_b = generate(prompt)
 
assert pred_a != pred_b
```

Now, lets use our LLM to select its preferred generation.

Python

```python
from langchain.evaluation import load_evaluator
from pprint import pprint as print
 
# create evaluator
evaluator = load_evaluator("pairwise_string", llm=evaluation_llm)
 
# evaluate
eval_result = evaluator.evaluate_string_pairs(
    prediction=pred_a,
    prediction_b=pred_b,
    input=prompt,
)
 
# print result
print(eval_result)
```

```plaintext
{'reasoning': 'Both Assistant A and Assistant B provided appropriate and '
              "relevant responses to the user's request. Both emails are "
              'professional, polite, and adhere to the typical format of a '
              "business email. However, Assistant B's response is more "
              'detailed, as it includes specifics about what will be discussed '
              'during the meeting (Smith project and sales numbers). This '
              "added level of detail makes Assistant B's response more helpful "
              'and demonstrates a greater depth of thought. Therefore, '
              "Assistant B's response is superior. \\n"
              '\\n'
              'Final verdict: [[B]]',
 'score': 0,
 'value': 'B'}
 
```

The LLM selected the second generation as the preferred one, we could now use this information to generate AI Feedback for RLAIF or DPO. As next we want to look a bit more in detail into our two generation and how they would be scored. Scoring can help us to more qualitative evaluate our generations.

Python

```python
from langchain.evaluation import load_evaluator
from pprint import pprint as print
 
# create evaluator
evaluator = load_evaluator("score_string", llm=evaluation_llm)
 
# evaluate
eval_result_a = evaluator.evaluate_strings(
    prediction=pred_a,
    input=prompt,
)
eval_result_b = evaluator.evaluate_strings(
    prediction=pred_b,
    input=prompt,
)
 
# print result
print(f"Score A: {eval_result_a['score']}")
print(f"Score B: {eval_result_b['score']}")
# 'Score A: 9'
# 'Score B: 9'
```

## Conclusion

In this post, we looked at practical methods for evaluating large language models using Langchain. Criteria-based evaluation allows us to check models against attributes like conciseness and correctness. We evaluated RAG pipelines whether models properly utilize provided context and looked at pairwise comparison and scoring to generate preference judgments between model outputs.

As large language models continue to advance, evaluation remains crucial for tracking progress and mitigating risks. Using LLM-as-a-judge can provide a scalable method to approximate human judgments. However, biases like verbosity preference can impact the performance. Combining lightweight human evaluations with LLM-as-a-judge can provide both quality and scale.

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
