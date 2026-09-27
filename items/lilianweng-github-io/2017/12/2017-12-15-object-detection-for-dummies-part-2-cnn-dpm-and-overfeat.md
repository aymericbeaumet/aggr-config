---
title: 'Object Detection for Dummies Part 2: CNN, DPM and Overfeat'
link: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/
source: lilianweng-github-io
published: 2017-12-15T00:00:00Z
updated: 2017-12-15T00:00:00Z
first_seen: 2026-09-27T19:29:15.293681927Z
summary: 'Part 1 of the “Object Detection for Dummies” series introduced: (1) the concept of image gradient vector and how HOG algorithm summarizes the information across all the gradient vectors in one image; (2) how the image segmentation algorithm works to detect regions that potentially contain objects; (3) how the Selective Search algorithm refines the outcomes of image segmentation for better region proposal.'
content: extracted
html: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.html
preview:
  file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.preview-3c2604b0c684.webp
  width: 256
  height: 154
  color: '#ececef'
images:
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/convolution-operation.png
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-c48a559ce4d5.png
    width: 364
    height: 219
  variants:
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-492dc2306e28.webp
    width: 364
    height: 219
  color: '#fcfcfc'
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/numerical_no_padding_no_strides.gif
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-3d8d75847628.gif
    width: 337
    height: 219
  color: '#fefefe'
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/numerical_padding_strides.gif
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-7a1e06de82dd.gif
    width: 396
    height: 278
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/alex_net_illustration.png
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-3959c15a9754.png
    width: 1900
    height: 876
  variants:
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-9213a7fff93b.webp
    width: 320
    height: 148
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-95c14209d04e.webp
    width: 640
    height: 295
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-44b52f463402.webp
    width: 960
    height: 443
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-30a731934616.webp
    width: 1280
    height: 590
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-a4d0fbe51582.webp
    width: 1600
    height: 738
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-1ff7245f73e8.webp
    width: 1900
    height: 876
  color: '#fafafa'
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/residual-block.png
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-6cc6a95b26c4.png
    width: 2388
    height: 992
  color: '#fdfcfc'
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/DPM.png
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-0b0993b71dcd.png
    width: 1999
    height: 619
  variants:
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-51280f8425ab.webp
    width: 320
    height: 99
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-d5b6ff904b64.webp
    width: 640
    height: 198
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-fbf697c09377.webp
    width: 960
    height: 297
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-8f45fb723932.webp
    width: 1280
    height: 396
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-6dea6b0d25c8.webp
    width: 1600
    height: 495
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-2c8f2e2e7940.webp
    width: 1999
    height: 619
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/DPM-matching.png
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-c32f4c05b93c.png
    width: 1158
    height: 1384
  variants:
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-0753628ca3e1.webp
    width: 320
    height: 382
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-1e999a20ac53.webp
    width: 640
    height: 765
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-32d875bbdcb8.webp
    width: 960
    height: 1147
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-6e228693b9e2.webp
    width: 1158
    height: 1384
  color: '#fdfdfd'
- source: https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/overfeat-training.png
  original:
    file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-8827b4ef4f0a.png
    width: 980
    height: 310
  variants:
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-a3822368b3e3.webp
    width: 320
    height: 101
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-153efe026d45.webp
    width: 640
    height: 202
  - file: 2017-12-15-object-detection-for-dummies-part-2-cnn-dpm-and-overfeat.image-05f3070e1cac.webp
    width: 980
    height: 310
  color: '#fefefe'
---

[Part 1](https://lilianweng.github.io/posts/2017-10-29-object-recognition-part-1/) of the “Object Detection for Dummies” series introduced: (1) the concept of image gradient vector and how HOG algorithm summarizes the information across all the gradient vectors in one image; (2) how the image segmentation algorithm works to detect regions that potentially contain objects; (3) how the Selective Search algorithm refines the outcomes of image segmentation for better region proposal.

In Part 2, we are about to find out more on the classic convolution neural network architectures for image classification. They lay the ***foundation*** for further progress on the deep learning models for object detection. Go check [Part 3](https://lilianweng.github.io/posts/2017-12-31-object-recognition-part-3/) if you want to learn more on R-CNN and related models.

CNN, short for “**Convolutional Neural Network**”, is the go-to solution for computer vision problems in the deep learning world. It was, to some extent, [inspired](https://lilianweng.github.io/posts/2017-06-21-overview/#convolutional-neural-network) by how human visual cortex system works.

I strongly recommend this [guide](https://arxiv.org/pdf/1603.07285.pdf) to convolution arithmetic, which provides a clean and solid explanation with tons of visualizations and examples. Here let’s focus on two-dimensional convolution as we are working with images in this post.

In short, convolution operation slides a predefined [kernel](https://en.wikipedia.org/wiki/Kernel_\(image_processing\)) (also called “filter”) on top of the input feature map (matrix of image pixels), multiplying and adding the values of the kernel and partial input features to generate the output. The values form an output matrix, as usually, the kernel is much smaller than the input image.

Figure 2 showcases two real examples of how to convolve a 3x3 kernel over a 5x5 2D matrix of numeric values to generate a 3x3 matrix. By controlling the padding size and the stride length, we can generate an output matrix of a certain size.

![](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/numerical_no_padding_no_strides.gif)

![](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/numerical_padding_strides.gif)\
Two examples of 2D convolution operation: (top) no padding and 1x1 strides; (bottom) 1x1 border zeros padding and 2x2 strides. (Image source: [deeplearning.net](http://deeplearning.net/software/theano_versions/dev/tutorial/conv_arithmetic.html))

## AlexNet (Krizhevsky et al, 2012)

- 5 convolution \[+ optional max pooling\] layers + 2 MLP layers + 1 LR layer
- Use data augmentation techniques to expand the training dataset, such as image translations, horizontal reflections, and patch extractions.

![](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/alex_net_illustration.png)\
The architecture of AlexNet. (Image source: [link](http://vision03.csail.mit.edu/cnn_art/index.html))

## VGG (Simonyan and Zisserman, 2014)

- The network is considered as “very deep” at its time; 19 layers
- The architecture is extremely simplified with only 3x3 convolutional layers and 2x2 pooling layers. The stacking of small filters simulates a larger filter with fewer parameters.

## ResNet (He et al., 2015)

- The network is indeed very deep; 152 layers of simple architecture.
- **Residual Block**: Some input of a certain layer can be passed to the component two layers later. Residual blocks are essential for keeping a deep network trainable and eventually work. Without residual blocks, the training loss of a plain network does not monotonically decrease as the number of layers increases due to [vanishing and exploding gradients](http://www.wildml.com/2015/10/recurrent-neural-networks-tutorial-part-3-backpropagation-through-time-and-vanishing-gradients/).

![](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/residual-block.png)\
An illustration of the residual block of ResNet. In some way, we can say the design of residual blocks is inspired by V4 getting input directly from V1 in the human visual cortex system. (left image source: [Wang et al., 2017](https://arxiv.org/pdf/1312.6229.pdf))

## Evaluation Metrics: mAP

A common evaluation metric used in many object recognition and detection tasks is “**mAP**”, short for “**mean average precision**”. It is a number from 0 to 100; higher value is better.

- Combine all detections from all test images to draw a precision-recall curve (PR curve) for each class; The “average precision” (AP) is the area under the PR curve.
- Given that target objects are in different classes, we first compute AP separately for each class, and then average over classes.
- A detection is a true positive if it has **“intersection over union” (IoU)** with a ground-truth box greater than some threshold (usually 0.5; if so, the metric is “[mAP@0.5](mailto:mAP@0.5)”)

## Deformable Parts Model

The Deformable Parts Model (DPM) ([Felzenszwalb et al., 2010](http://people.cs.uchicago.edu/~pff/papers/lsvm-pami.pdf)) recognizes objects with a mixture graphical model (Markov random fields) of deformable parts. The model consists of three major components:

1. A coarse ***root filter*** defines a detection window that approximately covers an entire object. A filter specifies weights for a region feature vector.
2. Multiple ***part filters*** that cover smaller parts of the object. Parts filters are learned at twice resolution of the root filter.
3. A ***spatial model*** for scoring the locations of part filters relative to the root.

![](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/DPM.png)\
The DPM model contains (a) a root filter, (b) multiple part filters at twice the resolution, and (c) a model for scoring the location and deformation of parts.

The quality of detecting an object is measured by the score of filters minus the deformation costs. The matching score $f$, in laymen’s terms, is:

$$ f(\\text{model}, x) = f(\\beta\_\\text{root}, x) + \\sum\_{\\beta\_\\text{part} \\in \\text{part filters}} \\max\_y \[f(\\beta\_\\text{part}, y) - \\text{cost}(\\beta\_\\text{part}, x, y)\] $$

in which,

- $x$ is an image with a specified position and scale;
- $y$ is a sub region of $x$.
- $\\beta\_\\text{root}$ is the root filter.
- $\\beta\_\\text{part}$ is one part filter.
- cost() measures the penalty of the part deviating from its ideal location relative to the root.

The basic score model is the dot product between the filter $\\beta$ and the region feature vector $\\Phi(x)$: $f(\\beta, x) = \\beta \\cdot \\Phi(x)$. The feature set $\\Phi(x)$ can be defined by HOG or other similar algorithms.

A root location with high score detects a region with high chances to contain an object, while the locations of the parts with high scores confirm a recognized object hypothesis. The paper adopted latent SVM to model the classifier.

![](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/DPM-matching.png)\
The matching process by DPM. (Image source: [Felzenszwalb et al., 2010](http://people.cs.uchicago.edu/~pff/papers/lsvm-pami.pdf))

The author later claimed that DPM and CNN models are not two distinct approaches to object recognition. Instead, a DPM model can be formulated as a CNN by unrolling the DPM inference algorithm and mapping each step to an equivalent CNN layer. (Check the details in [Girshick et al., 2015](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Girshick_Deformable_Part_Models_2015_CVPR_paper.pdf)!)

Overfeat \[[paper](https://pdfs.semanticscholar.org/f2c2/fbc35d0541571f54790851de9fcd1adde085.pdf)\]\[[code](https://github.com/sermanet/OverFeat)\] is a pioneer model of integrating the object detection, localization and classification tasks all into one convolutional neural network. The main idea is to (i) do image classification at different locations on regions of multiple scales of the image in a sliding window fashion, and (ii) predict the bounding box locations with a regressor trained on top of the same convolution layers.

The Overfeat model architecture is very similar to [AlexNet](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/#alexnet-krizhevsky-et-al-2012). It is trained as follows:

![](https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/overfeat-training.png)\
The training stages of the Overfeat model. (Image source: [link](http://vision.stanford.edu/teaching/cs231b_spring1415/slides/overfeat_eric.pdf))

1. Train a CNN model (similar to AlexNet) on the image classification task.
2. Then, we replace the top classifier layers by a regression network and train it to predict object bounding boxes at each spatial location and scale. The regressor is class-specific, each generated for one image class.
   - Input: Images with classification and bounding box.
   - Output: $(x\_\\text{left}, x\_\\text{right}, y\_\\text{top}, y\_\\text{bottom})$, 4 values in total, representing the coordinates of the bounding box edges.
   - Loss: The regressor is trained to minimize $l2$ norm between generated bounding box and the ground truth for each training example.

At the detection time,

1. Perform classification at each location using the pretrained CNN model.
2. Predict object bounding boxes on all classified regions generated by the classifier.
3. Merge bounding boxes with sufficient overlap from localization and sufficient confidence of being the same object from the classifier.

* * *

Cited as:

```
@article{weng2017detection2,
  title   = "Object Detection for Dummies Part 2: CNN, DPM and Overfeat",
  author  = "Weng, Lilian",
  journal = "lilianweng.github.io",
  year    = "2017",
  url     = "https://lilianweng.github.io/posts/2017-12-15-object-recognition-part-2/"
}
```

## Reference

\[1\] Vincent Dumoulin and Francesco Visin. [“A guide to convolution arithmetic for deep learning.”](https://arxiv.org/pdf/1603.07285.pdf) arXiv preprint arXiv:1603.07285 (2016).

\[2\] Haohan Wang, Bhiksha Raj, and Eric P. Xing. [“On the Origin of Deep Learning.”](https://arxiv.org/pdf/1702.07800.pdf) arXiv preprint arXiv:1702.07800 (2017).

\[3\] Pedro F. Felzenszwalb, Ross B. Girshick, David McAllester, and Deva Ramanan. [“Object detection with discriminatively trained part-based models.”](http://people.cs.uchicago.edu/~pff/papers/lsvm-pami.pdf) IEEE transactions on pattern analysis and machine intelligence 32, no. 9 (2010): 1627-1645.

\[4\] Ross B. Girshick, Forrest Iandola, Trevor Darrell, and Jitendra Malik. [“Deformable part models are convolutional neural networks.”](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Girshick_Deformable_Part_Models_2015_CVPR_paper.pdf) In Proc. IEEE Conf. on Computer Vision and Pattern Recognition (CVPR), pp. 437-446. 2015.

\[5\] Sermanet, Pierre, David Eigen, Xiang Zhang, Michaël Mathieu, Rob Fergus, and Yann LeCun. [“OverFeat: Integrated Recognition, Localization and Detection using Convolutional Networks”](https://pdfs.semanticscholar.org/f2c2/fbc35d0541571f54790851de9fcd1adde085.pdf) arXiv preprint arXiv:1312.6229 (2013).
