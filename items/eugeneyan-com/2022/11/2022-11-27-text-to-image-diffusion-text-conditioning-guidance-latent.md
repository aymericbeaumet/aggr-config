---
title: 'Text-to-Image: Diffusion, Text Conditioning, Guidance, Latent Space'
link: https://eugeneyan.com//writing/text-to-image/
source: eugeneyan-com
published: 2022-11-27T00:00:00Z
updated: 2022-11-27T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- deeplearning
- llm
- survey
summary: The fundamentals of text-to-image generation, relevant papers, and experimenting with DDPM.
content: extracted
html: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.html
preview:
  file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.preview-3aeba3ea9b6d.webp
  width: 256
  height: 134
  alt: 'OG image prompt: ''a robot holding a paint brush painting on an art stand'''
  color: '#838387'
images:
- source: https://eugeneyan.com/assets/og_image/robot-painting.jpg
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-cada627f2f74.jpg
    width: 1200
    height: 630
  color: '#59839a'
- source: https://eugeneyan.com/assets/diffusion.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-429ed0797923.webp
    width: 800
    height: 577
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-dffa7ddf2aa3.webp
    width: 320
    height: 231
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/ddpm.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-4d33957ae18e.webp
    width: 800
    height: 152
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/ddpm-training.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-aca83e44a515.webp
    width: 800
    height: 404
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-2fe8b4cd03d4.webp
    width: 320
    height: 162
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/ddpm-sampling.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-71e6c312b28c.webp
    width: 800
    height: 391
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-85c66d918bfd.webp
    width: 320
    height: 156
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/clip.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-ca770982e223.webp
    width: 1200
    height: 576
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-345852ea9471.webp
    width: 320
    height: 154
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/dalle-avocado.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-1fe8f034feab.webp
    width: 800
    height: 217
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/vqgan-codebook.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-b248fec65bcb.webp
    width: 1200
    height: 563
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-873e8ef09e90.webp
    width: 320
    height: 150
  color: '#f9fafa'
- source: https://eugeneyan.com/assets/dalle-sequence.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-85fc5c3ad010.webp
    width: 1000
    height: 372
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-09b46ef7b9fa.webp
    width: 320
    height: 119
  color: '#f8f8f8'
- source: https://eugeneyan.com/assets/unclip.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-d8246524b163.webp
    width: 1200
    height: 625
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-661736a665a8.webp
    width: 320
    height: 167
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/imagen-curves.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-4609a62ea305.webp
    width: 1200
    height: 320
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-d3c76c1e4ca9.webp
    width: 320
    height: 85
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/imagen-text-embedding.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-bbfdd47a8256.webp
    width: 1200
    height: 689
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/imagen-conditioning.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-bdda4d09d0cf.webp
    width: 1000
    height: 563
  color: '#fefefe'
- source: https://eugeneyan.com/assets/imagen-high-level.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-82125451e409.webp
    width: 1200
    height: 682
  color: '#fefefe'
- source: https://eugeneyan.com/assets/classifier-guidance.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-cc45d0f4c392.webp
    width: 1200
    height: 469
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-cae20fc8f042.webp
    width: 320
    height: 125
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/glide-guidance.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-1e9fdac903cf.webp
    width: 1200
    height: 414
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-e24e44e81197.webp
    width: 320
    height: 110
  color: '#373a36'
- source: https://eugeneyan.com/assets/stable-diffusion.webp
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-4f4aafc31989.webp
    width: 1200
    height: 585
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2022-11-27-text-to-image-diffusion-text-conditioning-guidance-latent.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Text-to-image has advanced at a breathless pace in 2021 - 2022, starting with DALL·E, then DALL·E 2, Imagen, and now Stable Diffusion. I dug into a couple of papers to learn more about the space and organized my understanding into a few key concepts:

- [Diffusion](https://eugeneyan.com//writing/text-to-image/#diffusion-from-data-to-noise-and-back): Gradually add noise to data and then learn to generate data from noise
- [Text conditioning](https://eugeneyan.com//writing/text-to-image/#text-conditioning-influencing-image-output-via-text): Generating images given (i.e., conditioned on) a text prompt
- [Classifier guidance](https://eugeneyan.com//writing/text-to-image/#classifier-guidance-increasing-the-strength-of-prompts): Using classifier gradients to text-increase image alignment
- [Latent space](https://eugeneyan.com//writing/text-to-image/#latent-space-diffusion-on-latents-instead-of-pixels): Applying diffusion on image embeddings instead of image pixels

![OG image prompt: 'a robot holding a paint brush painting on an art stand'](https://eugeneyan.com/assets/og_image/robot-painting.jpg "OG image prompt: 'a robot holding a paint brush painting on an art stand'")

OG image prompt: "a robot holding a paint brush painting on an art stand"

## Diffusion: From data to noise and back

Let’s start with the earliest diffusion paper I know, cryptically titled **“[Deep Unsupervised Learning using Nonequilibrium Thermodynamics](https://arxiv.org/abs/1503.03585)**, by Sohl-Dickstein in 2015. In it, the authors explained that the idea of diffusion was inspired by non-equilibrium statistical physics (perhaps the [particle physics concept](https://en.wikipedia.org/wiki/Diffusion) with the same name?)

The key idea is to gradually destroy structure in a data distribution (e.g., image) via a forward diffusion process, and then learn a reverse diffusion process (via a model) to restore the structure in the data. And once we have a trained model, we can generate images by starting from pure noise and applying reverse diffusion (aka sampling).

To implement forward diffusion, they apply a Markov chain that progressively adds Gaussian noise to the data until the signal is destroyed (i.e., complete noise).

For reverse diffusion, they train a **diffusion probabilistic model (DPM)** to transform noised images to less noisy images. Reverse diffusion is done via many small denoising steps, instead of a single large step from pure noise to clean image—the intuition is that taking many small steps is more tractable than a single large step.

They shared an example of diffusion on 2d Swiss roll data. Forward diffusion (top in blue, left to right) gradually adds Gaussian noise until the data is pure noise. Reverse diffusion (middle in red, *right to left*) gradually denoises the data to get back the 2d swiss roll.

![Image](https://eugeneyan.com/assets/diffusion.webp "Image")

Forward (blue) and backward (red) diffusion process ([source](https://arxiv.org/abs/1503.03585))

**[Denoising Diffusion Probabilistic Models (DDPM; 2020)](https://arxiv.org/abs/2006.11239)** shares a gentler explanation of the diffusion process: Forward diffusion (\\(q\\)) is a predefined process that gradually adds Gaussian noise to the image until it’s pure noise. Reverse diffusion (\\(p\\)) is a learned process that gradually denoises an image starting from pure noise until we get an actual image.

![Forward (q) and backward (p) diffusion process](https://eugeneyan.com/assets/ddpm.webp "Forward (q) and backward (p) diffusion process")

Forward (q) and backward (p) diffusion process ([source](https://arxiv.org/abs/2006.11239))

If we knew the conditional distribution of \\(p(x\_{t-1}\\vert x\_{t})\\), we could just run the forward diffusion process in reverse: Sample random Gaussian noise (\\(x\_{t}\\)) and denoise it to get a sample image from the real distribution.

Unfortunately, we don’t know \\(p(x\_{t-1}\\vert x\_{t})\\) as it’s intractable—it requires knowing the distribution of *all possible images* to compute the conditional probability. Thus, we train a model (i.e., neural network) to learn the conditional probability distribution \\(p\_{\\theta}(x\_{t-1}\\vert x\_{t})\\), where \\(\\theta\\) are the model parameters.

Given that the forward diffusion process is Gaussian (since the added noise is Gaussian), perhaps we can assume the reverse process to be Gaussian too. If so, reverse diffusion can be represented via mean (\\(\\mu\_{\\theta}\\)) and variance (\\(\\Sigma\_{\\theta}\\)) and be parameterized as:

\\\[p\_{\\theta}(x\_{t-1}\\vert x\_{t}) := \\mathcal{N} (x\_{t-1}; \\mu\_{\\theta}(x\_{t}, t), \\Sigma\_{\\theta} (x\_{t}, t))\\\]

In layman’s terms, the probability of the less noisy image (\\(x\_{t-1}\\)) given a noisier image (\\(x\_{t}\\)) from the previous timestep (\\(t\\)) is drawn from a Gaussian distribution (\\(\\mathcal{N}\\)) where the mean is the mean of \\(x\_{t}\\) at timestep \\(t\\) and the variance is the variance of \\(x\_{t}\\) at timestep \\(t\\). Thus, the neural network needs to learn the mean (\\(\\mu\_{\\theta}\\)) and variance (\\(\\Sigma\_{\\theta}\\)). That said, in DDPM, the variance is predefined and the network only has to learn the mean.

![Training algorithm for DDPM](https://eugeneyan.com/assets/ddpm-training.webp "Training algorithm for DDPM")

Training algorithm for DDPM ([source](https://arxiv.org/abs/2006.11239))

Here’s a step-by-step of the training algorithm:

- Line 1: Start while loop
- Line 2: Sample a random clean image (\\(x\_{0}\\)) from the set of images
- Line 3: Sample a noise level (\\(t\\)) uniformly from 1 to max \\(T\\)
- Line 4: Sample some noise (\\(\\epsilon\\)) from a Gaussian and corrupt image with the noise
- Line 5: Train the neural network to predict the noise based on the corrupted image
- Line 6: End while loop when model converges

![Sampling algorithm for DDPM](https://eugeneyan.com/assets/ddpm-sampling.webp "Sampling algorithm for DDPM")

Sampling algorithm for DDPM ([source](https://arxiv.org/abs/2006.11239))

When we have a trained model, here’s how to generate new images starting from noise:

- Line 1: Get a sample noise image (\\(x\_{T}\\)) from a Gaussian distribution
- Line 2: Iterate from timestep \\(T\\) to timestep 1
- Line 3: Sample additional Gaussian noise (\\(z\\) if timestep > 1 else zero, because we just return the clean image at timestep 1)
- Line 4: Get the slightly denoised image (\\(x\_{t-1}\\)) by subtracting the noise (\\(\\epsilon\_{\\theta}(x\_{t}, t)\\)) from the noisy image (\\(x\_{t}\\)), and then adding back some noise (\\(z\\))
- Line 5: End iteration
- Line 6: Return clean image from timestep 1

Notice the model predicts all the noise from a noisy image (\\(\\epsilon\_{\\theta}(x\_{t}, t)\\)). However, we only subtract a fraction of it, weighted by \\(\\frac{1-\\sigma\_{t}}{\\sqrt{}1-\\bar{\\sigma\_{t}}}\\), and then add back noise (\\(z\\)) weighted by \\(\\sigma\_{t}\\).

I was curious about these noise removal and addition weights and coded up a [DDPM](https://github.com/eugeneyan/text-to-image/blob/main/the-annotated-diffusion-fashion-mnist-more-epochs.ipynb) to tinker with it. To my surprise, the noise removal weights are as low as 0.01 to 0.02 while the noise addition weights go as high as 0.14. Remember the intuition that estimating small amounts of noise for multiple timesteps is more tractable than estimating all noise via a single timestep? This is how it’s implemented in the algorithm and code.

```bash
t = 799 - Latent weight: 1.010, Removed noise weight: 0.020, Added noise weight: 0.141
t = 640 - Latent weight: 1.008, Removed noise weight: 0.016, Added noise weight: 0.127
t = 480 - Latent weight: 1.006, Removed noise weight: 0.012, Added noise weight: 0.110
t = 320 - Latent weight: 1.004, Removed noise weight: 0.009, Added noise weight: 0.090
t = 160 - Latent weight: 1.002, Removed noise weight: 0.008, Added noise weight: 0.064
t = 0   - Latent weight: 1.000, Removed noise weight: 0.010, Added noise weight: 0.000
```

Weights for the latent, noise removal, and noise addition at t = 799, 640, 480, 320, 160, 0 ([source](https://github.com/eugeneyan/text-to-image))

Via experimenting with the DDPM, I learned that more timesteps had a positive impact on sample quality though it also required more epochs and a large timestep embedding. Larger dimensions for the timestep embedding also improved loss and sample quality. On the other hand, more epochs, different loss functions, and batch size didn’t help. (Want to understand diffusion better? Clone this [GitHub repo](https://github.com/eugeneyan/text-to-image.git) and play with the code!)

So far, these models are *only capable of basic diffusion* (i.e., generating images from noise, limited by the dataset they’re trained on). They are unable to generate images from text prompts (unlike DALL·E and Stable Diffusion)—to do this, *we need text conditioning.*

(Aside: I thought the approach of corrupting input with noise and learning to remove the noise was similar to denoising autoencoders and wrote a [brief comparison](https://eugeneyan.com/writing/autoencoders-vs-diffusers/).)

## Text conditioning: Influencing image output via text

To understand text conditioning, I think it helps to start with **[Contrastive Language-Image Pre-training (CLIP; 2021)](https://arxiv.org/abs/2103.00020)**. It embeds text and image in the same space via a projection layer. Thus, it can efficiently learn visual concepts, in the form of text, via natural language supervision and perform zero-shot classification.

![CLIP pre-training and zero-shot classification](https://eugeneyan.com/assets/clip.webp "CLIP pre-training and zero-shot classification")

CLIP pre-training and zero-shot classification ([source](https://arxiv.org/abs/2103.00020))

In the pre-training stage, the image and text encoders are trained to predict which images are paired with which texts in a dataset of 400M image-caption pairs. CLIP is trained to maximize the cosine similarity of the image and text embeddings of image-caption pairs via a multi-modal embedding space.

This is implemented via a linear projection to map each encoder’s representation to the multi-modal embedding space (lines 13 - 15 below). As a result, the text and image embeddings are now in the same space. Thus, given a text embedding, we can apply k-nearest neighbors to find similar images.

```python
# image_encoder - ResNet or Vision Transformer
# text_encoder  - CBOW or Text Transformer
# I[n, h, w, c] - minibatch of aligned images
# T[n, l]       - minibatch of aligned texts
# W_i[d_i, d_e] - learned projection of image to embed
# W_t[d_t, d_e] - learned projection of text to embed
# t             - learned temperature parameter

# extract feature representations of each modality
I_f = image_encoder(I) #[n, d_i]
T_f = text_encoder(T)  #[n, d_t]

# joint multimodal embedding [n, d_e]
I_e = l2_normalize(np.dot(I_f, W_i), axis=1)
T_e = l2_normalize(np.dot(T_f, W_t), axis=1)

# scaled pairwise cosine similarities [n, n]
logits = np.dot(I_e, T_e.T) * np.exp(t)

# symmetric loss function
labels = np.arange(n)
loss_i = cross_entropy_loss(logits, labels, axis=0)
loss_t = cross_entropy_loss(logits, labels, axis=1)
loss   = (loss_i + loss_t) / 2
```

Pseudo-code to embed images and text via a multi-modal embedding ([source](https://arxiv.org/abs/2103.00020))

For zero-shot classification, all the dataset classes are converted into captions such as “a photo of a \<class>”. Then, CLIP is used to predict the class that best matches the image.

CLIP was quickly followed up by **[DALL·E (2021)](https://arxiv.org/abs/2102.12092), one of the first text-to-image generation models open to the public**—this is our first example of text conditioning.

![Remember the avocado chair from DALL·E?](https://eugeneyan.com/assets/dalle-avocado.webp "Remember the avocado chair from DALL·E?")

Remember the avocado chair from DALL·E? ([source](https://openai.com/blog/dall-e/))

At a high level, DALL·E starts by compressing images into 8,192 discrete tokens in a visual codebook (\\(\\mathcal{Z}\\) in the image below). These image tokens can then be concatenated with text embeddings. The combined embedding is then fed into a transformer which learns how to predict the image tokens given a text embedding.

![Visual example of a codebook from the VQGAN paper](https://eugeneyan.com/assets/vqgan-codebook.webp "Visual example of a codebook from the VQGAN paper")

Example of a visual codebook from the VQGAN paper (Note, this is not DALL·E; [source](https://arxiv.org/abs/2012.09841))

Why compress images into tokens in a codebook? The authors explained that using pixels directly as image tokens would require too much memory for high-resolution images. As a result, model capacity is spent on high-frequency details (i.e., pixels) instead of low-frequency structure (i.e., lines) that make images visually recognizable. (This is the same reason Stable diffusion encodes images into the latent space before running diffusion.)

First, DALL·E trains a discrete variational encoder (dVAE) to compress 256 x 256 images into 32 x 32 image tokens (vocabulary size = 8,192). The parameters of the dVAE are then frozen when training the transformer.

Next, image captions are lowercased and truncated to a max length of 256 tokens before being encoded (vocabulary size = 16,384). The image tokens are then concatenated after the text tokens (example below).

![Example of concatenated text and image tokens in DALL·E](https://eugeneyan.com/assets/dalle-sequence.webp "Example of concatenated text and image tokens in DALL·E")

Example of concatenated text and image tokens in DALL·E ([source](https://arxiv.org/abs/2102.12092))

Finally, an autoregressive transformer (i.e., predict the next item in a sequence) is trained to learn the joint distribution over the text and image tokens. The transformer is decoder-only, where each image token can attend to all text tokens earlier in the sequence.

To generate images from text, the text prompt is embedded and fed into the transformer. The transformer then generates the sequence of image tokens. Finally, the dVAE decodes the image tokens to return a 256 x 256 image.

**[DALL·E 2 (aka unCLIP, 2022)](https://arxiv.org/abs/2204.06125) builds on the previous two papers** by using the text and image encoder from CLIP and the autoregressive transformer from DALL·E. Similarly, unCLIP is trained on a dataset of image-caption pairs which are embedded via CLIP text and image encoders into text embeddings (\\(z\_{t}\\)) and image embeddings (\\(z\_{i}\\)).

![How the encoded text (blue) generates images via the prior and decoder](https://eugeneyan.com/assets/unclip.webp "How the encoded text (blue) generates images via the prior and decoder")

How the encoded text (blue) generates images via the prior and decoder ([source](https://arxiv.org/abs/2204.06125))

In the image above, the prior (\\(p(z\_{i}\\vert y)\\)) learns to produce CLIP image embeddings (\\(z\_{i}\\)) conditioned on the text prompt (\\(y\\)). The decoder (\\(p(x\\vert z\_{i}, y)\\)) then produces the image conditioned on the CLIP image embedding (\\(z\_{i}\\)) and optional text prompt (\\(y\\)). In other words, to generate images from text prompts (\\(p(x\\vert y)\\)), we first sample CLIP image embeddings via the prior before decoding them via the decoder.

\\\[p(x\\vert y) = P(x, z\_{i}\\vert y) = P(x\\vert z\_{i}, y)P(z\_{i}\\vert y)\\\]

The paper shared two approaches to learn the prior: autoregressive and diffusion.

The autoregressive approach is similar to that of DALL·E where text conditioning is done by having the text embedding early in the sequence. They also prepend a dot product token (of text and image embedding) between the text and image embedding. This allowed the autoregressive prior to condition the model on the higher dot product since a higher text-image dot product indicates images that are more representative of the caption.

For the diffusion approach, they trained a decoder-only transformer with a casual attention mask on a sequence of encoded text, text embedding, time step embedding, noised CLIP image embedding, and final embedding. The final embedding’s output is then used to predict the unnoised CLIP image embedding. Interestingly, in contrast to DDPM, they found it better to train the model to directly predict the unnoised image, instead of predicting the noise and then subtracting from the noisy image.

The latter shows one way text conditioning can be applied to diffusion. The transformer attends to the text information in the sequence and uses it to predict the final output.

**[Imagen (2022)](https://arxiv.org/abs/2205.11487) takes it further by using a text encoder that wasn’t even trained on image-caption pairs (🤯).** It uses the encoder network of the [T5](https://arxiv.org/abs/1910.10683). This is a departure from CLIP-based approaches, where the text encoder is specifically trained on image-caption pairs and the text embeddings are projected into a multi-modal embedding space.

It works because extremely large language models (LLMs), by virtue of sheer size, can still learn useful representations despite not being explicitly trained on text-to-image tasks. The benefit is that LLMs can learn on a text-only corpus which is easily larger than image-text datasets. Furthermore, they found that scaling the text encoder size is more impactful than UNet size in image-text alignment and image fidelity.

![Text encoder size > UNet size; dynamic thresholding > static thresholding](https://eugeneyan.com/assets/imagen-curves.webp "Text encoder size > UNet size; dynamic thresholding > static thresholding")

Text encoder size > UNet size; dynamic thresholding > static thresholding ([source](https://arxiv.org/abs/2205.11487))

Imagen does text conditioning by first tokenizing the input text and encoding it via the T5 encoder. The encoded text then passes through a pooling step (image below).

![Encoding text via the T5 transformer in Imagen](https://eugeneyan.com/assets/imagen-text-embedding.webp "Encoding text via the T5 transformer in Imagen")

Encoding text via the T5 transformer in Imagen ([source](https://www.assemblyai.com/blog/how-imagen-actually-works/))

The text embedding is then combined with the image and time step embedding (image below). The model is conditioned via cross-attention over the text embedding. This is implemented by concatenating the text embedding to the key-value pairs of each self-attention layer in the UNet. Cross-attention on the text embedding outperformed simple mean or attention-based pooling.

![Conditioning on time and text embeddings in Imagen](https://eugeneyan.com/assets/imagen-conditioning.webp "Conditioning on time and text embeddings in Imagen")

Conditioning on time and text embeddings in Imagen ([source](https://www.assemblyai.com/blog/how-imagen-actually-works/))

(Note how text is conditioned differently in DALL·E variants and Imagen. In DALL·E, text conditioning is done by concatenating the text embedding to the image embedding and then passing it through a transformer. In Imagen, text conditioning is done via cross-attention in the UNet.)

The text embedding (green and red boxes below) is used throughout the image generation step. First, it’s used to generate the initial 64 x 64 image from noise (blue box). Then, it is used to increase the image resolution to 256 x 256 and then 1,024 x 1,024 (yellow boxes).

![High-level overview of Imagen](https://eugeneyan.com/assets/imagen-high-level.webp "High-level overview of Imagen")

High-level overview of Imagen's text encoder, diffusion generator, and resolution model ([source](https://www.assemblyai.com/blog/how-imagen-actually-works/))

With text conditioning, we can now generate images based on text prompts. But text conditioning alone is insufficient to generate high-quality images that adhere to the text prompt—*we also need guidance.*

## Classifier guidance: Increasing the strength of prompts

Guidance is a technique to explicitly incorporate image class—or text prompt—directly in the diffusion process. (This is the often tweaked [`guidance_scale`](https://huggingface.co/docs/diffusers/api/pipelines/stable_diffusion#diffusers.StableDiffusionPipeline.__call__.guidance_scale) hyperpameter.)

**The [classifier-guidance paper (2021)](https://arxiv.org/abs/2105.05233) noted that GANs relied heavily on class labels**, often via class-conditioned normalization or discriminators with heads designed to behave like classifiers. This suggests that class information is crucial to the success of GANs for generation. So, to take a leaf from GANs, they use a classifier \\(p\_\\phi(y\\vert x)\\) to improve image generation via diffusion.

As a result, \\(\\nabla\_{x} \\log p\_\\phi(y\\vert x)\\) is added to the score function, updating it from \\(\\nabla\_{x} \\log p\_\\theta(x)\\) to \\(\\nabla\_{x} \\log p\_\\gamma (x\\vert y) = \\nabla\_{x} \\log p\_\\theta(x) + \\gamma \\nabla\_{x} \\log p\_\\phi(y\\vert x)\\), where \\(\\nabla\_x \\log p\_\\phi(y\\vert x)\\) is the gradient of the classifier and \\(\\gamma\\) is the guidance scale.

(Note: For simplicity, I’ve dropped the \\(t\\) variable in the equations. In practice, text-to-image diffusion is also conditioned on time step which indicates the level of input noise.)

To achieve this, they train a classifier (\\(p\_\\phi(y \\vert x)\\)) on noised images (\\(x\\)) and then use the gradient (\\(\\nabla\_{x} \\log p\_\\phi(y \\vert x)\\)) to guide the sampling process towards the class label (\\(y\\)). To produce noised images, forward diffusion is applied on ImageNet with random crops to reduce overfitting. The classifier adopts the downsampling truck of the UNet, with pooling on the 8 x 8 layer to produce the final output.

Recall that the score function is now \\(\\nabla\_x \\log p\_\\gamma (x\\vert y) = \\nabla\_x \\log p\_\\theta(x) + \\gamma \\nabla\_x \\log p\_\\phi(y \\vert x)\\) and \\(\\gamma\\) is the guidance scale. The paper found that increasing the guidance scale beyond 1 had the effect of amplifying the conditioning signal (i.e., text prompt) and led to better images.

![With standard (left) and stronger (right; scale = 10) classifier-guidance](https://eugeneyan.com/assets/classifier-guidance.webp "With standard (left; scale=1) and stronger (right; scale=10) classifier-guidance")

With standard (left; scale=1) and stronger (right; scale=10) classifier-guidance ([source](https://arxiv.org/abs/2105.05233))

When using a scale of 1, while the classifier assigned reasonable probabilities of ~50% to the classes of the sampled images, upon visual inspection, these images did not match the classes. Scaling up the classifier gradients (i.e., guidance > 1) solved this issue and the class probabilities of the classifier increased to nearly 100%.

However, *classifier guidance is impractical* as the classifier needs to be trained—from scratch—on noisy images. I.e., we can’t use a pre-trained classifier.

Thus, **[classifier-free guidance (2021)](https://arxiv.org/abs/2207.12598)** was proposed. Instead of training a separate classifier, **it trains a conditional diffusion model (\\(p(x \\vert y)\\)) with conditioning dropout.** Some proportion of the time, the conditioning information (i.e., image caption) is replaced with a null token. This is simple to implement and does not complicate the training pipeline or increase model parameters.

As a result, the single neural network can function as either a conditional model \\(p(x|y)\\) or unconditional model \\(p(x)\\), depending on the guidance scale:

- \\(\\gamma = 0\\): unconditional model
- \\(\\gamma = 1\\): standard conditional model
- \\(\\gamma > 1\\): strongly conditional model that adheres more to the conditional (i.e., text prompt) and returns images with better text-alignment

**[GLIDE (2021)](https://arxiv.org/abs/2112.10741) explored using both CLIP-guidance and classifier-free guidance.** CLIP guidance was implemented by replacing the classifier with a CLIP model trained on noised images. They found that users preferred images generated via classifier-free guidance.

![No guidance (left) vs. CLIP-guidance (middle) vs. classifier-free guidance (right) in GLIDE](https://eugeneyan.com/assets/glide-guidance.webp "No guidance (left) vs. CLIP-guidance (middle) vs. classifier-free guidance in GLIDE")

“A stained glass window of a panda eating bamboo”: No guidance (left) vs. CLIP-guidance (middle) vs. classifier-free guidance (right) in GLIDE ([source](https://arxiv.org/abs/2112.10741))

To be clear, DALL·E, DALL·E 2, and Imagen apply classifier-free guidance too. DALL·E randomly drops the text-conditioning 10% of the time while DALL·E 2 randomly sets the text embeddings to zero and randomly drops text captions 10% and 50% of the time. Imagen randomly sets text embeddings to zero for 10% of training instances. In the space of text-to-image diffusion, classifier-free guidance (aka conditioning dropout) is now *as essential as dropout for regularization*.

That said, it was another development that allowed regular users to hack text-to-image diffusion on their laptops—*departure to latent space*.

## Latent space: Diffusion on latents instead of pixels

**[Stable Diffusion (2021)](https://arxiv.org/abs/2112.10752) differs from the previous diffusion models by working in the latent space instead of pixel space.** It first compresses images via a variational autoencoder (VAE) into a more efficient and lower dimensional latent embedding. Next, the diffusion model learns to generate latent (i.e., compressed) representations of images which are then decoded into images via the VAE decoder.

Similar to DALL·E and its visual codebook, latent space is motivated by the observation that most pixels in an image are imperceptible details that are semantically meaningless. However, because regular diffusion models are trained and evaluated in the pixel space, it leads to unnecessary computation and thus costly training and inference. Thus, the paper proposes diffusion on compressed images where the imperceptible details are excluded.

![Using a VAE to encode images from pixel space to latent space (left)](https://eugeneyan.com/assets/stable-diffusion.webp "Using a VAE to encode images from pixel space to latent space (left)")

Using a VAE to encode images from pixel space to latent space (left) ([source](https://arxiv.org/abs/2112.10752))

In Stable Diffusion, the VAE encodes noised images (via \\(\\mathcal{E}\\)) into a low-dimensional latent representation which is fed into the UNet. It then decodes UNet-generated latent representations (via \\(\\mathcal{D}\\)) into human-understandable images. The VAE has a reduction factor of 8, where the original image pixel space of 3 x 512 x 512 is encoded into latent space of 6 x 64 x 64, thus requiring 1/8 x 1/8 = 1/64 of the memory. During sampling, only the VAE decoder is needed.

Stable Diffusion uses the CLIP text encoder. (But as Imagen has demonstrated, probably any sufficiently large text-only LLM can be used).

Latent diffusion leads to faster training and sampling because we’re now working in the latent—instead of pixel—space. This leads to lower cost which leads to more experiments. The lower memory requirement also allows sampling run on consumer-grade laptops, putting text-to-image generation in the hands of regular hackers.

We started with how diffusion can generate images from noise. Then, we saw how text conditioning enabled prompt-based image generation via autoregressive transformers and cross-attention, even using text encoders that weren’t trained on text-to-image tasks. Finally, we learned how guidance helps with generating images that are more aligned with the text prompt though at the cost of image diversity.

The space of text-to-image has progressed rapidly since I started deliberately studying it. Two weeks ago, NVIDIA released [eDiff-I](https://arxiv.org/abs/2211.01324) which uses an ensemble of diffusion models. And we haven’t touched on text-to-video (e.g., Facebook’s [Make-A-Video](https://arxiv.org/abs/2209.14792), Google’s [Imagen Video](https://arxiv.org/abs/2210.02303)) and text-to-3d (e.g., Google’s [DreamFusion](https://arxiv.org/abs/2209.14988), NVIDIA’s [Magic3D](https://arxiv.org/abs/2211.10440)).

Unfortunately, we can’t discuss all of them here (mostly because I don’t have the spare time and energy 😅). Nonetheless, I hope these fundamentals will help with your understanding of text-to-media generation and its future advances.

Did I misunderstand or misrepresent any of the concepts or papers? If so, please [reach out](https://twitter.com/eugeneyan)!

## References

- [Deep Unsupervised Learning using Nonequilibrium Thermodynamics](https://arxiv.org/abs/1503.03585)
- [Generative Modeling by Estimating Gradients of the Data Distribution (NCSN)](https://arxiv.org/abs/1907.05600)
- [Improved Techniques for Training Score-Based Generative Models](https://arxiv.org/abs/2006.09011)
- [Denoising Diffusion Probabilistic Models (DDPM)](https://arxiv.org/abs/2006.11239)
- [Denoising Diffusion Implicit Models (DDIM)](https://arxiv.org/abs/2010.02502)
- [Improved Denoising Diffusion Probabilistic Models](https://arxiv.org/abs/2102.09672)
- [Learning Transferable Visual Models From Natural Language Supervision (CLIP)](https://arxiv.org/abs/2103.00020)
- [Zero-Shot Text-to-Image Generation (DALL·E)](https://arxiv.org/abs/2102.12092)
- [Taming Transformers for High-Resolution Image Synthesis (VQGAN)](https://arxiv.org/abs/2012.09841)
- [Hierarchical Text-Conditional Image Generation with CLIP Latents (DALL·E 2)](https://arxiv.org/abs/2204.06125)
- [Photorealistic Text-to-Image Diffusion Models with Deep Language Understanding (Imagen)](https://arxiv.org/abs/2205.11487)
- [How Imagen Actually Works](https://www.assemblyai.com/blog/how-imagen-actually-works/)
- [Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer (T5)](https://arxiv.org/abs/1910.10683)
- [Diffusion Models Beat GANs on Image Synthesis (classifier-guidance)](https://arxiv.org/abs/2105.05233)
- [Classifier-Free Diffusion Guidance](https://arxiv.org/abs/2207.12598)
- [GLIDE: Towards Photorealistic Image Generation and Editing with Text-Guided Diffusion Models](https://arxiv.org/abs/2112.10741)
- [High-Resolution Image Synthesis with Latent Diffusion Models (Stable Diffusion)](https://arxiv.org/abs/2112.10752)

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Nov 2022). Text-to-Image: Diffusion, Text Conditioning, Guidance, Latent Space. eugeneyan.com. https://eugeneyan.com/writing/text-to-image/.

or

```
@article{yan2022diffusion,
  title   = {Text-to-Image: Diffusion, Text Conditioning, Guidance, Latent Space},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2022},
  month   = {Nov},
  url     = {https://eugeneyan.com/writing/text-to-image/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
