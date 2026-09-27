---
title: Training an LLM-RecSys Hybrid for Steerable Recs with Semantic IDs
link: https://eugeneyan.com//writing/semantic-ids/
source: eugeneyan-com
published: 2025-09-14T00:00:00Z
updated: 2025-09-14T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- learning
- llm
- recsys
- 🛠
- 🩷
summary: An LLM that can converse in English & item IDs, and make recommendations w/o retrieval or tools.
content: extracted
html: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.html
preview:
  file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.preview-134f6117ccd8.webp
  width: 256
  height: 134
  color: '#3a3a38'
images:
- source: https://eugeneyan.com/assets/og_image/semantic-ids-og.jpg
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-d4904af18931.jpg
    width: 1200
    height: 630
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-0bcfdab24978.webp
    width: 320
    height: 168
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-7621eb0dcedd.webp
    width: 640
    height: 336
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-f450ba01db40.webp
    width: 960
    height: 504
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-06205f593fc8.webp
    width: 1200
    height: 630
  color: '#343433'
- source: https://eugeneyan.com/assets/semantic-id.webp
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-76423cf7a0d5.webp
    width: 1200
    height: 489
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-df2406afef9a.webp
    width: 320
    height: 130
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/rqvae.webp
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-564d20b66f71.webp
    width: 1200
    height: 485
  color: '#f9f9f9'
- source: https://eugeneyan.com/assets/rqvae-curves.webp
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-12529a253cf0.webp
    width: 1200
    height: 638
  color: '#fcfcfb'
- source: https://eugeneyan.com/assets/rqvae-curves2.webp
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-8119712a3f55.webp
    width: 1200
    height: 641
  color: '#fbfcfb'
- source: https://eugeneyan.com/assets/codebook-usage.webp
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-65490be2b776.webp
    width: 1990
    height: 1189
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-6258c052285d.webp
    width: 320
    height: 191
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/codebook-usage2.webp
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-96b3a29c2b28.webp
    width: 1990
    height: 1189
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-f79117bfc24d.webp
    width: 320
    height: 191
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://i.ytimg.com/vi/_0n4QS--3V8/hqdefault.jpg
  original:
    file: 2025-09-14-training-an-llm-recsys-hybrid-for-steerable-recs-with.image-88db3e5d2b7a.jpg
    width: 480
    height: 360
  color: '#323447'
---

I got nerdsniped when I first heard about Semantic IDs. The idea is simple: Instead of using random hash IDs for videos or songs or products, we can use semantically meaningful tokens that an LLM can natively understand. I wondered, could we train an LLM-recommender hybrid on the rich behavioral data that makes today’s recommender systems so effective?

To my surprise, we can! The result is a language model that can converse in both English and item IDs, not with retrieval or other tools, but as a single, “bilingual” model where items (i.e., semantic IDs) are part of its vocabulary. Like a recommender model, it can recommend items given historical interactions. But the big surprise—and capability unlock—was when I found that **I could simply chat with the model to steer its recommendations, and it could reason about its choices, offer explanations, and creatively name product bundles**.

Here’s a demo video. And here’s the [code](https://github.com/eugeneyan/semantic-ids-llm) to prep the data, train the model, and chat with it. (Note: This is a small model with very basic finetuning, so prompting matters a lot. Also, it’s not as general purpose and robust as most LLMs because of the limited finetuning.)

[YouTube video player](https://www.youtube.com/watch?v=_0n4QS--3V8)

## Wait, why is this useful?

This marries the best of recsys and language modeling. On one hand, language models have world knowledge and can eloquently talk about products, but are unaware of our catalog. Also, their recommendations are generic and suffer from popularity bias. Conversely, search and recommendation models are trained on our catalog and billions of user interactions. They excel at predicting what a user will click or buy next, but can’t be steered via natural language or reason on their choices. (What they have in common though, is learning on sequences.)

To do this, we first extend the language model’s vocabulary with semantic ID tokens like `<|sid_0|>`, `<|sid_1|>`, `<|sid_2|>`, etc. These tokens are used to represent the catalog. Then, we apply continued pretraining to teach the model the relationships between semantic IDs and the catalog, and then further finetune it on sequences of user behavior. Together, this teaches the model to make recommendations—from our catalog—based on user preferences.

While this LLM-recommender hybrid may not match the raw precision of a specialized multi-stage recsys, it offers a new capability: **steerability and reasoning on recommendations**. The result is a unified experience across search, recommendations, and chat, giving users the ability to find what they need in our catalog by simply asking for it.

> **User:** I like animal and cute games. `<|rec|>`
>
> > *Model interprets the natural language query (“animal and cute”) and generates semantic IDs for relevant games.*
> >
> > ```text
> > <|sid_start|><|sid_173|><|sid_324|><|sid_764|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_201|><|sid_397|><|sid_701|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_173|><|sid_305|><|sid_670|><|sid_768|><|sid_end|>
> > ```
>
> **Assistant:** “Animal Crossing: New Leaf”, “DISNEY INFINITY Starter Pack 3DS”, “Nintendogs + Cats: Golden Retriever and New Friends”

## Data and models

### Finding training data with both item metadata and user sequences

We’ll use data from the Video Games category of [Amazon Reviews 2023 (Hou et al., 2024)](https://amazon-reviews-2023.github.io). The data has rich product metadata and we can create user interaction sequences from it.

The dataset contains 137k products, each with fields for title, description, features, category, store information, ratings, prices, etc. After keeping only products with titles longer than 20 characters and descriptions longer than 100 characters, we’re left with 66k products. The behavioral data contains 737k records. From this, I built 79k user purchase sequences, each containing at least three valid items. These sequences had an average length of 6.5 items.

I also considered the [Amazon KDD Cup 2023 data (Amazon, 2023)](https://www.aicrowd.com/challenges/amazon-kdd-cup-23-multilingual-recommendation-challenge). It had 500k items and a focus on sequential behavior data. However, its multilingual nature added complexity, and the lack of a product category field made it difficult to work with. In the end, I went with the Amazon Reviews dataset to start simple and be frugal with my compute budget.

### Semantic IDs from RQ-VAEs

**Semantic IDs** ([Rajput et al., 2023](https://arxiv.org/abs/2305.05065); [Singh et al., 2023](https://arxiv.org/abs/2306.08121)) are hierarchical representations that **encode items into a sequence of tokens, replacing embeddings or hash-based IDs**. Unlike a conventional item ID (`B0040JHNQG`) that has no inherent meaning, a semantic ID (`<|sid_0|><|sid_256|><|sid_512|><|sid_768|>`) encodes item information. As a result of the training process, similar items naturally share common prefixes, forming a tree-like structure where each level of the ID represents increasingly fine-grained information about the item.

![Image](https://eugeneyan.com/assets/semantic-id.webp "Image")

The hierarchical structure of semantic IDs for food videos ([source](https://arxiv.org/abs/2306.08121))

**Residual Quantized Variational Autoencoders** (RQ-VAEs; [Zeghidour et al., 2021](https://arxiv.org/abs/2107.03312), [Lee et al., 2022](https://arxiv.org/abs/2203.01941)) is what we use to convert continuous embeddings into discrete semantic IDs. We start by encoding an item’s metadata (e.g., title, description) into an embedding. The RQ-VAE then uses hierarchical quantization to convert this embedding into a sequence of discrete tokens.

![Image](https://eugeneyan.com/assets/rqvae.webp "Image")

How RQVAEs convert embeddings to semantic IDs ([source](https://arxiv.org/abs/2305.05065))

This is an iterative process. For the first level, the model finds the closest vector in the first codebook to the input embedding; this vector becomes the first token of the Semantic ID. The model then calculates the quantization error, or *residual*, by subtracting the chosen codebook vector from the input embedding. For the second level, it finds the closest vector in the second codebook to this residual, which gives us the second token. This process repeats for each level, with each step capturing progressively finer details that the previous levels missed.

The loss function of the RQ-VAE is worth discussing, as understanding it is key to generating high-quality semantic IDs. The overall loss has two main components:

\\\[L(x) = L\_\\text{recon} + L\_\\text{rqvae}\\\]

The first component, the **reconstruction loss**, ensures the decoder can accurately reconstruct the original item embedding ($x$) from the final quantized representation ($\\hat{x}$). It’s a standard squared error loss:

\\\[L\_{recon} = ||x-\\hat{x}||^2\\\]

The second component, the **quantization loss**, measures how well the codebook vectors match the residuals generated by the encoder. It contains two terms:

\\\[L\_{\\text{rqvae}} := \\sum\_{i=0}^{m-1} \\left\[ \\|\\text{sg}\[r\_i\] - e\_{c\_i}\\|^2 + \\beta\\|r\_i - \\text{sg}\[e\_{c\_i}\]\\|^2 \\right\]\\\]

The first term ($|\\text{sg}\[r\_i\] - e\_{c\_i}|^2)$, the **codebook loss**, is responsible for training the codebook embeddings. It measures the distance between the encoder’s residual ($r\_i$) and the chosen codebook vector ($e\_{c\_i}$). The stop-gradient is applied to the encoder’s output ($\\text{sg}\[r\_i\]$) to treat the residuals as the fixed target. Thus, the gradients only flow to the codebook vector, pulling it closer to the encoder’s output.

The second term ($\\beta|r\_i - \\text{sg}\[e\_{c\_i}\]|^2$), the **commitment loss**, is responsible for training the encoder. It measures the same distance, but the stop-gradient is applied to the codebook vector ($\\text{sg}\[e\_{c\_i}\]$) instead. This stops updates to the codebook and forces the encoder to produce outputs, or *commit* to, vectors that are already in the codebook. The hyperparameter $\\beta$ controls the strength of this commitment penalty.

```python
# Pytorch code for loss function (without recursive loop)
reconstruction_loss = F.mse_loss(x, x_reconstructed)

codebook_loss = F.mse_loss(residual.detach(), codebook_vector)
commitment_loss = F.mse_loss(residual, codebook_vector.detach())
quantization_loss = codebook_loss + commitment_weight * commitment_loss

total_loss = recon_loss + quantization_loss
```

Through this process, the RQ-VAE produces a semantic ID as a sequence of tokens, one from each quantization level. Because similar items share common prefixes, language models can better understand product relationships, which is also useful for tree-based retrieval.

```python
# How to hierarchically encode embeddings to semantic IDs
def encode_to_semantic_ids(self, x: Tensor) -> Tensor:

    with torch.no_grad():
        residual = self.encode(x)
        indices_list = []

        for vq_layer in self.vq_layers:
            vq_output = vq_layer(residual)
            indices_list.append(vq_output.indices)
            residual = residual - vq_output.quantized
        
    return torch.stack(indices_list, dim=-1)
```

Nonetheless, one practical challenge is that this doesn’t guarantee a unique ID for every item. In my experiments with a three-level codebook, with each level having 256 codes, we saw collisions on ~10% of the 66k products. To solve this, I added a fourth level where I appended a sequentially increasing token to each ID to ensure every product is uniquely identifiable.

### SASRec, Qwen3-Embedding-0.6B, and Qwen3-8b

In addition to the RQ-VAE, we use three other models. First, we’ll train a **SASRec** on semantic IDs to validate their quality and compare it to an item ID SASRec baseline. Then, we use the **Qwen3-Embedding-0.6B model** to encode product metadata to embeddings. Finally, we finetune the **Qwen3-8B model** to understand and recommend items via semantic IDs.

[SASRec (Kang & McAuley, 2018)](https://arxiv.org/abs/1808.09781) is a sequential recommender inspired by the [Transformer](https://arxiv.org/abs/1706.03762) architecture. It encodes a user’s interaction history and uses a self-attention mechanism to weigh the most relevant past items in order to predict the next one. This allows the model to learn long-term dependencies in user behavior and thus outperform older recurrent models like RNNs and GRUs while being more efficient due to its parallelizable nature.

[Qwen3-Embedding-0.6B (Zhang et al., 2025)](https://arxiv.org/abs/2506.05176) is part of a series of embedding models available in 0.6B, 4B, and 8B sizes. They are trained via a multi-stage process that includes pre-training on synthetic data, followed by supervised finetuning and model merging for robustness. The 8B model achieves SOTA performance on the MTEB Multilingual benchmark.

[Qwen3-8B (Yang et al., 2025)](https://arxiv.org/abs/2505.09388) is a dense language model in the Qwen3 family. Despite being one of the smaller models, its post-training is optimized through strong-to-weak distillation on Qwen3-235B-A22B and Qwen3-32B. This makes the Qwen3-8B relatively capable for its size, surpassing bigger, previous-generation models like Qwen2.5-14B on more than half of the evaluated benchmarks, especially STEM and coding. Like other models in the Qwen3 series, the Qwen3-8B has dual thinking and non-thinking modes.

## Cleaning data and creating user sequences

First, we prepare the item metadata to ensure high-quality inputs for the semantic ID model. We start by excluding items that have titles with less than 20 characters or descriptions with less than 100 characters. This reduced the item count by half, from 137k to 66k unique items.

Next, we clean item descriptions with Gemini 2.5 Flash Lite ([Comanici et al., 2025](https://arxiv.org/abs/2507.06261)) by fixing truncated sentences, removing HTML, and reducing verbosity. This halved the average description length from 1,038 to 538 characters. Similarly, we remove promotional text and standardize formatting for titles, turning verbose listings like “NEW! LIMITED! Sega Saturn RGB SCART LEAD CABLE…” into a clean “Sega Saturn RGB SCART Cable”.

Then, we augmented the data by extracting structured metadata such as product type (Game, Hardware, Accessory), platform (PS4, Xbox, Wii), genre (Roguelike, Soulslike, Metroidvania), hardware type, brand, multiplayer modes, etc. This process had 98% coverage for platform information, 78% for brand identification, and 51% for genre classification.

Finally, to build the user sequences, we deduplicated on users and built interaction histories, resulting in 91.5k sequences. From these sequences, we excluded items that had no metadata, and then filtered out sequences that had less than three items. We also truncated sequences to a maximum length of 100 items (only 28 sequences were truncated). This gave us a dataset of 78.6k sequences that have a median length of 5 items and a mean length of 6.5 items.

## Training an RQ-VAE to output Semantic IDs

To embed the items, we use the Qwen3-Embedding-0.6B model. It supports customization of the input instruction for various tasks, and we add the prefix of “Given a product description, generate a semantic embedding that captures its key features and characteristics”. From this, we get 1024-dim embeddings via last token pooling and L2-normalize them before saving.

The RQ-VAE consists of an encoder, three quantization levels with 256 codes each, and a symmetric decoder. For training stability, we use the [rotation trick (Fifty et al., 2025)](https://arxiv.org/abs/2410.06424) as a replacement for the Straight-Through Estimator (to calculate the gradient for $L\_\\text{recon}$). Other optimizations included initializing codebooks with k-means clustering, resetting unused codes, and using a large batch size. I also tried a few techniques that didn’t help, such as updating the codebook with EMA and stopping gradients to the decoder.

The trained RQ-VAE achieved 89% unique semantic IDs across 66k products on the three quantization levels. To resolve the remaining collisions, I appended a fourth token that assigns a unique, sequential ID (0, 1, 2, …) to any products that share the same first three codes. This ensures every product has a unique 4-part semantic ID.

I ran a few dozen experiments to understand more about RQ-VAEs and their output semantic IDs, and to find an optimal configuration for the model. Here are some key findings.

First, I experimented with the commitment weight $\\beta$ that balances reconstruction accuracy and codebook commitment. I tested values of 0.25 (yellow), 0.5 (orange), and 1.0 (red), and found that a higher $\\beta$ of 1.0 led to the most unique IDs but also had the highest validation loss. And while a lower $\\beta$ of 0.25 led to slightly more unique IDs, $\\beta$ of 0.5 had the lowest validation loss. Thus, on this dataset, I trained subsequent RQ-VAEs with $\\beta$ = 0.5. (Note: This differs from the Semantic ID papers which used $\\beta$ = 0.25.)

![Image](https://eugeneyan.com/assets/rqvae-curves.webp "Image")

Curves with beta = 0.25 (yellow), 0.5 (orange), and 1.0 (red)

Aside: A quick primer on what each of these metrics mean:

- **loss/reconstruction**: Measures how well the RQ-VAE can reconstruct the original item embeddings after compressing it into semantic IDs and then decompressing it.
- **loss/vq**: Combined codebook and commitment loss across all levels. Ensures encoder outputs stay closs to the codebook vectors and codebook vectors adapt to encoder outputs. Essential for compressing embeddings into meaningful semantic IDs.
- **loss/total**: Sum of reconstruction loss and VQ loss to monitor overall progress.
- **loss/validation**: Total loss on held-out validation set to monitor generalization.
- **metrics/avg\_residual\_norm**: The amount of “leftover” residual after all quatization levels. Lower residual = codebooks are better at capturing the input embedding.
- **metrics/unique\_id\_proportion**: % of items with unique IDs in a batch. Checks for codebook collapse. Higher = better ability to distingush between items.

I also experimented with a shallower encoder and the impact of metadata cleaning. The shallower encoder (green) performed worse, increasing validation loss and reducing the number of unique IDs. However, investing in data cleaning paid off (blue). It led to a model with the lowest reconstruction and validation losses while having the highest proportion of unique IDs. I used the RQ-VAE from this run.

![Image](https://eugeneyan.com/assets/rqvae-curves2.webp "Image")

Curves with beta = 0.5 (orange), shallower encoder (green), and clean data (blue)

Another way to evaluate RQ-VAEs is to inspect the codebook utilization. Relatively uniform usage across all codes suggests the model is using its full expressive capacity. The final RQ-VAE demonstrates this well; across all three quantization levels, usage is spread evenly with low variance, as shown in the histogram below.

![Image](https://eugeneyan.com/assets/codebook-usage.webp "Image")

Example of an RQVAE with well distributed codebook usage

In contrast, a poorly converged RQ-VAE will have sparse and highly concentrated code usage. The histogram below shows this failure mode, where a few codes are overused, and the majority of the codebook is ignored.

![Image](https://eugeneyan.com/assets/codebook-usage2.webp "Image")

Example of an RQVAE with poorly distributed codebook usage

With the trained RQ-VAE, we encode all item embeddings into their semantic ID format, such as `<|sid_start|><|sid_191|><|sid_260|><|sid_716|><|sid_768|><|sid_end|>`. Then, we transformed the 78.6k user purchase sequences from regular IDs to semantic ID sequences. These sequences are the training data used to both validate the quality of the IDs with a SASRec baseline and to finetune the Qwen3-8B model.

## Training a SASRec on regular item IDs vs. semantic IDs

To validate that our semantic IDs capture meaningful product relationships, we train two variants of the SASRec, a baseline trained on regular item IDs and a variant trained on semantic IDs, and then compare their performance.

The **baseline SASRec** follows the standard architecture. It treats each product as a distinct, atomic unit, learning an embedding for it from scratch. This is based purely on behavioral patterns. The model uses 2 causal self-attention blocks, a 64-dimensional hidden state, and is trained on the discriminative task of distinguishing the next item in a sequence from randomly sampled negative items using a binary cross-entropy (BCE) loss.

```python
# Baseline SASRec predict function
def predict(self, input_ids: torch.Tensor, candidate_ids: torch.Tensor) -> torch.Tensor:
    """Predict scores for candidate items.

    Args:
        input_ids: Item sequences [batch_size, seq_length]
        candidate_ids: Candidate items to score [batch_size, num_candidates]

    Returns:
        Scores for each candidate [batch_size, num_candidates]
    """
    # Get sequence representations
    hidden_states = self.forward(input_ids)  # [B, T, H]

    # Use only the last hidden state for prediction
    final_hidden = hidden_states[:, -1, :]  # [B, H]

    # Get candidate embeddings
    candidate_embs = self.item_emb(candidate_ids)  # [B, C, H]

    # Compute scores via dot product
    scores = torch.bmm(candidate_embs, final_hidden.unsqueeze(-1)).squeeze(-1)  # [B, C]

    return scores
```

In contrast, the **Semantic ID SASRec** reframes recommendation as a conditional generative task. Instead of scoring items, its objective is to generate the next item’s 4-part semantic ID, token-by-token. This requires a larger architecture with 4 transformer blocks and 384-dim hidden states. Unlike the [TIGER paper](https://arxiv.org/abs/2305.05065) which uses a T5 encoder-decoder, this SASRec variant is decoder-only, making it a more direct and equitable comparison to the baseline SASRec. Because we use semantic IDs, instead having an embedding for each of the 66k items, we have a total of 1,024 token-level embeddings, with 256 tokens per level in the semantic ID.

```python
# Semantic ID SASRec predict function
def predict_next_item(self, input_ids: torch.Tensor, teacher_forcing: bool = True, 
    target_tokens: Optional[torch.Tensor] = None) -> Dict[str, torch.Tensor]:
    """Predict the next item's semantic ID tokens sequentially.

    Args:
        input_ids: Token sequences [batch_size, seq_length * num_levels]
        teacher_forcing: Use ground truth for conditioning during training
        target_tokens: Ground truth tokens for next item [batch_size, num_levels]

    Returns:
        Dictionary with logits for each level
    """
    hidden_states = self.forward(input_ids)  # [B, T*L, H]

    # Get representation at the last position as context for all previous items
    last_hidden = hidden_states[:, -1, :]  # [B, H]

    predictions = {}

    # Sequential generation: Predict each level conditioned on previous
    for level in range(self.num_levels):
        if level == 0:
            # Level 0: predict directly from sequence representation
            context = last_hidden
        else:
            # Levels 1-3: condition on previously predicted/true tokens
            if teacher_forcing and target_tokens is not None:
                # Use ground truth previous levels during training
                prev_tokens = target_tokens[:, :level]  # [B, level]
            else:
                # Use predicted tokens during inference
                prev_tokens = self._sample_from_predictions(predictions, level)

            prev_embeds = self.token_emb(prev_tokens)  # [B, level, input_dim]
            prev_embeds_projected = self.input_projection(prev_embeds)  # [B, level, H]
            prev_context = prev_embeds_projected.mean(dim=1)  # [B, H]

            # Combine with sequence context
            combined = torch.cat([last_hidden, prev_context], dim=-1)  # [B, 2*H]
            context = self.context_combiners[level - 1](combined)  # [B, H]

        # Predict current level
        logits = self.level_heads[level](context)  # [B, codebook_size]
        predictions[f"logits_l{level}"] = logits

    return predictions
```

This generative approach changes how we train and evaluate. The loss function is no longer a simple BCE loss but a sum of cross-entropy losses across each level of the semantic ID, forcing the model to predict the entire sequence correctly. Evaluation is also more complex, where instead of a dot product, an item’s score is its joint log-probability, calculated by summing the log-probs of generating each token. To improve training stability, we apply teacher forcing, where the ground-truth token from a prior level helps guide the prediction for the next level.

To evaluate both models, we use a validation set where we added 500 negative samples for each positive next item. While the baseline SASRec outperformed the semantic ID variant, the semantic model’s performance is respectable given the difficult generative task of predicting four correct tokens. Furthermore, the semantic ID variant has the **ability to handle cold-start items** by leveraging shared token prefixes from similar products, a capability the baseline lacks. This reveals the core trade-off, where gaining this ability to generalize requires 4x more predictions per item and requires higher training and inference compute.

| Model                  | Hit@10 | NDCG@10 | MRR    | Mean Rank | Median Rank |
| ---------------------- | ------ | ------- | ------ | --------- | ----------- |
| **Baseline SASRec**    | 0.2812 | 0.1535  | 0.1300 | 138.9     | 41.0        |
| **Semantic ID SASRec** | 0.2020 | 0.1138  | 0.1007 | 179.7     | 79.0        |

## Fine-tuning Qwen3-8B to Recommend Semantic IDs

Next, we teach a language model to converse in semantic IDs. For this, we finetune **Qwen3-8B** to become “bilingual”, fluent in both natural language and semantic IDs.

First, we build a training dataset of 4.2 million conversational examples to teach the model about semantic IDs and recommendations. The data covers several task types, including mapping semantic IDs to their corresponding text descriptions (and vice-versa), predicting the next item in a user’s sequence, understanding relationships between item categories, and multi-hop reasoning. Each of these examples are formatted as conversations with a system prompt, a user instruction, and an assistant’s response.

Then, we finetune the model in two phases. The first phase focuses on vocabulary extension, where we add 1,027 new tokens for the semantic IDs (i.e., `<|sid_start|>`, `<|sid_end|>`, `<|sid_0|>` to `<|sid_1023|>`, and `<|rec|>`) to the Qwen3-8B tokenizer and resize the model’s embedding matrix for them. During this phase, we freeze all model parameters except the input and output embedding layers, training 1.23B parameters (15.3% of the total) for 1,000 steps with a relatively high learning rate.

With the semantic ID token embeddings initialized, we proceed to the next phase of full finetuning on all 8.3B parameters. For this, we used a lower learning rate, a 3% warmup schedule, and the 8-bit AdamW optimizer for memory efficiency, training for three epochs on the full dataset. To monitor progress, we used several custom callbacks that tracked training metrics like gradient norms, measured hierarchical accuracy on recommendation tasks, and sampled model outputs on a fixed set of conversational inputs.

## Chatting with the LLM-recommender hybrid

The finetuned model can then be used to recommend items, and is conversant in semantic IDs and natural language. Because semantic IDs are a native part of its vocabulary, the model generates recommendations by simply predicting the next ID in a sequence, token-by-token. For interactive sessions, a chat interface translates the model’s raw output (that contains semantic IDs) back into product titles, creating a human-readable conversation.

> **User:** “Halo 3 Limited Edition -Xbox 360”, “Lost Planet: Extreme Condition - Xbox 360”, “Lost Planet: Extreme Condition Collector’s Edition -Xbox 360” `<|rec|>`
>
> > *Model takes the user’s interaction history (a sequence of Xbox 360 sci-fi shooters) and performs next-item prediction.*
> >
> > ```text
> > <|sid_start|><|sid_64|><|sid_351|><|sid_733|><|sid_768|><|sid_end|>
> > ```
>
> **Assistant:** “Mass Effect - Xbox 360”

Basic tests show the model can provide standard next-item recommendations based on a user’s history and refine recs based on natural language like platform constraints (*“Xbox games similar to…“*) or genre (*“I like animal and cute games.”*).

> **User:** Xbox games similar to “The Legend of Zelda: Ocarina of Time 3D”: `<|rec|>`
>
> > *Model takes a seed item (“The Legend of Zelda”) and a natural language constraint (“Xbox”) to steer recommendations to a different platform.*
> >
> > ```text
> > <|sid_start|><|sid_64|><|sid_271|><|sid_576|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_64|><|sid_435|><|sid_615|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_167|><|sid_271|><|sid_578|><|sid_768|><|sid_end|>
> > ```
>
> **Assistant:** “Fallout: New Vegas - Xbox 360 Ultimate Edition”, “Halo 4 - Xbox 360 (Standard Game)”, “Halo Reach - Legendary Edition”

The model can also explain its recommendations, describe the relationship between two items, and even suggest creative names and descriptions for product bundles.

> **User:** > I just finished “Dragon Quest Heroes II Explorer’s Edition - PlayStation 4”. Suggest another `<|rec|>` and explain why:
>
> > *Model performs a hybrid task where it generates a relevant semantic ID and then provides an explanation for the recommendation.*
> >
> > ```text
> > <|sid_start|><|sid_139|><|sid_289|><|sid_534|><|sid_768|><|sid_end|>
> > 
> > If you liked Dragon Quest Heroes II, you might like Nights of Azure 
> > because both are action RPGs for the PlayStation 4 with a focus on 
> > combat and character progression. Both games offer a narrative-
> > driven experience with a strong emphasis on combat mechanics, 
> > suggesting a shared player interest in this genre.
> > ```
>
> **Assistant:** “Nights of Azure - PlayStation 4”
>
> If you liked Dragon Quest Heroes II, you might like Nights of Azure because both are action RPGs for the PlayStation 4 with a focus on combat and character progression. Both games offer a narrative-driven experience with a strong emphasis on combat mechanics, suggesting a shared player interest in this genre.

Finally, while it may not match the raw accuracy of a specialized recommender, this unified interface allows users to discover products from our catalog through multi-turn dialogue, leading to the convergence of search, recsys, and chat.

And this is all done via the model itself, without intent classifiers, routers, retrieval, etc.

> **\[TURN 1\] User:** I’m looking for games similar to Mario Kart. `<|rec|>`
>
> > *Model generates semantic IDs for three kart-style racing games.*:
> >
> > ```text
> > <|sid_start|><|sid_131|><|sid_503|><|sid_738|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_131|><|sid_260|><|sid_577|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_145|><|sid_416|><|sid_671|><|sid_768|><|sid_end|>
> > ```
>
> **Assistant:** “Need for Speed Most Wanted - Sony PSP”, “Need for Speed Hot Pursuit 2 - PlayStation 2”, “Burnout Legends - Nintendo DS”
>
> **\[TURN 2\] User:** How about something similar but for Xbox? `<|rec|>`
>
> > *Model uses the conversation history and the new “Xbox” constraint to generate semantic IDs for relevant Xbox arcade-racers.*
> >
> > ```text
> > <|sid_start|><|sid_183|><|sid_461|><|sid_517|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_183|><|sid_461|><|sid_562|><|sid_768|><|sid_end|>, 
> > <|sid_start|><|sid_183|><|sid_461|><|sid_616|><|sid_768|><|sid_end|>
> > ```
>
> **Assistant:** “Need for Speed Carbon - Xbox 360”, “Project Gotham Racing 3 - Xbox 360”, “Forza Motorsport 4 - Xbox 360”
>
> **\[TURN 3\] User:** Suggest a name and description for the bundle.
>
> > *Model understands the context has shifted from recommendation to a creative text generation task.*
>
> **Assistant:**: Xbox Racing Legends: Speed & Style Pack

Okay, that’s it! I hope you found this as exciting as I do! If I had more compute, I’d experiment with sentence piece tokenization on the semantic IDs, which [Deepmind and Youtube have found helpful](https://arxiv.org/abs/2306.08121), and further explore the impact of semantic IDs on cold-start. I’d also train RQ-VAEs and language models on datasets with millions of products and a greater variety of tasks and conversations to test how this approach scales and generalizes.

Have you trained models or built UXes with semantic IDs? I’d love to chat, please [reach out](https://x.com/eugeneyan)!

Thanks to [Devansh](https://x.com/devanshtandon_) and [Shrikar](https://x.com/shrikar84) for the discussions, and [Luke](https://x.com/LukePiette) and [Runpod](https://www.runpod.io/?ref=4uddqig9) for the compute to run these experiments. (Please use my [affiliate code](https://runpod.io?ref=4uddqig9) so I get more credit for experiments!) Thanks to [Vibhu](https://x.com/vibhuuuus), [Shawn](https://x.com/swyx), [Cheng](https://x.com/_chenglou), and everyone who had to tolerate my yapping on this.

## References

Hou, Yupeng, Jiacheng Li, Zhankui He, An Yan, Xiusi Chen, and Julian McAuley. “Bridging Language and Items for Retrieval and Recommendation.” arXiv:2403.03952. Preprint, arXiv, March 6, 2024. https://doi.org/10.48550/arXiv.2403.03952.

Amazon. “Amazon KDD Cup ‘23 - Multilingual Recommendation Challenge Dataset.” AIcrowd, 2023. https://www.aicrowd.com/challenges/amazon-kdd-cup-23-multilingual-recommendation-challenge.

Rajput, Shashank, Nikhil Mehta, Anima Singh, et al. “Recommender Systems with Generative Retrieval.” arXiv:2305.05065. Preprint, arXiv, November 3, 2023. https://doi.org/10.48550/arXiv.2305.05065.

Singh, Anima, Trung Vu, Nikhil Mehta, et al. “Better Generalization with Semantic IDs: A Case Study in Ranking for Recommendations.” arXiv:2306.08121. Preprint, arXiv, May 30, 2024. https://doi.org/10.48550/arXiv.2306.08121.

Zeghidour, Neil, Alejandro Luebs, Ahmed Omran, Jan Skoglund, and Marco Tagliasacchi. “SoundStream: An End-to-End Neural Audio Codec.” arXiv:2107.03312. Preprint, arXiv, July 7, 2021. https://doi.org/10.48550/arXiv.2107.03312.

Lee, Doyup, Chiheon Kim, Saehoon Kim, Minsu Cho, and Wook-Shin Han. “Autoregressive Image Generation Using Residual Quantization.” arXiv:2203.01941. Preprint, arXiv, March 9, 2022. https://doi.org/10.48550/arXiv.2203.01941.

Kang, Wang-Cheng, and Julian McAuley. “Self-Attentive Sequential Recommendation.” arXiv:1808.09781. Preprint, arXiv, August 20, 2018. https://doi.org/10.48550/arXiv.1808.09781.

Vaswani, Ashish, Noam Shazeer, Niki Parmar, et al. “Attention Is All You Need.” arXiv:1706.03762. Preprint, arXiv, August 2, 2023. https://doi.org/10.48550/arXiv.1706.03762.

Zhang, Yanzhao, Mingxin Li, Dingkun Long, et al. “Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models.” arXiv:2506.05176. Preprint, arXiv, June 11, 2025. https://doi.org/10.48550/arXiv.2506.05176.

Yang, An, Anfeng Li, Baosong Yang, et al. “Qwen3 Technical Report.” arXiv:2505.09388. Preprint, arXiv, May 14, 2025. https://doi.org/10.48550/arXiv.2505.09388.

Comanici, Gheorghe, Eric Bieber, Mike Schaekermann, et al. “Gemini 2.5: Pushing the Frontier with Advanced Reasoning, Multimodality, Long Context, and Next Generation Agentic Capabilities.” arXiv:2507.06261. Preprint, arXiv, July 22, 2025. https://doi.org/10.48550/arXiv.2507.06261.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Sep 2025). Training an LLM-RecSys Hybrid for Steerable Recs with Semantic IDs. eugeneyan.com. https://eugeneyan.com/writing/semantic-ids/.

or

```
@article{yan2025semantic-ids,
  title   = {Training an LLM-RecSys Hybrid for Steerable Recs with Semantic IDs},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2025},
  month   = {Sep},
  url     = {https://eugeneyan.com/writing/semantic-ids/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
