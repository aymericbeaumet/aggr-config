---
title: Train and Deploy open Embedding Models on Amazon SageMaker
link: https://www.philschmid.de/sagemaker-train-deploy-embedding-models
source: philschmid-de
published: 2024-06-25T00:00:00Z
updated: 2024-06-25T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to fine-tune and deploy a custom embedding model on Amazon SageMaker using the new Hugging Face Embedding Container.
content: extracted
html: 2024-06-25-train-and-deploy-open-embedding-models-on-amazon-sagemaker.html
preview:
  file: 2024-06-25-train-and-deploy-open-embedding-models-on-amazon-sagemaker.preview-774311739644.webp
  width: 256
  height: 128
  alt: Train and Deploy open Embedding Models on Amazon SageMaker
  color: '#6d6760'
images:
- source: https://www.philschmid.de/static/blog/sagemaker-train-deploy-embedding-models/thumbnail.jpg
  original:
    file: 2024-06-25-train-and-deploy-open-embedding-models-on-amazon-sagemaker.image-22bc2c52a342.jpg
    width: 1200
    height: 600
  color: '#151411'
---

Embedding models are crucial for successful RAG applications, but they're often trained on general knowledge, which limits their effectiveness for company or domain specific adoption. Customizing embedding for your domain specific data can significantly boost the retrieval performance of your RAG Application. With the new release of [Sentence Transformers 3](https://huggingface.co/blog/train-sentence-transformers) and the [Hugging Face Embedding Container](https://huggingface.co/blog/sagemaker-huggingface-embedding), it's easier than ever to fine-tune and deploy embedding models.

In this blog post, we'll show you how to fine-tune and deploy a custom embedding model on Amazon SageMaker using the new Hugging Face Embedding Container. We'll use the [Sentence Transformers 3](https://huggingface.co/blog/train-sentence-transformers) library to fine-tune a model on a custom dataset and deploy it on Amazon SageMaker for inference. We will fine-tune [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5) for financial RAG applications using a synthetic dataset from the [2023\_10 NVIDIA SEC Filing](https://stocklight.com/stocks/us/nasdaq-nvda/nvidia/annual-reports/nasdaq-nvda-2023-10K-23668751.pdf).

1. [Setup development environment](https://www.philschmid.de/sagemaker-train-deploy-embedding-models#1-setup-development-environment)
2. [Create and prepare the dataset](https://www.philschmid.de/sagemaker-train-deploy-embedding-models#2-create-and-prepare-the-dataset)
3. [Fine-tune Embedding model on Amazon SageMaker](https://www.philschmid.de/sagemaker-train-deploy-embedding-models#3-fine-tune-embedding-model-on-amazon-sagemaker)
4. [Deploy & Test fine-tuned Embedding Model on Amazon SageMaker](https://www.philschmid.de/sagemaker-train-deploy-embedding-models#4-deploy--test-fine-tuned-embedding-model-on-amazon-sagemaker)

*Note: This blog is an extension and dedicated version to my [Fine-tune Embedding models for Retrieval Augmented Generation (RAG)](https://www.philschmid.de/fine-tune-embedding-model-for-rag) version, specifically tailored to run on Amazon SageMaker.*

**What is new with Sentence Transforemrs 3?**

Sentence Transformers v3 introduces a new trainer that makes it easier to fine-tune and train embedding models. This update includes enhanced components like diverse datasets, updated loss functions, and a streamlined training process, improving the efficiency and flexibility of model development.

**What is the Hugging Face Embedding Container?**

The Hugging Face Embedding Container is a new purpose-built Inference Container to easily deploy Embedding Models in a secure and managed environment. The DLC is powered by [Text Embedding Inference (TEI)](https://github.com/huggingface/text-embeddings-inference) a blazing fast and memory efficient solution for deploying and serving Embedding Models. TEI enables high-performance extraction for the most popular models, including FlagEmbedding, Ember, GTE and E5. TEI implements many features such as:

*Note: This blog was created and validated on `ml.g5.xlarge` for training and `ml.c6i.2xlarge` for inference instance.*

## 1\. Setup Development Environment

Our first step is to install Hugging Face Libraries we need on the client to correctly prepare our dataset and start our training/evaluations jobs.

Python

```python
!pip install transformers "datasets[s3]==2.18.0" "sagemaker>=2.190.0" "huggingface_hub[cli]" --upgrade --quiet
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

## 2\. Create and prepare the dataset

An embedding dataset typically consists of text pairs (question, answer/context) or triplets that represent relationships or similarities between sentences. The dataset format you choose or have available will also impact the loss function you can use. Common formats for embedding datasets:

- **Positive Pair**: Text Pairs of related sentences (query, context | query, answer), suitable for tasks like similarity or semantic search, example datasets: `sentence-transformers/sentence-compression`, `sentence-transformers/natural-questions`.
- **Triplets**: Text triplets consisting of (anchor, positive, negative), example datasets `sentence-transformers/quora-duplicates`, `nirantk/triplets`.
- **Pair with Similarity Score**: Sentence pairs with a similarity score indicating how related they are, example datasets: `sentence-transformers/stsb`, `PhilipMay/stsb_multi_mt`

Learn more at [Dataset Overview](https://sbert.net/docs/sentence_transformer/dataset_overview.html).

We are going to use [philschmid/finanical-rag-embedding-dataset](https://huggingface.co/datasets/philschmid/finanical-rag-embedding-dataset), which includes 7,000 positive text pairs of questions and corresponding context from the [2023\_10 NVIDIA SEC Filing](https://stocklight.com/stocks/us/nasdaq-nvda/nvidia/annual-reports/nasdaq-nvda-2023-10K-23668751.pdf).

The dataset has the following format

JSON

```json
{"question": "<question>", "context": "<relevant context to answer>"}
{"question": "<question>", "context": "<relevant context to answer>"}
{"question": "<question>", "context": "<relevant context to answer>"}
```

We are going to use the [FileSystem integration](https://huggingface.co/docs/datasets/filesystems) to upload our dataset to S3. We are using the `sess.default_bucket()`, adjust this if you want to store the dataset in a different S3 bucket. We will use the S3 path later in our training script.

Python

```python
from datasets import load_dataset
 
# Load dataset from the hub
dataset = load_dataset("philschmid/finanical-rag-embedding-dataset", split="train")
input_path = f's3://{sess.default_bucket()}/datasets/rag-embedding'
 
# rename columns
dataset = dataset.rename_column("question", "anchor")
dataset = dataset.rename_column("context", "positive")
 
# Add an id column to the dataset
dataset = dataset.add_column("id", range(len(dataset)))
 
# split dataset into a 10% test set
dataset = dataset.train_test_split(test_size=0.1)
 
# save train_dataset to s3 using our SageMaker session
 
# save datasets to s3
dataset["train"].to_json(f"{input_path}/train/dataset.json", orient="records")
train_dataset_s3_path = f"{input_path}/train/dataset.json"
dataset["test"].to_json(f"{input_path}/test/dataset.json", orient="records")
test_dataset_s3_path = f"{input_path}/test/dataset.json"
 
print(f"Training data uploaded to:")
print(train_dataset_s3_path)
print(test_dataset_s3_path)
print(f"https://s3.console.aws.amazon.com/s3/buckets/{sess.default_bucket()}/?region={sess.boto_region_name}&prefix={input_path.split('/', 3)[-1]}/")
```

## 3\. Fine-tune Embedding model on Amazon SageMaker

We are now ready to fine-tune our model. We will use the [SentenceTransformerTrainer](https://www.sbert.net/docs/package_reference/sentence_transformer/trainer.html) from `sentence-transformers` to fine-tune our model. The `SentenceTransformerTrainer` makes it straightfoward to supervise fine-tune open Embedding Models, as it is a subclass of the `Trainer` from the `transformers`. We prepared a script [run\_mnr.py](https://github.com/huggingface/notebooks/blob/main/sagemaker/32_train_deploy_embedding_models/scripts/run_mnr.py) which will loads the dataset from disk, prepare the model, tokenizer and start the training. The `SentenceTransformerTrainer` makes it straightfoward to supervise fine-tune open Embedding supporting:

- **Integrated Components**: Combines datasets, loss functions, and evaluators into a unified training framework.
- **Flexible Data Handling**: Supports various data formats and easy integration with Hugging Face datasets.
- **Versatile Loss Functions**: Offers multiple loss functions for different training tasks.
- **Multi-Dataset Training**: Facilitates simultaneous training with multiple datasets and different loss functions.
- **Seamless Integration**: Easy saving, loading, and sharing of models within the Hugging Face ecosystem.

In order to create a sagemaker training job we need an `HuggingFace` Estimator. The Estimator handles end-to-end Amazon SageMaker training and deployment tasks. The Estimator manages the infrastructure use. Amazon SagMaker takes care of starting and managing all the required ec2 instances for us, provides the correct huggingface container, uploads the provided scripts and downloads the data from our S3 bucket into the container at `/opt/ml/input/data`. Then, it starts the training job by running.

> Note: Make sure that you include the `requirements.txt` in the `source_dir` if you are using a custom training script. We recommend to just clone the whole repository.

Lets first define our trainings parameter. Those are passed as cli arguments to our training script. We are going to use the `BAAI/bge-base-en-v1.5` model, which is a pre-trained model on a large corpus of English text. We will use the `MultipleNegativesRankingLoss` in combination with the `MatryoshkaLoss`. This approach allows us to leverage the efficiency and flexibility of Matryoshka embeddings, enabling different embedding dimensions to be utilized without significant performance trade-offs. The `MultipleNegativesRankingLoss` is a great loss function if you only have positive pairs as it adds in batch negative samples to the loss function to have per sample n-1 negative samples.

Python

```python
from sagemaker.huggingface import HuggingFace
 
# define Training Job Name 
job_name = f'bge-base-exp1'
 
# define hyperparameters, which are passed into the training job
training_arguments = {
  "model_id": "BAAI/bge-base-en-v1.5", # model id from the hub
  "train_dataset_path": "/opt/ml/input/data/train/", # path inside the container where the training data is stored
  "test_dataset_path": "/opt/ml/input/data/test/", # path inside the container where the test data is stored
  "num_train_epochs": 3, # number of training epochs
  "learning_rate": 2e-5, # learning rate
}
 
# create the Estimator
huggingface_estimator = HuggingFace(
    entry_point          = 'run_mnr.py',      # train script
    source_dir           = 'scripts',         # directory which includes all the files needed for training
    instance_type        = 'ml.g5.xlarge',    # instances type used for the training job
    instance_count       = 1,                 # the number of instances used for training
    max_run              = 2*24*60*60,        # maximum runtime in seconds (days * hours * minutes * seconds)
    base_job_name        = job_name,          # the name of the training job
    role                 = role,              # Iam role used in training job to access AWS ressources, e.g. S3
    transformers_version = '4.36.0',          # the transformers version used in the training job
    pytorch_version      = '2.1.0',           # the pytorch_version version used in the training job
    py_version           = 'py310',           # the python version used in the training job
    hyperparameters      =  training_arguments,
    disable_output_compression = True,        # not compress output to save training time and cost
    environment  = {
        "HUGGINGFACE_HUB_CACHE": "/tmp/.cache", # set env variable to cache models in /tmp
    }, 
)
```

We can now start our training job, with the `.fit()` method passing our S3 path to the training script.

Python

```python
# define a data input dictonary with our uploaded s3 uris
data = {
  'train': train_dataset_s3_path,
  'test': test_dataset_s3_path,
  }
 
# starting the train job with our uploaded datasets as input
huggingface_estimator.fit(data, wait=True)
```

In our example the training BGE Base with Flash Attention 2 (SDPA) for 3 epochs with a dataset of 6,3k train samples and 700 eval samples took 645 seconds (~10minutes) on a `ml.g5.xlarge` (1.2575 $/h) or ~$0.2.

## 4\. Deploy & Test fine-tuned Embedding Model on Amazon SageMaker

We are going to use the [Hugging Face Embedding Container](https://huggingface.co/blog/sagemaker-huggingface-embedding#what-is-the-hugging-face-embedding-container) a purpose-built Inference Container to easily deploy Embedding Models in a secure and managed environment. The DLC is powered by Text Embedding Inference (TEI) a blazing fast and memory efficient solution for deploying and serving Embedding Models.

To retrieve the new Hugging Face Embedding Container in Amazon SageMaker, we can use the `get_huggingface_llm_image_uri` method provided by the sagemaker SDK. This method allows us to retrieve the URI for the desired Hugging Face Embedding Container. Important to note is that TEI has 2 different versions for cpu and gpu, so we create a helper function to retrieve the correct image uri based on the instance type.

Python

```python
from sagemaker.huggingface import get_huggingface_llm_image_uri
 
# retrieve the image uri based on instance type
def get_image_uri(instance_type):
  key = "huggingface-tei" if instance_type.startswith("ml.g") or instance_type.startswith("ml.p") else "huggingface-tei-cpu"
  return get_huggingface_llm_image_uri(key, version="1.2.3")
 
```

We can now create a `HuggingFaceModel` using the container uri and the S3 path to our model. We also need to set our TEI configuration.

Python

```python
from sagemaker.huggingface import HuggingFaceModel
 
# sagemaker config
instance_type = "ml.c6i.2xlarge"
 
# create HuggingFaceModel with the image uri
emb_model = HuggingFaceModel(
  role=role,
  image_uri=get_image_uri(instance_type),
  model_data=huggingface_estimator.model_data,
  env={'HF_MODEL_ID': "/opt/ml/model"}     # Path to the model in the container
)
```

After we have created the `HuggingFaceModel` we can deploy it to Amazon SageMaker using the deploy method. We will deploy the model with the `ml.c6i.2xlarge` instance type.

Python

```python
# Deploy model to an endpoint
emb = emb_model.deploy(
  initial_instance_count=1,
  instance_type=instance_type,
)
```

SageMaker will now create our endpoint and deploy the model to it. This can take ~5 minutes. After our endpoint is deployed we can run inference on it. We will use the `predict` method from the predictor to run inference on our endpoint.

Python

```python
data = {
  "inputs": "the mesmerizing performances of the leads keep the film grounded and keep the audience riveted .",
}
 
res = emb.predict(data=data)
 
 
# print some results
print(f"length of embeddings: {len(res[0])}")
print(f"first 10 elements of embeddings: {res[0][:10]}")
 
```

We trained our model with the Matryoshka Loss means that the semantic meaning is frontloaded. To use the different mathryshoka dimension we need to manually truncate our embeddings manually. Below is an example on how you would truncate the embeddings to 256 dimension, which is 1/3 of the original size. If we check our training logs we can see that the NDCG metric for 768 is `0.823` and for 256 `0.818` meaning we preserve > 99% accuracy.

Python

```python
data = {
  "inputs": "the mesmerizing performances of the leads keep the film grounded and keep the audience riveted .",
}
 
res = emb.predict(data=data)
 
# truncate embeddings to matryoshka dimensions
dim = 256
res = res[0][0:dim]
 
# print some results
print(f"length of embeddings: {len(res)}")
```

Awesome! 🚀 Now that we can generate embeddings and integrate your endpoint into your RAG application.

To clean up, we can delete the model and endpoint.

Python

```python
emb.delete_model()
emb.delete_endpoint()
```

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
