---
title: Deploy Llama 3 70B on AWS Inferentia2 with Hugging Face Optimum
link: https://www.philschmid.de/inferentia2-llama3-70b
source: philschmid-de
published: 2024-05-23T00:00:00Z
updated: 2024-05-23T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to deploy Llama 3 70B on AWS Inferentia2 with Hugging Face Optimum on Amazon SageMaker.
content: extracted
html: 2024-05-23-deploy-llama-3-70b-on-aws-inferentia2-with-hugging-face.html
preview:
  file: 2024-05-23-deploy-llama-3-70b-on-aws-inferentia2-with-hugging-face.preview-2658ce9e5aa4.webp
  width: 256
  height: 128
  alt: Deploy Llama 3 70B on AWS Inferentia2 with Hugging Face Optimum
  color: '#738680'
images:
- source: https://www.philschmid.de/static/blog/inferentia2-llama-3-70b/thumbnail.jpg
  original:
    file: 2024-05-23-deploy-llama-3-70b-on-aws-inferentia2-with-hugging-face.image-b898c5c8f3a2.jpg
    width: 1200
    height: 600
  color: '#181909'
- source: https://www.philschmid.de/static/blog/inferentia2-llama-3-70b/llama3.png
  original:
    file: 2024-05-23-deploy-llama-3-70b-on-aws-inferentia2-with-hugging-face.image-16952fd6a53a.png
    width: 834
    height: 637
  color: '#141929'
---

[Llama 3](https://huggingface.co/blog/llama3) is the latest is the open LLM from Meta, released in April 2024. It is trained on more data - 15T tokens and supports context length window up to 8K tokens and is one of the best open available LLMs. Meta fine-tuned conversational models with Reinforcement Learning from Human Feedback on over 10 million human annotations.

In this blog you will learn how to deploy [/meta-llama/Meta-Llama-3-70B-Instruct](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct) model on AWS Inferentia2 with Hugging Face Optimum on Amazon SageMaker. We are going to use the Hugging Face LLM Inf2 Container a new purpose-built Inference Container to easily deploy LLMs on AWS Inferentia2 powered by [Text Generation Inference](https://huggingface.co/docs/text-generation-inference/index) and [Optimum Neuron](https://huggingface.co/docs/optimum-neuron/index).

In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/inferentia2-llama3-70b#1-setup-development-environment)
2. [Retrieve the new Hugging Face LLM Inf2 DLC](https://www.philschmid.de/inferentia2-llama3-70b#2-retrieve-the-new-hugging-face-llm-inf2-dlc)
3. [Deploy Llama 3 70B to inferentia2](https://www.philschmid.de/inferentia2-llama3-70b#3-deploy-llama-3-70b-to-inferentia2)
4. [Run inference and chat with the model](https://www.philschmid.de/inferentia2-llama3-70b#4-run-inference-and-chat-with-the-model)
5. [Benchmark llama 3 70B with llmperf on AWS Inferentia2](https://www.philschmid.de/inferentia2-llama3-70b#5-benchmark-llama-3-70b-with-llmperf-on-aws-inferentia2)
6. [Clean up](https://www.philschmid.de/inferentia2-llama3-70b#6-clean-up)

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

We are going to use the `sagemaker` python SDK to deploy Llama 3 to Amazon SageMaker. We need to make sure to have an AWS account configured and the `sagemaker` python SDK installed.

Python

```python
!pip install "sagemaker>=2.199.0" "gradio<4" transformers --upgrade --quiet
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
# TODO: Comment in when released
from sagemaker.huggingface import get_huggingface_llm_image_uri
 
# retrieve the llm image uri
llm_image = get_huggingface_llm_image_uri(
  "huggingface-neuronx",
  version="0.0.22"
)
 
print(f"llm image uri: {llm_image}")
```

## 3\. Deploy Llama 3 70B to inferentia2

At the time of writing, [AWS Inferentia2 does not support dynamic shapes for inference](https://awsdocs-neuron.readthedocs-hosted.com/en/v2.6.0/general/arch/neuron-features/dynamic-shapes.html#neuron-dynamic-shapes), which means that we need to specify our sequence length and batch size ahead of time. To make it easier for customers to utilize the full power of Inferentia2, we created a [neuron model cache](https://huggingface.co/docs/optimum-neuron/guides/cache_system), which contains pre-compiled configurations for the most popular LLMs, including Llama 3 70B.

This means we don't need to compile the model ourselves, but we can use the pre-compiled model from the cache. You can find compiled/cached configurations on the [Hugging Face Hub](https://huggingface.co/aws-neuron/optimum-neuron-cache/tree/main/inference-cache-config). If your desired configuration is not yet cached, you can compile it yourself using the [Optimum CLI](https://huggingface.co/docs/optimum-neuron/cli/compile) or open a request at the [Cache repository](https://huggingface.co/aws-neuron/optimum-neuron-cache/discussions)

Below is an example on how to compile Llama 3 70B with Optimum CLI, thats not needed in this case as we are using the pre-compiled model from the cache.

**Example: Compile Llama 3 70B with Optimum CLI**

Bash

```bash
# login into the huggingface hub to access gated models, like llama
huggingface-cli login --token [API_TOKEN]
# compile model with optimum for batch size 4 and sequence length 2048
optimum-cli export neuron -m meta-llama/Meta-Llama-3-70B-Instruct --batch_size 4 --sequence_length 2048 --num_cores 24 --auto_cast_type fp16 ./llama-70b-chat-neuron
# push model to hub [repo_id] [local_path] [path_in_repo]
huggingface-cli upload  aws-neuron/Llama-3-70b-chat-seqlen-2048-bs-4 ./llama-70b-chat-neuron ./ --exclude "checkpoint/**"
# Move tokenizer to neuron model repository
python -c "from transformers import AutoTokenizer; AutoTokenizer.from_pretrained('meta-llama/Meta-Llama-3-70B-Instruct').push_to_hub('aws-neuron/Llama-3-70b-chat-seqlen-2048-bs-4')"
```

*Note: You need to compile models on an AWS EC2 instance with Inferentia2 support. Compilation can take up to 45 minutes.*

**Deploying Llama 3 70B as Endpoint**

Before deploying the model to Amazon SageMaker, we must define the TGI Neuronx endpoint configuration. We need to make sure the following additional parameters are defined:

- `HF_NUM_CORES`: Number of Neuron Cores used for the compilation.
- `HF_BATCH_SIZE`: The batch size that was used to compile the model.
- `HF_SEQUENCE_LENGTH`: The sequence length that was used to compile the model.
- `HF_AUTO_CAST_TYPE`: The auto cast type that was used to compile the model.

We still need to define traditional TGI parameters with:

- `HF_MODEL_ID`: The Hugging Face model ID.
- `HF_TOKEN`: The Hugging Face API token to access gated models.
- `MAX_BATCH_SIZE`: The maximum batch size that the model can handle, equal to the batch size used for compilation.
- `MAX_INPUT_LENGTH`: The maximum input length that the model can handle.
- `MAX_TOTAL_TOKENS`: The maximum total tokens the model can generate, equal to the sequence length used for compilation.

**Select the right instance type**

Llama 3 70B is a large model and requires a lot of memory. We are going to use the `inf2.48xlarge` instance type, which has 192 vCPUs and 384 GB of accelerator memory. The `inf2.48xlarge` instance comes with 12 Inferentia2 accelerators that include 24 Neuron Cores. If you want to find the cached configurations for Llama 3 70B, you can find them [here](https://huggingface.co/aws-neuron/optimum-neuron-cache/blob/6392789bb1f2ab2af2867f0ab564058b781054ca/inference-cache-config/llama.json#L102). In our case we will use a batch size of 4 and a sequence length of 4096.

Before we can deploy Llama 3 70B to Inferentia2, we need to make sure we are logged in to the Hugging Face Hub and have the necessary permissions to access the model. You can request access to the model [here](https://huggingface.co/meta-llama/Meta-Llama-3-70B-Instruct).

Bash

```bash
huggingface-cli login --token [API_TOKEN]
```

After that we can create our endpoint configuration and deploy the model to Amazon SageMaker.

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
    "HF_MODEL_ID": "meta-llama/Meta-Llama-3-70B-Instruct",
    "HF_NUM_CORES": "24", # number of neuron cores
    "HF_BATCH_SIZE": "4", # batch size used to compile the model
    "HF_SEQUENCE_LENGTH": "4096", # length used to compile the model
    # "HF_AUTO_CAST_TYPE": "bf16",  # dtype of the model
    "HF_AUTO_CAST_TYPE": "fp16",  # dtype of the model
    "MAX_BATCH_SIZE": "4", # max batch size for the model
    "MAX_INPUT_LENGTH": "4000", # max length of input text
    "MAX_TOTAL_TOKENS": "4096", # max length of generated text
    "MESSAGES_API_ENABLED": "true", # Enable the messages API
    "HF_TOKEN": HfFolder.get_token(), # pass the huggingface token
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
# deactivate warning since model is compiled
llm_model._is_compiled_model = True
 
llm = llm_model.deploy(
  initial_instance_count=1,
  instance_type=instance_type,
  container_startup_health_check_timeout=health_check_timeout,
  volume_size=volume_size
)
```

SageMaker will now create our endpoint and deploy the model to it. This can takes a 30-40 minutes, we are working on improving the deployment time.

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
    "max_tokens": 50,
    "stop": ["<|eot_id|>"],
}
```

Okay lets test it.

Python

```python
chat = llm.predict({"messages" :messages, **parameters,"steam":True})
 
print(chat["choices"][0]["message"]["content"].strip())
```

Awesome, we tested infernece now lets build a cool demo which support streaming responses. [Amazon SageMaker supports streaming responses](https://aws.amazon.com/de/blogs/machine-learning/elevating-the-generative-ai-experience-introducing-streaming-support-in-amazon-sagemaker-hosting/) from your model. We can use this to stream responses, we can leverage this to create a streaming gradio application with a better user experience.

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
    system_prompt="You are an helpful Assistant, called Llama 3. Knowing everyting about AWS.",
    concurrency_count=4,         # Number of concurrent requests
    share=True,                  # Share app publicly
)
```

![custom container](https://www.philschmid.de/static/blog/inferentia2-llama-3-70b/llama3.png)

## 5\. Benchmark llama 3 70B with llmperf on AWS Inferentia2

We successfully deployed Llama 3 70B to Amazon SageMaker and tested it. Now we want to benchmark the model to see how it performs. We will use a [llmperf](https://github.com/philschmid/llmperf) fork with support for `sagemaker`.

First lets install the `llmperf` package.

Python

```python
!git clone https://github.com/philschmid/llmperf.git
!pip install -e llmperf/
```

Now we can run the benchmark with the following command. We are going to benchmark using `5` concurrent users and max `50` requests. The benchmark will measure `first-time-to-token`, `latency (ms/token)` and `throughput (tokens/s)` full details can be found in the `results` folder

*🚨Important🚨: This benchmark was initiatied from Europe, while the endpoint runs in us-east-1. This has significant impact on the `first-time-to-token` metric, since it includes the network communication. If you want to measure the `first-time-to-token` correctly, you need to run the benchmark on the same host or your production region.*

Python

```python
# tell llmperf that we are using the messages api
!MESSAGES_API=true python llmperf/token_benchmark_ray.py \
--model {llm.endpoint_name} \
--llm-api "sagemaker" \
--max-num-completed-requests 50 \
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
print(f"Avg. Input token length: {data['mean_input_tokens']}")
print(f"Avg. Output token length: {data['mean_output_tokens']}")
print(f"Avg. First-Time-To-Token: {data['results_ttft_s_mean']*1000:.2f}ms")
print(f"Avg. Thorughput: {data['results_mean_output_throughput_token_per_s']:.2f} tokens/sec")
print(f"Avg. Latency: {data['results_inter_token_latency_s_mean']*1000:.2f}ms/token")
```

Results with 150 token generation:

Bash

```bash
Concurrent requests: 5
Avg. Input token length: 550
Avg. Output token length: 150
Avg. First-Time-To-Token: 5505.33ms
Avg. Thorughput: 132.8 tokens/sec
Avg. Latency: 23.46ms/token
```

Thats it! We successfully deployed, tested and benchmarked Llama 3 70B on AWS Inferentia2. The benchmark is not a full representation of the model performance, but it gives you a first good indication. If you plan to use the model in production, we recommend to run a longer and closer to your production benchmark, modify the number of replicas see ([Scale LLM Inference on Amazon SageMaker with Multi-Replica Endpoints](https://www.philschmid.de/sagemaker-multi-replicahttps://www.philschmid.de/sagemaker-multi-replica)) and most importantly test the model with your own data.

## 6\. Clean up

To clean up, we can delete the model and endpoint.

Python

```python
llm.delete_model()
llm.delete_endpoint()
```

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
