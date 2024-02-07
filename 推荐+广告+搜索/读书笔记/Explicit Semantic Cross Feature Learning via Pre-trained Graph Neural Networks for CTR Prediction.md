---
alias:
发布时间: 2021-01-01
出品方: 阿里妈妈
价值: ⭐⭐⭐⭐
---

环节:: 精排
关键词:: #Reco/特征交互  #Reco/辅助训练 #ML/GNN

---
# Explicit Semantic Cross Feature Learning via Pre-trained Graph Neural Networks for CTR Prediction

## 概述

### 提出问题

处在Hidden Layers - Feature Interaction Modeling 优化路径上，文章提出一种针对显性交叉语义表征的预训练图学习方案。

交叉特征对于点击率预估模型而言至关重要，实际建模提效的过程会将交叉特征的设计分为两类：

* **隐式语义建模** ，两个id特征共现交叉完对应的Embedding表征，例如<user_age,item_id>交叉特征对应的Embedding隐式语义向量；
  
  * 最直接的方式，就是<user_age, item_id>直接hash再映射到embedding table的某个位置，这是一个纯粹的交叉特征，只负责表达交叉的语义，不会与单独表示user_age/item_id的任务相冲突，但是会超级稀疏。有阿里的CAN来解决这样的问题。
  * 现有大多数方法（例如DeepFM、DCN等）主要聚焦在前者，通过模型结构的设计来充分**拟合**隐式语义表征；
  * 缺点：we can never guarantee the learned cross features are what we want

* **显式语义建模** ，==两个id特征共现交叉完对应的历史统计值等，==
  
  * For example, we can count the times that a user who is a basketball player clicks "Nike-Air Jordan" in the history, and take this **counting value as the explicit semantic cross feature <user_occupation=basketball player, item=Nike-Air Jordan> for the samples having the same user occupation and items**.
  * 很少有工作会通过模型结构设计来处理后者，事实上显式建模信号对CTR任务非常有效，是业界提效的“公开的秘密”。

然而，**直接利用交叉特征的统计值作为显性建模信号**存在两大挑战：

* ==算法侧泛化性能较差，交叉特征的统计值依赖历史出现的共现特征==，非共现特征表达无能为力；比如，同样是<职业，品牌>这样的统计特征，我们能统计出`<运动员，乔丹>`的历史统计数据，但是却无法置信地统计出`<程序员，乔丹>`的统计数据。
* ==系统侧存储规模开销巨大，存储开销对应特征笛卡尔积的量级==，在线需要配备额外的分布式存储引擎，通信时延又会进一步影响计算性能。

### 方法简述

针对上述挑战，我们提出一种基于**预训练**图神经网络的交叉语义特征学习模型(PCF-GNN)。

* 图节点表示特征，边表示交叉特征的历史交互信息，

* **预训练时**，通过链边预测的方式拟合交互节点的边权重信息，从而显式建模交叉语义表征。

* 训练主任务时，
  
  * 预训练好的GNN发挥一个特征提取器的任务，对于输入的两个特征（**哪些特征要显式交叉，在综合考虑效果与成本后，人工决定**），能够预估并返回这两个特征之间的一些statistical value。
  * **we can simply infer rather than store the explicit semantic cross features**，节省存储空间。we can reduce the memory cost from $𝑂(𝑁_1 𝑁_2 )$ to $𝑂((𝑁_1 + 𝑁_2 )𝑑)$.(d是node embedding dim)

## 预训练

### 建图

construct an informative graph where 

* nodes refer to features,
* edges refer to feature interactions and
* the edge attributes refer to statistical explicit semantic cross features (比如下图中计算的就是CTR)

![image.png | 500](assets/image-20211205164045-xu1bepm.png)

### 聚合邻居节点

![image.png | 300](assets/image-20211205165017-8jzufq0.png)

* $h_𝑖^k$ denotes the output vector for node 𝑖 at the 𝑘-th layer
  
  * $h_i^0$j是第i个节点（i.e., 第i个特征）的原始特征表示，其实就是一个**可学习的embedding**
  * $h_i^K$是K层卷积过后最终embedding，**作为特征embedding初始值，喂入主模型**

* $𝑁_𝑟(𝑖)$ refers to the neighbors have the relationship 𝑟 with the node

* $m_{i,r}^k$表示第i个节点，在r种关系下所有邻居$𝑁_𝑟(𝑖)$ 的node embedding的聚合结果。

### 边预测

* 常规的边预测，CrossNet can be a simple dot production or a single- or multi-layer perceptron (MLP). ![image.png | 200](assets/image-20211205171541-w8km7xu.png)
* 计算预测出来的边上的statistical value ($p_{u,v}$)与实际统计出来的statistical value ($a_{u,v}$)之间的MSE。**但是事情没那么简单**。
* 我理解还是**edge上的statitical value的置信度的问题**。the pair <basketball player, "Nike-Air Jordan"> may appear more times than the pair <basketball player, mouse> in history. If we treat these two pairs equally in loss calculation, the learned PCF-GNN may be **difficult to distinguish which kind of cross features is important** for the interaction modeling and may mislead the CTR model to recommend a mouse to a basketball player.
* To address this problem, we propose a simple but effective weighted square loss, which allocates ==different weight to different edges by their co-occurrence== (**某特征对出现次数越多，这个特征对的统计值也就越置信**)。![image.png | 300](assets/image-20211205172304-711qznx.png)

## 主任务训练

* 通常为了节省主模型的训练时间，没必要fine tune，所以在训练主任务期间，PGF-GNN的参数固定，只是充当特征提取器
* we can take the predicted $𝑝_{𝑢,𝑣}$ as an **additional feature**, and concatenate it with the embeddings of the CTR model
* we drop the node encoding module for the efficient purpose and directly keep $h_i^K$ as the pre-trained embedding

![image.png | 500](assets/image-20211205172458-bex50st.png)

## 思考
* 其实就是在主模型之外，训练一个特征提取器： 输入两个categorical feature，输出是这两个categorical feature embedding，和二者之间的relation score（具体物理含义要看当初喂入模型的label是什么了，如果当初喂入的label是CTR，那么预测出的就是predicted ctr）
  * 要达到这样的目的，==不是非要GNN不可，一个双塔模型同样可以==。甚至将塔去掉，`直接拿dot(user feature embedding, item feature embedding)去和statistical value做MSE来预训练`，也同样能够work。
* 所提出的方法，<font color="blue">只能解决某个罕见的特征对对应的statistical value的问题，但是组成这个罕见特征对的两个特征必须是常见的</font>

