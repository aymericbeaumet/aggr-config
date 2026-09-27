---
title: How to fine-tune open LLMs in 2025 with Hugging Face
link: https://www.philschmid.de/fine-tune-llms-in-2025
source: philschmid-de
published: 2024-12-20T00:00:00Z
updated: 2024-12-20T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The only guide you need to fine-tune open LLMs in 2025, including QLoRA, Spectrum, Flash Attention, Liger Kernels and more.
content: extracted
html: 2024-12-20-how-to-fine-tune-open-llms-in-2025-with-hugging-face.html
preview:
  file: 2024-12-20-how-to-fine-tune-open-llms-in-2025-with-hugging-face.preview-3a124da5da3e.webp
  width: 256
  height: 134
  alt: How to fine-tune open LLMs in 2025 with Hugging Face
  color: '#dfe1e2'
images:
- source: https://www.philschmid.de/static/blog/fine-tune-llms-in-2025/thumbnail.jpg
  original:
    file: 2024-12-20-how-to-fine-tune-open-llms-in-2025-with-hugging-face.image-8d2b19fb21d6.jpg
    width: 1400
    height: 732
  color: '#f4f5f7'
---

Large Language Models (LLMs) continued their important role in 2024, with several major developments completely outperforming previous models. The focus continued to more smaller, more powerful models from companies like Meta, Qwen, or Google. These models not only became more powerful, but also more efficient. We got Llama models as small as 1B parameters outperforming Llama 2 13B.

LLMs can now handle many tasks out-of-the-box through prompting, including chatbots, question answering, and summarization. However, for specialized applications requiring high accuracy or domain expertise, fine-tuning remains a powerful approach to achieve higher quality results than prompting alone, reduce costs by training smaller, more efficient models, and ensure reliability and consistency for specific use cases.

Contrary to last years guide [How to Fine-Tune LLMs in 2024 with Hugging Face](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl) this guide focuses more on optimization, distributed training and being more customizable. This means support for different PEFT methods from Full-Finetuning to QLoRA and Spectrum, optimizations for faster and more efficient training, with [Flash Attention](https://github.com/Dao-AILab/flash-attention) or [Liger Kernels](https://github.com/linkedin/Liger-Kernel) and how to scale training to multiple GPUs using DeepSpeed.

This guide is created using a script rather than notebook. If you are compeltely new to fine-tuning LLMs, I recommend you to start with the [How to Fine-Tune LLMs in 2024 with Hugging Face](https://www.philschmid.de/fine-tune-llms-in-2024-with-trl) guide and then come back to this guide.

You will learn how to:

1. [Define a good use case for fine-tuning](https://www.philschmid.de/fine-tune-llms-in-2025#1-define-a-good-use-case-for-fine-tuning)
2. [Setup the development environment](https://www.philschmid.de/fine-tune-llms-in-2025#2-setup-development-environment)
3. [Create and prepare the dataset](https://www.philschmid.de/fine-tune-llms-in-2025#3-create-and-prepare-the-dataset)
4. [Fine-tune the model using `trl` and the `SFTTrainer` with QLoRA as example](https://www.philschmid.de/fine-tune-llms-in-2025#4-fine-tune-the-model-using-trl-and-the-sfttrainer-with-qlora)
5. [Test and evaluate the model using GSM8K](https://www.philschmid.de/fine-tune-llms-in-2025#5-test-model-and-run-inference)

**What is Qlora?**

[QLoRA (Quantized Low-Rank Adaptation)](https://huggingface.co/papers/2305.14314) enables efficient fine-tuning of LLMs using 4-bit quantization and minimal parameter updates, reducing resource needs but potentially impacting performance due to quantization trade-offs.

**What is Spectrum?**

[Spectrum](https://huggingface.co/papers/2406.06623) is a fine-tuning method that identifies the most informative layers of a LLM using Signal-to-Noise Ratio (SNR) analysis and selectively fine-tunes them, offering performance comparable to full fine-tuning with reduced resource usage, especially in distributed training setups.

*Note: This guide is designed for consumer GPUs (24GB+) like the NVIDIA RTX 4090/5090 or A10G, but can be adapted for larger systems.*

## 1\. Define a good use case for fine-tuning

Open LLMs became more powerful and smaller in 2024. This often could mean fine-tuning might not be the first choice to solve your problem. Before you think about fine-tuning, you should always evaluate if prompting or already fine-tuned models can solve your problem. Create an evaluation setup and compare the performance of existing open models.

However, fine-tuning can be particularly valuable in several scenarios. When you need to:

- Consistently improve performance on a specific set of tasks
- Control the style and format of model outputs (e.g., enforcing a company's tone of voice)
- Teach the model domain-specific knowledge or terminology
- Reduce hallucinations for critical applications
- Optimize for latency by creating smaller, specialized models
- Ensure consistent adherence to specific guidelines or constraints

As an example, we are going to use the following use case:

> We want to fine-tune a model, which can solve high-school math problems to teach students how to solve math problems.

This can be a good use case for fine-tuning, as it requires a lot of domain-specific knowledge about math and how to solve math problems.

*Note: This is a made-up example, as existing open models already can solve this task.*

## 2\. Setup development environment

Our first step is to install Hugging Face Libraries and Pyroch, including trl, transformers and datasets. If you haven't heard of trl yet, don't worry. It is a new library on top of transformers and datasets, which makes it easier to fine-tune, rlhf, align open LLMs.

Python

```python
# Install Pytorch & other libraries
%pip install "torch==2.4.1" tensorboard flash-attn "liger-kernel==0.4.2" "setuptools<71.0.0" "deepspeed==0.15.4" openai "lm-eval[api]==0.4.5"
 
# Install Hugging Face libraries
%pip install  --upgrade \
  "transformers==4.46.3" \
  "datasets==3.1.0" \
  "accelerate==1.1.1" \
  "bitsandbytes==0.44.1" \
  "trl==0.12.1" \
  "peft==0.13.2" \
  "lighteval==0.6.2" \
  "hf-transfer==0.1.8"
```

We will use the [Hugging Face Hub](https://huggingface.co/models) as a remote model versioning service. This means we will automatically push our model, logs and information to the Hub during training. You must register on the [Hugging Face](https://huggingface.co/join) for this. After you have an account, we will use the `login` util from the `huggingface_hub` package to log into our account and store our token (access key) on the disk.

Python

```python
from huggingface_hub import login
 
login(token="", add_to_git_credential=True) # ADD YOUR TOKEN HERE
```

## 3\. Create and prepare the dataset

Once you've determined that fine-tuning is the right solution, you'll need a dataset. Most datasets are now created using automated synthetic workflows with LLMs, though several approaches exist:

- **Synthetic Generation with LLMs**: Most common approach using frameworks like [Distilabel](https://distilabel.argilla.io/) to generate high-quality synthetic data at scale
- **Existing Datasets**: Using public datasets from [Hugging Face Hub](https://huggingface.co/datasets)
- **Human Annotation**: For highest quality but most expensive option

The [LLM Datasets](https://github.com/mlabonne/llm-datasets) provides an overview of high-quality datasets to fine-tune LLMs for all kind of purposes. For our example, we'll use [Orca-Math](https://huggingface.co/datasets/microsoft/orca-math-word-problems-200k) dataset including 200,000 Math world problems.

Modern fine-tuning frameworks like `trl` support standard formats:

JSON

```json
// Conversation format
{
    "messages": [
        {"role": "system", "content": "You are..."},
        {"role": "user", "content": "..."},
        {"role": "assistant", "content": "..."},
    ]
}
// Instruction format
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
 
```

*Note: If you are interested in a guide on how to create high-quality datasets, let me know.*

To prepare our datasets we will use the Datasets library and then convert it into the the conversational format, where we include the schema definition in the system message for our assistant. We'll then save the dataset as jsonl file, which we can then use to fine-tune our model.

*Note: This step can be different for your use case. For example, if you have already a dataset from, e.g. working with OpenAI, you can skip this step and go directly to the fine-tuning step.*

Python

```python
from datasets import load_dataset
 
# Create system prompt
system_message = """Solve the given high school math problem by providing a clear explanation of each step leading to the final solution.
 
Provide a detailed breakdown of your calculations, beginning with an explanation of the problem and describing how you derive each formula, value, or conclusion. Use logical steps that build upon one another, to arrive at the final answer in a systematic manner.
 
# Steps
 
1. **Understand the Problem**: Restate the given math problem and clearly identify the main question and any important given values.
2. **Set Up**: Identify the key formulas or concepts that could help solve the problem (e.g., algebraic manipulation, geometry formulas, trigonometric identities).
3. **Solve Step-by-Step**: Iteratively progress through each step of the math problem, justifying why each consecutive operation brings you closer to the solution.
4. **Double Check**: If applicable, double check the work for accuracy and sense, and mention potential alternative approaches if any.
5. **Final Answer**: Provide the numerical or algebraic solution clearly, accompanied by appropriate units if relevant.
 
# Notes
 
- Always clearly define any variable or term used.
- Wherever applicable, include unit conversions or context to explain why each formula or step has been chosen.
- Assume the level of mathematics is suitable for high school, and avoid overly advanced math techniques unless they are common at that level.
"""
 
# convert to messages 
def create_conversation(sample):
  return {
    "messages": [
      {"role": "system", "content": system_message},
      {"role": "user", "content": sample["question"]},
      {"role": "assistant", "content": sample["answer"]}
    ]
  }  
 
# Load dataset from the hub
dataset = load_dataset("microsoft/orca-math-word-problems-200k", split="train")
 
# Convert dataset to OAI messages
dataset = dataset.map(create_conversation, remove_columns=dataset.features, batched=False)
 
print(dataset[345]["messages"])
 
# save datasets to disk 
dataset.to_json("train_dataset.json", orient="records")
```

## 4\. Fine-tune the model using `trl` and the `SFTTrainer` with QLoRA

We are now ready to fine-tune our model. We will use the [SFTTrainer](https://huggingface.co/docs/trl/sft_trainer) from `trl` to fine-tune our model. The `SFTTrainer` makes it straightfoward to supervise fine-tune open LLMs. The `SFTTrainer` is a subclass of the `Trainer` from the `transformers` library and supports all the same features, including logging, evaluation, and checkpointing, but adds additiional quality of life features, including:

- Dataset formatting, including conversational and instruction format
- Training on completions only, ignoring prompts
- Packing datasets for more efficient training
- PEFT (parameter-efficient fine-tuning) support including Q-LoRA, or Spectrum
- Preparing the model and tokenizer for conversational fine-tuning (e.g. adding special tokens)
- distributed training with `accelerate` and FSDP/DeepSpeed

We prepared a [run\_sft.py](https://github.com/philschmid/deep-learning-pytorch-huggingface/blob/main/training/scripts/run_sft.py) scripts, which supports providing a yaml configuration file to run the fine-tuning. This allows you to easily change the model, dataset, hyperparameters, and other settings. This is done by using the `TrlParser`, which parses the yaml file and converts it into the `TrainingArguments` arguments. That way we can support Q-LoRA, Spectrum, and other PEFT methods with the same script. See Appendix A for execution examples for different models and PEFT methods and distributed training.

> Question: Why don't we use frameworks like [axolotl](https://github.com/axolotl-ai-cloud/axolotl)?

That's a great question! Axolotl is a fantastic framework, it is used by many open source builders and is well tested. However, it is good to know how to do things manually. This will give you a better understanding of the inner workings and how it can be customized. Especially when you ran into an issue or want to extend the scripts and add new features.

Before we can start our training lets take a look at our [training script](https://github.com/philschmid/deep-learning-pytorch-huggingface/blob/main/training/scripts/run_sft.py). The script is kept very simple and is easy to understand. This should help you understand, customize and extend the script for your own use case. We define `dataclasses` for our arguments. Every argument can then be provided either via the command line or by providing a yaml configuration file. That way we have better type safety and intellisense support.

Python

```python
# ....
 
@dataclass
class ScriptArguments:
    dataset_id_or_path: str
    ...
# ....
```

We can customize behavior for different training methods and use them in our script with `script_args`. The training script is separated by `#######` blocks for the different parts of the script. The main training function:

1. Logs all hyperperparameters
2. Loads the dataset from Hugging Face Hub or local disk
3. Loads the tokenizer and model with our training strategy (e.g. Q-LoRA, Spectrum)
4. Initializes the `SFTTrainer`
5. Starts the training loop (optionally continue training from a checkpoint)
6. Saves the model and optionally pushes it to the Hugging Face Hub

Below is an example recipe of how we can fine-tune a [Llama-3.1-8B model with Q-LoRA](https://github.com/philschmid/deep-learning-pytorch-huggingface/blob/main/training/receipes/llama-3-1-8b-qlora.yaml).

YAML

```yaml
# Model arguments
model_name_or_path: Meta-Llama/Meta-Llama-3.1-8B
tokenizer_name_or_path: Meta-Llama/Meta-Llama-3.1-8B-Instruct
model_revision: main
torch_dtype: bfloat16
attn_implementation: flash_attention_2
use_liger: true
bf16: true
tf32: true
output_dir: runs/llama-3-1-8b-math-orca-qlora-10k-ep1
 
# Dataset arguments
dataset_id_or_path: train_dataset.json
max_seq_length: 1024
packing: true
 
# LoRA arguments
use_peft: true
load_in_4bit: true
lora_target_modules: "all-linear"
# important as we need to train the special tokens for the chat template of llama 
lora_modules_to_save: ["lm_head", "embed_tokens"] # you might need to change this for qwen or other models
lora_r: 16
lora_alpha: 16
 
# Training arguments
num_train_epochs: 1
per_device_train_batch_size: 8
gradient_accumulation_steps: 2
gradient_checkpointing: true
gradient_checkpointing_kwargs:
  use_reentrant: false
learning_rate: 2.0e-4 
lr_scheduler_type: constant
warmup_ratio: 0.1
 
# Logging arguments
logging_strategy: steps
logging_steps: 5
report_to:
- tensorboard
save_strategy: "epoch"
seed: 42
 
# Hugging Face Hub 
push_to_hub: true
# hub_model_id: llama-3-1-8b-math-orca-qlora-10k-ep1 # if not defined same as output_dir
hub_strategy: every_save
```

This config works for single-GPU training and for multi-GPU training with DeepSpeed (see Appendix for full command). If you want to use Spectrum check the [Appendix](https://www.philschmid.de/Appendix) for more information.

Python

```python
!python scripts/run_sft.py --config receipes/llama-3-1-8b-qlora.yaml
```

I ran several experiments with different optimization strategies, including Flash Attention, Liger Kernels, Q-Lora, and the Spectrum method to compare the time it takes to fine-tune a model. The results are summarized in the following table:

| Model        | Train samples | Hardware   | Method         | train sequence length | per device batch size |  gradient accumulation | packing |  Flash Attention | Liger Kernels | est. optimization steps | est. train time |
| ------------ | ------------- | ---------- | -------------- | --------------------- | --------------------- | ---------------------- | ------- | ---------------- | ------------- | ----------------------- | --------------- |
| Llama-3.1-8B | 10,000        | 1x L4 24GB | Q-LoRA         | 1024                  | 1                     | 2                      | ❌       | ❌                | ❌             | 5000                    | \~360 min       |
| Llama-3.1-8B | 10,000        | 1x L4 24GB | Q-LoRA         | 1024                  | 2                     | 2                      | ✅       | ❌                | ❌             | 1352                    | \~290 min       |
| Llama-3.1-8B | 10,000        | 1x L4 24GB | Q-LoRA         | 1024                  | 2                     | 4                      | ✅       | ✅                | ❌             | 676                     | \~220 min       |
| Llama-3.1-8B | 10,000        | 1x L4 24GB | Q-LoRA         | 1024                  | 4                     | 4                      | ✅       | ✅                | ✅             | 338                     | \~135 min       |
| Llama-3.1-8B | 10,000        | 4x L4 24GB | Q-LoRA         | 1024                  | 8                     | 2                      | ✅       | ✅                | ✅             | 84                      | \~33 min        |
| Llama-3.1-8B | 10,000        | 8x L4 24GB | Q-LoRA         | 1024                  | 8                     | 2                      | ✅       | ✅                | ✅             | 42                      | \~18 min        |
| Llama-3.1-8B | 10,000        | 8x L4 24GB | Spectrum (30%) | 1024                  | 8                     | 2                      | ✅       | ✅                | ✅             | 42                      | \~21 min        |

**Notes:**

- Q-Lora included training the embedding layer and the lm\_head, as we use the Llama 3.1 chat template and in the base model the special tokens are not trained.
- For distributed training Deepspeed (0.15.4) with ZeRO3 and Hugging Face Accelerate was used.
- Spectrum with 30% SNR layers took slightly longer than Q-Lora, but achieves 58% accuracy on GSM8K dataset, which is 4% higher than Q-Lora.

Using Q-LoRA only saves the trained adapter weights. If you want to use the model as standalone model, e.g. for inference you might want to merge the adapter and base model. This can be done using the following command:

Python

```python
!python scripts/merge_adapter_weights.py --peft_model_id runs/llama-3-1-8b-math-orca-qlora-10k-ep1 --push_to_hub True --repository_id llama-3-1-8b-math-orca-qlora-10k-ep1-merged
```

## 5\. Test Model and run Inference

After the training is done we want to evaluate and test our model. As we trained our model on solving math problems, we will evaluate the model on [GSM8K](https://huggingface.co/datasets/openai/gsm8k) dataset. GSM8K (Grade School Math 8K) is a dataset of 8.5K high quality linguistically diverse grade school math word problems. The dataset was created to support the task of question answering on basic mathematical problems that require multi-step reasoning.

Evaluating Generative AI models is not a trivial task since 1 input can have multiple correct outputs. If you want to learn more about evaluating generative models, check out:

- [Evaluate LLMs and RAG a practical example using Langchain and Hugging Face](https://www.philschmid.de/evaluate-llm).
- [Evaluate LLMs using Evaluation Harness and Hugging Face TGI/vLLM](https://www.philschmid.de/evaluate-llms-with-lm-eval-and-tgi-vllm)
- [LLM Evaluation doesn't need to be complicated](https://www.philschmid.de/llm-evaluation)
- [Evaluating Open LLMs with MixEval: The Closest Benchmark to LMSYS Chatbot Arena](https://www.philschmid.de/evaluate-llm-mixeval)

We are going to use [Evaluation Harness](https://github.com/EleutherAI/lm-evaluation-harness) an open-source framework to evaluate language models on a wide range of tasks and benchmarks. The frameworks support evaluating models behind OpenAI compatible API endpoints, those can be locally or remotely. This super helpful as we can evaluate our model in the same environment we will use for production.

We are going to use [Text Generation Inference (TGI)](https://github.com/huggingface/text-generation-inference) for testing and deploying our model. TGI is a purpose-built solution for deploying and serving Large Language Models (LLMs). TGI enables high-performance text generation using Tensor Parallelism and continous batching. If you are or want to use vLLM you can check the Appendix on how to start the inference server.

*Note: Make sure that you have enough GPU memory to run the container. Restart kernel to remove all allocated GPU memory from the notebook.*

We will start the on 1 GPU detached. Meaning we can can continue to use the notebook while the container is running. If you have more GPUs you can change the `--gpus` and `--num-shard` flags to the number of GPUs.

Bash

```bash
%%bash
 
num_gpus=1
model_id=philschmid/llama-3-1-8b-math-orca-spectrum-10k-ep1 # replace with your model id
 
docker run --name tgi --gpus ${num_gpus} -d -ti -p 8080:80 --shm-size=2GB \
  -e HF_TOKEN=$(cat ~/.cache/huggingface/token) \
  ghcr.io/huggingface/text-generation-inference:3.0.1 \
  --model-id ${model_id} \
  --num-shard ${num_gpus}
 
```

Our container will now start in the background and download the model from Hugging Face Hub. We can check the logs to see the progress with `docker logs -f tgi`.

Once our container is running we can send requests using the `openai` or `huggingface_hub` sdk. Here we ll use the `openai` sdk to send a request to our inference server. If you don't have the `openai` sdk installed you can install it using `pip install openai`.

Python

```python
from openai import OpenAI
 
# create client 
client = OpenAI(base_url="http://localhost:8080/v1",api_key="-")
 
system_message = """Solve the given high school math problem by providing a clear explanation of each step leading to the final solution.
 
Provide a detailed breakdown of your calculations, beginning with an explanation of the problem and describing how you derive each formula, value, or conclusion. Use logical steps that build upon one another, to arrive at the final answer in a systematic manner.
 
# Steps
 
1. **Understand the Problem**: Restate the given math problem and clearly identify the main question and any important given values.
2. **Set Up**: Identify the key formulas or concepts that could help solve the problem (e.g., algebraic manipulation, geometry formulas, trigonometric identities).
3. **Solve Step-by-Step**: Iteratively progress through each step of the math problem, justifying why each consecutive operation brings you closer to the solution.
4. **Double Check**: If applicable, double check the work for accuracy and sense, and mention potential alternative approaches if any.
5. **Final Answer**: Provide the numerical or algebraic solution clearly, accompanied by appropriate units if relevant.
 
# Notes
 
- Always clearly define any variable or term used.
- Wherever applicable, include unit conversions or context to explain why each formula or step has been chosen.
- Assume the level of mathematics is suitable for high school, and avoid overly advanced math techniques unless they are common at that level.
"""
 
messages = [
    {"role": "system", "content": system_message},
    {"role": "user", "content": "Natalia sold clips to 48 of her friends in April, and then she sold half as many clips in May. How many clips did Natalia sell altogether in April and May?"},
]
expected_answer = "72"
 
# Take a random sample from the dataset and remove the last message and send it to the model
response = client.chat.completions.create(
	model="orca",
	messages=messages,
	stream=False, # no streaming
	max_tokens=256,
)
response = response.choices[0].message.content
 
# Print results
print(f"Query:\n{messages[1]['content']}")
print(f"Original Answer:\n{expected_answer}")
print(f"Generated Answer:\n{response}")
```

Awesome that looks great! Now we can evaluate our model with the [Evaluation Harness](https://github.com/EleutherAI/lm-evaluation-harness).

*Note: Make sure to change the model id to your fine-tuned model.*

Python

```python
!lm_eval --model local-chat-completions \
  --tasks gsm8k_cot \
  --model_args model=philschmid/llama-3-1-8b-math-orca-spectrum-10k-ep1,base_url=http://localhost:8080/v1/chat/completions,num_concurrent=8,max_retries=3,tokenized_requests=False \
  --apply_chat_template \
  --fewshot_as_multiturn
```

Wow, 54% accuracy with only using 10k samples is pretty good! We successfully validated that our model can solve math problems. Now, don't forget to stop your container once you are done.

Python

```python
!docker stop tgi
!docker rm tgi
```

## Conclusion

This guide provides the foundation for fine-tuning LLMs in 2025. The modular training scripts and configurations make it easy to adapt to your specific use case, whether you're training on a single GPU or scaling across multiple nodes.

If you encounter issues, have questions, or want to contribute improvements to the training pipeline, please open a PR on the repository.

## Appendix

The Appendix contains additional commands and documentation on how to run distributed training, inference and how to use Spectrum.

## Distributed Training

### Deepspeed + Q-LoRA

Note: change the `num_processes` to the number of GPUs you want to use.

Bash

```bash
accelerate launch --config_file configs/accelerate_configs/deepspeed_zero3.yaml --num_processes 8 scripts/run_sft.py --config receipes/llama-3-1-8b-qlora.yaml
```

## Inference

### vLLM

Note: Replace the model id with your fine-tuned model.

Bash

```bash
docker run --runtime nvidia --gpus all \
    -p 8000:8000 \
    vllm/vllm-openai --model philschmid/llama-3-1-8b-math-orca-qlora-10k-ep1-merged 
```

## Spectrum

Spectrum uses Signal-to-Noise Ratio (SNR) analysis to select the most useful layers for fine-tuning. It provides scripts and pre-run scanned for different models. If your model isn't scanned it will prompt you for the batch size to use for scanning. Batch size of 4 for 70b models requires 8xH100. But popular models like Llama 3.1 8B are already scanned. You can find the scanned models [here](https://github.com/cognitivecomputations/spectrum/tree/main/model_snr_results).

The script will generate a yaml configuration file in the `model_snr_results` with the name of the model and the top-percent, e.g. for `meta-llama/Llama-3.1-8B` and `30` it will generate it at `snr_results_meta-llama-Meta-Llama-3.1-8B_unfrozenparameters_30percent.yaml`.

- `--model-name`: Specify the local model path or the Hugging Face repository.
- `--top-percent`: Specify the top percentage of SNR layers you want to retrieve.

Bash

```bash
# clone spectrum
git clone https://github.com/cognitivecomputations/spectrum.git
cd spectrum
# generate yaml configuration
python3 spectrum.py --model-name meta-llama/Meta-Llama-3.1-8B --top-percent 30
# Top 30% SNR layers saved to snr_results_meta-llama-Meta-Llama-3.1-8B_unfrozenparameters_30percent.yaml
cd ..
```

After the yaml configuration is generated we can use it to fine-tune our model. We need to define the yaml configuration file in our train config yaml file and provide the path to the yaml file as `spectrum_config_path`. Take a look at [receipes/llama-3-1-8b-spectrum.yaml](https://github.com/philschmid/deep-learning-pytorch-huggingface/blob/main/training/receipes/llama-3-1-8b-spectrum.yaml) for an example.

Then we can start the training with the following command for single GPU training:

Bash

```bash
CUDA_VISIBLE_DEVICES=0 python scripts/run_sft.py --config receipes/llama-3-1-8b-spectrum.yaml
```

*Note: Spectrum requires a more memory than Q-Lora. According to the paper ~30-50GB on a single GPU.*

For multi-GPU training with FSDP and Deepspeed you can use the following command:

Bash

```bash
accelerate launch --config_file configs/accelerate_configs/deepspeed_zero3.yaml --num_processes 8 scripts/run_sft.py --config receipes/llama-3-1-8b-spectrum.yaml
```

*Note: Training on 8x L4 GPUs with Spectrum takes ~21 minutes. Q-Lora on the same config took 18 minutes.*

Results:

- Spectrum model trained for 1 epoch with 30% SNR layers on GSM8K dataset achieved 58% accuracy, which is 4% higher than Q-Lora.
- Spectrum model trained for 3 epochs with 30% SNR layers on GSM8K dataset achieved 60% accuracy.
