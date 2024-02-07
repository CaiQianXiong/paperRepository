---
alias:
发布时间: 2021-01-01
出品方: Facebook
价值: ⭐⭐⭐⭐⭐
---

环节:: 召回
关键词::  #Reco/双塔模型 

---
# Que2Search: Fast and Accurate Query and Document Understanding for Search

## 双塔模型

![|500](https://api2.mubu.com/v3/document_image/e9490277-8964-4525-9f39-c6f36d91df7a-11457030.jpg)

* 变之前的一个大塔由多个小塔
	* 每个小塔处理一部分信息，生成自己的embedding
	* 最后再由各个小塔的embedding融合成final embedding
* 融合的时候，
	* 尝试过最传统的先concatenate再mlp
	* 发现另一种所谓simple attention fusion的方式更好 （$\varphi_i$是第i个小塔得到的embedding） ^wfhjkv
		* ![|400](https://api2.mubu.com/v3/document_image/5c163e19-c483-44b4-8678-9fd307220d43-11457030.jpg)
* 对item tower增加“预测item是哪一类”的分类任务 #Reco/辅助训练

## Curriculum Training

注意💡💡💡， <span style="color:crimson;font-weight:bold">Facebook的easy traininig和hard training不是通过negative来实现的</span>。而是通过前后两轮的curriculumn training来实现的。

### 第一轮是Easy Training
- 样本
	- 整个batch都是正样本对，$q_i$与$d_i$是正样本，
	- 整个batch$<q_i,d_j>(j\ne i)$  ，$q_i$与其他所有$j\ne i$ 都是负样本 

- Loss
	- scaled multi-class cross entropy loss
		- ![[Pasted image 20220505103408.png | 500]]
		- 作者认为scale对于收敛非常重要
		- scale选择在\[15,20\]之间
		- 实操中，我不信分母会是整个batch_size=B
	- 也有一种Symmetrical Scaled Cross Entropy Loss
		- ![[Pasted image 20220505103740.png | 500]]
		- 对于第i个样本
			- 前半部分，是让$q_i$ 与所有d比较；
			- 后半部分，是让$d_i$ 与所有q比较

- 训练
	- 1st training通过在validation set early stop来停止 ^hfxon2
	- <span style="color:red;font-weight:bold">但是问题是，在online learning的情况下怎么实现early stop？</span>❓❓❓
	- early stop依据的metric是[[Que2Search-Fast and Accurate Query and Document Understanding for Search#^qnflqm | AUC]]

### 第二轮是hard training
* 样本
	* 正样本还是batch中的一条，$\langle q_i,d_i \rangle$ 
	* 负样本这次只有一个，就是除了i，与$q_i$ 的cosine similarity最大的$d_j$, ![[Pasted image 20220505111731.png | 150]]
		* <span style="color:blue;font-weight:bold">其实这个样本在第一轮中已经用过了</span>
	* 样本是pairwise loss时常见的tripplet $\langle q_i, d_i, d_{n^{q_i}} \rangle$

- Loss
	- <span style="color:orange;font-weight:bold">本轮的hard不是体现在hard negative上，而是体现在从multi-class cross entropy loss升级为pairwise loss</span>
	- 尝试了两种pairwise loss: bpr loss & margin-hinge loss
		- ![[Pasted image 20220505112936.png | 500]]
	- and observed that **margin rank loss with margin between 0.1 and 0.2 works best**

- 训练
	- initially, curriculum training with harder negatives did not work well.
	- 用了两个方法来缓解
		- 要等1st training收敛(early stop那种吗？)
		- 对batch内reduce所有loss时，要用sum而不是mean
	- 也都没啥道理，就是经验之谈

## 离线评估 

- Batch Recall@K
	- This metric measures whether diagonal element 𝑐𝑜𝑠 (𝑞𝑖, 𝑑𝑖 ) is among the top K scores of the row 𝑐𝑜𝑠 (𝑞𝑖, 𝑑𝑗 ), 𝑗 ∈ \[1, 𝐵\].
	- 用于在线监控指标

- AUC ^qnflqm
	- 人工制作数据集
		- 找一些query
		- 抓取这些query的search result
		- 人工打标1/0表示“是否相关”
	- 拿`cos<q,d>`与上述binary label计算AUC
	- 用于[[Que2Search-Fast and Accurate Query and Document Understanding for Search#^hfxon2 | early stop]]的依据

- KNN Recall@K
	- 不同于以上两个在线指标，这个指标是模型训练好之后的，离线评测指标
	- 训练好的embedding灌入faiss
	- 拿query embedding在faiss中进行ANN搜索，heck whether the original query’s ground-truth document is among the top K retrieved documents

## 多模块融合 Gradient blending

* 双塔，每侧塔都是由若干小塔组成的，最终final embedding是小塔embedding的融合
* 对于loss的设计，要求每个小塔都能发挥作用，而不允许偷懒

* **所以loss，既包括双塔final embedding来loss，也包括user/item中的某个小塔embedding对item/user final embedding来做loss**. 
	* for example we say the query tower has 𝑚 = 2 modalities: 
		* query and 
		* country; 
	* and the document tower has 𝑛 = 3 modalities: 
		* title, 
		* description and 
		* image. 
	* We then get 𝑚 + 𝑛 + 1 losses as formulated below, where 
		* ${\{\varphi_i^1\}}_1^m$是左侧的m个小塔各自输出embedding
		* ${\{\varphi_j^2\}}_1^n$是右侧n个小塔各自输出的embedding
		* and ⊕ denotes a fusion operation，可以是简单concat+mlp，也可以是[[Que2Search-Fast and Accurate Query and Document Understanding for Search#^wfhjkv | Simple Attention Fusion]]
* ![|300](https://api2.mubu.com/v3/document_image/4928031a-1f2e-4dc6-9dca-089038717685-11457030.jpg)
* 优化时肯定是以上loss的weighed sum，但是weight怎么定没有说明

## 线上inference加速

* 好像没说出啥来，就是一些试验出一些模型超参
* 一些针对faiss的优化

## 特征重要性

比如ablation test（某列特征清零或shuffle）