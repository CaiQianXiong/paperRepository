---
alias: M2M
发布时间: 2022-02-18
出品方: 阿里
价值: ⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/多场景推荐, #ML/MetaLearning 

---

# 背景
- 现代推荐系统，肯定都是多任务的。
- 多场景
	- 应用于多个国家算是多场景
	- 应用于首页推荐、猜你喜欢等场合，也是多场景
- 难点
	- 每个场景建一个模型，浪费资源，难于维护
	- 新场景也没有那么多数据独立训练，也确实需要迁移
	- 跨域推荐这个迁移规则，很难学

# 模型
![[Pasted image 20220412123835.png | 500]]

## 基础模型Backbone
- 基础就是一个针对multi-task的MMOE模型
	- 底层有个embedding layer
	- 上面接一个multi-head transformer
	- 最后喂入MMOE，每个expert一个embedding

Task embedding as Anchor ^vbczri
``` ad-quote
title: 有一个比较新颖的点就是针对每个task引入一个task embedding，作为anchor point。

we also embed all the tasks in the same space, which act as the “anchor points” of the tasks and introduce prior knowledge of tasks and influence the weight of feature information
```
 一般MMOE为每个task生成各expert weight的输入也是与expert同样的input。接下来会看到，这里是将这个task embedding用于生成各expert weight。

## Meta Unit
就是将scenario related feature喂入一个MLP，输出向量通过reshape，成为另一个MLP的各层权重。we use scenarios knowledge˜S as the input of meta unit. The meta unit transforms the scenario knowledge into the **dynamic weights**. ^6mxrpw

Meta Unit在使用的时候，就是一个正常的MLP
![[Pasted image 20220412164550.png | 300]]
但是上述公式中的各层W和b是通过scenario-related feature动态生成的
![[Pasted image 20220412164700.png | 300]]
![[Pasted image 20220412164727.png | 300]] ^l0ffo1
### 相关方法
- 与[[One Model to Serve All-Star Topology Adaptive Recommender for Multi-Domain CTR Prediction | STAR]]模型类似的点是，每个scenario要pass的MLP的权重也是动态生成的。
	- 但是，STAR中各scenario-specific MLP的权重合成是非常简单的
- 与[[Personalized Transfer of User Preferences for Cross-domain Recommendation#Step2 Meta Network | 微信的作法]]类似
- 就是LHUC中拿重要特征当裁判的作法，道理相通

## Meta Unit的应用
有了Meta Unit，接下来的操作就是用Meta Unit来替换原有的MLP。使得每个MLP都具备根据scenario动态调整的能力。

- 一个应用场景，是在聚合各expert embedding时，使用了Meta Unit
	- ![[Pasted image 20220417124257.png | 150]]![[Pasted image 20220417124326.png | 100]]
	- $T_t$就是task embedding，作为task的[[Leaving No One Behind-A Multi-Scenario Multi-Task Meta Learning Approach for Advertiser Modeling#^vbczri | Anchor]]
- 另一个应用场景，就是普通的Residual Network替换用Meta Unit来实现。