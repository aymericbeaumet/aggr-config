---
title: Deploy Llama 3 on Amazon SageMaker
link: https://www.philschmid.de/sagemaker-llama3
source: philschmid-de
published: 2024-04-18T00:00:00Z
updated: 2024-04-18T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to deploy Llama 3 70B to Amazon SageMaker.
content: extracted
html: 2024-04-18-deploy-llama-3-on-amazon-sagemaker.html
preview:
  file: 2024-04-18-deploy-llama-3-on-amazon-sagemaker.preview-89fda2f06cb3.webp
  width: 256
  height: 128
  alt: Deploy Llama 3 on Amazon SageMaker
  color: '#a89491'
images:
- source: https://www.philschmid.de/static/blog/sagemaker-llama3/thumbnail.jpg
  original:
    file: 2024-04-18-deploy-llama-3-on-amazon-sagemaker.image-07ccbb30e4dc.jpg
    width: 1200
    height: 600
  color: '#fdd7b5'
---

Earlier today Meta released [Llama 3](https://huggingface.co/blog/llama3), the next iteration of the open-access Llama family. Llama 3 comes in two sizes: [8B](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct) for efficient deployment and development on consumer-size GPU, and [70B](https://huggingface.co/meta-llama/Meta-Llama-3-70B-instruct) for large-scale AI native applications. Both come in base and instruction-tuned variants. In addition to the 4 models, a new version of Llama Guard was fine-tuned on Llama 3 8B and is released as Llama Guard 2 (safety fine-tune).

In this blog you will learn how to deploy [meta-llama/Meta-Llama-3-70B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct) model to Amazon SageMaker. We are going to use the Hugging Face LLM DLC is a purpose-built Inference Container to easily deploy LLMs in a secure and managed environment. The DLC is powered by [Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) a scalelable, optimized solution for deploying and serving Large Language Models (LLMs). The Blog post also includes Hardware requirements for the different model sizes.

In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/sagemaker-llama3#1-setup-development-environment)
2. [Hardware requirements](https://www.philschmid.de/sagemaker-llama3#2-hardware-requirements)
3. [Deploy Llama 3 70b to Amazon SageMaker](https://www.philschmid.de/sagemaker-llama3#3-deploy-llama-3-to-amazon-sagemaker)
4. [Run inference and chat with the model](https://www.philschmid.de/sagemaker-llama3#4-run-inference-and-chat-with-the-model)
5. [Benchmark llama 3 70B with llmperf](https://www.philschmid.de/sagemaker-llama3#5-benchmark-llama-3-70b)
6. [Clean up](https://www.philschmid.de/sagemaker-llama3#6-clean-up)

Lets get started!

## 1\. Setup development environment

We are going to use the `sagemaker` python SDK to deploy Mixtral to Amazon SageMaker. We need to make sure to have an AWS account configured and the `sagemaker` python SDK installed.

Python

```python
!pip install "sagemaker>=2.216.0" --upgrade --quiet
```

If you are going to use Sagemaker in a local environment. You need access to an IAM Role with the required permissions for Sagemaker. You can find [here](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) more about it.

Python

```python
import sagemaker
import boto3
sess = sagemaker.Session()
# sagemaker session bucket -> used for uploading data, models and logs
# sagemaker will automatically create this bucket if it not exists
sagemaker_session_bucket=None
if sagemaker_session_bucket is None and sess is not None:
    # set to default bucket if a bucket name is not given
    sagemaker_session_bucket = sess.default_bucket()
 
try:
    role = sagemaker.get_execution_role()
except ValueError:
    iam = boto3.client('iam')
    role = iam.get_role(RoleName='sagemaker_execution_role')['Role']['Arn']
 
sess = sagemaker.Session(default_bucket=sagemaker_session_bucket)
 
print(f"sagemaker role arn: {role}")
print(f"sagemaker session region: {sess.boto_region_name}")
 
```

Compared to deploying regular Hugging Face models we first need to retrieve the container uri and provide it to our `HuggingFaceModel` model class with a `image_uri` pointing to the image. To retrieve the new Hugging Face LLM DLC in Amazon SageMaker, we can use the `get_huggingface_llm_image_uri` method provided by the `sagemaker` SDK. This method allows us to retrieve the URI for the desired Hugging Face LLM DLC based on the specified `backend`, `session`, `region`, and `version`. You can find the available versions [here](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#huggingface-text-generation-inference-containers)

*Note: At the time of writing this blog post the latest version of the Hugging Face LLM DLC is not yet available via the `get_huggingface_llm_image_uri` method. We are going to use the raw container uri instead.*

Python

```python
# COMMENT IN WHEN PR (https://github.com/aws/sagemaker-python-sdk/pull/4314) IS MERGED
# from sagemaker.huggingface import get_huggingface_llm_image_uri
 
# # retrieve the llm image uri
# llm_image = get_huggingface_llm_image_uri(
#   "huggingface",
#   version="2.0.0"
# )
llm_image = f"763104351884.dkr.ecr.{sess.boto_region_name}.amazonaws.com/huggingface-pytorch-tgi-inference:2.1-tgi2.0-gpu-py310-cu121-ubuntu22.04"
 
# print ecr image uri
print(f"llm image uri: {llm_image}")
```

llm image uri: 763104351884.dkr.ecr.us-east-1.amazonaws.com/huggingface-pytorch-tgi-inference:2.1-tgi2.0-gpu-py310-cu121-ubuntu22.04

## 2\. Hardware requirements

Llama 3 comes in 2 different sizes - 8B & 70B parameters. The hardware requirements will vary based on the model size deployed to SageMaker. Below is a set up minimum requirements for each model size we tested.

| Model                                                                     | Instance Type       | Quantization | \# of GPUs per replica |
| ------------------------------------------------------------------------- | ------------------- | ------------ | ---------------------- |
| [Llama 8B](https://huggingface.co/meta-llama/Meta-Llama-3-8B-Instruct)   | `(ml.)g5.2xlarge`   | `-`          | 1                      |
| [Llama 70B](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct) | `(ml.)g5.12xlarge`  | `gptq / awq` | 8                      |
| [Llama 70B](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct) | `(ml.)g5.48xlarge`  | `-`          | 8                      |
| [Llama 70B](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct) | `(ml.)p4d.24xlarge` | `-`          | 8                      |

*Note: We haven't tested GPTQ or AWQ models yet.*

## 3\. Deploy Llama 3 to Amazon SageMaker

To deploy [Llama 3 70B](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct) to Amazon SageMaker we create a `HuggingFaceModel` model class and define our endpoint configuration including the `hf_model_id`, `instance_type` etc. We will use a `p4d.24xlarge` instance type, which has 8 NVIDIA A100 GPUs and 320GB of GPU memory. Llama 3 70B instruct is a fine-tuned model for conversational AI this allows us to enable the [Messages API](https://huggingface.co/docs/text-generation-inference/messages_api) from TGI to interact with llama using the common OpenAI format of `messages`.

JSON

```json
{
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "What is deep learning?" }
  ]
}
```

*Note: Llama 3 is a gated model, please visit the [Model Card](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct) and accept the license terms and acceptable use policy before submitting this form.*

Python

```python
import json
from sagemaker.huggingface import HuggingFaceModel
 
# sagemaker config
instance_type = "ml.p4d.24xlarge"
health_check_timeout = 900
 
# Define Model and Endpoint configuration parameter
config = {
  'HF_MODEL_ID': "meta-llama/Meta-Llama-3-70B-Instruct", # model_id from hf.co/models
  'SM_NUM_GPUS': "8", # Number of GPU used per replica
  'MAX_INPUT_LENGTH': "2048",  # Max length of input text
  'MAX_TOTAL_TOKENS': "4096",  # Max length of the generation (including input text)
  'MAX_BATCH_TOTAL_TOKENS': "8192",  # Limits the number of tokens that can be processed in parallel during the generation
  'MESSAGES_API_ENABLED': "true", # Enable the messages API
  'HUGGING_FACE_HUB_TOKEN': "<REPLACE WITH YOUR TOKEN>"
}
 
# check if token is set
assert config['HUGGING_FACE_HUB_TOKEN'] != "<REPLACE WITH YOUR TOKEN>", "Please set your Hugging Face Hub token"
 
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  image_uri=llm_image,
  env=config
)
 
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the `deploy` method. We will deploy the model with the `ml.p4d.24xlarge` instance type. TGI will automatically distribute and shard the model across all GPUs.

Python

```python
# Deploy model to an endpoint
# https://sagemaker.readthedocs.io/en/stable/api/inference/model.html#sagemaker.model.Model.deploy
llm = llm_model.deploy(
  initial_instance_count=1,
  instance_type=instance_type,
  container_startup_health_check_timeout=health_check_timeout, # 10 minutes to be able to load the model
)
 
```

SageMaker will now create our endpoint and deploy the model to it. This can takes a 10-15 minutes.

## 4\. Run inference and chat with the model

After our endpoint is deployed we can run inference on it. We will use the `predict` method from the `predictor` to run inference on our endpoint. We can inference with different parameters to impact the generation. Parameters can be defined as in the `parameters` attribute of the payload. You can find supported parameters in the [here](https://huggingface.co/docs/text-generation-inference/messages_api).

The Messages API allows us to interact with the model in a conversational way. We can define the role of the message and the content. The role can be either `system`,`assistant` or `user`. The `system` role is used to provide context to the model and the `user` role is used to ask questions or provide input to the model.

JSON

```json
{
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "What is deep learning?" }
  ]
}
```

Python

```python
# Prompt to generate
messages=[
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "What is deep learning?" }
  ]
 
# Generation arguments
parameters = {
    "model": "meta-llama/Meta-Llama-3-70B-Instruct", # placholder, needed
    "top_p": 0.6,
    "temperature": 0.9,
    "max_tokens": 512,
    "stop": ["<|eot_id|>"],
}
```

Okay lets test it.

Python

```python
chat = llm.predict({"messages" :messages, **parameters})
 
print(chat["choices"][0]["message"]["content"].strip())
```

## 5\. Benchmark llama 3 70B with llmperf

We successfully deployed Llama 3 70B to Amazon SageMaker and tested it. Now we want to benchmark the model to see how it performs. We will use a [llmperf](https://github.com/philschmid/llmperf) fork with support for `sagemaker`.

First lets install the `llmperf` package.

Python

```python
!git clone https://github.com/philschmid/llmperf.git
!pip install -e llmperf/
```

Now we can run the benchmark with the following command. We are going to benchmark using `25` concurrent users and max `500` requests. The benchmark will measure `first-time-to-token`, `latency (ms/token)` and `throughput (tokens/s)` full details can be found in the `results` folder

*🚨Important🚨: This benchmark was initiatied from Europe, while the endpoint runs in us-east-1. This has significant impact on the `first-time-to-token` metric, since it includes the network communication. If you want to measure the `first-time-to-token` correctly, you need to run the benchmark on the same host or your production region.*

Python

```python
# tell llmperf that we are using the messages api
!MESSAGES_API=true python llmperf/token_benchmark_ray.py \
--model {llm.endpoint_name} \
--llm-api "sagemaker" \
--max-num-completed-requests 500 \
--timeout 600 \
--num-concurrent-requests 25 \
--results-dir "results"
```

Lets parse the results and display them nicely.

Python

```python
import glob
import json
 
# Reads the summary.json file and prints the results
with open(glob.glob(f'results/*summary.json')[0], 'r') as file:
    data = json.load(file)
 
print("Concurrent requests: 25")
print(f"Avg. Input token length: {data['mean_input_tokens']}")
print(f"Avg. Output token length: {data['mean_output_tokens']}")
print(f"Avg. First-Time-To-Token: {data['results_ttft_s_mean']*1000:.2f}ms")
print(f"Avg. Thorughput: {data['results_mean_output_throughput_token_per_s']:.2f} tokens/sec")
print(f"Avg. Latency: {data['results_inter_token_latency_s_mean']*1000:.2f}ms/token")
#    Concurrent requests: 25
#    Avg. Input token length: 550
#    Avg. Output token length: 150
#    Avg. First-Time-To-Token: 1301.28ms
#    Avg. Thorughput: 1116.25 tokens/sec
#    Avg. Latency: 9.45ms/token
```

Thats it! We successfully deployed, tested and benchmarked Llama 3 70B on Amazon SageMaker. The benchmark is not a full representation of the model performance, but it gives you a first good indication. If you plan to use the model in production, we recommend to run a longer and closer to your production benchmark, modify the number of replicas see ([Scale LLM Inference on Amazon SageMaker with Multi-Replica Endpoints](https://www.philschmid.de/sagemaker-multi-replicahttps://www.philschmid.de/sagemaker-multi-replica)) and most importantly test the model with your own data.

## 6\. Clean up

To clean up, we can delete the model and endpoint.

Python

```python
llm.delete_model()
llm.delete_endpoint()
```

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
