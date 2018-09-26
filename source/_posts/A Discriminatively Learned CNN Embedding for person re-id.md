---
title: A Discriminatively Learned CNN Embedding for Person Re-identification 论文阅读笔记
date: 
tags:[阅读笔记]
categories: ”阅读笔记“
---

# 前言
>论文阅读笔记
><!--more-->
>

# A Discriminatively Learned CNN Embedding for Person Re-identification

## abstract
* 该文章将2个CNN网络用于person re-id：verification and identification models
* 提出了一个同时计算识别损失和验证损失的暹罗网络。

## introduction
* 人物re-id通常被认为是一个 **图像检索**问题，匹配来自不同摄像机的行人
* 该领域的最近进展为：
    - 大规模可用的行人数据集
    - 利用CNN的嵌入
* 验证模型以图片组作为输入，判断他们是否属于同一个人。但验证网络缺乏考虑图像对与数据集中的其他图像之间的关系
* 分类模型是将任务re-id作为一个多分类任务，他们直接学习从输入图像到人的ID的非线性函数，在最后一层之后使用交叉熵损失。
* 上述的两种模型由互补的有点和局限性，结合两个网络的优势并利用它们的互补性来提高学习嵌入的辨别能力
* **本文的主要贡献**：
    - 提出一个siamese网络，有两个loss值：识别loss和验证loss
    - 报告了在两种大型人员re-id数据集，和一个实例检索数据集上的状态方法相比的竞争准确性

## RELATED WORK
## PROPOSED METHOD
### preview

* 在验证模型中： 
    - 两个input image中有几种操作
    - 数据之间的显式关系是通过成对比较建立的，例如部分匹配或对比损失
    - 对于相似的图像来说，所学习的嵌入最终是接近的，而对于不同类型的图片而言则是远离的
    - 由于使用了弱标签，验证模型考虑了有限的关系

### Overall Network
* 整个网络是基于一个siamese网络，结合了验证损失和分类损失
* 这个网络用于同时预测两个图形的ID和相似性得分
* **该网络用Imagenet与训练两个CNN网络，3个额外的卷积+1个平方层+3个损失组成**

### Identification Loss
* 在算法中有两个caffenet(CNN),它们共享权值并同时预测输入图像的身份标签
* 这里使用了交叉熵进行身份验证

### Verification Loss
* 在验证模型中，行人描述符f1;和识别模型中的f2是直接由验证损失来监督的
* 引入了一个 **非参数层的square layer**来比较高级特征
* 如果图像对描绘了同一个人，则 $q_1=1,q_2=0$,否则，$q_1=0,q_2=1$

### Identification vs. Verification
* 从Market1501中选取三张测试图片，一个图像被认为被很好地检测到，而另外两个图像没有很好地对齐
* 所提出的模型利用了两个网络，并且新的激活图几乎是两个单独的地图的联合
* 从可视化结果图来看，我们发现网络通常对人的中心部分（通常是衣服）有很强的关注，它也说明了衣服的颜色是人们重新识别的主要线索

### Training and Optimization
* **Input preparation**
    - resize image 256*256
    - cropped to 227*227(caffeNet) 224*224(ResNet or VGG) and mirrored horziontally
* **Training**
    - 使用Matconvnet训练和测试,CNN网络使用CaffeNet、VGG16、ResNet-50
    - maximum number of training epochs is set to 75 for ResNet-50, 65 for VGG16net and 155 for CaffeNet
    - The batch size (in image pairs) is set to 128 for CaffeNet, 48 for VGG16 and ResNet-50.
    - The learning rate is initialized as 0.001 and then set to 0.0001 for the final 5 epochs.
    - batch size: 128 for caffenet\ 48 for VGG and ResNet
    - The learning rate is initialized as 0.001 and then set to 0.0001 for the final 5 epochs
    - 梯度下降：SGD
    - 由于有 three objectives在网络中，所以：
        + 首先分别计算每个目标产生的所有梯度
        + 将加权渐变添加到一起以更新网络。
    - 我们将校验损失产生的梯度的权重分配为1，将两个识别损失产生的两个梯度的权重分配为0.5
    - 在最后一层卷积之前，插入了dropout
* **testing**
    - 仅通过激活一个模型来提取特征
    - 输入一个227*227的图像，获得一个4096维的行人描述符f
    - 我们对查询与所有图库特征之间的余弦距离进行排序，以获得最终的排名结果
    - 当特征是L2标准化的时候，余弦距离是欧式距离

## EXPERIMENTS
### Dataset
* Market1501 contains 32,668 annotated bounding boxes of 1,501 identities.
* CUHK03 dataset contains 14,097 cropped images of 1,467 identities collected in the CUHK campus
* Oxford5k buildings consists of 5062 images collected from the internet and corresponding to particular Oxford landmarks.

### Person Re-id Evaluation
* **Comparison with the CNN baseline**
* **Cross-entropy vs. Contrastive loss.**
    - 在该网络中，我们用对比损失代替了交叉熵
    - 结果推测认为，对比损失倾向于 **过拟合**重新ID数据集，因为没有将正则化添加到验证中
    - 所以本文设计的交叉熵添加了dropout以预防过拟合
* **Comparison with the state of the art**
* **Results between camera pairs.**
* **Large-scale experiments**

## Instance Retrieval


