---
title: Scale LLM Inference on Amazon SageMaker with Multi-Replica Endpoints
link: https://www.philschmid.de/sagemaker-multi-replica
source: philschmid-de
published: 2024-01-11T00:00:00Z
updated: 2024-01-11T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to increase the throughput of Llama 13B on Amazon SageMaker using single instance multi-replica endpoints.
content: extracted
html: 2024-01-11-scale-llm-inference-on-amazon-sagemaker-with-multi-replica.html
preview:
  file: 2024-01-11-scale-llm-inference-on-amazon-sagemaker-with-multi-replica.preview-e4aac6472d75.webp
  width: 256
  height: 134
  alt: Scale LLM Inference on Amazon SageMaker with Multi-Replica Endpoints
  color: '#8fbcc5'
images:
- source: https://www.philschmid.de/static/blog/sagemaker-multi-replica/thumbnail.jpg
  original:
    file: 2024-01-11-scale-llm-inference-on-amazon-sagemaker-with-multi-replica.image-8199b6688ba4.jpg
    width: 2400
    height: 1254
  color: '#aaead6'
---

One of the key Amazon SageMaker announcements at this year's re:Invent (2023) was the new Hardware Requirements object for Amazon SageMaker endpoints. This provides granular control over the compute resources for models deployed on SageMaker, including minimum CPU, GPU, memory, and number of replicas. This allows you to optimize your model's throughput and cost by matching the compute resources to the model's requirements and allows you to deploy multiple LLMs on the same instance. Previously it was not possible to deploy multiple replicas of a LLM or multiple LLMs on a single endpoint, can limit the overall throughput of models are not compute bound, e.g. open LLMs like a single Llama 13B on p4d.24xlarge instances.

In this post, we show how to use the new feature using the sagemaker sdk and `ResourceRequirements` object to optimize the deployment of Llama 13B for increased throughput and cost performance on Amazon SageMaker on a `p4d.24xlarge` instance. The `p4d.24xlarge` instance has 8x A100 GPUs 40GB, which allows us to deploy 8 replicas of Llama 13B on a single instance. You can also use this example to deploy other open LLMs like Mistral, T5 or StarCoder. Additionally it is possible to deploy multiple models on a single instance, e.g. 4x Llama 13B and 4x Mistral 7B. Check out the amazing [blog post from Antje for this](https://aws.amazon.com/de/blogs/aws/amazon-sagemaker-adds-new-inference-capabilities-to-help-reduce-foundation-model-deployment-costs-and-latency/).

We are going to use the Hugging Face LLM DLC is a new purpose-built Inference Container to easily deploy LLMs in a secure and managed environment. The DLC is powered by [Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) a scalelable, optimized solution for deploying and serving Large Language Models (LLMs). The Blog post also includes Hardware requirements for the different model sizes.

In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/sagemaker-multi-replica#1-setup-development-environment)
2. [Retrieve the new Hugging Face LLM DLC](https://www.philschmid.de/sagemaker-multi-replica#2-retrieve-the-new-hugging-face-llm-dlc)
3. [Configure Hardware requirements per replica](https://www.philschmid.de/sagemaker-multi-replica#3-configure-hardware-requirements-per-replica)
4. [Deploy and Test Llama 2 on Amazon SageMaker](https://www.philschmid.de/sagemaker-multi-replica#4-deploy-llama-2-to-amazon-sagemaker)
5. [Run performance a simple performance benchmark](https://www.philschmid.de/sagemaker-multi-replica#5-benchmark-multi-replica-endpoint)

Lets get started!

## 1\. Setup development environment

We are going to use the `sagemaker` python SDK to deploy Llama 2 to Amazon SageMaker. We need to make sure to have an AWS account configured and the `sagemaker` python SDK installed.

Python

```python
!pip install "sagemaker>=2.199.0" "transformers==4.35.2" --upgrade --quiet
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

## 2\. Retrieve the new Hugging Face LLM DLC

Compared to deploying regular Hugging Face models we first need to retrieve the container uri and provide it to our `HuggingFaceModel` model class with a `image_uri` pointing to the image. To retrieve the new Hugging Face LLM DLC in Amazon SageMaker, we can use the `get_huggingface_llm_image_uri` method provided by the `sagemaker` SDK. This method allows us to retrieve the URI for the desired Hugging Face LLM DLC based on the specified `backend`, `session`, `region`, and `version`. You can find the available versions [here](https://github.com/aws/deep-learning-containers/blob/master/available_images.md#huggingface-text-generation-inference-containers)

Python

```python
from sagemaker.huggingface import get_huggingface_llm_image_uri
 
# retrieve the llm image uri
llm_image = get_huggingface_llm_image_uri(
  "huggingface",
  version="1.1.0"
)
 
# print ecr image uri
print(f"llm image uri: {llm_image}")
```

## 3\. Configure Hardware requirements per replica

Llama 2 comes in 3 different sizes - 7B, 13B & 70B parameters. The hardware requirements will vary based on the model size deployed to SageMaker. Below is an example configuration for Llama 13B. In addition we tried to provide some high level overview of the different hardware requirements for the different model sizes. To keep it simple we only looked at the `p4d.24xlarge` instance type and AWQ/GPTQ quantization.

| Model                                                               | Instance Type       | Quantization | \# replica |
| ------------------------------------------------------------------- | ------------------- | ------------ | ---------- |
| [Llama 7B](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf)   | `(ml.)p4d.24xlarge` | `-`          | 8          |
| [Llama 7B](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf)   | `(ml.)p4d.24xlarge` | `GPTQ/AWQ`   | 8          |
| [Llama 13B](https://huggingface.co/meta-llama/Llama-2-13b-chat-hf) | `(ml.)p4d.24xlarge` | `-`          | 8          |
| [Llama 13B](https://huggingface.co/meta-llama/Llama-2-13b-chat-hf) | `(ml.)p4d.24xlarge` | `GPTQ/AWQ`   | 8          |
| [Llama 70B](https://huggingface.co/meta-llama/Llama-2-70b-chat-hf) | `(ml.)p4d.24xlarge` | `-`          | 2          |
| [Llama 70B](https://huggingface.co/meta-llama/Llama-2-70b-chat-hf) | `(ml.)p4d.24xlarge` | `GPTQ/AWQ`   | 4          |

*We didn't test the configurations yet. If you run into errors please let me know and I will update the blog post.*

Python

```python
from sagemaker.compute_resource_requirements.resource_requirements import ResourceRequirements
 
llama2_13b_resource_config = ResourceRequirements(
    requests = {
        "copies": 8, # Number of replicas
        "num_accelerators": 1, # Number of GPUs
        "num_cpus": 10,  # Number of CPU cores  96 // num_replica - more for management
        "memory": 100 * 1024,  # Minimum memory in MB 1152 // num_replica - more for management
    },
)
```

## 4\. Deploy Llama 2 to Amazon SageMaker

To deploy [meta-llama/Llama-2-13b-chat-hf](https://huggingface.co/models?other=llama-2) to Amazon SageMaker we create a `HuggingFaceModel` model class and define our endpoint configuration including the `hf_model_id`, `instance_type` and then add our `ResourceRequirements` object to the `deploy` method.

*Note: This is a form to enable access to Llama 2 on Hugging Face after you have been granted access from Meta. Please visit the [Meta website](https://ai.meta.com/resources/models-and-libraries/llama-downloads) and accept our license terms and acceptable use policy before submitting this form. Requests will be processed in 1-2 days. We alternatively use the ungated weights from `NousResearch/Llama-2-13b-chat-hf`.*

Python

```python
import json
import uuid
from sagemaker.huggingface import HuggingFaceModel
from sagemaker.enums import EndpointType
 
# sagemaker config
instance_type = "ml.p4d.24xlarge"
health_check_timeout = 300
 
# Define Model and Endpoint configuration parameter
config = {
  'HF_MODEL_ID': "meta-llama/Llama-2-13b-chat-hf", # model_id from hf.co/models
  'SM_NUM_GPUS': json.dumps(1), # Number of GPU used per replica
  'MAX_INPUT_LENGTH': json.dumps(2048),  # Max length of input text
  'MAX_TOTAL_TOKENS': json.dumps(4096),  # Max length of the generation (including input text)
  'MAX_BATCH_TOTAL_TOKENS': json.dumps(16384),  # Limits the number of tokens that can be processed in parallel during the generation
  'HUGGING_FACE_HUB_TOKEN': "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" # uncomment when using a private model
  # 'HUGGING_FACE_HUB_TOKEN': "<REPLACE WITH YOUR TOKEN>" # uncomment when using a private model
  # ,'HF_MODEL_QUANTIZE': "gptq", # comment in when using awq quantized checkpoint
 
}
 
assert config['HUGGING_FACE_HUB_TOKEN'] != '<REPLACE WITH YOUR TOKEN>', "You have to provide a token."
 
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  image_uri=llm_image,
  env=config,
)
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the `deploy` method using the `ResourceRequirements` object.

Python

```python
# Deploy model to an endpoint
# https://sagemaker.readthedocs.io/en/stable/api/inference/model.html#sagemaker.model.Model.deploy
llm = llm_model.deploy(
  initial_instance_count=1, # number of instances
  instance_type=instance_type, # base instance type
  resources=llama2_13b_resource_config, # resource config for multi-replica
  container_startup_health_check_timeout=health_check_timeout, # 10 minutes to be able to load the model
  endpoint_name=f"llama-2-13b-chat-{str(uuid.uuid4())}", # name needs to be unique
  endpoint_type=EndpointType.INFERENCE_COMPONENT_BASED # needed to use resource config
 
)
 
```

SageMaker will now create our endpoint and deploy the model to it. This can takes a 15-25 minutes, since the replicas are deployed after each other. After the endpoint is created we can use the `predict` method to send a request to our endpoint. To make it easier we will use the [apply\_chat\_template](https://www.philschmid.de/apply_chat_template) method from transformers. This allow us to send "openai" like converstaions to our model.

Python

```python
from transformers import AutoTokenizer
 
tok = AutoTokenizer.from_pretrained(config["HF_MODEL_ID"],use_auth_token=config["HUGGING_FACE_HUB_TOKEN"])
 
# OpenAI like conversational messages
messages = [
  {"role": "system", "content": "You are an helpful AWS Expert Assistant. Respond only with 1-2 sentences."},
  {"role": "user", "content": "What is Amazon SageMaker?"},
]
 
# generation parameters
parameters = {
    "do_sample" : True,
    "top_p": 0.6,
    "temperature": 0.9,
    "top_k": 50,
    "max_new_tokens": 50,
    "repetition_penalty": 1.03,
    "return_full_text": False,
}
 
res = llm.predict(
  {
    "inputs": tok.apply_chat_template(messages, tokenize=False),
    "parameters": parameters
   })
 
print(res[0]['generated_text'].strip())
```

> Sure, I'd be happy to help! Amazon SageMaker is a fully managed service that provides a range of machine learning (ML) algorithms and tools to help you build, train, and deploy ML models at scale.

## 5\. Benchmark multi-replica endpoint

To Benchmark our new endpoint we will use the same code as for the ["Llama 2 on Amazon SageMaker a Benchmark"](https://huggingface.co/blog/llama-sagemaker-benchmark) from [text-generation-inference-tests](https://github.com/philschmid/text-generation-inference-tests/tree/master/sagemaker_llm_container). When running the benchmark back then it was not possible to deploy multiple replicas of a model on a single endpoint. This limited the maximum throughput we could achieve with Llama 13B on p4d.24xlarge instances. We benchmark a typical usecase, with long inputs and short generation with a prompt length of 1000 tokens and number of generated tokens of 50.

To run the benchmark we need to clone the [text-generation-inference-tests](https://github.com/philschmid/text-generation-inference-tests) and also make sure we meet all the `Prerequisites` from the [README](https://github.com/philschmid/text-generation-inference-tests/tree/master/sagemaker_llm_container), including the installation of [k6](https://k6.io/) which used to run the benchmark.

Python

```python
# check out branch with test using 1000 tokens
!git clone -b "input-1000-gen-50"  https://github.com/philschmid/text-generation-inference-tests.git
```

Since we already a deployed endpoint we can provide the `endpoint_name` and our hardware requirements as `inference_component`. The name of the `inference_component` can currently only be retrieved using `boto3`

Python

```python
inference_component = llm_model.sagemaker_session.list_inference_components(endpoint_name_equals=llm.endpoint_name).get("InferenceComponents")[0].get("InferenceComponentName")
endpoint_name = llm.endpoint_name
```

Change the directory to `sagemaker_llm_container` and run the benchmark. The command below will run a load test for 90s with 300 concurrent users. The result will be saved to `text-generation-inference-tests/sagemaker_llm_container/{CONIFG_NAME}.json`.

Python

```python
!cd text-generation-inference-tests/sagemaker_llm_container && python benchmark.py \
  --endpoint_name {endpoint_name} \
  --inference_component {inference_component} \
  --model_id {config["HF_MODEL_ID"]} \
  --tp_degree 1 \
  --vu 300
```

We ran multiple benchmarks for different concurrent users starting with 100 up to 300 and `MAX_BATCH_TOTAL_TOKENS` of 16384. Eace request sends a prompt of 1000 tokens and generates 50 tokens. The table below includes the total number of requests, throughput (req/sec), the median request time and the percentage of successful requests (in %).

| Model     | Concurrent Users | requests | throughput (req/sec) | med. latency (sec) | request rate (%) |
| --------- | ---------------- | -------- | -------------------- | ------------------ | ---------------- |
| Llama 13B | 100              | 2322     | 24.741461/s          | 3.76s              | 100%             |
| Llama 13B | 150              | 2543     | 26.316863/s          | 5.58s              | 100%             |
| Llama 13B | 200              | 2648     | 26.720773/s          | 7.81s              | 100%             |
| Llama 13B | 250              | 2769     | 25.82929/s           | 8.89s              | 97%              |
| Llama 13B | 300              | 2776     | 25.662124/s          | 9.78s              | 100%             |
| Llama 13B | 350              | 2827     | 26.713271/s          | 12.57s             | 100%             |
| Llama 13B | 400              | 2880     | 24.161486/s          | 12.53s             | 100%             |
| Llama 13B | 450              | 2934     | 26.138126/s          | 15.69s             | 100%             |
| Llama 13B | 500              | 2976     | 26.170628/s          | 16.9s              | 100%             |

We were able to serve 500 concurrent users with a throughput of 26.17 requests per second and a median latency of 16.9 seconds without requests failing. 500 concurrent user is not the limit of the endpoint. We could increase the number of concurrent users and extend the benchmark to find the limit of the endpoint. But this is out of scope for this blog post. It is also important to respect the latency of the endpoint. If you have a latency requirement of < 100s second, you can only serve 300 concurrent users.

Python

```python
!rm -rf text-generation-inference-tests
```

## 6\. Clean up

To clean up, we can delete the model, endpoint and inference component for the hardware requirements.

*Note: If you have issues deleting an endpoint with an attached inference component, see: [https://repost.aws/es/questions/QUEiuS2we2TEKe9GUUYm67kQ/error-when-deleting-and-inference-endpoint-in-sagemaker](https://repost.aws/es/questions/QUEiuS2we2TEKe9GUUYm67kQ/error-when-deleting-and-inference-endpoint-in-sagemaker)*

Python

```python
# Delete Inference Component & Model
llm_model.sagemaker_session.delete_inference_component(inference_component_name=inference_component)
llm.delete_model()
```

we have to wait until the component is deleted before we can delete the endpoint. (can take 2minutes)

Python

```python
# If this call fails, you can delete the endpoint manually using the AWS Console
llm.delete_endpoint()
```

## Conclusion

By leveraging the ability to deploy multiple replicas of models like Llama 13B on instances such as p4d.24xlarge, companies can serve a substantially higher number of concurrent users without compromising the stability of the service. While there is an increase in latency under heavy load, the system maintains its robustness. This balance between scalability and stability can be crucial for organizations looking to deploy LLMs in production environments.

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
