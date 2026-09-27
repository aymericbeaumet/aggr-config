---
title: How to fine-tune Google Gemma with ChatML and Hugging Face TRL
link: https://www.philschmid.de/fine-tune-google-gemma
source: philschmid-de
published: 2024-03-01T00:00:00Z
updated: 2024-03-01T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to fine tune Google Gemma using Hugging Face Transformers, Datasets and TRL.
content: extracted
html: 2024-03-01-how-to-fine-tune-google-gemma-with-chatml-and-hugging-face.html
preview:
  file: 2024-03-01-how-to-fine-tune-google-gemma-with-chatml-and-hugging-face.preview-970ec6b6882b.webp
  width: 256
  height: 146
  alt: How to fine-tune Google Gemma with ChatML and Hugging Face TRL
  color: '#2563c5'
images:
- source: https://www.philschmid.de/static/blog/fine-tune-google-gemma/thumbnail.jpg
  original:
    file: 2024-03-01-how-to-fine-tune-google-gemma-with-chatml-and-hugging-face.image-84066316b19b.jpg
    width: 1792
    height: 1024
  color: '#0556dd'
---

Last week, Google released [Gemma](https://huggingface.co/blog/gemma), a new family of state-of-the-art open LLMs. Gemma comes in two sizes: 7B parameters, for efficient deployment and development on consumer-size GPU and TPU and 2B versions for CPU and on-device applications. Both come in base and instruction-tuned variants.

After the first week it seemed that Gemma is not very friendly to fine-tune using the [ChatML format](https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/ai-services/openai/includes/chat-markup-language.md#working-with-chat-markup-language-chatml), which is adapted and used by the open soruce community, e.g. OpenHermes or Dolphin. I created this blog post to show you how to fine-tune Gemma using ChatML and Hugging Face TRL.

This blog post is derived from my [How to Fine-Tune LLMs in 2024 with Hugging Face](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl) blog tailored to fine-tune Gemma 7B. We will use Hugging Face [TRL](https://huggingface.co/docs/trl/index), [Transformers](https://huggingface.co/docs/transformers/index) & [datasets](https://huggingface.co/docs/datasets/index).

1. [Setup development environment](https://www.philschmid.de/fine-tune-google-gemma#1-setup-development-environment)
2. [Create and prepare the dataset](https://www.philschmid.de/fine-tune-google-gemma#2-create-and-prepare-the-dataset)
3. [Fine-tune LLM using `trl` and the `SFTTrainer`](https://www.philschmid.de/fine-tune-google-gemma#3-fine-tune-llm-using-trl-and-the-sfttrainer)
4. [Test and evaluate the LLM](https://www.philschmid.de/fine-tune-google-gemma#3-test-model-and-run-inference)

*Note: This blog was created to run on consumer size GPUs (24GB), e.g. NVIDIA A10G or RTX 4090/3090, but can be easily adapted to run on bigger GPUs.*

## 1\. Setup development environment

Our first step is to install Hugging Face Libraries and Pyroch, including trl, transformers and datasets. If you haven't heard of trl yet, don't worry. It is a new library on top of transformers and datasets, which makes it easier to fine-tune, rlhf, align open LLMs.

Python

```python
# Install Pytorch & other libraries
!pip install "torch==2.1.2" tensorboard
 
# Install Hugging Face libraries
!pip install  --upgrade \
  "transformers==4.38.2" \
  "datasets==2.16.1" \
  "accelerate==0.26.1" \
  "evaluate==0.4.1" \
  "bitsandbytes==0.42.0" \
  "trl==0.7.11" \
  "peft==0.8.2"
```

If you are using a GPU with Ampere architecture (e.g. NVIDIA A10G or RTX 4090/3090) or newer you can use Flash attention. Flash Attention is a an method that reorders the attention computation and leverages classical techniques (tiling, recomputation) to significantly speed it up and reduce memory usage from quadratic to linear in sequence length. The TL;DR; accelerates training up to 3x. Learn more at [FlashAttention](https://github.com/Dao-AILab/flash-attention/tree/main).

*Note: If your machine has less than 96GB of RAM and lots of CPU cores, reduce the number of `MAX_JOBS`. On the `g5.2xlarge` we used `4`.*

Python

```python
import torch; assert torch.cuda.get_device_capability()[0] >= 8, 'Hardware not supported for Flash Attention'
# install flash-attn
!pip install ninja packaging
!MAX_JOBS=4 pip install flash-attn --no-build-isolation --upgrade
```

*Installing flash attention from source can take quite a bit of time (10-45 minutes).*

We will also need to login into our [Hugging Face](https://huggingface.co) account to be able to access Gemma. To use Gemma you first need to agree to the terms of use. You can do this by visiting the [Gemma page](https://huggingface.co/google/gemma-7b) following the gate mechanism.

Python

```python
from huggingface_hub import login
 
login(
  token="", # ADD YOUR TOKEN HERE
  add_to_git_credential=True
)
 
```

## 2\. Create and prepare the dataset

We are not going to focus on creating a dataset in this blog post. If you want to learn more about creating a dataset, I recommend reading the [How to Fine-Tune LLMs in 2024 with Hugging Face](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl) blog post. We are going to use the Databricks Dolly datatset, formated already as messages. This means we can use the `conversational` format to fine-tune our model.

JSON

```json
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
{"messages": [{"role": "system", "content": "You are..."}, {"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}
```

The latest release of `trl` supports the conversation dataset formats. This means don't need to do any additional formatting of the dataset. We can use the dataset as is.

Python

```python
from datasets import load_dataset
 
# Load Dolly Dataset.
dataset = load_dataset("philschmid/dolly-15k-oai-style", split="train")
 
print(dataset[3]["messages"])
```

## 3\. Fine-tune LLM using `trl` and the `SFTTrainer`

We will use the [SFTTrainer](https://huggingface.co/docs/trl/sft_trainer) from `trl` to fine-tune our model. The `SFTTrainer` makes it straightfoward to supervise fine-tune open LLMs. The `SFTTrainer` is a subclass of the `Trainer` from the `transformers` library and supports all the same features, including logging, evaluation, and checkpointing, but adds additiional quality of life features, including:

- Dataset formatting, including conversational and instruction format
- Training on completions only, ignoring prompts
- Packing datasets for more efficient training
- PEFT (parameter-efficient fine-tuning) support including Q-LoRA
- Preparing the model and tokenizer for conversational fine-tuning (e.g. adding special tokens)

We will use the dataset formatting, packing and PEFT features in our example. As peft method we will use [QLoRA](https://arxiv.org/abs/2305.14314) a technique to reduce the memory footprint of large language models during finetuning, without sacrificing performance by using quantization.

*Note: Gemma comes with a big vocabulary of ~250,000 tokens. Normally if you want to fine-tune LLMs on the ChatML format you would need to add special tokens to the tokenizer and model and teach to understand the different roles in a conversation. But Google included ~100 placeholder tokens in the vocabulary, which we can replace with special tokens, like `<|im_start|>` and `<|im_end|>`. I created a Tokenizer for the ChatML format [philschmid/gemma-tokenizer-chatml](https://huggingface.co/philschmid/gemma-tokenizer-chatml) which you can use to fine-tune Gemma with ChatML.*

The Chat template used during fine-tuning is not 100% compatible with the ChatML format. Since [Google/gemma-7b](https://huggingface.co/google/gemma-7b) requires inputs always to start with a `<bos>` token. This means our inputs will look like.

Xml

```xml
<bos><|im_start|>system
You are Gemma.<|im_end|>
<|im_start|>user
Hello, how are you?<|im_end|>
<|im_start|>assistant
I'm doing great. How can I help you today?<|im_end|>\n<eos>
```

*Note: We are not having an idea why Gemma needs `<bos>` token at the beginning of the input.*

Python

```python
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM, BitsAndBytesConfig
 
# Hugging Face model id
model_id = "google/gemma-7b"
tokenizer_id = "philschmid/gemma-tokenizer-chatml"
 
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
tokenizer = AutoTokenizer.from_pretrained(tokenizer_id)
tokenizer.padding_side = 'right' # to prevent warnings
```

The `SFTTrainer`  supports a native integration with `peft`, which makes it super easy to efficiently tune LLMs using, e.g. QLoRA. We only need to create our `LoraConfig` and provide it to the trainer.

Python

```python
from peft import LoraConfig
 
# LoRA config based on QLoRA paper & Sebastian Raschka experiment
peft_config = LoraConfig(
        lora_alpha=8,
        lora_dropout=0.05,
        r=6,
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
    output_dir="gemma-7b-dolly-chatml", # directory to save and repository id
    num_train_epochs=3,                     # number of training epochs
    per_device_train_batch_size=2,          # batch size per device during training
    gradient_accumulation_steps=2,          # number of steps before performing a backward/update pass
    gradient_checkpointing=True,            # use gradient checkpointing to save memory
    optim="adamw_torch_fused",              # use fused adamw optimizer
    logging_steps=10,                       # log every 10 steps
    save_strategy="epoch",                  # save checkpoint every epoch
    bf16=True,                              # use bfloat16 precision
    tf32=True,                              # use tf32 precision
    learning_rate=2e-4,                     # learning rate, based on QLoRA paper
    max_grad_norm=0.3,                      # max gradient norm based on QLoRA paper
    warmup_ratio=0.03,                      # warmup ratio based on QLoRA paper
    lr_scheduler_type="constant",           # use constant learning rate scheduler
    push_to_hub=False,                       # push model to hub
    report_to="tensorboard",                # report metrics to tensorboard
)
```

We now have every building block we need to create our `SFTTrainer` to start then training our model.

Python

```python
from trl import SFTTrainer
 
max_seq_length = 1512 # max sequence length for model and packing of the dataset
 
trainer = SFTTrainer(
    model=model,
    args=args,
    train_dataset=dataset,
    peft_config=peft_config,
    max_seq_length=max_seq_length,
    tokenizer=tokenizer,
    packing=True,
    dataset_kwargs={
        "add_special_tokens": False, # We template with special tokens
        "append_concat_token": False, # No need to add additional separator token
    }
)
```

Start training our model by calling the `train()` method on our `Trainer` instance. This will start the training loop and train our model for 3 epochs. Since we are using a PEFT method, we will only save the adapted model weights and not the full model.

Python

```python
# start training, the model will be automatically saved to the hub and the output directory
trainer.train()
 
# save model
trainer.save_model()
```

The training with Flash Attention for 3 epochs with a dataset of 15k samples took 4:14:36 on a `g5.2xlarge`. The instance costs `1.21$/h` which brings us to a total cost of only ~`5.3$`.

### Optional: Merge LoRA adapter in to the original model

When using QLoRA, we only train adapters and not the full model. This means when saving the model during training we only save the adapter weights and not the full model. If you want to save the full model, which makes it easier to use with Text Generation Inference you can merge the adapter weights into the model weights using the `merge_and_unload` method and then save the model with the save\_pretrained method.

Check out the [How to Fine-Tune LLMs in 2024 with Hugging Face](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl#optional-merge-lora-adapter-in-to-the-original-model) blog post on how to do it .

## 3\. Test Model and run Inference

After the training is done we want to evaluate and test our model. We will load different samples from the original dataset and evaluate the model on those samples, using a simple loop and accuracy as our metric.

*Note: Evaluating Generative AI models is not a trivial task since 1 input can have multiple correct outputs. If you want to learn more about evaluating generative models, check out [Evaluate LLMs and RAG a practical example using Langchain and Hugging Face](https://www.philschmid.de/evaluate-llm) blog post.*

Python

```python
# free the memory again
del model
del trainer
torch.cuda.empty_cache()
```

We load the adapted model and the tokenize into the `pipeline` to easily test it and extract the token id of `<|im_end|>` to use it in the `generate` method.

Python

```python
import torch
from peft import AutoPeftModelForCausalLM
from transformers import  AutoTokenizer, pipeline
 
peft_model_id = "gemma-7b-dolly-chatml"
 
# Load Model with PEFT adapter
tokenizer = AutoTokenizer.from_pretrained(peft_model_id)
model = AutoPeftModelForCausalLM.from_pretrained(peft_model_id, device_map="auto", torch_dtype=torch.float16)
pipe = pipeline("text-generation", model=model, tokenizer=tokenizer)
# get token id for end of conversation
eos_token = tokenizer("<|im_end|>",add_special_tokens=False)["input_ids"][0]
```

Lets test some prompt samples and see how the model performs.

Python

```python
prompts = [
    "What is the capital of Germany? Explain why thats the case and if it was different in the past?",
    "Write a Python function to calculate the factorial of a number.",
    "A rectangular garden has a length of 25 feet and a width of 15 feet. If you want to build a fence around the entire garden, how many feet of fencing will you need?",
    "What is the difference between a fruit and a vegetable? Give examples of each.",
]
 
def test_inference(prompt):
    prompt = pipe.tokenizer.apply_chat_template([{"role": "user", "content": prompt}], tokenize=False, add_generation_prompt=True)
    outputs = pipe(prompt, max_new_tokens=1024, do_sample=True, temperature=0.7, top_k=50, top_p=0.95, eos_token_id=eos_token)
    return outputs[0]['generated_text'][len(prompt):].strip()
 
 
for prompt in prompts:
    print(f"    prompt:\n{prompt}")
    print(f"    response:\n{test_inference(prompt)}")
    print("-"*50)
```

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
