---
alias: 
发布时间: 2019-09-16
出品方: Google
价值: ⭐⭐⭐⭐⭐
环节: 召回
---
关键词:: #Reco/双塔模型 #Reco/样本策略 
URL:: [zotero](zotero://open-pdf/0_42ZYQZ7Q/1)

---

因为是batch negative sampling，所以可以将分母限定在一个batch内的样本
![[Pasted image 20220621181845.png |300]]
![[Pasted image 20220621182024.png |300]]
Here $p_j$ denotes the sampling probability of item j in a random batch.

前面的$r_i$ 代表user engagement，比如时长、完成度之类的。
![[Pasted image 20220621181915.png |400]]

- 另外在计算`s(x,y)`时要注意normalization和temperature
	- normalization，也就是说，要用cosine而不是dot-product，能够improves model trainability
		- ![[Pasted image 20220621182425.png |300]]
		- ![[Pasted image 20220621182446.png |300]]
	- a temperature τ is added to each logit to sharpen the predictions
		- ![[Pasted image 20220621182556.png |300]]
