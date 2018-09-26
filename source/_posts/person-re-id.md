---
title: Person Re-identiﬁcation:Past, Present and Future论文阅读笔记
date: 
tags: [阅读笔记]
categories: "阅读笔记"
---

# Person Re-identiﬁcation: Past, Present and Future

## Abstract

### 贡献：
1. 介绍person re-ID及其与图片分类和实例检索
2. 调查广泛的手工制作系统以及基于图像和视频的re-id方法
3. 描述了大型画框中端到端re-id和快速检索的关键与未来方向
4. 介绍重要但尚未完善的问题

## Introduction
* 人re-id技术在各方面的学习都遵循 **莱布尼茨定律**
* 在视频监控中，当被呈现给感兴趣的人(查询)时，人重新标识告诉此人是否在另一个地方(时间)被另一台照相机观察到
* 该任务出现的原因：
    - 公共安全需求的增长
    - 公园街道等广泛存在摄像机，靠人力去发现和跟踪成本昂贵
* 视频监控中的re-id可以分为：
    - 人检测
    - 人跟踪
    - 人物检索(大多数工作几种)
### Organization of This Suevey
* 本文主要关注于当前可用或可能在未来可见的不同重标识子任务，而不是非常详细的技术或体系结构

### A Brief History of Person Re-ID
* 任务Re-id起始于多摄像机追踪
*  **多摄像机跟踪**
    -  最早利用了贝叶斯估计一个相机中某物体出现的后验性
    -  外观模型包含了众多特征：颜色、车辆长度、高度和宽度、速度、观察时间等多时空特征
*  **re-id的多相机跟踪**
    -  2005年第一次提出re-id：重新确定一个人离开视野后重新进入
    -  在他们的方法中，输入者的ID是由一个近似贝叶斯推理算法计算的后验标签分布确定的。
*  **The independence of re-ID (image-based).**
    -  “Person reidentification using spatiotemporal appearance,”
*  **Video-based re-ID**
    -  最初的re-id专注于使用视频替代，后来在2010年，提出了用多帧re-id,其中帧随机选择
    -  颜色特征被两者都使用，而后者还另外使用了分割模型来检测前景
    -  对于距离的测算，两者都计算两个图像集中的边界框之间的最小距离，而后者还使用Bhattacharyya距离作为颜色和一般缩影特征
*  **Deep learning for re-ID**
    -  2014年，使用了Siamese神经网络来确定一堆输入图像是否属于同一ID

### Relationship with Classification and Retrieval
* 根据训练和测试类之间的关系，人员重新标识位于图像分类和实例检索之间
* 可以将人物re-id定位为利用分类和检索两者的优点，一方面使用训练类、判断距离度量或特征嵌入、可以在人物空间中学习

## IMAGE-BASED PERSON RE-ID
* Hand-crafted Systems
    - 在行人检测中，最常用的特征是颜色，而纹理特征则不怎么常用
    - 在13文章中，行人前景被从背景分割，并且为每个身体部分计算对称轴
    - 基于人体构型，计算加权颜色直方图（WH）、最大稳定颜色区域（MSCR）和递归高结构斑块（RHSP）。

* Datasets and Evaluation
    - VIPeR\iLIDS\GRID\CAVIAR\PRID2011\WARD\CUHK01\CUHK02\CUHK03\RAiD\PRID 450S\Market-1501，这些数据集分别包含不同类型的视频
    - 近年来，数据集的发展如下
        + 数据集规模越来越大
        + bounding box的开始采用predestrain detection获得
        + 收集时使用了更多的相机
    - 验证re-id效果时，常用CMC曲线来表示（查询标识出现在不同大小的候选列表中的概率。）
        + 因为CMC只能在计算第一次查询，所以当每个查询只有一个ground truth存在的时候，CMC是基本准确的
    - 为了解决多个groud truth的情况，提出了mAP
        + CMC和mAP在发现第一个基本事实方面具有相同的能力，但是mAP更具有检索回忆能力。
    - Re-ID Accuracy Over the Years
        + 三个主要的见解
            * 1.从6个数据集可以观察到性能的改进明显趋势
            * 2.除了VIPeR，深度学习方法在剩下的5个数据集上产生了新的艺术状态。
            * 3.推测仍有很大的改进空间，特别是当要发布更大的数据集时
        
## VIDEO-BASED PERSON RE-ID
* 基于视频的方法更加关注多镜头匹配方案和时间信息的集成

### Hand-crafted Systems
* 多镜头人物re-id，其中两组帧之间的相似性起着关键性作用
* 也可以采用多个身份标识来增强身体部位对齐
* 距离度量学习在匹配视频时也很重要

### Deeply-learned Systems
* 基于视频的数据量通常大于基于图像的数据集，因为每个轨迹包含多个帧
* 基于视频与基于图像的不同之处在于：对于每个匹配单元(视频序列)的多个图像，应该采用多匹配策略或视频池之后的单个匹配策略
* 基于池的方法将帧级特征聚合为全局向量，具有更好的可扩展性。
* 在本次调查中，我们注意到也许外观和时空模型的区别性组合是未来视频重标识研究的有效解决方案。

### Datasets and Evaluation
* multi-short re-ID数据库有：ETH\3DPES\PRID-2011\iLIDS-VID\MARS
* 从验证结果来看可以发现：
    - 1.ETZ数据集已经达到其性能饱和
    - 2.ILID VID和PRID-2011数据集仍在进行视频Re-ID研究
    - 3.深度学习方法在基于视频的RID中产生了绝对优越的精度。

## FUTURE: DETECTION, TRACKING AND PERSON RE-ID
### Previous Works
* 尽管person re-id来源于多摄像机追踪，但它被认为是一个独立的研究任务。但在未来会结合pedestrian detection and tracking
* 特别的，文章认为end-to-end re-ID 系统（spotting a query person from raw videos），把raw videos作为输入，整合pedestrian detection 和tracking，再进行re-identification。
* 目前大部分的re-id工作都假定两点：
    - 给出行人边界框
    - 手绘边界框
* 但在实际情况中，上述两种假设可能不成立：一方面，gallery 大小会随着detector threshold而变化。低的阈值会产生更多的bounding boxing（更大的gallery，高的recall，但低的precision），反之亦然。re-ID检测的准确度将会由于不同的阈值，而不问题。另一方面，使用pedestrian detectors，bounding boxes中不可避免的会出现错误（misalignment, miss-detection, and false alarms），这将大大影响re-ID的检测准确性，这现在还很少有人考虑这个问题。
* 为了解决上述的第二个问题，尝试使用了CUHK03、Market-1501和MARS。这些数据集不承担完美的检测/跟踪输出，并且是更接近实际应用的一步。

### Future Issues
* System Performance Evaluation
    - 对于端到端的re-id来说，没有一个单一正确的协议
    - 文章从两方面提出re-id评估问题
        + 首先，在RID中使用有效的评估指标对行人检测和跟踪至关重要
            * AP/MR的计算，这个涉及IoU的值，试验结果表明，IoU阈值取0.7要比取0.5，检测精度更加稳定
        + 评价过程的第二个问题涉及整个系统的RID精度。
        
    - 记住gallery的大小取决于检测器的阈值，其他信息量大且无偏的新的评价度量可以在将来设计。
    - 文章给出了另一个端到端re-id评估协议
        + 在实践中，当被呈现为查询边界框/视频序列时，虽然在某一帧中定位身份并通过行人检测/跟踪来告诉其坐标是件好事，但是系统仅知道身份重新出现在哪个帧中也是可以接受的。
        + 本质上，确定被查询人出现的确切帧比确定“检测/跟踪+再确认”任务相对容易，因为检测/跟踪错误可能不会产生大的影响
        + 可以学习检测器/建议书以定位具有松散IoU限制的粗略行人区域，并且更加强调匹配，即，从较大的边界框/时空管中找到特定人
* The Influence of Detector/Tracker on Re-ID
    - 人员重新身份识别起源于行人跟踪，其中如果确定来自多个摄像机的轨迹具有相同的身份，则将它们相关联
    - 对于端到端的人物re-id,必须理解检测/跟踪对重新标识的影响，并且提出检测/跟踪方法/数据能够帮助重新标识的方法。
        + 首先，行人/跟踪误差确实会影响重新标识的准确性，但是其内在机制和可行的解决方案仍然存在挑战。
        + 其次，我们应该知道，检测和跟踪，如果适当设计，可能有助于re-id
    - 行人检测/跟踪可以帮助重新识别身份或反过来，这似乎并不十分直接，但如果考虑一般图像分类和细粒度分类的类比，我们可能会想到一些线索

## FUTURE: PERSON RE-ID IN VERY LARGE GALLERIES
* 近年来Re-ID社区的数据规模明显增加
* 无论从研究和应用的角度来看，在大型画廊中重新识别人物应该是未来的一个关键方向。应努力提高准确性和效率问题。
* 一方面，描述符和距离度量的鲁棒和大规模学习更为重要。
    - 虽然当前的方法解决了在非常有限的时间窗中一个或多个相机对之间的重新标识问题，但是相机网络中长时间段的鲁棒性尚未得到充分考虑。
* 另一方面，效率是这样大规模设置中的另一个重要问题。
* 利用图像搜索，我们得到了两个可能提高效率的方向
    - Inverted index-based
    - Hashing-based
* 因此，这个调查需要非常大的重新ID数据集，这些数据集将评估重新ID方法和可伸缩算法的可伸缩性，特别是那些使用哈希代码来进一步将这个任务推向真实世界的应用程序的算法

## OTHER IMPORTANT YET UNDER-DEVELOPED OPEN ISSUES
### Battle Against Data Volumn
* 标注大型数据集一直是视觉界关注的焦点。
* 这两年发布了几个大的数据集，但仍不能令人满意。于是本文提出两种替代策略帮助绕过数据问题
    - 首先，如何使用跟踪和检测数据集的注释仍在探索中
    - 第二个策略是将学习模型从源到目标域的转移学习。

### Re-ranking Re-ID Results
* 重新识别过程可以看作是一个检索任务，其中重新排序是提高检索精度的重要步骤
* Re-ranking在Re-ID中仍然是一个开放的方向，在实例检索中得到了广泛的研究
* 由于Re-ID是专注于行人，重新排序方法可以具体设计。

### Open-World Person Re-ID
* 目前大部分re-id工作都可以被看成是独立任务
* 相比之下，open-word的Re-ID系统研究了人的验证问题
* open-word ReID仍然是一项具有挑战性的任务，在低误接受率下的识别率很低，其挑战主要在两个方面 
    - detection
    - recognition
* 100%的FAR对应于标准近集重新标识，并且其精度受到当前技术状态的限制；较低的FAR伴随着较低的重新标识精度，这是由于真实匹配的召回率低。

## CONCLUDING REMARKS
* 本文对人的再识别问题进行了综述。首先，简要介绍了人物重标识的历史，描述了其与图像分类和实例检索的异同。然后，回顾现有的基于图像和视频的方法，它们被分类为手工制作和深入学习的系统。
