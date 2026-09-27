---
title: Deploy open LLMs with Terraform and Amazon SageMaker
link: https://www.philschmid.de/terraform-llm-sagemaker
source: philschmid-de
published: 2024-08-05T00:00:00Z
updated: 2024-08-05T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: Learn how to deploy open large language models (LLMs) like Llama 3.1 8B Instruct to Amazon SageMaker using Terraform Infrastructure as Code (IaC).
content: extracted
html: 2024-08-05-deploy-open-llms-with-terraform-and-amazon-sagemaker.html
preview:
  file: 2024-08-05-deploy-open-llms-with-terraform-and-amazon-sagemaker.preview-f8563845413e.webp
  width: 256
  height: 128
  alt: Deploy open LLMs with Terraform and Amazon SageMaker
  color: '#5b655a'
images:
- source: https://www.philschmid.de/static/blog/terraform-llm-sagemaker/thumbnail.jpg
  original:
    file: 2024-08-05-deploy-open-llms-with-terraform-and-amazon-sagemaker.image-996d8ea1749e.jpg
    width: 1200
    height: 600
  color: '#030303'
---

Deploying open LLMs into production environments can often be a complex process that requires coordination between data scientists, machine learning engineers, and DevOps teams. Traditionally, data scientists or ML engineers focus on model development, while the deployment and are not always responsible for—or experienced in —deploying LLMs to production. This is where Infrastructure as Code (IaC) tools like Terraform come into play.

## The Importance of Infrastructure as Code

*“Infrastructure as Code (IaC) is the managing and provisioning of infrastructure through code instead of through manual processes. With IaC, configuration files are created that contain your infrastructure specifications, which makes it easier to edit and distribute configurations. It also ensures that you provision the same environment every time.”* - [Red Hat](https://www.redhat.com/en/topics/automation/what-is-infrastructure-as-code-iac)

IaC ensures:

1. Consistency: By defining infrastructure in code, we ensure that every deployment is identical, eliminating the "it works on my machine" problem.
2. Version Control: Infrastructure configurations can be versioned, allowing for easy rollbacks and collaborative development.
3. Scalability: IaC makes it simple to replicate environments for testing or scaling purposes.
4. Automation: Deployments can be automated, reducing human error and speeding up the process.

## Terraform LLM SageMaker Module

The [Terraform LLM SageMaker Module](https://github.com/philschmid/terraform-aws-llm-sagemaker) simplifies the process of deploying open LLMs from Hugging Face to Amazon SageMaker real-time endpoints.

It handles the creation of all necessary resources, including:

- IAM roles (if not provided)
- SageMaker Model
- SageMaker Endpoint Configuration
- SageMaker Endpoint
- Autoscaling

With this module, you can easily deploy popular models like Llama 3, Mistral, Mixtral, and Command from Hugging Face to Amazon SageMaker.

Hcl

```hcl
 
module "sagemaker-huggingface" {
  source               = "philschmid/llm-sagemaker/aws"
  version              = "0.1.0"
  endpoint_name_prefix = "llama3"
  hf_model_id          = "meta-llama/Meta-Llama-3.1-8B-Instruct"
  hf_token             = "YOUR_HF_TOKEN_WITH_ACCESS_TO_THE_MODEL"
  instance_type        = "ml.g5.2xlarge"
  instance_count       = 1 # default is 1
 
  tgi_config = {
    max_input_tokens       = 4000
    max_total_tokens       = 4096
    max_batch_total_tokens = 6144
  }
}
 
```

## Deploy Llama 3 with Terraform

Before we get started, make sure you have the [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) installed and configured, as well as access to AWS Credentials to create the necessary services.

### Create a new Terraform configuration

Each Terraform configuration must be in its own directory including a `main.tf` file. Our first step is to create the `llama-terraform` directory with a `main.tf` file.

Bash

```bash
mkdir llama-terraform
touch llama-terraform/main.tf
cd llama-terraform
```

This configuration will deploy the Llama 3 model to a SageMaker endpoint, handling all the necessary setup behind the scenes.

### Initialize the AWS provider and our module

Next, we open the `main.tf` in a text editor and add the `aws` provider as well as our `module`.

*Note: the snippet below assumes that you have an AWS profile `default` configured with the needed permissions*

Hcl

```hcl
provider "aws" {
  profile = "default"
  region  = "us-east-1"
}
 
 
module "sagemaker-huggingface" {
  source               = "philschmid/llm-sagemaker/aws"
  version              = "0.1.0"
  endpoint_name_prefix = "llama3"
  hf_model_id          = "meta-llama/Meta-Llama-3.1-8B-Instruct"
  hf_token             = "YOUR_HF_TOKEN_WITH_ACCESS_TO_THE_MODEL"
  instance_type        = "ml.g5.2xlarge"
  instance_count       = 1# default is 1
 
  tgi_config = {
    max_input_tokens       = 4000
    max_total_tokens       = 4096
    max_batch_total_tokens = 6144
  }
}
 
output "endpoint_name" {
  value = module.sagemaker-huggingface.sagemaker_endpoint_name
}
```

*Note: Make sure to replace the* `YOUR_HF_TOKEN_WITH_ACCESS_TO_THE_MODEL` with a valid Hugging Face Token that has access to Llama 3.1.

When we create a new configuration — or check out an existing configuration from version control — we need to initialize the directory with `terraform init`. Initializing will download and install our AWS provider as well as the `sagemaker-llm` module.

Bash

```bash
terraform init
 
# Initializing the backend...
# Initializing modules...
# Downloading registry.terraform.io/philschmid/llm-sagemaker/aws 0.1.0 for sagemaker-huggingface...
# - sagemaker-huggingface in .terraform/modules/sagemaker-huggingface
 
# Initializing provider plugins...
# - Finding hashicorp/aws versions matching "5.60.0"...
# - Finding hashicorp/random versions matching "3.1.0"...
```

### Deploy the Llama 3.1 8B instruct model

To deploy/apply our configuration we run `terraform apply` command. Terraform will then print out which resources are going to be created and ask us if we want to continue, which can we confirm with `yes`.

Bash

```bash
terraform apply
```

Now Terraform will deploy our model to Amazon SageMaker as a real-time endpoint. This can take 5-10 minutes.

### Test the endpoint and run inference

To test our deployed endpoint we can use the [aws sdk](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html#API_runtime_InvokeEndpoint_SeeAlso) in our example we are going to use the Python SDK (`boto3`), but you can easily switch this to use Java, Javascript, .NET, or Go SDK to invoke the Amazon SageMaker endpoint.

To be able to invoke our endpoint we need the endpoint name. You can get the endpoint name by inspecting the output of Terraform with `terraform output endpoint_name` or going to the SageMaker service in the AWS Management console.

We create a new file `request.py` with the following snippet.

*Make sure you have configured your credentials (and region) correctly*

Python

```python
import boto3
import json
 
client = boto3.client("sagemaker-runtime")
 
ENDPOINT_NAME = "YOUR_ENDPOINT_NAME"
 
body = {
    "messages": [
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "What is deep learning?"},
    ],
    "top_p": 0.6,
    "temperature": 0.9,
    "max_tokens": 512,
}
 
response = client.invoke_endpoint(
    EndpointName=ENDPOINT_NAME,
    ContentType="application/json",
    Accept="application/json",
    Body=json.dumps(body),
)
response = json.loads(response["Body"].read().decode("utf-8"))
print(response["choices"][0]["message"]["content"])
```

Now we can execute our request.

Bash

```bash
python3 request.py
# Deep learning is a subset of machine learning that involves the use of artificial neural networks (ANNs) with multiple layers to analyze and interpret data. These neural networks are designed to mimic the structure and function of the human brain, with layers of interconnected nodes or "neurons" that process and transmit information.
#
# The key characteristics of deep learning are:
# 
# 1. Multiple layers: Deep learning models typically consist of multiple layers of interconnected nodes, with each layer processing and transforming the input data in some way.
# 2. Neural networks: Deep learning models are based on artificial neural networks, which are designed to mimic the structure and function of the human brain.
# 3. Non-linear transformations: Deep learning models use non-linear transformations to process and transform the input data, allowing them to learn complex patterns and relationships.
# 4. Training with large datasets: Deep learning models require large amounts of data to train, as they need to learn from a vast number of examples to develop their predictive capabilities.
```

### Destroy the infrastructure

To clean up our created resources we can run `terraform destroy`, which will delete all the created resources from the module.

## Conclusion

The [llm-sagemaker](https://registry.terraform.io/modules/philschmid/llm-sagemaker/aws/latest) terraform module abstracts away the heavy lifting for deploying open LLMs to Amazon SageMaker away, which enables controlled, consistent and understandable managed deployments after concepts of IaC. This should help companies to move faster and include deployed models to Amazon SageMaker into their existing Applications and IaC definitions.

Give it a try and tell me know what you think about the module. Its still a very basic module. If you have feature requests please open an [issue](https://github.com/philschmid/terraform-aws-llm-sagemaker).

* * *

Thanks for reading! If you have any questions or feedback, please let me know on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
