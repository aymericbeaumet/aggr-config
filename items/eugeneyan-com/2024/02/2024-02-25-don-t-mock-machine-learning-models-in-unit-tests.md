---
title: Don't Mock Machine Learning Models In Unit Tests
link: https://eugeneyan.com//writing/unit-testing-ml/
source: eugeneyan-com
published: 2024-02-25T00:00:00Z
updated: 2024-02-25T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- machinelearning
- python
summary: How unit testing machine learning code differs from typical software practices
content: extracted
html: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.html
preview:
  file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.preview-1df24ccfac0b.webp
  width: 256
  height: 134
  color: '#3c3e3a'
images:
- source: https://eugeneyan.com/assets/og_image/unit-testing-ml.png
  original:
    file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-d104bbf78bb9.png
    width: 1200
    height: 630
  variants:
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-f1df1f9e5eb7.webp
    width: 320
    height: 168
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-cf86fc83c678.webp
    width: 640
    height: 336
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-c816837229ac.webp
    width: 960
    height: 504
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-a1ea7e8c560d.webp
    width: 1200
    height: 630
  color: '#272822'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2024-02-25-don-t-mock-machine-learning-models-in-unit-tests.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

I’ve been applying typical unit testing practices to machine learning code and it hasn’t been straightforward. In software, units are small, isolated pieces of logic that we can test independently and quickly. In machine learning, models are blobs of logic learned from data, and machine learning code is the logic to learn and use these derived blobs of logic. This difference makes it necessary to rethink how we unit test machine learning code.

## How ML code differs from regular software

**In software, we write code that *contains* logic; in ML, we write code that *learns* logic and then uses that learned logic.** Software code transforms input data + handcrafted logic into expected output. We can then test these outputs against asserts. In contrast, machine learning code transforms input data + expected output into learned logic (i.e., a model).

\\\[\\text{Software}: \\text{Input Data} + Handcrafted \\text{ Logic} = \\text{Expected Output}\\\] \\\[\\text{Machine Learning}: \\text{Input Data} + \\text{Expected Output} = Learned \\text{ Logic}\\\]

Thus, in machine learning, instead of writing code that contains logic, we write code to learn logic, such as via [building a decision tree](https://github.com/eugeneyan/testing-ml/blob/master/src/tree/decision_tree.py#L149) or [finetuning a hallucination classifier](https://github.com/eugeneyan/visualizing-finetunes/blob/main/3_ft_usb_then_fib.ipynb). Because the logic that acts on the input data is embedded within the model, if we want to test the learned logic, we’ll need to load the model, perform inference on some sample output, and then assert if the output matches the expected input.

**In software, we typically mock dependencies like APIs; in ML, we want to test the actual model (sometimes).** When unit testing software, it’s good practice to mock database calls, filesystem access, sending emails/push notifications, etc. However, in ML, there are scenarios where we’ll want to test against the actual model.

For example, we want to test that loss decreases with each batch and the model can overfit (before wasting compute on an hopeless run.) If the model is a classifier, we want to check that the inference logic is correct. For instance, two models may have different output classes: [Google’s T5 NLI model](https://huggingface.co/google/t5_11b_trueteacher_and_anli) classifies factual consistency with class = 1 while [Meta’s BART NLI model](https://huggingface.co/facebook/bart-large-mnli) classifies it with class = 2!

**Machine learning / language models can be large and unwieldy.** Some neural networks can be in the billions of parameters, exceeding what a laptop or standard dev environment can load. And even if we have the memory for smaller models, they are slow to load and perform inference on, testing our patience as we unit test while coding.

## Some guidelines for unit testing ML code & models

(These are a work in progress and my thinking’s still evolving—all feedback welcome!)

**Use small, simple data samples.** Avoid loading CSVs or Parquet files as sample data. (It’s fine for integration tests and evals but not unit tests.) Define sample data directly in unit test code—so that the test is self-contained—to test key functionality such as:

- Splitting into train/test tests when you have custom logic
- Custom implementations, such as Cosine or Euclidean distance in Java
- Preprocessing such as data augmentation or encoding
- Postprocessing such as diversification or filtering recommendations
- Error handling for empty or malformed input

**When viable, test against random or empty weights.** For example, we can initialize a model configuration with random weights to test output shape and device movement (from CPU to GPU and back). Here’s an example of how to initialize a model without having to download the weights and then assert the output shape:

```python
from transformers import AutoConfig, AutoModelForSequenceClassification

model_name = "valhalla/distilbart-mnli-12-1"
config = AutoConfig.from_pretrained("valhalla/distilbart-mnli-12-1")
model = AutoModelForSequenceClassification.from_config(config)
assert model.classification_head.out_proj.out_features == 3
```

The accelerate library also has [an example of initializing a model with empty weights](https://github.com/huggingface/accelerate/blob/main/tests/test_big_modeling.py#L955):

```python
def test_dispatch_model_bnb(self):
    """Tests that `dispatch_model` quantizes int8 layers"""
    from huggingface_hub import hf_hub_download
    from transformers import AutoConfig, AutoModel, BitsAndBytesConfig
    from transformers.utils.bitsandbytes import replace_with_bnb_linear

    with init_empty_weights():
        model = AutoModel.from_config(AutoConfig.from_pretrained("bigscience/bloom-560m"))

    quantization_config = BitsAndBytesConfig(load_in_8bit=True)
    model = replace_with_bnb_linear(
        model, modules_to_not_convert=["lm_head"], quantization_config=quantization_config
    )

    model_path = hf_hub_download("bigscience/bloom-560m", "pytorch_model.bin")

    model = load_checkpoint_and_dispatch(
        model,
        checkpoint=model_path,
        device_map="balanced",
    )

    assert model.h[0].self_attention.query_key_value.weight.dtype == torch.int8
    assert model.h[0].self_attention.query_key_value.weight.device.index == 0

    assert model.h[(-1)].self_attention.query_key_value.weight.dtype == torch.int8
    assert model.h[(-1)].self_attention.query_key_value.weight.device.index == 1
```

**Write critical tests against the actual model.** If they take a while to run, [mark them as slow](https://docs.pytest.org/en/latest/how-to/mark.html#registering-marks) and run only when needed (e.g., pre-commit and pre-merge). Some essentials include:

- Verify training is done correctly, such as loss going down, model overfitting, and training till convergence on a small sample of data
- Verify model outputs match expectation, such as 0.99 = unsafe instead of safe
- Verify model server can start, take batch input, and return the expected output

**Don’t test external libraries.** We can assume that external libraries work. Thus, no need to test data loaders, tokenizers, optimizers, etc.

What are your best practices for unit testing machine learning code and models? I would love to hear from you. [Please reach out!](https://twitter.com/eugeneyan)

## Further reading

- [How to Test Machine Learning Code and Systems](https://eugeneyan.com/writing/testing-ml/)
- [Writing Robust Tests for Data & Machine Learning Pipelines](https://eugeneyan.com/writing/testing-pipelines/)
- [Effective Testing for Machine Learning Systems](https://www.jeremyjordan.me/testing-ml/)
- [How to Trust Your Deep Learning Code](https://krokotsch.eu/posts/deep-learning-unit-tests/)
- [A Recipe for Training Neural Networks](https://karpathy.github.io/2019/04/25/recipe/)
- [How to Unit Test Deep Learning](https://theaisummer.com/unit-test-deep-learning/)
- [Testing Data Science and MLOps Code](https://microsoft.github.io/code-with-engineering-playbook/machine-learning/ml-testing/)

> The problem when mocks happen when all your unit test passes and the program fails on integration. The mocks are a pristine place where your library unit test works like a champ. Bad mocks or bad library? Or both. Developers are then sent to debug the unit test… overhead.

> Probablistic tests create an impossible problem: if you tighten your assertions you struggle with meaningless failing tests and normalize ignoring test failures, while if you loosen your assertions your tests aren’t really asserting anything any more. And there isn’t a happy balance: if you go somewhere in the middle, you end up having both problems.

> How does it matter whether I inline my test data inside the unit test code, or have my unit test code load that same data from a checked-in file instead?
>
> > It makes tests self-contained and easier to reason about. As a side-effect, random tests won’t accidentally break whenever you change some seemingly unrelated csv file. As a rule of thumb, I also only assert on input/output values that are explicitly defined as part of the test body. Saves a ton of time chasing down fixture definitions.

> I was expecting an article about side effects of hurting an LLM’s feelings in tests.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Feb 2024). Don't Mock Machine Learning Models In Unit Tests. eugeneyan.com. https://eugeneyan.com/writing/unit-testing-ml/.

or

```
@article{yan2024unit,
  title   = {Don't Mock Machine Learning Models In Unit Tests},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2024},
  month   = {Feb},
  url     = {https://eugeneyan.com/writing/unit-testing-ml/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
