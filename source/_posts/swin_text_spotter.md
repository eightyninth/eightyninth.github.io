---
title: SwinTextSpotter - Scene Text Spotting via Better Synergy between Text Detection and Text Recognition
date: 2022/7/27 下午5:12
tags: [文字定位]
categories: [文献阅读]
---

论文地址: [Huang_SwinTextSpotter_Scene_Text_Spotting_via_Better_Synergy_Between_Text_Detection_CVPR_2022_paper.pdf](https://openaccess.thecvf.com/content/CVPR2022/papers/Huang_SwinTextSpotter_Scene_Text_Spotting_via_Better_Synergy_Between_Text_Detection_CVPR_2022_paper.pdf)

代码地址: [SwinTextSpotter](https://github.com/mxin262/SwinTextSpotter)


# 摘要

1. 近年来SOTA的文字定位方法仅仅是把从backbone得到的特征, 输送到检测与识别分支, 并没有直接利用两个任务之间的特征交互.


2. SwinTextSpotter: 端到端的文字定位框架. 使用动态头的Transformer编码器作为检测器, 此次工作将这两个任务与一种新的识别转换机制统一起来，通过识别损失明确指导文本定位.


3. 这样直接的设计产生了一个简洁的框架，既不需要额外的纠正模块，也不需要对任意形状的文本进行字符级注释.


# 介绍

传统的文字定位方法把检测和识别视作两个分离的任务. 首先, 从输入图像上定位并裁剪出文本区域, 然后把裁剪的区域送入文本识别器预测文本序列.

这样的框架会有以下限制:
    
1. 两个任务的误差累加. eg: 不精确的检测结果会严重影响文字识别的性能.
    
2. 检测与分割两个任务的各自优化或许不会使得文字定位得到最好的效果.

3. 内存消耗大, 推理效率低.


为解决以上限制, 现尝试在一个端到端的框架解决文字定位问题.

现存这样的框架同样存在限制:

1. 如果检测仅仅基于输入特征的视觉信息, 检测器容易被背景噪声分散注意力并提出不一致的检测.

2. 检测和识别之间的交互通过共享backbone是不够的, 因为检测器既没有优化识别损失, 识别器也没有利用检测特征.


此次工作提出**SwinTextSpotter**, 是一个端到端训练, 基于Transformer的框架.

为了更好的区别拥挤场景中密集分散的文本实例, 在**SwinTextSpotter**中使用Transformer和两层自注意力机制激励文本实例之间的交互.


为了解决任意形状场景文字定位, 把文本检测任务视作集合预测问题, 然后使用基于查询的文字检测器.

此次工作进一步提出了**识别转换RC**模块, 它通过结合检测特征隐式地引导识别头. **识别转换RC**模块可以反向传播识别信息到检测器, 并且抑制识别特征中的背景噪声, 引导检测器与识别器的联合优化.

通过使用**识别转换RC**模块, **SwinTextSpotter**是一个简洁的框架, 没有先前工作中用于提升识别器的字符及注释和矫正模块.


此次工作主要贡献总结如下:

1. **SwinTextSpotter**开创性的展示了Transformer和集合预测在端到端场景文字预测的有效性.

2. **SwinTextSpotter**使用了**识别转换RC**模块来利用文字检测和识别之间的协同作用.

3. **SwinTextSpotter**是一个简单的框架, 既不需要字符级别的注释, 也不需要为了识别任意形状的文字设计修正模块.

4. **SwinTextSpotter**在多个公共场景文字基准上实现了SOTA性能.



# 方法

<span id="model">![图1: SwinTextSpotter model](/images/swinTextSpotter_img/model.png)</span>


**SwinTextSpotter**模型的整体结构如[图1](#model)所示.它包含四个组件:

1. 基于Swin-Transformer的backbone,
2. 基于查询的文字检测器,
3. 连接文字检测器和识别器的**识别转换RC**模块,
4. 基于注意力的识别器.


如[图1](#model)绿色箭头所示, **检测**的第一阶段, 首先随机初始化训练参数作为box$ bbox_{0} $和proposal特征$ f_{0}^{prop} $.

为了proposal特征能够获得全局信息, 使用全局平均池化提取图像特征, 并把他们加入到$ f_{0}^{prop} $中.

然后使用$ bbox_{0} $提取ROI特征.

把ROI特征和$ f_{0}^{prop} $送入带动态头的Transformer编码器中.

Transformer编码器的输出展开并命名为$ f_{1}^{prop} $, 送入检测头输出检测结果.

box$ bbox_{k - 1} $和proposal特征$ f_{k - 1}^{prop} $作为第$ k^{th} $个阶段检测器的输入.

proposal特征$ f_{k}^{prop} $通过融合ROI特征与之前的$ f_{k - 1}^{prop} $来反复更新自己, 这样使得proposal特征保存了先前阶段的信息.

此次工作对总计$ K $个阶段重复这样的细化, 类似与基于查询检测器的迭代结构.

这样的设计使得在尺寸和纵横比方面进行更加鲁棒的检测.


在**识别阶段**, [图1](#model)即橙色箭头所示, 相较于检测, 需要更高的分辨率.

此次工作使用检测的第$ K $个阶段的输出box$ bbox_{k} $来获取ROI特征, 此时的分辨率是检测阶段的四倍.

为了在融合proposal特征时保持分辨率与检测器一致, 对ROI特征进行下采样, 得到三个尺寸递减的特征图, 以$ \lbrace a_1, a_2, a_3 \rbrace $表示.

通过融合最小的$ a_3 $和proposal特征$ f_{K}^{prop} $得到检测特征$ f^{det} $.

在识别阶段, 检测特征$ f^{det} $包含先前所有的检测信息.

最终, $ \lbrace a_1, a_2, a_3 \rbrace $和检测特征$ f^{det} $送入**识别转换RC**模块以及为了生成识别结果的识别器中.


## Dilated Swin-Transformer

<span id="Dilated_Swin_Transformer">![图2: Dilated Swin Transformer](/images/swinTextSpotter_img/Dilated_Swin_Transformer.png)</span>

对于文字定位, 建模不同文字之间的关系是十分重要的, 因为同一张图像上的文字有着极强的相似性, 例如其背景和样式.

考虑到全局建模能力和计算效率, 使用FPN + Swin-Transformer作为backbone. 

鉴于一行文字字之间存在空白, 感受野需要足够大来帮助区分来相邻文字是否属于同一文本行.

为了获得这样的感受野, 在原始Swin-Transformer中使用了两个空洞卷积, 一个普通卷积和一个残差结构, 如[图2](#Dilated_Swin_Transformer)所示, 这样也把CNN的属性引入Transformer中.


## 基于查询的检测器


使用基于查询的检测器检测文本, 基于查询的检测器建立在ISTR之上, 把检测视为一个集合预测问题.

检测器使用一个proposal boxes集合, 用来替代RPN的庞大候选集, 一个proposal 特征集合表示目标的高阶语义向量.

检测器根据经验设计为具有六个查询阶段.

用动态头的Transformer编码器, 使得后续的阶段可以获得存储在proposal特征中的之间阶段的信息.

通过多个阶段的微调, 检测器可以适用于任何尺寸.

<span id="detection_head">![图3: k Stage detection head](/images/swinTextSpotter_img/detectionAtKStage.png)</span>

检测器在$ k^{th} $阶段的结构如[图2](#detection_head)所示.

$ k - 1 $阶段的输出: proposal 特征$ f_{k - 1}^{prop} \in \mathbb{R}^{N, d} $在$ k^{th} $阶段输入到自注意力模块$ MSA_{k} $来建模关系, 并生成两个卷积的参数. 因此, 先前阶段的检测信息嵌入到卷积之中.

以先前的proposal特征为约束的卷积用来编码ROI特征.

$ k - 1 $阶段的输出: $ bbox_{k - 1} $, 利用其在RoI Aligin模块提取ROI特征, ROI特征输入到卷积层中, 然后输入到线性映射中, 生成下一个阶段的输入$ f_{k}^{prop} $.

$ f_{k}^{prop} $ 接着输入到预测头中生成$ bbox_{k} $和$ mask_{k} $.

为了减少计算, 2D mask 通过主成分分析(PCA)转换为 1D mask向量.

当 $ k = 1 $时, $ bbox_{0} $和$ f_{0}^{prop} $是随机初始化的参数, 作为输入. 

在整个训练过程中, 这些参数仅仅通过反向传播进行更新, 学习文本高级语义特征的归纳偏置.

整个文本检测任务是做一个集合预测问题. 使用二分匹配来匹配预测和真值.如$ 式 \eqref{eq:one} $.

$$ L_{match} = \lambda_{cls} \cdot L_{cls} + \lambda_{L1} \cdot L_{L1} + \lambda_{giou} \cdot L_{giou} + \lambda_{mask} \cdot L_{mask} \tag{1} \label{eq:one}$$

其中, $ \lambda $是用于平衡损失的超参数, $ L_{cls} $是focal 损失, $ L_{L1} $损失是用于回归检测框的L1 损失, $ L_{giou} $ 是generalized IoU 损失, $ L_{mask} $本应计算预测mask和真值之间的余弦相似度, 但检测损失与匹配成本相似，改用 L2 损失和dice 损失来代替余弦相似度.


## **识别转换RC**模块

<span id="rc">![图4: RC模块](/images/swinTextSpotter_img/RC.png)</span>

为了更好地协同检测与识别, **识别转换RC**模块被用来在空间上结合从检测头得到的特征到识别阶段, 如[图4](#rc)所示.

**识别转换RC**模块由Transformer 编码器和四个上采样结构组成.

**识别转换RC**模块的输入是检测特征$ f^{det} $和三个下采样特征$ \lbrace a_1, a_2, a_3 \rbrace $.

检测特征输送到Transformer 编码器$ TrE() $中, 使得先前检测阶段的信息与$ a_3 $进一步融合, 然后进行一系列上采样操作$ E_{u}() $和Sigmoid函数$ \phi() $, 生成三个文本区域掩码$ \lbrace M_1, M_2, M_3 \rbrace $.

$$ d_{1} = TrE(f^{det}) \tag{2} \label{eq:two} $$

$$ d_{2} = (E_{u}(d_{1}) + a_{2}) \tag{3} \label{eq:three} $$

$$ d_{3} = (E_{u}(d_{2}) + a_{1}) \tag{4} \label{eq:four} $$

$$M_{i} = \phi (d_{i}), i=1,2,3. \tag{5} \label{eq:five} $$

进一步有效整合文本区域掩码$ \lbrace M_1, M_2, M_3 \rbrace $和输入特征$ \lbrace a_1, a_2, a_3 \rbrace $:

$$ r_1 = M_1 \cdot a_3, \tag{6} \label{eq:six} $$

$$ r_2 = M_2 \cdot (E_u(r_1) +a_2), \tag{7} \label{eq:seven} $$

$$ r_3 = M_3 \cdot (E_u(r_2) +a_1), \tag{8} \label{eq:eight} $$

其中, $ {r_1, r_2, r_3} $表示识别特征. $ r_3 $ 如[图4](#rc)所示, 是最终的融合特征, 会输送到最高分辨率的识别器中.

如[图4](#rc)蓝色虚线所示, 识别损失$ L_{reg} $的梯度能够反向传播到检测特征中, 使得RC通过识别监督隐式改进检测头.

一般来说, 为了抑制背景, 融合特征会乘以检测头的输出$ L_{mask} $. 然而, 检测框不够精细, 特征图中仍然存在背景噪声. 这个问题可以通过RC模块缓解, 因为RC可以通过使用检测特征生成精细掩码抑制背景噪声, 整个过程受识别损失和检测损失监督.

如[图4](#rc)右上角所示, $ M_3 $相较于$ mask_{K} $更能抑制背景噪声, $ M_3 $在文本区域有着更高的激活, 在背景区域有着更低的激活.因此, RC模块输出的掩码$ \lbrace M_1, M_2, M_3 \rbrace $可以应用于识别特征$ \lbrace r_1, r_2, r_3 \rblace $, 使得识别器更好地专注于文本区域.

使用RC模块, 识别损失的梯度不仅可以影响backbone网络, 也会影响proposal特征.

通过检测和识别监督优化, proposal特征能够更好的编码文本高阶语义信息.因此, RC模块可以激励检测与识别之间的协调.


# 识别器

在特征图使用RC模块后, 背景噪声被有效抑制, 因此文本区域的框定更加精细. 这使得仅仅使用序列识别网络就可以得到有保证的识别结果, 而不需要矫正模块或字符级分割模块.

为了增强细粒度特征提取和序列建模，采用了双层自注意机制作为识别编码器.

两级自注意力机制分别包含局部邻域区域和全局区域的细粒度和粗粒度自注意力机制. 因此, 可以在维持全局建模能力的同时有效提取细粒度特征. 

解码器使用SAM.

识别损失如$ 式 \eqref{eq:nine} $.

$$ L_{reg} = -\frac{1}{T} \sum_{k = 1}^{T} \log p(y_i) \tag{9} \label{eq: nine} $$

其中, $ T $是序列的最大长度, $ p(y_i) $是序列概率.


# 实验

在ROIC3, ICDAR 2015, ReCTS, Vintext, Total-Text, SCUT-CTW 1500数据集上进行实验.


[表1](#table1)是模型在ICDAR 2015数据集上的结果, S, W和G分别代表强,弱和通用词典的识别.

<span id="table1">![Table1: ICDAR 2015](/images/swinTextSpotter_img/table1.png)</span>

[表2](#table2)是模型在ReCTS数据集上的结果, P, R, H分别表示准确率, 召回率和H-mean.

<span id="table2">![Table2: ReCTS](/images/swinTextSpotter_img/table2.png)</span>

[表3](#table3)是模型在RoIC13数据集上的结果, P, R, H分别表示准确率, 召回率和H-mean.

<span id="table3">![Table3: RoIC13](/images/swinTextSpotter_img/table3.png)</span>

[表4](#table4)是模型在VinText数据集上的结果.

<span id="table4">![Table4: VinText](/images/swinTextSpotter_img/table4.png)</span>

[表5](#table5)是模型在Total-Text数据集上的结果.

<span id="table5">![Table5: Total-Text](/images/swinTextSpotter_img/table5.png)</span>

[表6](#table6)是模型在SCUT-CTW1500数据集上的结果.

<span id="table6">![Table6: SCUT-CTW1500](/images/swinTextSpotter_img/table6.png)</span>

[表7](#table7)是模型的消融实验.

<span id="table7">![Table7: 消融实验](/images/swinTextSpotter_img/abl.png)</span>

[图5](#vis)是模型的消融实验.

<span id="vis">![图5: 结果可视化](/images/swinTextSpotter_img/vis.png)</span>


# 限制

对于长任意形状文本的识别有限制. 长任意形状文本的识别需要分辨率高的特征图, 当特征图越大, 在识别器中的注意力图也会随之扩张. 大的注意力图可能会导致识别器的不匹配, 从而导致端到端文字定位性能的下降.

