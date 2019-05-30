---
title: ZFNet
tags: CNNs_for_classification
categories:
  - CNNs_for_classification
date: 2019-05-16 10:40:23
---

paper: [Visualizing and Understanding Convolutional Networks](https://cs.nyu.edu/~fergus/papers/zeilerECCV2014.pdf)

Larger labeled training sets, powerful GPU, better regularization strategies achieved good performance in detection, such as [AlexNet](https://blog.dtrimina.cn/CNNs-for-image-classification/AlexNet/). We want to know how and why they work. Let's look what happened in the network.

### **Contributions**

* use a multi-layered deconvolutional network to visialize the input stimuli that excite individual feature maps at any layer in the model
* performs a sensitivity analysis of the classifier output by occluding portions of the input image, revealing which parts of the scene are important for classification.
* discuss the arcitecture of the network based on AlexNet.

### **Architecture and Comparion with AlexNet**
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/ZFNet/ZFNet.jpg" height="80%" width="80%">
</div>

There are many similarities between ZFNet and AlexNet in Arcitecture and training details. Preprocessing, SGD optimizer, learning rate adjustment strategy, using dropout and so on, more details can be seen in [AlexNet](https://blog.dtrimina.cn/CNNs-for-image-classification/AlexNet/). While it initilize the network's weight to 0.01 and bias to 0. And the structure is different in the first layer with different kernel size and stride for better visialization. This change silghtly outperforms the arcitecture of AlexNet. Layers 3,4,5 are replaced with dense connections not sparse connections in 2 GPUs.


### **Visualization with a deconvnet**
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/ZFNet/deconv.jpg" height="50%" width="50%">
</div>

Here, deconvnets are not used in any learning capacity, just as a probe of an already trained connet.

Process: conv(learned filters) -> ReLU -> MaxPooling(need to save the location of the max value used in UnPooling) -> UnPooling -> ReLU -> deconv(use transposed versions of learned conv filters, this means flipping each filter vertically and horizontally.)

[This](https://www.zybuluo.com/lutingting/note/459569) and [this](https://medium.com/coinmonks/paper-review-of-zfnet-the-winner-of-ilsvlc-2013-image-classification-d1a5a0c45103) explain the deconve processing very well. More things about convolution is [A guide to convolution arithmetic for deep learning](https://arxiv.org/pdf/1603.07285.pdf).

### **Something found in visialization(images can be seen in the paper)**
* Feature Visialization: Layer 2 responds to corners and other edge/color conjunctions. Layer 3 has more complex invariances, capturing similar textures. Layer 4 shows significant variation, and is more class-specific: dog faces; bird’s legs. Layer 5 shows entire objects with significant pose variation.
* Feature Evolution during Training: The lower layers of the model can be seen to converge within a few epochs. However,
the upper layers only develop develop after a considerable number of epochs (40-50), demonstrating the need to let the models train until fully converged.
* Occlusion Sensitivity:  the model is localizing the objects within the scene, as the probability of the correct class drops significantly when the object is occluded. When occluder covers the image region that appears in the visualization, we see a strong drop in activity in the feature map.

### **Arcitecture Analysis**
* Removing the fully connected layers (6,7) or  two of the middle convolutional layers makes a relatively small difference to the error rate. However, removing both the middle convolution layers and the fully connected layers yields a model with only 4 layers whose performance is dramatically worse. This would suggest that the overall depth of the model is important for obtaining good performance.
* while making network fatter(increasing the size of middle conv layers and fc layers), it improves slightly and has the risk of overfitting.
* Using **pre-trained model** then fine tuning the softmax layer is much more better than model trained from beginning on the new set, shows the power of ImageNet feature extractor.

### **Discussion**
* **visualize what your network learned** can be used to identify problems and so obtain better results.
* use **models pre-trained** in large datasets, such as ImageNet