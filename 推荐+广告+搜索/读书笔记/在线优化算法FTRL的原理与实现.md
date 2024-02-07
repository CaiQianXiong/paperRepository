---
alias: FTRL
发布时间: 2020-04-13
出品方: 
价值: ⭐⭐⭐⭐
环节: 
---
关键词:: 在线学习
URL:: [在线优化算法 FTRL 的原理与实现 - massquantity - 博客园](cubox://card?id=ff808081804f5f12018064fba0a666e9)

---

# Online Learning
Online Learning的两大目标：
- 减少regret
- 提高sparsity

## Regret
- 在线学习与离线学习不同，
	- 喂入模型的样本不再是i.i.d的，而是包含有时间顺序的
	- 模型的训练也不再是一次性的，每个batch都会得到一个当时的权重$w_t$，$w_t$也是一个序列
	- 优化的不再是loss，而是距离全局最优解（existed but unknown yet）的差距，也就是regret：![[Pasted image 20220427104237.png | 200]]
		- 当regret是轮数t的次线性函数，随着t的增加，$w_t$会接近全局最优的w

## 稀疏性难点
Online Gradient Descent（一个接一个地喂样本） 在<span style="color:crimson;font-weight:bold">准确率上表现不错，即 regret 低，但在上文的另一个考量因素 sparsity 上则表现不佳</span>，即使加上了 L1 正则也很难使大量的参数变零。
- 一个原因是浮点运算很难让最后的参数出现绝对零值；
- 另一个原因是不同于批处理模式，online 场景下每次 w  的更新并不是沿着全局梯度进行下降，而是沿着某个样本的产生的梯度方向进行下降，整个寻优过程变得像是一个“随机” 查找的过程，这样 online 最优化求解即使采用 L1 正则化的方式， 也很难产生稀疏解。
正因为 OGD 存在这样的问题，FTRL 才致力于在准确率不降低的前提下提高稀疏性。


# FTRL原理
## Follow The Leader
- FTL的基本思路是：不仅要负责mimize当前样本的loss，还要负责mimize之前的loss之和
	- ![[Pasted image 20220426153643.png | 500]]
- FTRL算法的损失函数，一般也不是能够很快求解的，需要找一个代理的损失函数。
	- 代理损失函数比较容易求解，最好是有解析解  
	- 优化代理损失函数求的解，和优化原函数得到的解差距不能太大
	- [[在线优化算法FTRL的原理与实现#^w2kcu2 | OGD]]和[[在线优化算法FTRL的原理与实现#^ib8ezk | FTRL]]只不过是使用了不同的代理损失函数

## 从OGD开始
![[Pasted image 20220427105258.png| 150]]，学习率![[Pasted image 20220427105321.png | 50]]
以上公式等价于![[Pasted image 20220427105428.png | 250]]，因为一求导就能得到，![[Pasted image 20220427105513.png | 300]] ^w2kcu2

## Setup FTRL
- 首先，为了降低 regret，FTRL 用$g_{1:t}$代替 $g_t$ ，
	- $g_{1:t}$为前 1 到 t 轮损失函数的累计梯度（因为FTL就要将minize sum of prior losses），即![[Pasted image 20220427110039.png | 300]] 。
	- 由于在线学习随机性大的特点，累计梯度可避免由于某些维度样本局部抖动太大导致错误判断。这是从 FTL ( Follow the Leader) 那借鉴而来的，而 FTRL 的全称为 Follow the Regularized Leader ，从名字上看其实就是在 FTL 的基础上加上了正则化项

- 其次，保留OGD公式中的![[Pasted image 20220427110254.png | 100]]。这意味着每次更新时我们不希望新的w离之前的$w_t$太远 (这也是有时其被称为 _**FTRL-proximal**_ 的原因)，这同样是为了降低 regret，在线学习噪音大，若一次更新错得太远后面难以收回来，没法轻易“后悔”。
- 增加了L2正则

- FTRL要优化的代理损失函数 ^ib8ezk
	- ![[Pasted image 20220427111022.png | 500]]，![[Pasted image 20220427111049.png | 500]]

## 推导FTRL
- 第1步：将![[Pasted image 20220427111214.png | 100]]展开，得到![[Pasted image 20220427111308.png | 500]]

- 第2步：![[Pasted image 20220427111350.png | 500]]，得到![[Pasted image 20220427111533.png | 400]]

- 第3步：不再针对整个w向量，而是针对每一位特征的权重，![[Pasted image 20220427111702.png | 400]]



- 第4步：对[[Pasted image 20220427111702.png |以上公式]]求导
	- 由于$\lambda_1|w_i|$在$w_i=0$时不可导，代之以subgradient![[Pasted image 20220427112227.png | 200]]
	- 求导数=0，得到公式(1.18)，![[Pasted image 20220427114905.png | 300]]

- 第5步：![[Pasted image 20220427114932.png | 900]]

- 最终：综合几种情况，得到公式(1.19)![[Pasted image 20220427115052.png | 300]]

可以看到当![[Pasted image 20220427115205.png | 200]]时，参数置为零，这就是 FTRL 稀疏性的由来

## Per-Coordinate Learning Rate
- 以上公式中，![[Pasted image 20220427115355.png | 100]]，而learning rate $\eta$  还是全局为所有特征所共同采纳的。
- FTRL 采用的是 Per-Coordinate Learning Rate，即每个特征采用不同的学习率，这种方法考虑了训练样本本身在不同特征上分布的不均匀性。如果一个特征变化快，则对应的学习率也会下降得快，反之亦然。其实近年来随着深度学习的流行这种操作已经是很常见了，常用的 AdaGrad、Adam 等梯度下降的变种都蕴含着这类思想。
	- ![[Pasted image 20220427115609.png | 150]]

- [[Pasted image 20220427115052.png | 公式(1.19)]]中$\sum_{s=1}^t \sigma_s$写成![[Pasted image 20220427120054.png | 500]]
- 最终公式![[Pasted image 20220427120215.png | 500]]

# FTRL实现
根据[[Pasted image 20220427120215.png | 公式]]，从$t\rightarrow t+1$，只需要![[Pasted image 20220427121206.png | 300]]。

而$z_{t,i}$是可以累加的![[Pasted image 20220427121320.png | 300]]，我们记录![[Pasted image 20220427121601.png | 100]]

因此，实现起来的伪代码如下：
![[Pasted image 20220427121716.png | 500]]




