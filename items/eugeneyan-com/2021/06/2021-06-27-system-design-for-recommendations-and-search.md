---
title: System Design for Recommendations and Search
link: https://eugeneyan.com//writing/system-design-for-discovery/
source: eugeneyan-com
published: 2021-06-27T00:00:00Z
updated: 2021-06-27T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- engineering
- production
- recsys
- teardown
- 🔥
summary: Breaking it into offline vs. online environments, and candidate retrieval vs. ranking steps.
content: extracted
html: 2021-06-27-system-design-for-recommendations-and-search.html
preview:
  file: 2021-06-27-system-design-for-recommendations-and-search.preview-fa1f4e6e9284.webp
  width: 256
  height: 134
  color: '#f4f5f6'
images:
- source: https://eugeneyan.com/assets/og_image/discovery-2x2-v2.jpg
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-14b00f8ed778.jpg
    width: 1200
    height: 630
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/discovery-2x2.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-1d2e84bb27a9.webp
    width: 1000
    height: 556
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-fb40fd07e0a8.webp
    width: 320
    height: 178
  color: '#fefefe'
- source: https://eugeneyan.com/assets/discovery-system-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-535f9e994481.webp
    width: 1000
    height: 559
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-9b3afa1a1b06.webp
    width: 320
    height: 179
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/alibaba-retrieval-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-81a138c7e438.webp
    width: 1000
    height: 550
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-08e0b3efb8fe.webp
    width: 320
    height: 176
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/alibaba-ranking-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-0f4a39be947a.webp
    width: 1000
    height: 880
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-9905c1766ebd.webp
    width: 320
    height: 282
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/facebook-retrieval-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-a542d65c9cde.webp
    width: 1000
    height: 796
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/jd-retrieval-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-1c580f352e63.webp
    width: 1000
    height: 387
  color: '#fbfbfb'
- source: https://eugeneyan.com/assets/jd-model-server.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-42d2aa72cdba.webp
    width: 1000
    height: 555
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/doordash-search-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-03da94e7b24e.webp
    width: 1000
    height: 801
  color: '#d1f0f5'
- source: https://eugeneyan.com/assets/linkedin-offline-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-d355851ef45e.webp
    width: 1000
    height: 532
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-7ed019c1bf7d.webp
    width: 320
    height: 170
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/linkedin-online-design.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-efe514fb370b.webp
    width: 1000
    height: 578
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-8c11784278a9.webp
    width: 320
    height: 185
  color: '#fcfcfc'
- source: https://eugeneyan.com/assets/4-stage-recsys.webp
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-f9504c113fef.webp
    width: 1200
    height: 562
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-782efd4afb93.webp
    width: 320
    height: 150
  color: '#fafafa'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2021-06-27-system-design-for-recommendations-and-search.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2021-06-27-system-design-for-recommendations-and-search.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

> Also translated to [Korean](https://ziminpark.github.io/posts/system-design-for-discovery/) thanks to Zimin Park.

How do the system designs for industrial recommendations and search look like? It’s uncommon to see system design discussed in machine learning papers or blogs; most focus on model design, training data, and/or loss functions. Nonetheless, the handful of papers that discuss implementation details elucidate design patterns and best practices that are hard to gain outside of hands-on experience.

Specific to discovery systems (i.e., recommendations and search), most implementations I’ve come across follow a similar paradigm—components and processes are split into offline vs. online environments, and candidate retrieval vs. ranking steps. The 2 x 2 below tries to simplify this.

![2x2 of online vs. offline environments, and candidate retrieval vs. ranking.](https://eugeneyan.com/assets/discovery-2x2.webp "2x2 of online vs. offline environments, and candidate retrieval vs. ranking.")

2 x 2 of online vs. offline environments, and candidate retrieval vs. ranking.

**The offline environment** largely hosts batch processes such as model training (e.g., representation learning, ranking), creating embeddings for catalog items, and building an approximate nearest neighbors (ANN) index or knowledge graph to find similar items. It may also include loading item and user data into a feature store that is used to augment input data during ranking.

**The online environment** then uses the artifacts generated (e.g., ANN indices, knowledge graphs, models, feature stores) to serve individual requests. A typical approach is converting the input item or search query into an embedding, followed by candidate retrieval and ranking. There are also other preprocessing steps (e.g., standardizing queries, tokenization, spell check) and post-processing steps (e.g., filtering undesirable items, business logic) though we won’t discuss them in this writeup.

**Candidate retrieval** is a fast—but coarse—step to narrow down millions of items into hundreds of candidates. We trade off precision for efficiency to quickly narrow the search space (e.g., from millions to hundreds, a 99.99% reduction) for the downstream ranking task. Most contemporary retrieval methods convert the input (i.e., item, search query) into an embedding before using ANN to find similar items. Nonetheless, in the examples below, we’ll also see systems using graphs (DoorDash) and decision trees (LinkedIn).

**Ranking** is a slower—but more precise—step to score and rank top candidates. As we’re processing fewer items (i.e., hundreds instead of millions), we have room to add features that would have been infeasible in the retrieval step (due to compute and latency constraints). Such features include item and user data, and contextual information. We can also use more sophisticated models with more layers and parameters.

Ranking can be modeled as a learning-to-rank or classification task, with the latter being more commonly seen. If deep learning is applied, the final output layer is either a softmax over a catalog of items, or a sigmoid predicting the likelihood of user interaction (e.g., click, purchase) for each user-item pair.

Next, let’s see how the processes above come together in a recommender or search system.

![Basic system design of a recommender or search system.](https://eugeneyan.com/assets/discovery-system-design.webp "Basic system design of a recommender or search system.")

Basic system design for recommendations and search, based on the 2 x 2 above.

**In the offline environment, data flows bottom-up,** where we use training data and item/user data to create artifacts such as models, ANN indices, and feature stores. These artifacts are then loaded into the online environment (via the dashed arrows). **In the online environment, each request flows left to right,** through the retrieval and ranking steps before returning a set of results (e.g., recommendations, search results).

Additional details on some arrows in the diagram:

1. With the trained representation learning model, embed items in the catalog.
2. With the item embeddings, build the ANN index that allows retrieval of similar embeddings and their respective items.
3. Get (historical) features to augment training data for the ranking model. Use the same feature store in offline training and online serving to minimize train-serve skew. Might require [time travel](https://eugeneyan.com/writing/feature-stores/.#integrity-creating-correct-offline-and-online-features).
4. Use the input query/item(s) embedding to retrieve `k` similar items via ANN.
5. Add item and user features to the candidates for downstream ranking.
6. Rank the candidates based on objectives such as click, conversion, etc.

> Update: This 2x2 has since been referenced in other resources, including:
>
> - NVIDIA’s “[Recommender Systems, Not Just Recommender Models](https://medium.com/nvidia-merlin/recommender-systems-not-just-recommender-models-485c161c755e)”
> - Xavier Amatriain’s “[Blueprints for RecSys Architectures](https://amatriain.net/blog/RecsysArchitectures)”

## Examples from Alibaba, Facebook, JD, Doordash, etc.

Next, we’ll briefly discuss the high-level system design of some discovery systems, based on their respective papers and tech blogs. I’ll highlight how these systems are split into offline and online environments, and their retrieval and ranking steps. For full details on the methodology, model, etc., I recommend you read the full paper/blog.

**We start with Alibaba’s sharing about [building item embeddings for candidate retrieval](https://arxiv.org/abs/1803.02349).** In the *offline* environment, session-level user-item interactions are mined to construct a weighted, bidirectional item graph. The graph is then used to generate item sequences via random walks. Item embeddings are then learned via representation learning (i.e., word2vec skip-gram), doing away with the need for labels. Finally, with the item embeddings, they get the nearest neighbor for each item and store it in their item-to-item (i2) similarity map (i.e., a key-value store).

![Alibaba's design for candidate retrieval in Taobao.](https://eugeneyan.com/assets/alibaba-retrieval-design.webp "Alibaba's design for candidate retrieval in Taobao.")

Alibaba's design for candidate retrieval in Taobao via item embeddings and ANN.

In the *online* environment, when the user launches the app, the [Taobao](https://taobao.com) Personalization Platform (TPP) starts by fetching the latest items that the user interacted with (e.g., click, like, purchase). These items are then used to *retrieve* candidates from the i2i similarity map. The candidates are then passed to the Ranking Service Platform (RSP) for *ranking* via a deep neural network, before being displayed to the user.

**Alibaba also shared a similar example where they apply a [graph network for ranking](https://arxiv.org/abs/2005.12002).** In the *offline* environment, they combine an existing knowledge graph (`G`), user behavior (e.g., impressed but not clicked, clicked), and item data to create an adaptive knowledge graph (`G_ui`). This is then merged with user data (e.g., demographics, user-item preferences) to train the ranking model (Adaptive Target-Behavior Relational Graph Network, ATBRN).

![Alibaba's design for ranking in Taobao.](https://eugeneyan.com/assets/alibaba-ranking-design.webp "Alibaba's design for ranking in Taobao.")

Alibaba's design for ranking in Taobao via a graph network (ATBRN).

In the *online* environment, given a user request, the candidate generator *retrieves* a set of candidates and the user ID, before passing them to the Real-Time Prediction (RTP) platform. RTP then queries the knowledge graph and feature stores for item and user attributes. The graph representations, item data, and user data is then fed into the *ranking* model (i.e., ATBRN) to predict the probability of click on each candidate item. The candidates are then reordered based on probability and displayed to the user.

**Next, we look at Facebook’s [embedding-based retrieval for search](https://arxiv.org/abs/2006.11632).** In the *offline* environment (right half of image), they first train a two-tower network—with a query encoder and document encoder—that outputs cosine similarity for each query-document pair (not shown in image). This ensures that search queries and documents (e.g., user profiles, groups) are in the same embedding space.

![Facebook's design for embedding-based retrieval.](https://eugeneyan.com/assets/facebook-retrieval-design.webp "Facebook's design for embedding-based retrieval.")

Facebook's design for embedding-based retrieval via query and document encoders.

Then, with the document encoder, they embed each document via Spark batch jobs. The embeddings are then quantized and published into their ANN index (“inverted index”). This ANN index is based on [Faiss](https://engineering.fb.com/2017/03/29/data-infrastructure/faiss-a-library-for-efficient-similarity-search/) and fine-tuned (see Section 4.1 of paper). The embeddings are also published in a forward index, without quantization, for ranking. The forward index can also include other data such as profile and group attributes to augment candidates during ranking.

In the *online* environment, each search request goes through query understanding and is embedded via the query encoder. The search request and associated embedding then go through the *retrieval* step to get nearest neighbor candidates via the ANN index and boolean filtering (e.g., match on name, location, etc.). The candidates are then augmented with full embeddings and additional data from the forward index before being *ranked*.

**JD shared a similar approach for [semantic retrieval for search](https://arxiv.org/abs/2006.02282).** In the *offline* environment, a two-tower model with a query encoder and an item encoder is trained to output a similarity score for each query-item pair. The item encoder then embeds catalog items before loading them into an embedding index (i.e., key-value store).

![Major stages of an e-commerce search systems (left), JD's design for candidate retrieval (right).](https://eugeneyan.com/assets/jd-retrieval-design.webp "Major stages of an e-commerce search systems (left), JD's design for candidate retrieval (right).")

Major stages of an e-commerce search systems (left), JD's design for candidate retrieval (right).

Then, in the *online* environment, each query goes through preprocessing (e.g., spelling correction, tokenization, expansion, and rewriting) before being embedding via the query encoder. The query embedding is then used to *retrieve* candidates from the embedding index via nearest neighbors lookup. The candidates are then *ranked* on factors such as relevance, predicted conversion, etc.

The paper also shares practical tips to optimize model training and serving. For model training, they raised that the de facto input of user-item interaction, item data, and user data is duplicative—item and user data appear once per row, consuming significant disk space. To address this, they built a custom TensorFlow dataset where user and item data are first loaded into memory as lookup dictionaries. Then, during training, these dictionaries are queried to append user and item attributes to the training set. This simple practice reduced training data size by 90%.

They also called out the importance of ensuring offline training and online serving consistency. For their system, the most critical step was text tokenization which happens thrice (data preprocessing, training, serving). To minimize train-serve skew, they built a C++ tokenizer with a thin Python wrapper that was used for all tokenization tasks.

For model serving, they shared how they reduced latency by combining services. Their model had two key steps: query embedding and ANN lookup. The simple approach would be to have each as a separate service, but this would require two network calls and thus double network latency. Thus, they unified the query embedding model and ANN lookup in a single instance, where the query embedding is passed to the ANN via memory instead of network.

They also shared how they run hundreds of models simultaneously, for different retrieval tasks and various A/B tests. Each “servable” consists of a query embedding model and an ANN lookup, requiring 10s of GB. Thus, each servable had their own instance, with a proxy module (or load balancer) to direct incoming requests to the right servable.

![How JD organizes the embedding model and ANN indices across multiple versions.](https://eugeneyan.com/assets/jd-model-server.webp "How JD organizes the embedding model and ANN indices across multiple versions.")

How JD organizes the embedding model and ANN indices across multiple versions.

(Aside: My candidate retrieval systems have a similar design pattern. Embedding stores and ANN indices are hosted on the same docker container—you’ll be surprised how far this goes with efficiently-sized embeddings. Furthermore, Docker makes it easy to version, deploy, and roll back each model, as well as scale horizontally. Fronting the model instances with a load balancer takes care of directing incoming requests, [blue-green deployments](https://docs.aws.amazon.com/wellarchitected/latest/machine-learning-lens/bluegreen-deployments.html), and [A/B testing](https://docs.aws.amazon.com/sagemaker/latest/dg/model-ab-testing.html). SageMaker makes this easy.)

**Next, we move from the embedding + ANN paradigm and look at DoorDash’s use of a [knowledge graph for query expansion and retrieval](https://doordash.engineering/2020/12/15/understanding-search-intent-with-better-recall/).** In the *offline* environment, they train models for query understanding, query expansion, and ranking. They also load documents (i.e., restaurants and food items) into ElasticSearch for use in retrieval, and attribute data (e.g., ratings, price points, tags) into a feature store.

![How DoorDash splits their search into offline and online, and retrieval (recall) and ranking (precision)](https://eugeneyan.com/assets/doordash-search-design.webp "How DoorDash splits their search into offline and online, and retrieval (recall) and ranking (precision)")

DoorDash splits search into offline and online, and retrieval (recall) and ranking (precision).

In the *online* environment, each incoming query is first standardized (e.g., spell check) and synonymized (via a manually curated dictionary). Then, the knowledge graph (Neo4J) [expands the query](https://eugeneyan.com/writing/search-query-matching/#graph-based-adding-concepts-and-relationships) by finding related tags. For example, a query for “KFC” will return tags such as “fried chicken” and “wings”. These tags are then used to *retrieve* similar restaurants such as “Popeyes” and “Bonchon”.

These candidates are then *ranked* based on lexical similarity between the query and documents (aka restaurants, food items), store popularity, and possibly the search context (e.g., time of day, location). Finally, the ranked results are augmented with attributes such as ratings, price point, and delivery time and cost before displayed to the customer.

**Finally, we look at how LinkedIn [personalizes talent search results](https://arxiv.org/abs/1902.09041).** Their system relies heavily on XGBoost, first as a model to retrieve the top 1,000 candidates for ranking, and second, as a generator of features (i.e., model scores, tree interactions) for their downstream ranking model (a generalized linear mixed model aka GLMix).

![LinkedIn's offline design for generating GLMix ranking models.](https://eugeneyan.com/assets/linkedin-offline-design.webp "LinkedIn's offline design for generating GLMix ranking models.")

LinkedIn's offline design for generating tree-based features, and training GLMix ranking models.

In the *offline* environment, they first combine impression and label data to generate training data (step 1 in image above). Labels consist of instances where the recruiter sent a message to potential hires and the user responded positively. The training data is then fed into a pre-trained XGBoost model to generate model scores and tree interaction features (step 3 in image above) to augment the training data. This augmented data is then used to train the ranking model (GLMix).

![LinkedIn's online design for candidate retrieval, feature augmentation, and ranking via GLMix.](https://eugeneyan.com/assets/linkedin-online-design.webp "LinkedIn's online design for candidate retrieval, feature augmentation, and ranking via GLMix.")

LinkedIn's online design for candidate retrieval, feature augmentation, and ranking via GLMix.

In the *online* environment, with each search request, the search engine (maybe Elastic or Solr?) first *retrieves* candidates which are then scored via a first-level XGBoost model. The top 1,000 candidates are then augmented with (i) additional features (step 2 in image above) and (ii) tree interaction features via a second-level XGBoost model (step 3 in image above). Finally, these augmented candidates are ranked before the top 125 results are shown to the user.

## Conclusion

That was a brief overview of the offline-online, retrieval-ranking pattern for search and recommendations. While this isn’t the only approach for discovery systems, from what I’ve seen, it’s the most common design pattern. I’ve found it helpful to distinguish the latency-constrained online systems from the less-demanding offline systems, and split the online process into retrieval and ranking steps.

If you’re starting to build your discovery system, start with [candidate retrieval via simple embeddings and approximate nearest neighbors](https://eugeneyan.com/writing/real-time-recommendations/#how-to-design-and-implement-an-mvp), before adding a ranker on top of it. Alternatively, consider using a knowledge graph [like Uber and DoorDash did](https://eugeneyan.com/writing/search-query-matching/#graph-based-adding-concepts-and-relationships). But before you get too excited, think hard about whether you need real-time retrieval and ranking, or [if batch recommendations will suffice](https://eugeneyan.com/writing/real-time-recommendations/#when-not-to-use-real-time-recommendations).

Did I miss out anything? Please reach out and let me know!

> Let's explore how recsys & search are often split into:
>
> • Latency-constrained online vs. less-demanding offline environments \
> • Fast but coarse candidate retrieval vs. slower and more precise ranking
>
> Examples from Alibaba, Facebook, JD, DoorDash, etc.[https://t.co/zTsfElLw1z](https://t.co/zTsfElLw1z)
>
> — Eugene Yan (@eugeneyan) [June 30, 2021](https://twitter.com/eugeneyan/status/1410029817496576009?ref_src=twsrc%5Etfw)

Update (2022-04-14): Even Oldridge and Karl Byleen-Higley from NVIDIA updated the [2-stage design to 4-stages](https://medium.com/nvidia-merlin/recommender-systems-not-just-recommender-models-485c161c755e) by adding a filtering step and splitting ranking into scoring and ordering. They also presented it at [KDD’s Industrial Recommender Systems workshop](https://www.youtube.com/watch?v=5qjiY-kLwFY&list=PL65MqKWg6XcrdN4TJV0K1PdLhF_Uq-b43&index=4).

![NVIDIA augmented the 2-stage design with 2 more stages.](https://eugeneyan.com/assets/4-stage-recsys.webp "NVIDIA augmented the 2-stage design with 2 more stages.")

Even Oldridge & Karl Byleen-Higley (NVIDIA) augmented my 2-stage design with 2 more stages.

## References

- [Billion-scale Commodity Embedding for E-commerce Recommendation](https://arxiv.org/abs/1803.02349) `Alibaba`
- [Adaptive Target-Behavior Relational Graph Network for Recommendation](https://arxiv.org/abs/2005.12002) `Alibaba`
- [Embedding-based Retrieval in Facebook Search](https://arxiv.org/abs/2006.11632) `Facebook`
- [An End-to-End Solution for E-commerce Search via Embedding Learning](https://arxiv.org/abs/2006.02282) `JD`
- [Things Not Strings: Understanding Search Intent with Better Recall](https://doordash.engineering/2020/12/15/understanding-search-intent-with-better-recall/) `DoorDash`
- [Entity Personalized Talent Search Models with Tree Interaction Features](https://arxiv.org/abs/1902.09041) `LinkedIn`
- [Faiss: A Library for Efficient Similarity Search](https://engineering.fb.com/2017/03/29/data-infrastructure/faiss-a-library-for-efficient-similarity-search/) `Facebook`

 **Thanks** to Yang Xinyi for reading drafts of this.

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Jun 2021). System Design for Recommendations and Search. eugeneyan.com. https://eugeneyan.com/writing/system-design-for-discovery/.

or

```
@article{yan2021system,
  title   = {System Design for Recommendations and Search},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2021},
  month   = {Jun},
  url     = {https://eugeneyan.com/writing/system-design-for-discovery/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
