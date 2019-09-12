---
title: GoogLeNet
tags:
  - Classification
  - Detection
categories:
  - Classification
date: 2019-05-25 19:47:40
---


paper: [Going deeper with convolutions](https://arxiv.org/pdf/1409.4842.pdf)  

The most straightforward way of improving the performance of deep neural networks is by increasing both their depth and width. But it makes a larger number of parameters which makes the enlarged network more prone to overfitting and increases use of computational resources. The fundamental way of solving both issues would be by ultimately moving from fully connected to sparsely connected architectures. But todays computing infrastructures are very inefficient when it comes to numerical calculation on non-uniform sparse data structures. how about an architecture that makes use of the extra sparsity, even at filter level, as suggested by GoogLeNet. It's the winner of ILSVRC-2014 classification task.  

### **Keys**  
1. Inception module with dimension reduction 
2. Auxiliary classifiers

### **Inception module**
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v1/inception_module.jpg" height="50%" width="50%"></div>  

purpose of 1x1 conv in [NIN](https://blog.dtrimina.cn/CNNs-for-classification/NIN/): 
- increase the representational power of neural networks 
- dimension reduction 

visual information should be processed at various scales (1x1, 3x3, 5x5 conv) and then aggregated so that the next stage can abstract features from different scales simultaneously.  

### **GoogLeNet** 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v1/GoogLeNet_picture.png" height="70%" width="70%"></div>  

The network is 22 layers deep when counting only layers with parameters. The use of average pooling before the classifier which improves the performence is based on [NIN](https://blog.dtrimina.cn/CNNs-for-classification/NIN/).  Auxiliary classifiers put on top of the output of the Inception (4a) and (4d) modules. During training, their loss gets added to the total loss of the network with a discount weight to 0.3. At inference time, these auxiliary networks are discarded. The other details are in the paper. 

### **Training and Testing** 
no special training details but a set of techniques during testing to obtain a higher performance are as follows: 
- ensemble prediction using 7 models. 
- a more aggressive cropping approach than [AlexNet](https://blog.dtrimina.cn/CNNs-for-classification/AlexNet/). a) resize the image to 4 scales where the shorter dimension is 256, 288, 320 and 352 respectively; b) take the left, center and right square of these resized images (in the case of portrait images, we take the top, center and bottom squares); c) For each square, we then take the 4 corners and the center 224×224 crop as well as the square resized to 224×224, and their mirrored versions. This results in 4×3×6×2 = 144 crops per image. 
- The softmax probabilities are averaged over multiple crops and over all the individual classifiers to obtain the final prediction.

### **Detection** 
- Similar to R-CNN, but is augmented with the Inception model as the region classifier. 
- combining the Selective Search approach with multi-box predictions for higher object bounding box recall. 
- In order to cut down the number of false positives, the superpixel size was increased by 2×. 
- do not use bounding box regression due to lack of time.