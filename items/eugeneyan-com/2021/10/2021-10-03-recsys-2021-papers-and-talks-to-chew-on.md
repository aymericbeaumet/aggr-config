---
title: RecSys 2021 - Papers and Talks to Chew on
link: https://eugeneyan.com//writing/recsys2021/
source: eugeneyan-com
published: 2021-10-03T00:00:00Z
updated: 2021-10-03T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- deeplearning
- production
- recsys
- survey
summary: Simple baselines, ideas, tech stacks, and packages to try.
content: extracted
html: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.html
preview:
  file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.preview-038030c635fb.webp
  width: 256
  height: 134
  color: '#b6b4c3'
images:
- source: https://eugeneyan.com/assets/og_image/recsys-2021.jpg
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-bdd5fc28c769.jpg
    width: 1200
    height: 630
  color: '#fefefe'
- source: https://eugeneyan.com/assets/negative-interactions.webp
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-7d665ba0dc5d.webp
    width: 800
    height: 428
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/ydnabb.webp
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-a70d5c7540a8.webp
    width: 800
    height: 456
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/fashion-compatibility.webp
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-fa649b500d1e.webp
    width: 800
    height: 352
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/shared-item-representations.webp
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-cf175fa23833.webp
    width: 800
    height: 287
  color: '#fafdfc'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2021-10-03-recsys-2021-papers-and-talks-to-chew-on.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

RecSys 2021 happened this week (27 Sept - 1 Oct). Here are some papers I found interesting.

[**Negative Interactions for Improved Collaborative-Filtering: Don’t go Deeper, go Higher**](https://dl.acm.org/doi/pdf/10.1145/3460231.3474273) was motivated by the finding that modeling higher-order interactions helps with recommendation accuracy. They shared a simple extension of adding higher-order interactions to a linear model without a hidden layer (Embarrassingly Shallow AutoEncoders aka EASE^R). EASE^R learns pairwise relationships between each item *i* (input) and item *j* (output) of the autoencoder.

To add higher-order interactions as input, two items are now considered as input (*i* and *k*) to predict item *j* in the output. (They also tried even higher-order interactions but didn’t see any improvements.) This simple extension on a simple model was competitive with several SOTA deep learning models on datasets such as MovieLens-20M, Netflix, and MSD.

![Learning higher-order interactions with original pairwise interactions](https://eugeneyan.com/assets/negative-interactions.webp "Learning higher-order interactions with original pairwise interactions")

Learning higher-order interactions with original pairwise interactions ([source](https://dl.acm.org/doi/pdf/10.1145/3460231.3474273))

In addition, the paper showed that less active users benefited more from the higher-order model relative to EASE^R. They hypothesized that, because triplet-relations (*i*, *k*, *j*) are more prevalent among highly-active users, the pairwise relationships are freed up to better adapt to less-active users. *Loved the simplicity of this idea and implementation.*

[**Reenvisioning the comparison between Neural Collaborative Filtering and Matrix Factorization**](https://dl.acm.org/doi/pdf/10.1145/3460231.3475944) revisits the comparison between matrix factorization (MF) and neural collaborative filtering (NCF) again.

To recap, MF learns a latent representation of items and users and combines these representations to compute a preference score between each user and item (e.g., dot product). In comparison, NCF uses multilayer perceptrons (or other deep learning layers) to learn scores between each user and item.

The current paper reproduces the results from a [RecSys 2020 paper that compared MF and NCF,](https://dl.acm.org/doi/10.1145/3383313.3412488) and extends it by including other accuracy metrics, as well as metrics for diversity and novelty. It showed that MF outperforms NCF in performance (nDCG and Hit Rate), including in the long tail, though NCF provides more diversity and novelty. The paper also includes a useful list of various recommendation baselines. *Takeaway: Don’t throw your MF techniques out yet.*

[**You Do Not Need a Bigger Boat: Recommendations at Reasonable Scale in a (Mostly) Serverless and Open Stack**](https://dl.acm.org/doi/pdf/10.1145/3460231.3474604) shares a few principles and a suggested design for deploying recommenders using cloud services and open-source packages.

![Suggested design and tech stack for training and serving a recommender system](https://eugeneyan.com/assets/ydnabb.webp "Suggested design and tech stack for training and serving a recommender system")

Suggested design and tech stack for training and serving a recommender system ([source](https://dl.acm.org/doi/pdf/10.1145/3460231.3474604))

Principles include focusing on data quality (which leads to bigger gains relative to model improvements), using managed services instead of maintaining and scaling infrastructure, and reduced dependence on distributed computing (e.g., Spark) which can be slow and hard to debug. They also provide an [open-sourced implementation of a tech stack](https://github.com/jacopotagliabue/you-dont-need-a-bigger-boat) that goes from data ingestion (AWS Lambda) to recommendation serving (AWS SageMaker). *Batteries (read: [open dataset with 30 million rows](https://arxiv.org/abs/2104.09423)) included.*

[**Transformers4Rec: Bridging the Gap between NLP and Sequential / Session-Based Recommendation**](https://dl.acm.org/doi/pdf/10.1145/3460231.3474255) introduces [Transformers4Rec](https://github.com/NVIDIA-Merlin/Transformers4Rec), an open-source library built on HuggingFace’s Transformers.

It applies the Transformer architecture (and variants such as GPT-2, BERT, XLNet) to sequential and session-based recommendations. The paper includes results from several experiments, such as using different training regimes (casual language modeling (LM), permutation LM, mask LM) and different ways to integrate side information. *Experimenting with session-based recommenders? Try this library out.*

[**RecSysOps: Best Practices for Operating a Large-Scale Recommender System**](https://dl.acm.org/doi/pdf/10.1145/3460231.3474620) shared a set of best practices for identifying, diagnosing, and resolving issues in large-scale recommendation systems (RecSysOps). Best practices are divided into four categories:

- Detection: implementing know best practices, monitoring the system end-to-end, understanding why users engage with low ranked items
- Prediction: predicting items that will have cold-start before launch date (e.g., new shows or movies that are added to catalog)
- Diagnosis: logging, issue reproducibility, distinguishing between input data issue (e.g., incorrect language) and model issue (e.g., missing values handled incorrectly)
- Resolution: having a playbook of hotfixes, considering and handling issues (e.g., corrupted data, timeouts) into the system to make it more robust

[**Semi-Supervised Visual Representation Learning for Fashion Compatibility**](https://dl.acm.org/doi/pdf/10.1145/3460231.3474233) shares about how they overcame the constraints of limited labeled data for fashion compatibility prediction (e.g., an outfit consisting of dress, jacket, shoes). Their model is a siamese network with a ResNet18 backbone trained on labeled triplets of anchor item, compatible item, and non-compatible item images.

To augment their data, they adopt a semi-supervised learning approach. During training, pseudo positive outfits were generated by replacing compatible items with a nearest neighbor item to get pseudo compatible outfits. Pseudo non-compatible outfits are generated similarly via replacing items.

![Creating pseudo-labels (middle) and applying shape and color transformations (right)](https://eugeneyan.com/assets/fashion-compatibility.webp "Creating pseudo-labels (middle) and applying shape and color transformations (right)")

Creating pseudo-labels (middle) and applying shape and color transformations (right) ([source](https://dl.acm.org/doi/pdf/10.1145/3460231.3474233))

They also observed that compatible items have color and texture similarity, but not shape similarity. Thus, they applied self-supervised consistency regularization where shape and color perturbed images are used as positive and negative labels respectively.

[**Shared Neural Item Representations for Completely Cold Start Problem**](https://dl.acm.org/doi/pdf/10.1145/3460231.3474228) shared their findings that using user interaction vectors as input achieves better results in fewer iterations relative to using customer ID as input. (I had this intuition and it’s great to see experiment results on this.) Thus, they use item embeddings to represent users.

![Unifying item representations across user and item towers](https://eugeneyan.com/assets/shared-item-representations.webp "Unifying item representations across user and item towers")

Unifying item representations across user and item towers ([source](https://dl.acm.org/doi/pdf/10.1145/3460231.3474228))

With this approach, two sets of item embeddings are learned—item embeddings to represent the user, and item embeddings to represent items. To simplify and improve learning, they unify the item embeddings by using item embedding learned via the item tower to also represent users. They also include side information when learning item embeddings to handle item cold-start.

What papers did you enjoy? [Reach out](https://twitter.com/eugeneyan) and let me know!

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Oct 2021). RecSys 2021 - Papers and Talks to Chew on. eugeneyan.com. https://eugeneyan.com/writing/recsys2021/.

or

```
@article{yan2021recsys,
  title   = {RecSys 2021 - Papers and Talks to Chew on},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2021},
  month   = {Oct},
  url     = {https://eugeneyan.com/writing/recsys2021/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
