---
title: SEnet
date: 2020-03-10 11:20:17
tags:
- Deep Learning
- CV
mathjax: true
---

# {{ title }}



## Award

《Squeeze-and-Excitation Networks》 Momenta

CVPR 2017

最后一届ImageNet2017分类任务冠军



### 评述

- SE Block的目标：adaptively recalibrates channel-wise feature responses by explicitly modelling interdependencies between channels
- SE网络的出发点：利用通道维度上的关联关系

### 方法

卷积操作隐含了通道依赖，但是同时也混着局部空间相关；希望SE Block能单独、显式建模通道依赖性

![image-20200311103028816](https://raw.githubusercontent.com/JLUtangchuan/picBed/dev/image-20200311103028816.png)

挤压操作：global average pooling
$$
z_{c}=\mathbf{F}_{s q}\left(\mathbf{u}_{c}\right)=\frac{1}{H \times W} \sum_{i=1}^{H} \sum_{j=1}^{W} u_{c}(i, j)
$$
激励操作：
$$
\mathbf{s}=\mathbf{F}_{e x}(\mathbf{z}, \mathbf{W})=\sigma(g(\mathbf{z}, \mathbf{W}))=\sigma\left(\mathbf{W}_{2} \delta\left(\mathbf{W}_{1 \mathbf{Z}}\right)\right)
$$
<img src="https://raw.githubusercontent.com/JLUtangchuan/picBed/dev/image-20200311112551391.png" alt="image-20200311112551391" style="zoom: 80%;" />

最后再将学到的各通道的权重乘到输入的特征图上

网络：提供了两种，SE-ResNet结果好一些

![image-20200311112816088](https://raw.githubusercontent.com/JLUtangchuan/picBed/dev/image-20200311112816088.png)



#### 扩展阅读

- Attention

> 本质上，SE模块是在channel维度上做attention或者gating操作