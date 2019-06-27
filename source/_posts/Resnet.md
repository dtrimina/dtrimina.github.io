---
title: Resnet
tags:
  - CNNs_for_classification
categories:
  - CNNs_for_classification
mathjax: true
date: 2019-06-27 19:35:04
---


paper: [Deep Residual Learning for Image Recognition](http://xxx.itp.ac.cn/pdf/1512.03385)  

### **degradation problem**
what is the problem ?  
with the network depth increasing, accuracy gets saturated (which might be unsurprising) and then degrades rapidly, just as shown below. such degradation is **not caused by overfitting**.  
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Resnet/degradation_problem.png" height="70%" width="70%"></div>  

### **Architectures**
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Resnet/resblock.jpg" height="70%" width="70%"></div>  
Above are residual blocks, left is used for shallow networks, right is used for deeper networks.  
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Resnet/resnet_picture.png" height="100%" width="100%"></div>
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Resnet/resnets.jpg" height="70%" width="70%"></div> 

- plain version is similar to [VGG](https://blog.dtrimina.cn/CNNs-for-classification/VGG/), but performs downsampling directly by convolutional layers that have a stride of 2 and ends with a global average pooling and softmax. 
- [batch normalization](https://blog.dtrimina.cn/CNNs-for-classification/Batch-Normalization/) is used after conv. ReLU activation function is used. 
- residual block can solve the degradation problem, and can make model deeper.
