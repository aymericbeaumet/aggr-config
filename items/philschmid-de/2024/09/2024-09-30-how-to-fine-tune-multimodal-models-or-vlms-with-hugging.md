---
title: How to Fine-Tune Multimodal Models or VLMs with Hugging Face TRL
link: https://www.philschmid.de/fine-tune-multimodal-llms-with-trl
source: philschmid-de
published: 2024-09-30T00:00:00Z
updated: 2024-09-30T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to fine-tune multimodal models like Llama 3.2 Vision or Qwen 2 VL to create custom image-to-text generation models.
content: extracted
html: 2024-09-30-how-to-fine-tune-multimodal-models-or-vlms-with-hugging.html
preview:
  file: 2024-09-30-how-to-fine-tune-multimodal-models-or-vlms-with-hugging.preview-cb9bbca8f698.webp
  width: 256
  height: 165
  alt: How to Fine-Tune Multimodal Models or VLMs with Hugging Face TRL
  color: '#838374'
images:
- source: https://www.philschmid.de/static/blog/fine-tune-multimodal-llms-with-trl/thumbnail.jpg
  original:
    file: 2024-09-30-how-to-fine-tune-multimodal-models-or-vlms-with-hugging.image-2f50251fb543.jpg
    width: 1024
    height: 660
  color: '#020202'
---

Multimodal LLMs are making tremendous progress recently. We now have a diverse ecosystem of powerful open Multimodal models, mostly Vision-Language Models (VLM), including Meta AI's [Llama-3.2-11B-Vision](https://huggingface.co/meta-llama/Llama-3.2-11B-Vision-Instruct), Mistral AI's [Pixtral-12B](https://huggingface.co/mistralai/Pixtral-12B-2409), Qwen's [Qwen2-VL-7B](https://huggingface.co/Qwen/Qwen2-VL-7B), and Allen AI's [Molmo-7B-D-0924](https://huggingface.co/allenai/Molmo-7B-D-0924).

These VLMs can handle a variety of multimodal tasks, including image captioning, visual question answering, and image-text matching without additional training. However, to customize a model for your specific application, you may need to fine-tune it on your data to achieve higher quality results or to create a more efficient model for your use case.

This blog post walks you through how to fine-tune open VLMs using Hugging Face [TRL](https://huggingface.co/docs/trl/index), [Transformers](https://huggingface.co/docs/transformers/index) & [datasets](https://huggingface.co/docs/datasets/index) in 2024. We'll cover:

1. [Define our multimodal use case](https://www.philschmid.de/fine-tune-multimodal-llms-with-trl#1-define-our-multimodal-use-case)
2. [Setup development environment](https://www.philschmid.de/fine-tune-multimodal-llms-with-trl#2-setup-development-environment)
3. [Create and prepare the multimodal dataset](https://www.philschmid.de/fine-tune-multimodal-llms-with-trl#3-create-and-prepare-the-multimodal-dataset)
4. [Fine-tune VLM using `trl` and the `SFTTrainer`](https://www.philschmid.de/fine-tune-multimodal-llms-with-trl#4-fine-tune-vlm-using-trl-and-the-sfstrainer)
5. [Test and evaluate the VLM](https://www.philschmid.de/fine-tune-multimodal-llms-with-trl#5-test-and-evaluate-the-vlm)

*Note: This blog was created to run on consumer size GPUs (24GB), e.g. NVIDIA A10G or RTX 4090/3090, but can be easily adapted to run on bigger GPUs.*

## 1\. Define our multimodal use case

When fine-tuning VLMs, it's crucial to clearly define your use case and the multimodal task you want to solve. This will guide your choice of base model and help you create an appropriate dataset for fine-tuning. If you haven't defined your use case yet, you might want to revisit your requirements.

It's worth noting that for most use cases fine-tuning might not be the first option. We recommend evaluating pre-trained models or API-based solutions before committing to fine-tuning your own model.

As an example, we'll use the following multimodal use case:

> We want to fine-tune a model that can generate detailed product descriptions based on product images and basic metadata. This model will be integrated into our e-commerce platform to help sellers create more compelling listings. The goal is to reduce the time it takes to create product descriptions and improve their quality and consistency.

Existing models might already be very good for this use case, but you might want to tweak/tune it to your specific needs. This image-to-text generation task is well-suited for fine-tuning VLMs, as it requires understanding visual features and combining them with textual information to produce coherent and relevant descriptions. I created a test dataset for this use case using Gemini 1.5 [philschmid/amazon-product-descriptions-vlm](https://huggingface.co/datasets/philschmid/amazon-product-descriptions-vlm).

## 2\. Setup development environment

Our first step is to install Hugging Face Libraries and Pyroch, including trl, transformers and datasets. If you haven't heard of trl yet, don't worry. It is a library on top of transformers and datasets, which makes it easier to fine-tune, rlhf, align open LLMs.

Python

```python
# Install Pytorch & other libraries
%pip install "torch==2.4.0" tensorboard pillow
 
# Install Hugging Face libraries
%pip install  --upgrade \
  "transformers==4.45.1" \
  "datasets==3.0.1" \
  "accelerate==0.34.2" \
  "evaluate==0.4.3" \
  "bitsandbytes==0.44.0" \
  "trl==0.11.1" \
  "peft==0.13.0" \
  "qwen-vl-utils"
```

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

Once you have determined that fine-tuning is the right solution we need to create a dataset to fine-tune our model. We have to prepare the dataset in a format that the model can understand.

In our example we will use [philschmid/amazon-product-descriptions-vlm](https://huggingface.co/datasets/philschmid/amazon-product-descriptions-vlm), which contains 1,350 amazon products with title, images and descriptions and metadata. We want to fine-tune our model to generate product descriptions based on the images, title and metadata. Therefore we need to create a prompt including the title, metadata and image and the completion is the description.

TRL supports popular instruction and conversation dataset formats. This means we only need to convert our dataset to one of the supported formats and `trl` will take care of the rest.

JSON

```json
"messages": [
  {"role": "system", "content": [{"type":"text", "text": "You are a helpful...."}]},
  {"role": "user", "content": [{
    "type": "text", "text":  "How many dogs are in the image?", 
    "type": "image", "text": <PIL.Image> 
    }]},
  {"role": "assistant", "content": [{"type":"text", "text": "There are 3 dogs in the image."}]}
],
```

In our example we are going to load our dataset using the Datasets library and apply our frompt and convert it into the the conversational format.

Lets start with defining our instruction prompt.

Python

```python
# note the image is not provided in the prompt its included as part of the "processor"
prompt= """Create a Short Product description based on the provided ##PRODUCT NAME## and ##CATEGORY## and image. 
Only return description. The description should be SEO optimized and for a better mobile search experience.
 
##PRODUCT NAME##: {product_name}
##CATEGORY##: {category}"""
 
system_message = "You are an expert product description writer for Amazon."
```

Now, we can format our dataset.

Python

```python
from datasets import load_dataset
 
# Convert dataset to OAI messages       
def format_data(sample):
    return {"messages": [
                {
                    "role": "system",
                    "content": [{"type": "text", "text": system_message}],
                },
                {
                    "role": "user",
                    "content": [
                        {
                            "type": "text",
                            "text": prompt.format(product_name=sample["Product Name"], category=sample["Category"]),
                        },{
                            "type": "image",
                            "image": sample["image"],
                        }
                    ],
                },
                {
                    "role": "assistant",
                    "content": [{"type": "text", "text": sample["description"]}],
                },
            ],
        }
 
# Load dataset from the hub
dataset_id = "philschmid/amazon-product-descriptions-vlm"
dataset = load_dataset("philschmid/amazon-product-descriptions-vlm", split="train")
 
# Convert dataset to OAI messages
# need to use list comprehension to keep Pil.Image type, .mape convert image to bytes
dataset = [format_data(sample) for sample in dataset]
 
print(dataset[345]["messages"])
```

## 4\. Fine-tune VLM using `trl` and the `SFTTrainer`

We are now ready to fine-tune our model. We will use the [SFTTrainer](https://huggingface.co/docs/trl/sft_trainer) from `trl` to fine-tune our model. The `SFTTrainer` makes it straightfoward to supervise fine-tune open LLMs and VLMs. The `SFTTrainer` is a subclass of the `Trainer` from the `transformers` library and supports all the same features, including logging, evaluation, and checkpointing, but adds additiional quality of life features.

We will use the PEFT features in our example. As peft method we will use [QLoRA](https://arxiv.org/abs/2305.14314) a technique to reduce the memory footprint of large language models during finetuning, without sacrificing performance by using quantization. If you want to learn more about QLoRA and how it works, check out [Making LLMs even more accessible with bitsandbytes, 4-bit quantization and QLoRA](https://huggingface.co/blog/4bit-transformers-bitsandbytes) blog post.

*Note: We cannot use Flash Attention as we need to pad our multimodal inputs.*

We are going to use Qwen 2 VL 7B model, but we can easily swap out the model for another model, including Meta AI's [Llama-3.2-11B-Vision](https://huggingface.co/meta-llama/Llama-3.2-11B-Vision-Instruct), Mistral AI's [Pixtral-12B](https://huggingface.co/mistralai/Pixtral-12B-2409) or any other LLMs by changing our `model_id` variable. We will use bitsandbytes to quantize our model to 4-bit.

*Note: Be aware the bigger the model the more memory it will require. In our example we will use the 7B version, which can be tuned on 24GB GPUs.*

Correctly, preparing the LLM, Tokenizer and Processor for training VLMs is crucial. The Processor is responsible for including the special tokens and image features in the input.

Python

```python
import torch
from transformers import AutoModelForVision2Seq, AutoProcessor, BitsAndBytesConfig
 
# Hugging Face model id
model_id = "Qwen/Qwen2-VL-7B-Instruct" 
 
# BitsAndBytesConfig int-4 config
bnb_config = BitsAndBytesConfig(
    load_in_4bit=True, bnb_4bit_use_double_quant=True, bnb_4bit_quant_type="nf4", bnb_4bit_compute_dtype=torch.bfloat16
)
 
# Load model and tokenizer
model = AutoModelForVision2Seq.from_pretrained(
    model_id,
    device_map="auto",
    # attn_implementation="flash_attention_2", # not supported for training
    torch_dtype=torch.bfloat16,
    quantization_config=bnb_config
)
processor = AutoProcessor.from_pretrained(model_id)
```

Python

```python
# Preparation for inference
text = processor.apply_chat_template(
    dataset[2]["messages"], tokenize=False, add_generation_prompt=False
)
print(text)
```

The `SFTTrainer` supports a native integration with `peft`, which makes it super easy to efficiently tune LLMs using, e.g. QLoRA. We only need to create our `LoraConfig` and provide it to the trainer. Our `LoraConfig` parameters are defined based on the [qlora paper](https://arxiv.org/pdf/2305.14314.pdf) and sebastian's [blog post](https://magazine.sebastianraschka.com/p/practical-tips-for-finetuning-llms).

Python

```python
from peft import LoraConfig
 
# LoRA config based on QLoRA paper & Sebastian Raschka experiment
peft_config = LoraConfig(
        lora_alpha=16,
        lora_dropout=0.05,
        r=8,
        bias="none",
        target_modules=["q_proj", "v_proj"],
        task_type="CAUSAL_LM", 
)
```

Before we can start our training we need to define the hyperparameters (`SFTConfig`) we want to use and make sure our inputs are correcty provided to the model. Different to text-only supervised fine-tuning we need to provide the image to the model as well. Therefore we create a custom `DataCollator` which formates the inputs correctly and include the image features. We use the [process\_vision\_info](https://github.com/QwenLM/Qwen2-VL/blob/main/qwen-vl-utils/src/qwen_vl_utils/vision_process.py#L321) method from a utility package the Qwen2 team provides. If you are using another model, e.g. Llama 3.2 Vision you might have to check if that creates the same processsed image information.

Python

```python
from trl import SFTConfig
from transformers import Qwen2VLProcessor
from qwen_vl_utils import process_vision_info
 
args = SFTConfig(
    output_dir="qwen2-7b-instruct-amazon-description", # directory to save and repository id
    num_train_epochs=3,                     # number of training epochs
    per_device_train_batch_size=4,          # batch size per device during training
    gradient_accumulation_steps=8,          # number of steps before performing a backward/update pass
    gradient_checkpointing=True,            # use gradient checkpointing to save memory
    optim="adamw_torch_fused",              # use fused adamw optimizer
    logging_steps=5,                       # log every 10 steps
    save_strategy="epoch",                  # save checkpoint every epoch
    learning_rate=2e-4,                     # learning rate, based on QLoRA paper
    bf16=True,                              # use bfloat16 precision
    tf32=True,                              # use tf32 precision
    max_grad_norm=0.3,                      # max gradient norm based on QLoRA paper
    warmup_ratio=0.03,                      # warmup ratio based on QLoRA paper
    lr_scheduler_type="constant",           # use constant learning rate scheduler
    push_to_hub=True,                       # push model to hub
    report_to="tensorboard",                # report metrics to tensorboard
    gradient_checkpointing_kwargs = {"use_reentrant": False}, # use reentrant checkpointing
    dataset_text_field="", # need a dummy field for collator
    dataset_kwargs = {"skip_prepare_dataset": True} # important for collator
)
args.remove_unused_columns=False
 
# Create a data collator to encode text and image pairs
def collate_fn(examples):
    # Get the texts and images, and apply the chat template
    texts = [processor.apply_chat_template(example["messages"], tokenize=False) for example in examples]
    image_inputs = [process_vision_info(example["messages"])[0] for example in examples]
 
    # Tokenize the texts and process the images
    batch = processor(text=texts, images=image_inputs, return_tensors="pt", padding=True)
 
    # The labels are the input_ids, and we mask the padding tokens in the loss computation
    labels = batch["input_ids"].clone()
    labels[labels == processor.tokenizer.pad_token_id] = -100  #
    # Ignore the image token index in the loss computation (model specific)
    if isinstance(processor, Qwen2VLProcessor):
        image_tokens = [151652,151653,151655]
    else: 
        image_tokens = [processor.tokenizer.convert_tokens_to_ids(processor.image_token)]
    for image_token_id in image_tokens:
        labels[labels == image_token_id] = -100
    batch["labels"] = labels
 
    return batch
```

We now have every building block we need to create our `SFTTrainer` to start then training our model.

Python

```python
from trl import SFTTrainer
 
trainer = SFTTrainer(
    model=model,
    args=args,
    train_dataset=dataset,
    data_collator=collate_fn,
    dataset_text_field="", # needs dummy value
    peft_config=peft_config,
    tokenizer=processor.tokenizer,
)
```

Start training our model by calling the `train()` method on our `Trainer` instance. This will start the training loop and train our model for 3 epochs. Since we are using a PEFT method, we will only save the adapted model weights and not the full model.

Python

```python
# start training, the model will be automatically saved to the hub and the output directory
trainer.train()
 
# save model 
trainer.save_model(args.output_dir)
```

Training for 3 epochs with a dataset of ~1k samples took 01:31:58 on a `g6.2xlarge`. The instance costs `0.9776$/h` which brings us to a total cost of only `1.4$`.

Python

```python
# free the memory again
del model
del trainer
torch.cuda.empty_cache()
```

## 5\. Test Model and run Inference

After the training is done we want to evaluate and test our model. First we will load the base model and let it generate a description for a random Amazon product. Then we will load our Q-LoRA adapted model and let it generate a description for the same product.

Finally we can merge the adapter into the base model to make it more efficient and run inference on the same product again.

Python

```python
import torch
from transformers import AutoProcessor, AutoModelForVision2Seq
 
adapter_path = "./qwen2-7b-instruct-amazon-description"
 
# Load Model base model
model = AutoModelForVision2Seq.from_pretrained(
  model_id,
  device_map="auto",
  torch_dtype=torch.float16
)
processor = AutoProcessor.from_pretrained(model_id)
```

I selected a random product from Amazon and prepared a `generate_description` function to generate a description for the product.

Python

```python
from qwen_vl_utils import process_vision_info
 
# sample from amazon.com
sample = {
  "product_name": "Hasbro Marvel Avengers-Serie Marvel Assemble Titan-Held, Iron Man, 30,5 cm Actionfigur",
  "catergory": "Toys & Games | Toy Figures & Playsets | Action Figures",
  "image": "https://m.media-amazon.com/images/I/81+7Up7IWyL._AC_SY300_SX300_.jpg"
}
 
# prepare message
messages = [{
        "role": "user",
        "content": [
            {
                "type": "image",
                "image": sample["image"],
            },
            {"type": "text", "text": prompt.format(product_name=sample["product_name"], category=sample["catergory"])},
        ],
    }
]
 
def generate_description(sample, model, processor):
    messages = [
        {"role": "system", "content": [{"type": "text", "text": system_message}]},
        {"role": "user", "content": [
            {"type": "image","image": sample["image"]},
            {"type": "text", "text": prompt.format(product_name=sample["product_name"], category=sample["catergory"])},
        ]},
    ]
    # Preparation for inference
    text = processor.apply_chat_template(
        messages, tokenize=False, add_generation_prompt=True
    )
    image_inputs, video_inputs = process_vision_info(messages)
    inputs = processor(
        text=[text],
        images=image_inputs,
        videos=video_inputs,
        padding=True,
        return_tensors="pt",
    )
    inputs = inputs.to(model.device)
    # Inference: Generation of the output
    generated_ids = model.generate(**inputs, max_new_tokens=256, top_p=1.0, do_sample=True, temperature=0.8)
    generated_ids_trimmed = [out_ids[len(in_ids) :] for in_ids, out_ids in zip(inputs.input_ids, generated_ids)]
    output_text = processor.batch_decode(
        generated_ids_trimmed, skip_special_tokens=True, clean_up_tokenization_spaces=False
    )
    return output_text[0]
 
# let's generate the description
base_description = generate_description(sample, model, processor)
print(base_description)
# you can disable the active adapter if you want to rerun it with
# model.disable_adapters()
```

Awesome it is working! Lets load our adapter and compare if with the base model.

Python

```python
model.load_adapter(adapter_path) # load the adapter and activate
 
ft_description = generate_description(sample, model, processor)
print(ft_description)
```

Lets compare them side by side using a markdown table.

Python

```python
import pandas as pd
from IPython.display import display, HTML
 
def compare_generations(base_gen, ft_gen):
    # Create a DataFrame
    df = pd.DataFrame({
        'Base Generation': [base_gen],
        'Fine-tuned Generation': [ft_gen]
    })
    # Style the DataFrame
    styled_df = df.style.set_properties(**{
        'text-align': 'left',
        'white-space': 'pre-wrap',
        'border': '1px solid black',
        'padding': '10px',
        'width': '250px',  # Set width to 150px
        'overflow-wrap': 'break-word'  # Allow words to break and wrap as needed
    })
    
    # Display the styled DataFrame
    display(HTML(styled_df.to_html()))
  
compare_generations(base_description, ft_description)
```

> | Base Generation                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Fine-tuned Generation                                                                                                                                                                                                                                                                              |
> | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
> | Introducing the Hasbro Marvel Avengers Series Marvel Assemble Titan-Held Iron Man Action Figure, a 30.5 cm tall action figure that is sure to bring the excitement of the Marvel Universe to life! This highly detailed Iron Man figure is perfect for fans of all ages and makes a great addition to any toy collection. With its sleek red and gold armor, this Iron Man figure is ready to take on any challenge. The Titan-Held feature allows for a more realistic and dynamic pose, making it a must-have for any Marvel fan. Whether you're a collector or just looking for a fun toy to play with, this Iron Man action figure is the perfect choice. | Unleash the power of Iron Man with this 30.5 cm Hasbro Marvel Avengers Titan Hero Action Figure! This highly detailed Iron Man figure is perfect for collectors and kids alike. Features a realistic design and articulated joints for dynamic poses. A must-have for any Marvel fan's collection! |

Nice! Even though we just had ~1k samples we can see that the fine-tuning improve the product description generation. The description is way shorter and more concise, which fits our training data.

### Optional: Merge LoRA adapter in to the original model

When using QLoRA, we only train adapters and not the full model. This means when saving the model during training we only save the adapter weights and not the full model. If you want to save the full model, which makes it easier to use with Text Generation Inference you can merge the adapter weights into the model weights using the `merge_and_unload` method and then save the model with the `save_pretrained` method. This will save a default model, which can be used for inference.

*Note: This requires > 30GB CPU Memory.*

Python

```python
from peft import PeftModel
from transformers import AutoProcessor, AutoModelForVision2Seq
 
adapter_path = "./qwen2-7b-instruct-amazon-description"
base_model_id = "Qwen/Qwen2-VL-7B-Instruct"
merged_path = "merged"
 
# Load Model base model
model = AutoModelForVision2Seq.from_pretrained(model_id, low_cpu_mem_usage=True)
 
# Path to save the merged model
 
# Merge LoRA and base model and save
peft_model = PeftModel.from_pretrained(model, adapter_path)
merged_model = peft_model.merge_and_unload()
merged_model.save_pretrained(merged_path,safe_serialization=True, max_shard_size="2GB")
 
processor = AutoProcessor.from_pretrained(base_model_id)
processor.save_pretrained(merged_path)
```

## Bonus: Use TRL example script

TRL provides a simple example script to fine-tune multimodal models. You can find the script [here](https://github.com/huggingface/trl/blob/main/examples/scripts/sft_vlm.py). The script can be directly run from the command line and supports all the features of the `SFTTrainer`.

Bash

```bash
# Tested on 8x H100 GPUs
accelerate launch
    --config_file=examples/accelerate_configs/deepspeed_zero3.yaml \
    examples/scripts/sft_vlm.py \
    --dataset_name HuggingFaceH4/llava-instruct-mix-vsft \
    --model_name_or_path llava-hf/llava-1.5-7b-hf \
    --per_device_train_batch_size 8 \
    --gradient_accumulation_steps 8 \
    --output_dir sft-llava-1.5-7b-hf \
    --bf16 \
    --torch_dtype bfloat16 \
    --gradient_checkpointing
```
