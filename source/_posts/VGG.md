---
title: VGG
tags:
  - Classification
  - Localization
categories:
  - Classification
date: 2019-05-25 16:34:36
---


paper: [VERY DEEP CONVOLUTIONAL NETWORKS FOR LARGE-SCALE IMAGE RECOGNITION](https://arxiv.org/pdf/1409.1556.pdf)  

the winner and runner-up of localization and classification task in ILSVRC-2014.  

### **Contributions** 
- increase depth of the network using an architecture with very small (3×3) convolution filters.  

### **Architecture** 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/VGG/VGG.jpg" height="70%" width="70%"></div>

- use 1x1 conv in [NIN]() to add depth. 
- five max-poolinglayers are performed over a 2×2 pixel window, with stride 2. 
- padding is 1 pixel for 3x3 conv. 
- no LRN. 
- pretrained **VGG16** is the most frequently used. 
- It is easy to see that a stack of two 3×3 conv layers has an effective receptive field of 5×5; three such layers have a 7×7 effective receptive field, but have fewer paramenters. 

### **Training Details** 
- SGD optmizer, The batch size was set to 256, momentum to 0.9. The training was regularised by weight decay (the L2 penalty multiplier set to 5e−4 ) and dropout regularisation for the first two fully-connected layers. The learning rate was initially set to 1e−2 , and then decreased by a factor of 10 when the validation set accuracy stopped improving. 
- randomly cropped from rescaled training images (one crop per image per SGD iteration). 
- To further augment the training set, the crops underwent random horizontal flipping and random RGB colour shift. 
- pre-initialisation of certain layers, but found it can initialized by [Glorot & Bengio](http://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf)  
<div align="center">
<img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/VGG/VGG_Glorot_Bengio_initialisation.jpg" height="50%" width="50%">
</div>  

- testing similar to [OverFeat](https://blog.dtrimina.cn/CNNs-for-classification/OverFeat/) 
- a speedup of 3.75 times on an off-the-self 4-GPU systerm. 

### **Conclusions from Experiments**  
1. LRN is useless.  
2. depth is important.  
3. in the same depth, 3x3 conv is better than 1x1 conv.  
4. The error rate of VGG saturates when the depth reaches 19 layers, it might need more data for training.  
5. scale jittering at both training and testing time leads to better results than fixed smallest side.  
6. dense convnet evaluation similar used in [OverFeat](https://blog.dtrimina.cn/CNNs-for-classification/OverFeat/) is slightly worse than mult-crop evaluation used in [GoogLeNet](https://blog.dtrimina.cn/CNNs-for-classification/GoogLeNet/), but it's high-efficiency.  
7. use ensemble method.  

### **Localization** 
- a single object bounding box should be predicted for each of the top-5 classes. 
- A bounding box is represented by a 4-D vector storing its center coordinates, width, and height. Training detail can refer to OverFeat. 
- **pretrained vggs is very good feature extractors**. 