---
alias: DPP
发布时间: 2018-05-26
出品方: Hulu
价值: ⭐⭐⭐⭐⭐
环节: 重排
---
关键词:: #Reco/多样性 
URL::

---

# Fast Greedy MAP Inference for Determinantal Point Process to Improve Recommendation Diversity

- 参考
	-  [Determinantal Point Process（DPP） 相关论文解读](https://zhuanlan.zhihu.com/p/356094474)
	- [Hulu 基于行列式点过程的推荐多样性提升算法](https://mp.weixin.qq.com/s/oLG3f0mw1L5i2qMnHLugZA)
	- [开源实现](https://github.com/laming-chen/fast-map-dpp/blob/master/dpp.py)


## 梳理逻辑脉络
- 将用户从推荐出去的N个item中点击其中k的过程，“假定”“想像”成一个DPP
	- 没有论证过，如此想像，就是为了日后计算概率方便

- 那想像成DPP后，矩阵从哪里来？
	- 并不是通过supervised的方式，通过后验数据（历史样本，推荐了M个，点击了哪几个）拟合出来的
	- **完全是heuristic的**
		- 合理假设：用户喜欢相关性高的，不喜欢与之前重复的
		- 根据以上假设，才构建出一个对象阵都是rank prediction score，对角性上是相似度的矩阵来
		- 因为这样构建矩阵，根据DPP推导出来的用户点击哪些的概率（也就是如此构建矩阵的determinant）与rank prediction正相关，与彼此之间的相似度负相关。

## 求解DPP Mode的算法
- 按照贪心算法来求解 (1)
	- ![[Pasted image 20220710143448.png |500]]

### 化简优化目标
- Cholesky decomposition (2)
	- ![[Pasted image 20220710144155.png |100]]

- 公式(3) ^e3ufv2
	- ![[Pasted image 20220710144036.png |500]] ^yof660

- 公式（4） ^0u8tfx
	- 由[[Fast Greedy MAP Inference for Determinantal Point Process to Improve Recommendation Diversity#^e3ufv2 |公式(3)]]$\Longrightarrow$![[Pasted image 20220710144452.png |200]]

- 公式（5）
	- 由[[Fast Greedy MAP Inference for Determinantal Point Process to Improve Recommendation Diversity#^yof660 |公式(3)]]根据$det(AB)=det(A)det(B)$，推导出公式（5）
![[Pasted image 20220710144923.png |400]]

- 公式（6），简化后的优化目标
	- 因为$Y_g$已经固定了，所以公式（1）变成![[Pasted image 20220710145931.png |200]]

### 怎么迭代求解
- 如果公式（6）求解完毕，得到最优的j，那么$c_j$、$d_j$也就能够得到了
	- ![[Pasted image 20220710150337.png |250]]

- ==注意，对于同一个item 'i'，$c_i$和$d_i$是在不同变化的，而且可以增量变化==
	- $c_i$和$d_i$，对应$Y_g$，满足的是$L_{Y_g\cup{i}}$的Cholesky分解，即![[Pasted image 20220710151626.png |300]]
	- 当$Y_g\longrightarrow Y_g\cup{j}$后，$c_i$和$d_i$需要更新成$c^{\prime}_i$和$d^{\prime}_i$，$c^{\prime}_i$和$d^{\prime}_i$满足的是$L_{Y_g\cup{j}\cup{i}}$的Cholesky分解


- 原来有![[Pasted image 20220710152111.png |100]]，因为$Y_g$变成了$Y_g\cup{j}$，现在变成了![[Pasted image 20220710152230.png |200]]
	- 再根据$L_{X,Y}$就代表由X行、Y列抽取出来的子矩阵这个定义
	- ![[Pasted image 20220710152452.png |400]]
	- 求解这个方程，就得到![[Pasted image 20220710152550.png |300]] ^37c93w

- 再根据[[Fast Greedy MAP Inference for Determinantal Point Process to Improve Recommendation Diversity#^0u8tfx |公式（4）]]，得到![[Pasted image 20220710152824.png |400]]

- 算法伪实现
	- ![[Pasted image 20220710154744.png |500]]
	- The stopping criteria is 
		- $d_j^2$< 1 for unconstraint MAP inference，否则不再正定
			- we add $d_j^2 < \varepsilon$ to the stopping criteria, 保证计算$\frac{1}{d_j}$时的算法稳定性
		- or $|Yg| > N$ when the cardinality constraint is imposed.

### 只要求在滑窗内保持多样性
很麻烦，连证明都放在附录中
![[Pasted image 20220710161444.png |500]]


### 算法的时间复杂度

- 传统方法
	- Another method is the widely used greedy algorithm
	- Known exact implementations of the greedy algorithm have $O(M^4)$ complexity, where M is the total number of items. 
	- 有牺牲精度的简化算法，reduces the complexity down to $O(M^3)$ by introducing some approximations, which sacrifices accuracy
	- 本方法, we propose an *exact implementation of the greedy algorithm* with $O(M^3)$ complexity, and it runs much faster than the approximate one  empirically.

- Fast Greedy MAP算法
	- 如果精排的结果集大小是M，全重排一遍，时间复杂度是$O(M^3)$
	- 如果只返回其中的N个，时间复杂度由$O(M^3)$降低为$O(N^2M)$
	- 如果还只是要求在一个w大小的窗口内保持diverse，时间复杂度可以进一步降低为$O(wNM)$

- 这是因为[[Fast Greedy MAP Inference for Determinantal Point Process to Improve Recommendation Diversity#^37c93w |第k轮时，要两个k长度的向量点积]]
	- 第k轮的时间复杂度就是O(kM )
	- 所以，runs in $O(M^3)$ time for unconstraint MAP inference and 
	- $O(N^2M)$ to return N items.

## 如何构建DPP的矩阵

每个item向量是精排打分$r_i$，与粗排得到的embedding $f_i$的乘积
![[Pasted image 20220710161734.png |400]]
而且，粗排向量都是经过L2-normalize的![[Pasted image 20220710161821.png |400]]

![[Pasted image 20220710162002.png |300]]，其中![[Pasted image 20220710162026.png |100]]

![[Pasted image 20220710162123.png |500]]
体现出既要相关度高，又要diversity

### 引入超参平衡相关性与多样性
如果需要引入一个超参，a tunable parameter which allows users to adjust the trade-off between relevance and diversity
![[Pasted image 20220710162620.png |400]]

![[Pasted image 20220710162701.png |400]]
![[Pasted image 20220710162712.png |150]]

### 保证相关性为正的技巧
we need the similarity Sij ∈ \[0, 1\] for the recommendation task. This may be violated when the inner product of normalized vectors $〈fi, fj〉$ can take negative values.

To guarantee nonnegativity, we can take a linear mapping while keeping S a PSD matrix
![[Pasted image 20220710163024.png |500]]