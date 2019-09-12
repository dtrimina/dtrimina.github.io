---
title: SqueezeNet
tags:
  - Classification
categories:
  - Classification
mathjax: true
date: 2019-07-15 09:08:22
---


paper: [SQUEEZENET: ALEXNET-LEVEL ACCURACY WITH 50X FEWER PARAMETERS AND <0.5MB MODEL SIZE](http://xxx.itp.ac.cn/pdf/1602.07360v4) with [pytorch code](https://github.com/weiaicunzai/pytorch-cifar100/blob/master/models/squeezenet.py)

Why we need smaller CNN architectures?
 - More efficient distributed training;
 - Less overhead when exporting new models to clients;
 - Feasible FPGA and embedded deployment.  

SqueezeNet achieves AlexNet-level accuracy on ImageNet with 50x fewer parameters. Additionally, with model compression techniques, it is able to compress SqueezeNet to less than 0.5MB (510× smaller than AlexNet).
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/SqueezeNet/results.png" height="90%" width="90%"></div>


### **Fire module**
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/SqueezeNet/firemodule.png" height="50%" width="50%"></div>

```python  
class Fire(nn.Module):
    def __init__(self, in_channel, out_channel, squzee_channel):
        super().__init__()
        self.squeeze = nn.Sequential(
            nn.Conv2d(in_channel, squzee_channel, 1),
            nn.BatchNorm2d(squzee_channel),
            nn.ReLU(inplace=True)
        )
        self.expand_1x1 = nn.Sequential(
            nn.Conv2d(squzee_channel, int(out_channel / 2), 1),
            nn.BatchNorm2d(int(out_channel / 2)),
            nn.ReLU(inplace=True)
        )
        self.expand_3x3 = nn.Sequential(
            nn.Conv2d(squzee_channel, int(out_channel / 2), 3, padding=1),
            nn.BatchNorm2d(int(out_channel / 2)),
            nn.ReLU(inplace=True)
        )
    
    def forward(self, x):
        x = self.squeeze(x)
        x = torch.cat([
            self.expand_1x1(x),
            self.expand_3x3(x)
        ], 1)
        return x
```

### **Architecture**
<div align="center"><img src="https://saveimages.oss-cn-hangzhou.aliyuncs.com/CNNs_for_image_classification/SqueezeNet/squeezenet.png" height="90%" width="90%"></div>

It was designed follow three main strategies: 
 - Replace 3x3 filters with 1x1 filters (Fire module);
 - Decrease the number of input channels to 3x3 filters (Fire module);
 - Downsample late in the network so that convolution layers have large activation maps
