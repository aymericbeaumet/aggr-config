---
title: Programmatically manage 🤗 Inference Endpoints
link: https://www.philschmid.de/inference-endpoints-iac
source: philschmid-de
published: 2023-12-20T00:00:00Z
updated: 2023-12-20T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: In this blog post you will learn how to use the huggingface_hub library to create, send requests to, pause, and delete Hugging Face Inference Endpoints.
content: extracted
html: 2023-12-20-programmatically-manage-hugs-inference-endpoints.html
preview:
  file: 2023-12-20-programmatically-manage-hugs-inference-endpoints.preview-0dea92802dd1.webp
  width: 256
  height: 134
  alt: Programmatically manage 🤗 Inference Endpoints
  color: '#53545c'
images:
- source: https://www.philschmid.de/static/blog/inference-endpoints-iac/thumbnail.jpg
  original:
    file: 2023-12-20-programmatically-manage-hugs-inference-endpoints.image-5884b52f21f9.jpg
    width: 2400
    height: 1254
  color: '#0d1017'
---

Infrastructure as Code (IaC) allows us to manage and provise infrastructure through programmatic interfaces. This approach can help companies to easier manage, update and use Generative AI in more automatic setups.

By leveraging IaC, we can streamline the deployment and maintenance of models and unlock new capabilities like batch processing, automatic model end-to-end model evaluation.

I am happy to share the [huggingface\_hub](https://huggingface.co/docs/huggingface_hub/guides/inference_endpoints) python library now supports [Hugging Face Inference Endpoints](https://ui.endpoints.huggingface.co/). This allows you to programmatically manage Inference Endpoints. You can now create, update, pause, delete or send requests using the `huggingface_hub` library.

[Hugging Face Inference Endpoints](https://ui.endpoints.huggingface.co/) offers an easy and secure way to deploy Generative AI models for use in production. Inference Endpoints empower developers and data scientists alike to create AI applications without managing infrastructure: simplifying the deployment process to a few clicks, including handling large volumes of requests with autoscaling, reducing infrastructure costs with scale-to-zero, and offering advanced security.

The [huggingface\_hub](https://huggingface.co/docs/huggingface_hub/index) library allows you to interact with the **[Hugging Face Hub](https://hf.co/)**, a machine learning platform for creators and collaborators.

## End-to-End Example

This tutorial will guide you through an example of managing Inference Endpoints using the **`huggingface_hub`** library. We'll focus on the model [Zehpyr](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta) for this example. Support for managing inference endpoints came with version `0.20`.

Bash

```bash
pip install "huggingface_hub>=0.20.1" --upgrade
```

Before we can create and managed our endpoints we need to login using a [Hugging Face Token.](https://huggingface.co/settings/tokens) We also want to define our `namespace` the namespace is the organization/account we use. It is the identifier in the url when you go to your profile, e.g. `huggingface`.

Python

```python
from huggingface_hub import login
 
# set credentials
login(token="YOUR_TOKEN")
# set namespace for IE account
namespace="huggingface"
```

The `huggingface_hub` library provides a `create_inference_endpoints` method which accepts the same parameters as the [HTTP Endpoint from Inference Endpoints](https://huggingface.co/docs/inference-endpoints/api_reference). This means we need to define:

- `endpoint_name`: The name for the endpoint
- `repository`: The model repository to use
- `framework`: The framework to use, most likely `pytorch`
- `task`: The task to use for LLMs this is `text-generation`
- `vendor`: The cloud provider to use, e.g. `aws`
- `region`: The region to use, e.g. `us-east-1`
- `type`: The type security type to use, e.g. `protected`
- `instance_size`: The instance size to use can be found in the UI
- `instance_type`: The instance type to use can be found in the UI
- `accelerator`: The accelerator to use, e.g. `gpu`
- `namespace`: The namespace to use, e.g. `huggingface`
- `custom_image`: The custom image to use, this is optional and can be used to define a custom image to use.

We are going to use `custom_image` to use [Text Generation Inference](https://github.com/huggingface/text-generation-inference). This is the same image you get when going to the UI and deploying a LLM.

In our example we want to use the model [Zephyr](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta). First lets define our custom image. The `custom_image` allows us also to define TGI specific parameters like `MAX_BATCH_PREFILL_TOKENS`, `MAX_INPUT_LENGTH` and `MAX_TOTAL_TOKENS`. Below is an example of how those, make sure to adjust them to your needs, e.g. input length.

Python

```python
# define TGI as custom image
custom_image = {
    "health_route": "/health",  # Health route for TGI
    "env": {
        "MAX_BATCH_PREFILL_TOKENS": "2048", # can be adjusted to your needs
        "MAX_INPUT_LENGTH": "1024", # can be adjusted to your needs
        "MAX_TOTAL_TOKENS": "1512", # can be adjusted to your needs
        "MODEL_ID": "/repository",  # IE will save the model in /repository
    },
    "url": "ghcr.io/huggingface/text-generation-inference:1.3.3",
}
```

After we defined our custom image we can create our inference endpoint.

Python

```python
# Create Inference Endpoint to run Zephyr 7B
print("Creating Inference Endpoint for Zephyr 7B")
zephyr_endpoint = create_inference_endpoint(
    "zehpyr-ie-test",
    repository="HuggingFaceH4/zephyr-7b-beta",
    framework="pytorch",
    task="text-generation",
    vendor="aws",
    region="us-east-1",
    type="protected",
    instance_size="medium",
    instance_type="g5.2xlarge",
    accelerator="gpu",
    namespace=namespace,
    custom_image=custom_image,
)
```

The `huggingface_hub` library will return an `InferenceEndpoint` object. This object allows us to interact with the endpoint. This means we can directly send requests, pause, delete or update the endpoint. We can also call the `wait` method to wait for the endpoint to be ready for inference. This is super handy when you want to run inference right after creating the endpoint, e.g. for batch processing or automatic model evaluation.

*Note: This may take a few minutes.*

Python

```python
print("Waiting for endpoint to be deployed")
zephyr_endpoint.wait()
```

After the endpoint is ready the `.wait()` method will return. This means we can test our endpoint and send requests.

Python

```python
print("Running Inference")
res = zephyr_endpoint.client.text_generation(
    "<|system|>\nYou are a friendly chatbot who always responds in the style of a pirate.</s>\n<|user|>\nHow many helicopters can a human eat in one sitting?</s>\n<|assistant|>"
)
print(res)
# Matey, I'm afraid I've never heard of a human eating a helic
```

For more details about how to use the [InferenceClient](https://huggingface.co/docs/huggingface_hub/v0.19.3/en/package_reference/inference_client#huggingface_hub.InferenceClient), check out the [Inference guide](https://huggingface.co/docs/huggingface_hub/guides/inference).

If we want to temporarily pause the endpoint you can call the `pause` method.

Python

```python
print("Pausing Inference Endpoint")
zephyr_endpoint.pause()
```

To delete the endpoint we call the `delete` method.

Python

```python
print("Deleting Inference Endpoint")
zephyr_endpoint.delete()
```

## Conclusion

In this tutorial, you've learned how to use the `huggingface_hub` library to create, send requests to, pause, and delete Hugging Face Inference Endpoints. This allows for efficient and scalable management of Generative AI models in production environments.

We have a more in depth documentation about managing Inference Endpoints in our [documentation](https://huggingface.co/docs/huggingface_hub/guides/inference_endpoints). The `huggingface_hub` offers more capabilities like listing endpoints, updating scaling and more.

If you are missing a feature or have feedback, please let us know.

* * *

Thanks for reading! If you have any questions, feel free to contact me on [Twitter](https://twitter.com/_philschmid) or [LinkedIn](https://www.linkedin.com/in/philipp-schmid-a6a2bb196/).
