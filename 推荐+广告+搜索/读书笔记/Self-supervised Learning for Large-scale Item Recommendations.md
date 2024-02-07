---
alias:
发布时间: 2021-02-25
出品方: Google
价值: ⭐⭐⭐⭐⭐
---

环节:: 召回
关键词:: #Reco/双塔模型 #ML/自监督学习 

---

# Self-supervised Learning for Large-scale Item Recommendations 

## 概述

* Long-tail items的label非常sparse
* ==目标是用对比学习的方法，将long-tail item embedding学习好==

## **Self-Supervised Learning (SSL)基础**

**The basic idea is to enhance training data with various data augmentations, and supervised tasks to predict or reconstitute the original examples as auxiliary tasks.**。NLP中的bert这种mask and predict missing word就算是SSL

### SSL的优点，

* 一是不必人为收集label，
* 二是autonomous discovery of good semantic representations by **exploiting the internal relationship of input features**.
* 也作为一种更好的**regularization**. 这里是所谓的**spread-out regularization**, promoting large instance separations in the embedding space.

### SSL Framework

![|500](https://api2.mubu.com/v3/document_image/a12f6c2d-8944-4ee4-b8fa-c207a0d39ea8-11457030.jpg)

1. augment data by masking input information;
2. encode each pair of augmented examples by a two-tower DNN; **尽管pass两个塔，但是塔的参数可以是共享的**(也可以不共享)，**类似SimCLR**
3. apply a contrastive loss (i.e., **auxiliary loss**) to learn representations of augmented data. 也就是尽管augmented之后，两条样本长得可能差别很大，但是分别经过tower后得到的embedding应该是非常相似的。（公式中的N是batch中的样本数）

![|700](https://api2.mubu.com/v3/document_image/d0a1a968-a7f4-4db2-8e60-6cd85b9ee4fe-11457030.jpg)


## 基于SSL的双塔召回

![|1000](https://api2.mubu.com/v3/document_image/c72ccd4b-b78b-409c-a597-0119c9671179-11457030.jpg)

### item data augmentation

#### 传统方法

Given a set of item features, the key idea is to create two augmented examples by **masking** part of the information.

* **Masking**. Apply a masking pattern on the set of item features.

  * we could randomly split the feature set into two disjoint subsets into the two augmented examples. We call this method **Random Feature Masking (RFM)**.
  * 但是，随机masking，会导致模型会利用“多个相关特征的不完整”而轻易做出判断。为此本文提出[CFM](siyuan://blocks/20211114095148-cy97sl5)。
* **Dropout**. For categorical features with **multiple values**, we drop out each value with a certain probability.

#### **Correlated Feature Masking (CFM)** 

we further explore the feature correlations when creating masking patterns.

1. pre-calculate两个categorical feature Vi,Vj的mutual information，也就是在Vi, Vj这两个vocabulary set中每对特征出现的概率

    ![|500](https://api2.mubu.com/v3/document_image/56d755ba-1279-4927-8716-665baf3971b8-11457030.jpg)
2. by first uniformly sample a seed feature $𝑓_{𝑠𝑒𝑒𝑑}$ from all the available features 𝐹 = { 𝑓 1 , ..., 𝑓 𝑘 },
3. and then select the top-n most correlated features 𝐹 𝑐 = { 𝑓 𝑐,1 , ..., 𝑓 𝑐,𝑛 } according to their mutual information with 𝑓 𝑠𝑒𝑒𝑑 .
4. The final 𝐹 𝑚 will be the union of the seed and set of correlated features, i.e., 𝐹 𝑚 = { 𝑓 𝑠𝑒𝑒𝑑 , 𝑓 𝑐,1 , ..., 𝑓 𝑐,𝑛 }.

    We choose 𝑛 = ⌊𝑘/2⌋ so that the masked and retained set of features have roughly the same size.

### 离线训练

![|500](https://api2.mubu.com/v3/document_image/d75fa0c9-0b73-453b-836e-0351d648d247-11457030.jpg)

#### 主要任务

experiment中的main task是一个item-to-item retrieval问题 

* Note that we only collect positive examples, i.e., 𝑥 𝑐𝑎𝑛𝑑𝑖𝑑𝑎𝑡𝑒 is an installed app from the landing page of 𝑥 𝑠𝑒𝑒𝑑 . **All the impressed recommended apps with no installs are all ignored** since we consider them more like weak positives instead of negatives for building retrieval models.

  ![|500](https://api2.mubu.com/v3/document_image/ff80c376-95ef-4c23-ae38-96ac25615908-11457030.jpg)
* 所以一个batch只包含N个正样本，batch softmax如下。采用了batch中其他item作为负样本放入分母，而且还有温度系数。

#### 辅助任务

**main task中的item与contrastive task中的item来自不同分布** #Reco/样本策略

* main task中的item来自曝光数据，绝大部分是**头部item**
* 这篇文章的目的就是为了解决long-tail item的问题，因此ssl task中的item不能也来自log. =="we sample items uniformly from the corpus for $L_{SSL}$"==
* 注意，main task和SSL task的正样本item应该采用不同的分布，才能取得debias的效果。“==In practice, we find using the heterogeneous distributions for main and ssl tasks is critical for SSL to achieve superior performance.==“

#### 共同训练

* contrastive loss作为辅助loss和main task一同训练。
* 因为[主辅任务间item的不同分布](siyuan://blocks/20211114095148-0u7yjqo)，那么auxiliary task是如何辅助main task的呢？答案是==参数共享==

  * “In order to make SSL facilitate the supervised learning task, we ==share the embedding table== of sparse features for both neural encoder”
  * 如基于SSL的双塔召回所示，The whole item tower (in red) is ==shared== with the supervised task. 但是也不是绝对，"**Depending on the technique for data augmentation (ℎ, 𝑔 ), the MLPs of H and G could also be fully or partially shared.**"

#### 待解决的问题

* 本文用的是main task+ssl task jointly train的schema 共同训练，而非cv/nlp中那样pre-training+fine tuning那样的训练范式
* we plan to investigate how different training schemes impact the model quality. One direction is to **first pre-train** on SSL task to learn query and item representations and **fine-tune** on primary supervised tasks.