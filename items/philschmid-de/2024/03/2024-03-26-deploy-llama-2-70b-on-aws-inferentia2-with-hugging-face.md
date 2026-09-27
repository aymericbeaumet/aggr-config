---
title: Deploy Llama 2 70B on AWS Inferentia2 with Hugging Face Optimum
link: https://www.philschmid.de/inferentia2-llama-70b-inference
source: philschmid-de
published: 2024-03-26T00:00:00Z
updated: 2024-03-26T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to deploy Meta Llama 2 70B on AWS Inferentia2 with Hugging Face Optimum on Amazon SageMaker.
content: extracted
html: 2024-03-26-deploy-llama-2-70b-on-aws-inferentia2-with-hugging-face.html
preview:
  file: 2024-03-26-deploy-llama-2-70b-on-aws-inferentia2-with-hugging-face.preview-ebe55a6fd98c.webp
  width: 256
  height: 128
  alt: Deploy Llama 2 70B on AWS Inferentia2 with Hugging Face Optimum
  color: '#7e8788'
images:
- source: https://www.philschmid.de/static/blog/inferentia2-llama-70b-inference/thumbnail.jpg
  original:
    file: 2024-03-26-deploy-llama-2-70b-on-aws-inferentia2-with-hugging-face.image-f1360f6c869c.jpg
    width: 1200
    height: 600
  color: '#faf5d7'
- source: https://www.philschmid.de/static/blog/inferentia2-llama-70b-inference/gradio.png
  original:
    file: 2024-03-26-deploy-llama-2-70b-on-aws-inferentia2-with-hugging-face.image-2bffa4e6b6cc.png
    width: 907
    height: 456
  color: '#202837'
---

[Llama 2](https://www.philschmid.de/meta-llama/Llama-2-70b-chat-hf) is the latest is the open LLM from Meta, released in July 2023. It is trained on more data - 2T tokens and supports context length window upto 4K tokens and still one of the best open available LLMs. Meta fine-tuned conversational models with Reinforcement Learning from Human Feedback on over 1 million human annotations.

In this blog you will learn how to deploy [meta-llama/Llama-2-70b-chat-hf](https://www.philschmid.de/meta-llama/Llama-2-70b-chat-hf) model on AWS Inferentia2 with Hugging Face Optimum on Amazon SageMaker. We are going to use the Hugging Face LLM Inf2 Container a new purpose-built Inference Container to easily deploy LLMs on AWS Inferentia2 powered by [Text Generation Inference](https://huggingface.co/docs/text-generation-inference/index) and [Optimum Neuron](https://huggingface.co/docs/optimum-neuron/index).

In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/inferentia2-llama-70b-inference#1-setup-development-environment)
2. [Retrieve the new Hugging Face LLM Inf2 DLC](https://www.philschmid.de/inferentia2-llama-70b-inference#2-retrieve-the-new-hugging-face-llm-inf2-dlc)
3. [Deploy Llama 2 70B to inferentia2](https://www.philschmid.de/inferentia2-llama-70b-inference#3-deploy-llama-2-70b-to-inferentia2)
4. [Run inference and chat with the model](https://www.philschmid.de/inferentia2-llama-70b-inference#4-run-inference-and-chat-with-the-model)
5. [Benchmark Llama 2 70B on inferentia2](https://www.philschmid.de/inferentia2-llama-70b-inference#5-benchmark-llama-2-70b-on-inferentia2)
6. [Clean up](https://www.philschmid.de/inferentia2-llama-70b-inference#6-clean-up)

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
!pip install "sagemaker>=2.199.0" gradio transformers --upgrade --quiet
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

*Note: At the time of writing this blog post the latest version of the Hugging Face LLM DLC is not yet available via the `get_huggingface_llm_image_uri` method. We are going to use the raw container uri instead.*

Python

```python
from sagemaker.huggingface import get_huggingface_llm_image_uri
 
# retrieve the llm image uri
llm_image = get_huggingface_llm_image_uri(
  "huggingface-neuronx",
  version="0.0.20"
)
 
# print ecr image uri
print(f"llm image uri: {llm_image}")
```

## 3\. Deploy Llama 2 70B to inferentia2

At the time of writing, [AWS Inferentia2 does not support dynamic shapes for inference](https://awsdocs-neuron.readthedocs-hosted.com/en/v2.6.0/general/arch/neuron-features/dynamic-shapes.html#neuron-dynamic-shapes), which means that we need to specify our sequence length and batch size ahead of time. To make it easier for customers to utilize the full power of Inferentia2, we created a [neuron model cache](https://huggingface.co/docs/optimum-neuron/guides/cache_system), which contains pre-compiled configurations for the most popular LLMs, including Llama 2 70B.

This means we don't need to compile the model ourselves, but we can use the pre-compiled model from the cache. You can find compiled/cached configurations on the [Hugging Face Hub](https://huggingface.co/aws-neuron/optimum-neuron-cache/tree/main/inference-cache-config). If your desired configuration is not yet cached, you can compile it yourself using the [Optimum CLI](https://huggingface.co/docs/optimum-neuron/cli/compile) or open a request at the [Cache repository](https://huggingface.co/aws-neuron/optimum-neuron-cache/discussions)

Below is an example on how to compile Llama 2 70B with Optimum CLI, thats not needed in this case as we are using the pre-compiled model from the cache.

**Example: Compile Llama 2 70B with Optimum CLI**

Bash

```bash
# login into the huggingface hub to access gated models, like llama
huggingface-cli login --token [API_TOKEN]
# compile model with optimum for batch size 4 and sequence length 2048
optimum-cli export neuron -m meta-llama/Llama-2-70b-chat-hf --batch_size 4 --sequence_length 2048 --num_cores 24 --auto_cast_type fp16 ./llama-70b-chat-neuron
# push model to hub [repo_id] [local_path] [path_in_repo]
huggingface-cli upload  aws-neuron/Llama-2-70b-chat-seqlen-2048-bs-4 ./llama-70b-chat-neuron ./ --exclude "checkpoint/**"
# Move tokenizer to neuron model repository
python -c "from transformers import AutoTokenizer; AutoTokenizer.from_pretrained('meta-llama/Llama-2-70b-chat-hf').push_to_hub('aws-neuron/Llama-2-70b-chat-seqlen-2048-bs-4')"
```

*Note: You need to compile models on an AWS EC2 instance with Inferentia2 support. Compilation can take up to 45 minutes.*

**Deploying Llama2 70B as Endpoint**

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

Llama 2 70B is a large model and requires a lot of memory. We are going to use the `inf2.48xlarge` instance type, which has 192 vCPUs and 384 GB of accelerator memory. The `inf2.48xlarge` instance comes with 12 Inferentia2 accelerators that include 24 Neuron Cores. If you want to find the cached configurations for Llama 2 70B, you can find them [here](https://huggingface.co/aws-neuron/optimum-neuron-cache/blob/6392789bb1f2ab2af2867f0ab564058b781054ca/inference-cache-config/llama.json#L102). In our case we will use a batch size of 4 and a sequence length of 4096.

Before we can deploy Llama 2 70B to Inferentia2, we need to make sure we are logged in to the Hugging Face Hub and have the necessary permissions to access the model. You can request access to the model [here](https://huggingface.co/meta-llama/Llama-2-70b-chat-hf).

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
    "HF_MODEL_ID": "meta-llama/Llama-2-70b-chat-hf",
    "HF_NUM_CORES": "24", # number of neuron cores
    "HF_BATCH_SIZE": "4", # batch size used to compile the model
    "HF_SEQUENCE_LENGTH": "4096", # length used to compile the model
    "HF_AUTO_CAST_TYPE": "fp16",  # dtype of the model
    "MAX_BATCH_SIZE": "4", # max batch size for the model
    "MAX_INPUT_LENGTH": "3686", # max length of input text
    "MAX_TOTAL_TOKENS": "4096", # max length of generated text
    "HF_TOKEN": HfFolder.get_token(), # pass the huggingface token
}
 
 
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  image_uri=llm_image,
  env=config
)
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the `deploy` method. We will deploy the model with the `ml.inf2.48xlarge` instance type. TGI will automatically distribute and shard the model across all GPUs.

Python

```python
# Deploy model to an endpoint
# https://sagemaker.readthedocs.io/en/stable/api/inference/model.html#sagemaker.model.Model.deploy
llm = llm_model.deploy(
  initial_instance_count=1,
  instance_type=instance_type,
  container_startup_health_check_timeout=health_check_timeout,
  volume_size=volume_size
)
 
```

SageMaker will now create our endpoint and deploy the model to it. This can take a 20-30 minutes, we are working on improving the deployment time.

## 4\. Run inference and chat with the model

After our endpoint is deployed, we can run inference on it, using the `predict` method from `predictor`. We can provide different parameters to impact the generation, adding them to the `parameters` attribute of the payload. You can find the supported parameters [here](https://www.philschmid.de/sagemaker-llama-llm#5-run-inference-and-chat-with-the-model), or in the open API specification of TGI in the [swagger documentation](https://huggingface.github.io/text-generation-inference/)

The `meta-llama/Llama-2-70b-chat-hf` is a conversational chat model, meaning we can chat with it using a prompt structure like the following:

```plaintext
<s>[INST] <<SYS>>
{{ system_prompt }}
<</SYS>>
 
{{ user_msg_1 }} [/INST] {{ model_answer_1 }} </s><s>[INST] {{ user_msg_2 }} [/INST] {{ model_answer_2 }} </s><s>[INST] {{ user_msg_3 }} [/INST]
```

Manually preparing the prompt is error prone, so we can use the `apply_chat_template` method from the tokenizer to help with it. It expects a `messages` dictionary in the well-known OpenAI format, and converts it into the correct format for the model. Let's see if Llama 2 knows some facts about AWS.

Python

```python
from transformers import AutoTokenizer
 
# load the tokenizer
tokenizer = AutoTokenizer.from_pretrained("meta-llama/Llama-2-70b-chat-hf")
 
# Prompt to generate
messages = [
    {"role": "system", "content": "You are the AWS expert"},
    {"role": "user", "content": "Can you tell me an interesting fact about AWS?"},
]
prompt = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
 
# Generation arguments
parameters = {
    "do_sample": True,
    "top_p": 0.6,
    "temperature": 0.9,
    "max_new_tokens": 1024,
    "return_full_text": False,
}
 
res = llm.predict({"inputs": prompt, "parameters": parameters})
print(res[0]["generated_text"].strip().replace("</s>", ""))
```

Awesome, we tested infernece now lets build a cool demo which support streaming responses. [Amazon SageMaker supports streaming responses](https://aws.amazon.com/de/blogs/machine-learning/elevating-the-generative-ai-experience-introducing-streaming-support-in-amazon-sagemaker-hosting/) from your model. We can use this to stream responses, we can leverage this to create a streaming gradio application with a better user experience.

We created a sample application that you can use to test your model. You can find the code in [gradio-app.py](https://github.com/philschmid/huggingface-inferentia2-samples/tree/main/demo/sagemaker_chat.py). The application will stream the responses from the model and display them in the UI. You can also use the application to test your model with your own inputs. With `share=True` you can share the application with others, since gradio with create a public link for you valid for 72 hours.

Python

```python
# add apps directory to path ../apps/
import sys
sys.path.append("../demo")
from sagemaker_chat import create_gradio_app
 
# create gradio app
create_gradio_app(
    llm.endpoint_name,           # Sagemaker endpoint name
    session=sess.boto_session,   # boto3 session used to send request
    system_prompt="You are an helpful Assistant, called Llama 2. Knowing everyting about AWS.",
    tokenizer=tokenizer,         # Tokenizer to use format prompt
    concurrency_count=4,         # Number of concurrent requests
    share=True,                  # Share app publicly
)
```

![gradio](https://www.philschmid.de/static/blog/inferentia2-llama-70b-inference/gradio.png)

## 5\. Benchmark Llama 2 70B on inferentia2

In the last step we are going to benchmark the model on Inferentia2. We are going to run a simple load test where we send multiple parallel requests to the model and measure the latency and throughput of the model.

We added a `utils` helper to retrieve metrics from the cloudwatch logs, but this still includes Network latency and other overheads. We are working on a detailed, reproducible benchmarking guide for Optimum Neuron models on Inferentia2. Stay tuned!

Python

```python
import sys
import time
import concurrent.futures
from tqdm import tqdm
import json
 
sys.path.append("../utils")
from get_metrics import get_metrics_from_cloudwatch
 
# Generation arguments
parameters = {
    "do_sample": True,
    "top_p": 0.6,
    "temperature": 0.9,
    "max_new_tokens": 250,
    "return_full_text": False,
}
 
# The function to perform a single request
def make_request(payload):
    try:
        llm.predict(
            data={
                "inputs": tokenizer.apply_chat_template(
                    [
                        {
                            "role": "user",
                            "content": payload
                        }
                    ],
                    tokenize=False,
                    add_generation_prompt=True,
                ),
                "parameters": parameters,
            }
        )
        return 200
    except Exception as e:
        print(e)
        return 500
 
# Main function to run the load test
def run_load_test(total_requests, concurrent_users):
    with concurrent.futures.ThreadPoolExecutor(max_workers=concurrent_users) as executor:
        # Prepare a list of the same inputs to hit multiple times
        tasks = ["Write a long story about llamas and why should protect them."] * total_requests
        start_time = time.time()
 
        # run the requests
        results = list(tqdm(executor.map(make_request, tasks), total=total_requests, desc="Running load test"))
        end_time = time.time()
 
        print(f"Total time for {total_requests} requests with {concurrent_users} concurrent users: {end_time - start_time:.2f} seconds")
        print(f"Successful rate: {results.count(200) / total_requests * 100:.2f}%")
        # Get the metrics
        metrics = get_metrics_from_cloudwatch(
            endpoint_name=llm.endpoint_name,
            st=int(start_time),
            et=int(end_time),
            cu=concurrent_users,
            total_requests=total_requests,
            boto3_session=sess.boto_session
        )
        # store results
        with open("results.json", "w") as f:
            json.dump(metrics, f)
        # print results
        print(f"Llama 2 70B results on `inf2.48xlarge`:")
        print(f"Throughput: {metrics['Thorughput (tokens/second)']:,.2f} tokens/s")
        print(f"Latency p(50): {metrics['Latency (ms/token) p(50)']:,.2f} ms/token")
        return metrics
 
# Run the load test
concurrent_users = 5
number_of_requests = 100
res = run_load_test(number_of_requests, concurrent_users)
```

*Note: We want to mention again that the benchmark is not a perfect representation of the model performance, as it includes network latency and other overheads. We were sending request from eu-central-1 to us-east-1, which adds additional latency to the requests. We are working on a detailed benchmark with more metrics and a better understanding of the model performance.*

Results with 250 token generation:

Bash

```bash
Llama 2 70B results on inf2.48xlarge:
Throughput: 42.23 tokens/s
Latency p(50): 88.80 ms/token
```

*Note: We ran a similar test on a `ml.g5.48xlarge` instance with 8 NVIDIA A10G GPUs as well, but needed to decrease the context size to 2048 from 4096 and generated tokens from 250 to 50. With g5 we achieved an thorughput of `~38.5` tokens per second.*

On Inferentia2 we achieved a throughput of `~42.23` tokens per second. Thats a improvement compared to the `ml.g5.48xlarge` instance. The `g5` benchmark was slightly different so it is not 100% compareable. Assuming it would be the cost-performance per token between `g5.48xlarge` (`16.28$/h`) and `inf2.48xlarge` (`12,98$`) would show a ~44.7% improvement for $ per token price. This makes inferentia2 a valid alternative to NVIDIA A10G GPUs.

## 6\. Clean up

To clean up, we can delete the model and endpoint.

Python

```python
llm.delete_model()
llm.delete_endpoint()
```

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
