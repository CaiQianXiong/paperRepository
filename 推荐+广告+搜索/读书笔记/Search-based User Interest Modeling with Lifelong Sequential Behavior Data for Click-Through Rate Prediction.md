---
alias: SIM
发布时间: 2020-06-29
出品方: 阿里妈妈
价值: ⭐⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/用户行为序列 

---

- 技术路线是和DIN一脉相承，
	- DIN是用target item去和user history做attention，实际上是加强与target item相关（i.e., 或者引申为类似）的user history item的权重
	- 如果用到long history中，理想情况自然也拿target item与long history中的每个item做attention，但是代价太大
	- 所以思路变成了，既然target attention是从history sequence中给与target item相似的用户历史加大权重，相当于一种soft filtering。现在既然在long history上soft filtering行不通，干脆变成hard filtering。

- 所以整个SIM分两步走完成
	- General Search Unit (GSU)将long history进行hard filtering，从中挑选出与target item相似的history item。一般由<span style="color:red;font-weight:bold">万级别-->百级别</span>。
	- Exact Search Unit(ESU)与传统的DIN/DIEN没太大的区别。再拿target item在hard filtering后的Sub user Behavior Sequence(SBS)上做attention。

``` ad-note
title: 该方法能够适用的数据规模

- 淘宝的公开数据集
	- The maximum behavior sequence length of the Taobao dataset is 500. 
	- We split the recent 100 user behaviors as short-term user sequential features and 
	- the recent 400 user behaviors as long-term user sequential features
- 淘宝的工业级数据集
	- In this dataset, user behavior feature in each day’s sample contains historical behavior sequences from the preceding 180 days as long-term behavior features and 
		- Over 30% of samples contain sequential behavior data with a length of more than 10000
		- Moreover, the maximum length of behavior sequence reaches up to 54000
	- that from the preceding 14 days as short-term behavior features.
```

- 整体结构
	- ![[Pasted image 20220406150740.png | 700]]
	- 长短期历史都要参与建模
	- 从图中来看，无论是用hard-search还是soft-search，<span style="color:orange;font-weight:bold">都需要Index来加快检索SBS</span>

## General Search Unit (GSU)
- Hard Search
	- 与target item属于一个category中的long history item才进入SBS
	- 线上部署的时候，专门建立了一个user behavior tree(UBT)，是一个key-key-value的二层索引，方便快速查找SBS
		- 第1层key是user id
		- 第2层key是category
		- 第3层value是该user long history属于那个category的SBS
		- ❓其实论文里没说，我觉得这个UBT还应该有timestamp的功能，因为train/infer时都要拿target item在UBT中filter用户历史，千万不能发生time travel
	- 论文中说了，hard search与soft search效果差不多，而且显然deployment更方便
	- UBT index是天级更新的。“We build a two-stage index for the whole user behavior data offline, and it will be updated daily”

- Soft Search 
	- 说白了，就是拿target item embedding在long user history中找相似向量
	- <span style="color:orange;font-weight:bold">但是怎么得到target item embedding与long user history中的每个item embedding?</span> ❓
	- ![[Pasted image 20220406152237.png | 200]]
		- 就是拿target item和long user history一起做一个ctr模型。对long user history进行pooling时，也是拿target item与long user history做attention，再加权相加。
		- 问题的关键在于，作者认为long history和short history的数据分布不同，因此共享embedding是有害的。<span style="color:red;font-weight:bold">“It should be noticed that distributions of long-term and shortterm data are different. Thus, directly using the parameters learned from short-term user interest modeling in soft-search model may mislead the long-term user interest modeling”</span> 💡💡💡💡💡
		- 所以理论上，这个辅助模型喂入的完整的user long history。但是太长了，怎么办？作者的方案是sampling。<span style="color:skyblue;font-weight:bold">“it is impossible to directly fed the whole user behaviors into the model.
In that situation, one can randomly sample sets of sub-sequence from the long sequential user behaviors, which still follows the same distribution of the original one.”</span>
		- 换句话说，不用上述模型也可以，<span style="color:orange;font-weight:bold">比如用双塔召回时的item embedding，但是是否考虑了long/short item embeding分布不一致的问题</span>💡
	- 有了embedding，<span style="color:red;font-weight:bold">也不可能拿target embedding线性地与long history中的每一个item embedding做dot</span>
		- 如果逐一做dot，时间复杂度是=O(L\*B\*d)，L是历史长度，B是batch_size，d是向量维度
		- “<span style="color:orange;font-weight:bold">To further speed up the top-K search over ten thousands length of user behaviors, sublinear time maximum inner product search method ALSH is conducted based on the embedding vectors E to search the related top-K behaviors with target item</span>”，所以也需要[[End-to-End User Behavior Retrieval in Click-Through Rate Prediction Model#^g3gjgh | 建立索引]]。
			- 但是肯定不能像faiss那样拿全部item embedding建立索引，是否也要hard search那样，为每个userid建立自己的索引？❓
	- 另外，论文里还说<span style="color:blue;font-weight:bold">这个soft-search GSU作为辅助任务与主模型共同训练</span>
		- 我觉得这个难度挺高的，soft-search GSU应该多用long history以学习出long/short之间的数据分布差异。即使用了sampling，喂入soft-search gsu auxiliary task的user history也不会太短
		- 但是这样一来，训练的时间会不会大大增加？<span style="color:purple;font-weight:bold">是否还能够满足online learning的需求？</span> ❓❓❓❓❓
 ^qrlc73
- 根据我在快手的经验，对于long history这一块，实际上是采取了类似[[End-to-End User Behavior Retrieval in Click-Through Rate Prediction Model#^fiyiqe | Feature Store]]的特征提取方案💡💡💡💡💡
	- short user history是将一个item id数据直接保存进样本快照，再交给下游来提取特征
	- long user history没有将几万个item id也喂入快照，而是train/infer时直接拿target item去user behavior tree去重新抽取
``` ad-note
title: 长期历史没有进入样本快照
- 有一种想法，既然实际进入模型的是SBS，是不是将SBS进入样本快照作为训练数据就可以了。训练的时候，无须再拿target item对完整的long history进行二次提取。⚠️
- 如果是hard search也无所谓，反正category也不会变，而且不能发生time travel是基本原则，那么在train时再次筛选一遍，应该与inference时筛选出的结果是完全相同的。

‼️问题出在soft search时，
- 如果将infer时筛选出的sbs加入snapshot，train时不再进行二次提取，保持了train/infer是的consistency。
	- 但是，filtering过程是否得不到优化。毕竟，ground-truth是用户的完整long history，而非filtering出来sbs
- 如果不将infer时筛选出的sbs加入snapshot，而是在train时用target item embedding重新提取sbs
	- 当前前提肯定是绝对不能发生time travel，user long history绝对不会包括用户刚刚点击过的那个item
	- 如果target item embedding在infer时和train时已经发生了变化？train时得到的sbs和infer时的不一致
	- 问题在于target item embedding如果不是ctr model中所使用的，而是离线的，那它不会被优化，属于一个外部条件，理应在train/infer时保持不变。💡💡💡
```

## Exact Search Unit (ESU)
- 和DIN一样，只不过要注意每个sbs item embedding后面还要拼接上time diff embedding
