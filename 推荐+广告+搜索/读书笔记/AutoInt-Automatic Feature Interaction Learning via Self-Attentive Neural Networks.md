---
alias: AutoInt
发布时间: 2019-08-23
出品方: 北大
价值: ⭐⭐⭐⭐
环节: 
---
关键词:: #Reco/特征交互 

---

# AutoInt: Automatic Feature Interaction Learning via Self-Attentive Neural Networks

- 优点
	- 对于numeric & categorical feature都通用
	- 这个算是explicit feature interaction??
	- multi-head mechanism
		- project feature into different sub-space
		- hence, different feature interaction in different sub-space

## 要解决的痛点

- DNN的不足
	- MLP不能很好模拟multiplicative feature interaction
	- implicit feature interaction，不好解释

## 作法
- 多层multi-head self-attention + residual connection

### Embedding
- categorical feature embedding
	- 这个没啥新鲜的
	- 对于multi-value categorical field，就是field下多个feature embedding的平均

- 对于numeric feature
	- 第m个field是numeric field，值是$x_m$
	- 给field `m`定义一个field embedding, $v_m$
	- feature embedding $e_m = v_m x_m$  

- <span style="color:orange;font-weight:bold">缺点就是所有field embedding必须拥有相同维度</span>
	- 尽管self-attention中也有$W_Q$/$W_K$/$W_V$的mapping矩阵
	- 但是，如果参与self-attention的每个field embedding都有着不同的长度，每个field embedding在当key参与attention时，都要有不同的映射矩阵
	- 想想，实现起来也很麻烦

### Interaction
- self-attention时，$W_Q$/$W_K$/$W_V$是为了将本来相同的sequence embedding映射成含义不同的Q/K/V
	- $W_Q$/$W_K$/$W_V$为sequence中所有元素共享
	- 所以也就<span style="color:orange;font-weight:bold">要求参与self-attention的元素都必须具备相同长度的embedding</span>

- 单个head的self-attention
	- ![[Pasted image 20220506212042.png | 300]]
	- ![[Pasted image 20220506212118.png | 300]]
	- m是指第m个field，一共有M个field
	- h代表第h个head
	- $W_Q$/$W_K$/$W_V$都是$R^{d^{\prime} \times d}$
	- self-attention之前，原来的每个embedding都是d维，self-attention后的结果，每个embedding都是$d^{\prime}$维度

- multi-head
	- create different subspace and learn distinct feature interaction separately
	- 多头融合方式也很传统，就是普通concatenate
		- ![[Pasted image 20220506213827.png | 300]]

- 再叠加上residual connection
	- ![[Pasted image 20220506214019.png | 300]]
	- $W_{res}$是将$d\rightarrow Hd^{\prime}$ 的矩阵

- Stacking
	- 把以上multi-head self-attention + residual connection算作一层interaction layer
	- 把多个interaction layer叠加

### Output
- output
	- stack multiple interaction layer后，
	- 拼接多个field embedding，
	- 再接一层mlp
	- ![[Pasted image 20220506214620.png | 300]]