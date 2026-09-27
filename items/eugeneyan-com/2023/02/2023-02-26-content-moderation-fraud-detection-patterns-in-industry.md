---
title: Content Moderation & Fraud Detection - Patterns in Industry
link: https://eugeneyan.com//writing/content-moderation/
source: eugeneyan-com
published: 2023-02-26T00:00:00Z
updated: 2023-02-26T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
labels:
- machinelearning
- production
- teardown
summary: Collecting ground truth, data augmentation, cascading heuristics and models, and more.
content: extracted
html: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.html
preview:
  file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.preview-b761fe825a19.webp
  width: 256
  height: 134
  color: '#131247'
images:
- source: https://eugeneyan.com/assets/og_image/space-invaders.jpg
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-b926508a0ddc.jpg
    width: 1200
    height: 630
  color: '#02075b'
- source: https://eugeneyan.com/assets/doordash-labelling.webp
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-a0a092f7a894.webp
    width: 800
    height: 749
  color: '#fefefe'
- source: https://eugeneyan.com/assets/google-search.webp
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-caded1e54143.webp
    width: 1200
    height: 480
  variants:
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-0f05dd8c2e2f.webp
    width: 320
    height: 128
  color: '#f4f5f6'
- source: https://eugeneyan.com/assets/stackexchange-spam.webp
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-fea486a55206.webp
    width: 1200
    height: 329
  color: '#eaeaf2'
- source: https://eugeneyan.com/assets/linkedin-harassment.webp
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-4458066de714.webp
    width: 700
    height: 314
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/airbnb-loop.webp
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-242007685e38.webp
    width: 720
    height: 393
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/airbnb-features.webp
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-4136911b6787.webp
    width: 1200
    height: 487
  variants:
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-4bad78cc502a.webp
    width: 320
    height: 130
  color: '#fdfdfd'
- source: https://eugeneyan.com/assets/icon-twitter.svg
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-9f73746a86e9.png
    width: 512
    height: 512
  variants:
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-10d32c0d6dac.webp
    width: 320
    height: 320
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-1a1653690e12.webp
    width: 512
    height: 512
  color: '#000000'
- source: https://eugeneyan.com/assets/icon-linkedin.svg
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-50dfb45d5f9e.png
    width: 505
    height: 505
  variants:
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-b7fdeee63d68.webp
    width: 320
    height: 320
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-003cf9435e7e.webp
    width: 505
    height: 505
  color: '#000000'
- source: https://eugeneyan.com/assets/bluesky.svg
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-cd93481613cb.png
    width: 600
    height: 530
  variants:
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-1790c4125e73.webp
    width: 600
    height: 530
  color: '#1084fd'
- source: https://eugeneyan.com/assets/icon-facebook.svg
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-68df389f82c9.png
    width: 256
    height: 256
  variants:
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-6035d4122baf.webp
    width: 256
    height: 256
  color: '#3b5998'
- source: https://eugeneyan.com/assets/icon-mail.svg
  original:
    file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-bf2110bd7265.png
    width: 512
    height: 512
  variants:
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-c80acd329b85.webp
    width: 320
    height: 320
  - file: 2023-02-26-content-moderation-fraud-detection-patterns-in-industry.image-84c4e78a876d.webp
    width: 512
    height: 512
  color: '#000000'
---

Content moderation is the process of learning and inferring the quality of human-generated content such as product reviews, social media posts, and ads. How do we know which are irrelevant, incorrect, or downright harmful? A related problem is detecting anomalous activity such as fraudulent transactions or malicious traffic.

To learn more about building robust content moderation systems, I dug into industry papers and tech blogs on [classification](https://github.com/eugeneyan/applied-ml#classification), [anomaly detection](https://github.com/eugeneyan/applied-ml#anomaly-detection), and [search relevance](https://github.com/eugeneyan/applied-ml#search--ranking). Here are five patterns I observed:

- [Collect ground truth via human-in-the-loop](https://eugeneyan.com//writing/content-moderation/#collecting-ground-truth-via-human-in-the-loop)
- [Augment data to supplement initial ground truth](https://eugeneyan.com//writing/content-moderation/#data-augmentation-for-robustness)
- [Cascade pattern to break the problem into smaller pieces](https://eugeneyan.com//writing/content-moderation/#cascade-pattern-on-smaller-problems)
- [Combine the best of supervised and unsupervised ML](https://eugeneyan.com//writing/content-moderation/#supervised-and-unsupervised-learning)
- [Explainability to understand models and their outputs](https://eugeneyan.com//writing/content-moderation/#explainability-on-the-model-and-its-output)

## Collecting ground truth via human-in-the-loop

Regardless of whether a heuristic-based, supervised, or unsupervised solution is adopted, we typically start with collecting a set of ground truth. This ground truth can then be used to train supervised ML models as well as evaluate the performance of heuristics and unsupervised models. The ground truth also acts as seed data to bootstrap more labels via active or semi-supervised learning.

**The most straightforward way to collect ground truth is to ask users.** For [Stack Exchange to block spam on their sites](https://stackoverflow.blog/2020/06/25/how-does-spam-protection-work-on-stack-exchange), a valuable data source is users flagging posts as spam. These flags were then used to identify and act on spammy users by blocking or rate-limiting them. They were also used as training data for machine learning models.

Another example is [LinkedIn’s effort to prevent harassment](https://engineering.linkedin.com/blog/2020/fighting-harassment). Users can report messages as harassment and those messages become ground truth. Sometimes, instead of directly reporting harassment, some users may just block harassers to indirectly make the problem go away—this becomes a source of ground truth too, albeit a fuzzier one.

Similarly, as part of [Uber’s work to reduce payment fraud](https://www.uber.com/blog/project-radar-intelligent-early-fraud-detection/), users notify them of fraud when they dispute charges and file a chargeback (to have the charge refunded). After such incidents are investigated and confirmed to be fraud, they become ground truth.

**A second, less common approach is to use labels that are naturally generated as part of data pipelines and code.** We see this in [Meta’s initiative to classify sensitive data](https://arxiv.org/abs/2006.14109), such as addresses and phone numbers, to enforce data retention and access controls. Data lineage was a source of ground truth, where downstream data inherited data classifications from upstream tables. They also traced data as it went through code paths that carry known data types and used that as ground truth.

**The third, most common approach is to label data via annotation services such as Mechanical Turk or internal teams.** When [DoorDash tackled the cold-start problem in tagging menu items](https://doordash.engineering/2020/08/28/overcome-the-cold-start-problem-in-menu-item-tagging/), they built a labeling queue powered by annotation services. The queue focused on high precision (to ensure annotators don’t incorrectly label samples), high recall (to ensure annotators generated all relevant labels), and representativeness (to ensure labeled samples were representative of the actual data).

To ensure high precision and throughput, they used binary questions to simplify the task for annotators. DoorDash also provided an “Unsure” label so annotators could skip tasks they were not confident in, instead of being forced to choose a response that may not be correct. To ensure high throughput, they varied the amount of consensus needed per tag, only requiring higher consensus for tags with lower inter-annotator agreement.

To ensure label quality, they mixed in golden data when onboarding new annotators. The intent is to measure baseline precision before proceeding with labeling new samples.

![Annotating 'coffee' by first annotating 'drink' followed by 'alcoholic'](https://eugeneyan.com/assets/doordash-labelling.webp "Annotating 'coffee' by first annotating 'drink' followed by 'alcoholic'")

Annotating 'coffee' by first annotating 'drink' followed by 'alcoholic'

[Airbnb also relied on human-in-the-loop to categorize listings](https://medium.com/airbnb-engineering/building-airbnb-categories-with-ml-and-human-in-the-loop-e97988e70ebb). Listings were categorized into places of interest (e.g., coastal, national park), activities (e.g., skiing, surfing), home types (e.g., barn, castles), and home amenities (e.g., amazing pools, chef’s kitchen).

To select initial samples for annotation, they built heuristics based on guest reviews, listing data (e.g., description, attributes, images), wishlists, location, etc. After the annotation process, confirmed categories were used to power a new user experience where customers could browse listings by category. The category labels were also used as ground truth for machine learning.

Similarly, Uber had internal experts who reviewed fraud trends. These experts were able to analyze and identify new fraud patterns that machine learning models trained on past data would miss. As part of the process, they confirmed if the flagged transactions were indeed fraudulent, and then developed heuristics to programmatically identify fraud transactions.

**A final approach is to label or select samples for labeling via high-precision heuristics or models.** When [Cloudflare blocks undesired or malicious bot traffic](https://blog.cloudflare.com/cloudflare-bot-management-machine-learning-and-more/), they had heuristics that labelled data which was then used to train machine learning models. These heuristics could classify 15 - 30% of traffic with low false positive rates.

Similarly, when DoorDash wanted to select more samples for annotation, they relied on a high-precision model trained on the initial ground truth. To improve precision, they identified samples where the model prediction conflicted with the annotation label and selected similar samples. To improve recall, they use model predictions to select samples with high uncertainty (i.e., probability ≈ 0.5).

To sum up, there’s no single way to collect ground truth. A robust program will involve several of the above, starting with asking users or annotators, and then using the seed data to [bootstrap more labels](https://eugeneyan.com/writing/bootstrapping-data-labels/) via active or semi-supervised learning.

**Aside on writing labeling guides**: I chatted with ML practitioners who have experience building annotation processes to collect ground truth. Here’s a summary of their advice:

- Question responses should be binary. Instead of using a scale or having multiple choices, simplify the task.
- Labeling criteria should be as objective as possible. For example, “Is this nudity?” is more objective than “Is this adult content?”
- Have an “Unsure” label so annotators aren’t forced to pick a potentially incorrect label. Nonetheless, this may just be punting the question on hard samples.
- Measure inter-annotator agreement and calibrate among annotators—having binary labels makes this easier.
- For internal annotators, consider metrics such as [WTF@k](https://thenoisychannel.com/2012/08/20/wtf-k-measuring-ineffectiveness/).

Useful resources on writing labeling guides include DoorDash’s sharing of best practices for building a taxonomy and labeling guidelines. Also, [Google’s guidelines on search relevance](https://services.google.com/fh/files/misc/hsw-sqrg.pdf) where they annotate search results for page quality and needs met.

![Splitting search relevance labeling into Page Quality and Needs Met](https://eugeneyan.com/assets/google-search.webp "Splitting search relevance labeling into Page Quality and Needs Met")

Splitting search relevance labeling into Page Quality and Needs Met

## Data augmentation for robustness

After we’ve gathered some initial ground truth, we can augment it to generate more, albeit lower quality, ground truth. More data, especially more representative and diverse data, helps machine learning models learn more robustly.

**A typical approach is to generate synthetic data based on existing ground truth.** DoorDash did this via random text augmentation where they varied the sentence order in the description and randomly removed information such as menu category. This helped to simulate the variation in menus where merchants don’t have detailed descriptions or menu categories. During model training, they had a ratio of 100 synthetic labels to 1 actual label.

Similarly, [Cloudflare generated synthetic data to increase the diversity of their training data](https://blog.cloudflare.com/data-generation-and-sampling-strategies/). This improved ML performance when classifying malicious HTTP requests (aka payloads). The intent was to get the model to focus on the higher-level structural, semantic, and statistical aspects of the payload, instead of the tokens and keywords.

To create negative samples, they generated pseudo-random strings via a probability distribution over complex tokens. They also increased difficulty by adding elements such as valid URIs, user agents, XML content, and even “dangerous” keywords or n-grams that frequently occur in malicious payloads. The goal was to desensitize the model to the presence of malicious tokens if the payload lacked the proper semantics or structure.

The synthetic data was used to augment the core dataset by first training the model on increasingly difficult synthetic data before fine-tuning on real data. They also appended noise of varying complexity to malicious and benign samples to make their model more robust to padding attacks.

Similarly, Meta used synthetic data generators to generate sensitive data such as social security numbers, credit card numbers, addresses, etc. Uber also generated synthetic data to validate their anomaly detection algorithms during automated testing of data pipelines—this data was generated based on their experience and assumptions of fraud attacks.

**Another technique is to look for samples similar to labelled data.** This is akin to [active learning](https://eugeneyan.com/writing/bootstrapping-data-labels/#active-learning-find-samples-for-human-in-the-loop) where we find the “most” interesting samples for human annotation.

For Airbnb, after a human review confirms that the listing belongs to a particular category, they used pre-trained listing embeddings to find the 10 nearest neighbors (they called it candidate expansion). These 10 listings were then sent for human review to confirm if they belonged to the same category.

DoorDash adopted a similar approach where they quantified similarity via edit distance and embedding cosine similarity. These similar items were directly added to the training data.

In another paper, [Meta applied nearest neighbors on positive labels before active learning](https://arxiv.org/abs/2007.00077). They shared that data is often heavily skewed, with only a small fraction (1 in 1,000 or more) being relevant. Thus, using nearest neighbors as a filter reduced the number of samples to consider during active learning. This increased annotation efficiency.

## Cascade Pattern on smaller problems

In most applications, the initial problem can be **broken into smaller problems and solved via a cascade pattern.** The benefit is that it allows simple, cheap, and low-latency solutions—such as heuristics—to chip away at the problem upstream. Machine learning models can then perform inference on the remaining instances that couldn’t be dealt with confidently.

Stack Exchange has several layers of defense against spam. The first line of defense is triggered when a spammer posts too often to be humanly possible. The spammer is hit with an HTTP 429 Error (Too Many Requests) and blocked or rate-limited.

The second line of defense is based on heuristics. Specifically, they run posts through an “unholy amount of regular expressions” and some rules. If a post is caught, it is sent to users to check and potentially flag it as spam. If six users flag it as spam ([six flags](https://en.wikipedia.org/wiki/Six_Flags) lol), the post is marked as spam and the user is blocked, rate-limited, or prevented from posting.

The final line of defense is a (machine learning?) system that identifies posts most likely to be spam. They [shadow-tested](https://aws.amazon.com/sagemaker/shadow-testing/) it and found it to be extremely accurate. It was catching almost all of the blatantly obvious spam. Eventually, this system was armed to cast three automatic flags and it drastically reduced the time to spam post deletion.

![Time till spam deletion drops from no auto-flag (red) to 1 auto-flag (green) to 3 auto-flags (orange)](https://eugeneyan.com/assets/stackexchange-spam.webp "Time till spam deletion drops from no auto-flag (red) to 1 auto-flag (green) to 3 auto-flags (orange)")

Time till spam deletion drops from no auto-flag (red) to 1 auto-flag (green) to 3 auto-flags (orange)

Cloudflare also combines heuristics and machine learning (and other techniques) to identify bot traffic. They shared a comparison: If machine learning inference requires ~50ms, then hundreds of heuristics can be applied at ~20ms.

Their heuristics classify 15% of global traffic and 30% of bot management customer traffic. These heuristics also have the lowest false positive rate among detection mechanisms—pretty good for a bunch of if-else statements! Thus, incoming traffic can first be filtered by heuristics to exclude 15 - 30% of traffic before what’s left is sent to machine learning.

Meta started with rule-based heuristics to classify data sensitivity. These heuristics used counts and ratios to score data columns on sensitivity types. However, solely using these heuristics led to sub-par classification accuracy, especially for unstructured data. Thus, they augmented the rules with deep learning models that were able to use additional features such as column names and data lineage. This greatly improved accuracy.

Uber applied the cascade pattern by splitting the problem of payment fraud into two parts: *Detecting* if there’s an increased trend of fraud and *protecting* against fraud 24/7.

To detect an increased trend of fraud attacks, they trained multiple forecasting models along the dimensions of order time (when the order is fulfilled) and payment settlement maturity time (when the payment is processed). An increase in fraud shows up as a time-series anomaly. When this happens, it suggests that a new pattern of attack has emerged and fraud analysts are alerted to investigate it.

To protect against fraud, they apply pattern mining to generate high-precision algorithms. Analysts review these algorithms to minimize false positives and unnecessary incidents before the algorithms are put into production. Once in production, these algorithms scan incoming transactions and flag/prevent potentially fraudulent ones.

Similarly, LinkedIn applies a cascade of three models to identify sexual harassment in direct messages. This allows them to minimize unnecessary account or message analysis, protecting user privacy. Downstream models don’t proceed unless the upstream model flags the interaction as suspicious.

First, a sender model scores if the sender is likely to conduct harassment. This is trained on data from members that were confirmed to have conducted harassment. Features include site usage, invite behavior, etc. Next, a message model scores the content on harassment. This is trained on messages that were reported and confirmed as harassment. Finally, an interaction model scores whether the conversation is harassment. This is trained on historical conversations that resulted in harassment. Features include response time, proportion of predicted harassment messages, etc.

![Cascading models to identify harassment: Behavior, Message, Interaction](https://eugeneyan.com/assets/linkedin-harassment.webp "Cascading models to identify harassment: Behavior, Message, Interaction")

Cascading models to identify harassment: Behavior, Message, Interaction

## Supervised and unsupervised learning

Supervised classification is typically used to categorize items and predict fraud while unsupervised anomaly detection is used to identify outlier behavior that may be malicious. While the former has higher precision, it doesn’t work well if labels are sparse or low quality. Also, it can’t classify items that haven’t been seen before. This is where unsupervised techniques are complementary.

**Most supervised classifiers tend to be binary in output.** DoorDash trained a separate binary classifier for each food tag. The model was a single-layer LSTM with fasttext embeddings. While they also tried multi-class models, they did not perform as well because the training data didn’t match the natural distribution of tags. In addition, they had too few labelled samples.

Similarly, Airbnb trains a binary classifier for each listing category. Meta also used binary deep learners to predict data classification. Finally, LinkedIn’s model to identify harassment returns a binary output of harassment or not.

IMHO, it’s usually better to use multiple binary classifiers (MBC) instead of a single multi-class classifier (SMCC). Empirically, MBCs tend to outperform SMCCs. From DoorDash’s experience, it was harder to calibrate SMCCs, relative to MBCs, to match the actual data distribution. (My experience has been similar.)

Second, MBCs are more modular than SMCCs. If a new class is added, it’s easier to train and deploy a new binary classifier instead of updating the SMCC with a new class and retraining it. This is especially convenient when we’re in the early stages of using machine learning and have just started collecting ground truth. For example, when Airbnb was gathering labels, they only trained new binary classifiers when there was sufficient training data.

Nonetheless, MBCs have downsides too. When classifying an item, we’ll need to perform inference once via each binary classifier. With an SMCC, we just need to perform inference once. Furthermore, as the number of binary classifiers grow, it can be operationally challenging to support and monitor MBCs as opposed to an SMCC.

![If enough labels for machine learning, train model; else, find more samples for labeling](https://eugeneyan.com/assets/airbnb-loop.webp "If enough labels for machine learning, train model; else, find more samples for labeling")

If enough labels for machine learning, train model; else, find more samples for labeling

**Another employed technique is unsupervised anomaly detection.** [LinkedIn shared the benefits of using unsupervised isolation forests](https://engineering.linkedin.com/blog/2019/isolation-forest) to identify instances of abuse.

First, ground truth for abuse was often low quality and quantity. Labels were fuzzy with low precision and recall was sometimes poor. Second, the problem was adversarial and attackers evolved quickly. However, as long as new abusive behavior is different from normal organic behavior, it can still be detected via unsupervised techniques. Finally, the problem is imbalanced, where abusive traffic accounts for a small fraction of total traffic.

Cloudflare also shared about using unsupervised learning to identify bad bots. The main benefit was that, because unsupervised methods don’t rely on having known bot labels, they could detect bots that were never seen before. Furthermore, unsupervised techniques are harder to evade because anomalous behavior is often a direct result of the bot’s goal.

## Explainability on the model and its output

A less-mentioned but nonetheless important component in industry applications is explainability. It helps us understand the model and interpret model predictions.

To Uber, explainability was paramount in fraud detection. At their scale, incorrect fraud-related decisions can be disruptive to individuals and entire communities. Thus, they relied on human-readable rules—generated via pattern mining—to identify fraudulent transactions. Because these rules were human-readable, they could be evaluated by analysts before going into production.

Meta also emphasized feature importance. When adding a new feature, they wanted to understand its overall impact on the model. They also wanted to know what the model pays attention to when it predicts a given label. To operationalize this, they developed per-class feature importance for their PyTorch models, where feature importance is measured by the increase in prediction error after randomly permuting the feature.

Airbnb also examines the features that contribute most to a category decision via a feature importance graph. From the graph below, it’s clear that having places of interest is very important to model performance. This suggests why Airbnb put extra effort into collecting place of interest data from human reviewers.

![Distance to Point of Interest is the most important feature by far](https://eugeneyan.com/assets/airbnb-features.webp "Distance to Point of Interest is the most important feature by far")

Distance to Point of Interest is the most important feature by far and extra effort was made to collect it

Whew, that was a lot! Thanks for sticking with me through this trek of common patterns in content moderation. To recap:

- Collect ground truth from users, annotators, and high-precision heuristics/models
- Augment ground truth with synthetic data or similar instances for robust models
- Use the Cascade pattern to break the problem into smaller pieces via rules and ML
- Combine the best of precise supervised learning and robust unsupervised learning
- Apply explainability to understand how to help the model and humanize output

What other useful patterns are there in this problem space? Please share!

## References

- [How does Spam Protection Work on Stack Exchange?](https://stackoverflow.blog/2020/06/25/how-does-spam-protection-work-on-stack-exchange/)
- [The Technology Behind Fighting Harassment on LinkedIn](https://engineering.linkedin.com/blog/2020/fighting-harassment)
- [Project RADAR: Intelligent Early Fraud Detection System with Humans in the Loop](https://www.uber.com/blog/project-radar-intelligent-early-fraud-detection/)
- [Scalable Data Classification for Security and Privacy](https://arxiv.org/abs/2006.14109)
- [Using HITL to Overcome the Cold Start Problem in Menu Item Tagging](https://doordash.engineering/2020/08/28/overcome-the-cold-start-problem-in-menu-item-tagging/)
- [Building Airbnb Categories with ML and Human-in-the-Loop](https://medium.com/airbnb-engineering/building-airbnb-categories-with-ml-and-human-in-the-loop-e97988e70ebb)
- [Cloudflare Bot Management: Machine Learning and more](https://blog.cloudflare.com/cloudflare-bot-management-machine-learning-and-more/)
- [Improving the Accuracy of our Machine Learning WAF using Data Augmentation](https://blog.cloudflare.com/data-generation-and-sampling-strategies/)
- [Detecting and preventing abuse on LinkedIn using isolation forests](https://engineering.linkedin.com/blog/2019/isolation-forest)
- [Similarity Search for Efficient Active Learning and Search of Rare Concepts](https://arxiv.org/abs/2007.00077)
- [WTF! @ k: Measuring Ineffectiveness](https://thenoisychannel.com/2012/08/20/wtf-k-measuring-ineffectiveness/)
- [Search Quality Rater Guidelines: An Overview](https://services.google.com/fh/files/misc/hsw-sqrg.pdf)

If you found this useful, please cite this write-up as:

> Yan, Ziyou. (Feb 2023). Content Moderation & Fraud Detection - Patterns in Industry. eugeneyan.com. https://eugeneyan.com/writing/content-moderation/.

or

```
@article{yan2023content,
  title   = {Content Moderation & Fraud Detection - Patterns in Industry},
  author  = {Yan, Ziyou},
  journal = {eugeneyan.com},
  year    = {2023},
  month   = {Feb},
  url     = {https://eugeneyan.com/writing/content-moderation/}
}
```

Share on:

![](https://eugeneyan.com/assets/icon-twitter.svg)

![](https://eugeneyan.com/assets/icon-linkedin.svg)

![](https://eugeneyan.com/assets/bluesky.svg)

![](https://eugeneyan.com/assets/icon-facebook.svg)

![](https://eugeneyan.com/assets/icon-mail.svg)

Join **11,800+** readers getting updates on machine learning, RecSys, LLMs, and engineering.
