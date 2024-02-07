---
alias: ETA
发布时间: 2021-08-10
出品方: 阿里妈妈
价值: ⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/用户行为序列 

---

# End-to-End User Behavior Retrieval in Click-Through Rate Prediction Model

## 思路
- 之前方法的缺点
	- pooling的方法就是没有“千物千面”，而且pooling将所有history都包含在内，有噪声
	- DIN是“千物千面”，但是不能处理long history，因为attention的时间复杂度是O(L\*B\*d)
	- MIMN将建模user interest与主模型分离，MIMN can handle long-term user behavior sequence by decouplng the user interest modeling with the rest of CTR task. **The user interest vector is updated offline in an asynchronous way whenever a new behavior is observed**. 
	- SIM方法的缺点在于，两个阶段之间有<span style="color:orange;font-weight:bold">information gap</span>💡💡💡
		- 目标不一致
			- the target of retrieval part should keep the same with the whole CTR model. Only in this way can the top-k retrieved items contribute the most to the CTR model
			- 用category来进行hard search显然属于“目标不一致”的情况。use category to select items from user behavior sequence which share the same attribute with target candidate item, which is <span style="color:orange;font-weight:bold">divergent with the target of CTR model</span>
		- 更新不一致
			- <span style="color:crimson;font-weight:bold">SIM also tried to building an offline inverted index based on the pre-trained embedding</span>.  ^g3gjgh
			- <span style="color:red;font-weight:bold"></span>During <span style="color:crimson;font-weight:bold">training and inference</span>, the model can search the top-K “similar” items. <span style="color:crimson;font-weight:bold">train/infer时都要访问这个由offline pre-trained embedding建立的inverted index</span>。 ^fiyiqe
			- But most of the CTR model is in online learning paradigm and the embedding is updated continuously. 
			- <span style="color:crimson;font-weight:bold">Thus the pre-trained embedding in offline inverted index is outdated compared with the embedding in online CTR model</span>
		- 总而言之一句话，没有使用ctr model之中的embedding，导致retrieval与ctr model脱节。"<span style="color:crimson;font-weight:bold">However, the inputs they used for building the index are attribute information (e.g., category) or pre-trained embedding of items, which are different with the embedding used in CTR model</span>"

``` ad-note
title: ETA思路的演变

- 所以，问题的关键在于，用item embedding来搜索user long history
- 逐一dot计算相似性，时间复杂度=O(L\*B\*d)，L=history length, B=batch_size, d=embedding dimension
- SIM的作法是，将item embedding离线建立索引，时间复杂度是O(log(L)\*B\*d)，但是计算相似性的方法还是dot。
	- 付出的代价就是index是离线建立的，不可能与ctr model所使用的item embedding保持一致
- ETA提出的方法是，更换检验embedding相似性的方法，将时间复杂度变成O(L\*B\*1)💡💡💡
	- 优点是无需再离线建立索引，target item在long history中搜索相似item，与main ctr model共享一套embedding
	- 💡💡💡The reduction of complexity helps us removing the offline auxiliary model and conducting **real-time retrieval** during training and serving procedure
```

## SimHash
- SimHash function takes the embedding vector of an item as input and generates its binary fingerprint
- Each d-dimensional vector is converted to a m-length integer binary vector (i.e., 0 or 1).  
	- In real ctr model, <span style="color:blue;font-weight:bold">d=128, m=4</span>.
- ![[Pasted image 20220406204346.png | 500]]
	- not sensitive to the selection each rotation “hashing function”. Any fixed random hash vectors are enough
	- 结果是一个binary vector,  which can further be decoded using an integer to save storage cost and to speed up the following hamming distance calculation.
- ![[Pasted image 20220406204534.png | 500]]
	- we can observe that nearby embedding vectors can get the same hashing signature with high probability
- In other words, ==the inner product between two vectors can be replaced by hamming distance==.
	- The hamming distance for two integers is defined as the number of different bits positions at which the corresponding bits are different. 
	- To get the hamming distance of two m-bit numbers, we first conduct XOR and then count the number of bits in 1
	- E.g., 11011001 ⊕ 10011101 = 01000100. Since, this contains two 1s, the Hamming distance, d(11011001, 10011101) = 2
	- <span style="color:orange;font-weight:bold">If we define the multiplication as the atomic operation, then the complexity of hamming distance for two m-digit numbers is O(1)</span>. 
		- we can use ~~log(m)-bits integer~~(❓❓❓why not m bits 1 or 0) to represent the signature vector because each element is either 1 or 0. 
		- This can greatly reduce the cost of memory and can speed up the calculation of hamming distance. 
		- The calculation time of two integers can be conducted in O(1) time complexity and can be neglected.

## 基于SimHash的Top-K retrieval
- Top-K retrieval由inner product时的O(L\*B\*d)简化成O(L\*B\*1)
	- ~~每个向量的m-bit hash fingerprint可提前计算好保存进embedding table中，无需重复计算~~训练的时候，embedding变换频繁，没有缓存的必要
	- 我觉得这么比较是没意义的，大家都知道O(L\*B\*d)是没有上线可能的。所以之前SIM即使用了dot product来检索，也[[Search-based User Interest Modeling with Lifelong Sequential Behavior Data for Click-Through Rate Prediction#^qrlc73 | 肯定不会是linear time complexity,必然是sub-linear complexity]] ^j7y0or
	- 正确的比较应该是，<span style="color:orange;font-weight:bold">由SIM的O(log(L)*B*d)-->变成了ETA的O(L*B*1)</span>

- <span style="color:orange;font-weight:bold">Ensure that the top-k nearest keys to query in each iteration are selected using the latest embedding of CTR model seamlessly</span>
	- <span style="color:orange;font-weight:bold">即使是在训练每个batch时</span>，main ctr model更新了item embedding，永远用最新的item embedding来retrieval top-k similar history items w.r.t target item

- 注意，ETA用hamming distance只是改善了GSU部分，是用一种更简单的方法来从long history中筛选过滤出与target item相似的historical item
	- 筛选后，还是经过ESU，拿target item与retrieved top-k long historical item做target attention

- It is noteworthy that the hashed **fingerprints can be stored in an embedding table with the model each time the SimHash function is conducted**. 
	- During the inference time, only embedding lookup is needed and its complexity is negligible.
	- 也就是每次向infer server打一套embedding，就把每个embedding的fingerprint打过去，没必要反复计算


## 整体结构
- ![[Pasted image 20220406212723.png | 500]]

- 工业级场景下的数据规模
	- The recent 48 behaviors are selected as short-term user behavior sequence and 
	- the recent 1024 behaviors are selected as long-term user behavior sequence
	- 这个规模与SIM宣称的万级别的history length，又少了一个数量级
	- [[End-to-End User Behavior Retrieval in Click-Through Rate Prediction Model#^j7y0or |正如我之前所说的]]，正确的笔法是SIM的O(log(L)\*B\*d)-->变成了ETA的O(L\*B\*1)。哪个效率更高，还真不一定。