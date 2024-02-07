---
alias: CMDM
发布时间: 2021-12-07
出品方: 阿里
价值: ⭐
环节: 召回
---
关键词:: 

---
# CMDM：基于异构序列融合的多兴趣深度召回模型在内容平台的探索和实践
- [链接](cubox://card?id=ff808081808324e001808a427afe4ef4)

## 问题
- 输入有两类用户序列：
	- 一个是对内容的行为序列，
	- 一个是对商品的行为序列

## 方法
### 两层Attention
- ![[Pasted image 20220505083322.png | 500]]
- 第1层attention
	- 对内容序列用mind，提取多兴趣
	- 对商品序列用multi-head attention
	- 没什么理论道理，就是试出来的
	- 问题：看结构图，对商品序列用的是self-attention，输入是T长的序列，输出也是T长的序列，没有压缩，都喂入上层模型？
- 第2层attention
	- 用户兴趣的三个组成部分：
		- 用户基础画像
		- 从内容序列中提取出来的用户兴趣
			- 使用MIND提取出多兴趣
		- 从商品序列中提取出来的用户兴趣
			- 应该是只提取出一个向量，就是self-attention中比如最后一个item对应的embedding表示兴趣
	- 这三个组成部分之间做一个attention加权求和
		- 可能是类似[[AutoInt-Automatic Feature Interaction Learning via Self-Attentive Neural Networks|AutoInt]]
	- 问题：
		- 看第一层的输出，特别是商品序列用的是self-attention，下边一层绿色圆圈，怎么就只有第一个圆圈喂进一层？怎么选出来的
		- 这第2层的attention，是什么对什么的attention?
### 负采样
- 随机采样一些easy negative
- hard negative:
	- 曝光未点击：用了没效果。因为对召回来说，曝光未点击也算是正亲本
	- 召回未曝光：有明显提升。类似级联学习了。