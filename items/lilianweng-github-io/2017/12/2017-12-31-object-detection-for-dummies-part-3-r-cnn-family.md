---
title: 'Object Detection for Dummies Part 3: R-CNN Family'
link: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/
source: lilianweng-github-io
published: 2017-12-31T00:00:00Z
updated: 2017-12-31T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: '[Updated on 2018-12-20: Remove YOLO here. Part 4 will cover multiple fast object detection algorithms, including YOLO.] [Updated on 2018-12-27: Add bbox regression and tricks sections for R-CNN.] In the series of “Object Detection for Dummies”, we started with basic concepts in image processing, such as gradient vectors and HOG, in Part 1. Then we introduced classic convolutional neural network architecture designs for classification and pioneer models for object recognition, Overfeat and DPM, in Part 2. In the third post of this series, we are about to review a set of models in the R-CNN (“Region-based CNN”) family.'
content: extracted
html: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.html
preview:
  file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.preview-1db3cb08a442.webp
  width: 256
  height: 68
  color: '#d6d2ca'
images:
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/RCNN.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-9a4a5b5a8fd4.png
    width: 2000
    height: 531
  color: '#fbfbfb'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/RCNN-bbox-regression.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-a25cab69eb84.png
    width: 1406
    height: 904
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/non-max-suppression.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-69a61dfbd0d3.png
    width: 3268
    height: 1070
  color: '#fbfbfc'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/fast-RCNN.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-ba761ab62a86.png
    width: 1112
    height: 452
  variants:
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-7c48319f7ab6.webp
    width: 320
    height: 130
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-4363d560ce4a.webp
    width: 640
    height: 260
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-a2936bea8433.webp
    width: 960
    height: 390
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-c3d02bbe3519.webp
    width: 1112
    height: 452
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/roi-pooling.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-0bb712a286ac.png
    width: 1302
    height: 676
  variants:
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-8812dd1125ee.webp
    width: 320
    height: 166
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-22d5e65173b0.webp
    width: 640
    height: 332
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-6f3fa3392d4a.webp
    width: 960
    height: 498
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-fba48c00ae34.webp
    width: 1302
    height: 676
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/l1-smooth.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-ceeb58af8a67.png
    width: 580
    height: 466
  variants:
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-dd058e2f3117.webp
    width: 320
    height: 257
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-7550ba0e2c87.webp
    width: 580
    height: 466
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/faster-RCNN.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-20ecb0940de7.png
    width: 1892
    height: 902
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/mask-rcnn.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-14cd1eebea28.png
    width: 1364
    height: 670
  variants:
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-b32a7137c10f.webp
    width: 320
    height: 157
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-e561fe6756c4.webp
    width: 640
    height: 314
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-ef6089cc22f0.webp
    width: 960
    height: 472
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-dbf2ac46770b.webp
    width: 1280
    height: 629
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-f41298f04df5.webp
    width: 1364
    height: 670
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/mask-rcnn-examples.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-a00b39cc2cbc.png
    width: 1160
    height: 864
  variants:
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-32aff1e4a4b1.webp
    width: 320
    height: 238
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-e6294e365fa2.webp
    width: 640
    height: 477
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-9af3b09166da.webp
    width: 960
    height: 715
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-4cafd769a807.webp
    width: 1160
    height: 864
  color: '#737473'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/roi-align.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-02940e8a49ae.png
    width: 912
    height: 515
  variants:
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-b67b9ec22b4d.webp
    width: 320
    height: 181
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-8dbe0504bf32.webp
    width: 640
    height: 361
  - file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-c403f9811b43.webp
    width: 912
    height: 515
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/rcnn-family-summary.png
  original:
    file: 2017-12-31-object-detection-for-dummies-part-3-r-cnn-family.image-46319869479e.png
    width: 3270
    height: 1290
  color: '#fcfcfc'
---

\[Updated on 2018-12-20: Remove YOLO here. Part 4 will cover multiple fast object detection algorithms, including YOLO.\] \
 \[Updated on 2018-12-27: Add [bbox regression](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/#bounding-box-regression) and [tricks](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/#common-tricks) sections for R-CNN.\]

In the series of “Object Detection for Dummies”, we started with basic concepts in image processing, such as gradient vectors and HOG, in [Part 1](https://lilianweng.github.io/posts/2017-10-29-object-recognition-part-1/). Then we introduced classic convolutional neural network architecture designs for classification and pioneer models for object recognition, Overfeat and DPM, in [Part 2](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/). In the third post of this series, we are about to review a set of models in the R-CNN (“Region-based CNN”) family.

Links to all the posts in the series: \[[Part 1](https://lilianweng.github.io/posts/2017-10-29-object-recognition-part-1/)\] \[[Part 2](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/)\] \[[Part 3](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/)\] \[[Part 4](https://lilianweng.github.io/posts/2018-12-27-object-recognition-part-4/)\].

Here is a list of papers covered in this post ;)

| **Model**    | **Goal**           | **Resources**                                                                                           |
| ------------ | ------------------ | ------------------------------------------------------------------------------------------------------- |
| R-CNN        | Object recognition | \[[paper](https://arxiv.org/abs/1311.2524)\]\[[code](https://github.com/rbgirshick/rcnn)\]            |
| Fast R-CNN   | Object recognition | \[[paper](https://arxiv.org/abs/1504.08083)\]\[[code](https://github.com/rbgirshick/fast-rcnn)\]      |
| Faster R-CNN | Object recognition | \[[paper](https://arxiv.org/abs/1506.01497)\]\[[code](https://github.com/rbgirshick/py-faster-rcnn)\] |
| Mask R-CNN   | Image segmentation | \[[paper](https://arxiv.org/abs/1703.06870)\]\[[code](https://github.com/CharlesShang/FastMaskRCNN)\] |

R-CNN ([Girshick et al., 2014](https://arxiv.org/abs/1311.2524)) is short for “Region-based Convolutional Neural Networks”. The main idea is composed of two steps. First, using [selective search](https://lilianweng.github.io/posts/2017-10-29-object-recognition-part-1/#selective-search), it identifies a manageable number of bounding-box object region candidates (“region of interest” or “RoI”). And then it extracts CNN features from each region independently for classification.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/RCNN.png)\
The architecture of R-CNN. (Image source: [Girshick et al., 2014](https://arxiv.org/abs/1311.2524))

## Model Workflow

How R-CNN works can be summarized as follows:

1. **Pre-train** a CNN network on image classification tasks; for example, VGG or ResNet trained on [ImageNet](http://image-net.org/index) dataset. The classification task involves N classes.

> NOTE: You can find a pre-trained [AlexNet](https://github.com/BVLC/caffe/tree/master/models/bvlc_alexnet) in Caffe Model [Zoo](https://github.com/caffe2/caffe2/wiki/Model-Zoo). I don’t think you can [find it](https://github.com/tensorflow/models/issues/1394) in Tensorflow, but Tensorflow-slim model [library](https://github.com/tensorflow/models/tree/master/research/slim) provides pre-trained ResNet, VGG, and others.

2. Propose category-independent regions of interest by selective search (~2k candidates per image). Those regions may contain target objects and they are of different sizes.
3. Region candidates are **warped** to have a fixed size as required by CNN.
4. Continue fine-tuning the CNN on warped proposal regions for K + 1 classes; The additional one class refers to the background (no object of interest). In the fine-tuning stage, we should use a much smaller learning rate and the mini-batch oversamples the positive cases because most proposed regions are just background.
5. Given every image region, one forward propagation through the CNN generates a feature vector. This feature vector is then consumed by a **binary SVM** trained for **each class** independently. \
    The positive samples are proposed regions with IoU (intersection over union) overlap threshold >= 0.3, and negative samples are irrelevant others.
6. To reduce the localization errors, a regression model is trained to correct the predicted detection window on bounding box correction offset using CNN features.

## Bounding Box Regression

Given a predicted bounding box coordinate $\\mathbf{p} = (p\_x, p\_y, p\_w, p\_h)$ (center coordinate, width, height) and its corresponding ground truth box coordinates $\\mathbf{g} = (g\_x, g\_y, g\_w, g\_h)$ , the regressor is configured to learn scale-invariant transformation between two centers and log-scale transformation between widths and heights. All the transformation functions take $\\mathbf{p}$ as input.

$$ \\begin{aligned} \\hat{g}\_x &= p\_w d\_x(\\mathbf{p}) + p\_x \\\\ \\hat{g}\_y &= p\_h d\_y(\\mathbf{p}) + p\_y \\\\ \\hat{g}\_w &= p\_w \\exp({d\_w(\\mathbf{p})}) \\\\ \\hat{g}\_h &= p\_h \\exp({d\_h(\\mathbf{p})}) \\end{aligned} $$

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/RCNN-bbox-regression.png)\
Illustration of transformation between predicted and ground truth bounding boxes.

An obvious benefit of applying such transformation is that all the bounding box correction functions, $d\_i(\\mathbf{p})$ where $i \\in \\{ x, y, w, h \\}$, can take any value between \[-∞, +∞\]. The targets for them to learn are:

$$ \\begin{aligned} t\_x &= (g\_x - p\_x) / p\_w \\\\ t\_y &= (g\_y - p\_y) / p\_h \\\\ t\_w &= \\log(g\_w/p\_w) \\\\ t\_h &= \\log(g\_h/p\_h) \\end{aligned} $$

A standard regression model can solve the problem by minimizing the SSE loss with regularization:

$$ \\mathcal{L}\_\\text{reg} = \\sum\_{i \\in \\{x, y, w, h\\}} (t\_i - d\_i(\\mathbf{p}))^2 + \\lambda \\|\\mathbf{w}\\|^2 $$

The regularization term is critical here and RCNN paper picked the best λ by cross validation. It is also noteworthy that not all the predicted bounding boxes have corresponding ground truth boxes. For example, if there is no overlap, it does not make sense to run bbox regression. Here, only a predicted box with a nearby ground truth box with at least 0.6 IoU is kept for training the bbox regression model.

## Common Tricks

Several tricks are commonly used in RCNN and other detection models.

**Non-Maximum Suppression**

Likely the model is able to find multiple bounding boxes for the same object. Non-max suppression helps avoid repeated detection of the same instance. After we get a set of matched bounding boxes for the same object category: Sort all the bounding boxes by confidence score. Discard boxes with low confidence scores. *While* there is any remaining bounding box, repeat the following: Greedily select the one with the highest score. Skip the remaining boxes with high IoU (i.e. > 0.5) with previously selected one.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/non-max-suppression.png)\
Multiple bounding boxes detect the car in the image. After non-maximum suppression, only the best remains and the rest are ignored as they have large overlaps with the selected one. (Image source: [DPM paper](http://lear.inrialpes.fr/~oneata/reading_group/dpm.pdf))

**Hard Negative Mining**

We consider bounding boxes without objects as negative examples. Not all the negative examples are equally hard to be identified. For example, if it holds pure empty background, it is likely an “*easy negative*”; but if the box contains weird noisy texture or partial object, it could be hard to be recognized and these are “*hard negative*”.

The hard negative examples are easily misclassified. We can explicitly find those false positive samples during the training loops and include them in the training data so as to improve the classifier.

## Speed Bottleneck

Looking through the R-CNN learning steps, you could easily find out that training an R-CNN model is expensive and slow, as the following steps involve a lot of work:

- Running selective search to propose 2000 region candidates for every image;
- Generating the CNN feature vector for every image region (N images \* 2000).
- The whole process involves three models separately without much shared computation: the convolutional neural network for image classification and feature extraction; the top SVM classifier for identifying target objects; and the regression model for tightening region bounding boxes.

## Fast R-CNN

To make R-CNN faster, Girshick ([2015](https://arxiv.org/pdf/1504.08083.pdf)) improved the training procedure by unifying three independent models into one jointly trained framework and increasing shared computation results, named **Fast R-CNN**. Instead of extracting CNN feature vectors independently for each region proposal, this model aggregates them into one CNN forward pass over the entire image and the region proposals share this feature matrix. Then the same feature matrix is branched out to be used for learning the object classifier and the bounding-box regressor. In conclusion, computation sharing speeds up R-CNN.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/fast-RCNN.png)\
The architecture of Fast R-CNN. (Image source: [Girshick, 2015](https://arxiv.org/pdf/1504.08083.pdf))

## RoI Pooling

It is a type of max pooling to convert features in the projected region of the image of any size, h x w, into a small fixed window, H x W. The input region is divided into H x W grids, approximately every subwindow of size h/H x w/W. Then apply max-pooling in each grid.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/roi-pooling.png)\
RoI pooling (Image source: [Stanford CS231n slides](http://cs231n.stanford.edu/slides/2016/winter1516_lecture8.pdf).)

## Model Workflow

How Fast R-CNN works is summarized as follows; many steps are same as in R-CNN:

1. First, pre-train a convolutional neural network on image classification tasks.
2. Propose regions by selective search (~2k candidates per image).
3. Alter the pre-trained CNN:
   - Replace the last max pooling layer of the pre-trained CNN with a [RoI pooling](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/#roi-pooling) layer. The RoI pooling layer outputs fixed-length feature vectors of region proposals. Sharing the CNN computation makes a lot of sense, as many region proposals of the same images are highly overlapped.
   - Replace the last fully connected layer and the last softmax layer (K classes) with a fully connected layer and softmax over K + 1 classes.
4. Finally the model branches into two output layers:
   - A softmax estimator of K + 1 classes (same as in R-CNN, +1 is the “background” class), outputting a discrete probability distribution per RoI.
   - A bounding-box regression model which predicts offsets relative to the original RoI for each of K classes.

## Loss Function

The model is optimized for a loss combining two tasks (classification + localization):

| **Symbol** | **Explanation** | | $u$ | True class label, $ u \\in 0, 1, \\dots, K$; by convention, the catch-all background class has $u = 0$. | | $p$ | Discrete probability distribution (per RoI) over K + 1 classes: $p = (p\_0, \\dots, p\_K)$, computed by a softmax over the K + 1 outputs of a fully connected layer. | | $v$ | True bounding box $ v = (v\_x, v\_y, v\_w, v\_h) $. | | $t^u$ | Predicted bounding box correction, $t^u = (t^u\_x, t^u\_y, t^u\_w, t^u\_h)$. See [above](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/#bounding-box-regression). | {:.info}

The loss function sums up the cost of classification and bounding box prediction: $\\mathcal{L} = \\mathcal{L}\_\\text{cls} + \\mathcal{L}\_\\text{box}$. For “background” RoI, $\\mathcal{L}\_\\text{box}$ is ignored by the indicator function $\\mathbb{1} \[u \\geq 1\]$, defined as:

$$ \\mathbb{1} \[u >= 1\] = \\begin{cases} 1 & \\text{if } u \\geq 1\\\\ 0 & \\text{otherwise} \\end{cases} $$

The overall loss function is:

$$ \\begin{align\*} \\mathcal{L}(p, u, t^u, v) &= \\mathcal{L}\_\\text{cls} (p, u) + \\mathbb{1} \[u \\geq 1\] \\mathcal{L}\_\\text{box}(t^u, v) \\\\ \\mathcal{L}\_\\text{cls}(p, u) &= -\\log p\_u \\\\ \\mathcal{L}\_\\text{box}(t^u, v) &= \\sum\_{i \\in \\{x, y, w, h\\}} L\_1^\\text{smooth} (t^u\_i - v\_i) \\end{align\*} $$

The bounding box loss $\\mathcal{L}\_{box}$ should measure the difference between $t^u\_i$ and $v\_i$ using a **robust** loss function. The [smooth L1 loss](https://github.com/rbgirshick/py-faster-rcnn/files/764206/SmoothL1Loss.1.pdf) is adopted here and it is claimed to be less sensitive to outliers.

$$ L\_1^\\text{smooth}(x) = \\begin{cases} 0.5 x^2 & \\text{if } \\vert x \\vert < 1\\\\ \\vert x \\vert - 0.5 & \\text{otherwise} \\end{cases} $$

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/l1-smooth.png)\
The plot of smooth L1 loss, $y = L\\\_1^\\text{smooth}(x)$. (Image source: [link](https://github.com/rbgirshick/py-faster-rcnn/files/764206/SmoothL1Loss.1.pdf))

## Speed Bottleneck

Fast R-CNN is much faster in both training and testing time. However, the improvement is not dramatic because the region proposals are generated separately by another model and that is very expensive.

## Faster R-CNN

An intuitive speedup solution is to integrate the region proposal algorithm into the CNN model. **Faster R-CNN** ([Ren et al., 2016](https://arxiv.org/pdf/1506.01497.pdf)) is doing exactly this: construct a single, unified model composed of RPN (region proposal network) and fast R-CNN with shared convolutional feature layers.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/faster-RCNN.png)\
An illustration of Faster R-CNN model. (Image source: [Ren et al., 2016](https://arxiv.org/pdf/1506.01497.pdf))

## Model Workflow

1. Pre-train a CNN network on image classification tasks.
2. Fine-tune the RPN (region proposal network) end-to-end for the region proposal task, which is initialized by the pre-train image classifier. Positive samples have IoU (intersection-over-union) > 0.7, while negative samples have IoU < 0.3.
   - Slide a small n x n spatial window over the conv feature map of the entire image.
   - At the center of each sliding window, we predict multiple regions of various scales and ratios simultaneously. An anchor is a combination of (sliding window center, scale, ratio). For example, 3 scales + 3 ratios => k=9 anchors at each sliding position.
3. Train a Fast R-CNN object detection model using the proposals generated by the current RPN
4. Then use the Fast R-CNN network to initialize RPN training. While keeping the shared convolutional layers, only fine-tune the RPN-specific layers. At this stage, RPN and the detection network have shared convolutional layers!
5. Finally fine-tune the unique layers of Fast R-CNN
6. Step 4-5 can be repeated to train RPN and Fast R-CNN alternatively if needed.

## Loss Function

Faster R-CNN is optimized for a multi-task loss function, similar to fast R-CNN.

| **Symbol** | **Explanation** | | $p\_i$ | Predicted probability of anchor i being an object. | | $p^\*\_i$ | Ground truth label (binary) of whether anchor i is an object. | | $t\_i$ | Predicted four parameterized coordinates. | | $t^\*\_i$ | Ground truth coordinates. | | $N\_\\text{cls}$ | Normalization term, set to be mini-batch size (~256) in the paper. | | $N\_\\text{box}$ | Normalization term, set to the number of anchor locations (~2400) in the paper. | | $\\lambda$ | A balancing parameter, set to be ~10 in the paper (so that both $\\mathcal{L}\_\\text{cls}$ and $\\mathcal{L}\_\\text{box}$ terms are roughly equally weighted). | {:.info}

The multi-task loss function combines the losses of classification and bounding box regression:

$$ \\begin{align\*} \\mathcal{L} &= \\mathcal{L}\_\\text{cls} + \\mathcal{L}\_\\text{box} \\\\ \\mathcal{L}(\\{p\_i\\}, \\{t\_i\\}) &= \\frac{1}{N\_\\text{cls}} \\sum\_i \\mathcal{L}\_\\text{cls} (p\_i, p^\*\_i) + \\frac{\\lambda}{N\_\\text{box}} \\sum\_i p^\*\_i \\cdot L\_1^\\text{smooth}(t\_i - t^\*\_i) \\\\ \\end{align\*} $$

where $\\mathcal{L}\_\\text{cls}$ is the log loss function over two classes, as we can easily translate a multi-class classification into a binary classification by predicting a sample being a target object versus not. $L\_1^\\text{smooth}$ is the smooth L1 loss.

$$ \\mathcal{L}\_\\text{cls} (p\_i, p^\*\_i) = - p^\*\_i \\log p\_i - (1 - p^\*\_i) \\log (1 - p\_i) $$

## Mask R-CNN

Mask R-CNN ([He et al., 2017](https://arxiv.org/pdf/1703.06870.pdf)) extends Faster R-CNN to pixel-level [image segmentation](https://lilianweng.github.io/posts/2017-10-29-object-recognition-part-1/#image-segmentation-felzenszwalbs-algorithm). The key point is to decouple the classification and the pixel-level mask prediction tasks. Based on the framework of [Faster R-CNN](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/#faster-r-cnn), it added a third branch for predicting an object mask in parallel with the existing branches for classification and localization. The mask branch is a small fully-connected network applied to each RoI, predicting a segmentation mask in a pixel-to-pixel manner.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/mask-rcnn.png)\
Mask R-CNN is Faster R-CNN model with image segmentation. (Image source: [He et al., 2017](https://arxiv.org/pdf/1703.06870.pdf))

Because pixel-level segmentation requires much more fine-grained alignment than bounding boxes, mask R-CNN improves the RoI pooling layer (named “RoIAlign layer”) so that RoI can be better and more precisely mapped to the regions of the original image.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/mask-rcnn-examples.png)\
Predictions by Mask R-CNN on COCO test set. (Image source: [He et al., 2017](https://arxiv.org/pdf/1703.06870.pdf))

## RoIAlign

The RoIAlign layer is designed to fix the location misalignment caused by quantization in the RoI pooling. RoIAlign removes the hash quantization, for example, by using x/16 instead of \[x/16\], so that the extracted features can be properly aligned with the input pixels. [Bilinear interpolation](https://en.wikipedia.org/wiki/Bilinear_interpolation) is used for computing the floating-point location values in the input.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/roi-align.png)\
A region of interest is mapped \*\*accurately\*\* from the original image onto the feature map without rounding up to integers. (Image source: [link](https://blog.athelas.com/a-brief-history-of-cnns-in-image-segmentation-from-r-cnn-to-mask-r-cnn-34ea83205de4))

## Loss Function

The multi-task loss function of Mask R-CNN combines the loss of classification, localization and segmentation mask: $ \\mathcal{L} = \\mathcal{L}\_\\text{cls} + \\mathcal{L}\_\\text{box} + \\mathcal{L}\_\\text{mask}$, where $\\mathcal{L}\_\\text{cls}$ and $\\mathcal{L}\_\\text{box}$ are same as in Faster R-CNN.

The mask branch generates a mask of dimension m x m for each RoI and each class; K classes in total. Thus, the total output is of size $K \\cdot m^2$. Because the model is trying to learn a mask for each class, there is no competition among classes for generating masks.

$\\mathcal{L}\_\\text{mask}$ is defined as the average binary cross-entropy loss, only including k-th mask if the region is associated with the ground truth class k.

$$ \\mathcal{L}\_\\text{mask} = - \\frac{1}{m^2} \\sum\_{1 \\leq i, j \\leq m} \\big\[ y\_{ij} \\log \\hat{y}^k\_{ij} + (1-y\_{ij}) \\log (1- \\hat{y}^k\_{ij}) \\big\] $$

where $y\_{ij}$ is the label of a cell (i, j) in the true mask for the region of size m x m; $\\hat{y}\_{ij}^k$ is the predicted value of the same cell in the mask learned for the ground-truth class k.

## Summary of Models in the R-CNN family

Here I illustrate model designs of R-CNN, Fast R-CNN, Faster R-CNN and Mask R-CNN. You can track how one model evolves to the next version by comparing the small differences.

![](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/rcnn-family-summary.png)

* * *

Cited as:

```
@article{weng2017detection3,
  title   = "Object Detection for Dummies Part 3: R-CNN Family",
  author  = "Weng, Lilian",
  journal = "lilianweng.github.io",
  year    = "2017",
  url     = "https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/"
}
```

## Reference

\[1\] Ross Girshick, Jeff Donahue, Trevor Darrell, and Jitendra Malik. [“Rich feature hierarchies for accurate object detection and semantic segmentation.”](https://www.cv-foundation.org/openaccess/content_cvpr_2014/papers/Girshick_Rich_Feature_Hierarchies_2014_CVPR_paper.pdf) In Proc. IEEE Conf. on computer vision and pattern recognition (CVPR), pp. 580-587. 2014.

\[2\] Ross Girshick. [“Fast R-CNN.”](https://arxiv.org/pdf/1504.08083.pdf) In Proc. IEEE Intl. Conf. on computer vision, pp. 1440-1448. 2015.

\[3\] Shaoqing Ren, Kaiming He, Ross Girshick, and Jian Sun. [“Faster R-CNN: Towards real-time object detection with region proposal networks.”](http://papers.nips.cc/paper/5638-faster-r-cnn-towards-real-time-object-detection-with-region-proposal-networks.pdf) In Advances in neural information processing systems (NIPS), pp. 91-99. 2015.

\[4\] Kaiming He, Georgia Gkioxari, Piotr Dollár, and Ross Girshick. [“Mask R-CNN.”](https://arxiv.org/pdf/1703.06870.pdf) arXiv preprint arXiv:1703.06870, 2017.

\[5\] Joseph Redmon, Santosh Divvala, Ross Girshick, and Ali Farhadi. [“You only look once: Unified, real-time object detection.”](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Redmon_You_Only_Look_CVPR_2016_paper.pdf) In Proc. IEEE Conf. on computer vision and pattern recognition (CVPR), pp. 779-788. 2016.

\[6\] [“A Brief History of CNNs in Image Segmentation: From R-CNN to Mask R-CNN”](https://blog.athelas.com/a-brief-history-of-cnns-in-image-segmentation-from-r-cnn-to-mask-r-cnn-34ea83205de4) by Athelas.

\[7\] Smooth L1 Loss: [https://github.com/rbgirshick/py-faster-rcnn/files/764206/SmoothL1Loss.1.pdf](https://github.com/rbgirshick/py-faster-rcnn/files/764206/SmoothL1Loss.1.pdf)
