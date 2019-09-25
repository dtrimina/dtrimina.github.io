---
title: 'Inceptions - GoogLeNet, V3, V4'
tags:
  - Classification
categories:
  - Classification
mathjax: true
date: 2019-09-18 20:39:13
---


paper: [Going deeper with convolutions](https://arxiv.org/pdf/1409.4842.pdf)

##### Inception-V1 [GoogLeNet](https://blog.dtrimina.cn/Classification/GoogLeNet/)

-----------
paper: [Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/pdf/1512.00567.pdf)

##### Inception-v2，v3
新的Inceptionv3结构如下，论文在Inception-v2([BN-Inception](https://blog.dtrimina.cn/Classification/Batch-Normalization/))的基础上进行一些改进。
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v3/inceptionv3_architecture.jpg" height="60%" width="60%"></div>

其中三种Inception结构如下

<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v3/InceptionA.jpg" height="40%" width="40%"></div>
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v3/InceptionB.jpg" height="40%" width="40%"></div>
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v3/InceptionC.jpg" height="40%" width="40%"></div>


1.将大尺寸卷积分解为小尺寸卷积的堆叠。(e.g. 5x5卷积使用两个3x3卷积堆叠代替)  
2.卷积操作的进一步分解。(e.g. 3x3卷积使用3x1卷积和1x3卷积堆叠代替)  
3.辅助分类器中使用BatchNorm和Dropout性能会更好。  
4.先进行Pooling再通过Inception模块，训练参数较少但模型表征能力变弱；先通过Inception模块再进行Pooling，训练参数会稍增加，但模型具有更好的表征能力。一个折中的方案如下所示，It is both cheap and avoids the representational bottleneck. 

<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v3/reducing_grid_size.jpg" height="50%" width="50%"></div>  

5.label-smoothing regularization  
比较结果如下，应用以上所有性质的Inception称为Inceptionv3.  
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v3/Inceptionv3_results.jpg" height="50%" width="50%"></div>  

-----------
paper: [Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning](https://arxiv.org/pdf/1602.07261.pdf)

##### Inception-V4
总体结构如下：
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v4/inception_v4_architecture.jpg" height="40%" width="40%"></div>   
其中各模块参考论文： 

- Stem -> Figure 3
- Inception-A -> Figure 4
- Reduction-A -> Figure 7
- Inception-B -> Figure 5
- Reduction-B -> Figure 8
- Inception-C -> Figure 6 

##### Inception-ResNets
总体结构如下：
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v4/Inception_resnets.jpg" height="40%" width="40%"></div>   

- Inception-ResNet-v1和Inceptionv3计算量相似，各模块参考Figure 14、10、7、11、12、13  
- Inception-ResNet-v2和Inceptionv4计算量相似，各模块参考Figure 3、16、7、17、18、19  

一些结果比较如下：  
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v4/result1.jpg" height="50%" width="50%"></div>   
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v4/result2.jpg" height="50%" width="50%"></div>   
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v4/result3.jpg" height="50%" width="50%"></div>   
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/Inception_v4/result4.jpg" height="50%" width="50%"></div>   
