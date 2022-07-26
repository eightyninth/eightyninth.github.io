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



<span id="GNNframework">![(图1: 图迁移学习框架图)](/images/egi_img/framework.png)</span>


我们所设计的模型称作**EGI**.其利用基于ego-图信息最大化的训练目标来有效捕获图信息.


然后我们进一步具体说明了对可迁移节点特征的要求，并分析了依赖于源图和目标图的本地图拉普拉斯算子的 EGI 的可迁移性.


# 迁移GNN


在图迁移学习设置中, 预训练过程是无法得知下游任务的.我们认为应该优化和量化GNN的一般效用(general utility). 
而他捕获基础图信息的能力与图的拓扑结构和节点特征有关.


基于上述观点, 我们设计了ego-图信息最大化模型( ego-graph information maximization model, EGI).
然后通过其对目标图和源图建模能力的差距来衡量GNN模型的一般迁移性.


## EGI


此项工作关注于在原图上进行无监督预训练过后的GNN, 在直接迁移设置下, 不经过微调, 直接应用于目标图上.


| 含  义 |             公  式             |
|:----:|:----------------------------:|
|  源图  |          $ G_{a} $           |
| 目标图  |          $ G_{b} $           |
| 节点特征 |            $ X $             |
| 节点集  |            $ V $             |
|  边集  |            $ E $             |
|  图   | $ G = \lbrace V, E \rbrace $ |


直觉上来说, 只有在源图$ G_{a} $和目标图$ G_{b} $的特征和结构在某些方面相似时, 迁移学习才会成功.
因此, 在图$ G_{a} $上进行学习的GNN图滤波( graph filters)能够兼容$ G_{b} $的特征和结构.


我们提出了一个新的观点: 图可以视为节点特征和k阶ego-图结构的联合分布采样.
由于这样操作本质是对k阶ego图采样进行编码, 可以帮助我们在迁移学习下对图的结构信息进行具体定义.
这样有利于图之间的相似/不相似性度量.


我们设计了**EGI**, 此网络通过互信息最大化用以代替k阶图的恢复.


> k阶ego图: 对于节点$ v_{i} $, 只要它具有k层质心扩展, 使得$ v_{i} $到ego图中的人任何节点的最好距离是k ($ \forall v_{i} \in V(g_{i}), |distance(v_{i}, v_{j})| \leq k $), 它的k阶ego图定义为$g_{i} = \lbrace V(g_{i}), E(g_{i}) \rbrace $.


在此次工作中, 我们使用k阶有向图, 其方向由是否到中心节点的输入边还是输出边决定. 当然, 其结果同样适用于无向图.


> 结构信息: $ \mathcal{G} $是拓扑空间的子图, $ G $ 视作按照概率$ \mu $从$ \mathcal{G} $采样的独立同分布的k阶ego图$ \lbrace g_{i} \rbrace_{i=1}^{n} $. 从而$ G $的结构信息定义为k阶ego图集合$ \lbrace g_{i} \rbrace_{i=1}^{n} $和经验分布.


如[图1](#GNNframework)所示, $ G_{0} $, $ G_{1} $, $ G_{2} $通过1阶ego图的集合和经验分布进行区分, 这使得我们能够衡量图之间的结构相似性.


### ego图信息最大化 


![(图2: EGI框架图)](/images/egi_img/model.png)


给定从经验联合分布$ (g_{i}, x_{i}) \backsim \mathbb{P} $采样的ego图集合$ \lbrace (g_{i}, x_{i}) \rbrace_{i} $.
GNN编码器$ \Psi $的训练目标为最大化定义为结构信息的k阶ego图$ g_{i} $和节点嵌入$ z_{i} = \Psi(g_{i}, x_{i}) $的互信息$ MI(g_{i}, \Psi(g_{i}, x_{i}))$.
为了最大化互信息$ MI $, 引入判别器$ \mathcal{D}(g_{i}, z_{i}): E(g_{i}) \times z_{i} \rightarrow \mathbb{R}^{+} $来计算边$ e $属于ego图$ g_{i} $的概率.
EGI模型损失函数使用JS散度:


$$ \mathcal{L}\_{EGI} = -MI^{(JSD)}( \mathcal{G}, \Psi ) = \frac{1}{N} \sum\_{i=1}^{N} [sp( \mathcal{D}(g_i, z_{i}^{'})) + sp(-\mathcal{D}(g_i, z_{i}))] \tag{1} \label{eq:one} $$


其中, $ sp(x) = \log(1 + e^{x}) $ 是softplus function(一种激活函数), $ (g_{i}, z_{i}^{'}) $是从边缘分布的乘积中随机抽取的, 即$ z_{i}^{'} = \Psi (g_{i^{'}}, x_{i^{'}}), (g_{i^{'}}, x_{i^{'}}) \backsim \mathbb{P}, i^{'} \neq i $.


在$ 式 \eqref{eq:one} $中, 对边是否属于k阶ego图$ E(g_i) $进行计算的判别器$ \mathcal{D} $依赖于节点顺序.
以固定的图顺序来描述判别器$ \mathcal{D} $的决策过程.具体来说, $ E(g_i) $的顺序是广度优先遍历(BFS-ordering) $ \pi $.


$ \mathcal{D} = f \circ	\Phi $, 通过另一个GNN编码器$ \Phi $和对边序列$ E^{\pi}: \lbrace e_1 , e_2 , \cdots , e_n \rbrace $在有向图层面是否属于k阶ego图进行预测的得分函数$ f $ 输出的准确度得到的召回率进行相乘.


根/ 中心节点编码器$ \Psi $输入$ (g_i, x_i) $, 在判别器$ \mathcal{D} $中对邻域节点处理的编码器$ \phi $输入$ (\tilde{g_i}, x_i) $：


$$ \Psi(g_i , x_i ) = GNN_{\Psi} (A_i, X_i ), \Phi (\tilde{g_i}, x_i ) = GNN_{\Phi} (A_{i}^{'}, X_i) $$

$ A_i, A_{i}^{'} $是$ g_i, \tilde{g_i} $包括自向边的邻接矩阵.同时$ A_i = A_{i}^{'T}$.自向边的存在使得GNN可以关注到根节点自身的影响.


$ \Psi $ 的输出为$ z_i \in \mathbb{R}^n $ 是根节点的节点嵌入, $ \Phi $ 的输出$ K \in \mathbb{R}^{|g_i| \times n} $ 为ego图邻域节点的表示.


得到$ H $ 后, 就可以计算得分函数$ f $: 对于每个在边序列的节点对$ (p, q) \in E^{\pi} $, $ h_p $是从$ \Phi $得到的源节点表示, $ x_q $是目标节点特征.


$$ f(h_p, x_q, z_i) = \sigma (U^T \cdot	\tau (W^T [h_p || x_q || z_i])) \tag{2} \label{eq:two}$$


$ \sigma $ 和 $ \tau $ 是Sigmoid和ReLU激活函数. 

然后, 判别器$ \mathcal{D} $用以区分$ g_i $中边的正负对:正对-$ ((p, q), z_i) $,负对-$ ((p, q), z_{i}^{'}) $:


$$ \mathcal{D} (g_i, z_i)  = \sum_{(p,q) \in E^{\pi}} \log f(h_p, x_q, z_i), \mathcal{D} (g_i, z_{i}^{'})  = \sum_{(p,q)}^{E^{\pi}} \log f(h_p, x_q, z_{i}^{'}) \tag{3} \label{eq:three} $$


在节点序列中, 我们考虑了两种边$ (p, q) $, 第一种: 边连接了对于根节点来说跨阶的节点; 第二种: 边连接了对于根节点来说同阶的节点.


前述提及了节点对的广度优先遍历(BFS-ordering), 而在$ 式 \eqref{eq:three} $中, 对第一种边的序列变化很敏感, 对第二种不敏感.


基于上述事实, 由于k层GNN的输出仅取决于编码器$ \Psi $和$ \Phi $的k阶ego图, 因此可以通过对$ g_i $的批量采样来并行训练EGI.


### EGI 的使用场合


EGI 的使用场合为直接迁移, 即目标领域标签未知, 其可迁移性是通过没有进行微调的模型性能差异来衡量的.


## 基于局部图的拉普拉斯可迁移性分析


此次工作主要为基于源图$ G_a $和目标图$ G_b $之间相似性的GNN的可迁移性.


我们认为GNN的输出由三部分构成: 输入的节点特征, 固定的图拉普拉斯, 学习到的图过滤器.
GNN的效用由这三者决定.
为了实现这种兼容性，我们要求节点特征与结构有关.


> 结构相关节点特征: $g_i$表示有序的中间结点为$ v_i $的ego图, 其节点特征为$ \lbrace x_{p,q}^{i} \rbrace_{p=0,q=1}^{k,|V_p(g_i)|} $, 其中$ V_p(g_i) $是$ g_i $的k阶节点集合. 只有对于任意属于ego图的节点$ v_q \in V_p(g_i) $有$ x_{p,q}^{i} = [f(g_i)]_{p,q} \in \mathbb{R}^d $, 其中$ f: \mathcal{G} \rightarrow \mathbb{R}^{d \times |V(g_i)|} $, 这样的节点特征才可称为结构相关.


结构相关节点特征要求节点特征是图结构的函数.
这样的节点对图结构的变化很敏感, 同时在理想的情况下, 节点特征对图结构是内射的(映射不同的图到不同的特征).
这样, 当一个迁移GNN学习到的图过滤器兼容$ G $的结构时, 同样能够兼容$ G $的节点特征.


> GNN可迁移性: 源图$ G_a = \lbrace (g_, x_i) \rbrace_{i=1}^{n} $和目标图$ G_b = \lbrace (g_{i'}, x_{i'}) \rbrace_{i'=1}^{m} $以及假设与结构相关的节点特征, k层GCN $ \Psi_{\theta} $, 一阶多项式$ \phi $
> 在以上对$ G_a $和$ G_b $的局部谱域合理的假设下, $ \Psi_{\theta} $的经验性能差异通过$ \mathcal{L}_{EGI} $评估, 满足:


$$ | \mathcal{L}\_{EGI} (G_{a}) - \mathcal{L}\_{EGI} (G_{b})| \leq \mathcal{O}( \Delta_{ \mathcal{D}} (G_{a}, G_{b}) + C) \tag{4} \label{eq:four} $$


论文原文, <font color="red"> RHS表示公式右侧</font>


其中, $ C $只依赖于图编码器和节点特征.


$ \Delta_{ \mathcal{D}}(G_a ,G_b) $衡量源图和目标图结构区别的方式:


$$ \Delta_{\mathcal{D}}(G_a, G_b) = \tilde{C} \frac{1}{nm} \sum_{n}^{i=1} \sum_{m}^{i^{'}=1} \lambda_{max} (\tilde{L}\_{g_i} - \tilde{L}\_{g_{i^{'}}}) \tag{5} \label{eq:five} $$


其中, $ \lambda_{max} (A) : \lambda_{max}(A^T A)^(1/2) $, $ \tilde{L}\_{g_i} $通过入度表示$\tilde{g}\_i$的归一化图拉普拉斯, $ \tilde{C}$ 一直依赖于 $ \lambda_{max}( \tilde{L}_{g_i}) $ 和 $\mathcal{D}$.


**GNN可迁移性**的分析自然举例了我们对于结构相似性和GNN可迁移性之间对应关系的理解.
这使得我们可以告知EGI怎样把在源图上训练好的GNN仅仅通过衡量源图和目标图之间的局部拉普拉斯差异, 而不需要任何微调, 就可以在目标图上很好地工作.


# 实验


直接迁移的图编码器为GIN, 
存在微调迁移的图编码器为GCN, 
自监督GNN为GVAE, DGI, 
互信息衡量方法为GMI, MVC, 
对于预训练GNN, 有MaskGNN和ContextGNN,
此外, 对比方法还有Structural Pre-train.


![(图3: 实验图)](/images/egi_img/experiment1.png)



