---
alias: MVKE
发布时间: 2022-05-31
出品方: 腾讯
价值: ⭐⭐⭐⭐⭐
环节: 粗排
---
关键词:: #Reco/多样性 , #Reco/多任务学习 , #Reco/双塔模型 
URL::

---

# MVKE

- 技术关键点
	- 属于用户多兴趣建模
		- 即不再只是一个user embedding
		- 提出virtual kernel的概念，可以理解成一个粗粒度的Topic. **Topic-related and learnable virtual kernel**.
		- 用户针对每个virtual kernel, 即粗粒度的Topic，都有一个向量
	- 最终用户的向量，是由用户对多个Topic的向量组合而成
		- 组合的权重，是由target item与Topic做attention得到
		- 组合权重是“千物千面”的
		- 因此组合而成的user embedding，也是针对不同target item“千物千面”的
	- 但是性能怎么办？
		- 因为Topic向量不是user-specific的，而是global的，可以提前计算好
		- 那么每个item embedding与Topic向量的attention weight，也可以提前计算好
		- item embedding还是只有一套，只不过每个item要额外存储一堆attention weight
		- 粗排时，只不过要多算几套user embedding，多做几遍点积
		- 召回时，只要将attention weight也一并乘进item embedding，多建立几套Faiss，也不是不能做


## 针对单任务

- Item Tower没啥变化，常规作法

### VKE
- VKE
	- 在User Tower中，即使是单任务，也用上了Multiple Experts
	- 每个expert不是为了某个目标，而为了提取用户对某Topic的兴趣、看法
	- 而且这里的expert也不再是用DNN，而是使用了attention结构
	- Attention中的Query就是所谓的Virtual Kernel。既然每个expert是为了提取用户对某Topc信息，那么Virtual Kernel就是那个Topic。
	- 这个Attention中的Key/Value都是用户各Field embedding
	- 第k个expert，拿他代表的Topic，也就是第k个virtual kernel当尺子，去衡量用户所有field的重要性，再加权加和，就是第k个expert提取出来的用户向量
	- ![[Pasted image 20220818112726.png |400]]

- 这里的virual kernel，代表某个topic的embedding
	- 可以增加一个辅助loss，让各topic embedding更加disentangle一些
	- 比如[[Curriculum Disentangled Recommendation with Noisy Multi-feedback#^0e7bre |这种辅助loss]]

### VKG
- VKG
	- 用户塔中一共有K个expert，也就有K个virtual kernel，代表着K大类Topic。注意这是与某个用户无关的。
	- 将这K个expert套用在某个具体用户上，就得到该用户K个向量，每个都表示该用户对某Topic的兴趣、看法。
	- 用户对某item只需要一个打分，也就是要将用户的K个向量压缩成一个向量。如何压缩？是用线性组合的方式，权重哪里来？
	- 权重来自item embedding与topic embedding的attention weight
		- 在这个Attention中，Query=item embedding
		- Key=topic embedding，也就是virtual kernel
		- Value=topic-related user embedding
	- 也就是说，**如果某个candidate item越与某个topic越相关，针对这个candidate item的user embedding中，用户对该topic的embedding的分量也就越重。做到用户表达的“千物千面”**。
	- ![[Pasted image 20220818113808.png |400]]

### Bridge
- virtual kernel是关键，是桥梁
	- virtual kernel代表一类topic
	- 在user tower中，每个topic都可以衡量user fields的轻重，从而对每个topic有针对性地提取出topic-specific user embedding
	- 在item tower中，衡量candidate item与某个topic的匹配度，从而决定哪个topic-specific user embedding应该针对这个candidate item发挥更大作用。


## 针对多任务
- ![[Pasted image 20220818114456.png |500]]
	- 如果这时有T类任务（比如：CTR、CVR），item tower一侧就有T个expert
	- 而user侧需要更多的expert
		- 每类任务，至少有一个expert
		- 还要有若干shared expert，起的是PLE的路线

## 在线预测

- 关键在于, topic embedding也就是virtual kernel是global的，也某个具体user无关
	- 获得user embedding是，`sum(wk * topic-related user embedding)`, `wk`是item embedding与第k个topic之间的attention weight
	- 在计算得到每个item embedding之后，==这个item embedding与所有topic embedding的attention weight也就固定了，可以提前计算好==

### 粗排
- user feature经过user tower，得到k个topic-specific sub user embedding
- 对每个candidate item，不权要提取唯一的item embedding，还要再提取出这个item与K个topic的K个attention weight
- 这K个attention weight与K个topic-specific sub user embedding相乘，再相加，得到item-specific user embedding
- 这个专门针对当前candidate item的item-specific user embedding与item embedding点积，得到粗排打分

### 召回

- 既然点积是$score=\alpha_1 u_1 t+\alpha_2 u_2 t$的形式，可以写成两向量点积的形式
	- 用户向量=$conat(u1,u2)$
	- 物料向量=$conat(\alpha_1t,\alpha_2t)$，可以喂进FAISS建立索引
	- 除了可能向量长了一点，建立索引时有点费劲，其他没毛病

- ~~困难一点，因为上述过程，无法写成点积过程~~
	- 假如有t代表item embedding，还提前计算好了它对两个topic的attention weight $\alpha_1$和$\alpha2$
	- user embedding $E_u=\alpha_1u_1+\alpha_2u_2$
	- 那么点积就等于$score=\alpha_1 u_1 t+\alpha_2 u_2 t$
	- 但是我们希望user embedding也只生成一遍（与item无关），item embedding也只生成一遍（与user无关）

- ~~可以从Faiss多出一些，再筛选，因为毕竟要取$u_k\cdot t$总是没错的~~
	- 在线的时候，每个user生成K个topic-specific user embedding
	- 每个topic-specific user embedding去Faiss里面查找m个top item embedding
		- 再把点积结果乘上$\alpha_k$ 
	- 最后再把这些提取出来的结果的$\alpha_k \times 点积$相加，找出总结果最大的Top-N

- ~~当然如果不嫌弃麻烦，也有空间，可以针对各个topic专门建立一套faiss~~
	- 第k个faiss，存储的是$\alpha_k \times E_t$，并建立索引





