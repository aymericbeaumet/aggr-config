---
title: Deploy Idefics 9B and 80B on Amazon SageMaker
link: https://www.philschmid.de/sagemaker-idefics
source: philschmid-de
published: 2023-10-12T00:00:00Z
updated: 2023-10-12T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to deploy Hugging Face Idefics 9B and 80B to Amazon SageMaker and send requests with images and text to the model.
content: extracted
html: 2023-10-12-deploy-idefics-9b-and-80b-on-amazon-sagemaker.html
preview:
  file: 2023-10-12-deploy-idefics-9b-and-80b-on-amazon-sagemaker.preview-cb13d46bff38.webp
  width: 256
  height: 134
  alt: Deploy Idefics 9B & 80B on Amazon SageMaker
  color: '#93bec2'
images:
- source: https://www.philschmid.de/static/blog/sagemaker-idefics/thumbnail.jpg
  original:
    file: 2023-10-12-deploy-idefics-9b-and-80b-on-amazon-sagemaker.image-d01c23eeaf1d.jpg
    width: 2400
    height: 1254
  color: '#c6eae8'
- source: https://www.philschmid.de/static/blog/sagemaker-idefics/guarding_baguettes.png
  original:
    file: 2023-10-12-deploy-idefics-9b-and-80b-on-amazon-sagemaker.image-4ffbfb6e7cb8.png
    width: 1966
    height: 1030
  color: '#f3f7fc'
- source: https://www.philschmid.de/static/blog/sagemaker-idefics/powercord.jpeg
  original:
    file: 2023-10-12-deploy-idefics-9b-and-80b-on-amazon-sagemaker.image-fa0d368de443.jpg
    width: 750
    height: 750
  color: '#fefefe'
---

IDEFICS is an open-access visual language model with 80 billion parameters that can generate text based on sequences of images and text. It was created to reproduce capabilities similar to Deepmind's closed-source Flamingo or Open AI GPT-4V model using only publicly available data and models.

In this blog you will learn how to deploy Idefics model to Amazon SageMaker. We are going to use the Hugging Face LLM DLC is a new purpose-built Inference Container to easily deploy LLMs in a secure and managed environment. The DLC is powered by [Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) a scalelable, optimized solution for deploying and serving Large Language Models (LLMs). The Blog post also includes Hardware requirements for the different model sizes.

In the blog will cover how to:

1. [Setup development environment](https://www.philschmid.de/sagemaker-idefics#1-setup-development-environment)
2. [Retrieve the new Hugging Face LLM DLC](https://www.philschmid.de/sagemaker-idefics#2-retrieve-the-new-hugging-face-llm-dlc)
3. [Hardware requirements](https://www.philschmid.de/sagemaker-idefics#3-hardware-requirements)
4. [Deploy Idefics 80B to Amazon SageMaker](https://www.philschmid.de/sagemaker-idefics#4-deploy-idefics-80b-to-amazon-sagemaker)
5. [Run inference and chat with the model](https://www.philschmid.de/sagemaker-idefics#5-run-inference-and-chat-with-the-model)
6. [Clean up](https://www.philschmid.de/sagemaker-idefics#5-clean-up)

Before we gets started lets a quick look at Idefics

## What is Hugging Face IDEFICS?

IDEFICS (**I**mage-aware **D**ecoder **E**nhanced à la **F**lamingo with **I**nterleaved **C**ross-attention**S**) is an open-access visual language model developed by Hugging Face. It is a reproduction of Flamingo, a closed-source model created by Deepmind.

Like GPT-4V, IDEFICS is a multimodal model that accepts sequences of images and text as input and generates text as output. The model can answer questions about images, describe visual contents, and create stories grounded in multiple images.

IDEFICS comes in two main variants, a 9B and 80B parameter version. Hugging Face also provides fine-tuned versions called [idefics-80b-instruct](https://huggingface.co/HuggingFaceM4/idefics-80b-instruct) and [idefics-9b-instruct](https://huggingface.co/HuggingFaceM4/idefics-9b-instruct) that have been adapted for conversational use cases.

IDEFICS is trained solely on publicly available data like Wikipedia, LAION, and a new 115B token dataset called OBELICS. It is built on top of existing models like `CLIP-ViT-H-14-laion2B-s32B-b79K` and `Llama`.

![guarding_baguettes](https://www.philschmid.de/static/blog/sagemaker-idefics/guarding_baguettes.png)

You can learn more about Idefics in the [Hugging Face blog post](https://huggingface.co/blog/idefics).

## 1\. Setup development environment

We are going to use the `sagemaker` python SDK to deploy Idefics to Amazon SageMaker. We need to make sure to have an AWS account configured and the `sagemaker` python SDK installed.

Python

```python
!pip install "sagemaker>=2.192.0" --upgrade --quiet
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

## 3\. Hardware requirements

Idefics comes in 2 different sizes - 9B & 80B parameters. The hardware requirements will vary based on the model size deployed to SageMaker. Below is a set up minimum requirements for each model size we tested.

*Note: We haven't tested GPTQ models yet.*

| Model                                                            | Instance Type       | Quantization   | \# of GPUs per replica |
| ---------------------------------------------------------------- | ------------------- | -------------- | ---------------------- |
| [Idefics 9B](https://huggingface.co/HuggingFaceM4/idefics-9b)   | `(ml.)g5.12xlarge`  | `-`            | 4                      |
| [Idefics 80B](https://huggingface.co/HuggingFaceM4/idefics-80b) | `(ml.)g5.48xlarge`  | `bitsandbytes` | 8                      |
| [Idefics 80B](https://huggingface.co/HuggingFaceM4/idefics-80b) | `(ml.)p4d.24xlarge` | `-`            | 8                      |

*Note: Amazon SageMaker currently doesn't support instance slicing meaning, e.g. for Idefics 80B you cannot run multiple replica on a single instance.*

These are the setups we have validated for Idefics instruct 9B and 80B models to work on SageMaker.

## 4\. Deploy Idefics 80B to Amazon SageMaker

To deploy [HuggingFaceM4/idefics-80b-instruct](https://huggingface.co/HuggingFaceM4/idefics-80b-instruct) to Amazon SageMaker we create a `HuggingFaceModel` model class and define our endpoint configuration including the `hf_model_id`, `instance_type` etc. We will use a `g5.12xlarge` instance type, which has 4 NVIDIA A10G GPUs and 96GB of GPU memory.

Python

```python
import json
from sagemaker.huggingface import HuggingFaceModel
 
# sagemaker config
instance_type = "ml.p4d.24xlarge"
number_of_gpu = 8
health_check_timeout = 300
 
# Define Model and Endpoint configuration parameter
config = {
  'HF_MODEL_ID': "HuggingFaceM4/idefics-80b-instruct", # model_id from hf.co/models
  'SM_NUM_GPUS': json.dumps(number_of_gpu), # Number of GPU used per replica
  'MAX_INPUT_LENGTH': json.dumps(1024),  # Max length of input text
  'MAX_TOTAL_TOKENS': json.dumps(2048),  # Max length of the generation (including input text)
  'MAX_BATCH_TOTAL_TOKENS': json.dumps(8192),  # Limits the number of tokens that can be processed in parallel during the generation
  # ,'HF_MODEL_QUANTIZE': "bitsandbytes", # comment in to quantize
}
 
# create HuggingFaceModel with the image uri
llm_model = HuggingFaceModel(
  role=role,
  image_uri=llm_image,
  env=config
)
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the `deploy` method. We will deploy the model with the `ml.g5.12xlarge` instance type. TGI will automatically distribute and shard the model across all GPUs.

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

After our endpoint is deployed we can run inference on it. We will use the `predict` method from the `predictor` to run inference on our endpoint. We can inference with different parameters to impact the generation. Parameters can be defined as in the `parameters` attribute of the payload. You can find a full list of paramters in the [documentation](https://huggingface.github.io/text-generation-inference/) at the bottom in the GenerateParameters object.

The `HuggingFaceM4/idefics-80b-instruct` is a instruction tuned model meaning we can instruct with it using the following prompt:

```plaintext
User:<fake_token_around_image><image><fake_token_around_image>{in_context_prompt}<end_of_utterance>\n
Assistant: {in_context_answer}<end_of_utterance>\n
User:<fake_token_around_image><image><fake_token_around_image>{prompt}<end_of_utterance>\n
Assistant:
```

More [here](https://github.com/huggingface/transformers/issues/25803) for TGI we currenlty need to add the `<image>` as "markdown" url with `![](https://t4.ftcdn.net/jpg/00/97/58/97/360_F_97589769_t45CqXyzjz0KXwoBZT9PRaWGHRk5hQqQ.jpg)`. In the version 1.1.0 of TGI we can only provide urls to images. To run secure inference and use local image we will upload those to s3 and make them available via signed urls and delete them after inference again. Therefore we create a helper method `run_inference`, which accepts our prompt and an path to an image. The image will be uploaded to s3 and a signed url will be created. We will then run inference on our endpoint and delete the image again.

Python

```python
from botocore.client import Config
 
s3 = sess.boto_session.client('s3', config=Config(signature_version='s3v4'))
 
prompt_template="User:{prompt}![]({image})<end_of_utterance>\nAssistant:"
parameters = {
    "do_sample": True,
    "top_p": 0.2,
    "temperature": 0.4,
    "top_k": 50,
    "max_new_tokens": 512,
    "stop": ["User:","<end_of_utterance>"]
  }
 
def run_inference(prompt=None,image_path=None):
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
    # create prompt with image and text
    parsed_prompt = prompt_template.format(image=url,prompt=prompt)
 
    # Make calls the endpoint including the prompt and parameters
    chat = llm.predict({"inputs":parsed_prompt,"parameters":parameters})
 
    # Delete image from S3
    s3.delete_object(Bucket=bucket, Key=key)
 
    # return the generated text
    return chat[0]["generated_text"][len(parsed_prompt):].strip()
```

Lets get an image from internet we can use to run a request. We will use the following image:

![powercord](https://www.philschmid.de/static/blog/sagemaker-idefics/powercord.jpeg)

Lets ask if i can use this power cord in the U.S.

Python

```python
prompt = "Can i use the cable in the U.S.?"
 
# run inference
res = run_inference(prompt=prompt,image_path="../assets/powercord.jpeg")
 
print(res)
# No, the cable is not compatible with U.S. outlets. It has a European plug, which has two round prongs, and it is not compatible with the standard U.S. outlets that have two flat prongs.
```

Thats correct the cable is a European cable and not suitable for the U.S.

## 6\. Clean up

To clean up, we can delete the model and endpoint.

Python

```python
llm.delete_model()
llm.delete_endpoint()
```

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
