---
alias:
发布时间: 2019-01-01
出品方: 阿里妈妈
价值: ⭐
出处:
---
环节:: "精排"
关键词:: #ML/GNN #Reco/用户行为序列

---
# Graph Intention Network for Click-through Rate Prediction in Sponsored Search 


阿里妈妈 SIGIR-2019 👎👎👎 以为是怎样将GNN与推荐结合起来的？没想到就是靠蛮力，没有阿里的Euler图引擎是玩不转的

---

处在Hidden Layers - User Behavior Modeling 优化路径上。阿里妈妈搜索广告在过去几年的探索中在**User Behavior Modeling**方向基本确立了序建模（Sequence Learning）和图建模（Graph Learning）相融合的模型框架。
* 序建模聚焦在==用户自身的历史行为（私域行为），对于行为较为丰富的用户而言==，能够充分挖掘其个性和差异化特点；
* 图建模利用群体行为互联的结构（公域行为），借助群体智慧充分挖掘行为背后的共性和可迁移性特点，对于==低活用户和搜索新需求非常友好==

**相较传统的时序建模对于低活用户不友好和兴趣泛化性欠佳**的问题，我们==对用户历史行为序列的每个对象借助Graph的空间拓扑结构进行往外兴趣拓展，利用多层图卷积的汇聚能力==，使得用户兴趣表征的泛化能力更强

---

基于user historical behavior的建模有两大缺点

* 对低活用户有，behavior sparsity的问题
* 另外，有可能让用户陷入信息茧房。Weak generalization refers to the user’s inability to **jump** out of their specific historical behavior for possible interest **exploration**.

---

Graph-Intention Network

* Firstly, the GIN method enriches user's behavior by multi-layered graph diffusion of user historical behaviors, and solves the behavior sparsity problem.

  * 没啥特殊的，**就是在图中向外游走，将sequence每个click item的几度邻居也加进来**
* Secondly, the weak generalization problem is alleviated by introducing co-occurrence relationship of commodities to explore the potential preferences of users. 

  * 作者也没特别做，好像是sequence向外walk的副产品。肯定有不那么相关的邻居，就算是generialization了
* Finally, we combine this intention mining method based on co-occurrence commodity graph with the CTR prediction task by end-to-end joint training

  * 算法没出啥力，背后有一个好的graph service

## 建图

还是根据item co-occurence建设图

![image.png | 400](assets/image-20211208185954-z1ef0pt.png)

* Each row represents a user’s click sequence. The black arrow indicates the behavior direction, and the red arrow indicates the graph edge when the window size is 1.
* In the co-occurrence commodity graph, nodes represent clicked commodities, and **edge weights indicate the numbers of co-occurrence clicks**.

## Sequence向外扩散

训练的时候（文章没说，不过感觉也包括infer的时候），user action list中的每个clicked item都向graph server发请求，在图中向外walk，~~将邻居也加入user action list中~~。其实也算不上扩充user action list，**user action list中item不变（i.e., 不影响attention），只是user action list中item embedding时要考虑item co-occur图中的邻居而已**。

问题是，~~这些扩散进来的item，就不加任何标识吗？让算法与用户实际clicked混淆？？？~~ ==这个问题并不存在，未来参与target item对user action list attention的，还是用户实际clicked item，而不包括扩散进来的item==

![image.png | 300](assets/image-20211208204151-t6wils1.png)![image.png | 400](assets/image-20211208204217-gjknkqt.png)

## 图卷积

也就是扩散后的user action list中的每个item都通过图卷积，聚合邻居embedding，获得自己的embedding （对比没有图卷积，user action list中的每个item embedding只不过是来自embedding table；而这时，user action list中的每个item embedding已经是聚合邻居embedding后的结果了）

![image.png | 300](assets/image-20211208204310-66o44if.png)![image.png | 300](assets/image-20211208204451-y04sgsg.png)

## Target Attention

**还是只有用户实际点击过的item参与了attention。**既然如此，那么与之前普通的target attention有啥区别？**区别在于每个original clicked item的embedding不再只来自于embedding table，而是聚合了i2i graph中邻居的信息。**

![image.png | 600](assets/image-20211208205130-5b4y6te.png)

## End-To-End训练

![image.png | 600](assets/image-20211208205456-d2rf9d0.png)

* Each historical clicked sample first performs a multi-layered neighbor query on the graph service,
* and the attention mechanism is used to perform neighbor aggregation according to correlations between the current node and the neighbor nodes.
* Finally, the aggregated intention results and other features are concatenated as inputs for CTR prediction
* 需要DNN learner向graph learner传递梯度。