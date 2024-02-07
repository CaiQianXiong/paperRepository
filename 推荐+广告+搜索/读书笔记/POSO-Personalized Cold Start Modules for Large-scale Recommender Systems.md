---
alias:
发布时间: 2021-01-01
出品方: 快手
价值: ⭐
---

环节:: 
关键词:: #Reco/新用户冷启 

---
# [POSO: Personalized Cold Start Modules for Large-scale Recommender Systems](https://arxiv.org/pdf/2108.04690.pdf)

* 提出的问题很有价值，就是即使加上了人群属性区分性很强的特征，最终也还是会被优势人群的特征所淹没。
* 但是实现手段，就有点故弄玄虚了。其实有效果的就过就是LHUC+新用户返回加权这些东西。

提出问题，user id embedding学不好，只是一个方面。增加了一些个性化数据（e.g., 使用的app），也是一方面。但是新加的这些对新用户有用的特征，可能学不好，加了也是白加。

> However, we argue that there exists another neglected problem: the ==submergence== of personalization. The problem describes a phenomenon that even though personalized features（**自认为对新用户有效的特征**） are used to balance various user groups (whose distributions are very different), these features are ==overwhelmed== because of heavily imbalanced samples
>


## 其他冷启动的idea #价值/💡 

* 难点在于user id embedding学不好。meta-learning提供一个好的初始值？想法是好，就不知道效果如何。其实可以仿照airbnb的作法，人工规则将user分组，线上拿不到user id embedding就拿与他同一个组的group embedding代替之

  * 注意线上线下的一致性，如果线上要用group embedding代替之，那么是否线下训练的时候，也要用group embedding参与训练？一定要保证group embedding与其他embedding是来自同一个向量空间
* 学习出一个group embedding，$Emb_{uid}=\alpha*E_{UserGroup}+(1-\alpha)*E_{PersonalResidual}$  

  * $\alpha$随着browse size逐渐的变换?
* 加强new user sample的权重，权重与browse set size呈反比

