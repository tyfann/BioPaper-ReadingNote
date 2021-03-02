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

DSA平台提供统一化的注释风格和提供分析管理的接口，对于整个研究过程进行数据管理和任务分配。

#### 2.5 Annotation review process

以下区域在crowdsourcing过程中被标注：
i. tumor, stroma, lymphocyte-rich regions, necrosis（主要类）
ii. artifacts, adipose tissue, blood vessels, blood, glandular secretions, extracellular mucoid material（非主要类）
iii. plasma cells, mixed inflammatory infiltrates, normal ducts or acini, metaplastic changes, lymph vessels, skin adnexa, angioinvasion, nerves（有挑战性的类）

stroma作为最常见的组成部分，被认定为default类，不用给注释。JP和SP参与者集中精力在注释主要类上面，在注释非主要类和有挑战性的类时，需要在Slack平台上请求反馈。SP和研究协调员通过下面两种机制来检查标注错误：
i. 通过Slack向参与者提供反馈
ii. 在原始标注之上生成新的修正叠加标注
检查和校正分为两个阶段

#### 2.6 Measuring annotstion discordance

使用DSA的API查询多边形坐标并将其转化成脱机的掩码图像格式，其中像素值来编码区域类。对于参与者间不一致性的评估，使用Dice系数来计算：

<img src=https://tva1.sinaimg.cn/large/e6c9d24egy1go5itxaezkj20h604st8x.jpg width=70%>

其中i, j是两个参与者，对应的掩码i, j由c个二进制通道组成，$N_c$是所有考虑的类的总数。$\Delta_{i,j}$的取值范围在0到1之间，0表示完全一致。作者考虑采用两种方法来可视化参与者之间的不一致性：
第一种是参与者间不一致矩阵的 bi-clustered heatmap（双聚类热图），该热图根据不一致轮廓对参与者进行分组；
第二种是对不一致矩阵的MDS（多维标度）分析，该矩阵将参与者描述为二维空间中的点，其中越接近表示一致性越高。

#### 2.7 Semantic segmentation and classification models

预先训练好的完全卷积VGG-16和FCN-8神经网络被训练来讲组织学图像分割成五种类别：tumor, stroma, inflammatory infiltrates, necrosis, other classes. 平移变换和修剪等数据增强手段被用来提高模型鲁棒性。针对浸润性导管癌的125例ROI，对ROI的RGB图像采取颜色归一化。

首先，为了研究分别采用众包和单一专家注释的训练效果，训练了语义分割的“comparison”模型。这些模型使用evaluation集的ROIs注释进行训练，并使用校正后的core集注释上进行评估。

其次，为了评估峰值准确度，使用尽可能多的众包注释来训练用于语义分割的“full”模型。模型使用core集上的ROI注释进行训练，将82张slides（来自11家机构）分配给训练集，43张slides（来自7家机构）分配给测试集。这种按机构将ROI严格地分为要么训练集要么测试的处理手法，可以提供一种更好的对于数据开发的模型将如何推广到新机构和多机构的研究slides的度量。

最后，为了评估训练集大小对于预测模型准确度的影响，使用了不同数量的众包注释数据开发了“scale-dependent”图像分类模型。由于训练上百个语义分割模型时间有限，取而代之的改为训练基于预先训练好的VGG-16的分类模型将224\*224的像素块按三个主要类：tumor, stroma, inflammatory infiltration进行分类，在语义分割模型中采取相同的训练集/测试集分配方法。

### 3. Results

#### 3.1 Annotation concordance is class-dependent

不一致性随着种类的变化而变化，反映了在种类中固有的困难和主观性。Tumor注释的一致性最好，当只考虑主要类时，NP-NP注释的方差和SP-NP的偏差都比较低。

其中necrosis/debris出现了高度的不一致性，这反映了许多参与者要么当它存在的时候忽略错过了这一类，要么误把stroma当成了necrosis。

图例表显示了SP-NP像素平均的不一致性。Tumor的不一致大多发生在区域边界附近而lymphocytic infiltration和necrosis/debris的不一致性会更加的发散，且不局限于区域边界。

#### 3.2 Feedback improves annotation quality



## A deep learning approach for tissue spatial quantification and genomic correlations of histopathological images



## NuCLS: A scalable crowdsourcing, deep learning approach and dataset for nucleus classification, localization and segmentation



## Deep-learning–based characterization of tumor-infiltrating lymphocytes in breast cancers from histopathology images and multiomics data

