---
title: NIN
tags:
  - CNNs_for_classification
categories:
  - CNNs_for_classification
date: 2019-05-25 09:23:49
---

paper: [Network In Network](https://arxiv.org/pdf/1312.4400.pdf)  

### **Contributions** 
1. mlpconv layer 
2. global average pooling 

### **Architecture** 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/NIN/NIN.jpg" height="60%" width="60%"></div>  

The cross channel parametric pooling layer can be seen as **a convolution layer with 1x1 convolution kernel**. 

##### Why using multilayer perceptron ?  
- multilayer perceptron is compatible with the structure of convolutional neural networks, which is trained using back-propagation. 
- multilayer perceptron can be a deep model itself, which is consistent with the spirit of feature re-use. 

##### Why global average pooling ?  
- it is more native to the convolution structure by enforcing correspondences between feature maps and categories. 
- there is no parameter to optimize in the global average pooling thus overfitting is avoided at this layer. 
- global average pooling sums out the spatial information, thus it is more robust to spatial translations of the input.

Other details are in the paper. 