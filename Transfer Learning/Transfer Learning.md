**本文转载自CSDN大佬的文章，自己做了一点点改动。[原文地址](https://blog.csdn.net/dakenz/article/details/85954548)**
# 迁移学习
## 背景
随着越来越多的机器学习应用场景的出现，而现有表现比较好的监督学习需要大量的标注数据，标注数据是一项枯燥无味且花费巨大的任务，所以迁移学习受到越来越多的关注。  
**有监督的机器学习** 基于
**分布假设**所以需要大规模数据，但是实际中数据可能存在很多问题，比如数据分布差异，或者标注数据过期等等。
面临的问题就是怎么废物利用，用**前朝的剑，斩今朝的官**，于是就有了**迁移学习**
<img alt="Transfer Learning-ae96f471.png" src="assets/Transfer Learning-ae96f471.png" width="" height="" >
## 迁移学习定义分类
**Transfer Learning Definition:**
_Ability of a system to recognize and apply knowledge and skills learned in previous domains/tasks to novel domains/tasks._
### 目标
将某个领域或任务上学习到的知识或模式应用到不同但相关的领域或问题中。
### 思想
从相关领域中迁移标注数据或者知识结构、完成或改进目标领域或任务的学习效果。**机器能不能做到举一反三**  
### 提出了 域 和 任务 两个概念
**域（Domain）**就是不同领域，同样是图像处理，医学影像处理和生物影像处理就不是一个领域  
**任务（Task）**就是要干啥，脑瘤检测和猫狗识别就是两个任务
## 核心问题
哪些知识可以迁移？怎么针对具体问题迁移，采用何种算法？什么情况下适合迁移？  
核心是概率分布，分布概率相差特别大的时候，会出现**负迁移现象**，即旧知识阻碍新知识。  
<img alt="Transfer Learning-dcc3b044.png" src="assets/Transfer Learning-dcc3b044.png" width="" height="" >
## 三种迁移方法
### 基于实例的迁移
从源领域中挑出对目标领域有用的，给他重新分配权值，让它的分布结进目标领域的真实分布。
<img alt="Transfer Learning-7ee4fd7c.png" src="assets/Transfer Learning-7ee4fd7c.png" width="" height="" >
### 基于特征的迁移
先进行**特征选择**，找出源领域与目标领域共同的特征，基于这些特征做迁移。重点是将源数据原始特征空间映射到目标领域寻得特征空间，目标也是为了让源数据在目标领域中的分布更贴合目标领域真实的特征分布。
### 基于共享参数的迁移
找到源数据和目标数据的空间模型之间的共同参数或者先验分布，从而可以通过进一步处理，达到知识迁移的目的，假设前提是，学习任务中的的每个相关模型会共享一些相同的参数或者先验分布。

## 深度学习和迁移学习结合
深度学习需要大量的高质量标注数据，Pre-training + fine-tuning 是现在深度学习中一个非常流行的trick，尤其是以图像领域为代表，很多时候会选择预训练的ImageNet对模型进行初始化。
<img alt="Transfer Learning-188c60bc.png" src="assets/Transfer Learning-188c60bc.png" width="" height="" >
下面将主要通过一些paper对深度学习中的迁移学习应用进行探讨
2014年Bengio等人在NIPS上发表论文 How transferable are features in deep neural networks，研究深度学习中各个layer特征的可迁移性（或者说通用性）
文章中进行了如下图所示的实验，有四种模型

Domain A上的基本模型BaseA
Domain B上的基本模型BaseB
Domain B上前n层使用BaseB的参数初始化（后续有frozen和fine-tuning两种方式）
Domain B上前n层使用BaseA的参数初始化（后续有frozen和fine-tuning两种方式）
<img alt="Transfer Learning-fb403c0f.png" src="assets/Transfer Learning-fb403c0f.png" width="" height="" >
将深度学习应用在图像处理领域中时，会观察到第一层（first-layer）中提取的features基本上是类似于Gabor滤波器(Gabor filters)和色彩斑点(color blobs)之类的。
通常情况下第一层与具体的图像数据集关系不是特别大，而网络的最后一层则是与选定的数据集及其任务目标紧密相关的；文章中将第一层feature称之为一般(general)特征，最后一层称之为特定(specific)特征
<img alt="Transfer Learning-2095ec4f.png" src="assets/Transfer Learning-2095ec4f.png" width="" height="" >
* 特征迁移使得模型的泛化性能有所提升，即使目标数据集非常大的时候也是如此。
* 随着参数被固定的层数n的增长，两个相似度小的任务之间的transferability gap的增长速度比两个相似度
* 大的两个任务之间的transferability gap增长更快 两个数据集越不相似特征迁移的效果就越差
* 即使从不是特别相似的任务中进行迁移也比使用随机filters（或者说随机的参数）要好
* 使用迁移参数初始化网络能够提升泛化性能，即使目标task经过了大量的调整依然如此。
