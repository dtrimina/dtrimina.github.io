---
title: SPP-net
tags:
  - Classification
  - Detection
categories:
  - Classification
mathjax: true
date: 2019-06-19 15:28:40
---


paper: [Spatial Group-wise Enhance: Improving Semantic Feature Learning in Convolutional Networks](https://arxiv.org/pdf/1905.09646.pdf)


Because of fc layers, CNNs require a fixed input size. We can solve it by replacing fc layers with convolutional layers and then averaging which is used in [OverFeat](https://blog.dtrimina.cn/CNNs-for-classification/OverFeat/). Or using SPP layer in this paper.

### **How SPP layer work**
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/SPP-net/spp_layer.png" height="70%" width="70%"></div>  

It uses multi-level spatial bins to get fixed-lenght vectors, and then feed the fixed vector to fc layer.  
SPP layer can be used for classic CNN models by replaceing the last pooling layer with SPP layer as baseline.  

### **SPP-net for Detection**
[R-CNN](https://blog.csdn.net/Andrewseu/article/details/67632480) costs much time for feature extraction, because CNN extracts features for each region proposal of 2k~ obtained by selective search.  
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/SPP-net/RCNN_vs_SPP.png" height="50%" width="50%"></div> 
SPP-net-based system computes features 24-102× faster than R-CNN. It can run the convolutional layers only once on the entire image (regardless of the number of windows), and then extract features by SPP-net on the feature maps.
