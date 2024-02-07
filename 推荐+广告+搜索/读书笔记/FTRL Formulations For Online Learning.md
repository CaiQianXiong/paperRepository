---
alias: FTRL
发布时间: 2019--2-28
出品方: Booking.com
价值: ⭐⭐
---
环节:: 
关键词:: 在线学习

> [!INFO] 整体评价
> 价值不大，并没有最终推导出google那篇论文的公式
> 只是说明了FTRL是一个算法框架，能够推导出包括普通OGD算法

# [FTRL Formulations For Online Learning](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s)


- [02:02](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=122.52139616021728) What's out-of-core learning
	- used in scenario where large dataset doesn't fit in memory
		- ![[Pasted image 20220426144909.png | 500]]
	- Parallel learning和Out-of-core learning都适用于大数据场景
		- ![[Pasted image 20220426145047.png | 500]]
	- [09:29](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=569.81148) Out-of-core Learning
		- ![[Pasted image 20220426145711.png | 500]]
		- 还是属于离线场景，数据经过shuffle，所以喂入模型的样本还是i.i.d的

- [12:51](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=771.069501) Online Learning
	- different from batch learning or out-of-core learning
		- <span style="color:orange;font-weight:bold">进入模型的数据是沿时间顺序的，样本不再是i.i.d</span> 💡
			- ![[Pasted image 20220426150238.png | 500]]
		- [14:07](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=847.3043469570847)优化的是与全局最优参数$\theta_*$(unknown yet)之间的**regret** ^lvndwj
			- ![[Pasted image 20220426150609.png | 500]]
	- [16:16](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=976.029918)simplest approach: OGD
		- ![[Pasted image 20220426151957.png | 300]]
	- [17:58](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=1078.7916861978874) when to use online learning?
		- ![[Pasted image 20220426152250.png | 500]]

- [19:40](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=1180.200904) 用Online Learning方式离线训练-Naive解法
	- naive的方法：用最后一轮训练得到的$\theta_t$当成全局解
	- ![[Pasted image 20220426152930.png | 500]]
	- 缺点：“当成全局最优解”是不成立的，单个样本too much variance

- [22:35](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=1355.4499140166893) Follow the regularized leader (FTRL)
	- Idea：本轮训练，不仅要负责mimize当前样本的loss，还要负责mimize之前的loss之和 🌟💡
		- ![[Pasted image 20220426153643.png | 500]]
	- 而在实际工作中，不可能真的将所有之前的loss都加起来进行优化，所以要代之以agent loss

- [26:17](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=1577.975069) 基于FTRL推导出OGD
	- OGD在FTRL框架中所使用的agent loss![[Pasted image 20220426154255.png | 500]]
		- 优化的是[[FTRL Formulations For Online Learning#^lvndwj | regret]]
		- <span style="color:orange;font-weight:bold">OGD采用的agent loss，理论来源于凸函数的upper bound</span>
			- 凸函数示意![[Pasted image 20220426154454.png | 300]]
	- 由于最优参数$\theta_*$ 未知但是固定，因此可以从优化过程中忽略![[Pasted image 20220427045536.png | 500]]
	- 再求导并=0，从而得到OGD的表达式![[Pasted image 20220427045742.png | 500]]

- [32:23](https://www.youtube.com/watch?v=kWldlahI5L8&t=1955s#t=1943.1179484531929) 总结
	- ![[Pasted image 20220427045942.png | 500]]



