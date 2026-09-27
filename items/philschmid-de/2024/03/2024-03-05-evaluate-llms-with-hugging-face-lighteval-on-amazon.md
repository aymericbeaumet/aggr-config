---
title: Evaluate LLMs with Hugging Face Lighteval on Amazon SageMaker
link: https://www.philschmid.de/sagemaker-evaluate-llm-lighteval
source: philschmid-de
published: 2024-03-05T00:00:00Z
updated: 2024-03-05T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to evaluate LLMs using Hugging Face lighteval on Amazon SageMaker.
content: extracted
html: 2024-03-05-evaluate-llms-with-hugging-face-lighteval-on-amazon.html
preview:
  file: 2024-03-05-evaluate-llms-with-hugging-face-lighteval-on-amazon.preview-50a2bf8f214d.webp
  width: 256
  height: 128
  alt: Evaluate LLMs with Hugging Face Lighteval on Amazon SageMaker
  color: '#565960'
images:
- source: https://www.philschmid.de/static/blog/sagemaker-evaluate-llm-lighteval/thumbnail.jpg
  original:
    file: 2024-03-05-evaluate-llms-with-hugging-face-lighteval-on-amazon.image-73bfec363d2f.jpg
    width: 1200
    height: 600
  color: '#07080b'
---

March 5, 2024 6 minute read [View Code](https://github.com/huggingface/notebooks/blob/main/sagemaker/30_evaluate_llms_with_lighteval/sagemaker-notebook.ipynb)

In this sagemaker example, we are going to learn how to evaluate LLMs using Hugging Face [lighteval](https://github.com/huggingface/lighteval/tree/main). LightEval supports the evaluation suite used in [Hugging Face Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard).

Evaluating LLMs is crucial for understanding their capabilities and limitations, yet it poses significant challenges due to their complex and opaque nature. LightEval facilitates this evaluation process by enabling LLMs to be assessed on acamedic benchmarks like MMLU or IFEval, providing a structured approach to gauge their performance across diverse tasks.

In Detail you will learn how to:

1. [Setup Development Environment](https://www.philschmid.de/sagemaker-evaluate-llm-lighteval#1-setup-development-environment)
2. [Prepare the evaluation configuraiton](https://www.philschmid.de/sagemaker-evaluate-llm-lighteval#2-prepare-the-evaluation-configuraiton)
3. [Evaluate Zephyr 7B on TruthfulQA on Amazon SageMaker](https://www.philschmid.de/sagemaker-evaluate-llm-lighteval#3-evaluate-zephyr-7b-on-truthfulqa-on-amazon-sagemaker)

Lets get started.

## 1\. Setup Development Environment

Python

```python
!pip install sagemaker --upgrade --quiet
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
print(f"sagemaker bucket: {sess.default_bucket()}")
print(f"sagemaker session region: {sess.boto_region_name}")
 
```

## 2\. Prepare the evaluation configuraiton

[LightEval](https://github.com/huggingface/lighteval/tree/main) includes script to evaluate LLMs on common benchmarks like MMLU, Truthfulqa, IFEval, and more. Lighteval was inspired by the [Eleuther AI Harness](https://github.com/EleutherAI/lm-evaluation-harness) which is used to evaluate models on the [Hugging Face Open LLM Leaderboard](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard).

You can find all available benchmarks [here](https://github.com/huggingface/lighteval/blob/main/tasks_examples/all_tasks.txt).

We are going to use Amazon SageMaker Managed Training to evaluate the model. Therefore we will leverage the script available in [lighteval](https://github.com/huggingface/lighteval/blob/main/run_evals_accelerate.py). The Hugging Face DLC is not having lighteval installed. This means need to provide a `requirements.txt` file to install the required dependencies.

First lets load the `run_evals_accelerate.py` script and create a `requirements.txt` file with the required dependencies.

Python

```python
import os
import requests as r
 
lighteval_version = "0.2.0"
 
# create scripts directory if not exists
os.makedirs("scripts", exist_ok=True)
 
# load custom scripts from git
raw_github_url = f"https://raw.githubusercontent.com/huggingface/lighteval/v{lighteval_version}/run_evals_accelerate.py"
res = r.get(raw_github_url)
with open("scripts/run_evals_accelerate.py", "w") as f:
    f.write(res.text)
 
# write requirements.txt
with open("scripts/requirements.txt", "w") as f:
    f.write(f"lighteval=={lighteval_version}")
 
```

In lighteval, the evaluation is done by running the `run_evals_accelerate.py` script. The script takes a `task` argument which is defined as `suite|task|num_few_shot|{0 or 1 to automatically reduce num_few_shot if prompt is too long}`. Alternatively, you can also provide a path to a txt file with the tasks you want to evaluate the model on, which we are going to do. This makes it easier for you to extend the evaluation to other benchmarks.

We are going to evaluate the model on the Truthfulqa benchmark with 0 few-shot examples. [TruthfulQA](https://paperswithcode.com/dataset/truthfulqa) is a benchmark designed to measure whether a language model generates truthful answers to questions, encompassing 817 questions across 38 categories including health, law, finance, and politics.

Python

```python
with open("scripts/tasks.txt", "w") as f:
    f.write(f"lighteval|truthfulqa:mc|0|0")
```

To evaluate a model on all the benchmarks of the Open LLM Leaderboard you can copy this [file](https://github.com/huggingface/lighteval/blob/v0.2.0/tasks_examples/open_llm_leaderboard_tasks.txt)

## 3\. Evaluate Zephyr 7B on TruthfulQA on Amazon SageMaker

In this example we are going to evaluate the [HuggingFaceH4/zephyr-7b-beta](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta) on the MMLU benchmark, which is part of the Open LLM Leaderboard.

In addition to the `task` argument we need to define:

- `model_args`: Hugging Face Model ID or path, defined as `pretrained=HuggingFaceH4/zephyr-7b-beta`
- `model_dtype`: The model data type, defined as `bfloat16`, `float16` or `float32`
- `output_dir`: The directory where the evaluation results will be saved, e.g. `/opt/ml/model`

Lightevals can also evaluat peft models or use `chat_templates` you find more about it [here](https://github.com/huggingface/lighteval/blob/v0.2.0/run_evals_accelerate.py).

Python

```python
from sagemaker.huggingface import HuggingFace
 
# hyperparameters, which are passed into the training job
hyperparameters = {
  'model_args': "pretrained=HuggingFaceH4/zephyr-7b-beta", # Hugging Face Model ID
  'task': 'tasks.txt',  # 'lighteval|truthfulqa:mc|0|0',
  'model_dtype': 'bfloat16', # Torch dtype to load model weights
  'output_dir': '/opt/ml/model' # Directory, which sagemaker uploads to s3 after training
}
 
# create the Estimator
huggingface_estimator = HuggingFace(
    entry_point          = 'run_evals_accelerate.py',      # train script
    source_dir           = 'scripts',         # directory which includes all the files needed for training
    instance_type        = 'ml.g5.4xlarge',   # instances type used for the training job
    instance_count       = 1,                 # the number of instances used for training
    base_job_name        = "lighteval",          # the name of the training job
    role                 = role,              # Iam role used in training job to access AWS ressources, e.g. S3
    volume_size          = 300,               # the size of the EBS volume in GB
    transformers_version = '4.36',            # the transformers version used in the training job
    pytorch_version      = '2.1',            # the pytorch_version version used in the training job
    py_version           = 'py310',            # the python version used in the training job
    hyperparameters      =  hyperparameters,
    environment          = {
                            "HUGGINGFACE_HUB_CACHE": "/tmp/.cache",
                            # "HF_TOKEN": "REPALCE_WITH_YOUR_TOKEN" # needed for private models
                            }, # set env variable to cache models in /tmp
)
```

We can now start our evaluation job, with the `.fit()`.

Python

```python
# starting the train job with our uploaded datasets as input
huggingface_estimator.fit()
```

After the evaluation job is finished, we can download the evaluation results from the S3 bucket. Lighteval will save the results and generations in the `output_dir`. The results are savedas json and include detailed information about each task and the model's performance. The results are available in the `results` key.

Python

```python
import tarfile
import json
import io
import os
from sagemaker.s3 import S3Downloader
 
 
# download results from s3
results_tar = S3Downloader.read_bytes(huggingface_estimator.model_data)
model_id = hyperparameters["model_args"].split("=")[1]
result={}
 
# Use tarfile to open the tar content directly from bytes
with tarfile.open(fileobj=io.BytesIO(results_tar), mode="r:gz") as tar:
    # Iterate over items in tar archive to find your json file by its path
    for member in tar.getmembers():
        # get path of results based on model id used to evaluate
        if os.path.join("details", model_id) in member.name and member.name.endswith('.json'):
            # Extract the file content
            f = tar.extractfile(member)
            if f is not None:
                content = f.read()
                result = json.loads(content)
                break
 
# print results
print(result["results"])
# {'lighteval|truthfulqa:mc|0': {'truthfulqa_mc1': 0.40636474908200737, 'truthfulqa_mc1_stderr': 0.017193835812093897, 'truthfulqa_mc2': 0.5747003398184238, 'truthfulqa_mc2_stderr': 0.015742356478301463}}
```

In our test we achieved a `mc1` score of 40.6% and an `mc2` score of 57.47%. The `mc2` is the score used in the Open LLM Leaderboard. Zephyr 7B achieved a `mc2` score of 57.47% on the TruthfulQA benchmark, which is identical to the score on the Open LLM Leaderboard. The evaluation on Truthfulqa took `999 seconds`. The ml.g5.4xlarge instance we used costs `$2.03 per hour` for on-demand usage. As a result, the total cost for evaluating Zephyr 7B on Truthfulqa was `$0.56`.

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
