---
title: Pose-Normalized Image Generation for Person Re-identification论文阅读笔记
date: 
tags: [阅读笔记]
categories: "阅读笔记"
---

# 前言
>阅读论文笔记，GAN简介
<!--more-->

## 生成对抗网络（GAN)

* 生成模型：试图同时学习输入数据和标签的联合概率，即P(x,y)；也可以用于生成新的样本(x,y)
* 判别模型：学习将输入数据X映射到某个期望输出类别标签Y的函数
* GAN由生成器G和判别网络D组成
  * 生成器：将噪声作为输入，并生成样本；在训练中学习产生越来越多的现实样本；
  * 鉴别器：从发生器和训练数据中接收样本；学习如何更好地区分生成数据和真实的数据
* GAN与强化学习的主要区别在于：
  * GAN网络可以将梯度信息从“鉴别器”反向传播到“生成器”网络；以此协助生成器生成跟高的结果以“欺骗”鉴别器。



## Pose-Normalized Image Generation for Person Re-identification

### abstract

* 行人重识别的两个挑战：
  * 在存在大的姿势变化时缺乏交叉视图配对训练数据和学习辨别身份敏感和视图变化特征
* 本文提出了一种新的基于姿势的真实人物图像合成深度图像生成模型
* 利用GAN合成的图像，可以学习到一种新的re-id特征，不受姿态影响
* 在迁移学习的设置下，本文的模型可以很好的推广到任何新的re-id数据集中，不需要进行模型微调。

### Introduction

* 现有的一些re-id的方法，都是基于使用DNN学习身份敏感和视觉敏感特征
  * 但这种方法受到了一定的限制：
    * 缺乏大型摄像机网络的可扩展性
    * 缺乏对新摄像机网络的泛化能力：当换一个摄像机网络的时候需要收集额外的数据进行fine-tune
  * 由于上述的2个原因，即使深度学习在一些大的数据集上取得较好的成果，但在小型的数据集上（例如CUHK01)无法击败手工特征。
  * 即使有足够的训练数据，现有的深度re-id模型也面临着在大的姿态变化时，学习身份敏感和视图不敏感特征的挑战
  * 身份敏感：对应于语义相关的身份属性，例如性别，携带，服装风格，颜色和纹理。 试图不敏感：是前面提到的包括姿势的协变量。现存的模型都是希望能够保留前者而去除后者
* 本文认为，关键在于需要消除姿势对人的外观的影响
* PN-GAN
  * 输入：人物照片和理想的姿势
  * 输出：具有用新的姿势替换的原始姿势的相同身份的合成图像
* 在实践中定义了一组 八个规范姿势，并为任何给定图像合成8个新图像，使训练数据大小增加8倍
* **Contributions**
  * 将姿势确认为阻止深度学习模型有效身份敏感和视敏感特征的罪魁祸首，并提出一种基于生成姿态归一化图像的新颖解决方法
  * 提出了一种新的人物图像生成模型PN-GAN，用于生成保持身份和可控姿态的归一化图像
  * 考虑了一种更现实的无监督转移学习设置

### Related Work

* **Deep re-id models**
  * 不同的模型有不同的DNN算法
  * 不同的训练数据
  * 不同的loss值
    * 身份分类，成对验证和三元组排名损失
* 本文使用了 **ResNet** 和 **identity classification loss**

* **Pose-guided deep re-id**
  * 先前论文解决姿势问题都是**基于身体部位检测的姿势引导**
  * 本文则使用PN-GAN合成逼真的全身图像
* **Deep image generation**
  * 在所有GAN的变体中，我们的PN-GAN是建立在DCGAN(深度卷积GAN)

### Methodology

#### Problem Definition and Overview

* **Problem definition**
* **Framework Overview**

#### Deep Image Generator

*  图像生成器的目的在于生成不同姿势下同一个人的照片
* **Pose estimation**
  * 使用了现成的姿势检测工具包—Openpose
  * 输入一张图片，该工具包会检测出一张姿势图片，定位和检测了18个关键点和它们之间的连接
* **Generator**
  * 将target body image 作为一个三通道图片和原图片一起放入生成器G_p中
  * 生成器G_p是一个encoder-decoder网络
  * 它的encoder包括了9 ResNet basic blocks
  * 一般形状的ResNet可以用于将不变信息从编码器底层传送到解码器，并改变姿势的变量信息
  * 通过这种架构（参见图3），我们拥有两全其美：编码器 - 解码器网络可以帮助学习提取存储在瓶颈层中的语义信息，而ResNet块可以传递丰富的人员身份信息，帮助合成更逼真的图像，并改变姿势的变量信息，同时实现姿势归一化。
* **Discriminator**
  * 鉴别器D_p用于学习区分输入的图片是真是假

#### Person re-id with Pose Normalization

* 本文训练了2个re-id模型
  * 使用原始图像训练模型，在姿势变换的情况下提取不变的图片特征
  * 使用PN-GAN得到的标准化姿势合成图像训练，以计算没有姿势变化的re-id特征
* **Pose Normalization**
  * 使用预训练ImageNet的VGG-19在ILSVRC-2012上提取每个姿势图像的特征，并用K-means把姿势图像聚类成规范姿势
  * 从Market-1501中获取了8个姿势，生成器合成8章姿势照片替代原姿势
* **Re-id Feature with pose variation**
  * ResNet-50在ILSVRC-2012上预训练，对给定的训练集进行fine-tune，对训练数据进行分类
  * 本文中，在FC层后，合并了5a,5b和5c卷积层的ResNet-50结构合并成一个1024维的特征向量
  * 本个模型称为 **ResNet-50-A**
* **Re-id Feature without pose variation**
  * 第二个模型为**ResNet-50-B**
  * 该模型算法于上一个一样，但使用归一化姿势的合成图片进行学习
  * 因此获得了8个姿势的特征集合
* **Testing stage**
  * 训练后，通过RestNet-A得到一个特征向量；通过ResNet-B得到8个姿势的特征
  * 按元素方式最大化操作融合9个特征向量来获得一个最终特征向量
  * 最后，计算查询的最终特征向量和图库图像之间的欧氏距离，并使用距离图像对图库内容进行排名

### Experiments

#### Datasets and Settings

* Market-1501
* CUHK03
* DukeMTMC-reID
* CUHK01
* **Evaluation metrics**
  * Rank-1,Rank-5,Rank-10
  * mAP
* **Implementation details**
  * Tensorflow---PN-GAN 
  * Caffe---re-id feature learning
* **Experimental Settings**
  * 监督学习：对所有的数据集
  * 迁移学习：只对CUHK03\CUHK01\DukeMTMC-reID

### Supervised Learning Results

* **Results on large-scale datasets**
  * 在监督学习中，结果优于 ResNet-50-A,说明合成图片可以帮助re-id
  * 合成多个标准化姿势是处理姿势变化问题的更有效办法
* **Results on small-scale dataset**
  * 在CUHK01上，ResNet-50-A仍优于其他模型
  * 在使用PN-GAN后，我们的框架在监督设置中进一步提升了ResNet-50-A的性能3%以上
  * 通过姿态归一化，我们的模型更适应小型数据集，只有Market-1501预训练的模型可以在小型数据集上轻松微调，实现了比现有模型更好的结果。

#### Transfer Learning Results

* 这些结果表明，我们的模型有可能真正推广到新摄像机网络的新重新数据 - 当以“即插即用”模式运行时

#### Further Evaluation

* **Ablation Studies**
  *    将ResNet-50-A和ResNet-50-B直接放入最后测试中，发现：
    * （1）每个模型本身都非常强大 - 比之前的许多现有模型更好
    * （2）当组合两种类型的特征时，所有四个数据集的最终结果都有所改进
  * 在一个特定姿势下生成的图像的质量可能很差；因此，使用8个姿势降低了对生成图像的质量的敏感度
* **Examples of synthesized images**
  * 即使我们没有使用明确的属性来引导PN-GAN，但所生成的不同姿势的图像具有与原始图像大致相同的视觉属性
  * 我们的模型可以帮助缓解由遮挡引起的问题

### Conclusion









