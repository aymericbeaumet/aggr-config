---
title: How to Explain the Prediction of a Machine Learning Model?
link: https://lilianweng.github.io/posts/2017-08-01-interpretation/
source: lilianweng-github-io
published: 2017-08-01T00:00:00Z
updated: 2017-08-01T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: The machine learning models have started penetrating into critical areas like health care, justice systems, and financial industry. Thus to figure out how the models make the decisions and make sure the decisioning process is aligned with the ethnic requirements or legal regulations becomes a necessity.
content: extracted
html: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.html
preview:
  file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.preview-f77c8451c233.webp
  width: 256
  height: 234
  color: '#f4f4f4'
images:
- source: https://lilianweng.github.io/posts/2017-08-01-interpretation/interpretability_prediction_decomposition.png
  original:
    file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-124e89eb188a.png
    width: 882
    height: 806
  variants:
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-480bba287031.webp
    width: 320
    height: 292
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-24b572bcd456.webp
    width: 640
    height: 585
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-8d4e9e6abad8.webp
    width: 882
    height: 806
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2017-08-01-interpretation/interpretability_local_gradient.png
  original:
    file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-289b37d3ea81.png
    width: 1338
    height: 1180
  variants:
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-b24bf8094666.webp
    width: 320
    height: 282
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-5ec39b5e1544.webp
    width: 640
    height: 564
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-630c63b83cf1.webp
    width: 960
    height: 847
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-221c56bfea79.webp
    width: 1280
    height: 1129
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-b0012dc52d04.webp
    width: 1338
    height: 1180
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-08-01-interpretation/LIME_interpretable_representation.png
  original:
    file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-9d3d227217f4.png
    width: 1400
    height: 672
  variants:
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-4aa3b78c2c71.webp
    width: 320
    height: 154
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-1391be33b3b3.webp
    width: 640
    height: 307
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-610530d81aaa.webp
    width: 960
    height: 461
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-b06fb1a3ef2d.webp
    width: 1280
    height: 614
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-4a7347de9a9a.webp
    width: 1400
    height: 672
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2017-08-01-interpretation/LIME_illustration.png
  original:
    file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-9a8a6b816d13.png
    width: 553
    height: 344
  variants:
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-9559bd6204c5.webp
    width: 320
    height: 199
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-452251a000d8.webp
    width: 553
    height: 344
  color: '#f4e2e1'
- source: https://lilianweng.github.io/posts/2017-08-01-interpretation/LIME.png
  original:
    file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-07a7752bf9d8.png
    width: 1400
    height: 707
  variants:
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-4d4d0b38a759.webp
    width: 320
    height: 162
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-d2a31ecc22f8.webp
    width: 640
    height: 323
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-b8cd69295a7f.webp
    width: 960
    height: 485
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-2a0b36f16e4f.webp
    width: 1280
    height: 646
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-f0625120af88.webp
    width: 1400
    height: 707
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-08-01-interpretation/BETA.png
  original:
    file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-ae234d79c4ab.png
    width: 1514
    height: 798
  variants:
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-6a31509f80a6.webp
    width: 320
    height: 169
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-6592987f77ba.webp
    width: 640
    height: 337
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-0d13f8f6d4b7.webp
    width: 960
    height: 506
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-5c02729c7d23.webp
    width: 1280
    height: 675
  - file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-a6676a67059b.webp
    width: 1514
    height: 798
  color: '#fafafa'
- source: https://lilianweng.github.io/posts/2017-08-01-interpretation/adversarial_examples.png
  original:
    file: 2017-08-01-how-to-explain-the-prediction-of-a-machine-learning-model.image-05e65dcda2b1.png
    width: 1544
    height: 648
  color: '#fdfdfd'
---

The machine learning models have started penetrating into critical areas like health care, justice systems, and financial industry. Thus to figure out how the models make the decisions and make sure the decisioning process is aligned with the ethnic requirements or legal regulations becomes a necessity.

Meanwhile, the rapid growth of deep learning models pushes the requirement of interpreting complicated models further. People are eager to apply the power of AI fully on key aspects of everyday life. However, it is hard to do so without enough trust in the models or an efficient procedure to explain unintended behavior, especially considering that the deep neural networks are born as *black-boxes*.

Think of the following cases:

1. The financial industry is highly regulated and loan issuers are required by law to make fair decisions and explain their credit models to provide reasons whenever they decide to decline loan application.
2. Medical diagnosis model is responsible for human life. How can we be confident enough to treat a patient as instructed by a black-box model?
3. When using a criminal decision model to predict the risk of recidivism at the court, we have to make sure the model behaves in an equitable, honest and nondiscriminatory manner.
4. If a self-driving car suddenly acts abnormally and we cannot explain why, are we gonna be comfortable enough to use the technique in real traffic in large scale?

At [Affirm](https://www.affirm.com/), we are issuing tens of thousands of installment loans every day and our underwriting model has to provide declination reasons when the model rejects one’s loan application. That’s one of the many motivations for me to dig deeper and write this post. Model interpretability is a big field in machine learning. This review is never met to exhaust every study, but to serve as a starting point.

* * *

## Interpretable Models

Lipton (2017) summarized the properties of an interpretable model in a theoretical review paper, [“The mythos of model interpretability”](https://arxiv.org/pdf/1606.03490.pdf): A human can repeat (*“simulatability”*) the computation process with a full understanding of the algorithm (*“algorithmic transparency”*) and every individual part of the model owns an intuitive explanation (*“decomposability”*).

Many classic models have relatively simpler formation and naturally, come with a model-specific interpretation method. Meanwhile, new tools are being developed to help create better interpretable models ([Been, Khanna, & Koyejo, 2016](http://papers.nips.cc/paper/6300-examples-are-not-enough-learn-to-criticize-criticism-for-interpretability.pdf); [Lakkaraju, Bach & Leskovec, 2016](http://www.kdd.org/kdd2016/papers/files/rpp1067-lakkarajuA.pdf)).

## Regression

A general form of a linear regression model is:

$$ y = w\_0 + w\_1 x\_1 + w\_2 x\_2 + … + w\_n x\_n $$

The coefficients describe the change of the response triggered by one unit increase of the independent variables. The coefficients are not comparable directly unless the features have been standardized (check sklearn.preprocessing.[StandardScalar](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler) and [RobustScaler](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html#sklearn.preprocessing.RobustScaler)), since one unit of different features can refer to very different things. Without standardization, the product $w\_i \\dot x\_i$ can be used to quantify one feature’s contribution to the response.

## Naive Bayes

Naive Bayes is named as “Naive” because it works on a very simplified assumption that features are independent of each other and each contributes to the output independently.

Given a feature vector $\\mathbf{x} = \[x\_1, x\_2, \\dots, x\_n\]$ and a class label $c \\in \\{1, 2, \\dots, C\\}$, the probability of this data point belonging to this class is:

$$ \\begin{aligned} p(c | x\_1, x\_2, \\dots, x\_n) &\\propto p(c, x\_1, x\_2, \\dots, x\_n)\\\\ &\\propto p(c) p(x\_1 | c) p(x\_2 | c) \\dots p(x\_n | c)\\\\ &\\propto p(c) \\prod\_{i=1}^n p(x\_i | c). \\end{aligned} $$

The Naive Bayes classifier is then defined as:

$$ \\hat{y} = \\arg\\max\_{c \\in 1, \\dots, C} p(c) \\prod\_{i=1}^n p(x\_i | c) $$

Because the model has learned the prior $p(x\_i \\vert c)$ during the training, the contribution of an individual feature value can be easily measured by the posterior, $p(c \\vert x\_i) = p(c)p(x\_i \\vert c) / p(x\_i)$.

## Decision Tree/Decision Lists

Decision lists are a set of boolean functions, usually constructed by the syntax like `if... then... else...`. The if-condition contains a function involving one or multiple features and a boolean output. Decision lists are born with good interpretability and can be visualized in a tree structure. Many research on decision lists is driven by medical applications, where the interpretability is almost as crucial as the model itself.

A few types of decision lists are briefly described below:

- [Falling Rule Lists (FRL)](http://proceedings.mlr.press/v38/wang15a.pdf) (Wang and Rudin, 2015) has fully enforced monotonicity on feature values. One key point, for example in the binary classification context, is that the probability of prediction $Y=1$ associated with each rule decreases as one moves down the decision lists.
- [Bayesian Rule List (BRL)](https://arxiv.org/abs/1511.01644) (Letham et al., 2015) is a generative model that yields a posterior distribution over possible decision lists.
- [Interpretable Decision Sets (IDS)](https://cs.stanford.edu/people/jure/pubs/interpretable-kdd16.pdf) (Lakkaraju, Bach & Leskovec, 2016) is a prediction framework to create a set of classification rules. The learning is optimized for both accuracy and interpretability simultaneously. IDS is closely related to the BETA method I’m gonna describe [later](https://lilianweng.github.io/posts/2017-08-01-interpretation/#beta-black-box-explanation-through-transparent-approximations) for interpreting black-box models.

## Random Forests

Weirdly enough, many people believe that the [Random Forests](http://www.math.univ-toulouse.fr/~agarivie/Telecom/apprentissage/articles/randomforest2001.pdf) model is a black box, which is not true. Considering that the output of random forests is the majority vote by a large number of independent decision trees and each tree is naturally interpretable.

It is not very hard to gauge the influence of individual features if we look into a single tree at a time. The global feature importance of random forests can be quantified by the total decrease in node impurity averaged over all trees of the ensemble (“mean decrease impurity”).

For one instance, because the decision paths in all the trees are well tracked, we can use the difference between the mean value of data points in a parent node between that of a child node to approximate the contribution of this split. Read more in this series of blog posts: [Interpreting Random Forests](http://blog.datadive.net/interpreting-random-forests/).

## Interpreting Black-Box Models

A lot of models are not designed to be interpretable. Approaches to explaining a black-box model aim to extract information from the trained model to justify its prediction outcome, without knowing how the model works in details. To keep the interpretation process independent from the model implementation is good for real-world applications: Even when the base model is being constantly upgraded and refined, the interpretation engine built on top would not worry about the changes.

Without the concern of keeping the model transparent and interpretable, we can endow the model with greater power of expressivity by adding more parameters and nonlinearity computation. That’s how deep neural networks become successful in tasks involving rich inputs.

There is no hard requirement on how the explanation should be presented, but the primary goal is mainly to answer: **Can I trust this model?** When we rely on the model to make a critical or life-and-death decision, we have to make sure the model is trustworthy ahead of time.

The interpretation framework should balance between two goals:

- **Fidelity**: the prediction produced by an explanation should agree with the original model as much as possible.
- **Interpretability**: the explanation should be simple enough to be human-understandable.

> Side Notes: The next three methods are designed for local interpretation.

## Prediction Decomposition

[Robnik-Sikonja and Kononenko (2008)](http://lkm.fri.uni-lj.si/rmarko/papers/RobnikSikonjaKononenko08-TKDE.pdf) proposed to explain the model prediction for one instance by measuring the difference between the original prediction and the one made with omitting a set of features.

Let’s say we need to generate an explanation for a classification model $f: \\mathbf{X} \\rightarrow \\mathbf{Y}$. Given a data point $x \\in X$ which consists of $a$ individual values of attribute $A\_i$, $i = 1, \\dots, a$, and is labeled with class $y \\in Y$. The *prediction difference* is quantified by computing the difference between the model predicted probabilities with or without knowing $A\_i$:

$$ \\text{probDiff}\_i (y | x) = p(y| x) - p(y | x \\backslash A\_i) $$

(The paper also discussed on using the odds ratio or the entropy-based information metric to quantify the prediction difference.)

**Problem**: If the target model outputs a probability, then great, getting $ p(y \\vert x) $ is straightforward. Otherwise, the model prediction has to run through an appropriate post-modeling calibration to translate the prediction score into probabilities. This calibration layer is another piece of complication.

**Another problem**: If we generate $x \\backslash A\_i$ by replacing $A\_i$ with a missing value (like `None`, `NaN`, etc.), we have to rely on the model’s internal mechanism for missing value imputation. A model which replaces these missing cases with the median should have output very different from a model which imputes a special placeholder. One solution as presented in the paper is to replace $A\_i$ with all possible values of this feature and then sum up the prediction weighted by how likely each value shows in the data:

$$ \\begin{aligned} p(y \\vert x \\backslash A\_i) &= \\sum\_{s=1}^{m\_i} p(A\_i=a\_s \\vert x \\backslash A\_i) p(y \\vert x \\leftarrow A\_i=a\_s) \\\\ &\\approx \\sum\_{s=1}^{m\_i} p(A\_i=a\_s) p(y \\vert x \\leftarrow A\_i=a\_s) \\end{aligned} $$

Where $p(y \\vert x \\leftarrow A\_i=a\_s)$ is the probability of getting label $y$ if we replace the feature $A\_i$ with value $a\_s$ in the feature vector of $x$. There are $m\_i$ unique values of $A\_i$ in the training set.

With the help of the measures of prediction difference when omitting known features, we can *decompose* the impact of each individual feature on the prediction.

![](https://lilianweng.github.io/posts/2017-08-01-interpretation/interpretability_prediction_decomposition.png)\
Explanations for a SVM model predicting the survival of one male adult first-class passenger in the [Titanic dataset](https://www.kaggle.com/c/titanic/data). The information difference is very similar to the probability difference, but it measures the amount of information necessary to find out $y$ is true for the given instance without the knowledge of $A\\\_i$: $\\text{infDiff}\\\_i (y|x) = \\log\\\_2 p(y|x) - \\log\\\_2 p(y|x \\backslash A\\\_i)$. Explanations for particular instance are depicted with dark bars. The light shaded half-height bars are average positive and negative explanations for given attributes' values. In this case, being a male adult makes it very less likely to survive; the class level does not impact as much.

## Local Gradient Explanation Vector

This method ([Baehrens, et al. 2010](http://www.jmlr.org/papers/volume11/baehrens10a/baehrens10a.pdf)) is able to explain the local decision taken by arbitrary nonlinear classification algorithms, using the local gradients that characterize how a data point has to be moved to change its predicted label.

Let’s say, we have a [Bayes Classifier](https://en.wikipedia.org/wiki/Bayes_classifier) which is trained on the data set $X$ and outputs probabilities over the class labels $Y$, $p(Y=y \\vert X=x)$. And one class label $y$ is drawn from the class label pool, $\\{1, 2, \\dots, C\\}$. This Bayes classifier is constructed as:

$$ f^{\*}(x) = \\arg \\min\_{c \\in \\{1, \\dots, C\\}} p(Y \\neq c \\vert X = x) $$

The *local explanation vector* is defined as the derivative of the probability prediction function at the test point $x = x\_0$. A large entry in this vector highlights a feature with a big influence on the model decision; A positive sign indicates that increasing the feature would lower the probability of $x\_0$ assigned to $f^{\*}(x\_0)$.

However, this approach requires the model output to be a probability (similar to the [“Prediction Decomposition”](https://lilianweng.github.io/posts/2017-08-01-interpretation/#prediction-decomposition) method above). What if the original model (labelled as $f$) is not calibrated to yield probabilities? As suggested by the paper, we can approximate $f$ by another classifier in a form that resembles the Bayes classifier $f^{\*}$:

(1) Apply [Parzen window](https://en.wikipedia.org/?title=Parzen_window&redirect=no) to the training data to estimate the weighted class densities:

$$ \\hat{p}\_{\\sigma}(x, y=c) = \\frac{1}{n} \\sum\_{i \\in I\_c} k\_{\\sigma} (x - x\_i) $$

Where $I\_c$ is the index set containing the indices of data points assigned to class $c$ by the model $f$, $I\_c = \\{i \\vert f(x\_i) = c\\}$. $k\_{\\sigma}$ is a kernel function. Gaussian kernel is a popular one among [many candidates](https://en.wikipedia.org/wiki/Kernel_\(statistics\)#Kernel_functions_in_common_use).

(2) Then, apply the Bayes’ rule to approximate the probability $p(Y=c \\vert X=x)$ for all classes:

$$ \\begin{aligned} \\hat{p}\_{\\sigma}(y=c | x) &= \\frac{\\hat{p}\_{\\sigma}(x, y=c)}{\\hat{p}\_{\\sigma}(x, y=c) + \\hat{p}\_{\\sigma}(x, y \\neq c)} \\\\ &\\approx \\frac{\\sum\_{i \\in I\_c} k\_{\\sigma} (x - x\_i)}{\\sum\_i k\_{\\sigma} (x - x\_i)} \\end{aligned} $$

(3) The final estimated Bayes classifier takes the form:

$$ \\hat{f}\_{\\sigma} = \\arg\\min\_{c \\in \\{1, \\dots, C\\}} \\hat{p}\_{\\sigma}(y \\neq c \\vert x) $$

Noted that we can generate the labeled data with the original model $f$, as much as we want, not restricted by the size of the training data. The hyperparameter $\\sigma$ is selected to optimize the chances of $\\hat{f}\_{\\sigma}(x) = f(x)$ to achieve high fidelity.

![](https://lilianweng.github.io/posts/2017-08-01-interpretation/interpretability_local_gradient.png)\
An example of how local gradient explanation vector is applied on simple object classification with Gaussian Processes Classifier (GPC). The GPC model outputs the probability by nature. (a) shows the training points and their labels in red (positive 1) and blue (negative -1). (b) illustrates a probability function for the positive class. (c-d) shows the local gradients and the directions of the local explanation vectors.

> Side notes: As you can see both the methods above require the model prediction to be a probability. Calibration of the model output adds another layer of complication.

## LIME (Local Interpretable Model-Agnostic Explanations)

[LIME](https://github.com/marcotcr/lime), short for *local interpretable model-agnostic explanation*, can approximate a black-box model locally in the neighborhood of the prediction we are interested ([Ribeiro, Singh, & Guestrin, 2016](https://arxiv.org/pdf/1602.04938.pdf)).

Same as above, let us label the black-box model as $f$. LIME presents the following steps:

(1) Convert the dataset into interpretable data representation: $x \\Rightarrow x\_b$.

- Text classifier: a binary vector indicating the presence or absence of a word
- Image classifier: a binary vector indicating the presence or absence of a contiguous patch of similar pixels (super-pixel).

![](https://lilianweng.github.io/posts/2017-08-01-interpretation/LIME_interpretable_representation.png)\
An example of converting an image into interpretable data representation. (Image source: [www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime))

(2) Given a prediction $f(x)$ with the corresponding interpretable data representation $x\_b$, let us sample instances around $x\_b$ by drawing nonzero elements of $x\_b$ uniformly at random where the number of such draws is also uniformly sampled. This process generates a perturbed sample $z\_b$ which contains a fraction of nonzero elements of $x\_b$.

Then we recover $z\_b$ back into the original input $z$ and get a prediction score $f(z)$ by the target model.

Use many such sampled data points $z\_b \\in \\mathcal{Z}\_b$ and their model predictions, we can learn an explanation model (such as in a form as simple as a regression) with local fidelity. The sampled data points are weighted differently based on how close they are to $x\_b$. The paper used a lasso regression with preprocessing to select top $k$ most significant features beforehand, named “K-LASSO”.

![](https://lilianweng.github.io/posts/2017-08-01-interpretation/LIME_illustration.png)\
The pink and blue areas are two classes predicted by the black-box model $f$. the big red cross is the point to be explained and other smaller crosses (predicted as pink by $f$) and dots (predicted as blue by $f$) are sampled data points. Even though the model can be very complicated, we are still able to learn a local explanation model as simple as the grey dash line. (Image source: [homes.cs.washington.edu/~marcotcr/blog/lime](https://homes.cs.washington.edu/~marcotcr/blog/lime/))

Examining whether the explanation makes sense can directly decide whether the model is trustworthy because sometimes the model can pick up spurious correlation or generalization. One interesting example in the paper is to apply LIME on an SVM text classifier for differentiating “Christianity” from “Atheism”. The model achieved a pretty good accuracy (94% on held-out testing set!), but the LIME explanation demonstrated that decisions were made by very arbitrary reasons, such as counting the words “re”, “posting” and “host” which have no connection with neither “Christianity” nor “Atheism” directly. After such a diagnosis, we learned that even the model gives us a nice accuracy, it cannot be trusted. It also shed lights on ways to improve the model, such as better preprocessing on the text.

![](https://lilianweng.github.io/posts/2017-08-01-interpretation/LIME.png)\
Illustration of how to use LIME on an image classifier. (Image source: [www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime))

For more detailed non-paper explanation, please read [this blog post](https://www.oreilly.com/learning/introduction-to-local-interpretable-model-agnostic-explanations-lime) by the author. A very nice read.

> Side Notes: Interpreting a model locally is supposed to be easier than interpreting the model globally, but harder to maintain (thinking about the [curse of dimensionality](https://en.wikipedia.org/wiki/Curse_of_dimensionality)). Methods described below aim to explain the behavior of a model as a whole. However, the global approach is unable to capture the fine-grained interpretation, such as a feature might be important in this region but not at all in another.

## Feature Selection

Essentially all the classic feature selection methods ([Yang and Pedersen, 1997](http://www.surdeanu.info/mihai/teaching/ista555-spring15/readings/yang97comparative.pdf); [Guyon and Elisseeff, 2003](http://www.jmlr.org/papers/volume3/guyon03a/guyon03a.pdf)) can be considered as ways to explain a model globally. Feature selection methods decompose the contribution of multiple features so that we can explain the overall model output by individual feature impact.

There are a ton of resources on feature selection so I would skip the topic in this post.

## BETA (Black Box Explanation through Transparent Approximations)

[BETA](https://arxiv.org/abs/1707.01154), short for *black box explanation through transparent approximations*, is closely connected to [Interpretable Decision Sets](https://cs.stanford.edu/people/jure/pubs/interpretable-kdd16.pdf) (Lakkaraju, Bach & Leskovec, 2016). BETA learns a compact two-level decision set in which each rule explains part of the model behavior unambiguously.

The authors proposed an novel objective function so that the learning process is optimized for **high fidelity** (high agreement between explanation and the model), **low unambiguity** (little overlaps between decision rules in the explanation), and **high interpretability** (the explanation decision set is lightweight and small). These aspects are combined into one objection function to optimize for.

![](https://lilianweng.github.io/posts/2017-08-01-interpretation/BETA.png)\
Measures for desiderata of a good model explanation: fidelity, unambiguity, and interpretability. Given the target model is $\\mathcal{B}$, its explanation is a two level decision set $\\Re$ containing a set of rules ${(q\\\_1, s\\\_1, c\\\_1), \\dots, (q\\\_M, s\\\_M, c\\\_M)}$, where $q\\\_i$ and $s\\\_i$ are conjunctions of predicates of the form (feature, operator, value) and $c\\\_i$ is a class label. Check [the paper](https://arxiv.org/abs/1707.01154) for more details. (Image source: [arxiv.org/abs/1707.01154](https://arxiv.org/abs/1707.01154))

## Explainable Artificial Intelligence

I borrow the name of this section from the DARPA project [“Explainable Artificial Intelligence”](https://www.darpa.mil/program/explainable-artificial-intelligence). This Explainable AI (XAI) program aims to develop more interpretable models and to enable human to understand, appropriately trust, and effectively manage the emerging generation of artificially intelligent techniques.

With the progress of the deep learning applications, people start worrying about that [we may never know even if the model goes bad](https://www.technologyreview.com/s/601860/if-a-driverless-car-goes-bad-we-may-never-know-why/). The complicated structure, the large number of learnable parameters, the nonlinear mathematical operations and [some intriguing properties](https://arxiv.org/abs/1312.6199) (Szegedy et al., 2014) lead to the un-interpretability of deep neural networks, creating a true black-box. Although the power of deep learning is originated from this complexity — more flexible to capture rich and intricate patterns in the real-world data.

Studies on **adversarial examples** ([OpenAI Blog: Robust Adversarial Examples](https://blog.openai.com/robust-adversarial-inputs/), [Attacking Machine Learning with Adversarial Examples](https://blog.openai.com/adversarial-example-research/), [Goodfellow, Shlens & Szegedy, 2015](https://arxiv.org/pdf/1412.6572.pdf); [Nguyen, Yosinski, & Clune, 2015](http://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Nguyen_Deep_Neural_Networks_2015_CVPR_paper.pdf)) raise the alarm on the robustness and safety of AI applications. Sometimes the models could show unintended, unexpected and unpredictable behavior and we have no fast/good strategy to tell why.

![](https://lilianweng.github.io/posts/2017-08-01-interpretation/adversarial_examples.png)\
Illustrations of adversarial examples. (a-d) are adversarial images that are generated by adding human-imperceptible noises onto original images ([Szegedy et al., 2013](https://arxiv.org/abs/1312.6199)). A well-trained neural network model can successfully classify original ones but fail adversarial ones. (e-h) are patterns that are generated ([Nguyen, Yosinski & Clune, 2015](http://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Nguyen_Deep_Neural_Networks_2015_CVPR_paper.pdf)). A well-trained neural network model labels them into (e) school bus, (f) guitar, (g) peacock and (h) Pekinese respectively. (Image source: [Wang, Raj & Xing, 2017](https://arxiv.org/pdf/1702.07800.pdf))

Nvidia recently developed [a method to visualize the most important pixel points](https://blogs.nvidia.com/blog/2017/04/27/how-nvidias-neural-net-makes-decisions/) in their self-driving cars’ decisioning process. The visualization provides insights on how AI thinks and what the system relies on while operating the car. If what the AI believes to be important agrees with how human make similar decisions, we can naturally gain more confidence in the black-box model.

Many exciting news and findings are happening in this evolving field every day. Hope my post can give you some pointers and encourage you to investigate more into this topic :)

* * *

Cited as:

```
@article{weng2017gan,
  title   = "How to Explain the Prediction of a Machine Learning Model?",
  author  = "Weng, Lilian",
  journal = "lilianweng.github.io",
  year    = "2017",
  url     = "https://lilianweng.github.io/posts/2017-08-01-interpretation/"
}
```

## References

\[1\] Zachary C. Lipton. [“The mythos of model interpretability.”](https://arxiv.org/pdf/1606.03490.pdf) arXiv preprint arXiv:1606.03490 (2016).

\[2\] Been Kim, Rajiv Khanna, and Oluwasanmi O. Koyejo. “Examples are not enough, learn to criticize! criticism for interpretability.” Advances in Neural Information Processing Systems. 2016.

\[3\] Himabindu Lakkaraju, Stephen H. Bach, and Jure Leskovec. [“Interpretable decision sets: A joint framework for description and prediction.”](http://www.kdd.org/kdd2016/papers/files/rpp1067-lakkarajuA.pdf) Proc. 22nd ACM SIGKDD Intl. Conf. on Knowledge Discovery and Data Mining. ACM, 2016.

\[4\] Robnik-Šikonja, Marko, and Igor Kononenko. [“Explaining classifications for individual instances.”](http://lkm.fri.uni-lj.si/rmarko/papers/RobnikSikonjaKononenko08-TKDE.pdf) IEEE Transactions on Knowledge and Data Engineering 20.5 (2008): 589-600.

\[5\] Baehrens, David, et al. [“How to explain individual classification decisions.”](http://www.jmlr.org/papers/volume11/baehrens10a/baehrens10a.pdf) Journal of Machine Learning Research 11.Jun (2010): 1803-1831.

\[6\] Marco Tulio Ribeiro, Sameer Singh, and Carlos Guestrin. [“Why should I trust you?: Explaining the predictions of any classifier.”](https://arxiv.org/pdf/1602.04938.pdf) Proc. 22nd ACM SIGKDD Intl. Conf. on Knowledge Discovery and Data Mining. ACM, 2016.

\[7\] Yiming Yang, and Jan O. Pedersen. [“A comparative study on feature selection in text categorization.”](http://www.surdeanu.info/mihai/teaching/ista555-spring15/readings/yang97comparative.pdf) Intl. Conf. on Machine Learning. Vol. 97. 1997.

\[8\] Isabelle Guyon, and André Elisseeff. [“An introduction to variable and feature selection.”](http://www.jmlr.org/papers/volume3/guyon03a/guyon03a.pdf) Journal of Machine Learning Research 3.Mar (2003): 1157-1182.

\[9\] Ian J. Goodfellow, Jonathon Shlens, and Christian Szegedy. [“Explaining and harnessing adversarial examples.”](https://arxiv.org/pdf/1412.6572.pdf) ICLR 2015.

\[10\] Christian Szegedy, Wojciech Zaremba, Ilya Sutskever, Joan Bruna, Dumitru Erhan, Ian Goodfellow, Rob Fergus. [“Intriguing properties of neural networks.”](https://arxiv.org/abs/1312.6199) Intl. Conf. on Learning Representations (2014)

\[11\] Nguyen, Anh, Jason Yosinski, and Jeff Clune. [“Deep neural networks are easily fooled: High confidence predictions for unrecognizable images.”](http://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Nguyen_Deep_Neural_Networks_2015_CVPR_paper.pdf) Proc. IEEE Conference on Computer Vision and Pattern Recognition. 2015.

\[12\] Benjamin Letham, Cynthia Rudin, Tyler H. McCormick, and David Madigan. [“Interpretable classifiers using rules and Bayesian analysis: Building a better stroke prediction model.”](https://arxiv.org/abs/1511.01644) The Annals of Applied Statistics 9, No. 3 (2015): 1350-1371.

\[13\] Haohan Wang, Bhiksha Raj, and Eric P. Xing. [“On the Origin of Deep Learning.”](https://arxiv.org/pdf/1702.07800.pdf) arXiv preprint arXiv:1702.07800 (2017).

\[14\] [OpenAI Blog: Robust Adversarial Examples](https://blog.openai.com/robust-adversarial-inputs/)

\[15\] [Attacking Machine Learning with Adversarial Examples](https://blog.openai.com/adversarial-example-research/)

\[16\] [Reading an AI Car’s Mind: How NVIDIA’s Neural Net Makes Decisions](https://blogs.nvidia.com/blog/2017/04/27/how-nvidias-neural-net-makes-decisions/)
