---
alias: 
发布时间: 2020-10-19
出品方: 
价值: ⭐⭐⭐⭐⭐
环节: 
---
关键词:: #Reco/新用户冷启, #Reco/新物料冷启, #ML/Bandit算法
URL::

---
# Introduction to Bandits in Recommender Systems

- [01:15](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=75.59175305722046)exploration vs. exploitation dilema

- [07:45](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=465.2508660476837) 从强化的角度来看，RS是Agent，而User是Enviroment

- [09:18](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=558.3708869256134)dilema in RS
	- ![[Pasted image 20220918111848.png |400]]

## Frequentist Bandits
### MAB基础
- [23:12](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=1392.895609)Multi-Armed Bandit Problem
	- 每个arm的reward distribution是fixed but unknown
	- 不同arm之间相互independent
	- 有固定数量的arm

- [25:58](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=1558.434777) 目标是maximize expected return
	- ![[Pasted image 20220918113145.png |400]]
	- 注意平均的方法，我们要反复进行T episode，而一个episode中要进行t trails/steps
	- 一个episode中所有step计算出一个总收益
	- 我们对T个total rewards取平均

- [31:15](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=1875.646691) 简易方法“先exploration，再greedy exploitation”，将T trails划分成
	- 头部若干trails进行explore
	- 统计出每个arm的平均收益
	- 然后就找average return最高的那个arm一直exploit
	- ![[Pasted image 20220918114207.png |400]]
	- $Q_t(a)$中的t代表在t时刻对arm 'a'的收益的估计值

- [33:41](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2021.0851009928474)![[Pasted image 20220918114528.png |500]]
	- 前边exploration的steps是一个T trails（总数）的函数
	- [34:02](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2042.1930030476838) 缺点：explore找不到真正好的arm，从而使exploitation陷入sub-optimal

### Epsilon Greedy
- [38:56](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2336.283597909401) 掷硬币来决定当前要exploit还是explore
	- ![[Pasted image 20220918120038.png |400]]

- [41:02](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2462.274784) Optimistics Initial Values
	- 促使在一开始的时候，要多explore
	- ![[Pasted image 20220918120819.png |400]]
	- 以前用的是以上公式，这个问题在于只要有一个arm开始exploit，那么其平均收益肯定就>0，从而就停止explore了
	- [41:08](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2468.601482052452)optimistic initial value的思想是: assume each arm is good until it's proven to be bad
	- [41:16](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2476.505223742508) 将$Q_0(a)$设置成>>0的数字，这样即使有其他arm开始exploit了，它们一开始exploit的reward也很难>5，所以最开始还是主要以explore为主

- [42:10](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2530.85351) Decaying Epsilong Greedy
	- ![[Pasted image 20220918121448.png |400]]

### Probability Match
- [43:13](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2593.228391) 改进的点在于，explore时是每个arm平均采样的
	- 更好的方法，应该多采样那些后验指标好的
	- 没必要浪费太多实验在那些已经被证明不好的arms上
	- ![[Pasted image 20220918121946.png |300]]

- 注意❗，**这里提出的probability matching不再是像epsilon greedy那样的两步法**
	- 不再用$\epsilon$来决定当前步骤是explore还是exploit
	- probability matching是将explore与exploit合二为一
	- 选中一个arm的概率和该arm到目前为止的average rewards有关
	- [44:57](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2697.093590957085)average rewards越高，该arm被选中的概率越高，这是exploit
	- 但是毕竟是抽样，并不一定每次都选中average rewards最高的那个，这个算exploration

- [44:07](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2647.418511)Boltzmann Exploration
	- ![[Pasted image 20220918122052.png |400]]

### UCB算法

- [46:36](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2796.818435)以上方法，在t时刻决策时，都只使用了到t时刻为止的各arm的average return
	- [46:47](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2807.536029)但是统计过程中的uncertainty都还没有使用上

- [47:09](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2829.294475)UCB: play the arm with highest potential
	- ![[Pasted image 20220918123736.png |500]]
	- [47:35](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2855.5009380715255)一个arm越是uncertain，越是需要explore它

- [49:26](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=2966.921896128746)UCB1
	- ![[Pasted image 20220918142736.png |400]]
	- N: total number of played arms by time 't'
	- [50:04](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3004.54926) 也就是说，如果不选择$a_j$，那么exploration部分，只增加分子（N$\uparrow$），而不增加分母，所以$a_j$的exploration部分反而增加，增加了$a_j$被选中的概率
	- [51:01](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3061.302953)c: 也是decaying，越到后来，就主要是exploit，而不是explore了

### 总结Frequentist方法

- [53:51](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3231.8332289370574)![[Pasted image 20220918143755.png |500]]

## Bayesian Bandits

- [55:38](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3338.833725) Frequentist观点只计算了average rewards
	- **没有利用上rewards的一些先验知识**

- [56:11](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3371.20357)![[Pasted image 20220918144619.png |500]]
	- frequentist的观点，只拿$Q_t(a)$的点估计
	- bayesian的观点，是从$p(Q_t(a))$的后验估计中再采样

- [58:20](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3500.626559) Thompson Sampling
	- [58:41](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3521.216562) 假设reward由bernouli分布产生
	- [58:50](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3530.973796765396) 假设prior distribution由beta分布产生
		- ![[Pasted image 20220918145317.png |400]]
	- [59:35](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3575.347361) 后验数据更新beta分布
		- ![[Pasted image 20220918145429.png |400]]
	- [01:00:06](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3606.085227) 通过在后验分布中采样，选取抽样最大的那个arm，作为action
		- ![[Pasted image 20220918152204.png |450]]

## Contextual Bandits

- [01:02:22](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3742.8750288426436)以user作为context，不同item（不同arm）被展示时，根据context（用户）的不同，会产生不同的reward

### LinUCB
- [01:03:26](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3806.8492202121924)LinUCB，还是在UCB的基础上，action strategy还是选择upper bound最大的那一个
	- ![[Pasted image 20220918153445.png |400]]

- 需要为每个arm都训练出一段模型参数来
	- ![[Pasted image 20220918153633.png |400]]

## Application in RS

- LinUCB for online news recommendation
	- [01:05:55](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=3955.944388) 背景
		- 新news, 新user
		- 为front page挑选文章
	- [01:08:10](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=4090.228628) 收集数据
	- [01:09:41](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=4181.8230377282025)LinUCB with hybrid Linear Models
	- ![[Pasted image 20220918154819.png |500]]

- [01:10:38](https://www.youtube.com/watch?v=rDjCfQJ_sYY&ab_channel=ACMRecSys#t=4238.188571899864)![[Pasted image 20220918171254.png |500]]