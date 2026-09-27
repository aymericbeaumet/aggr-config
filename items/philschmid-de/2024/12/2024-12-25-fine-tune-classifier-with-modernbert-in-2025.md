---
title: Fine-tune classifier with ModernBERT in 2025
link: https://www.philschmid.de/fine-tune-modern-bert-in-2025
source: philschmid-de
published: 2024-12-25T00:00:00Z
updated: 2024-12-25T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Modern updated guide on how to fine-tune BERT models for classification tasks in 2025.
content: extracted
html: 2024-12-25-fine-tune-classifier-with-modernbert-in-2025.html
preview:
  file: 2024-12-25-fine-tune-classifier-with-modernbert-in-2025.preview-eacf35014598.webp
  width: 256
  height: 168
  alt: Fine-tune classifier with ModernBERT in 2025
  color: '#7a6f8d'
images:
- source: https://www.philschmid.de/static/blog/fine-tune-modern-bert-in-2025/thumbnail.jpg
  original:
    file: 2024-12-25-fine-tune-classifier-with-modernbert-in-2025.image-f85e23352b86.jpg
    width: 1024
    height: 671
  color: '#040304'
---

Large Language Models (LLMs) have become ubiquitous in 2024. However, smaller, specialized models - particularly for classification tasks - remain critical for building efficient and cost-effective AI systems. One key use case is routing user prompts to the most appropriate LLM or selecting optimal few-shot examples, where fast, accurate classification is essential.

This blog post demonstrates how to fine-tune ModernBERT, a new state-of-the-art encoder model, for classifying user prompts to implement an intelligent LLM router. ModernBERT is a refreshed version of BERT models, with 8192 token context length, significantly better downstream performance, and much faster processing speeds.

You will learn how to:

1. [Setup environment and install libraries](https://www.philschmid.de/fine-tune-modern-bert-in-2025#1-setup-environment-and-install-libraries)
2. [Load and prepare the classification dataset](https://www.philschmid.de/fine-tune-modern-bert-in-2025#2-load-and-prepare-the-classification-dataset)
3. [Fine-tune & evaluate ModernBERT with the Hugging Face `Trainer`](https://www.philschmid.de/fine-tune-modern-bert-in-2025#3-fine-tune--evaluate-modernbert-with-the-hugging-face-trainer)
4. [Run inference & test model](https://www.philschmid.de/fine-tune-modern-bert-in-2025#4-run-inference--test-model)

## Quick intro: ModernBERT

ModernBERT is a modernization of BERT maintaining full backward compatibility while delivering dramatic improvements through architectural innovations like rotary positional embeddings (RoPE), alternating attention patterns, and hardware-optimized design. The model comes in two sizes:

- ModernBERT Base (139M parameters)
- ModernBERT Large (395M parameters)

ModernBERT achieves state-of-the-art performance across classification, retrieval and code understanding tasks while being 2-4x faster than previous encoder models. This makes it ideal for high-throughput production applications like LLM routing, where both accuracy and latency are critical.

ModernBERT was trained on 2 trillion tokens of diverse data including web documents, code, and scientific articles - making it much more robust than traditional BERT models trained primarily on Wikipedia. This broader knowledge helps it better understand the nuances of user prompts across different domains.

If you want to learn more about ModernBERT's architecture and training process, check out the official [blog](https://huggingface.co/blog/modernbert).

* * *

Now let's get started building our LLM router with ModernBERT! 🚀

*Note: This tutorial was created and tested on an NVIDIA L4 GPU with 24GB of VRAM.*

## 1\. Setup environment and install libraries

Our first step is to install Hugging Face Libraries and Pyroch, including transformers and datasets.

Python

```python
# Install Pytorch & other libraries
%pip install "torch==2.4.1" tensorboard 
%pip install flash-attn "setuptools<71.0.0" scikit-learn 
 
# Install Hugging Face libraries
%pip install  --upgrade \
  "datasets==3.1.0" \
  "accelerate==1.2.1" \
  "hf-transfer==0.1.8"
  #"transformers==4.47.1" \
 
# ModernBERT is not yet available in an official release, so we need to install it from github
%pip install "git+https://github.com/huggingface/transformers.git@6e0515e99c39444caae39472ee1b2fd76ece32f1" --upgrade
 
```

We will use the [Hugging Face Hub](https://huggingface.co/models) as a remote model versioning service. This means we will automatically push our model, logs and information to the Hub during training. You must register on the [Hugging Face](https://huggingface.co/join) for this. After you have an account, we will use the `login` util from the `huggingface_hub` package to log into our account and store our token (access key) on the disk.

Python

```python
from huggingface_hub import login
 
login(token="", add_to_git_credential=True) # ADD YOUR TOKEN HERE
```

## 2\. Load and prepare the classification dataset

In our example we want to fine-tune ModernBERT to act as a router for user prompts. Therefore we need a classification dataset consisting of user prompts and their "difficulty" score. We are going to use the `DevQuasar/llm_router_dataset-synth` dataset, which is a synthetic dataset of ~15,000 user prompts with a difficulty score of "large\_llm" (`1`) or "small\_llm" (`0`).

We will use the `load_dataset()` method from the [🤗 Datasets](https://huggingface.co/docs/datasets/index) library to load the `DevQuasar/llm_router_dataset-synth` dataset.

Python

```python
from datasets import load_dataset
 
# Dataset id from huggingface.co/dataset
# dataset_id = "DevQuasar/llm_router_dataset-synth"
dataset_id = "legacy-datasets/banking77"
 
# Load raw dataset
raw_dataset = load_dataset(dataset_id)
 
print(f"Train dataset size: {len(raw_dataset['train'])}")
print(f"Test dataset size: {len(raw_dataset['test'])}")
```

Train dataset size: 10003 Test dataset size: 3080

Let’s check out an example of the dataset.

Python

```python
from random import randrange
 
random_id = randrange(len(raw_dataset['train']))
raw_dataset['train'][random_id]
# {'id': '6225a9cd-5cba-4840-8e21-1f9cf2ded7e6',
# 'prompt': 'How many legs does a spider have?',
# 'label': 0}
```

To train our model, we need to convert our text prompts to token IDs. This is done by a Tokenizer, which tokenizes the inputs (including converting the tokens to their corresponding IDs in the pre-trained vocabulary) if you want to learn more about this, out **[chapter 6](https://huggingface.co/course/chapter6/1?fw=pt)** of the [Hugging Face Course](https://huggingface.co/course/chapter1/1).

Python

```python
from transformers import AutoTokenizer
 
# Model id to load the tokenizer
# model_id = "answerdotai/ModernBERT-base"
model_id = "google-bert/bert-base-uncased"
# Load Tokenizer
tokenizer = AutoTokenizer.from_pretrained(model_id)
tokenizer.model_max_length = 512 # set model_max_length to 512 as prompts are not longer than 1024 tokens
 
# Tokenize helper function
def tokenize(batch):
    return tokenizer(batch['text'], padding='max_length', truncation=True, return_tensors="pt")
 
# Tokenize dataset
raw_dataset =  raw_dataset.rename_column("label", "labels") # to match Trainer
tokenized_dataset = raw_dataset.map(tokenize, batched=True,remove_columns=["text"])
 
print(tokenized_dataset["train"].features.keys())
# dict_keys(['input_ids', 'token_type_ids', 'attention_mask','lable'])
```

## 3\. Fine-tune & evaluate ModernBERT with the Hugging Face Trainer

After we have processed our dataset, we can start training our model. We will use the [answerdotai/ModernBERT-base](https://huggingface.co/answerdotai/ModernBERT-base) model. The first step is to load our model with `AutoModelForSequenceClassification` class from the [Hugging Face Hub](https://huggingface.co/answerdotai/ModernBERT-base). This will initialize the pre-trained ModernBERT weights with a classification head on top. Here we pass the number of classes (2) from our dataset and the label names to have readable outputs for inference.

Python

```python
from transformers import AutoModelForSequenceClassification
 
# Model id to load the tokenizer
# model_id = "answerdotai/ModernBERT-base"
model_id = "google-bert/bert-base-uncased"
 
# Prepare model labels - useful for inference
labels = tokenized_dataset["train"].features["labels"].names
num_labels = len(labels)
label2id, id2label = dict(), dict()
for i, label in enumerate(labels):
    label2id[label] = str(i)
    id2label[str(i)] = label
 
# Download the model from huggingface.co/models
model = AutoModelForSequenceClassification.from_pretrained(
    model_id, num_labels=num_labels, label2id=label2id, id2label=id2label,
)
```

We evaluate our model during training. The `Trainer` supports evaluation during training by providing a `compute_metrics` method. We use the `evaluate` library to calculate the [f1 metric](https://huggingface.co/spaces/evaluate-metric/f1) during training on our test split.

Python

```python
import numpy as np
from sklearn.metrics import f1_score
 
# Metric helper method
def compute_metrics(eval_pred):
    predictions, labels = eval_pred
    predictions = np.argmax(predictions, axis=1)
    score = f1_score(
            labels, predictions, labels=labels, pos_label=1, average="weighted"
        )
    return {"f1": float(score) if score == 1 else score}
```

The last step is to define the hyperparameters (`TrainingArguments`) we use for our training. Here we are adding optimizations introduced features for fast training times using `torch_compile` option in the `TrainingArguments`.

We also leverage the [Hugging Face Hub](https://huggingface.co/models) integration of the `Trainer` to push our checkpoints, logs, and metrics during training into a repository.

Python

```python
from huggingface_hub import HfFolder
from transformers import Trainer, TrainingArguments
 
# Define training args
training_args = TrainingArguments(
    output_dir= "modernbert-llm-router",
    per_device_train_batch_size=32,
    per_device_eval_batch_size=16,
    learning_rate=5e-5,
		num_train_epochs=5,
    bf16=True, # bfloat16 training 
    optim="adamw_torch_fused", # improved optimizer 
    # logging & evaluation strategies
    logging_strategy="steps",
    logging_steps=100,
    eval_strategy="epoch",
    save_strategy="epoch",
    save_total_limit=2,
    load_best_model_at_end=True,
    metric_for_best_model="f1",
    # push to hub parameters
    report_to="tensorboard",
    push_to_hub=True,
    hub_strategy="every_save",
    hub_token=HfFolder.get_token(),
 
)
 
# Create a Trainer instance
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=tokenized_dataset["train"],
    eval_dataset=tokenized_dataset["test"],
    compute_metrics=compute_metrics,
)
```

We can start our training by using the **`train`** method of the `Trainer`.

Python

```python
# Start training
trainer.train()
```

Fine-tuning `answerdotai/ModernBERT-base` on ~15,000 synthetic prompts for 5 epochs took `321` seconds and our best model achieved a `f1` score of `0.993`. 🚀 I also ran the training with `bert-base-uncased` to compare the training time and performance. The original BERT achieved a `f1` score of `0.99` and took `1048` seconds to train.

*Note: ModernBERT and BERT both almost achieve the same performance. This indicates that the dataset is not challenging and probably could be solved using a logistic regression classifier. I ran the same code on the [banking77](https://huggingface.co/datasets/legacy-datasets/banking77) dataset. A dataset of ~13,000 customer service queries with 77 classes. There the ModernBERT outperformed the original BERT by 3% (f1 score of 0.93 vs 0.90)*

Lets save our final best model and tokenizer to the Hugging Face Hub and create a model card.

Python

```python
# Save processor and create model card
tokenizer.save_pretrained("modernbert-llm-router")
trainer.create_model_card()
trainer.push_to_hub()
```

## 4\. Run Inference & test model

To wrap up this tutorial, we will run inference on a few examples and test our model. We will use the `pipeline` method from the `transformers` library to run inference on our model.

Python

```python
from transformers import pipeline
 
# load model from huggingface.co/models using our repository id
classifier = pipeline("sentiment-analysis", model="modernbert-llm-router", device=0)
 
sample = "How does the structure and function of plasmodesmata affect cell-to-cell communication and signaling in plant tissues, particularly in response to environmental stresses?"
 
 
pred = classifier(sample)
print(pred)
# [{'label': 'large_llm', 'score': 1.0}]
```

## Conclusion

In this tutorial, we learned how to fine-tune ModernBERT for an LLM routing classification task. We demonstrated how to leverage the Hugging Face ecosystem to efficiently train and deploy a specialized classifier that can intelligently route user prompts to the most appropriate LLM model.

Using modern training optimizations like flash attention, fused optimizers and mixed precision, we were able to train our model efficiently. Comparing ModernBERT with the original BERT we reduced training time by approximately 3x (1048s vs 321s) on our dataset and outperformed the original BERT by 3% on a more challenging dataset. But more importantly, ModernBERT was trained on 2 trillion tokens, which are more diverse and up to date than the Wikipedia-based training data of the original BERT.

This example showcases how smaller, specialized models remain valuable in the age of large language models - particularly for high-throughput, latency-sensitive tasks like LLM routing. By using ModernBERT's improved architecture and broader training data, we can build more robust and efficient classification systems.
