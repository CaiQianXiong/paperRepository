---
alias: MIC
发布时间: 2022-02-14
出品方: 腾讯 
价值: ⭐⭐⭐
---

环节:: 召回
关键词:: #Reco/双塔模型 #ML/自监督学习 

---

# Model-agnostic Integrated Cross-channel Recommenders 

## 概述
* 传统方法的不足：三种召回，u2u, u2i, i2i都只利用局部信息
* 提出model-agnostic integrated cross-channel (MIC), leverages the inherent **multi-channel mutual information** to enhance the matching performance. MIC exploits **correlation** within user-item, user-user, and item-item from latent interactions

  * 用一个模型(e.g., dssm)来统一做U2I, U2U, I2I三种召回.
  * Then we designed **cross-channel contrastive learning** techniques to boost a single model’s performance on three channels
* 说三渠道融合，有点牵强

  * 只不过增加了user侧和item侧的contrastive learning，相当于把u2u和i2i都学习出来了
  * 当然由于tower是共享的，embedding可能是共享的，增加的u2u和i2i contrastive learning导致user/item embedding学习得更好，从而惠及u2i，说得通，但是牵强

## Data Augmentation #Reco/样本策略

### Multi-level Perturbation

* 一个batch有N个user或item，每个user/item只做一次perturbation，那么共产生2\*N条样本
* we randomly masked the user fields, including attributes (Id, gender, age) and interaction sequence (item Id).
* we randomly masked attributes (item Id, keywords) and interaction records (user Id) of each item.
* 这里只是简单mask，没有google的[Correlated Feature Masking (CFM)](siyuan://blocks/20211114095148-cy97sl5) 那么复杂
* In addition to the field-level perturbations, the dropout is performed in the embedded features space. (就是训练中普通的dropout吧)
* original user/item，和其对应的perturbed user/item算是**正样本**，we treat the **other 2(𝑁 − 1) augmented examples within a mini-batch as negative examples**.

### 基于Embedding KNN的增强

* 论文中说，**fail to provide diversified samples**. 其实，我更同意google的说法，就是因为只用log里面的数据，不可避免带来sample selection bias。所以google在训练ssl时，[main task中的item与contrastive task中的item来自不同分布](siyuan://blocks/20211114095148-0u7yjqo)，ssl中的数据是来自整个corpus里的uniform sampling
* mic的作法，在user侧，we retrieve the anchor user’s k-nearest neighbor (**kNN**) in the representation space as the extension of user **positive pairs**. Besides, we adopt k-means++ to cluster the users and choose **users from different clusters as hard negative samples**.
* item与之类似，也是在embedding空间时里找extended positive & hard negative.

## 模型结构与训练

![网络结构图 | 500](https://api2.mubu.com/v3/document_image/1bb73dc2-c4d8-4c74-9be7-11b3fcf0cf07-11457030.jpg)

### user和item内部的contrastive learning

![|500](https://api2.mubu.com/v3/document_image/b2334713-4d59-427e-8315-999c9470240c-11457030.jpg)

### user~item之间的contrastive learning

就是传统双塔的任务，说是contrastive learning有点牵强

![|600](https://api2.mubu.com/v3/document_image/edc76282-8c2b-4f1d-b893-d04b09464fa2-11457030.jpg)

* 一般召回只是单侧的三元组<user, item+, item->，而这里还增加了<item, user+, user->
* 论文中没有明说，**但是我觉得在建模user~item之间的召回问题时，就没有必要考虑augmented user&item了**，反正数据已经够多了，都用上不现实，反而还要sampling

### 总loss

论文中的错误不值一驳

![|500](https://api2.mubu.com/v3/document_image/27baae2b-5814-4520-b3d2-81de78b0c257-11457030.jpg)


