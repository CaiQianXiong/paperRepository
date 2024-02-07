---
alias: 
发布时间: 2021-01-01
出品方: Google
价值: ⭐
环节: 
---
关键词:: #Reco/Embedding 

---
# Learning to Embed Categorical Features without Embedding Tables for Recommendation
==概念玩得挺好，文章中自己都承认，有dnn取代embedding table不容易训练好==

## 概述
- 传统Embedding存在的问题
	- Huge vocabulary size
	- 字典是动态变化的：
		- New users and new items enter the system on a daily basis, 从而造成**OOV**问题 
		- stale items are gradually vanishing.
	- 数据是严重不均衡的：small number of training examples on infrequent feature values，这些特征的embedding是学不好的
- Embedding Table: 可以看成1-layer wide NN
- 提出解决方法代替Embedding Table
	- we seek to explore a deep, narrow, and collision-free embedding scheme without using embedding tables. 
	- We propose the Deep Hash Embedding (DHE) approach, that uses dense encodings and a **deep embedding network** to compute embeddings on the fly. 
		- we use *multiple hashing* and appropriate transformations to generate a *unique, deterministic, dense, and real-valued vector* as the identifier encoding of the given feature value, 
			- 解决了huge vocabulary size的问题 
		- and then the deep embedding network transforms the encoding to the final feature embeddings.

## 传统基于Embedding Table的方法
- hash-trick是解决huge vocabulary size的一个方法，但是仍然无法避免collision
- 一个方法，就是每个特征不再只做一次hash
	- The core idea is that the concatenated encodings are less likely to be collided. 
	- We can lookup **𝑘 embeddings in 𝑘 embedding tables (respectively)** and **aggregate** them into the final embedding
	- 可以如上用k个独立的embedding table，可以k次hash还是使用一个**共享的embedding table**
	- A common aggregation approach is ‘add’
	- 代价还是比较大的（特别是独立embedding table），一般K取值较小，一般=2 ^73234c

## DEEP HASH EMBEDDINGS (DHE)
与传统方法比较
- embedding table
	- Encoding过程：将feature id映射成一个sparse one-hot encoding，唯一的value=1还是一个整数
	- Embedding过程：embedding table相当于一个1-layer shallow network
- DHE
	- Encoding过程：通过multiple-hashing，将一个feature id映射成一个**real-valued dense** vector
	- Embedding过程：不再是shallow，而是用deep NN来做embedding

### Dense Hash Encoding
- Step1: 𝐸 : N → $R^k$ uses **𝑘 universal hash functions** to map a feature value to a 𝑘-dimensional dense but integer encodings.
	- $E^{\prime}(s)$=$[H^{(1)}s,H^{(2)}s,...,H^{(k)}s,]$
	- $H^{(1)}: N \rightarrow {1,2,...,m}$，m是一个非常大的数，比如$10^6$
	- [[特征交互#^73234c | 与double-hashing不同]]，这里k取得越大越好。==好像解决了hash collision的问题==。
- Step2: $E^{\prime}(s)$是全部由整数组成的向量，不适合喂入dnn，需要做转换
	- 可以做uniform normalization到\[-1,1\]。
	- 在uniform normalization之后，可以再standard normalize到N(0,1)
	- 根据作者的经验，uniform normalization就已经足够了，没必要再做后面的standard normalization。
- 注意：
	- k hashing function是**deterministic and non-learnable**，不像embedding table那样是要学习的参数
	- 与embedding table不同，尽管这里也将feature id转化为一个k维的real-valued dense vector，但是transform on the fly，而不像embedding table那么要占据空间
	- k hashing可以并发进行

### Deep Embedding Network
![[Pasted image 20220226150051.png | 400]]
所谓的embedding network与常规的dnn没有任何区别。
We transform the 𝑘-dim encoding via ℎ hidden layers with DNN nodes. Then, the outputs layer (with 𝑑 nodes) transforms the last hidden layer to the 𝑑-dim feature value embedding.
However, we found that ==training the deep embedding network is quite challenging (in contrast, one-hot based shallow networks are much easier to train). We observed inferior training and testing performance==.
However, we found the embedding network is ==underfitting instead of overfitting== in our case, as the embedding generation task requires highly non-linear transformations from hash encodings to embeddings.
- We suspect the default ReLU activation is not expressive enough, as ReLU networks are piece-wise linear functions
- We found the recently proposed Mish activation $𝑓 (𝑥)=𝑥 · tanh(ln(1 + 𝑒 𝑥 ))$ consistently performs better than ReLU and others. 
- We also found batch normalization (BN) can stabilize the training and achieve better performance. 
- However, regularization methods like dropout are not beneficial,

### Side Features Enhanced Encodings for Generalization
传统的embedding table not imply any inherent similarity between different IDs. 可以用side feature来improve generalization，但是那一般都是用于训练模型，而不是用于提升embedding的质量。
？？？但是问题在于，你的这种方法还区分得出，哪里是在训练embedding，哪里是在训练model吗？所谓的embedding network不是和model是一样的吗？？？

One straightforward way to enhance the encoding is 
- directly concatenating the generalizable features and the hash encodings. 这种作法是将side feature喂入dnn有啥区别？？？
- The enhanced encoding is then fed into the deep embedding network for embedding generation