---
alias: SDIM
发布时间: 2022-05-20
出品方: 美团
价值: ⭐⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/用户行为序列 
URL:: [zotero](zotero://open-pdf/0_SUVNN46U/1)

---

# Sampling Is All You Need on Modeling Long-Term User Behaviors for CTR Prediction

## 与ETA的区别与联系

- 延续的是阿里[[End-to-End User Behavior Retrieval in Click-Through Rate Prediction Model|ETA]]的思路
	- 在线根据target item动态提取用户长期序列中的关键子序列
	- 避免像SIM hard or soft那样，需要离线建立索引，带来建模目标与更新频繁上的gap
	- 用的方法也很类似，都是用simhash

- 与ETA的区别，在于将user historical item与target item都用simhash获得signature后，如何利用signature
	- ETA是在hash signature上用hamming distance来计算距离，取代attention，找到与target item最相似的historical item
	- SDIM是用hash signature直接当成key去查找，找到与target item具有相同hash signature的historical item

- 能够用hash signature当key直接查找的原理在于
	- simhash不是瞎做的，而是根据embedding的内容进行的，两个embedding得到相同的simhash，只能说明原来两个embedding也是相似的
	- 因此，用simhash signature当key去检索，simhash signature冲突的概率就取决于target item embedding与某个historical item embedding的相似程度

- 另外需要注意的是，ETA是两步走，而SDIM是一步到位
	- ETA用simhash只是在SIM第1步的GSU阶段，用一种更简单的方式来从long history中筛选过滤出与target item相关的historical item。但是如何将筛选出的多个long historical item聚合成一个向量，还是常规的multi-head target attention
	- DSIM是一步到位，hash signature collide的概率就相当于target attention，用hash sigature检索的过程相当于在整个long history sequence上做target attention


## 作法

### 第1步：生成hash signature
- 第1步，we sample from multiple hash functions to generate hash signatures of the target item and each item in user behavior sequence. 

- LSH & SimHash
	- LSH有locality-preserving属性，相似向量，有较大概率collide
	- simhash是LSH的一种实现。The random projection scheme (SimHash) is an efficient implementation of LSH. 
	- The basic idea of SimHash is to sample a random projection (defined by a normal unit vector r) as hash function to hash input vectors into two axes (+1 or -1).
	- ![[Pasted image 20220710104657.png |300]]
	- 一次性生成m个hash signature, ![[Pasted image 20220710104759.png |300]]

- ![[Pasted image 20220710095813.png |400]] ^5886dd
	- 所谓的(m,$\tau$)-simhash，
		- m就是用多少个hash function，一个embedding用了m个hash function，就得到m-bits
		- 而$\tau$就是在m-bits当中，以$\tau$个bit为单位，组成$\frac{m}{\tau}$个signature
		- 一个embedding，就会被分入对应的$\frac{m}{\tau}$个bucket中
		- 分配到一个bucket中的多个embedding要聚合成一个向量，图中`norm(s0+s1)`就代表相加，再L2-normalization

### 第2步：直接根据hash signature提取

- we directly gather behavior items that share the same hash signature with the target item to form the user interest
	- ![[Pasted image 20220710112652.png |300]]
	- *Instead of retrieving top-𝑘 similar items using a certain metric like ETA*
	- The intrinsic idea behind our method is to approximate the softmax distribution of user interest with LSH collision probability

- The outputs of 𝑚/𝜏 hash signatures are averaged for a low variance estimation of user interest.
	- ![[Pasted image 20220710101031.png |500]]


## 等价于target attention
- We show theoretically that this simple sampling-based method produces ==very similar attention patterns as softmax-based target attention== and achieves consistent model performance, while being much more time-efficient. 
	- our method behaviors like ==computing attention directly on the original long sequence==

- 拿某一个$\tau$-bit去检索的过程就相当于如下的target attention
	- ![[Pasted image 20220710105402.png |400]]
	- the width parameter 𝜏 plays a role in controlling the strength of the model on paying more attention to more similar items. **As 𝜏 increases, encourages the model to pay more attention to more similar items**.
	- 如果$\tau$非常非常长，实际上除非target item与某个historical item完全一致，否则很少可能冲撞上。所以$\tau$越大，attention时对“相似”的要求越高，相似性差一点就不attend了。
	- $\tau=0$，就相当于对所有historical item做mean pooling了

## 架构

- ![[Pasted image 20220710112857.png |500]]
	- 只有对short history才做target attention
	- 对于长期历史，从根据target item的hash signature提取出来的向量，就已经是attention过了（**sampled-based attention**），无须再像SIM ESU那样再做attention

- ![[Pasted image 20220710094219.png |500]]
	- 训练的时候，合在一起训练，因为user interest modeling part也没有什么参数要训练
	- 部署的时候，要behavior sequence encoding server与ctr server分开部署
	- 分开部署的好处，We put the BSE Server before the CTR Server and **compute it in parallel with the retrieval module**，相当于提取一个新特征
		- **item embedding对应的hash signature是可以缓存的**
		- 或者每次增量更新embedding时，就把最新的hash signature计算好
		- 有了缓存，避免重复计算item embedding的hash signature，BSE的工作只相当于将long historical seuqnce分桶
	- For each request, we need to *transmit bucket representations from the BSE server to the CTR server*. the size of this vector is 8KB and the transmission cost is about 1ms.

## 性能分析
- Batch Size一般都是1000级水平
- d一般是100级水平
- m=48
- $\tau=3$

- 无论一个request里面有多少candidate item，[[Sampling Is All You Need on Modeling Long-Term User Behaviors for CTR Prediction#^5886dd |将user historical sequence先simhash再分桶]]，只需要做一遍
	- The most time-consuming part of SDIM is the multi-round hashing of the behavior sequence, i.e., transform a 𝑑-dimensional matrix into 𝑚-dimensional hash codes. 
	- The time complexity of this operation is 𝑂 (𝐿𝑚𝑑) and 
	- can be **reduced to 𝑂 (𝐿𝑚 log(𝑑)) using the Approximating Random Projection algorithm**
	- 而且==还可以与召回并行做，在排序开始之前就已经完成了==
