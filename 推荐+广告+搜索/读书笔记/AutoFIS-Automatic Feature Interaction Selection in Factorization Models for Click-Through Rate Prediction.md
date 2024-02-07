---
alias: AutoFIS
发布时间: 2020-01-01
出品方: 华为
价值: ⭐
环节: 
---
关键词:: #Reco/特征交互 

---
# [AutoFIS: Automatic Feature Interaction Selection in Factorization Models for Click-Through Rate Prediction](https://arxiv.org/abs/2003.11235)

价值不大的一篇文章 👎👎

思路和COLD中学习feature权重是一样的，也和我们用hammer算法来挑选重要特征和连接是一样的。只不过这里换成了对特征交叉做筛选。

文章做筛选的主要目的，倒不是为了提升线上预估效率，而是为了减少噪声，提升效果。

也是两段式的训练，和cold一样。search阶段先筛选出重要的feature interaction，然后再只用这些重要的feature interaction再re-train。

筛选时，![image.png | 300](assets/image-20211229190844-mh4y5mr.png)

* 加上batch normalization来稳定训练
* 用GRDA Optimizer得到稀疏解。

在re-train时，把不重要的feature interaction删除，但是剩下的interaction之前scaling factor还保留，说是起到activation unit的作用。但是我很不看好这种作法，因为它们都仅仅是learnable weight，而不似乎lhuc那样和输入是相关。