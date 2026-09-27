---
title: Evaluate LLMs using Evaluation Harness and Hugging Face TGI/vLLM
link: https://www.philschmid.de/evaluate-llms-with-lm-eval-and-tgi-vllm
source: philschmid-de
published: 2024-09-19T00:00:00Z
updated: 2024-09-19T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Evaluate Llama 3.1 8B Instruct on IFEval and GSM8K benchmarks with Chain of Thought reasoning using Evaluation Harness and Hugging Face TGI/vLLM.
content: extracted
html: 2024-09-19-evaluate-llms-using-evaluation-harness-and-hugging-face-tgi.html
preview:
  file: 2024-09-19-evaluate-llms-using-evaluation-harness-and-hugging-face-tgi.preview-65576434780e.webp
  width: 256
  height: 151
  alt: Evaluate LLMs using Evaluation Harness and Hugging Face TGI/vLLM
  color: '#4b5146'
images:
- source: https://www.philschmid.de/static/blog/evaluate-llms-with-lm-eval-and-tgi-vllm/thumbnail.jpg
  original:
    file: 2024-09-19-evaluate-llms-using-evaluation-harness-and-hugging-face-tgi.image-ec81226402bd.jpg
    width: 1200
    height: 706
  color: '#040404'
---

As Large Language Models (LLMs) like OpenAI o1, Meta Llama, and Anthropic Claude continue to become more performance, it's crucial to validate their general performance on core capabilities such as instruction following, reasoning, and mathematical skills using benchmarks like IFEval and GSM8K. While these may not perfectly for downstream use case, they provide a valuable general picture of a model's strengths and weaknesses.

However, running these comprehensive evaluations can be time-consuming and computationally intensive, especially with larger models. This is where optimized LLM serving tools like Hugging Face's Text Generation Inference (TGI) and vLLM come into play. Additionally, this allows us to validate the accuracy and implementation of the models in a production-like environment.

In this blog post, we will learn how to evaluate LLMs hosten using TGI or vLLM behind OpenAI compatible API endpoints, those can be locally or remotely. We will use on the Evaluation Harness to evaluate the Llama 3.1 8B Instruct model on the IFEval and GSM8K benchmarks with Chain of Thought reasoning.

**Evaluation Harness**

[Evaluation Harness](https://github.com/EleutherAI/lm-evaluation-harness) is a open-source framework to evaluate language models on a wide range of tasks and benchmarks. It supports various models and provides tools to streamline the evaluation process. It is used for the [Hugging Face Open LLM Leaderboard](https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard)

**Hugging Face Text Generation Inference (TGI)**

[Text Generation Inference](https://huggingface.co/docs/text-generation-inference/index) is a scalelable, optimized solution for deploying and serving Large Language Models (LLMs). TGI supports popular open-source models like Llama, Mistral, and Gemma.

## Evaluate Llama 3.1 8B Instruct on IFEval & GSM8K

Now, let's get started evaluating the **Llama 3.1 8B Instruct** model on **IFEval** and **GSM8K** benchmarks using Chain of Thought reasoning.

*Note: This tutorial was run on an AWS g6e.2xlarge instance with 1x NVIDIA L40S GPU.*

### 1\. Running the Model with TGI

First, we'll use TGI to serve the Llama 3.1 8B Instruct model. Ensure you have Docker installed and a valid Hugging Face token.

**Run Command:**

Bash

```bash
docker run --gpus all -ti --shm-size 1g --ipc=host --rm -p 8000:80 \
  -e MODEL_ID=meta-llama/Meta-Llama-3.1-8B-Instruct \
  -e NUM_SHARD=1 \
  -e MAX_INPUT_TOKENS=8000 \
  -e MAX_TOTAL_TOKENS=8192 \
  -e HF_TOKEN=$(cat ~/.cache/huggingface/token) \
  ghcr.io/huggingface/text-generation-inference:2.2.0
```

- **MODEL\_ID**: Specifies the model to use.
- **NUM\_SHARD**: Number of shards (GPUs) to use.
- **MAX\_INPUT\_TOKENS**: Maximum number of tokens in the input, you might need to adjust based on the GPU you use
- **MAX\_TOTAL\_TOKENS**: Maximum number of tokens in the input and output, you might need to adjust based on the GPU you use
- **HF\_TOKEN**: Your Hugging Face access token, you need to first run `huggingface-cli login`

> **Note**: Alternatively, you can use vLLM's OpenAI-compatible API server to serve the model. vLLM is another efficient serving library that supports high-throughput inference.

### 2\. Evaluate LLM with `lm_eval` through OpenAI API Endpoints

Evaluation Harness provides a CLI tool to evaluate models on various tasks and benchmarks. We can run 1 or more tasks in parallel and evaluate the model's performance using a `,` separated list of tasks. `lm-eval` and supports many different configurations and options to evaluate models. A full list can be found in the [documentation](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/interface.md).

Most important parameters are `model`, `tasks` and `model_args` those used to tell the harness which [model (strategy)](https://github.com/EleutherAI/lm-evaluation-harness/tree/main?tab=readme-ov-file#model-apis-and-inference-servers) with what [arguments to use and on which tasks](https://github.com/EleutherAI/lm-evaluation-harness/blob/main/docs/API_guide.md#templateapi-arguments) to evaluate. We are using LLMs hosted through a OpenAI compatbile API, so we will use `local-chat-completions` model interface, that allows us to evaluate models using the OpenAI `messages` API format. This comes with benefits as that we can easily switch between models or run the evaluation on a different host, but this also comes with requirements as the `local-chat-completions` is not supporting `loglikelihood` which is needed for some tasks. Important CLI flags for this are:

- `--model`: Specifies the model interface. We'll use `local-chat-completions`.
- `--tasks`: Comma-separated list of tasks to evaluate (e.g., `gsm8k_cot_llama,ifeval`).
- `--model_args`: Additional model arguments like `model`, `base_url`, and `num_concurrent`.
  - **model**: The model identifier, for tokenizer and other model-specific configurations.
  - **base\_url**: The API endpoint where the model is served, `http://localhost:8000/v1/chat/completions`
  - **num\_concurrent**: Number of concurrent requests, e.g. `32`.
  - **max\_retries**: Number of retries for failed requests, e.g. `3`.
  - **tokenized\_requests**: Set to `False` for chat models.
- `--apply_chat_template`: Applies the chat template for formatting prompts.
- `--fewshot_as_multiturn`: Treats few-shot examples as multiple turns (useful for instruct models).

As mentioned in the beginning, we are evaluating the Llama 3.1 8B Instruct model on IFEval and GSM8K benchmarks with Chain of Thought reasoning.

Python

```python
# install lm_eval and openai
!pip install "lm_eval[ifeval]==0.4.4"
```

After we installed the CLI we can evaluate the model with the following command:

Python

```python
!lm_eval --model local-chat-completions \
  --tasks gsm8k_cot_llama,ifeval \
  --model_args model=meta-llama/Meta-Llama-3.1-8B-Instruct,base_url=http://localhost:8000/v1/chat/completions,num_concurrent=32,max_retries=3,tokenized_requests=False \
  --apply_chat_template \
  --fewshot_as_multiturn
```

Running the evaluation on IFEval and GSM8K with Chain of Thought reasoning took ~10 min on a AWS g6e.2xlarge instance with 1x NVIDIA L40S GPU.

### 3\. Comparing Results

After running the evaluation, we can compare the results with what [Meta officially reported](https://ai.meta.com/blog/meta-llama-3-1/).

| Task   | Meta Reported | Our Result |
| ------ | ------------- | ---------- |
| IFEval | 0.804         | 0.803      |
| GSM8K  | 0.845         | 0.856      |

The results are consistent with Meta's official report, indicating that the model and serving solution perform as expected.

## Conclusion

We learned how to efficiently evaluate LLMs on benchmarks, like IFEval or GMS8k using OpenAI-compatible endpoints provided by TGI and vLLM, that can run locally or on a remote cloud environment. We confirmed the offical reported results for the Llama 3.1 8B Instruct model on IFEval and GSM8K benchmarks with Chain of Thought reasoning. Allowing us to validate the model's performance and implementation in a production runtime.

Leveraging Evaluation Harness and a optimized serving solution like TGI or vLLM, we can streamline the evaluation process and get accurate results quickly. This helps us iterate faster and validate production performance of LLMs.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
