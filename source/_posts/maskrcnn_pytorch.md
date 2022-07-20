---
title: 实现Mask RCNN Pytorch版本
date: 2022-07-02 21:22:45
tags: [语义分割, COCO, github]
categories: [语义分割]
---

# 整体架构

本次实现的Mask RCNN主要分为以下几个部分：
   
* backbone

* RPN

# backbone

首先原文提出了四种backbone：
* resnet50
* resnet50 + FPN
* resnet101
* resnet101 + FPN

本次基于resnet101 + FPN实现

## resnet101[^1]

就是基础的resnet模型, 可借用torchvision实现

![(resnet模型图)](/images/maskrcnn_pytorch/resnet.png)

## 特征金字塔(FPN)[^2]

FPN, 又称特征金字塔, 初始的FPN使用来进行目标检测的, 但由于其性能, 现大多用来做特征提取.

![(FPN模型图)](/images/maskrcnn_pytorch/base_fpn.png)

基础的FPN先对高层特征图**上采样**(2倍的最邻近插值), 再对低层特征图**升维**(1 * 1 卷积), 最后对维度, 高度, 宽度一致的两个特征图进行元素对应相加, 得到融合特征图.

![(resnet模型图)[^3]](/images/maskrcnn_pytorch/rcnn_fpn.jpg)

Mask RCNN内的FPN上采样部分不变, 在**升维**部分, 这里的处理是把所有特征图维度处理至<font color="blue">256</font>.

在得到第一次**P5, P4, P3, P2**后, 对每一个特征图作3 * 3 卷积, 输出仍为<font color="blue">256</font>.

如图示, **P6, P5, P4, P3, P2**会送至**RPN层**.

# RPN



[^1]: [He K, Zhang X, Ren S, et al. Deep residual learning for image recognition[C]//Proceedings of the IEEE conference on computer vision and pattern recognition. 2016: 770-778.](http://openaccess.thecvf.com/content_cvpr_2016/html/He_Deep_Residual_Learning_CVPR_2016_paper.html)

[^2]: [Lin T Y, Dollár P, Girshick R, et al. Feature pyramid networks for object detection[C]//Proceedings of the IEEE conference on computer vision and pattern recognition. 2017: 2117-2125.](https://arxiv.org/abs/1612.03144)

[^3]: [Mask-RCNN 算法及其实现详解](https://blog.csdn.net/remanented/article/details/79564045)
