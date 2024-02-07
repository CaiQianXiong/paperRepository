---
alias: 
发布时间: 2021-01-01
出品方: 阿里达摩院
价值: ⭐
环节: 召回
---
关键词:: #Reco/样本策略 

---
# Contrastive Learning for Debiased Candidate Generation in Large-Scale Recommender Systems 

## 概述

* **这篇文章完全是挂羊头卖狗肉，标题上写着Contrastive Learning，实际上完全不沾边。完全没有涉及对比学习中的一些常见话题，比如Data Augmentation，辅助训练之类的。**
* **其实通篇文章讲的就是召回中如何采集负样本的一些问题。**
* 面向的问题就是exposure bias导致的传统召回, continue to under-estimate the relevance of the under-explored items in order to faithfully fit the observed data.
* 理论上证明，establishing the theoretical connection between contrastive learning and inverse propensity weighting (IPW)
* 使用了**基于queue的方法来negative sampling，之前batch的positive item embedding，会成为后续batch的negative item embedding的候选集**。这个其实没啥新意，就是普通的"batch外负采样"的常见作法。
* 将上述使用single queue的方法，扩展成multi-queue, for more accurate user-intention aware bias reduction.