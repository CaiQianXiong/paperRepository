---
alias: 
发布时间: 2021-06-17
出品方: 阿里
价值: ⭐⭐⭐
环节: 召回
---
关键词:: #Reco/双塔模型 #Reco/用户行为序列 
URL:: [zotero](zotero://open-pdf/0_22EYSHVJ/1)

---
# Embedding-based Product Retrieval in Taobao Search

## 概述

- 要解决的一个主要痛点是：搜索与推荐不同，<span style="color:orange;font-weight:bold">搜出来的东西必须与query有非常强的相关性，不能太发散</span>
	- 我看论文解决这个问题，最大的努力就是拿query去和实时、近期、长期用户行为序列，做attention
	- 但是推荐场景下没有query，召回时又没有target不能像DIN那样做target-attention，可以拿last clicked item当query，相当于用user最新的兴趣当尺子去衡量其他序列成员的轻重。

## 模型结构

### 如何得到query embedding

总而言之，就是将query sentense各种变形+各种交叉，组成一个embedding
![[Pasted image 20220524141751.png |400]]

### 用query对三类序列做attention

- 按时间划分三类行为序列
	- real-time $R^u$点击序列, before the current time step)
	- short-term $S^u$点击序列, before R and within ten days) 
	- long-term sequences $L^u$, before S and within one month
		- 包含了click, buy, collect等各种行为

- sequence中的每个item embedding是拼接而成
	- 包含id embedding
	- 各种side information embedding

- 对realtime sequence
	- 先让realtime sequence做LSTM
	- 现在LSTM得到的隐向量上做self-attention, 得到![[Pasted image 20220524142459.png |150]]
	- <span style="color:cyan;font-weight:bold">再加上一个全0向量</span>，得到![[Pasted image 20220524142534.png |200]]
		- 否则attention总会注意到一些什么。加上全0向量，就是允许attention to no previous hisotry
	- 再拿querry做attention，得到![[Pasted image 20220524142628.png |200]]

- 对short-term sequence
	- 处理方法与realtime sequence类似，只是没有LSTM
	- 先做self-attention，得到![[Pasted image 20220524142805.png |150]]
	- 再加一个全0向量，允许attention to nothing，得到![[Pasted image 20220524142913.png |200]]
	- 再做query-attention，得到![[Pasted image 20220524142954.png |200]]

- 对long-term sequence
	- <span style="color:crimson;font-weight:bold">再在long-term sequence上做attention是行不通的，因为计算量太大</span>。
	- 对于long-term item sequence
		- 先把click/buy/collect item三种sequence，每种序列内部都做mean pooling，得到![[Pasted image 20220524144447.png|200]]
		- 再拿query对以上4个向量做attention, ![[Pasted image 20220524144530.png |200]]
	- 再对long-term shop/leaf/brand sequence做类似的操作
	- 最后user的长期兴趣表达，是以上各种embedding的按位相加，![[Pasted image 20220524144659.png |250]]

### user tower
user tower没有传统的DNN操作，是用self-attention，对query, realtime user interest, short-term user interest, long-term user interest做了一个fusion.

1. 加上一个CLS，把query和三类兴趣，拼接一个大向量，![[Pasted image 20220524145049.png |200]]
2. 再做self-attention，CLS对应的embedding就是user tower的最终输出，![[Pasted image 20220524145524.png |250]]

- 疑问
	- 为什么不直接拼接起来，喂入DNN，反而用这种“反传统”的方式？
	- 给一个句子加上一个CLS，让self-attention中CLS对应的embedding，代表整句的embedding，算是NLP transformer中的传统艺能。
		- 但是这里也不像transformer那样是multi-layer self-attention，只做一篇self-attention的话，为什么不用cls当query对剩下的![[Pasted image 20220524145824.png |150]]做attention不就结了吗？
		- 而且不用self-attention，也就避免了cls对cls自己计算权重并相加

### item tower
很奇怪，item侧也没有DNN，非常非常浅
![[Pasted image 20220524151059.png |200]]
- e是item id embedding
- $w_i$ 算是item title分词的embedding

## Loss
- 没有用pairwise loss，
	- 说是因为inference时，是由all candidate中选，需要有global comparision
	- 而pairwise loss只注重local comparison，缺乏全局比较能力❓--- 一派胡言

### 温度系数平滑softmax
![[Pasted image 20220524152413.png |500]]
- 可能就是batch内的负采样
- 采用了温度系数$\tau$进行平滑
	- $\tau$无穷小时，相当于拟合one-hot分布，无限拉大正样本和负样本之间的差距；
	- $\tau$无穷大时，相当于拟合均匀分布，无视正样本还是负样本
	- 这里要**增大一些$\tau$，不想一味拟合click信号**。因为作者觉得点击受其他因素影响太大，不完全是源于query~product之间的匹配度。

### 生成hard negative
使用user embedding去Item Pool中找到和其"点积最大"的top-N个负样本集合：$I_{hard}$，然后通过插值的方式，来混合正样本和困难负样本，形成N个困难负样本
![[Pasted image 20220524153828.png |300]]
其中，$\alpha$是从均匀分布中采样到的，$\alpha$越接近1，生成的样本越接近正样本，生成的样本越困难。--- **瞎搞，正样本就是正样本，把正样本的一部分加入负样本，是什么道理**❗❗❗

