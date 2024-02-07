---
alias: TDM 2.0
发布时间: 2019-11-20
出品方: 阿里妈妈
价值: ⭐⭐⭐
---

环节:: 召回
关键词:: 

---

# [Joint Optimization of Tree-based Index and Deep Model for Recommender Systems](https://arxiv.org/abs/1902.07565)
[如何优化大规模推荐？下一代算法技术JTM来了](https://zhuanlan.zhihu.com/p/85883448)

是"TDM 1.0"[^1]的升级版。说实在的，没看懂。

## 训练模型与建树统一建模

论文主要解决的就是用node embedding来建树的缺陷。作者认为用node embedding聚类方式生成树是启发式的，没能直接优化目标，所以在本版中将建树的过程也写成一个优化问题。

1. We propose a joint optimization framework to learn the tree index and user preference prediction model in tree-based recommendation, where a uniﬁed performance measure, i.e., the accuracy of user preference prediction is optimized; 

    ![image.png | 600](assets/image-20211231162113-hh3ikos.png)

    * 图中的$\theta$是模型的参数，$\pi$ projection function that projects an item to a leaf node in T .
    * 最不明白的就是❓，"Note that π(·) completely determines the tree hierarchy T ,  and optimizing T is actually optimizing π(·)."。为啥？为了定义了叶子节点的对应关系，就相当于定义了树？非叶子节点就不用定义了吗？
    * 这是与TDM 1.0最大的区别，建树不再是启发式的，而是也被建进模型
2. 那怎么解建树的问题？We demonstrate that the proposed tree learning algorithm is equivalent to the **weighted maximum matching problem of bipartite graph, and give an approximate algorithm to learn the tree**; 反正论文里写了，没看得下去。

## 层次化的用户表示

说白了，就是用中间层的节点训练的时候，**用户的action list不能再由原始item组成，而是由这些原始item（叶子节点）在那一层的祖先节点组成**。“the ancestor nodes of items the user interacts are used as the hierarchical user preference representation.”。说是有两方面的好处：

* 这下，不同层的user action history是完全不同的embedding了。但是不同leaf node可能在某层有相同的ancestor node embedding，这个怎么处理？去重吗？没说
* 和预测时保持一致。既然预测的时候，是一个coarse到fine的过程。训练的时候，中间层的embedding也要粗粒度。