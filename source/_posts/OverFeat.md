---
title: OverFeat
tags: 
- Classification  
- Localization  
- Detection
categories:
  - Classification
date: 2019-05-21 14:28:59
---

paper: [OverFeat: Integrated Recognition, Localization and Detection using Convolutional Networks](https://arxiv.org/pdf/1312.6229.pdf)

Classification, localization and detection are three computer vision tasks.The paper proposes a new integrated approach to these tasks with a singleConvNet.

### **Contributions**
1. use a single shared network to learn different tasks
2. a feature extractor called OverFeat

### **Vision tasks from easy to hard**
- classification: each image is assigned a single label corresponding to the main object in the image. Five guesses are allowed to find the correct answer. 
- localization: compare to classification, a bounding box for the predicted object must be returned with each guess.
- detection: differs from localization in that there can be any number of objects in each image (including zero), and false positives are penalized by the mean average precision(mAP) measure.

### **Classification**
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/OverFeat/fast_model.jpg" height="70%" width="70%">
</div>  
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/OverFeat/accurate_model.jpg" height="70%" width="70%">
</div>

Two models are provided, a fast(top) and accurate(down) one. The accurate model is more accurate than the fast one, however it requires nearly twice as many connections.  
This model is similar to [AlexNet](https://blog.dtrimina.cn/CNNs-for-image-classification/AlexNet/), but with the following differences: (i) no contrast normalization is used; (ii) pooling regions are non-overlapping; (iii) larger 1st and 2nd layer feature maps, thanks to a smaller stride (2 instead of 4).  
For classification, we use accurate model. There are no special details in training, but using multi-scale for classification rather than averaging a fixed set of 10 views(4 cornersandcenter, with horizontal flip). Using the entire image by densely running the network at each location and at multiple scales yields more views for voting, which increases robustness while remaining **efficient with convnets**. The last fc layers are replaced by 1x1 convolution operations, **The entire ConvNet is then simply a sequence of convolutions, max-pooling and thresholding operations exclusively**. The main steps are as follows:   
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/OverFeat/steps_for_classification.jpg" height="60%" width="60%">
</div>  

1. As shown in the picture, suppose the size of unpooled layer 5 maps is 20x20x1024(1024 is number of channels);  
2. use 3x3 max pooling operation (non-overlapping regions), repeated 3x3 times for (∆x ,∆y) pixel offsets of {0,1,2}, then we get 3x3=9 maps with size 6x6x1024.  
3. The classifier can be seen as a box which has a fixed input size of 5x5 and produces a C-dimensional output vector for each location within the pooled maps. So 6x6 in and 2x2 out, we can get 3x3=9 maps with size 2x2xC.  
4. Then reshape the size to (2x3)x(2x3)xC=6x6xC, we get 36 outputs of classification.  
5. Then class-wise maxpooling, from 36xC to 1xC.
6. Then average the resulting C-dimensional vectors from different scales and flips and take the top-1 or top-5 elements from the mean class vector. different scales are shown as follows:  
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/OverFeat/diff_scales.jpg" height="70%" width="70%">
</div>  

### **Localization**  
Using classification-trained network and fixing the feature extraction layer(1-5), we add a regression network (just like classifier) and train it with L2 loss to predict object bounding boxes. We then combine the regression predictions with the classification results at each location.  
The regression network outputs 4 units which specify the edges of the bounding box. And use the same (∆x ,∆y) pixel offsets operatin similar to classification. Besides, the final regressor layer is class-specific, having 1000 different versions, one for each class.  
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/OverFeat/localization.jpg" height="40%" width="40%">
</div>  

Then predict the set of classes in the top k for 6 scales, and conbine predictions using a greedy merge strategy (conbine boxed with high overlap ratio). The final predictionis given by taking the merged bounding boxes with maximum class scores.  

### **Detection**
Similar to classification training but need to predict background, Multiple location of an image may be trained simultaneously.
