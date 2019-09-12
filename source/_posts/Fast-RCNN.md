---
title: Fast R-CNN
tags:
  - Detection
categories:
  - Detection
mathjax: true
date: 2019-06-21 19:17:50
---


paper: [Fast R-CNN](http://xxx.itp.ac.cn/pdf/1504.08083v2)

[SPPnet](https://blog.dtrimina.cn/CNNs-for-classification/SPP-net/) solved the R-CNN's problem that it extracts features for each of the 2k~ region proposal and costs a lot of time. SPPnet runs the convolutional layers only once on the entire image (regardless of the number of windows), and then extract features by SPP-net on the feature maps. But it is **not an end-to-end model** so that extracted features need to be written to the disk, and uses two stages for classification and bbox regression. Fast R-CNN can do back-propagation end-to-end.

### **Architecture**
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_detection/Fast_RCNN/Fast_RCNN.png" height="70%" width="70%"></div>
I think the main difference of Fast R-CNN is that it's RoI layer is simply the special-case of the spatial pyramid pooling layer used in SPPnets in which there is only one pyramid level **to get fixed feature map**. Then using a single-stage training algorithm that jointly learns to classify object proposals and refine their spatial locations. The loss function is as follows:  

$L\left(p, u, t^{u}, v\right)=L_{\mathrm{cls}}(p, u)+\lambda[u \geq 1] L_{\mathrm{loc}}\left(t^{u}, v\right)$  
classification loss use cross-entropy and regression loss use smooth L1 loss. However, it **still use selective search** to achieve region proposals.  

### **Others**
- Fast R-CNN uses a faster sampling method for training compare to R-CNN and SPPnets, more details are in paper. 
- use **truncated SVD** to compress the fc layers.

