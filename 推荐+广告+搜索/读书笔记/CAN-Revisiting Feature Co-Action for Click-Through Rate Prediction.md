---
alias: CAN
发布时间: 2021-12-07
出品方: 阿里妈妈
价值: ⭐⭐⭐⭐ 
---

环节:: "精排"
关键词::  #Reco/特征交互 

---

# CAN: Revisiting Feature Co-Action for Click-Through Rate Prediction
[offical implementation](https://github.com/CAN-Paper/Co-Action-Network)
[想为特征交互走一条新的路](https://zhuanlan.zhihu.com/p/287898562)

## A. 存在的问题
- the non-linear feature interaction is learned in an implicit manner. The non-linear interaction may be hard to capture 
- current CTR models do not fully explore the potential of feature co-action 
  - 模型也能够交叉，但是未必有能力学好交叉

对于交叉特征，最有效的方法就是每对儿特征交叉，都是一个新的单独特征，来学出这个“交叉特征”的embedding。The most direct way to learn feature interaction is to treat feature combinations as new features and learn an embedding vector for each feature combination directly.
但是这样做有两个问题：
- 引入的参数太多，参数空间膨胀得太厉害
- as cartesian product regards <A, B> and <A, C> as totally different features, there is no information sharing between combinations, which also limits the representation ability.
	- Since different feature pairs may share the same feature, there exists an implicit similarity between any two feature pairs, which is ignored by cartesian product. If the implicit similarity can be effectively dealt with, the feature interactions among these pairs could be modeled more effectively and efficiently with a smaller parameter scale than cartesian product.

## B. Related Work
- Aggregation 
  - 就是DIN/DIEN那一套，utilize the feature co-action to model the weight of each user action in historical behaviour sequence. The weighted user behavior sequence is then sum-pooled
- Graph-based  
  - the feature co-action serves as an edge weight for information propagation along edges
  - The weights are used to aggregating neighborhood information to model feature co-action. 
- Combine Embedding 各种dnn隐式或显式的交叉
  - **缺点：Embedding的两角色相互冲突**
    - embedding methods is that the embeddings take the responsibility of both representation learning and co-action modeling. 
    - The representation learning and co-action modeling may conflict with each other thus bound the performance.

## C. Co-Action Network
![[Pasted image 20220410113429.png | 700]]

### 1. 引入交叉特征
![](assets/220226110437_clip_image002.jpg)
一定要注意，**交叉是发生在feature级别上，而不是只在field级别上**

#### DIN也能交叉
缺点是： 
1. 仅限于target item与user history 
2. 还是一个embedding两用而导致冲突的问题

#### 每对特征交叉都学习一个Embedding
- 缺点1：参数膨胀 
- 缺点2：彻底独立又带不来参数共享 
  - there are no information sharing between two combinations that contains same feature, which also limits the representation ability of cartesian product.
  - 面临着与纯LR一样的问题，对于罕见特征交叉也学不好，预测时也没见过

#### CAN的方法
就是引入co-action unit，选择一些$P_{item}$当mlp，选择一些$P_{user}$ 当input。
![](assets/220226110437_clip_image004.jpg)

- 为了减少线上耗时，不是所有user特征与所有item特征都参与交叉，只是选择一些user/item特征才交叉
- [[CAN-Revisiting Feature Co-Action for Click-Through Rate Prediction#^h8zkue|同时为了减少耗时，即使是选中的user/item之间，也并非任意两个之间都要交叉]]
- co-action unit使用单独的embedding，与dnn其他部分使用的embedding分离 

### 2. Co-Action Unit
introduces a pluggable module, co-action unit. 
- The unit **differentiates the parameters for embedding and feature interaction learning**. 
- Specifically, it is made up of two sides of information from raw features, i.e., **induction** and **feed** side. 
	- The **induction side is used to construct a micro-MLP** 
	- while the **feed side provides the input** for it.
	- Empirically, the target item features like **“item_id” and “category_id” are selected as the induction side**
	- while the **user features are for the feed side**
- As **different feature pairs can share the same micro-network**, the similarity information is learned and stored naturally in this micro-network as illustrated in Fi

![ | 200](assets/220226110437_clip_image006.jpg)

#### a) Embedding的构造 
![](assets/220226110437_clip_image008.jpg)
- 注意图中的M是unique ID的个数，再次提醒交叉是发生在feature级别，不是发生在field级别

- 这里是拿item当MLP, 而user当input 
	- 实际场景下，一般是拿target item-id与user history item-id做交叉
	- ==拿target item embedding当MLP，是因为出现次数少，复用机会多，如果能够隔离体现的效果也更好==

#### b) Item embedding reshape
![](assets/220226110437_clip_image010.jpg)
实验中，item-embedding, eight-layer MLP is used with weight dimension set to 4×4, which results in a dimension of (4∗4+4) ×8 = 160 (bias included). 

#### c) Co-action过程
![](assets/220226110437_clip_image012.jpg)
- For sequence features like user click history, the co-action unit is **applied to each item** followed by a sum-pool over the sequence.
- 理论上使用relu当activation更符合同一个item embedding面对不同user feature embedding呈现不同状态的直觉

在新修订的论文里，是将各隐层的输出拼接起来当成交叉embedding
![[Pasted image 20220410113303.png | 300]]

#### d) 作用是什么？

- ==理论上能够达到，item feature embedding使用自己的不同部分，与不同user feature embedding交叉==  ^d1dc76
	- different from previous works that using the same latent vectors in different types of interfield interactions
	- couple two component features by dynamical parameter and input but not a fixed model  
- 大大缩小了参数空间
	- 不需要指数级的参数空间
	- comparing to the cartesian product that shares no information between different feature combination with same feature, the coaction unit improves the utilization of parameters since the MLPs are directly derived from the feature embeddings
- 类似FM的扩展性 
	- the co-action unit has better generalization to new feature combinations comparing with other previous works. Given a new feature combination, the co-action unit still works as long as embeddings of two sides are trained before.

### 3. Multi-Order Enhancement 
we explicitly introduce multi-order information in the co-action unit to obtain a polynomial input. This is achieved by applying $MLP_{can}$ to different orders of $P_{user}$.
![](assets/220226110437_clip_image014.jpg)
From 1st order to 2nd order, the AUC promotes lot. Afterwards, as the order growing, the gap starts to shrink even cause negative effect. so that 2 or 3 power terms is proper in practical applications.

### 4. Multi-Level Independence 
- multi-level independence including 
	- 必须的：参数空间的独立性 
		- our approach differentiate the parameters for representation learning and co-action modeling. 也就是同一个feature，有不同的embedding
		- where 𝐷 is the dimension of embeddings. However, by using co-action unit, this scale will decrease to <span style="color:orange;font-weight:bold">𝑂 (𝑁 × (𝐷 ′ + 𝐷)), where 𝐷 ′ is the dimension of the 𝑃𝑖𝑛𝑑𝑢𝑐𝑡𝑖𝑜𝑛 in co-action unit</span>. 被选中作为induction的那部分参数需要有独立的embedding。
		- The parameter independence is the basis of our CAN.
	- 推荐的：组合上的独立性
		- [[CAN-Revisiting Feature Co-Action for Click-Through Rate Prediction#^d1dc76| 同一个embedding与不同其他embedding交叉组合时，使用其中不同段的数据]]
		- ==类似于同一个id在不同field下有不同的embedding==，像FFM
	- 可选的：不同阶的输入，使用不同weight embedding 
- the purpose is to guarantees the learning independence of co-action by broadening the parameter space.
- 但是注意代价是：Emperically, the higher independence level the model use, the more training data the model need.

## D. 部署 

### 1. 部署上的困难 
- The significantly increasing embeddings space still lead to heavy pressure of online serving. 
- the computational costs of feature co-action grow linearly according to the number of feature combination which also bring considerable response latency 

### 2. 解决方案 
- Sequence cut-off: all user behaviour sequences of length 200 are reduce to 50. The most recent behaviours are kept
- ==削减交叉范围== ^h8zkue
	- Empirically, the combinations with ad feature and user feature of the same type can better model the feature co-occurrence. 
	- we keep the combinations like 💡
		- *“item_id” and “item_click_history”* and 
		- *“category_id” and “category_click_history” *
	- and remove some irrelevant combinations
- 底层kernel优化 
	- The dim_in and dim_out are not commonly used shape so that such matrix multiplication are not well optimized by the BLAS
	- we further make a kernel fusion between matrix multiplication and sum-pooling

## E. 总结
- efficiently and effectively capture the feature co-action, which improves the model performance while reduce the storage and computation consumption
- 引入交叉特征，但是又不像cartesian product那么大的参数空间
- 参数独立性，CAN differentiate the embedding space for representation learning and co-action modeling, enriches its expressive power and alleviates the conflict

三大优势：
- First, **different from previous works that use the same latent vectors in different types of inter-field interactions**, the co-action unit utilizes the computational ability of micro-MLP and **couples two component features 𝑃𝑖𝑛𝑑𝑢𝑐𝑡𝑖𝑜𝑛 and 𝑃𝑓 𝑒𝑒𝑑 dynamically** rather than a fixed model, which provides more capacity to guarantee the disentangled update of two field features. 
- Second, **a smaller scale of the parameters needs to learn**. For instance, considering two features with both 𝑁 IDs, the parameter scale of their cartesian product should be $𝑂(𝑁^2\times𝐷)$, where 𝐷 is the dimension of embeddings. However, by using co-action unit, this scale will decrease to **𝑂 (𝑁 × (𝐷 ′ + 𝐷)), where 𝐷 ′ is the dimension of the 𝑃𝑖𝑛𝑑𝑢𝑐𝑡𝑖𝑜𝑛** in co-action unit. Fewer parameters are not only conducive to learning but also can effectively reduce the burden of the online system. 
- Third, **the co-action unit has a better generalization** to new feature combinations compared with cartesian product. Given a new feature combination, the co-action unit still works as long as their embeddings of two sides are trained before.