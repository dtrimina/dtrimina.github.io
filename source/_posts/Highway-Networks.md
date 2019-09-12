---
title: Highway Networks
tags:
  - Classification
categories:
  - Classification
date: 2019-05-31 14:43:35
---


paper: [Highway Networks](https://arxiv.org/pdf/1505.00387.pdf)  

It is well known that deep networks can represent certain function classes exponentially more efficiently than shallow ones. However, network training becomes more difficult with increasing depth and training of very deep networks. Inspired by LSTM, highway networks actually utilize the gating mechanism to pass information almost unchanged through many layers.


### **Architecture** 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Highway_Networks/3.jpg" height="90%" width="90%"></div>  
the transform gate defined as: 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Highway_Networks/Tx.jpg" height="50%" width="50%"></div> 
so: 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Highway_Networks/4.jpg" height="50%" width="50%"></div> 

- H(x) and T(x) are the same dimension. 
- use a plain layer (without highways) to change dimensionality and then continue with stacking highway layers. 
- **b<sub>T</sub>** can be initialized with a negative value (e.g. -1, -3 etc.) 
- highway networks performs better than plain networks when networks go deeper. 


