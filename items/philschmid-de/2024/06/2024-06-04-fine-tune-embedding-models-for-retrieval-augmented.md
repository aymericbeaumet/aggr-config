---
title: Fine-tune Embedding models for Retrieval Augmented Generation (RAG)
link: https://www.philschmid.de/fine-tune-embedding-model-for-rag
source: philschmid-de
published: 2024-06-04T00:00:00Z
updated: 2024-06-04T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Customizing embedding models for domain-specific data can significantly boost the retrieval performance of your RAG Application.
content: extracted
html: 2024-06-04-fine-tune-embedding-models-for-retrieval-augmented.html
preview:
  file: 2024-06-04-fine-tune-embedding-models-for-retrieval-augmented.preview-6179c02f59b4.webp
  width: 256
  height: 128
  alt: Fine-tune Embedding models for Retrieval Augmented Generation (RAG)
  color: '#656764'
images:
- source: https://www.philschmid.de/static/blog/fine-tune-embedding-model-for-rag/thumbnail.jpg
  original:
    file: 2024-06-04-fine-tune-embedding-models-for-retrieval-augmented.image-aa61b4239e58.jpg
    width: 1200
    height: 600
  color: '#fefce7'
---

Embedding models are crucial for successful RAG applications, but they're often trained on general knowledge, which limits their effectiveness for company or domain specific adoption. Customizing embedding for your domain specific data can significantly boost the retrieval performance of your RAG Application. With the new release of [Sentence Transformers 3](https://huggingface.co/blog/train-sentence-transformers), it's easier than ever to fine-tune embedding models.

In this blog, we'll show you how to fine-tune an embedding model for a financial RAG applications using a synthetic dataset from the [2023\_10 NVIDIA SEC Filing](https://stocklight.com/stocks/us/nasdaq-nvda/nvidia/annual-reports/nasdaq-nvda-2023-10K-23668751.pdf). We'll also leverage Matryoshka Representation Learning to boost efficiency. In the blog, we are going to:

1. Create & Prepare embedding dataset
2. Create baseline and evaluate pretrained model
3. Define loss function with Matryoshka Representation
4. Fine-tune embedding model with `SentenceTransformersTrainer`
5. Evaluate fine-tuned model against baseline

**🪆 Matryoshka Embeddings**

[Matryoshka Representation Learning (MRL)](https://huggingface.co/blog/matryoshka) is a technique designed to create embeddings that can be truncated to various dimensions without significant loss of performance. This approach frontloads important information into earlier dimensions of the embedding, allowing for efficient storage and processing while maintaining high accuracy in downstream tasks such as retrieval, classification, and clustering.

For example, a Matryoshka model can preserve ~99.9% of its performance while needing 3x less storage. This is particularly useful for applications where storage and processing resources are limited, such as on-device applications or large-scale retrieval systems.

*Note: This blog was created to run on consumer size GPUs (24GB), e.g. NVIDIA A10G or RTX 4090/3090, but can be easily adapted to run on bigger GPUs.*

* * *

Before we can get started we need to install Hugging Face Libraries and Pytorch, including Sentence Transformers, transformers and datasets.

Python

```python
# Install Pytorch & other libraries
!pip install "torch==2.1.2" tensorboard
 
# Install Hugging Face libraries
!pip install --upgrade \
  "sentence-transformers>=3" \
  "datasets==2.19.1"  \
  "transformers==4.41.2" 
 
```

We will use the [Hugging Face Hub](https://huggingface.co/models) as a remote model versioning service. This means we will automatically push our model, logs and information to the Hub during training. You must register on the [Hugging Face](https://huggingface.co/join) for this. After you have an account, we will use the `login` util from the `huggingface_hub` package to log into our account and store our token (access key) on the disk.

Python

```python
from huggingface_hub import login
 
login(token="", add_to_git_credential=True)  # ADD YOUR TOKEN HERE
```

## 1\. Create & Prepare embedding dataset

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

We are going to load our open-source dataset using the 🤗 Datasets library, rename the colums to match what `sentence-transforemrs` expects and then split our dataset into a train and test spilt to be able to evaluate our model.

Python

```python
from datasets import load_dataset
 
# Load dataset from the hub
dataset = load_dataset("philschmid/finanical-rag-embedding-dataset", split="train")
 
# rename columns
dataset = dataset.rename_column("question", "anchor")
dataset = dataset.rename_column("context", "positive")
 
# Add an id column to the dataset
dataset = dataset.add_column("id", range(len(dataset)))
 
# split dataset into a 10% test set
dataset = dataset.train_test_split(test_size=0.1)
 
# save datasets to disk
dataset["train"].to_json("train_dataset.json", orient="records")
dataset["test"].to_json("test_dataset.json", orient="records")
```

## 2\. Create baseline and evaluate pretrained model

After we created our dataset we want to create a baseline. A baseline provides use reference point against which the performance of your customized model can be measured. By evaluating the performance of a pretrained model on our specific dataset, we gain insights into the initial effectiveness and can identify areas for improvement.

For our example, we will use the [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5) model as our starting point. [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5) is one of the strongest open embedding models for it size, with only 109M parameters and a hidden dimension of 768 it achieves `63.55` on the [MTEB Leaderboard](https://huggingface.co/spaces/mteb/leaderboard).

We are going to use the [InformationRetrievalEvaluator](https://sbert.net/docs/package_reference/sentence_transformer/evaluation.html#sentence_transformers.evaluation.InformationRetrievalEvaluator) to evaluate the performance of our model on a given set of queries and corpus set. It will retrieve for each query the top-k most similar document. It measures Mean Reciprocal Rank (MRR), Recall@k, Mean Average Precision (MAP) and Normalized Discounted Cumulative Gain (NDCG).

For us the most important metric will be Normalized Discounted Cumulative Gain (NDCG) as it is a measure of ranking quality. It takes into account the position of the relevant document in the ranking and discounts it. The discounted value is logarithmic, which means that relevant documents are more important if they are ranked higher.

For our evaluation corpus we will use all "documents" for potential retrieval from the train and test split and the queries from the test set. As mentioned in the beginning we are going to use Matryoshka Representation Learning to boost efficiency. We are going to create a baseline for the following dimensions `64`, `128`, `256`, `512`, `768`. Since those are the dimensions we are going to use for our Matryoshka Representation Learning.

Python

```python
import torch
from sentence_transformers import SentenceTransformer
from sentence_transformers.evaluation import (
    InformationRetrievalEvaluator,
    SequentialEvaluator,
)
from sentence_transformers.util import cos_sim
from datasets import load_dataset, concatenate_datasets
 
model_id = "BAAI/bge-base-en-v1.5"  # Hugging Face model ID
matryoshka_dimensions = [768, 512, 256, 128, 64] # Important: large to small
 
# Load a model
model = SentenceTransformer(
    model_id, device="cuda" if torch.cuda.is_available() else "cpu"
)
 
# load test dataset
test_dataset = load_dataset("json", data_files="test_dataset.json", split="train")
train_dataset = load_dataset("json", data_files="train_dataset.json", split="train")
corpus_dataset = concatenate_datasets([train_dataset, test_dataset])
 
# Convert the datasets to dictionaries
corpus = dict(
    zip(corpus_dataset["id"], corpus_dataset["positive"])
)  # Our corpus (cid => document)
queries = dict(
    zip(test_dataset["id"], test_dataset["anchor"])
)  # Our queries (qid => question)
 
# Create a mapping of relevant document (1 in our case) for each query
relevant_docs = {}  # Query ID to relevant documents (qid => set([relevant_cids])
for q_id in queries:
    relevant_docs[q_id] = [q_id]
 
 
matryoshka_evaluators = []
# Iterate over the different dimensions
for dim in matryoshka_dimensions:
    ir_evaluator = InformationRetrievalEvaluator(
        queries=queries,
        corpus=corpus,
        relevant_docs=relevant_docs,
        name=f"dim_{dim}",
        truncate_dim=dim,  # Truncate the embeddings to a certain dimension
        score_functions={"cosine": cos_sim},
    )
    matryoshka_evaluators.append(ir_evaluator)
 
# Create a sequential evaluator
evaluator = SequentialEvaluator(matryoshka_evaluators)
```

We are going to use the `evaluator` also for evaluation during training. But first lets create our baseline.

Python

```python
# Evaluate the model
results = evaluator(model)
 
# # COMMENT IN for full results
# print(results)
 
# Print the main score
for dim in matryoshka_dimensions:
    key = f"dim_{dim}_cosine_ndcg@10"
    print
    print(f"{key}: {results[key]}")
```

Great our default baseline with the [BAAI/bge-base-en-v1.5](https://huggingface.co/BAAI/bge-base-en-v1.5) are:

- dim 768: `0.7683576219287339`
- dim 512: `0.7642500951356254`
- dim 256: `0.7546468566985919`
- dim 128: `0.7233663127615272`
- dim 64: `0.6439999058834552`

Now, let's see if we can improve this score by fine-tuning the model on our specific dataset.

## 3\. Define loss function with Matryoshka Representation

When fine-tuning embedding models we select a [loss function based on our dataset format](https://sbert.net/docs/sentence_transformer/loss_overview.html). For Positive Text pairs we can use the `MultipleNegativesRankingLoss` in combination with the `MatryoshkaLoss`. This approach allows us to leverage the efficiency and flexibility of Matryoshka embeddings, enabling different embedding dimensions to be utilized without significant performance trade-offs. The `MultipleNegativesRankingLoss` is a great loss function if you only have positive pairs as it adds in batch negative samples to the loss function to have per sample n-1 negative samples.

Lets reload our model using `SDPA` or Flash Attention 2 as `attn_implementation` and define a model card.

Python

```python
from sentence_transformers import SentenceTransformerModelCardData, SentenceTransformer
 
# Hugging Face model ID: https://huggingface.co/BAAI/bge-base-en-v1.5
model_id = "BAAI/bge-base-en-v1.5"
 
# load model with SDPA for using Flash Attention 2
model = SentenceTransformer(
    model_id,
    model_kwargs={"attn_implementation": "sdpa"},
    model_card_data=SentenceTransformerModelCardData(
        language="en",
        license="apache-2.0",
        model_name="BGE base Financial Matryoshka",
    ),
)
```

After we loaded our model we can initialize our loss function.

Python

```python
from sentence_transformers.losses import MatryoshkaLoss, MultipleNegativesRankingLoss
 
matryoshka_dimensions = [768, 512, 256, 128, 64]  # Important: large to small
inner_train_loss = MultipleNegativesRankingLoss(model)
train_loss = MatryoshkaLoss(
    model, inner_train_loss, matryoshka_dims=matryoshka_dimensions
)
```

## 4\. Fine-tune embedding model with `SentenceTransformersTrainer`

We are now ready to fine-tune our model. We will use the [SentenceTransformersTrainer](https://sbert.net/docs/package_reference/sentence_transformer/trainer.html#sentencetransformertrainer) a subclass of the `Trainer` from the `transformers` library, which supports all the same features, including logging, evaluation, and checkpointing.

In addition to this there is a `SentenceTransformerTrainingArguments` class that allows us to specify all the training parameters.

Python

```python
from sentence_transformers import SentenceTransformerTrainingArguments
from sentence_transformers.training_args import BatchSamplers
 
# load train dataset again
train_dataset = load_dataset("json", data_files="train_dataset.json", split="train")
 
# define training arguments
args = SentenceTransformerTrainingArguments(
    output_dir="bge-base-financial-matryoshka", # output directory and hugging face model ID
    num_train_epochs=4,                         # number of epochs
    per_device_train_batch_size=32,             # train batch size
    gradient_accumulation_steps=16,             # for a global batch size of 512
    per_device_eval_batch_size=16,              # evaluation batch size
    warmup_ratio=0.1,                           # warmup ratio
    learning_rate=2e-5,                         # learning rate, 2e-5 is a good value
    lr_scheduler_type="cosine",                 # use constant learning rate scheduler
    optim="adamw_torch_fused",                  # use fused adamw optimizer
    tf32=True,                                  # use tf32 precision
    bf16=True,                                  # use bf16 precision
    batch_sampler=BatchSamplers.NO_DUPLICATES,  # MultipleNegativesRankingLoss benefits from no duplicate samples in a batch
    eval_strategy="epoch",                      # evaluate after each epoch
    save_strategy="epoch",                      # save after each epoch
    logging_steps=10,                           # log every 10 steps
    save_total_limit=3,                         # save only the last 3 models
    load_best_model_at_end=True,                # load the best model when training ends
    metric_for_best_model="eval_dim_128_cosine_ndcg@10",  # Optimizing for the best ndcg@10 score for the 128 dimension
)
```

We now have every building block we need to create our `SentenceTransformersTrainer` to start then training our model.

Python

```python
from sentence_transformers import SentenceTransformerTrainer
 
trainer = SentenceTransformerTrainer(
    model=model, # bg-base-en-v1
    args=args,  # training arguments
    train_dataset=train_dataset.select_columns(
        ["positive", "anchor"]
    ),  # training dataset
    loss=train_loss,
    evaluator=evaluator,
)
```

Start training our model by calling the `train()` method on our `SentenceTransformerTrainer` instance. This will start the training loop and train our model for 4 epochs.

Python

```python
# start training, the model will be automatically saved to the hub and the output directory
trainer.train()
 
# save the best model
trainer.save_model()
 
# push model to hub
trainer.model.push_to_hub("bge-base-financial-matryoshka")
```

The training with Flash Attention (SDPA) for 4 epochs on 6.3k samples took ~00:03:26 on a `g5.2xlarge`. The instance costs `1.212$/h` which brings us to a total cost of only `0.07$` for the training.

## 5\. Evaluate fine-tuned model against baseline

We evaluated our model during training, but we also want to evaluate it against our baseline at the end. We use the same `InformationRetrievalEvaluator` to evaluate the performance of our model on a given set of queries and corpus set.

Python

```python
from sentence_transformers import SentenceTransformer
 
fine_tuned_model = SentenceTransformer(
    args.output_dir, device="cuda" if torch.cuda.is_available() else "cpu"
)
# Evaluate the model
results = evaluator(fine_tuned_model)
 
# # COMMENT IN for full results
# print(results)
 
# Print the main score
for dim in matryoshka_dimensions:
    key = f"dim_{dim}_cosine_ndcg@10"
    print(f"{key}: {results[key]}")
```

Our fine-tuned model achieves:

- dim 768: `0.8253652077429072`
- dim 512: `0.8274643684492735`
- dim 256: `0.8230326345640168`
- dim 128: `0.8184049256658124`
- dim 64: `0.7892150398663841`

Lets put this into a table and compare the performance of our fine-tuned model against the baseline.

| Dimension | Baseline | Fine-tuned | Improvement |
| --------- | -------- | ---------- | ----------- |
| 768       | 0.7684   | 0.8254     | 7.42%       |
| 512       | 0.7643   | 0.8275     | 8.27%       |
| 256       | 0.7546   | 0.8230     | 9.06%       |
| 128       | 0.7234   | 0.8184     | 13.13%      |
| 64        | 0.6440   | 0.7892     | 22.55%      |

Lets try to understand this:

- Fine-tuned model outperforms the baseline model in all dimensions.
- BGE base is already a very strong base model but fine-tuning it on only 6.3k samples still improves performance by ~7.4%.
- Matryoshka representation learning works and keeps 95% performance at 64 dimensions (12x size reduction), 99% at 128 dimensions (6x size reduction), >99.5% at 256 dimensions (3x size reduction).
- The Fine-tuned model with lowest dimension (64) outperforms the baseline model with highest dimensions (768).
- Optimizing the model dimension of 128 allows us to keep 99% of the performance of the 768 dimension model but reducing the storing cost by 6x and improving the cosine similarity search.
- The fine-tuned model with 128 dimensions outperforms the baseline model with 768 dimensions by 6.51%, while being 6x smaller.

## Conclusion

Embedding models are crucial for successfull RAG applications, since if you don't retrieve the right context you can't generate the right answer. Customizing embedding models for domain-specific data can improve retrieval performance significantly compared to using general knowledge models. Fine-tuning embedding models has become highly accessible, and using synthetic data generated by LLMs, one can easily customize models for specific needs, resulting in substantial improvements.

Our results show that fine-tuning can boost performance by ~7% with only 6.3k sample. The training took 3 minutes on a consumer size GPU and by leveraging modern techniques like Matryoshka Representation Learning we achieved over 99% performance retention with 6x storage reduction and efficiency gains.

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
