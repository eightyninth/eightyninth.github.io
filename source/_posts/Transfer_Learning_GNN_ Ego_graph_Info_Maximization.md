---
title: Transfer Learning of Graph Neural Networks with Ego-graph Information Maximization
date: 2022/7/19 上午10:47
tags: [GNN, 迁移学习]
categories: [文献阅读]
---

论文地址: [2009.05204.pdf](https://arxiv.org/pdf/2009.05204.pdf)

代码地址: [EGI](https://github.com/GentleZhu/EGI)

# 摘要

1. GNN效果良好, 但针对大规模图的训练消耗良多.


2. 在近年进行GNN预训练的研究中, 一是没有提供模型设计的理论支撑, 二是没有清楚地提出模型可转移性的要求和保证.


3. 本文观点: 首先, 对**基本图信息**提出一个新的观点, 主张捕获**基本图信息**作为迁移GNN训练的目标. 这启发了**EGI**的设计, 使用**EGI**实现此目标.
其次, 当节点特征与结构相关时, 对目标图和源图局部拉普拉斯的不同进行EGI可转移性分析.


# 介绍

此次工作为迁移学习GNN构建了一个有理论支撑的框架, 并具现化为一个可实际应用的迁移学习GNN模型.

下图为模型的概览, 其基于一个新的视角: 把图视作节点特征和k阶ego-图结构的联合分布采样.
这样的观点使得我们可以通过定义图的信息和相似度来分析图的可迁移性.

![(EGI模型图)](/images/Transfer_Learning_GNN_Ego_graph_Info_Maximization/EGI.png)


我们所设计的模型称作**EGI**.其利用基于ego-图信息最大化的训练目标来有效捕获图信息.


然后我们进一步具体说明了对可迁移节点特征的要求，并分析了依赖于源图和目标图的本地图拉普拉斯算子的 EGI 的可迁移性.


# 迁移GNN


在图迁移学习设置中, 预训练过程是无法得知下游任务的.我们认为应该优化和量化GNN的一般效用(general utility). 
而他捕获基础图信息的能力与图的拓扑结构和节点特征有关.


基于上述观点, 我们设计了ego-图信息最大化模型( ego-graph information maximization model, EGI).
然后通过其对目标图和源图建模能力的差距来衡量GNN模型的一般迁移性.


## EGI


此项工作关注于在原图上进行无监督预训练过后的GNN, 在直接迁移设置下, 不经过微调, 直接应用于目标图上.




