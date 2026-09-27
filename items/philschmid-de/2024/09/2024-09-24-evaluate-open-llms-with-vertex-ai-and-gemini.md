---
title: Evaluate open LLMs with Vertex AI and Gemini
link: https://www.philschmid.de/evaluate-llm-with-gemini
source: philschmid-de
published: 2024-09-24T00:00:00Z
updated: 2024-09-24T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Evaluate Llama 3.1 8B on Vertex AI with Gemini 1.5 Pro as LLM as a Judge using the Gen AI Evaluation Service.
content: extracted
html: 2024-09-24-evaluate-open-llms-with-vertex-ai-and-gemini.html
preview:
  file: 2024-09-24-evaluate-open-llms-with-vertex-ai-and-gemini.preview-fc1a79302e79.webp
  width: 256
  height: 165
  alt: Evaluate open LLMs with Vertex AI and Gemini
  color: '#556262'
images:
- source: https://www.philschmid.de/static/blog/evaluate-llm-with-gemini/thumbnail.jpg
  original:
    file: 2024-09-24-evaluate-open-llms-with-vertex-ai-and-gemini.image-5da20995ea3c.jpg
    width: 1024
    height: 660
  color: '#020202'
- source: https://www.philschmid.de/static/blog/evaluate-llm-with-gemini/experiment-results.png
  original:
    file: 2024-09-24-evaluate-open-llms-with-vertex-ai-and-gemini.image-f9097044cba3.png
    width: 1387
    height: 245
  color: '#fbfbfb'
---

The [Gen AI Evaluation Service in Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/models/evaluation-overview) lets us evaluate LLMs or Application using existing or your own evaluation criterias. It supports academic metrics like BLEU, ROUGE, or LLM as a Judge with Pointwise and Pairwise metrics or custom metrics you can define yourself. As default LLM as a Judge `Gemini 1.5 Pro` is used.

We can use the Gen AI Evaluation Service to evaluate the performance of open models and finetuned models using Vertex AI Endpoints and compute resources. In this example we will evaluate [meta-llama/Meta-Llama-3.1-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct) generated summaries from news articles using a Pointwise metric based on [G-Eval](https://arxiv.org/abs/2303.16634) Coherence metric.

We will cover the following topics:

1. Setup / Configuration
2. Deploy Llama 3.1 8B on Vertex AI
3. Evaluate Llama 3.1 8B using different prompts on Coherence
4. Interpret the results
5. Clean up resources

## Setup / Configuration

First, you need to install `gcloud` in your local machine, which is the command-line tool for Google Cloud, following the instructions at [Cloud SDK Documentation - Install the gcloud CLI](https://cloud.google.com/sdk/docs/install).

Then, you also need to install the `google-cloud-aiplatform` Python SDK, required to programmatically create the Vertex AI model, register it, acreate the endpoint, and deploy it on Vertex AI.

Python

```python
!pip install --upgrade --quiet "google-cloud-aiplatform[evaluation]"  huggingface_hub transformers datasets
```

For ease of use we define the following environment variables for GCP.

*Note 1: Make sure to adapt the project ID to your GCP project.*\
 *Note 2: The Gen AI Evaluation Service is not available in all regions. If you want to use it, you need to select a region that supports it. `us-central1` is currently supported.*

Python

```python
%env PROJECT_ID=gcp-partnership-412108
%env LOCATION=us-central1
%env CONTAINER_URI=us-docker.pkg.dev/deeplearning-platform-release/gcr.io/huggingface-text-generation-inference-cu121.2-2.ubuntu2204.py310 
```

Then you need to login into your GCP account and set the project ID to the one you want to use to register and deploy the models on Vertex AI.

Python

```python
!gcloud auth login
!gcloud auth application-default login  # For local development
!gcloud config set project $PROJECT_ID
```

Once you are logged in, you need to enable the necessary service APIs in GCP, such as the Vertex AI API, the Compute Engine API, and Google Container Registry related APIs.

Python

```python
!gcloud services enable aiplatform.googleapis.com
!gcloud services enable compute.googleapis.com
!gcloud services enable container.googleapis.com
!gcloud services enable containerregistry.googleapis.com
!gcloud services enable containerfilesystem.googleapis.com
```

## Deploy Llama 3.1 8B on Vertex AI

Once everything is set up, we can deploy the Llama 3.1 8B model on Vertex AI. We will use the `google-cloud-aiplatform` Python SDK to do so. [meta-llama/Meta-Llama-3.1-8B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3.1-8B-Instruct) is a gated model, you need to login into your Hugging Face Hub account with a read-access token either fine-grained with access to the gated model, or just overall read-access to your account. More information on how to generate a read-only access token for the Hugging Face Hub in the instructions at [Hugging Face Hub Security Tokens](https://huggingface.co/docs/hub/en/security-tokens).

Python

```python
from huggingface_hub import interpreter_login
 
interpreter_login()
```

After we are logged in we can "upload" the model i.e. register the model on Vertex AI. If you want to learn more about the arguments you can pass to the `upload` method, check out [Deploy Gemma 7B with TGI on Vertex AI](https://github.com/huggingface/Google-Cloud-Containers/blob/main/examples/vertex-ai/notebooks/deploy-gemma-on-vertex-ai/vertex-notebook.ipynb).

Python

```python
import os
from google.cloud import aiplatform
 
aiplatform.init(
    project=os.getenv("PROJECT_ID"),
    location=os.getenv("LOCATION"),
)
```

We will deploy the `meta-llama/Meta-Llama-3.1-8B-Instruct` to 1x NVIDIA L4 accelerator with 24GB memory. We set TGI parameters to allow for a maximum of 8000 input tokens, 8192 maximum total tokens, and 8192 maximum batch prefill tokens.

Python

```python
from huggingface_hub import get_token
 
vertex_model_name = "llama-3-1-8b-instruct"
 
model = aiplatform.Model.upload(
    display_name=vertex_model_name,
    serving_container_image_uri=os.getenv("CONTAINER_URI"),
    serving_container_environment_variables={
        "MODEL_ID": "meta-llama/Meta-Llama-3.1-8B-Instruct",
        "MAX_INPUT_TOKENS": "8000",
        "MAX_TOTAL_TOKENS": "8192",
        "MAX_BATCH_PREFILL_TOKENS": "8192",
        "HUGGING_FACE_HUB_TOKEN": get_token(),
    },
    serving_container_ports=[8080],
)
model.wait() # wait for the model to be registered
 
# create endpoint
endpoint = aiplatform.Endpoint.create(display_name=f"{vertex_model_name}-endpoint")
 
# deploy model to 1x NVIDIA L4
deployed_model = model.deploy(
    endpoint=endpoint,
    machine_type="g2-standard-4",
    accelerator_type="NVIDIA_L4",
    accelerator_count=1,
)
```

**WARNING**: *The Vertex AI endpoint deployment via the `deploy` method may take from 15 to 25 minutes.*

After the model is deployed, we can test our endpoint. We generate a helper `generate` function to send requests to the deployed model. This will be later used to send requests to the deployed model and collect the outputs for evaluation.

Python

```python
import re
from transformers import AutoTokenizer
 
# grep the model id from the container spec environment variables
model_id = next((re.search(r'value: "(.+)"', str(item)).group(1) for item in list(model.container_spec.env) if 'MODEL_ID' in str(item)), None)
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Meta-Llama-3.1-8B-Instruct")
 
generation_config = {
  "max_new_tokens": 256,
  "do_sample": True,
  "top_p": 0.2,
  "temperature": 0.2,
}
 
def generate(prompt, generation_config=generation_config):
  formatted_prompt = tokenizer.apply_chat_template(
        [
          {"role": "user", "content": prompt},
        ],
        tokenize=False,
        add_generation_prompt=True,
      )
  
  payload = {
    "inputs": formatted_prompt,
    "parameters": generation_config
  }
  output = deployed_model.predict(instances=[payload])
  generated_text = output.predictions[0]
  return generated_text
 
 
generate("How many people live in Berlin?", generation_config)
# 'The population of Berlin is approximately 6.578 million as of my cut off data. However, considering it provides real-time updates, the current population might be slightly higher'
```

## Evaluate Llama 3.1 8B using different prompts on Coherence

We will evaluate the Llama 3.1 8B model using different prompts on Coherence. Coherence measures how well the individual sentences within a summarized news article connect together to form a unified and easily understandable narrative.

We are going to use the new [Generative AI Evaluation Service](https://cloud.google.com/vertex-ai/generative-ai/docs/models/evaluation-overview). The Gen AI Evaluation Service can be used to:

- Model selection: Choose the best pre-trained model for your task based on benchmark results and its performance on your specific data.
- Generation settings: Tweak model parameters (like temperature) to optimize output for your needs.
- Prompt engineering: Craft effective prompts and prompt templates to guide the model towards your preferred behavior and responses.
- Improve and safeguard fine-tuning: Fine-tune a model to improve performance for your use case, while avoiding biases or undesirable behaviors.
- RAG optimization: Select the most effective Retrieval Augmented Generation (RAG) architecture to enhance performance for your application.
- Migration: Continuously assess and improve the performance of your AI solution by migrating to newer models when they provide a clear advantage for your specific use case.

In our case, we will use it to evaluate different prompt templates to achieve the most coherent summaries using Llama 3.1 8B Instruct.

We are going to use a reference free Pointwise metric based on [G-Eval](https://arxiv.org/abs/2303.16634) Coherence metric.

The first step is to define our prompt template and create our `PointwiseMetric`. Vertex AI returns our response from the model in the `response` field our news article will be made available in the `text` field.

Python

```python
from vertexai.evaluation import EvalTask, PointwiseMetric
 
g_eval_coherence = """
You are an expert evaluator. You will be given one summary written for a news article.
Your task is to rate the summary on one metric.
Please make sure you read and understand these instructions carefully. Please keep this document open while reviewing, and refer to it as needed.
 
Evaluation Criteria:
 
Coherence (1-5) - the collective quality of all sentences. We align this dimension with the DUC quality question of structure and coherence whereby "the summary should be well-structured and well-organized. The summary should not just be a heap of related information, but should build from sentence to a coherent body of information about a topic."
 
Evaluation Steps:
 
1. Read the news article carefully and identify the main topic and key points.
2. Read the summary and compare it to the news article. Check if the summary covers the main topic and key points of the news article, and if it presents them in a clear and logical order.
3. Assign a score for coherence on a scale of 1 to 5, where 1 is the lowest and 5 is the highest based on the Evaluation Criteria.
 
 
Example:
 
 
Source Text:
 
{text}
 
Summary:
 
{response}
 
Evaluation Form (scores ONLY):
 
- Coherence:"""
 
metric = PointwiseMetric(
    metric="g-eval-coherence",
    metric_prompt_template=g_eval_coherence,
)
```

We are going to use [argilla/news-summary](https://huggingface.co/datasets/argilla/news-summary) dataset consisting of news article from Reuters. We are going to use a random subset of 15 articles to keep the evaluation fast. Feel free to change the dataset and the number of articles to evaluate the model with more data and different topics.

Python

```python
from datasets import load_dataset
 
subset_size = 15
dataset = load_dataset("argilla/news-summary", split=f"train").shuffle(seed=42).select(range(subset_size))
 
# print first 150 characters of the first article
print(dataset[0]["text"][:150])
 
```

Before we can run the evaluation, we need to convert our dataset into a pandas dataframe.

Python

```python
# remove all columns except for "text"
to_remove = [col for col in dataset.features.keys() if col != "text"]
dataset = dataset.remove_columns(to_remove)
df = dataset.to_pandas()
df.head()
 
```

Awesome! We are almost ready. Last step is to define our different summarization prompts we want to use for evaluation.

Python

```python
summarization_prompts = {
  "simple": "Summarize the following news article: {text}",
  "eli5": "Summarize the following news article in a way a 5 year old would understand: {text}",
  "detailed": """Summarize the given news article, text, including all key points and supporting details? The summary should be comprehensive and accurately reflect the main message and arguments presented in the original text, while also being concise and easy to understand. To ensure accuracy, please read the text carefully and pay attention to any nuances or complexities in the language.
  
Article:
{text}"""
}
```

Now we can iterate over our prompts and create different evaluation tasks, use our coherence metric to evaluate the summaries and collect the results.

Python

```python
import uuid
 
 
results = {}
for prompt_name, prompt in summarization_prompts.items():
    prompt = summarization_prompts[prompt_name]
 
    # 1. add new prompt column
    df["prompt"] = df["text"].apply(lambda x: prompt.format(text=x))
 
    # 2. create eval task
    eval_task = EvalTask(
        dataset=df,
        metrics=[metric],
        experiment="llama-3-1-8b-instruct",
    )
    # 3. run eval task
    # Note: If the last iteration takes > 1 minute you might need to retry the evaluation
    exp_results = eval_task.evaluate(model=generate, experiment_run_name=f"prompt-{prompt_name}-{str(uuid.uuid4())[:8]}")
    print(f"{prompt_name}: {exp_results.summary_metrics['g-eval-coherence/mean']}")
    results[prompt_name] = exp_results.summary_metrics["g-eval-coherence/mean"]
 
for prompt_name, score in sorted(results.items(), key=lambda x: x[1], reverse=True):
    print(f"{prompt_name}: {score}")
```

Nice, it looks like on our limited test the "simple" prompt yields the best results. We can inspect and compare the results in the GCP Console at [Vertex AI > Model Development > Experiments](https://console.cloud.google.com/vertex-ai/experiments).

![experiment-results](https://www.philschmid.de/static/blog/evaluate-llm-with-gemini/experiment-results.png)

The overview allows to compare the results across different experiments and to inspect the individual evaluations. Here we can see that the standard deviation of detailed is quite high. This could be because of the low sample size or that we need to improve the prompt further.

You can find more examples on how to use the Gen AI Evaluation Service in the [Vertex AI Generative AI documentation](https://cloud.google.com/vertex-ai/generative-ai/docs/models/evaluation-overview) including how to:

- [how to customize the LLM as a Judge](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/bring_your_own_autorater_with_custom_metric.ipynb)
- [how to use Pairwise metrics and compare different LLMs](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/compare_generative_ai_models.ipynb)
- [how to evaluate different prompts more efficiently](https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/evaluation/prompt_engineering_gen_ai_evaluation_service_sdk.ipynb)

## Resource clean-up

Finally, you can already release the resources that you've created as follows, to avoid unnecessary costs:

- `deployed_model.undeploy_all` to undeploy the model from all the endpoints.
- `deployed_model.delete` to delete the endpoint/s where the model was deployed gracefully, after the `undeploy_all` method.
- `model.delete` to delete the model from the registry.

Python

```python
deployed_model.undeploy_all()
deployed_model.delete()
model.delete()
```
