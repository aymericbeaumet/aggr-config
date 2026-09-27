---
title: Deploy Mixtral 8x7B on AWS Inferentia2 with Hugging Face Optimum
link: https://www.philschmid.de/inferentia2-mixtral-8x7b
source: philschmid-de
published: 2024-06-18T00:00:00Z
updated: 2024-06-18T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Deploy Mixtral 8x7B to AWS Inferentia2 with Hugging Face Optimum on Amazon SageMaker and benchmark it with llmperf.
content: extracted
html: 2024-06-18-deploy-mixtral-8x7b-on-aws-inferentia2-with-hugging-face.html
preview:
  file: 2024-06-18-deploy-mixtral-8x7b-on-aws-inferentia2-with-hugging-face.preview-5197c5133905.webp
  width: 256
  height: 128
  alt: Deploy Mixtral 8x7B on AWS Inferentia2 with Hugging Face Optimum
  color: '#736656'
images:
- source: https://www.philschmid.de/static/blog/inferentia2-mixtral-8x7b/thumbnail.jpg
  original:
    file: 2024-06-18-deploy-mixtral-8x7b-on-aws-inferentia2-with-hugging-face.image-21dbf096d79f.jpg
    width: 1200
    height: 600
  color: '#09080c'
---

[Mixtral 8x7B](https://huggingface.co/NousResearch/Nous-Hermes-2-Mixtral-8x7B-DPO) is the open LLM from [Mistral AI](https://huggingface.co/mistralai). The Mixtral-8x7B is a Sparse Mixture of Experts it has a similar architecture to Mistral 7B, but comes with a twist: it’s actually 8 “expert” models in one. If you want to learn more about MoEs check out [Mixture of Experts Explained](https://huggingface.co/blog/moe).

In this blog you will learn how to deploy [NousResearch/Nous-Hermes-2-Mixtral-8x7B-DPO](https://huggingface.co/NousResearch/Nous-Hermes-2-Mixtral-8x7B-DPO) model on AWS Inferentia2 with Hugging Face Optimum on Amazon SageMaker. We are going to use the Hugging Face LLM Inf2 Container a new purpose-built Inference Container to easily deploy LLMs on AWS Inferentia2 powered by [Text Generation Inference](https://huggingface.co/docs/text-generation-inference/index) and [Optimum Neuron](https://huggingface.co/docs/optimum-neuron/index).

In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/inferentia2-mixtral-8x7b#1-setup-development-environment)
2. [Retrieve the new Hugging Face LLM Inf2 DLC](https://www.philschmid.de/inferentia2-mixtral-8x7b#2-retrieve-the-new-hugging-face-llm-inf2-dlc)
3. [Deploy Mixtral 8x7B to inferentia2](https://www.philschmid.de/inferentia2-mixtral-8x7b#3-deploy-mixtral-8x7b-to-inferentia2)
4. [Run inference and chat with the model](https://www.philschmid.de/inferentia2-mixtral-8x7b#4-run-inference-and-chat-with-the-model)
5. [Benchmark Mixtral 8x7B with llmperf on AWS Inferentia2](https://www.philschmid.de/inferentia2-mixtral-8x7b#5-benchmark-mixtral-8x7b-with-llmperf-on-aws-inferentia2)
6. [Clean up](https://www.philschmid.de/inferentia2-mixtral-8x7b#6-clean-up)

Lets get started! 🚀

## Quick intro: AWS Inferentia 2

[AWS inferentia (Inf2)](https://aws.amazon.com/de/ec2/instance-types/inf2/) are purpose-built EC2 for deep learning (DL) inference workloads. Inferentia 2 is the successor of [AWS Inferentia](https://aws.amazon.com/ec2/instance-types/inf1/?nc1=h_ls), which promises to deliver up to 4x higher throughput and up to 10x lower latency.

| instance size | accelerators | Neuron Cores | accelerator memory | vCPU | CPU Memory | on-demand price ($/h) |
| ------------- | ------------ | ------------ | ------------------ | ---- | ---------- | --------------------- |
| inf2.xlarge   | 1            | 2            | 32                 | 4    | 16         | 0.76                  |
| inf2.8xlarge  | 1            | 2            | 32                 | 32   | 128        | 1.97                  |
| inf2.24xlarge | 6            | 12           | 192                | 96   | 384        | 6.49                  |
| inf2.48xlarge | 12           | 24           | 384                | 192  | 768        | 12.98                 |

Additionally, inferentia 2 will support the writing of custom operators in c++ and new datatypes, including `FP8` (cFP8).

## 1\. Setup development environment

We are going to use the `sagemaker` python SDK to deploy Mixtral to Amazon SageMaker. We need to make sure to have an AWS account configured and the `sagemaker` python SDK installed.

Python

```python
!pip install "sagemaker>=2.223.0" "gradio<4" transformers --upgrade --quiet
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

## 2\. Retrieve the new Hugging Face LLM Inf2 DLC

The new Hugging Face TGI Neuronx DLCs can be used to run inference on AWS Inferentia2. You can use the `get_huggingface_llm_image_uri` method of the `sagemaker` SDK to retrieve the appropriate Hugging Face TGI Neuronx DLC URI based on your desired `backend`, `session`, `region`, and `version`. You can find all the available versions [here](https://github.com/aws/deep-learning-containers/releases?q=tgi+AND+neuronx&expanded=true).

Python

```python
from sagemaker.huggingface import get_huggingface_llm_image_uri
 
# retrieve the llm image uri
llm_image = get_huggingface_llm_image_uri(
  "huggingface-neuronx",
  version="0.0.23"
)
 
# print ecr image uri
print(f"llm image uri: {llm_image}")
```

## 3\. Deploy Mixtral 8x7B to inferentia2

At the time of writing, [AWS Inferentia2 does not support dynamic shapes for inference](https://awsdocs-neuron.readthedocs-hosted.com/en/v2.6.0/general/arch/neuron-features/dynamic-shapes.html#neuron-dynamic-shapes), which means that we need to specify our sequence length and batch size ahead of time.

Below is an example on how to compile Mixtral 8x7B with Optimum CLI, thats not needed in this case as we pre-compiled the model [aws-neuron/hermes-mixtral-instruct-seqlen-4096-bs-4-optimum-0-0-23](https://huggingface.co/aws-neuron/mixtral-instruct-seqlen-4096-bs-4-optimum-0-0-23) with a batch size of 4 and a sequence length of 4096.

**Example: Compile Mixtral 8x7B with Optimum CLI**

*Note: You need to compile models on an AWS EC2 instance with Inferentia2 support. Compilation can take up to 45 minutes if there is no cached configuration available.*

Python

```python
MODEL_ID = "NousResearch/Nous-Hermes-2-Mixtral-8x7B-DPO"
SEQUENCE_LENGTH = 4096
BATCH_SIZE = 4
NUM_CORES = 24 # each inferentia chip has 2 cores, e.g. inf2.48xlarge has 12 chips or 24 cores
PRECISION = "fp16"
HF_MODEL_ID_TO_PUSH="aws-neuron/hermes-mixtral-instruct-seqlen-4096-bs-4-optimum-0-0-23" # change this to your desired model id
HF_TOKEN = "YOUR_TOKEN"
 
# login into the huggingface hub to access gated models, like llama or mistral
!huggingface-cli login --token $HF_TOKEN
# compile model with optimum for batch size 4 and sequence length 2048
!optimum-cli export neuron -m {MODEL_ID} --batch_size {BATCH_SIZE} --sequence_length {SEQUENCE_LENGTH} --num_cores {NUM_CORES} --auto_cast_type {PRECISION} ./mixtral-instruct-neuron
# push model to hub [repo_id] [local_path] [path_in_repo]
!huggingface-cli upload {HF_MODEL_ID_TO_PUSH} ./mixtral-instruct-neuron ./
```

*Note: We only compile and push the architecture and not the weights. Those will still be loaded from the original repository (NousResearch/Nous-Hermes-2-Mixtral-8x7B-DPO). If you also want to push the weights remove `--exclude "checkpoint/**"` from the `upload` command. This has been avoided to speed up things.*

**Deploying Mixtral 8x7B as Endpoint**

Before deploying the model to Amazon SageMaker, we must define the TGI Neuronx endpoint configuration. We need to make sure the following additional parameters are defined:

- `HF_MODEL_ID`: The Hugging Face model ID or path to where model is stored, e.g. `/opt/ml/model`.
- `HF_NUM_CORES`: Number of Neuron Cores used for the compilation.
- `MAX_BATCH_SIZE`: The maximum batch size that the model can handle, equal to the batch size used for compilation.
- `MAX_INPUT_LENGTH`: The maximum input length that the model can handle, equal to the sequence length used for compilation.
- `MAX_TOTAL_TOKENS`: The maximum total tokens the model can generate, equal to the sequence length used for compilation.
- `HF_AUTO_CAST_TYPE`: The auto cast type that was used to compile the model.
- `HF_TOKEN`: The Hugging Face API token to access gated models, optional if the model is public.

**Select the right instance type**

Mixtral 8x7B is a large model and requires a lot of memory. We are going to use the `inf2.48xlarge` instance type, which has 192 vCPUs and 384 GB of accelerator memory. The `inf2.48xlarge` instance comes with 12 Inferentia2 accelerators that include 24 Neuron Cores.

Python

```python
from huggingface_hub import HfFolder
from sagemaker.huggingface import HuggingFaceModel
 
# sagemaker config
instance_type = "ml.inf2.48xlarge"
health_check_timeout=2400 # additional time to load the model
volume_size=512 # size in GB of the EBS volume
 
# Define Model and Endpoint configuration parameter
config = {
    "HF_MODEL_ID": "aws-neuron/hermes-mixtral-instruct-seqlen-4096-bs-4-optimum-0-0-23", # replace with your model id if you are using your own model
    "HF_NUM_CORES": "24", # number of neuron cores
    "HF_AUTO_CAST_TYPE": "fp16",  # dtype of the model
    "MAX_BATCH_SIZE": "4", # max batch size for the model
    "MAX_INPUT_LENGTH": "4000", # max length of input text
    "MAX_TOTAL_TOKENS": "4096", # max length of generated text
    "MESSAGES_API_ENABLED": "true", # Enable the messages API
}
 
 
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  image_uri=llm_image,
  env=config
)
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the `deploy` method. We will deploy the model with the `ml.inf2.48xlarge` instance type. TGI will automatically distribute and shard the model across all Inferentia devices.

Python

```python
# Deploy model to an endpoint
# https://sagemaker.readthedocs.io/en/stable/api/inference/model.html#sagemaker.model.Model.deploy
llm_model._is_compiled_model = True # We precompiled the model
 
llm = llm_model.deploy(
  initial_instance_count=1,
  instance_type=instance_type,
  container_startup_health_check_timeout=health_check_timeout,
  volume_size=volume_size
)
 
```

SageMaker will now create our endpoint and deploy the model to it. This can takes a 10-15 minutes, we are working on improving the deployment time.

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
    "model": "aws-neuron/mixtral-instruct-seqlen-4096-bs-4-optimum-0-0-23", # placholder, needed
    "top_p": 0.6,
    "temperature": 0.9,
    "max_tokens": 512,
    # "stop": ["</s>"],
}
```

Okay lets test it.

Python

```python
chat = llm.predict({"messages" :messages, **parameters})
 
print(chat["choices"][0]["message"]["content"].strip())
```

Awesome, we tested inference. Now, let's build a cool demo that supports streaming responses. [Amazon SageMaker supports streaming responses](https://aws.amazon.com/de/blogs/machine-learning/elevating-the-generative-ai-experience-introducing-streaming-support-in-amazon-sagemaker-hosting/) from your model. We can use this to stream responses, we can leverage this to create a streaming gradio application with a better user experience.

We created a sample application that you can use to test your model. You can find the code in [gradio-app.py](https://www.philschmid.de/demo/sagemaker_chat.py). The application will stream the responses from the model and display them in the UI. You can also use the application to test your model with your own inputs. With `share=True` you can share the application with others, since gradio with create a public link for you valid for 72 hours.

Python

```python
# add apps directory to path ../apps/
import sys
sys.path.append("../demo") 
from llama3_chat import create_gradio_app
 
# create gradio app
create_gradio_app(
    llm.endpoint_name,           # Sagemaker endpoint name
    session=sess.boto_session,   # boto3 session used to send request 
    system_prompt="You are an helpful Assistant, called Mixtral. Knowing everyting about AWS.",
    concurrency_count=4,         # Number of concurrent requests
    share=True,                  # Share app publicly
)
```

## 5\. Benchmark Mixtral 8x7B with llmperf on AWS Inferentia2

We successfully deployed Mixtral 8x7B to Amazon SageMaker and tested it. Now we want to benchmark the model to see how it performs. We will use a [llmperf](https://github.com/philschmid/llmperf) fork with support for `sagemaker`.

First lets install the `llmperf` package.

Python

```python
!git clone https://github.com/philschmid/llmperf.git
!pip install -e llmperf/
```

Now we can run the benchmark with the following command. We are going to benchmark using `5` concurrent users and max `100` requests. The benchmark will measure `Time-to-first-Token`, `Inter-Token-Latency (ms/token)` and `Throughput (tokens/sec)` full details can be found in the `results` folder

*🚨Important🚨: This benchmark was initiated on an instance in us-east-1. Network communication through the internet can have an impact on the `Time-to-first-Token` metric. If you want to measure the `Time-to-first-Token` correctly, you need to run the benchmark on the same host or your production region.*

Python

```python
# tell llmperf that we are using the messages api
!MESSAGES_API=true python llmperf/token_benchmark_ray.py \
--model {llm.endpoint_name} \
--llm-api "sagemaker" \
--max-num-completed-requests 100 \
--timeout 600 \
--num-concurrent-requests 5 \
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
 
print("Concurrent requests: 5")
print(f"Avg. Input token length: {int(data['results_number_input_tokens_mean'])}")
print(f"Avg. Output token length: {int(data['results_number_output_tokens_mean'])}")
print(f"Avg. Time-to-first-Token: {data['results_ttft_s_mean']*1000:.2f}ms")
print(f"Avg. Inter-Token-Latency: {data['results_inter_token_latency_s_mean']*1000:.2f}ms/token")
print(f"Avg. Thorughput: {data['results_mean_output_throughput_token_per_s']:.2f} tokens/sec")
print(f"Request per minute (RPM): {data['results_num_completed_requests_per_min']:.2f} req/min")
```

We ran the benchmark for different concurrent requests and got the following results:

Mixtral 8x7B results on inf2.48xlarge:

| Metric                              | 1      | 2      | 5       | 10      | 25       |
| ----------------------------------- | ------ | ------ | ------- | ------- | -------- |
| Avg. Input Token Length             | 568    | 559    | 562     | 538     | 561      |
| Avg. Output Token Length            | 676    | 668    | 676     | 658     | 667      |
| Avg. Time-to-First-Token (ms)       | 643.33 | 890.52 | 2435.47 | 5977.68 | 11051.98 |
| Avg. Inter-Token Latency (ms/token) | 6.45   | 7.44   | 10.67   | 17.97   | 34.05    |
| Avg. Throughput (tokens/sec)        | 136.06 | 193.89 | 288.00  | 337.28  | 354.68   |
| Requests per Minute (RPM)           | 12.07  | 17.41  | 25.54   | 30.72   | 31.87    |

We achieved a throughput of `288.00 tokens/sec` with an average inter-token latency of `10.67ms/token` and a time-to-first-token of `2435.47ms` for Mixtral 8x7B on inf2.48xlarge with 5 concurrent requests. The fastest latency was `6.45ms/token` with a time-to-first-token of `643.33ms` at 1 concurrent request.

While scaling the number of concurrent requests, we observed that throughput peaked before reaching 10 concurrent users, as the throughput and number of requests did not increase afterward. We would need to increase the number of replicas or batch size to improve the throughput. Scaling beyond 50 concurrent users, will lead to timeouts on the SageMaker side since requests are processed for longer than 60s. The inf2.48xlarge instance costs $12.98/hour on-demand and $7.79/hour with a 1-year savings plan for EC2.

This benchmark is a good start to understand the performance of Mixtral 8x7B, but if you plan to use the model in production, we recommend running a longer, more optimal detailed benchmark. Using your own data and moving client and host into the correct infrastructure setup. We successfully deployed, tested and benchmarked Mixtral 8x7B on AWS Inferentia2. 🎉

## 6\. Clean up

To clean up, we can delete the model and endpoint.

Python

```python
llm.delete_model()
llm.delete_endpoint()
```

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
