---
title: PReLU-nets
tags:
  - Classification
categories:
  - Classification
date: 2019-06-01 15:48:18
---


paper: [Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification](https://arxiv.org/pdf/1502.01852.pdf)

### Parametric Rectified Linear Unit (PReLU) 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/PReLU-nets/PReLU.jpg" height="70%" width="70%"></div>  

Above is ReLU vs. PReLU,  *a* is a coefficient controlling the slope of the negative part and a learnable parameter.  

two interesting phenomena:
- First, the first conv layer (conv1) has coefficients (0.681 and 0.596) significantly greater than 0. As the filters of conv1 are mostly Gabor-like filters such as edge or texture detectors, the learned results show that both positive and negative responses of the filters are respected.
- Second, for the channel-wise version, the deeper conv layers in general have smaller coefficients. This implies that
the activations gradually become “more nonlinear” at increasing depths. In other words, the learned model tends to keep more information in earlier stages and becomes more discriminative in deeper stages.

notice:
- do not use weight decay (l2 regularization) when updating *a*. A weight decay tends to push *a* to zero, and thus biases PReLU toward ReLU. Even without regularization, the learned coefficients rarely have a magnitude larger than 1 in our experiments.
- do not constrain the range of *a* so that the activation function may be non-monotonic. 
- We use *a* = 0.25 as the initialization. 


### He's initialization method  
**a zero-mean Gaussian distribution whose standard deviation (std) is np.sqrt(2/n) and bias to 0.**