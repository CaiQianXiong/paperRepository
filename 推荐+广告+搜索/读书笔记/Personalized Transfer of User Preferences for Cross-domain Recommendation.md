---
alias:
发布时间: 2021-12-19
出品方: 腾讯微信
价值: ⭐⭐⭐⭐
---

环节:: 
关键词:: #Reco/多场景推荐 #Reco/新用户冷启 

---

# [Personalized Transfer of User Preferences for Cross-domain Recommendation](https://arxiv.org/abs/2110.11154)
[GitHub](https://github.com/easezyc/WSDM2022-PTUPCDR) 

## 概述

整篇的思路是如何学到target domain中的user embedding。有了这个target domain user embedding

* For the extreme cold-start users who have no interactions in the target domain, **directly utilize** the initial embedding for prediction.
* For the warm-start users who have some interaction in the target domain, it is convenient to **fine-tune** the initial embeddings


之前的工作
* 认为user兴趣从source domain-->target domain的迁移规则是通用的，因此learn a common preference bridge就可以了。本文打破这一限制，it is necessary to use **personalized bridges** to model various relationships between user preferences in different domains.
* existing bridge-based methods adopt a mapping-oriented optimization procedure to **directly minimize the distance between the transformed users’ embeddings from the informative source domain and the user’s embedding in the target domain**. In other words, with such an optimization procedure, the bridge function is **sensitive to the quality of the users’ embeddings**. In practical recommender systems, it is pretty **hard to learn reasonable embeddings**

![image.png | 500](assets/image-20220110185733-l9afs4s.png)

![image.png | 700](assets/image-20220111114753-p9ax9mw.png)

## Step1: Pre-training
1. Learning a source model which contains $𝒖^𝑠 , 𝒗^𝑠$ .
2. Learning a target model which contains $𝒖^𝑡 , 𝒗^𝑡$ .

就是普普通通的训练模型，拿source domain/target domain的数据，**分别**在两个域中将user tower学习好，给定一个user就能得到user embedding。

这里有一个极大的槽点，就是这个attention有点迷，**问题是既没有“和target item做attention”，也没有“和邻居做attention”，就是“自己和自己做attention”**

![image.png | 200](assets/image-20220111114610-r00dvww.png)

![image.png | 300](assets/image-20220111114636-15yia53.png)

* $p_{ui}$是**transferable characteristic** embedding of user $𝑢_𝑖$ ,
  * **注意$p_{ui}$和完整的user embedding不同，
    * $p_{ui}$是meta network的输入，
    * meta network的输出是personal bridge，
    * 而personal bridge的输入才是whole source-domain user embedding
  * 可以与**LHUC类比，$p_{ui}$只相当于gate input**。
    * 你当然可以拿整个concatenated embedding当gate input，但是肯定不是一个好注意。
    * gate input应该选那么有区分性的、高bias的特征，也就是文章中的**transferable characteristic**.
  * 注意这里的$p_{ui}$只有source domain user action list相关。
* $v_j^s$是source domain中，每个user的action list(i.e., $S_{ui}$)中的某个item


## Step2: Meta Network
Meta Network的概念有点陌生，但是与LHUC类比就很清晰了

* ![image.png | 200](assets/image-20220111120510-bnbtape.png)
  * $\phi$是meta network的权重，
  * $w_{ui}$是personalized bridge weight，接下来会用到（**用之前可能要reshape一下**）
  * <span style="color:orange;font-weight:bold">meta-network类似于LHUC中的gate network。LHUC gate是生成personalized filter weight，这里是生成更general weight（不仅仅用于filtering）</span>

* ![image.png | 200](assets/image-20220111120832-10dkvvq.png)

  * $w_{ui}$是personalized bridge weight
  * $u_i^s$是第i个用户的source domain embedding（这里是完整的user info embedding，区别于$p_{ui}$只包含[transferable characteristics](siyuan://blocks/20220111115030-2l7iphc)）
  * $\hat u_i^t$是transformed target-domain user embedding


## Step3: Fine-tune
After training, we feed user embeddings in the source domain into the meta-generated personalized bridge functions and obtain the **transformed user embeddings. The transformed user embeddings are utilized as the initial embeddings in the target domain**. With the initial embeddings, our method is effective for cold-start users who have no interactions in the target domain

---
原来bridge方法，是训练embedding在source/target domain的distance，![image.png | 200](assets/image-20220111121339-frwhr7f.png)。这种loss对embedding质量敏感，学不好。

---
thus, to train the meta network, we take a task-oriented optimization procedure, which **skips the users’ embeddings in the target domain and directly utilizes the rating task as the optimization goal**
![image.png | 300](assets/image-20220111121432-l67wr8n.png)

有两点好处：
* 用了用户直接反馈
* 有更多的训练数据，原来一个overlapped user只能训练一次distance closeness，现在这个overlapped user所有rating都可以用于训练

## Step4: Inference

预测的时候，

* 如果一个用户在source domain有user action，那么transformed target-domain user embedding作为target domain的initial user embedding可直接拿来使用
* 如果一个用户在source&target domains都有action，那么transformed target-domain user embedding作为初值，已经经过target domain的fine tune了，想必效果更好