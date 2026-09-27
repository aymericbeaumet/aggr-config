---
title: Efficiently fine-tune Llama 3 with PyTorch FSDP and Q-Lora
link: https://www.philschmid.de/fsdp-qlora-llama3
source: philschmid-de
published: 2024-04-22T00:00:00Z
updated: 2024-04-22T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to fine-tune Llama 3 70b with PyTorch FSDP and Q-Lora using Hugging Face TRL, Transformers, PEFT and Datasets.
content: extracted
html: 2024-04-22-efficiently-fine-tune-llama-3-with-pytorch-fsdp-and-q-lora.html
preview:
  file: 2024-04-22-efficiently-fine-tune-llama-3-with-pytorch-fsdp-and-q-lora.preview-1709111b87b9.webp
  width: 256
  height: 128
  alt: Efficiently fine-tune Llama 3 with PyTorch FSDP and Q-Lora
  color: '#687e70'
images:
- source: https://www.philschmid.de/static/blog/fsdp-qlora-llama3/thumbnail.jpg
  original:
    file: 2024-04-22-efficiently-fine-tune-llama-3-with-pytorch-fsdp-and-q-lora.image-ac3fda9d159e.jpg
    width: 1200
    height: 600
  color: '#131503'
---

Open LLMs like Meta [Llama 3](https://huggingface.co/meta-llama/Meta-Llama-3-70b), Mistral AI [Mistral](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2) & [Mixtral](https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1) models or AI21 [Jamba](https://huggingface.co/ai21labs/Jamba-v0.1) are now OpenAI competitors. However, most of the time you need to fine-tune the model on your data to unlock the full potential of the model. Fine-tuning smaller LLMs, like Mistral became very accessible on a single GPU by using Q-Lora. But efficiently fine-tuning bigger models like Llama 3 70b or Mixtral stayed a challenge until now.

This blog post walks you thorugh how to fine-tune a Llama 3 using PyTorch FSDP and Q-Lora with the help of Hugging Face [TRL](https://huggingface.co/docs/trl/index), [Transformers](https://huggingface.co/docs/transformers/index), [peft](https://huggingface.co/docs/peft/index) & [datasets](https://huggingface.co/docs/datasets/index). In addition to FSDP we will use [Flash Attention v2 through the Pytorch SDPA](https://pytorch.org/blog/pytorch2-2/) implementation.

1. [Setup development environment](https://www.philschmid.de/fsdp-qlora-llama3#1-setup-development-environment)
2. [Create and prepare the dataset](https://www.philschmid.de/fsdp-qlora-llama3#2-create-and-prepare-the-dataset)
3. [Fine-tune the LLM with PyTorch FSDP, Q-Lora and SDPA](https://www.philschmid.de/fsdp-qlora-llama3#3-fine-tune-the-llm-with-pytorch-fsdp-q-lora-and-sdpa)
4. [Test Model and run Inference](https://www.philschmid.de/fsdp-qlora-llama3#4-test-model-and-run-inference)

*Note: This blog was created and validated on NVIDIA H100 and NVIDIA A10G GPUs. The configurations and code is optimized for 4xA10G GPUs each with 24GB of Memory. I hope this keeps the example as accessible as possible for most people. If you have access to more compute you can make changes to the config (yaml) at step 3.*

**FSDP + Q-Lora Background**

In a collaboration between [Answer.AI](https://www.answer.ai/posts/2024-03-06-fsdp-qlora.html), Tim Dettmers [Q-Lora creator](https://github.com/TimDettmers/bitsandbytes) and [Hugging Face](https://huggingface.co/), we are proud to announce to share the support of Q-Lora and PyTorch FSDP (Fully Sharded Data Parallel). FSDP and Q-Lora allows you now to fine-tune Llama 2 70b or Mixtral 8x7B on 2x consumer GPUs (24GB). If you want to learn more about the background of this collaboration take a look at [You can now train a 70b language model at home](https://www.answer.ai/posts/2024-03-06-fsdp-qlora.html). Hugging Face PEFT is were the magic happens for this happens, read more about it in the [PEFT documentation](https://huggingface.co/docs/peft/v0.10.0/en/accelerate/fsdp).

- [PyTorch FSDP](https://pytorch.org/blog/introducing-pytorch-fully-sharded-data-parallel-api/) is a data/model parallelism technique that shards model across GPUs, reducing memory requirements and enabling the training of larger models more efficiently.
- Q-LoRA is a fine-tuning method that leverages quantization and Low-Rank Adapters to efficiently reduced computational requirements and memory footprint.

## 1\. Setup development environment

Our first step is to install Hugging Face Libraries and Pyroch, including trl, transformers and datasets. If you haven't heard of trl yet, don't worry. It is a new library on top of transformers and datasets, which makes it easier to fine-tune, rlhf, align open LLMs.

Python

```python
# Install Pytorch for FSDP and FA/SDPA
%pip install "torch==2.2.2" tensorboard
 
# Install Hugging Face libraries
%pip install  --upgrade "transformers==4.40.0" "datasets==2.18.0" "accelerate==0.29.3" "evaluate==0.4.1" "bitsandbytes==0.43.1" "huggingface_hub==0.22.2" "trl==0.8.6" "peft==0.10.0"
```

Next we need to login into Hugging Face to access the Llama 3 70b model. If you don't have an account yet and accepted the terms, you can create one [here](https://huggingface.co/join).

Python

```python
!huggingface-cli login --token ""
```

## 2\. Create and prepare the dataset

After our environment is set up, we can start creating and preparing our dataset. A fine-tuning dataset should have a diverse set of demonstrations of the task you want to solve. If you want to learn more about how to create a dataset, take a look at the [How to Fine-Tune LLMs in 2024 with Hugging Face](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#3-create-and-prepare-the-dataset).

We will use the [HuggingFaceH4/no\_robots](https://huggingface.co/datasets/HuggingFaceH4/no_robots) dataset a high-quality dataset of 10,000 instructions and demonstrations created by skilled human annotators. This data can be used for supervised fine-tuning (SFT) to make language models follow instructions better. No Robots was modelled after the instruction dataset described in OpenAI's [InstructGPT paper](https://huggingface.co/papers/2203.02155), and is comprised mostly of single-turn instructions.

JSON

```json
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
```

The [no\_robots](https://huggingface.co/datasets/HuggingFaceH4/no_robots) dataset has 10,000 split into 9,500 training and 500 test examples. Some samples are not including a `system` message. We will load the dataset with the `datasets` library, add a missing `system` message and save them to separate json files.

Python

```python
from datasets import load_dataset
 
# Convert dataset to OAI messages
system_message = """You are Llama, an AI assistant created by Philipp to be helpful and honest. Your knowledge spans a wide range of topics, allowing you to engage in substantive conversations and provide analysis on complex subjects."""
 
def create_conversation(sample):
    if sample["messages"][0]["role"] == "system":
        return sample
    else:
      sample["messages"] = [{"role": "system", "content": system_message}] + sample["messages"]
      return sample
 
# Load dataset from the hub
dataset = load_dataset("HuggingFaceH4/no_robots")
 
# Add system message to each conversation
columns_to_remove = list(dataset["train"].features)
columns_to_remove.remove("messages")
dataset = dataset.map(create_conversation, remove_columns=columns_to_remove,batched=False)
 
# Filter out conversations which are corrupted with wrong turns, keep which have even number of turns after adding system message
dataset["train"] = dataset["train"].filter(lambda x: len(x["messages"][1:]) % 2 == 0)
dataset["test"] = dataset["test"].filter(lambda x: len(x["messages"][1:]) % 2 == 0)
 
# save datasets to disk
dataset["train"].to_json("train_dataset.json", orient="records", force_ascii=False)
dataset["test"].to_json("test_dataset.json", orient="records", force_ascii=False)
```

## 3\. Fine-tune the LLM with PyTorch FSDP, Q-Lora and SDPA

We are now ready to fine-tune our model with PyTorch FSDP, Q-Lora and SDPA. Since we are running in a distributed setup, we need to use `torchrun` and a python script to start the training.

We prepared a script [run\_fsdp\_qlora.py](https://github.com/philschmid/deep-learning-pytorch-huggingface/blob/main/training/scripts/run_fsdp_qlora.py) which will load the dataset from disk, prepare the model, tokenizer and start the training. It usees the [SFTTrainer](https://huggingface.co/docs/trl/sft_trainer) from `trl` to fine-tune our model. The `SFTTrainer` makes it straightfoward to supervise fine-tune open LLMs supporting:

- Dataset formatting, including conversational and instruction format (✅ used)
- Training on completions only, ignoring prompts (❌ not used)
- Packing datasets for more efficient training (✅ used)
- PEFT (parameter-efficient fine-tuning) support including Q-LoRA (✅ used)
- Preparing the model and tokenizer for conversational fine-tuning (❌ not used, see below)

*Note: We are using an Anthropic/Vicuna like Chat Template with `User:` and `Assistant:` roles. This done because the special tokens in base Llama 3 (`<|begin_of_text|>` or `<|reserved_special_token_XX|>`) are not trained. Meaning if want would like to use them for the template we need to train them which requires more memory, since we need to update the embedding layer and lm\_head. If you have access to more compute you can modify `LLAMA_3_CHAT_TEMPLATE` in the [run\_fsdp\_qlora.py](https://github.com/philschmid/deep-learning-pytorch-huggingface/blob/main/training/scripts/run_fsdp_qlora.py) script.*

For configuration we use the new `TrlParser`, that allows us to provide hyperparameters in a yaml file or overwrite the arguments from the config file by explicitly passing them to the CLI, e.g. `--num_epochs 10`. Below is the config file for fine-tuning Llama 3 70B on 4x A10G GPUs or 4x24GB GPUs.

Python

```python
%%writefile llama_3_70b_fsdp_qlora.yaml
# script parameters
model_id: "meta-llama/Meta-Llama-3-70b" # Hugging Face model id
dataset_path: "."                      # path to dataset
max_seq_len:  3072 # 2048              # max sequence length for model and packing of the dataset
# training parameters
output_dir: "./llama-3-70b-hf-no-robot" # Temporary output directory for model checkpoints
report_to: "tensorboard"               # report metrics to tensorboard
learning_rate: 0.0002                  # learning rate 2e-4
lr_scheduler_type: "constant"          # learning rate scheduler
num_train_epochs: 3                    # number of training epochs
per_device_train_batch_size: 1         # batch size per device during training
per_device_eval_batch_size: 1          # batch size for evaluation
gradient_accumulation_steps: 2         # number of steps before performing a backward/update pass
optim: adamw_torch                     # use torch adamw optimizer
logging_steps: 10                      # log every 10 steps
save_strategy: epoch                   # save checkpoint every epoch
evaluation_strategy: epoch             # evaluate every epoch
max_grad_norm: 0.3                     # max gradient norm
warmup_ratio: 0.03                     # warmup ratio
bf16: true                             # use bfloat16 precision
tf32: true                             # use tf32 precision
gradient_checkpointing: true           # use gradient checkpointing to save memory
# FSDP parameters: https://huggingface.co/docs/transformers/main/en/fsdp
fsdp: "full_shard auto_wrap offload" # remove offload if enough GPU memory
fsdp_config:
  backward_prefetch: "backward_pre"
  forward_prefetch: "false"
  use_orig_params: "false"
```

*Note: At the end of the training there will be a slight increase in GPU memory usage (~10%). This is due to the saving of the model correctly. Make sure to have enough memory left on your GPU to save the model. [REF](https://huggingface.co/docs/peft/v0.10.0/en/accelerate/fsdp#memory-usage)*

To launch our training we will use `torchrun` to keep the example flexible and easy to adjust to, e.g. Amazon SageMaker or Google Cloud Vertex AI. For `torchrun` and FSDP we need to set the environment variable `ACCELERATE_USE_FSDP` and `FSDP_CPU_RAM_EFFICIENT_LOADING` to tell transformers/accelerate to use FSDP and load the model in a memory-efficient way.

*Note: To NOT CPU offloading you need to change the value of `fsdp` and remove `offload`. This only works on > 40GB GPUs since it requires more memory.*

Now, lets launch the training with the following command:

Python

```python
!ACCELERATE_USE_FSDP=1 FSDP_CPU_RAM_EFFICIENT_LOADING=1 torchrun --nproc_per_node=4 ./scripts/run_fsdp_qlora.py --config llama_3_70b_fsdp_qlora.yaml
```

**Expected Memory usage:**

- Full-finetuning with FSDP needs ~16X80GB GPUs
- FSDP + LoRA needs ~8X80GB GPUs
- FSDP + Q-Lora needs ~2x40GB GPUs
- FSDP + Q-Lora + CPU offloading needs 4x24GB GPUs, with 22 GB/GPU and 127 GB CPU RAM with a sequence length of 3072 and a batch size of 1.

The training of Llama 3 70B with Flash Attention for 3 epochs with a dataset of 10k samples takes 45h on a `g5.12xlarge`. The instance costs `5.67$/h` which would result in a total cost of `255.15$`. This sounds expensive but allows you to fine-tune a Llama 3 70B on small GPU resources. If we scale up the training to 4x H100 GPUs, the training time will be reduced to ~1,25h. If we assume 1x H100 costs `5-10$/h` the total cost would between `25$-50$`.

We can see a trade-off between accessibility and performance. If you have access to more/better compute you can reduce the training time and cost, but even with small resources you can fine-tune a Llama 3 70B. The cost/performance is different since for 4x A10G GPUs we need to offload the model to the CPU which reduces the overall flops.

*Note: During evaluation and testing of the blog post I noticed that ~40 max steps (80 samples stacked to 3k sequence length) are enough for first results. The training for 40 steps ~1h or ~$5.*

### *Optional: Merge LoRA adapter in to the original model*

When using QLoRA, we only train adapters and not the full model. This means when saving the model during training we only save the adapter weights and not the full model. If you want to save the full model, which makes it easier to use with Text Generation Inference you can merge the adapter weights into the model weights using the `merge_and_unload` method and then save the model with the `save_pretrained` method. This will save a default model, which can be used for inference.

*Note: You might require > 192GB CPU Memory.*

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

## 4\. Test Model and run Inference

After the training is done we want to evaluate and test our model. We will load different samples from the original dataset and evaluate the model manually. Evaluating Generative AI models is not a trivial task since 1 input can have multiple correct outputs. If you want to learn more about evaluating generative models, check out [Evaluate LLMs and RAG a practical example using Langchain and Hugging Face](https://www.philschmid.de/evaluate-llm) blog post.

Python

```python
import torch
from peft import AutoPeftModelForCausalLM
from transformers import AutoTokenizer
 
peft_model_id = "./llama-3-70b-hf-no-robot"
 
# Load Model with PEFT adapter
model = AutoPeftModelForCausalLM.from_pretrained(
  peft_model_id,
  torch_dtype=torch.float16,
  quantization_config= {"load_in_4bit": True},
  device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained(peft_model_id)
```

Let’s load our test dataset try to generate an instruction.

Python

```python
from datasets import load_dataset
from random import randint
 
 
# Load our test dataset
eval_dataset = load_dataset("json", data_files="test_dataset.json", split="train")
rand_idx = randint(0, len(eval_dataset))
messages = eval_dataset[rand_idx]["messages"][:2]
 
# Test on sample
input_ids = tokenizer.apply_chat_template(messages,add_generation_prompt=True,return_tensors="pt").to(model.device)
outputs = model.generate(
    input_ids,
    max_new_tokens=512,
    eos_token_id= tokenizer.eos_token_id,
    do_sample=True,
    temperature=0.6,
    top_p=0.9,
)
response = outputs[0][input_ids.shape[-1]:]
 
print(f"**Query:**\n{eval_dataset[rand_idx]['messages'][1]['content']}\n")
print(f"**Original Answer:**\n{eval_dataset[rand_idx]['messages'][2]['content']}\n")
print(f"**Generated Answer:**\n{tokenizer.decode(response,skip_special_tokens=True)}")
 
# **Query:**
# How long was the Revolutionary War?
# **Original Answer:**
# The American Revolutionary War lasted just over seven years. The war started on April 19, 1775, and ended on September 3, 1783.
# **Generated Answer:**
# The Revolutionary War, also known as the American Revolution, was an 18th-century war fought between the Kingdom of Great Britain and the Thirteen Colonies. The war lasted from 1775 to 1783.
```

That looks pretty good! 🚀 Now, its your turn!

If you want to deploy your model into production check out [Deploy the LLM for Production](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#6-deploy-the-llm-for-production).
