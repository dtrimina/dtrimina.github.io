---
title: Batch-Normalization
tags:
  - CNNs_for_classification
categories:
  - CNNs_for_classification
date: 2019-05-30 14:05:53
---


paper: [Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift](https://arxiv.org/pdf/1502.03167v3.pdf)  


**internal covariate shift problem**: the distribution of each layer’s inputs changes during training, as the parameters of the previous layers change. This slows down the training by requiring lower learning rates and careful parameter initialization, and makes it notoriously hard to train models with saturating nonlinearities.  


### **Algorithm** 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Batch_Normalization/BN.jpg" height="60%" width="60%"></div>  

Normalize each scalar feature independently,by making it have the mean of zero and the variance of 1. but **this may change what the layer can represent**. To address this, it should make sure that the transformation inserted in the network can represent the identity transform. γ and β which can scale and shift the normalized value are to be learned.

### **How it work** 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Batch_Normalization/why_BN.png" height="60%" width="60%"></div>  

- allowing us to use much **higher learning rates** without the risk of divergence can make learning faster. 
- regularizes the model and reduces the need for Dropout 
- make it possible to use saturating nonlinearities by preventing the network from getting stuck in the saturated modes

### **Batch-Normalized Convolutional Networks** 
- the bias b can be ignoredsince its effect will be canceled by the subsequent mean subtraction. Thus, `z = g(W*u + b)` is replaced with `z = g(BN(W*u))` 
- suppose input feature map size is (batch_size, channel, weight, height), it's a channel-wise normalization. 

