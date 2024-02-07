---
alias: CDR
发布时间: 2021-02-17
出品方: 腾讯微信
价值: ⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/用户行为序列, #Reco/多样性 

---

- 要解决的问题
	- 要将disentagled user interest学出来
	- 依赖的是多种user feedback（之前的工作都只依赖一种动作）
		- 问题1：多种feedback之间有复杂关系
		- 问题2：feedback有噪声
		- 比如：unclick可能是因为不喜欢，也可能因为喜欢看看过类似的

- related work
	- Disentangled Representation Learning: 要学出相互关联较小的，多个latent factor
	- Curriculum Learning: 动态调节sample weight

# Co-filtering Dynamic Routing

## 第1步：衡量序列元素的重要性
像是DIN的扩展

### 准备
- 组成三个序列, clicked sequence, dislike sequence, unclicked（曝光未点击） sequence
	- 序列中每个embedding，是item id embedding + category（应该是动作类型） embedding

- 每个序列上都套用transformer encoder，也就是self-attention
	- click/unclick/dislike sequence的原始长度分别是`m/n/l`
	- self-attention后，得到$z_c,z_u,z_d$还分别是`m/n/l`这么长

### 用dislike算click/unclick的权重
- 把encoded dislike sequence做一个average pooling，得到表示第v个user的negative intention的向量，![[Pasted image 20220505154020.png | 100]]
	- 好处是dislike的信号是非常强的，click不代表喜欢，但是dislike是一定不喜欢
	- 坏处是，dislike太少了吧？？

- 拿$n^v$ 当query，计算clicked sequence中每个item的importance。
	- 与negative intention越相似的item的importance越小
	- ![[Pasted image 20220505154245.png | 300]]

- 拿$n^v$ 当query，计算unclicked sequence中每个item的importance。
	- ![[Pasted image 20220505155652.png | 300]]
	- 也是与dislike越相似，该item对user interest的importance就越小
	- 作者觉得unclicked（曝光未点击）噪声大，还考虑了user profile

### 用target item和最近点击item衡量click权重
- ![[Pasted image 20220505160315.png | 500]]
- $z_{cm}$是最后一个clicked item在self-attention之后的向量，m是clicked sequence的长度
- p是position embedding，应该考虑了click time距离当前时间的时间差

### 用target item衡量unclick权重
- ![[Pasted image 20220505160609.png]]
- 用candidate item来衡量，类似din

## 第2步：聚合成多个兴趣向量
公式写得稀碎 👿

- 先定义K个向量，算是K种兴趣的锚点
- 计算每个clicked/unclicked item与某个兴趣锚点的相似度
	- ![[Pasted image 20220505162617.png | 500]]
- 用户v在第k类兴趣上的表达
	- ![[Pasted image 20220505162729.png | 500]]
	- $\lambda<1$算是unclicked item的置信度
	- $\beta_k$ 是第k类兴趣的bias（也是待学习的参数）
	- $z1_{ci}^{(v)}$ ：user "v"的clicked sequence中的第i个item，经过negative intention filtering之后的一个表达式
	-  $d_{ci}^{v}$ 是用negative intention给clicked item计算的权重
	- $f_{ci}$是用most recent clicked item与target item给clicked sequence打的分
	- $c_i$ 是clicked sequence item与K类兴趣的相似度

## 第3步：预测
- part1: 遍历所有K类兴趣，每个user interest与item embedding做点积，再加和

- part2：基于user profile和item feature打一次分
	- 首先拿user profile的各个field，和item feature各个field，做multi-head self-attention，![[Pasted image 20220505164246.png | 300]]
	- 再把self-attention结果喂进MLP

- 最终的预测得分
	- ![[Pasted image 20220505164730.png | 500]]

- loss
	- ![[Pasted image 20220505203504.png | 500]]
	- 最后一项，==使不同interest category的距离越远越好== ^0e7bre

# Adjustable Self-evaluating Curriculum towards A Better Self
- curriculum learning的目的在于denoise

- 训练过程中动态调节每个sample的weight
	- ![[Pasted image 20220505204831.png | 500]]
	- 要调的超参数太多了👎👎👎

