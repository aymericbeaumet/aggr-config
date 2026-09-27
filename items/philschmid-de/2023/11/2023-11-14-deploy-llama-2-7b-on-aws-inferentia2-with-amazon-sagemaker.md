---
title: Deploy Llama 2 7B on AWS inferentia2 with Amazon SageMaker
link: https://www.philschmid.de/inferentia2-llama-7b
source: philschmid-de
published: 2023-11-14T00:00:00Z
updated: 2023-11-14T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post, you will learn how to compile and deploy Llama 2 7B on AWS Inferentia2 with Amazon SageMaker.
content: extracted
html: 2023-11-14-deploy-llama-2-7b-on-aws-inferentia2-with-amazon-sagemaker.html
preview:
  file: 2023-11-14-deploy-llama-2-7b-on-aws-inferentia2-with-amazon-sagemaker.preview-d40a788800e5.webp
  width: 256
  height: 134
  alt: Deploy Llama 2 7B on AWS inferentia2 with Amazon SageMaker
  color: '#a2bce2'
images:
- source: https://www.philschmid.de/static/blog/inferentia2-llama-7b/thumbnail.jpg
  original:
    file: 2023-11-14-deploy-llama-2-7b-on-aws-inferentia2-with-amazon-sagemaker.image-052648659cc1.jpg
    width: 2400
    height: 1254
  color: '#cac4fc'
---

In this end-to-end tutorial, you will learn how to deploy and speed up Llama 2 inference using AWS Inferentia2 and [optimum-neuron](https://huggingface.co/docs/optimum-neuron/index) on Amazon SageMaker. [Optimum Neuron](https://huggingface.co/docs/optimum-neuron/index) is the interface between the Hugging Face Transformers & Diffusers library and AWS Accelerators including AWS Trainium and AWS Inferentia2.

You will learn how to:

1. [Convert Llama 2 to AWS Neuron (Inferentia2) with `optimum-neuron`](https://www.philschmid.de/inferentia2-llama-7b#1-convert-llama-2-to-aws-neuron-inferentia2-with-optimum-neuron)
2. [Create a custom `inference.py` script for Llama 2](https://www.philschmid.de/inferentia2-llama-7b#2-create-a-custom-inferencepy-script-for-llama-2-7b)
3. [Upload the neuron model and inference script to Amazon S3](https://www.philschmid.de/inferentia2-llama-7b#3-upload-the-neuron-model-and-inference-script-to-amazon-s3)
4. [Deploy a Real-time Inference Endpoint on Amazon SageMaker](https://www.philschmid.de/inferentia2-llama-7b#4-deploy-a-real-time-inference-endpoint-on-amazon-sagemaker)
5. [Run inference and chat with Llama 2](https://www.philschmid.de/inferentia2-llama-7b#5-run-inference-and-chat-with-llama-2)

## Quick intro: AWS Inferentia 2

[AWS inferentia (Inf2)](https://aws.amazon.com/de/ec2/instance-types/inf2/) are purpose-built EC2 for deep learning (DL) inference workloads. Inferentia 2 is the successor of [AWS Inferentia](https://aws.amazon.com/ec2/instance-types/inf1/?nc1=h_ls), which promises to deliver up to 4x higher throughput and up to 10x lower latency.

| instance size | accelerators | Neuron Cores | accelerator memory | vCPU | CPU Memory | on-demand price ($/h) |
| ------------- | ------------ | ------------ | ------------------ | ---- | ---------- | --------------------- |
| inf2.xlarge   | 1            | 2            | 32                 | 4    | 16         | 0.76                  |
| inf2.8xlarge  | 1            | 2            | 32                 | 32   | 128        | 1.97                  |
| inf2.24xlarge | 6            | 12           | 192                | 96   | 384        | 6.49                  |
| inf2.48xlarge | 12           | 24           | 384                | 192  | 768        | 12.98                 |

Additionally, inferentia 2 will support the writing of custom operators in c++ and new datatypes, including `FP8` (cFP8).

Let's get started! 🚀

*If you are going to use Sagemaker in a local environment (not SageMaker Studio or Notebook Instances). You need access to an IAM Role with the required permissions for Sagemaker. You can find [here](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) more about it.*

## 1\. Convert Llama 2 to AWS Neuron (Inferentia2) with `optimum-neuron`

We are going to use the [optimum-neuron](https://huggingface.co/docs/optimum-neuron/index) to compile/convert our model to neuronx. Optimum Neuron provides a set of tools enabling easy model loading, training and inference on single- and multi-Accelerator settings for different downstream tasks.

As a first step, we need to install the `optimum-neuron` and other required packages.

*Tip: If you are using Amazon SageMaker Notebook Instances or Studio you can go with the `conda_python3` conda kernel.*

Python

```python
# Install the required packages
%pip install "optimum-neuron==0.0.13" --upgrade
%pip install "sagemaker>=2.197.0"  --upgrade
```

After we have installed the `optimum-neuron` we can convert load and convert our model.

We are going to use the [meta-llama/Llama-2-7b-chat-hf](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf) model. Llama 2 is a collection of pretrained and fine-tuned generative text models ranging in scale from 7 billion to 70 billion parameters. This is the repository for the 7B fine-tuned model, optimized for dialogue use cases.

At the time of writing, the [AWS Inferentia2 does not support dynamic shapes for inference](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/general/arch/neuron-features/dynamic-shapes.html?highlight=dynamic%20shapes#), which means that the we need to specify our image size in advanced for compiling and inference.

In simpler terms, this means we need to define the input shapes for our prompt (sequence length), batch size, height and width of the image.

We precompiled the model with the following parameters and pushed it to the Hugging Face Hub:

- `sequence_length`: 2048
- `batch_size`: 2
- `neuron`: 2.15.0

*Note: If you want to compile your own model or a different Llama 2 checkpoint you need to use ~120GB of memory and the compilation can take ~60 minutes. We used an `inf2.24xlarge` ec2 instance with the [Hugging Face Neuron Deep Learning AMI](https://aws.amazon.com/marketplace/pp/prodview-gr3e6yiscria2) to compile the model.*

Python

```python
from huggingface_hub import snapshot_download
 
# compiled model id
compiled_model_id = "aws-neuron/Llama-2-7b-chat-hf-seqlen-2048-bs-2"
 
# save compiled model to local directory
save_directory = "llama_neuron"
# Downloads our compiled model from the HuggingFace Hub
# using the revision as neuron version reference
# and makes sure we exlcude the symlink files and "hidden" files, like .DS_Store, .gitignore, etc.
snapshot_download(compiled_model_id, revision="2.15.0", local_dir=save_directory, local_dir_use_symlinks=False, allow_patterns=["[!.]*.*"])
 
 
###############################################
# COMMENT IN BELOW TO COMPILE DIFFERENT MODEL #
###############################################
#
# from optimum.neuron import NeuronModelForCausalLM
# from transformers import AutoTokenizer
#
# # model id you want to compile
# vanilla_model_id = "meta-llama/Llama-2-7b-chat-hf"
#
# # configs for compiling model
# compiler_args = {"num_cores": 2, "auto_cast_type": "fp16"}
# input_shapes = {
#   "sequence_length": 2048, # max length to generate
#   "batch_size": 1 # batch size for the model
#   }
#
# llm = NeuronModelForCausalLM.from_pretrained(vanilla_model_id, export=True, **input_shapes, **compiler_args)
# tokenizer = AutoTokenizer.from_pretrained(model_id)
#
# # Save locally or upload to the HuggingFace Hub
# save_directory = "llama_neuron"
# llm.save_pretrained(save_directory)
# tokenizer.save_pretrained(save_directory)
```

## 2\. Create a custom `inference.py` script for Llama 2 7B

The [Hugging Face Inference Toolkit](https://github.com/aws/sagemaker-huggingface-inference-toolkit) supports zero-code deployments on top of the [pipeline feature](https://huggingface.co/transformers/main_classes/pipelines.html) from 🤗 Transformers. This allows users to deploy Hugging Face transformers without an inference script \[[Example](https://github.com/huggingface/notebooks/blob/master/sagemaker/11_deploy_model_from_hf_hub/deploy_transformer_model_from_hf_hub.ipynb)\].

Currently is this feature not supported with AWS Inferentia2, which means we need to provide an `inference.py` for running inference. But `optimum-neuron` has integrated support for the 🤗 Diffusers pipeline feature. That way we can use the `optimum-neuron` to create a pipeline for our model.

If you want to know more about the `inference.py` script check out this [example](https://github.com/huggingface/notebooks/blob/master/sagemaker/17_custom_inference_script/sagemaker-notebook.ipynb). It explains amongst other things what the `model_fn` and `predict_fn` are.

Python

```python
# create code directory in our model directory
!mkdir {save_directory}/code
```

We are using the `NEURON_RT_NUM_CORES=2` to make sure that each HTTP worker uses 2 Neuron core for inference. In additon we are going to use ["templates for chat models"](https://huggingface.co/docs/transformers/main/en/chat_templating) and new feature of transformers, which allows us to provide OpenAI messages, which are then converted to the correct input format for the model.

Python

```python
messages = [
    {"role": "user", "content": "Hi there!"},
    {"role": "assistant", "content": "Nice to meet you!"},
    {"role": "user", "content": "Can I ask a question?"}
]
```

For this to work we need `jinja2` installed. Lets create a `requirements.txt` file and install the required packages.

Python

```python
%%writefile {save_directory}/code/requirements.txt
 
jinja2>="3.0.0"
```

Now, we create our `inference.py` file using the `apply_chat_template` method.

Python

```python
%%writefile {save_directory}/code/inference.py
import os
# To use two neuron core per worker
os.environ["NEURON_RT_NUM_CORES"] = "2"
import torch
import torch_neuronx
import base64
from io import BytesIO
from optimum.neuron import pipeline
 
def model_fn(model_dir):
    # load local converted model and tokenizer
    print("loading pipeline...")
    pipe = pipeline("text-generation", model_dir)
    print("loading complete")
    return pipe
 
 
def predict_fn(data, pipe):
    # extract prompt from data
    messages = data.pop("inputs", data)
    parameters = data.pop("parameters", None)
 
    # convert messages to input ids
    inputs = pipe.tokenizer.apply_chat_template(messages, add_generation_prompt=True,tokenize=False)
    # run generation
    if parameters is not None:
        outputs = pipe(inputs, **parameters)[0]
    else:
        outputs = pipe(inputs)[0]
 
    # decode generation
    return {"generation": outputs["generated_text"][len(inputs):].strip()}
```

## 3\. Upload the neuron model and inference script to Amazon S3

Before we can deploy our neuron model to Amazon SageMaker we need to upload it all our model artifacts to Amazon S3.

*Note: Currently `inf2` instances are only available in the `us-east-2` & `us-east-1` region \[[REF](https://aws.amazon.com/de/about-aws/whats-new/2023/05/sagemaker-ml-inf2-ml-trn1-instances-model-deployment/)\]. Therefore we need to force the region to us-east-2.*

Lets create our SageMaker session and upload our model to Amazon S3.

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
print(f"sagemaker bucket: {sess.default_bucket()}")
print(f"sagemaker session region: {sess.boto_region_name}")
assert sess.boto_region_name in ["us-east-2", "us-east-1"] , "region must be us-east-2 or us-west-2, due to instance availability"
```

We create our `model.tar.gz` with our `inference.py` script.

*Note: We will use `pigz` for multi-core compression to speed up the process. Make sure `pigz` is installed on your system, you can install it on ubuntu with `sudo apt install pigz`. With `pigz` and 32 cores compression takes ~2.4min*

Python

```python
# create a model.tar.gz archive with all the model artifacts and the inference.py script.
%cd {save_directory}
!tar -cf model.tar.gz --use-compress-program=pigz *
%cd ..
```

Next, we upload our `model.tar.gz` to Amazon S3 using our session bucket and `sagemaker` sdk.

Python

```python
from sagemaker.s3 import S3Uploader
 
# create s3 uri
s3_model_path = f"s3://{sess.default_bucket()}/neuronx/llama"
 
# upload model.tar.gz
s3_model_uri = S3Uploader.upload(local_path=f"{save_directory}/model.tar.gz", desired_s3_uri=s3_model_path)
print(f"model artifcats uploaded to {s3_model_uri}")
```

## 4\. Deploy a Real-time Inference Endpoint on Amazon SageMaker

After we have uploaded our model artifacts to Amazon S3 can we create a custom `HuggingfaceModel`. This class will be used to create and deploy our real-time inference endpoint on Amazon SageMaker.

The `inf2.xlarge` instance type is the smallest instance type with AWS Inferentia2 support. It comes with 1 Inferentia2 chip with 2 Neuron Cores. This means we can use 2 Neuron Cores to minimize latency for our image generation.

Python

```python
import time
from sagemaker.huggingface.model import HuggingFaceModel
 
# create Hugging Face Model Class
huggingface_model = HuggingFaceModel(
   model_data=s3_model_uri,        # path to your model.tar.gz on s3
   role=role,                      # iam role with permissions to create an Endpoint
   transformers_version="4.34.1",  # transformers version used
   pytorch_version="1.13.1",       # pytorch version used
   py_version='py310',             # python version used
   model_server_workers=1,         # number of workers for the model server
)
 
# deploy the endpoint endpoint
predictor = huggingface_model.deploy(
    initial_instance_count=1,      # number of instances
    instance_type="ml.inf2.8xlarge", # AWS Inferentia Instance
    volume_size = 100
)
# ignore the "Your model is not compiled. Please compile your model before using Inferentia." warning, we already compiled our model.
# We need to sent a warmup request to the endpoint, which loads the model on the neuron device
# this takes around 2 minutes
print("send warmup request")
try:
    predictor.predict({"inputs": [{"role":"user","content":"warmup"}]})
except:
    time.sleep(90)
```

## 5\. Run inference and chat with Llama 2

The `.deploy()` returns an `HuggingFacePredictor` object which can be used to request inference. Our endpoint expects a `json` with `messages`. Since we are leveraging the new [apply\_chat\_template](https://huggingface.co/docs/transformers/main/en/internal/tokenization_utils#transformers.PreTrainedTokenizerBase.apply_chat_template) in our inference.py script we can send "openai" like converstaions to our model.

Additionally we can send inference parameters, e.g. `top_p` or `temperature` using the `parameters` key.

Python

```python
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
    "max_new_tokens": 512,
    "repetition_penalty": 1.03,
}
 
# run prediction
response = predictor.predict({
  "inputs": messages,
  "parameters": parameters
  }
)
 
# lets our response to the messages and print the generation
messages.append({"role": "assistant", "content": response["generation"]})
 
# small helper function to print the messages
def pretty_print(messages):
    for message in messages:
        print(f"{message['role']}: {message['content']}")
 
pretty_print(messages)
```

Since Llama is a conversational model lets ask a follow up question. Therefore we can extend our `messages` with a new message.

Python

```python
# add follow up question
messages.append({"role": "user", "content": "Can I run Hugging Face Transformers on it?"})
 
# run prediction
response = predictor.predict({
  "inputs": messages,
  "parameters": parameters
  }
)
 
# lets our response to the messages and print the generation
messages.append({"role": "assistant", "content": response["generation"]})
pretty_print(messages)
```

Result:

**system**: You are an helpful AWS Expert Assistant. Respond only with 1-2 sentences.\
 **user**: What is Amazon SageMaker?\
 **assistant**: Amazon SageMaker is a fully managed service that provides a range of machine learning (ML) algorithms, tools, and frameworks to build, train, and deploy ML models at scale. It allows data scientists and engineers to focus on building better ML models instead of managing infrastructure.\
 **user**: Can I run Hugging Face Transformers on it?\
 **assistant**: Yes, you can run Hugging Face Transformers on Amazon SageMaker. Amazon SageMaker provides a pre-built Python SDK that supports popular deep learning frameworks like Hugging Face Transformers, making it easy to use these frameworks in your machine learning workflows.

If you are interested in the performance of Inferentia2 for throughput and latency check out [Make your llama generation time fly with AWS Inferentia2](https://huggingface.co/blog/inferentia-llama2) blog post.

### Delete model and endpoint

To clean up, we can delete the model and endpoint.

Python

```python
predictor.delete_model()
predictor.delete_endpoint()
```

## Conclusion

In this end-to-end tutorial, we walked through deploying Llama 2, a large conversational AI model, for low-latency inference using AWS Inferentia2 and Amazon SageMaker.

We converted the model with `optimum-neuron`, created a custom inference script, deployed a real-time endpoint, and chatted with Llama 2 using Inferentia2 acceleration.

If you are interested in the performance of Inferentia2 for throughput and latency check out [Make your llama generation time fly with AWS Inferentia2](https://huggingface.co/blog/inferentia-llama2) blog post.

The combination of large AI models like Llama 2 and purpose-built inference chips like Inferentia2 enables low-latency deployments. Using Amazon SageMaker we can go from training to production hosting with just a few lines of code.

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
