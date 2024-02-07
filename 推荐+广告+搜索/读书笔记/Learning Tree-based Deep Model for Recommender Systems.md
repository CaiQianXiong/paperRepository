---
alias: TDM 1.0
发布时间: 2018-12-21
出品方: 阿里妈妈
价值: ⭐⭐⭐
---

环节:: 召回
关键词:: 
URL:: [zotero](zotero://open-pdf/0_CTFARB6B/1)

---

# [Learning Tree-based Deep Model for Recommender Systems](https://arxiv.org/abs/1801.02294) 
[官方demo实现](https://github.com/alibaba/x-deeplearning/wiki/%E6%B7%B1%E5%BA%A6%E6%A0%91%E5%8C%B9%E9%85%8D%E6%A8%A1%E5%9E%8B(TDM))

``` ad-warning
title: 实战起来，问题多多
参考[[三问TDM]]
```

We propose a novel tree-based method which can provide logarithmic complexity w.r.t. corpus size even with more expressive models such as DNN.

* The main idea is to predict user interests from coarse to ﬁne by traversing tree nodes in a top-down fashion and making decisions for each user-node pair. 实际上tree的作用就是，充当全库item的索引，缩小检索范围，没必要全库逐一检验。如果用户已经对某个coarse候选节点不感兴趣了，就没有必要沿这个coarse节点向下继续搜索了。
* the tree structure can be jointly learnt towards better compatibility with users’ interest distribution and hence facilitate both training and prediction

所要解决的问题就是，Conventionally, advanced expressive models can not be regulated to inner product form between user and item vectors to utilize eﬃcient approximate kNN search, they can not be used to recall candidates in large-scale recommender systems.

**树结构**

* each item in the corpus C corresponds to one and only one leaf node in the tree, and
* those non-leaf nodes are coarse-grained concepts.
* max-heap like tree, i.e., every non-leaf node `n` in level`j`satisﬁes the following equation for **each user u (???难道树的结构是因user而异吗？)**. It says that a parent node’s **ground truth** preference equals to the maximum preference of its children nodes, divided by the normalization term.

![image.png](assets/image-20211227201247-2fhicic.png)

---

## Training

### 学习模型

#### 正负样本

* Leaf node that have positive interaction with u, and its ancestor nodes constitute the set of **positive** samples in each level for u.
* Randomly selected nodes except positive ones in each level constitute the set of **negative** samples.

![image.png](assets/image-20211227201904-641mtdo.png)

These samples are then fed into binary probability models (所以这里所说的负采样与approximate softmax中的negative sampling不同，单纯采样而已，不涉及对softmax的修正) to get levels’ order discriminators. We use one **global DNN** binary model for all levels’ order discriminators.

We **randomly select negative samples in the same level for each positive node**.

* Such method makes each level’s discriminator be an intra-level global one. Each level’s global discriminator can make precise decisions **independently, without depending on the goodness of upper levels’ decisions**.
* It ensures that even if the model makes bad decision and low quality nodes leak into the candidate set in an upper-level, those relatively better nodes rather than very bad ones can be chosen by the model in the following levels.


#### 对于某棵树时训练模型

对于训练过程中的每一棵树（训练过程中的中间状态），都要训练如下的模型

![image.png | 800](assets/image-20211227202653-omltm4l.png)

* 每个item只能用item id来当特征，学习其item id embedding，**而不能用side information**。因为非叶节点是抽象的，不对应任何具体item，非叶节点上的side information没法填。
  * **其实也不是完全不行。可以拿叶子节点的side information，逐层向上堆积。比如非叶节点的tag是叶子节点的tag的集合**。💡
* 交叉特征也不能用（？不确定），线上抽取交叉特征太费劲了吧
* 重要特征是用户的user action list，而且还按时间划分成了**若干time window**
  * 每个window中的action item embedding与node embedding是复用关系吗？可能是，可能不是，看实验吧。
* 每个time window中的user action items与target item做attention，从而将**每个time window加权平均成一个embedding**
* Each time window’s output along with the candidate node’s embedding are concatenated as the neural network input

训练量是极大的，所以阿里也只能用一天的数据来训练
``` ad-question
title: 岂不是只能item id当特征？

因为如果用了其他非id类的特征，那么非叶节点怎么办？非叶节点不是实体，它们不可能有除了id之外的其他特征

有一种说法是，可以通过叶子节点的特征向上聚合，比如非叶子节点的标签是其所有子节点的标签的集合，这样做是有点粗，也麻烦
```

``` ad-question
title: 中间非叶节点的embedding从哪里来？

最关键的问题是中间非叶节点是dynamic,virtual的，不具备连续训练的继承性

叶子节点都是item id，把embedding存储下来，下次可以load进来，继续训练

但是中间节点呢？总不能因为因为前后两次训练，都编号成“第2层第3个节点”就继承上次的embedding吧，位置相同，并不代表背后的实体意义是相同的
```

``` ad-question
title: 对于不同层的正负节点，权重都是一样的？？？一视同仁？

显然不对呀，如果上边分错了，检索时下边就根本没机会检索到
显然越向上，分错的代价越大，应该给予更大的权重

而且如何避免父亲分错了，而儿子分对了，这种现实荒唐，而训练时有可能发生的现象？？？
```

### 学习树结构

#### 初始化

it’s natural to build the tree in a way that similar items are organized in close positions. Alibaba leverages item’s **category information to build the initial tree**.

* Firstly, we sort all categories randomly, and
  * place items belonging to the same category together in an intra-category random order.
  * If an item belongs to more than one category, the item is assigned to a random one for uniqueness.
  * 第一层就是一个根节点，然后第二层有M个节点，M是总的category数目
* Secondly, those ranked items are halved to two equal parts recursively until the current set contains only one item


在实际场景下，**各item embedding无需从0随机初始化，有各种其他算法（word2vec, 双塔）能够提供已经非常靠谱的item embedding了**，用这些item embedding建成的树，实际上就已经是最终的树了，**无需多次建树**。


#### 学习新树

Learning each leaf node’s embedding can be learnt after model training. Then we use the learnt leaf nodes’ embedding vectors to cluster a new tree.

* After a DNN is learnt, items are clustered into two subsets according to their embedding vectors, via K-means.
* the two subsets are **adjusted** to equal for a more balanced tree.
* The recursion stops when only one item is left, and a binary tree could be constructed in such a top-down way.


### 交替学习

![image.png | 300](assets/image-20211227205804-8cl8jk8.png)

The deep model and tree structure are learnt ~~jointly in an alternative way~~（实际场景下并不需要）:

1. Construct an initial tree and train the model till converging;
2. Learn to get a new tree structure in basis of trained leaf nodes’ embeddings;
3. 根据新树结构，重新定义正负样本
4. Train the model again with the learnt new tree structure.

* 而实际工作中，**无需在学习树结构与学习模型之间多次交替迭代**
* 从其他召回算法中已经能够获得非常好的item embedding了，用这些item embedding就能够建树了，而且建成的树就已经靠谱了，**就可以在一段时间内固定下来了（e.g., 一天）而无需反复重建**
* 一天之内，树可以是基本固定的。之所以是基本，是因为在这一天之内新出现的item，可以通过其他算法提供的item embedding，被插入在当前树中。插入后，需要重新采样正负样本，重新训练模型。

## Serving

For level `l`, only the children of nodes with top-k probabilities in level `l − 1` are scored and sorted to pick k candidate nodes in level l.

![image.png | 500](assets/image-20211227201524-rqmeys6.png)