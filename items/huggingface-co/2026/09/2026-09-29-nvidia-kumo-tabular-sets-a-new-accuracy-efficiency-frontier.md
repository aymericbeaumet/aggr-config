---
title: NVIDIA Kumo Tabular Sets a New Accuracy-Efficiency Frontier for Tabular Prediction
link: https://huggingface.co/blog/nvidia/kumo-tabular
source: huggingface-co
published: 2026-09-29T15:30:38Z
updated: 2026-09-29T15:30:38Z
first_seen: 2026-09-29T19:40:04.433827810Z
content: extracted
html: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.html
preview:
  file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.preview-a373c06c471a.webp
  width: 256
  height: 138
  color: '#acc489'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/HyVlq-3d7EVj_M-m_TfHJ.png
  original:
    file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-902cab8107c2.png
    width: 1200
    height: 648
  color: '#dadbda'
- source: https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/TkT695UAWJnUIyEydIYRv.png
  original:
    file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-d79c408e1031.png
    width: 1432
    height: 560
  variants:
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-1e856fbbce3b.webp
    width: 320
    height: 125
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-8a52e5ce9854.webp
    width: 640
    height: 250
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-03aa9d8414cc.webp
    width: 960
    height: 375
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-e1403fb4fab2.webp
    width: 1280
    height: 501
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-605905c1364f.webp
    width: 1432
    height: 560
  color: '#f3f6f6'
- source: https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/ipJNJTNO5ZH2zviIdR8eo.png
  original:
    file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-c92a902d4f7b.png
    width: 6968
    height: 5360
  variants:
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-429f50fb3c6e.webp
    width: 320
    height: 246
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-cacdbf8727b7.webp
    width: 640
    height: 492
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-5c8e3c51846f.webp
    width: 960
    height: 738
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-dd7748dd3330.webp
    width: 1280
    height: 985
  - file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-d77dc245b131.webp
    width: 1600
    height: 1231
  color: '#f9f9f9'
- source: https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/CpOXm3g9du6Uw3zQDh2QC.png
  original:
    file: 2026-09-29-nvidia-kumo-tabular-sets-a-new-accuracy-efficiency-frontier.image-1608b1ae6a0b.png
    width: 3036
    height: 1434
  color: '#fcfcfc'
---

## Highlights (TL;DR)

NVIDIA Kumo Tabular, part of the NVIDIA Kumo Structured model collection, is an open foundation model for tabular data now available on [Hugging Face](https://huggingface.co/nvidia/Kumo-Tabular). Given a table of labeled rows, it predicts the labels of new rows in a single forward pass, with no training, no tuning, and no feature engineering, for both classification and regression. It was pretrained only on artificial data, comes in three sizes (28M to 215M parameters), runs through our [open-source library](https://github.com/NVIDIA/structured-data-models), and is released under the [OpenMDW-1.1 license](https://openmdw.ai/license/1-1/) for commercial use. It ranks first on the four benchmarks [TabArena](https://github.com/autogluon/tabarena), [BeyondArena](https://github.com/autogluon/tabarena), [TALENT](https://github.com/LAMDA-Tabular/TALENT) and [ScoringBench](https://github.com/jonaslandsgesell/ScoringBench).

- **Model Code:** [https://github.com/NVIDIA/structured-data-models](https://github.com/NVIDIA/structured-data-models)
- **Model Weights:** [https://huggingface.co/nvidia/Kumo-Tabular](https://huggingface.co/nvidia/Kumo-Tabular)

## The Shift to Tabular Foundation Models

Tabular data is the backbone of enterprise machine learning. Customer records, transactions, sensor logs, claims, and orders all live in tables, and predicting churn, default, demand, or price from them is among the most common machine learning tasks in industry. For two decades, this work has been done with gradient-boosted trees, and it has worked well. But the lifecycle around those models has barely changed. Every new question means collecting labels, engineering features, searching hyperparameters, validating, and deploying a model that knows nothing about tables in general and learns each task from scratch.

Large Language Models showed a different way of working with new tasks. Given a few examples in the prompt, a pretrained model solves the task without updating a single weight. This is *in-context learning*, and it applies to tables just as well as to text: a model pretrained on millions of tables can read a labeled table as its context and predict the labels of new rows directly.

Today, we are releasing **NVIDIA Kumo Tabular** ([GitHub](https://github.com/NVIDIA/structured-data-models), [HuggingFace](https://huggingface.co/nvidia/Kumo-Tabular)), an open foundation model for tabular classification and regression. Given a table with labeled rows and the rows you want predictions for, Kumo Tabular returns class probabilities or numeric predictions in a single forward pass.

## How Kumo Tabular Works

Kumo Tabular is a Transformer built around the structure of a table, utilizing column, row and in-context attention as introduced in [TabICL](https://github.com/soda-inria/tabicl) and [TabPFN](https://github.com/PriorLabs/tabpfn). To predict a label it has to do three things: **(1)** understand what each value means within its column, **(2)** understand how the columns of a row interact, and **(3)** relate the context rows with existing labels to the query rows with unknown labels. Kumo Tabular achieves this as follows:

[![architecture](https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/TkT695UAWJnUIyEydIYRv.png)](https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/TkT695UAWJnUIyEydIYRv.png)

**Cell Embedding:** A group of cells becomes a token. Numerical and categorical values pass through Fourier features, sines and cosines of learned frequencies, with separate weights for each type. Missing values need no imputation and are treated specially. Finally, every token in the context receives a label embedding.

**Row Embedding:** We then turn each row into an embedding by alternating two kinds of attention multiple times. Column attention looks down a single column and learns what a value means in the distribution of its column, *e.g.*, whether a `42` is typical or extreme, via induced self-attention. Its cost therefore grows linearly with the number of rows. Row attention looks across the tokens of a single row and learns how features interact, with rotary positions to tell columns apart. Four learnable `[CLS]` tokens join each row and act as the final readout of a row. After this row compression, the cost of the final stage no longer depends on the number of columns.

**In-context Learning:** A final Transformer operates on the row embeddings. Context rows attend to each other, while query rows attend to context rows only. Each prediction therefore depends only on the context and on the row itself, not on which other rows are scored alongside it. Because the context never looks at the queries, its keys and values are computed once and can be reused for follow-up predictions. Query rows utilize Test-GQA, which shrinks the cache that every prediction reads. A head turns each query row into class probabilities for classification and 999 quantiles for regression, from which a point prediction and an uncertainty estimate follow.

**Length-aware Attention Temperature:** Softmax attention spreads out as the number of keys grows. Attention that is sharp over a few hundred rows can dissolve over tens of thousands, which is exactly the situation when a table at inference is much larger than a typical training table. Kumo Tabular therefore scales every query by a temperature that grows with the logarithm of the number of keys, with a coefficient learned separately for each attention head. The result is attention that stays sharp as tables grow longer or wider.

## How Kumo Tabular was Built

Kumo Tabular is pretrained entirely on artificial tables. Each training table is sampled from a *Structural Causal Model (SCM)* in the six steps shown below:

[![prior](https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/ipJNJTNO5ZH2zviIdR8eo.png)](https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/ipJNJTNO5ZH2zviIdR8eo.png)

We first draw a configuration for the whole table, from its size and task to its mechanisms and missingness. A random causal graph then links hidden variables, evaluated from root to leaf via randomly drawn functions at every node (*e.g.*, linear maps, small neural networks, trees or Gaussian processes). Some nodes become numerical or categorical columns, one becomes the target, and the rest stay hidden, like the unmeasured causes behind real data. Post-processing correlates groups of columns, clips outliers, and injects missing values, and a quick tree-ensemble check discards any table without a learnable signal. Because the generator is a procedural sampler rather than a trained model, it produces an endless supply of tables, each with a new graph and new mechanisms.

Real-world tables are messy, so we built more of their imperfections into the generator. Values go missing in several patterns, some features are coarsened so that duplicate rows may disagree on their label, some categorical columns carry many levels, and regression targets can be heavy-tailed. A model that has seen millions of such tables learns to handle these imperfections without any cleanup.

On every artificial table, the model sees most of the rows with their labels as context and learns to predict the labels of the remaining rows, with a cross-entropy loss for classification and a quantile loss for regression. Classification and regression are trained as separate models. Similarly to TabICLv2, training runs in three stages. The first and longest stage uses tables of 1,024 rows and up to 100 columns and teaches the model what tables look like. The second stage varies the context from 400 to 10,240 rows, and the third extends it to 60,000 rows, still with up to 100 columns. In total, Kumo Tabular-Small/Medium/Large saw about 35/71/137 million artificial tables.

Our training recipe and artificial data generators will be released soon.

## Performance

We ran all three Kumo Tabular sizes with default settings against the full [TabArena](https://github.com/autogluon/tabarena) leaderboard, spanning tuned gradient-boosted trees, AutoGluon, and the latest tabular foundation models. Kumo Tabular ranks first overall with an ELO of 1950 while running 17 faster than [LimiX-2](https://github.com/limix-ldm-ai/LimiX) under a uniform single RTX 6000 Pro evaluation setup. Across all three three model sizes, Kumo Tabular establishes a new state-of-the-art on the accuracy-efficiency Pareto front:

[![pareto](https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/CpOXm3g9du6Uw3zQDh2QC.png)](https://cdn-uploads.huggingface.co/production/uploads/684040a5de1ad7f4fcec9508/CpOXm3g9du6Uw3zQDh2QC.png)

We also evaluated Kumo Tabular on [BeyondArena](https://github.com/autogluon/tabarena), [TALENT](https://github.com/LAMDA-Tabular/TALENT) and [ScoringBench](https://github.com/jonaslandsgesell/ScoringBench). On BeyondArena, Kumo Tabular reaches an ELO of 1418 with an Improvability score of 7.78%, placing first on the leaderboard. On TALENT, it achieves the top overall ranking across classification accuracy, classification log-loss, and regression RMSE, with average ranks of 6.67, 3.98, and 4.22. On ScoringBench, a benchmark for predictive distributions, Kumo Tabular-Large and Medium rank first and second on average rank.

## Limitations

Kumo Tabular works on numerical and categorical columns only, while text, images, or timestamps can be turned into features via built-in pre-processing recipes. A single forward pass covers up to 10 classes, which the library extends to any number of classes with error-correcting output codes. Accuracy may degrade on tables far beyond the training ranges or when the query rows come from a different distribution than the context rows, so, as with any predictive model, validate accuracy and calibration on your own held-out data before deployment.

## Demo

Kumo Tabular runs via NVIDIA's newly released GPU-native library for [`structured-data-models`](https://github.com/NVIDIA/structured-data-models). The library downloads the weights from the Hub on first use and provides the preprocessing, ensembling, and many-class handling used in our evaluations. The code below is all it takes to go from a `pandas.DataFrame` to a prediction:

```python
import sdm  # structured-data-models

# Tensorize tabular data:
table = sdm.TableTensor.from_pandas(pd.load_csv(...), device="cuda")
na_mask = table["target"].isnan()

model = sdm.models.KumoTabular(device="cuda")
pred = model(
    # In-context examples (features/targets):
    x_context=table[~na_mask].drop_columns("target"),
    y_context=table[~na_mask, "target"],
    # Prediction examples (features):
    x_query=table[na_mask].drop_column("target"),
)
```

## Start Building with Kumo Tabular

Kumo Tabular is released under the [OpenMDW License Agreement, version 1.1](https://huggingface.co/blog/nvidia/\(https://openmdw.ai/license/1-1/). NVIDIA believes Trustworthy AI is a shared responsibility, and we have established policies and practices to enable development for a wide array of AI applications. When downloaded or used in accordance with our terms of service, developers should work with their supporting model team to ensure this model meets requirements for the relevant industry and use case and addresses unforeseen product misuse. Please report model quality, risk, security vulnerabilities, or NVIDIA AI concerns [here](https://github.com/NVIDIA/structured-data-models/issues).

- **Model Code:** [https://github.com/NVIDIA/structured-data-models](https://github.com/NVIDIA/structured-data-models)
- **Model Weights:** [https://huggingface.co/nvidia/Kumo-Tabular](https://huggingface.co/nvidia/Kumo-Tabular)

## Acknowledgements

We thank [David Holzmüller](https://dholzmueller.github.io/) for contributing significant ideas and ablations to Kumo Tabular. We thank [Vignesh Kothapalli](https://kvignesh1420.github.io/) for his help on Kumo Tabular during his internship.
