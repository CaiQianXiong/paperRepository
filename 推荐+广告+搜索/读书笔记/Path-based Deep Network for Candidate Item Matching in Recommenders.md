---
alias: PDN
发布时间: 2021-05-18
出品方: 阿里
价值: ⭐⭐⭐
环节: 召回
---
关键词:: #Reco/多样性 

---

# Path-based Deep Network for Candidate Item Matching in Recommenders

## 基本思路
- 非常普通的i2i“两步走”召回
	- 用user behavior sequence中找出哪些重要的history item
	- 然后再用这些重要的history item做i2i召回
- 区别
	- 传统方法中，
		- 如何从user behavior sequence中找出重要history item是依赖规则的，比如只选完成度>90%的
		- i2i通过另外的模型来完成，比如通过word2vec计算出item embedding，或者统计出item co-currence来统计出每个item的相似item
		- 两个步骤之间存在gap
	- 这里，尽管线上召回时还是两步走，但是训练时却是end-to-end一体的
		- 第一步，user选取重要的trigger（i.e., history item）和第二步，计算item-item相似性，都是为一个优化目标而训练出来的
		- 消灭了两阶段之间的information gap & target gap

- 传统方法的缺点
	- i2i: 缺少个性化
	- user-item双塔：只生成了一个user embedding，个性化太弱

![[算法周报-20220525#^1yuhvq]]

## 模型结构

### 整体结构
![[Pasted image 20220522094329.png |500]]

![[Pasted image 20220522094456.png | 400]]
- $p_u$和$q_i$是双塔生成的user embedding和item embedding
- $N(u)$是一个user历史序列中的item个数
- $TrigNet(z_u,a_{uj},x_j)$是用户对每个history item的打分
	- $z_u$是user profile，除了user_id这样的人口画像，也包括一些“对各category点击次数”之类的动态画像
	- $x_j$ 是history item
	- $a_{uj}$ 应该是user 'u'对history item'j'的一些动作程度之类的
- $SimNet(x_j,c_{ji},x_i)$是history item "j"与target item "i"之间的相关性
	- $x_j$是history item
	- $x_i$是target item
	- $c_{ji}$是两者共现的一些信息，比如共现次数，或者两者的相关系数

### 一条Path
- TriggerNet
	- 无非就是把user profile, history item先embedding再拼接送入mlp
	- 得到一个实数，表示history item对user的重要性
	- ![[Pasted image 20220522095341.png | 400]]

- SimNet
	- ![[Pasted image 20220522095412.png |400]]

- 用户通过某个hisory item "j"对target item "i"的打分
	- ![[Pasted image 20220522095517.png |400]]

### 最终的预测与Loss
- 最后还加上一个shallow tower来考虑bias
	- position bias, hour feature for temporal bias, ......

- ![[Pasted image 20220522095752.png |500]]
	- softplus: $y=log(1+e^{-x})$
	- $d_{u,i}$是由双塔组成的DirectNet的点积

## Online Serving

### 离线计算i2i的相似度

- 如果计算所有$N \times N$的相似性得分，计算量太大
- 只考虑在如下item-item对上才计算相似度
	- 在同一个session中被点击过的
	- 具有相同attribute的items，比如同属一个品牌的
- 把这些候选$<item,item>$pair喂入SimNet，打分相似度
	- ❓❓❓为什么要把两个item都喂入模型才能得到相似度
	- 得到所有item embedding，直接点积，不就行了吗？
- 每个item只保留最相似的K个，一共N\*k个pair和相似度得分$s_{ji}$建立index

### 在线召回
![[Pasted image 20220522102031.png |300]]
- 在线模型，只部署TrigNet
- 一次用户请求，用TrigNet给他的所有history item打分$t_{uj}$，选出top-m triggers
- 每个trigger再去offline index找出k个与这个trigger相似的item
- **一共$m \times k$个candidate item**，再逐一用以下公式打分
	- ![[Pasted image 20220522101613.png |400]]
	- $d_{u,i}$是由双塔组成的DirectNet的点积
	- <span style="color:orange;font-weight:bold">一共要打分m*k次，这个计算量也不低</span>


