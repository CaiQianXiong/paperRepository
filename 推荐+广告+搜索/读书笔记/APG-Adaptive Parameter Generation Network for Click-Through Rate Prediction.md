---
alias: APG
发布时间: 2022-03-30
出品方: 阿里
价值: ⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/多场景推荐, #ML/MetaLearning 

---

# 存在问题
dnn weight是静态的，忽视训练数据中存在数据分布差异，dnn weight被高频数据主导。“the shared parameters tend to be dominated by high-frequency features and may give a misleading decision for the long-tailed instances”

和[[One Model to Serve All-Star Topology Adaptive Recommender for Multi-Domain CTR Prediction | STAR]]相比，
- 相同点是，都存在一个dnn weight被调制的过程。STAR中，各scenario tower weight都被common tower weight所调制。
- 但是，参与调制的common tower weight也是static and shared


# 方法
使用dynamic generated weight
![[Pasted image 20220418110323.png | 500]]
![[Pasted image 20220418110248.png | 500]]

## Condition Design
哪些特征喂入meta neural network作为输入来生成dynamic weight，这些特征必须能够提供给模型足够的“先验知识”。
![[Pasted image 20220418110147.png | 500]]

- Group-wise
	- 在训练样本中分群，比如来自同一个用户的样本属于一个群体。
	- 想做到，每个用户都有自己的weight
- Mix-wise 👎👎👎
	- input aggregation: 多个condition embedding先聚合，再喂入APG
	- output aggregation：先喂入APG，再聚合结果
- Self-wise👎👎👎
	- 每一层的weight都是自适应的
	- 下一层的输出不仅是上一层的输入，还决定了上一层的权重
	- 每层的权重都自适应，简直就是走火入魔☠️

## Re-Parameterization
### 技术难题
由一个D维的输入$z_i$，生成一个$N\times M$的权重矩阵$W_i$
![[Pasted image 20220418105203.png]]

- 技术难题1：生成这样矩阵的时间和内存复杂度都太高
	- 生成这样一个矩阵的时间复杂度和内存复杂度=O(NMD)
	- 生成之后，再进行前代的时间复杂度=O(NM)
	- 总复杂度=O(NMD+NM)
	- 这还只是一层而已，而且实际系统中N & M一般都在thousand量级
- 技术难题2：生成的$W_i$只与$z_i$有关，太senario sensitive了，而忽略了一些common knowledge

从而提出re-parameterization，分为如下四步：
1. low-rank parameterization
2. decomposed feedforwarding
3. parameter sharing
4. over parameterization
![[Pasted image 20220418110023.png | 700]]

### low-rank parameterization
![[Pasted image 20220418110517.png | 500]]![[Pasted image 20220418110542.png | 500]]

### decomposed feedforwarding
有了U、S、V这三个小矩阵，在前代的时候，不用重新将USV相乘重组成W再前代，而是将USV依次使用于输入
![[Pasted image 20220418110940.png]]
![[Pasted image 20220418110957.png | 500]]

### Parameter sharing
- 为了进一步减少时间复杂度和空间复杂度，
- 另外，也是为了能够增强对common pattern的学习能力
- 分解的三个小矩阵中，只有中间的小方阵是由$z_i$动态生成，剩下的两个矩阵还是static defined
![[Pasted image 20220418111249.png | 500]]

### Over Parameterization
因为U & V变成了static defined，又开始担心这两个矩阵太小，表达能力不足。人为增加更多的学习参数
![[Pasted image 20220418111744.png | 500]]

但是，
- 这种over-parameterization只发生在training phase。
- inference之前，还是需要将U & V都提前计算好，以节省inference时间。
![[Pasted image 20220418111934.png | 500]]

### 各简化步骤的复杂度对比
![[Pasted image 20220418140739.png | 700]]