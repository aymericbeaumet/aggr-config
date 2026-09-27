---
title: 'Evaluating Open LLMs with MixEval: The Closest Benchmark to LMSYS Chatbot Arena'
link: https://www.philschmid.de/evaluate-llm-mixeval
source: philschmid-de
published: 2024-06-28T00:00:00Z
updated: 2024-06-28T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Evaluate open LLMs with MixEval, the closest benchmark to LMSYS Chatbot Arena for a fraction of the cost and time.
content: extracted
html: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.html
preview:
  file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.preview-c5c776096840.webp
  width: 256
  height: 128
  alt: 'Evaluating Open LLMs with MixEval: The Closest Benchmark to LMSYS Chatbot Arena'
  color: '#6d6151'
images:
- source: https://www.philschmid.de/static/blog/evaluate-llm-mixeval/thumbnail.jpg
  original:
    file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-6aa11baa43ca.jpg
    width: 1200
    height: 600
  color: '#020202'
- source: https://www.philschmid.de/static/blog/evaluate-llm-mixeval/mixeval.png
  original:
    file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-08b0c887aaca.png
    width: 3000
    height: 1800
  variants:
  - file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-edacefbb0be3.webp
    width: 320
    height: 192
  - file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-81aae9400bef.webp
    width: 640
    height: 384
  - file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-14925e57aa0a.webp
    width: 960
    height: 576
  - file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-b803514c4575.webp
    width: 1280
    height: 768
  - file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-b586d781b12c.webp
    width: 1600
    height: 960
  - file: 2024-06-28-evaluating-open-llms-with-mixeval-the-closest-benchmark-to.image-9181d7b5d8b8.webp
    width: 3000
    height: 1800
  color: '#fcfbfb'
---

June 28, 2024 6 minute read [View Code](https://github.com/philschmid/MixEval)

*Last updated: 2024-09-23.*

Traditional ground-truth-based benchmarks, while efficient and reproducible, often fail to capture the complexity and nuance of real-world applications to evaluate LLMs. LLM-as-judge benchmarks attempt to address this by using models to evaluate responses, but they can suffer from grading biases and limited query quantity. User-facing evaluations like [LMSYS Chatbot Arena](https://huggingface.co/spaces/lmsys/chatbot-arena-leaderboard) are currently one of the most trusted sources for assessing the quality of LLMs. While they provide reliable signals, they have significant drawbacks—they are costly, time-consuming, and not easily reproducible.

[MixEval](https://mixeval.github.io/) tries to bridge the gap between real-world user queries and efficient ground-truth-based benchmarks. It currently has a 0.96 model ranking correlation with Chatbot Arena and costs less than $1 dollar to run.

![mixeval](https://www.philschmid.de/static/blog/evaluate-llm-mixeval/mixeval.png)

## What is MixEval?

[MixEval and MixEval-Hard](https://mixeval.github.io/) combine existing benchmarks with real-world user queries from the web to close the gap between academic and real-world use. They match queries mined from the web with similar queries from existing benchmarks.

1. High Correlation: Achieves a 0.96 model ranking correlation with Chatbot Arena, ensuring accurate model evaluation.
2. Cost-Effective: Costs only around $0.6 to run using GPT-3.5 as a judge, which is about 6% of the time and cost of running MMLU.
3. Dynamic Evaluation: Utilizes a rapid and stable data update pipeline to mitigate contamination risk.
4. Comprehensive Query Distribution: Based on a large-scale web corpus, providing a less biased evaluation.
5. Fair Grading: Ground-truth-based nature ensures an unbiased evaluation process.

MixEval comes in two versions:

1. MixEval: The standard benchmark that balances comprehensiveness and efficiency.
2. MixEval-Hard: A more challenging version designed to offer more room for model improvement and enhance the benchmark's ability to distinguish strong models.

## Evaluate open LLM on MixEval

MixEval offers comes with a [public GitHub](https://github.com/Psycoy/MixEval/) repository to evaluate LLMs. However, this comes with some limitations. I created this [fork](https://github.com/philschmid/MixEval) to make the integration and use of MixEval easier during the training of new models. This Fork includes several improved features to make usage easier and more flexible. Including:

1. **Local Model Evaluation**: Support for evaluating local models during or post-training with `transformers`.
2. **Hugging Face Datasets Integration**: Eliminates the need for local files by integrating with Hugging Face Datasets.
3. **Accelerated Evaluation**: Utilizes Hugging Face TGI or vLLM to speed up evaluation
4. **Enhanced Output**: Improved markdown outputs and timing for training.
5. **Simplified Installation**: Fixed pip install for remote or CI integration.

To install this enhanced version of MixEval, use the following command:

Bash

```bash
pip install git+https://github.com/philschmid/MixEval --upgrade
```

For more detailed instructions on evaluating models not included by default, refer to the MixEval documentation.

*Note: Make sure you have a valid OpenAI Key, since GPT-3.5 will be used as “parser”.*

### Evaluate Local LLMs during or post training

A single command can evaluate local LLMs on MixEval (`mixeval_hard`).

```plaintext
MODEL_PARSER_API=$(echo $OPENAI_API_KEY) python -m mix_eval.evaluate \
    --data_path hf://zeitgeist-ai/mixeval \
    --model_path my/local/path \
    --output_dir results/agi-5 \
    --model_name local_chat \
    --benchmark mixeval_hard \
    --version 2024-08-11 \
    --batch_size 20 \
    --api_parallel_num 20
```

Let's try to better understand this:

- `data_path`: Location of the mixeval set we want to use, here hosted on [hugging face](https://huggingface.co/datasets/zeitgeist-ai/mixeval).
- `model_path`: Path to our trained model, this can also be a Hugging Face ID, e.g. `organization/model`
- `output_dir` : Where the results will be stored
- `model_name`: Here, you would normally specify the `mixeval` model as defined in the GitHub, but in the fork, we implemented an agnostic `local_chat` version. (if you want to customize the .generate args, you need to [register a new model](https://github.com/Psycoy/MixEval/?tab=readme-ov-file#registering-new-models).)
- `benchmark`: Either `mixeval` or `mixeval_hard`
- `version`: Version of MixEval, the team plans to update the prompts and tasks regularly
- `batch_size`: The batch size of the generator model
- `api_parallel_num`: Number of parallel requests sent to OpenAI.

You can find all possible args in the [code](https://github.com/philschmid/MixEval/blob/d0f5a31e0a769886274236b5604e2859adeed370/mix_eval/evaluate.py#L36).

After running the evaluation, MixEval provides a comprehensive breakdown of the model's performance across various metrics.

```plaintext
| Metric                      | Score   |
| --------------------------- | ------- |
| MBPP                        | 100.00% |
....
| overall score (final score) | 35.15%  |
```

### Accelerated evaluation LLMs using vLLM

When training new models or experimenting, you want to iterate quickly. Therefore, I added support for any “API” that implements the OpenAI API, e.g., [vLLM](https://docs.vllm.ai/en/v0.4.0/getting_started/installation.html) or [Hugging Face TGI](https://huggingface.co/docs/text-generation-inference/index). This allows us to evaluate LLMs quickly and without the need to register or implement custom code, including the evaluation of Hosted.

1. Start your LLM serving framework, e.g. vLLM

Python

```python
python -m vllm.entrypoints.openai.api_server --model alignment-handbook/zephyr-7b-dpo-full
```

1. Run MixEval in another terminal.

Python

```python
MODEL_PARSER_API=$(echo $OPENAI_API_KEY) API_URL=http://localhost:8000/v1 python -m mix_eval.evaluate \
    --data_path hf://zeitgeist-ai/mixeval \
    --model_name local_api \
    --model_path alignment-handbook/zephyr-7b-dpo-full \
    --benchmark mixeval_hard \
    --version 2024-08-11 \
    --batch_size 20 \
    --output_dir results \
    --api_parallel_num 20
```

In comparison to the local evaluation, we need to make some changes:

- `API_URL`: Domain/URL where our model is hosted
- `API_KEY`: Optional in case you need authorization
- `model_name` : similar to the “local\_chat” model we added a “local\_api” model
- `model_path`: the model id in your Hosted API, e.g., for vLLM, it is the Hugging Face model id

After running the evaluation, MixEval provides a comprehensive breakdown of the model's performance across various metrics.

```plaintext
| Metric                      | Score   |
| --------------------------- | ------- |
| MBPP                        | 100.00% |
...
| overall score (final score) | 34.85%  |
 
```

Evaluating Zephyr 7b on 1x H100 with vLLM took 398s or ~7 minutes. That is very quick and also allows evaluation during training. But as you might have noticed, the score is slightly different; instead of `35.15`, we achieved `34.85`.

## Conclusion

MixEval offers a powerful and easy way to evaluate open LLMs. The 0.96 correlation to the LMSYS Chatbot Arena provides reliable results at a fraction of the cost and time. Running MixEval costs just $0.6 using GPT-3.5 as a judge—only 6% of the time and expense of running MMLU.

The improved fork of MixEval allows you to evaluate models using vLLM or TGI in just 7 minutes on a single H100 GPU.

This combination of accuracy, speed, and cost-effectiveness makes MixEval a perfect benchmark to use when building new models.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
