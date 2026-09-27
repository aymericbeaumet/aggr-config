---
title: How to Fine-Tune LLMs in 2024 with Hugging Face
link: https://www.philschmid.de/fine-tune-llms-in-2024-with-trl
source: philschmid-de
published: 2024-01-23T00:00:00Z
updated: 2024-01-23T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to fine-tune LLMs using Hugging Face TRL, Transformers and Datasets in 2024. We will fine-tune a LLM on a text to SQL dataset.
content: extracted
html: 2024-01-23-how-to-fine-tune-llms-in-2024-with-hugging-face.html
preview:
  file: 2024-01-23-how-to-fine-tune-llms-in-2024-with-hugging-face.preview-00b6955236b2.webp
  width: 256
  height: 128
  alt: How to Fine-Tune LLMs in 2024 with Hugging Face
  color: '#423a43'
images:
- source: https://www.philschmid.de/static/blog/fine-tune-llms-in-2024-with-trl/thumbnail.jpg
  original:
    file: 2024-01-23-how-to-fine-tune-llms-in-2024-with-hugging-face.image-17f2527a2222.jpg
    width: 1200
    height: 600
  color: '#040403'
---

Large Language Models or LLMs have seen a lot of progress in the last year. We went from no ChatGPT competitor to a whole zoo of LLMs, including Meta AI's [Llama 2](https://huggingface.co/meta-llama/Llama-2-70b-chat-hf), Mistrals [Mistral](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2) & [Mixtral](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1) models, TII [Falcon](https://huggingface.co/tiiuae/falcon-40b), and many more. Those LLMs can be used for a variety of tasks, including chatbots, question answering, summarization without any additional training. However, if you want to customize a model for your application. You may need to fine-tune the model on your data to achieve higher quality results than prompting or saving cost by training smaller models more efficient model.

This blog post walks you thorugh how to fine-tune open LLMs using Hugging Face [TRL](https://huggingface.co/docs/trl/index), [Transformers](https://huggingface.co/docs/transformers/index) & [datasets](https://huggingface.co/docs/datasets/index) in 2024. In the blog, we are going to:

1. [Define our use case](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#1-define-our-use-case)
2. [Setup development environment](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#2-setup-development-environment)
3. [Create and prepare the dataset](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#3-create-and-prepare-the-dataset)
4. [Fine-tune LLM using `trl` and the `SFTTrainer`](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#4-fine-tune-llm-using-trl-and-the-sfttrainer)
5. [Test and evaluate the LLM](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#5-test-and-evaluate-the-llm)
6. [Deploy the LLM for Production](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#6-deploy-the-llm-for-production)

*Note: This blog was created to run on consumer size GPUs (24GB), e.g. NVIDIA A10G or RTX 4090/3090, but can be easily adapted to run on bigger GPUs.*

## 1\. Define our use case

When fine-tuning LLMs, it is important you know your use case and the task you want to solve. This will help you to choose the right model or help you to create a dataset to fine-tune your model. If you haven't defined your use case yet. You might want to go back to the drawing board. I want to mention that not all use cases require fine-tuning and it is always recommended to evaluate and try out already fine-tuned models or API-based models before fine-tuning your own model.

As an example, we are going to use the following use case:

> We want to fine-tune a model, which can generate SQL queries based on a natural language instruction, which can then be integrated into our BI tool. The goal is to reduce the time it takes to create a SQL query and make it easier for non-technical users to create SQL queries.

Text to SQL can be a good use case for fine-tuning LLMs, as it is a complex task that requires a lot of (internal) knowledge about the data and the SQL language.

## 2\. Setup development environment

Our first step is to install Hugging Face Libraries and Pytorch, including trl, transformers and datasets. If you haven't heard of trl yet, don't worry. It is a new library on top of transformers and datasets, which makes it easier to fine-tune, rlhf, align open LLMs.

Python

```python
# Install Pytorch & other libraries
!pip install "torch==2.1.2" tensorboard
 
# Install Hugging Face libraries
!pip install  --upgrade \
  "transformers==4.36.2" \
  "datasets==2.16.1" \
  "accelerate==0.26.1" \
  "evaluate==0.4.1" \
  "bitsandbytes==0.42.0" \
  # "trl==0.7.10" # \
  # "peft==0.7.1" \
 
# install peft & trl from github
!pip install git+https://github.com/huggingface/trl@a3c5b7178ac4f65569975efadc97db2f3749c65e --upgrade
!pip install git+https://github.com/huggingface/peft@4a1559582281fc3c9283892caea8ccef1d6f5a4f --upgrade
```

If you are using a GPU with Ampere architecture (e.g. NVIDIA A10G or RTX 4090/3090) or newer you can use Flash attention. Flash Attention is a an method that reorders the attention computation and leverages classical techniques (tiling, recomputation) to significantly speed it up and reduce memory usage from quadratic to linear in sequence length. The TL;DR; accelerates training up to 3x. Learn more at [FlashAttention](https://github.com/Dao-AILab/flash-attention/tree/main).

*Note: If your machine has less than 96GB of RAM and lots of CPU cores, reduce the number of `MAX_JOBS`. On the `g5.2xlarge` we used `4`.*

Python

```python
import torch; assert torch.cuda.get_device_capability()[0] >= 8, 'Hardware not supported for Flash Attention'
# install flash-attn
!pip install ninja packaging
!MAX_JOBS=4 pip install flash-attn --no-build-isolation
```

*Installing flash attention can take quite a bit of time (10-45 minutes).*

We will use the [Hugging Face Hub](https://huggingface.co/models) as a remote model versioning service. This means we will automatically push our model, logs and information to the Hub during training. You must register on the [Hugging Face](https://huggingface.co/join) for this. After you have an account, we will use the `login` util from the `huggingface_hub` package to log into our account and store our token (access key) on the disk.

Python

```python
from huggingface_hub import login
 
login(
  token="", # ADD YOUR TOKEN HERE
  add_to_git_credential=True
)
 
```

## 3\. Create and prepare the dataset

Once you have determined that fine-tuning is the right solution we need to create a dataset to fine-tune our model. The dataset should be a diverse set of demonstrations of the task you want to solve. There are several ways to create such a dataset, including:

- Using existing open-source datasets, e.g., [Spider](https://huggingface.co/datasets/spider)
- Using LLMs to create synthetically datasets, e.g., [Alpaca](https://huggingface.co/datasets/tatsu-lab/alpaca)
- Using Humans to create datasets, e.g., [Dolly](https://huggingface.co/datasets/databricks/databricks-dolly-15k).
- Using a combination of the above methods, e.g., [Orca](https://huggingface.co/datasets/Open-Orca/OpenOrca)

Each of the methods has its own advantages and disadvantages and depends on the budget, time, and quality requirements. For example, using an existing dataset is the easiest but might not be tailored to your specific use case, while using humans might be the most accurate but can be time-consuming and expensive. It is also possible to combine several methods to create an instruction dataset, as shown in [Orca: Progressive Learning from Complex Explanation Traces of GPT-4.](https://arxiv.org/abs/2306.02707)

In our example we will use an already existing dataset called [sql-create-context](https://huggingface.co/datasets/b-mc2/sql-create-context), which contains samples of natural language instructions, schema definitions and the corresponding SQL query.

With the latest release of `trl` we now support popular instruction and conversation dataset formats. This means we only need to convert our dataset to one of the supported formats and `trl` will take care of the rest. Those formats include:

- conversational format

JSON

```json
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
```

- instruction format

JSON

```json
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
```

In our example we are going to load our open-source dataset using the 🤗 Datasets library and then convert it into the the conversational format, where we include the schema definition in the system message for our assistant. We'll then save the dataset as jsonl file, which we can then use to fine-tune our model. We are randomly downsampling the dataset to only 10,000 samples.

*Note: This step can be different for your use case. For example, if you have already a dataset from, e.g. working with OpenAI, you can skip this step and go directly to the fine-tuning step.*

Python

```python
from datasets import load_dataset
 
# Convert dataset to OAI messages
system_message = """You are an text to SQL query translator. Users will ask you questions in English and you will generate a SQL query based on the provided SCHEMA.
SCHEMA:
{schema}"""
 
def create_conversation(sample):
  return {
    "messages": [
      {"role": "system", "content": system_message.format(schema=sample["context"])},
      {"role": "user", "content": sample["question"]},
      {"role": "assistant", "content": sample["answer"]}
    ]
  }
 
# Load dataset from the hub
dataset = load_dataset("b-mc2/sql-create-context", split="train")
dataset = dataset.shuffle().select(range(12500))
 
# Convert dataset to OAI messages
dataset = dataset.map(create_conversation, remove_columns=dataset.features,batched=False)
# split dataset into 10,000 training samples and 2,500 test samples
dataset = dataset.train_test_split(test_size=2500/12500)
 
print(dataset["train"][345]["messages"])
 
# save datasets to disk
dataset["train"].to_json("train_dataset.json", orient="records")
dataset["test"].to_json("test_dataset.json", orient="records")
```

## 4\. Fine-tune LLM using `trl` and the `SFTTrainer`

We are now ready to fine-tune our model. We will use the [SFTTrainer](https://huggingface.co/docs/trl/sft_trainer) from `trl` to fine-tune our model. The `SFTTrainer` makes it straightfoward to supervise fine-tune open LLMs. The `SFTTrainer` is a subclass of the `Trainer` from the `transformers` library and supports all the same features, including logging, evaluation, and checkpointing, but adds additiional quality of life features, including:

- Dataset formatting, including conversational and instruction format
- Training on completions only, ignoring prompts
- Packing datasets for more efficient training
- PEFT (parameter-efficient fine-tuning) support including Q-LoRA
- Preparing the model and tokenizer for conversational fine-tuning (e.g. adding special tokens)

We will use the dataset formatting, packing and PEFT features in our example. As peft method we will use [QLoRA](https://arxiv.org/abs/2305.14314) a technique to reduce the memory footprint of large language models during finetuning, without sacrificing performance by using quantization. If you want to learn more about QLoRA and how it works, check out [Making LLMs even more accessible with bitsandbytes, 4-bit quantization and QLoRA](https://huggingface.co/blog/4bit-transformers-bitsandbytes) blog post.

Now, lets get started! 🚀 Let's load our json dataset from disk.

Python

```python
from datasets import load_dataset
 
# Load jsonl data from disk
dataset = load_dataset("json", data_files="train_dataset.json", split="train")
```

Next, we will load our LLM. For our use case we are going to use CodeLlama 7B. CodeLlama is a Llama model trained for general code synthesis and understanding. But we can easily swap out the model for another model, e.g. [Mistral](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2) or [Mixtral](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1) models, TII [Falcon](https://huggingface.co/tiiuae/falcon-40b), or any other LLMs by changing our `model_id` variable. We will use bitsandbytes to quantize our model to 4-bit.

*Note: Be aware the bigger the model the more memory it will require. In our example we will use the 7B version, which can be tuned on 24GB GPUs. If you have a smaller GPU.*

Correctly, preparing the model and tokenizer for training chat/conversational models is crucial. We need to add new special tokens to the tokenizer and model to teach them the different roles in a conversation. In `trl` we have a convenient method with [setup\_chat\_format](https://huggingface.co/docs/trl/main/en/sft_trainer#add-special-tokens-for-chat-format), which:

- Adds special tokens to the tokenizer, e.g. `<|im_start|>` and `<|im_end|>`, to indicate the start and end of a conversation.
- Resizes the model’s embedding layer to accommodate the new tokens.
- Sets the `chat_template` of the tokenizer, which is used to format the input data into a chat-like format. The default is `chatml` from OpenAI.

Python

```python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, BitsAndBytesConfig
from trl import setup_chat_format
 
# Hugging Face model id
model_id = "codellama/CodeLlama-7b-hf" # or `mistralai/Mistral-7B-v0.1`
 
# BitsAndBytesConfig int-4 config
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True, bnb_4bit_use_double_quant=True, bnb_4bit_quant_type="nf4", bnb_4bit_compute_dtype=torch.bfloat16
)
 
# Load model and tokenizer
model = AutoModelForCausalLM.from_pretrained(
    model_id,
    device_map="auto",
    attn_implementation="flash_attention_2",
    torch_dtype=torch.bfloat16,
    quantization_config=bnb_config
)
tokenizer = AutoTokenizer.from_pretrained(model_id)
tokenizer.padding_side = 'right' # to prevent warnings
 
# # set chat template to OAI chatML, remove if you start from a fine-tuned model
model, tokenizer = setup_chat_format(model, tokenizer)
```

The `SFTTrainer`  supports a native integration with `peft`, which makes it super easy to efficiently tune LLMs using, e.g. QLoRA. We only need to create our `LoraConfig` and provide it to the trainer. Our `LoraConfig` parameters are defined based on the [qlora paper](https://arxiv.org/pdf/2305.14314.pdf) and sebastian's [blog post](https://magazine.sebastianraschka.com/p/practical-tips-for-finetuning-llms).

Python

```python
from peft import LoraConfig
 
# LoRA config based on QLoRA paper & Sebastian Raschka experiment
peft_config = LoraConfig(
        lora_alpha=128,
        lora_dropout=0.05,
        r=256,
        bias="none",
        target_modules="all-linear",
        task_type="CAUSAL_LM",
)
```

Before we can start our training we need to define the hyperparameters (`TrainingArguments`) we want to use.

Python

```python
from transformers import TrainingArguments
 
args = TrainingArguments(
    output_dir="code-llama-7b-text-to-sql", # directory to save and repository id
    num_train_epochs=3,                     # number of training epochs
    per_device_train_batch_size=3,          # batch size per device during training
    gradient_accumulation_steps=2,          # number of steps before performing a backward/update pass
    gradient_checkpointing=True,            # use gradient checkpointing to save memory
    optim="adamw_torch_fused",              # use fused adamw optimizer
    logging_steps=10,                       # log every 10 steps
    save_strategy="epoch",                  # save checkpoint every epoch
    learning_rate=2e-4,                     # learning rate, based on QLoRA paper
    bf16=True,                              # use bfloat16 precision
    tf32=True,                              # use tf32 precision
    max_grad_norm=0.3,                      # max gradient norm based on QLoRA paper
    warmup_ratio=0.03,                      # warmup ratio based on QLoRA paper
    lr_scheduler_type="constant",           # use constant learning rate scheduler
    push_to_hub=True,                       # push model to hub
    report_to="tensorboard",                # report metrics to tensorboard
)
```

We now have every building block to create our `SFTTrainer` and start training our model.

Python

```python
from trl import SFTTrainer
 
max_seq_length = 3072 # max sequence length for model and packing of the dataset
 
trainer = SFTTrainer(
    model=model,
    args=args,
    train_dataset=dataset,
    peft_config=peft_config,
    max_seq_length=max_seq_length,
    tokenizer=tokenizer,
    packing=True,
    dataset_kwargs={
        "add_special_tokens": False,  # We template with special tokens
        "append_concat_token": False, # No need to add additional separator token
    }
)
```

We can start training our model by calling the `train()` method on our `Trainer` instance. This will start the training loop and train our model for 3 epochs. Since we are using a PEFT method, we will only save the adapted model weights and not the full model.

Python

```python
# start training, the model will be automatically saved to the hub and the output directory
trainer.train()
 
# save model
trainer.save_model()
```

The training with Flash Attention for 3 epochs with a dataset of 10k samples took 01:29:58 on a `g5.2xlarge`. The instance costs `1,212$/h` which brings us to a total cost of only `1.8$`.

Python

```python
# free the memory again
del model
del trainer
torch.cuda.empty_cache()
```

### *Optional: Merge LoRA adapter in to the original model*

When using QLoRA, we only train adapters and not the full model. This means when saving the model during training we only save the adapter weights and not the full model. If you want to save the full model, which makes it easier to use with Text Generation Inference you can merge the adapter weights into the model weights using the `merge_and_unload` method and then save the model with the `save_pretrained` method. This will save a default model, which can be used for inference.

*Note: You might require > 30GB CPU Memory.*

Python

```python
 
#### COMMENT IN TO MERGE PEFT AND BASE MODEL ####
# from peft import AutoPeftModelForCausalLM
 
# # Load PEFT model on CPU
# model = AutoPeftModelForCausalLM.from_pretrained(
#     args.output_dir,
#     torch_dtype=torch.float16,
#     low_cpu_mem_usage=True,
# )
# # Merge LoRA and base model and save
# merged_model = model.merge_and_unload()
# merged_model.save_pretrained(args.output_dir,safe_serialization=True, max_shard_size="2GB")
```

## 5\. Test and evaluate the LLM

After the training is done we want to evaluate and test our model. We will load different samples from the original dataset and evaluate the model on those samples, using a simple loop and accuracy as our metric.

*Note: Evaluating Generative AI models is not a trivial task since 1 input can have multiple correct outputs. If you want to learn more about evaluating generative models, check out [Evaluate LLMs and RAG a practical example using Langchain and Hugging Face](https://www.philschmid.de/evaluate-llm) blog post.*

Python

```python
import torch
from peft import AutoPeftModelForCausalLM
from transformers import AutoTokenizer, pipeline
 
peft_model_id = "./code-llama-7b-text-to-sql"
# peft_model_id = args.output_dir
 
# Load Model with PEFT adapter
model = AutoPeftModelForCausalLM.from_pretrained(
  peft_model_id,
  device_map="auto",
  torch_dtype=torch.float16
)
tokenizer = AutoTokenizer.from_pretrained(peft_model_id)
# load into pipeline
pipe = pipeline("text-generation", model=model, tokenizer=tokenizer)
```

Let’s load our test dataset try to generate an instruction.

Python

```python
from datasets import load_dataset
from random import randint
 
 
# Load our test dataset
eval_dataset = load_dataset("json", data_files="test_dataset.json", split="train")
rand_idx = randint(0, len(eval_dataset))
 
# Test on sample
prompt = pipe.tokenizer.apply_chat_template(eval_dataset[rand_idx]["messages"][:2], tokenize=False, add_generation_prompt=True)
outputs = pipe(prompt, max_new_tokens=256, do_sample=False, temperature=0.1, top_k=50, top_p=0.1, eos_token_id=pipe.tokenizer.eos_token_id, pad_token_id=pipe.tokenizer.pad_token_id)
 
print(f"Query:\n{eval_dataset[rand_idx]['messages'][1]['content']}")
print(f"Original Answer:\n{eval_dataset[rand_idx]['messages'][2]['content']}")
print(f"Generated Answer:\n{outputs[0]['generated_text'][len(prompt):].strip()}")
```

Nice! Our model was able to generate a SQL query based on the natural language instruction. Lets evaluate our model on the full 2,500 samples of our test dataset. *Note: As mentioned above, evaluating generative models is not a trivial task. In our example we used the accuracy of the generated SQL based on the ground truth SQL query as our metric. An alternative way could be to automatically execute the generated SQL query and compare the results with the ground truth. This would be a more accurate metric but requires more work to setup.*

Python

```python
from tqdm import tqdm
 
 
def evaluate(sample):
    prompt = pipe.tokenizer.apply_chat_template(sample["messages"][:2], tokenize=False, add_generation_prompt=True)
    outputs = pipe(prompt, max_new_tokens=256, do_sample=True, temperature=0.7, top_k=50, top_p=0.95, eos_token_id=pipe.tokenizer.eos_token_id, pad_token_id=pipe.tokenizer.pad_token_id)
    predicted_answer = outputs[0]['generated_text'][len(prompt):].strip()
    if predicted_answer == sample["messages"][2]["content"]:
        return 1
    else:
        return 0
 
success_rate = []
number_of_eval_samples = 1000
# iterate over eval dataset and predict
for s in tqdm(eval_dataset.shuffle().select(range(number_of_eval_samples))):
    success_rate.append(evaluate(s))
 
# compute accuracy
accuracy = sum(success_rate)/len(success_rate)
 
print(f"Accuracy: {accuracy*100:.2f}%")
 
```

We evaluated our model on 1000 samples from the evaluation dataset and got an accuracy of `79.50%`, which took ~25 minutes.

This is quite good, but as mentioned you need to take this metric with a grain of salt. It would be better if we could evaluate our model by running the qureies against a real database and compare the results. Since there might be different "correct" SQL queries for the same instruction. There are also several ways on how we could improve the performance by using few-shot learning, using RAG, Self-healing to generate the SQL query.

## 6\. Deploy the LLM for Production

You can now deploy your model to production. For deploying open LLMs into production we recommend using [Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference). TGI is a purpose-built solution for deploying and serving Large Language Models (LLMs). TGI enables high-performance text generation using Tensor Parallelism and continous batching for the most popular open LLMs, including Llama, Mistral, Mixtral, StarCoder, T5 and more. Text Generation Inference is used by companies as IBM, Grammarly, Uber, Deutsche Telekom, and many more. There are several ways to deploy your model, including:

- [Deploy LLMs with Hugging Face Inference Endpoints](https://huggingface.co/blog/inference-endpoints-llm)
- [Hugging Face LLM Inference Container for Amazon SageMaker](https://huggingface.co/blog/sagemaker-huggingface-llm)
- DIY

If you have docker installed you can use the following command to start the inference server.

*Note: Make sure that you have enough GPU memory to run the container. Restart kernel to remove all allocated GPU memory from the notebook.*

Bash

```bash
%%bash
# model=$PWD/{args.output_dir} # path to model
model=$(pwd)/code-llama-7b-text-to-sql # path to model
num_shard=1             # number of shards
max_input_length=1024   # max input length
max_total_tokens=2048   # max total tokens
 
docker run -d --name tgi --gpus all -ti -p 8080:80 \
  -e MODEL_ID=/workspace \
  -e NUM_SHARD=$num_shard \
  -e MAX_INPUT_LENGTH=$max_input_length \
  -e MAX_TOTAL_TOKENS=$max_total_tokens \
  -v $model:/workspace \
  ghcr.io/huggingface/text-generation-inference:latest
```

Once your container is running you can send requests.

Python

```python
import requests as r
from transformers import AutoTokenizer
from datasets import load_dataset
from random import randint
 
# Load our test dataset and Tokenizer again
tokenizer = AutoTokenizer.from_pretrained("code-llama-7b-text-to-sql")
eval_dataset = load_dataset("json", data_files="test_dataset.json", split="train")
rand_idx = randint(0, len(eval_dataset))
 
# generate the same prompt as for the first local test
prompt = tokenizer.apply_chat_template(eval_dataset[rand_idx]["messages"][:2], tokenize=False, add_generation_prompt=True)
request= {"inputs":prompt,"parameters":{"temperature":0.2, "top_p": 0.95, "max_new_tokens": 256}}
 
# send request to inference server
resp = r.post("http://127.0.0.1:8080/generate", json=request)
 
output = resp.json()["generated_text"].strip()
time_per_token = resp.headers.get("x-time-per-token")
time_prompt_tokens = resp.headers.get("x-prompt-tokens")
 
# Print results
print(f"Query:\n{eval_dataset[rand_idx]['messages'][1]['content']}")
print(f"Original Answer:\n{eval_dataset[rand_idx]['messages'][2]['content']}")
print(f"Generated Answer:\n{output}")
print(f"Latency per token: {time_per_token}ms")
print(f"Latency prompt encoding: {time_prompt_tokens}ms")
```

Awesome, Don't forget to stop your container once you are done.

Python

```python
!docker stop tgi
```

## Conclusion

Large Language Models and the availability of tools TRL make it an ideal time for companies to invest in open LLM technology. Fine-tuning open LLMs for specific tasks can significantly enhance efficiency and open new opportunities for innovation and improved services. With the increasing accessibility and cost-effectiveness, there has never been a better time to start using open LLMs.

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
