---
title: segmentation(4) -- DilatedNet、DRN
tags:
  - Segmentation
categories:
  - Segmentation
mathjax: true
date: 2019-10-17 20:05:48
---



paper: [DilatedNet: MULTI-SCALE CONTEXT AGGREGATION BY DILATED CONVOLUTIONS](http://xxx.itp.ac.cn/pdf/1511.07122v3) | [ [code](https://github.com/bordesf/dilation/blob/master/dilated_cnn.py)/[pytorch_code](https://github.com/Blade6570/Dilation-Pytorch-Semantic-Segmentation/blob/master/CAN_network.py) ]


##### 总体结构  

**a VGG16 based front-end prediction module + a context module**  

- 对于front-end: We adapted the VGG-16 network for dense prediction and removed the last two pooling and striding layers. Specifically, each of these pooling and striding layers was removed and convolutions in all subsequent layers were dilated by a factor of 2 for each pooling layer that was ablated.  
- 接下来得到channel为C(类别)的feature map. 通过context module, 得到C channel的输出, 如下所示  
  
  <div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/DilatedNet/context_module.jpg" height="70%" width="70%"></div>  


##### 特点  

- 使用dilated convolution. 作者认为Pooling or upsample增加感受野的方式会使得图片分辨率变小，空间信息丢失，而使用dilated convolution可以在不缩减尺寸的情况下增加感受野。  
  
  <div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/DilatedNet/dilated_convolution.jpg" height="60%" width="60%"></div>  


-------------------------------


paper: [DRN: Dilated Residual Networks](http://xxx.itp.ac.cn/pdf/1705.09914v1)  


##### 总体结构  

<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/DRN/DRNs.png" height="90%" width="90%"></div>  

DRN算是DilatedNet的升级版，使用resnet结构代替VGG。并在此表明自己的观点：  
- 考虑到小物体在大背景的情况下，经过多次缩减,the background response may suppress the signal from the object of interest. What’s worse,if the object’s signal is lost due to downsampling, there is little hope to recover it during training.
- 增加输出分辨率最直接的办法就是减少尺寸缩减的次数。但是尺寸缩减次数减少，后层感受野减小，有可能reduce the amount of context that can inform the prediction。
- 考虑到以上两点，又要减少尺寸缩减的次数，又要感受野不变小，就在后面几层使用dilated convolution来提高感受野。  

上面DRN-A-18结构就是在resnet18的基础上进行改进。Group4和Group5的residual blocks不使用尺寸缩减，而是使用factor为2和4的dilated convolution。  

<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/DRN/resnet_vs_DRN.png" height="50%" width="50%"></div>  

对于DRN-B-26，because this maxpooling operation leads to high-amplitude high-frequency activations, 所以用了两个resblock来代替，并在最后加了两个resblock来减小输出网格效应，但也一定加深了网络的深度。为了进一步减小网格效应，DRN-C-26将最后加的两个resblock跳跃连接去掉了。  

<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/DRN/why_remove_maxpooling.png" height="50%" width="50%"></div>  


##### results  

- Imagenet classification  
  <div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/DRN/imagenet_result.png" height="50%" width="50%"></div>   

- Cityscapes segmentation
  <div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/DRN/cityscapes_result.png" height="90%" width="90%"></div>   

