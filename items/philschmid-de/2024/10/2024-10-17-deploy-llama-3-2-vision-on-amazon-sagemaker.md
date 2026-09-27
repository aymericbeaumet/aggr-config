---
title: Deploy Llama 3.2 Vision on Amazon SageMaker
link: https://www.philschmid.de/sagemaker-llama32-vision
source: philschmid-de
published: 2024-10-17T00:00:00Z
updated: 2024-10-17T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to deploy Llama 3.2 Vision on Amazon SageMaker and run inference with it.
content: extracted
html: 2024-10-17-deploy-llama-3-2-vision-on-amazon-sagemaker.html
preview:
  file: 2024-10-17-deploy-llama-3-2-vision-on-amazon-sagemaker.preview-d660786ae7de.webp
  width: 256
  height: 167
  alt: Deploy Llama 3.2 Vision on Amazon SageMaker
  color: '#b3b4b2'
images:
- source: https://www.philschmid.de/static/blog/sagemaker-llama32-vision/thumbnail.jpg
  original:
    file: 2024-10-17-deploy-llama-3-2-vision-on-amazon-sagemaker.image-ee1aa1129852.jpg
    width: 1200
    height: 785
  color: '#fcfdfd'
- source: https://huggingface.co/datasets/huggingface/release-assets/resolve/main/invoice.png
  original:
    file: 2024-10-17-deploy-llama-3-2-vision-on-amazon-sagemaker.image-012a7e22e005.png
    width: 710
    height: 582
  color: '#fcfcfc'
---

[Llama 3.2](https://huggingface.co/blog/llama32) is the latest release of open LLMs from the Llama family released by Meta (as of October 2024); Llama 3.2 Vision comes in two sizes: 11B for efficient deployment and development on consumer-size GPU, and 90B for large-scale applications.

In this blog you will learn how to deploy [meta-llama/Llama-3.2-11B-Vision-Instruct](https://huggingface.co/meta-llama/Llama-3.2-11B-Vision-Instruct) to Amazon SageMaker. We are going to use the Hugging Face LLM DLC is a purpose-built Inference Container to easily deploy LLMs in a secure and managed environment. The DLC is powered by [Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) a scalelable, optimized solution for deploying and serving Large Language Models (LLMs). The Blog post also includes Hardware requirements for the different model sizes.

> Regarding the licensing terms, Llama 3.2 comes with a very similar license to Llama 3.1, with one key difference in the acceptable use policy: any individual domiciled in, or a company with a principal place of business in, the European Union (EU) is not being granted the license rights to use multimodal models included in Llama 3.2. This restriction does not apply to end users of a product or service that incorporates any such multimodal models, so people can still build global products with the vision variants.
>
> For full details, please make sure to read [the official license](https://huggingface.co/meta-llama/Llama-3.2-1B/blob/main/LICENSE.txt) and [the acceptable use policy](https://huggingface.co/meta-llama/Llama-3.2-1B/blob/main/USE_POLICY.md).

In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/sagemaker-llama32-vision#1-setup-development-environment)
2. [Retrieve the new Hugging Face LLM DLC](https://www.philschmid.de/sagemaker-llama32-vision#2-retrieve-the-new-hugging-face-llm-dlc)
3. [Hardware requirements](https://www.philschmid.de/sagemaker-llama32-vision#3-hardware-requirements)
4. [Deploy Llama 3.2 11B to Amazon SageMaker](https://www.philschmid.de/sagemaker-llama32-vision#4-deploy-llama-32-11b-to-amazon-sagemaker)
5. [Run inference and chat with the model](https://www.philschmid.de/sagemaker-llama32-vision#5-run-inference-and-chat-with-the-model)
6. [Clean up](https://www.philschmid.de/sagemaker-llama32-vision#5-clean-up)

Lets get started!

## 1\. Setup development environment

We are going to use the `sagemaker` python SDK to deploy Llama 3.2 Vision to Amazon SageMaker. We need to make sure to have an AWS account configured and the `sagemaker` python SDK installed.

Python

```python
# TODO: Update version when image included 
!pip install "sagemaker>=2.232.2" --upgrade --quiet
 
# install huggingface hub
!pip install huggingface_hub --quiet
 
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
# TODO: COMMENT IN WHEN IMAGE IS RELEASED
# from sagemaker.huggingface import get_huggingface_llm_image_uri
 
# # retrieve the llm image uri
# llm_image = get_huggingface_llm_image_uri(
#   "huggingface",
#   version="2.3.1"
# )
 
# # print ecr image uri
# print(f"llm image uri: {llm_image}")
 
llm_image = f"763104351884.dkr.ecr.{sess.boto_region_name}.amazonaws.com/huggingface-pytorch-tgi-inference:2.4-tgi2.3-gpu-py311-cu124-ubuntu22.04"
 
```

## 3\. Hardware requirements

Llama 3.2 comes in 2 different sizes - 11B & 90B parameters. The hardware requirements will vary based on the model size deployed to SageMaker. Below is a set up minimum requirements for each model size we tested.

| Model                                                                                | Instance Type        | \# of GPUs per replica |
| ------------------------------------------------------------------------------------ | -------------------- | ---------------------- |
| [Llama 3.2 11B](https://www.philschmid.de/meta-llama/Llama-3.2-11B-Vision-Instruct) | `(ml.)g5/6.12xlarge` | 4                      |
| [Llama 3.2 90B](https://www.philschmid.de/meta-llama/Llama-3.2-90B-Vision-Instruct) | `(ml.)g6e.48xlarge`  | 8                      |
| [Llama 3.2 90B](https://www.philschmid.de/meta-llama/Llama-3.2-90B-Vision-Instruct) | `(ml.)p4d.24xlarge`  | 8                      |

*Note: Amazon SageMaker currently doesn't support instance slicing meaning, e.g. for Llama 3.2 90B you cannot run multiple replica on a single instance.*

These are the setups we have validated for Llama 3.2 11B and 90B models to work on SageMaker.

## 4\. Deploy Llama 3.2 11B to Amazon SageMaker

To deploy [meta-llama/Llama-3.2-11B-Vision-Instruct](https://huggingface.co/meta-llama/Llama-3.2-11B-Vision-Instruct) to Amazon SageMaker we create a `HuggingFaceModel` model class and define our endpoint configuration including the `hf_model_id`, `instance_type` etc. We will use a `g6.12xlarge` instance type, which has 4 NVIDIA L4 GPUs and 96GB of GPU memory.

As [meta-llama/Llama-3.2-11B-Vision-Instruct](https://huggingface.co/meta-llama/Llama-3.2-11B-Vision-Instruct) is a gated model with restricted access on the European Union (EU), meaning that you need to accept the license agreement.

To generate a token for the Hugging Face Hub, you can follow the instructions in [Hugging Face Hub - User access tokens](https://huggingface.co/docs/hub/en/security-tokens); the generated token can either be fine-grained to have access to the model, or just overall read-only access to your account.

Python

```python
from huggingface_hub import interpreter_login
 
interpreter_login()
```

After we are logged in we can create our HuggingFaceModel.

Python

```python
import json
from sagemaker.huggingface import HuggingFaceModel
from huggingface_hub import get_token
 
# sagemaker config
instance_type = "ml.g6.12xlarge"
number_of_gpu = 4
health_check_timeout = 300
 
# Define Model and Endpoint configuration parameter
config = {
  'HF_MODEL_ID': "meta-llama/Llama-3.2-11B-Vision-Instruct", # model_id from hf.co/models
  'SM_NUM_GPUS': json.dumps(number_of_gpu), # Number of GPU used per replica
  'MAX_INPUT_LENGTH': json.dumps(6000),  # Max length of input text
  'MAX_TOTAL_TOKENS': json.dumps(8192),  # Max length of the generation (including input text)
  'HF_HUB_ENABLE_HF_TRANSFER': "1", # Enable HF transfer for faster downloads
  'HUGGING_FACE_HUB_TOKEN': get_token(), # Hugging Face token
  'MESSAGES_API_ENABLED': "true", # Enable messages API
}
 
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  image_uri=llm_image,
  env=config
)
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the `deploy` method. We will deploy the model with the `ml.g6.12xlarge` instance type. TGI will automatically distribute and shard the model across all GPUs.

Python

```python
# Deploy model to an endpoint
llm = llm_model.deploy(
  initial_instance_count=1,
  instance_type=instance_type,
  container_startup_health_check_timeout=health_check_timeout, # 10 minutes to be able to load the model
)
 
```

SageMaker will now create our endpoint and deploy the model to it. This can takes a 10-15 minutes.

## 5\. Run inference and chat with the model

After our endpoint is deployed we can run inference on it. We will use the `predict` method from the `predictor` to run inference on our endpoint. We deployed Llama 3.2 Vision with `MESSAGES_API_ENABLED=true` which allows us to use the OpenAI compatible messages API. This allows us to include "type" "image\_url", which can be a link to an image or a base64 encoded image. To keep things realistic we are going to upload an image to s3 and create a pre-signed url to it, which we will use in our inference request. Thats how you could handle images in a real-world application.

Python

```python
from botocore.client import Config
 
s3 = sess.boto_session.client('s3', config=Config(signature_version='s3v4'))
 
def upload_image(image_path):
    # params
    bucket = sess.default_bucket()
    key = os.path.join("input", os.path.basename(image_path))
    
    # Upload image to S3    
    s3.upload_file(image_path, bucket, key)
 
    # Generate pre-signed URL valid for 5 minutes
    url = s3.generate_presigned_url(
        ClientMethod='get_object', 
        Params={'Bucket': bucket, 'Key': key},
        ExpiresIn=300
    )
    return url
 
messages = [
    {
        "role": "user",
        "content": [
            {"type": "text", "text": "How long does it take from invoice date to due date? Be short and concise."},
            {
                "type": "image_url",
                "image_url": {
                    "url": upload_image("../assets/invoice.png")
                },
            },
        ],
    },
]
 
# Make calls the endpoint including the prompt and parameters
chat = llm.predict({
  "messages":messages,
  "max_tokens": 512,
#   "do_sample": True,
  "top_p": 0.95,
  "temperature": 1.0,
  "stream": False,
})
 
print(chat["choices"][0]["message"]["content"])
 
```

![Invoice Image](https://huggingface.co/datasets/huggingface/release-assets/resolve/main/invoice.png)

To calculate the time difference between the invoice date and the due date, we need to subtract the invoice date from the due date.

Invoice Date: 11/02/2019 Due Date: 26/02/2019

Time Difference = Due Date - Invoice Date Time Difference = 26/02/2019 - 11/02/2019 Time Difference = 15 days

Therefore, it takes **15 days** from the invoice date to the due date.

## 6\. Clean up

To clean up, we can delete the model and endpoint.

Python

```python
llm.delete_model()
llm.delete_endpoint()
```

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
