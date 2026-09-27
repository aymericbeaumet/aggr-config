---
title: Accelerate Mixtral 8x7B with Speculative Decoding and Quantization on Amazon SageMaker
link: https://www.philschmid.de/sagemaker-awq-medusa
source: philschmid-de
published: 2024-04-02T00:00:00Z
updated: 2024-04-02T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to accelerate Mixtral using Speculative Decoding (Medusa) and Quantization (AWQ).
content: extracted
html: 2024-04-02-accelerate-mixtral-8x7b-with-speculative-decoding-and.html
preview:
  file: 2024-04-02-accelerate-mixtral-8x7b-with-speculative-decoding-and.preview-1928fb72f02b.webp
  width: 256
  height: 128
  alt: Accelerate Mixtral 8x7B with Speculative Decoding and Quantization on Amazon SageMaker
  color: '#939794'
images:
- source: https://www.philschmid.de/static/blog/sagemaker-awq-medusa/thumbnail.jpg
  original:
    file: 2024-04-02-accelerate-mixtral-8x7b-with-speculative-decoding-and.image-30aa51838ec1.jpg
    width: 1200
    height: 600
  color: '#fefdfd'
---

In this blog you will learn how to accelerate [mistralai/Mixtral-8x7B-Instruct-v0.1](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1) on Amazon SageMaker using Speculative Decoding (Medusa) and Quantization (AWQ). We are going to use the Hugging Face LLM DLC a purpose-built Inference Container powered by [Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) a scalelable, optimized solution for deploying and serving Large Language Models (LLMs). We will prepare the artifacts locally, upload them to Amazon S3, and deploy the model to Amazon SageMaker.

Combining Medusa and AWQ allows not only to reduce the memory footprint but also to accelerate the inference of Mixtral 8x7B. With this we can deploy Mixtral 8x7B on a single `g5.12xlarge` instance with only 4x NVIDIA A10G GPUs achieving a latency of ~50-60ms per token.

[Mixtral 8x7B](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1) is an open LLM from [Mistral AI](https://huggingface.co/mistralai) Mixtral-8x7B is a Sparse Mixture of Experts. If you want to learn more about MoEs check out [Mixture of Experts Explained](https://huggingface.co/blog/moe). In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/sagemaker-awq-medusa#1-setup-development-environment)
2. [Retrieve the new Hugging Face LLM DLC](https://www.philschmid.de/sagemaker-awq-medusa#2-retrieve-the-new-hugging-face-llm-dlc)
3. [Prepare Medusa and AWQ artifacts](https://www.philschmid.de/sagemaker-awq-medusa#3-prepare-medusa-and-awq-artifacts)
4. [Deploy Mixtral 8x7B to Amazon SageMaker](https://www.philschmid.de/sagemaker-awq-medusa#4-deploy-mixtral-8x7b-to-amazon-sagemaker)
5. [Run inference and chat with the model](https://www.philschmid.de/sagemaker-awq-medusa#5-run-inference-and-chat-with-the-model)

Lets get started!

Before we go into the details, let's first understand what Speculative Decoding and Quantization are and how they can help accelerate Mixtral 8x7B.

**Speculative Decoding (Medusa)**

[Medusa](https://github.com/FasterDecoding/Medusa/tree/main) is a speculative decoding like technique that can be used to accelerate inference for large language models. It adds extra "heads" to LLMs to predict multiple future tokens simultaneously. To use Medusa, you need to train medusa heads on a small dataset preferably similar to the original training data. Medusa can deliver up to a 2x speed increase across a range of LLMs. We are going to use [text-generation-inference/Mixtral-8x7B-Instruct-v0.1-medusa](https://huggingface.co/text-generation-inference/Mixtral-8x7B-Instruct-v0.1-medusa). The performance improvements depends on the input prompt and can vary.

**Quantization (AWQ)**

[AWQ or Activation-aware Weight Quantization](https://github.com/mit-han-lab/llm-awq) is a method designed for efficiently compressing and accelerating LLMs by quantizing weights based on the significance of their activations. Resulting in significant reduced memory requirements and improved model latency. AWQ can reduce the memory footprint of a model by up to 4x. We are going to use [ybelkada/Mixtral-8x7B-Instruct-v0.1-AWQ](https://huggingface.co/ybelkada/Mixtral-8x7B-Instruct-v0.1-AWQ) a awq quantized version of Mixtral 8x7B Instruct.

## 1\. Setup development environment

We are going to use the `sagemaker` python SDK to deploy Mixtral to Amazon SageMaker. We need to make sure to have an AWS account configured and the `sagemaker` python SDK installed.

Python

```python
!pip install "sagemaker>=2.199.0" transformers --upgrade --quiet
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
  version="1.4.2"
)
 
# print ecr image uri
print(f"llm image uri: {llm_image}")
```

## 3\. Prepare Medusa and AWQ artifacts

In this step we are going to prepare the Medusa and AWQ artifacts. Hugging Face Text Generation Inference container supports [Medusa](https://huggingface.co/docs/text-generation-inference/conceptual/speculation) and AWQ. For Medusa we need to provide the a [config.json](https://huggingface.co/text-generation-inference/Mixtral-8x7B-Instruct-v0.1-medusa/blob/main/config.json) with the medusa specifications, including `base_model_name_or_path` and `medusa_lm_head.safetensors`. If we only wanted to deploy the Medusa model we could use `text-generation-inference/Mixtral-8x7B-Instruct-v0.1-medusa` as `HF_MODEL_ID`. But since we want to combine Medusa and AWQ and deploy from Amazon S3 we need to modify the `config.json` to include the AWQ model. We are going to use the [ybelkada/Mixtral-8x7B-Instruct-v0.1-AWQ](https://huggingface.co/ybelkada/Mixtral-8x7B-Instruct-v0.1-AWQ) model.

This will lead to a directory structure like this:

```plaintext
model/
├── llm/
│   ├── config.json
│   ├── model-00001-of-00005.safetensors
│   ├── ...
├── config.json
├── medusa_lm_head.safetensors
└── ...
```

We will first load the Medusa model and AWQ model from Hugging Face, modify the `config.json` and then upload the artifacts to Amazon S3.

Python

```python
import json
import os
from huggingface_hub import snapshot_download
from sagemaker.s3 import S3Uploader
 
tmp_dir = "./tmp"
medusa_repository = "text-generation-inference/Mixtral-8x7B-Instruct-v0.1-medusa" # Medusa model
# https://huggingface.co/TheBloke/Mixtral-8x7B-Instruct-v0.1-AWQ/discussions/6
llm_repository = "ybelkada/Mixtral-8x7B-Instruct-v0.1-AWQ" # AWQ LLM model
 
snapshot_download(repo_id=medusa_repository, local_dir=tmp_dir)
snapshot_download(repo_id=llm_repository, local_dir=os.path.join(tmp_dir, "llm"),ignore_patterns="*.bin")
 
# rewrite meudsa base model value
with open(os.path.join(tmp_dir, "config.json"), "r") as f:
    data = json.load(f)
    data["base_model_name_or_path"] = "/opt/ml/model/llm/" # path to llm model in side Amazon SageMaker
with open(os.path.join(tmp_dir, "config.json"), "w") as f_out:
    json.dump(data, indent=2, fp=f_out)
 
# upload the model to s3
s3_path = S3Uploader.upload(
    local_path=tmp_dir,
    desired_s3_uri=f"s3://{sess.default_bucket()}/medusa/mixtral"
)
```

## 4\. Deploy Mixtral 8x7B to Amazon SageMaker

After we have prepared the Medusa and AWQ artifacts we can deploy Mixtral 8x7B to Amazon SageMaker. We will use the `HuggingFaceModel` model class and define our endpoint configuration including the `hf_model_id`, `instance_type` etc. We will use a `g5.12xlarge` instance type, which has 4 NVIDIA A10G GPUs and 96GB of GPU memory.

We make sure to have `QUANTIZE` set to `awq` and that `HF_MODEL_ID` is set to `/opt/ml/model`, since SageMaker will load the model from S3 to there.

Python

```python
import json
from sagemaker.huggingface import HuggingFaceModel
 
# sagemaker config
instance_type = "ml.g5.12xlarge"
number_of_gpu = 4
health_check_timeout = 300
 
# Define Model and Endpoint configuration parameter
config = {
  'HF_MODEL_ID': "/opt/ml/model", # path to where sagemaker stores the model
  'SM_NUM_GPUS': json.dumps(number_of_gpu), # Number of GPU used per replica
  'MAX_INPUT_LENGTH': json.dumps(8000),  # Max length of input text
  'MAX_BATCH_PREFILL_TOKENS': json.dumps(16384),  # Number of tokens for the prefill operation.
  'MAX_TOTAL_TOKENS': json.dumps(16384),  # Max length of the generation (including input text)
  'QUANTIZE': "awq" # Quantization method
}
 
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  # path to s3 bucket with model, we are not using a compressed model
  model_data={'S3DataSource':{'S3Uri': s3_path + "/",'S3DataType': 'S3Prefix','CompressionType': 'None'}},
  image_uri=llm_image,
  env=config
)
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the `deploy` method. TGI will automatically distribute and shard the model across all GPUs.

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

## 5\. Run inference and chat with the model

After our endpoint is deployed we can run inference on it. We will use the `predict` method from the `predictor` to run inference on our endpoint. We can inference with different parameters to impact the generation. Parameters can be defined as in the `parameters` attribute of the payload. You can find supported parameters in the [here](https://www.philschmid.de/sagemaker-llama-llm#5-run-inference-and-chat-with-the-model) or in the open api specification of the TGI in the [swagger documentation](https://huggingface.github.io/text-generation-inference/)

The `mistralai/Mixtral-8x7B-Instruct-v0.1` is a conversational chat model meaning we can chat with it using the following prompt:

```plaintext
<s> [INST] User Instruction 1 [/INST] Model answer 1</s> [INST] User instruction 2 [/INST]
```

Lets see, if Mixtral can come up with some cool ideas for the summer.

Python

```python
from transformers import AutoTokenizer
 
tok = AutoTokenizer.from_pretrained("ybelkada/Mixtral-8x7B-Instruct-v0.1-AWQ")
 
# Prompt to generate
messages = [
    { "role": "user", "content": "How can i make a good american cheese cake? Explain it step-by-step and include time estimates." },
]
 
# Generation arguments
payload = {
    "do_sample": True,
    "top_p": 0.6,
    "temperature": 0.9,
    "top_k": 50,
    "max_new_tokens": 1024,
    "repetition_penalty": 1.03,
    "return_full_text": False,
    "stop": ["</s>"]
}
```

Okay lets test it.

Python

```python
chat = llm.predict({
  "inputs":tok.apply_chat_template(messages,tokenize=False,add_generation_prompt=True), # convert messages to model input
  "parameters":payload
})
 
print(chat[0]["generated_text"])
```

Awesome, We have successfully deployed Mixtral 8x7B to Amazon SageMaker with Medusa and AWQ on a `g5.12xlarge` instance. Thats not possible with the regular Mixtral 8x7B model. If we now look up the latency of the model on Cloudwatch we can see that we have a latency of ~50-60ms per token.

## 6\. Clean up

To clean up, we can delete the model and endpoint.

Python

```python
llm.delete_model()
llm.delete_endpoint()
```

## Conclusion

Leveraging Medusa and AWQ allowed us to deploy Mixtral-8x7B on a `g5.12xlarge`, reducing costs by ~3x (from `g5.48xlarge`) and improving latency to 50-60ms per token. This is a great example of how you can go beyond the standard deployment of Hugging Face models on Amazon SageMaker and leverage the power of the Hugging Face LLM DLC to accelerate inference and reduce costs.

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
