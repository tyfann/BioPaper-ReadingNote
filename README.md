# BioPaper-ReadingNote

## Structured crowdsourcing enables convolutional segmentation of histology images

### Abstract

动机：要想深度学习模型有比较好的表现，大量的具有标签的数据集需要被用来创建模型。

结果：招募的25名参与者在幻灯片中的对于组织区域的描绘中，系统来评估他们之间的描绘差异，发现tumor和stroma的一致性较高，subjectively defined和rare tissue classes的一致性较低。
通过资深参与者的反馈，20000+带有标签注释的组织区域被用于生成模型。这样训练出来的卷积网络有非常高的精度（平均的AUC=0.945），同时标注数据的规模显著提高了图像分类精度。

### 1. Introduction

一个有着比较多临床经验的病理专家通常没有足够的时间来对临床图片进行标注以充分训练深度学习的模型。

图像注释crowdsourcing被广泛用于非医疗任务中，但在病理学领域，只仅仅停留在了相对简单的一些malaria diagnosis和immunohistochemical biomarkers任务中。近期的研究工作中，研究人员基于少量的幻灯片来观察一个小区域（400\*400至800\*800像素大小），并没有探索更有挑战性的语义分割任务。并且这一过往的研究工作没有探究如何将不同经验水平的参与者进行组织整合以及如何利用技术来协调经验较多和较少的参与者之间的合作以提高crowdsourcing的效率和准确率.

为了研究上述中的一部分问题，文章研究了breast cancer图像使用众包语义分割的应用。同时还描述了Digital Slide Archive（DSA）作为简化注释和审阅过程的工具。

### 2. Materials and methods

#### 2.1 Dataset Description

数据集包含了151张 hematoxylin 和 eosin 染色的 WSIs，对应151例breast cancer的病例。研究员和医生选择当中感兴趣的同时被 SP 批准的区域（ROI）。ROI的选择代表每张slide中主要的区域类别和纹理。避免了过大的ROI使得参与人产生疲劳导致质量下降。同时会尽可能选择高密度 tumor 区域，为了最大限度提高tumor所占ROI的比例以及尽可能减少花费在区分正常细胞和癌组织的需求。

#### 2.2 Participant recruitment and training

从网络社交媒体招募的25名参与者，分别是2名SP（高级医师），3名JP（初级医师）和20名NP（医学生）。这些参与者接受了具体的使用和教学培训。提供Slack在线平台供他们进行交流反馈。

#### 2.3 Structured crowdsourcing

结构化众包指基于参与者经验和专业知识的系统化任务分配。SP参与者帮助纠正NP参与者的注释。其中两种类型ROIs被标注：
i. 用于训练和验证算法（ALs）的core集（包含了151个大型的ROIs）
ii. 用于评价参与者一致性的evaluation集（包含了10个小型的ROIs）

每个参与者负责注释约5-6个（1/25的core集总量）core集的ROI和整个的包含10个ROI的evaluation集。SP和JP负责其中具有挑战性的21个ROI（如果某一ROI中一部分被相当的不常见的特征占据，则认为这一ROI具有挑战性），剩下130个被均匀分配给参与者。SP会对core集中的参与者给出的注释提供反馈和纠正。而evaluation集没有被提供反馈，防止对参与者的一致性分析产生偏差。

#### 2.4 The DSA annotation interface





## A deep learning approach for tissue spatial quantification and genomic correlations of histopathological images



## NuCLS: A scalable crowdsourcing, deep learning approach and dataset for nucleus classification, localization and segmentation



## Deep-learning–based characterization of tumor-infiltrating lymphocytes in breast cancers from histopathology images and multiomics data

