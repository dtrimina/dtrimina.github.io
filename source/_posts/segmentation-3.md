---
title: segmentation(3) -- ENet、LinkNet、FC-DenseNet
tags:
  - Segmentation
categories:
  - Segmentation
mathjax: true
date: 2019-10-09 20:11:11
---



paper: [ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation](http://xxx.itp.ac.cn/pdf/1606.02147v1)

##### 总体结构  
<center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/ENet/ENet_architecture.jpg" height="50%" width="50%"></center>  

主要模块如下  
<center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/ENet/ENet_block.jpg" height="70%" width="70%"></center>   


(a)为初始的init  
(b)为整体结构主要模块，借鉴resnet中bottleneck，其中  
- regularizer use Spatial Dropout, with p = 0.01 before bottleneck2.0, and p = 0.1 afterwards. 
- bottleneck downsampling: using maxpooling and replace the first 1x1 projection with a 2x2 conv stride=2
- bottleneck dilated: conv使用空洞卷积
- bottleneck asymmetric: conv使用分离卷积 eg. 5x1conv + 1x5conv
- bottleneck unsampling(decoder): replace maxpooling with max unpooling  


特点： 
- use dilated convolutions to have a wide receptive field
- early downsampling
- a large encoder and a small decoder
- use PReLUs, not ReLUs
- factorizing filters by using 5x1 conv and 1x5 conv
- regularization update from L2 -> stochastic depth -> Spatial Dropout


------------------

paper: [LinkNet: Exploiting Encoder Representations for Efficient Semantic Segmentation](http://xxx.itp.ac.cn/pdf/1707.03718.pdf)  [ [website](https://codeac29.github.io/projects/linknet) | [pytorch_code](https://github.com/e-lab/pytorch-linknet/blob/master/models/linknet.py) ]

##### 总体结构  
<center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/LinkNet/LinkNet_architecture.jpg" height="50%" width="50%"></center>

其中encoder block 及 decoder block 分别如下  
<center> <figure class="half">
    <img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/LinkNet/LinkNet_encoder_block.jpg" height="30%" width="30%">
    <img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/LinkNet/LinkNet_decoder_block.jpg" height="30%" width="30%">
</figure> </center>

- use ResNet18 as its encoder  
- link each encoder with decoder which is aimed to recover lost spatial information
- can give real-time performance even on NVIDIA TX1 embedded system module  
- use weighing scheme $w_{\text {class }}=\frac{1}{\ln \left(1.02+p_{\text {class }}\right)}$, which is better than mean average frequency  
- use database cityscape and camvid

##### 实验结果 
- cityscape 
  <center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/LinkNet/cityscape_result.jpg" height="50%" width="50%"></center>
- camvid
  <center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/LinkNet/camvid_result.jpg" height="90%" width="90%"></center>

------------------

paper: [The One Hundred Layers Tiramisu: Fully Convolutional DenseNets for Semantic Segmentation](http://xxx.itp.ac.cn/pdf/1611.09326.pdf)

##### 总体结构

<center> <figure class="half">
    <img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/FCDensNets/FCDensnets_architecture.jpg" height="40%" width="40%">
    <img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/FCDensNets/configs.jpg" height="50%" width="50%">
</figure> </center>

其中DB(Dense Block)、DB中的layer、TD(尺寸缩减)、TU(尺寸变大)部分如下
<center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/FCDensNets/dense_block.jpg" height="20%" width="20%"></center>  
<center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/FCDensNets/layer_down_up.jpg" height="70%" width="70%"></center>  

- use DenseNet architecture  
- use database camvid and gatech

##### 实验结果  
- camvid
  <center><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/Segmentation/FCDensNets/camvid_result.jpg" height="100%" width="100%"></center>
