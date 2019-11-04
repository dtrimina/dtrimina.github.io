---
title: classification(2) -- NIN、BatchNorm、Highway、PReLU
tags:
  - Classification
categories:
  - Classification
date: 2019-05-25 09:23:49
---

paper: [Network In Network](https://arxiv.org/pdf/1312.4400.pdf)  

### **Contributions** 
1. mlpconv layer 
2. global average pooling 

### **Architecture** 
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/NIN/NIN.jpg" height="60%" width="60%"></div>  

The cross channel parametric pooling layer can be seen as **a convolution layer with 1x1 convolution kernel**. 

##### Why using multilayer perceptron ?  
- multilayer perceptron is compatible with the structure of convolutional neural networks, which is trained using back-propagation. 
- multilayer perceptron can be a deep model itself, which is consistent with the spirit of feature re-use. 

##### Why global average pooling ?  
- it is more native to the convolution structure by enforcing correspondences between feature maps and categories. 
- there is no parameter to optimize in the global average pooling thus overfitting is avoided at this layer. 
- global average pooling sums out the spatial information, thus it is more robust to spatial translations of the input.

Other details are in the paper. 


------------------------------

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

-----------------------------


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


---------------------------

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