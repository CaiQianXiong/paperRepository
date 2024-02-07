---
alias: 
发布时间: 2022-04-19
出品方: 
价值: ⭐⭐⭐⭐⭐
环节: 召回
---
关键词:: #Reco/样本策略 
URL:: [b站](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0)

---

# 【FunRec】Softmax负采样优化 #价值/💎

## 语言模型
### 统计语言模型
[04:01](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=241.911572) 统计语言模型
![[Pasted image 20220604094657.png |400]]

[06:20](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=380.073706) 通过统计得出概率的问题
![[Pasted image 20220604095024.png |300]]

[07:02](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=422.074143) 马尔科夫假设：不必考虑前边出现的所有词，只考虑前边出现的k个词
![[Pasted image 20220604095136.png |400]]

[08:58](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=538.237894) N-Gram模型
![[Pasted image 20220604095259.png |400]]

### 神经网络语言模型
![[Pasted image 20220604100220.png |400]]
网络的输入`x`是之前的n个词先embedding，再拼接一起。

logit的公式
![[Pasted image 20220604100427.png |400]]
y的长度就是整个vocabulary中的词数|V|

### Word2Vec

[14:36](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=876.644269)Skip-Gram
![[Pasted image 20220604100742.png |400]]

[17:11](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=1031.608269)
![[Pasted image 20220604101535.png |400]]


[17:31](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=1051.014644) 构造样本的代码
![[Pasted image 20220604101829.png |200]]


[18:04](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=1084.755082) Word2Vec TF模型代码
![[Pasted image 20220604101905.png |350]]

## Softmax
[23:04](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=1384.981908)![[Pasted image 20220604102823.png |400]]

## NCE
[23:16](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=1396.060354)![[Pasted image 20220604104223.png |400]]


[28:10](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=1690.280428)接下来就是计算$p(y|c,w)$，需要将两个概率分布$p_{train}$和噪声分布Q，合成一个分布
![[Pasted image 20220604104501.png |400]]
- $p_{train}(w|c)$就是我们要建立的模型，是根据c变化的条件概率
- Q(w)是一个固定的分布，也c无关

[29:59](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=1799.47752)![[Pasted image 20220604105031.png |400]]
有非常好的理论保证，**随着采样K的增加，NCE导致趋向于softmax的梯度**

## NEG

- [34:22](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=2062.74512)**NEG是NCE的近似估计，并不保证趋向于Softmax**
![[Pasted image 20220604110256.png |1000]]

[34:58](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=2098.851057)其中的假设是Q(w)是均匀分布，而且k是整个词表大小。其实就是把噪声分布去掉了。

[41:30](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=2490.884911)NEG追求的是学习到高质量的word embedding，而非降低语言模型的困惑度perplexity。用符合语法的一批句子当测试集，测试集中的句子的概率越大，则perplexity越低。

## Sampled Softmax

### 修正后的概率公式

[51:30](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=3090.111372) Youtube建模成多分类问题

[52:40](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=3160.366061)通过重要性采样(MC)近似Softmax计算
![[Pasted image 20220604112652.png |600]]

[55:02](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=3302.673561)![[Pasted image 20220604113036.png |600]]
在求梯度的公式里，是对所有negative term的expectation。既然是expectation，就可以用采样来近似了。
[01:13:13](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=4393.323618) 推导出分母是对梯度求期望
![[Pasted image 20220604115918.png |700]]

根据[[Softmax负采样精讲#^e31g3a |重要性采样的MC方法]]
![[Pasted image 20220604142634.png |400]]


[01:15:42](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=4542.456432)但是这时的$P(y_i)$ 仍然要计算一个大分母
![[Pasted image 20220604143205.png |400]]
[01:16:28](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=4588.844684)$R({y_i})=\frac{1}{M}$ 看成一个均匀分布？总之，把$\sum$ 里面变成一个expectation，才好再次使用importance sampling


[01:17:08](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=4628.70431)再次运用重要性采样
![[Pasted image 20220604143545.png |400]]


[01:19:11](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=4751.83637) 将![[Pasted image 20220604144015.png |100]]代入![[Pasted image 20220604144059.png |150]]
![[Pasted image 20220604144123.png |400]]


- [01:21:26](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=4886.948491) 对比
- 原来分母的梯度是![[Pasted image 20220604144448.png |300]]，要计算所有item。
- 现在只需要累加采样出来的那一部分数据![[Pasted image 20220604144551.png |400]]，并且需要进行修正


- [01:21:50](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=4910.687553) [01:32:03](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=5523.699654)<span style="color:orange;font-weight:bold">所以最终用以下修正公式表示每个item的概率</span>
	- ![[Pasted image 20220604144946.png |400]]


[01:24:54](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=5094.021113) 回顾推导过程


### TF代码

看的是TF源码中关于`sampled_softmax_loss`的代码

- [01:38:35](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=5915.973655) 修正logits的部分
	- ![[Pasted image 20220604151404.png |400]]

- [01:40:46](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6046.782906)[01:53:10](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6790.980245)`_compute_sampled_logits`函数
	- [01:40:58](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6058.625083)`candidate_sampling_ops.log_uniform_candidate_sampler`

- [01:51:08](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6668.372611) `nce_loss`

- [01:52:25](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6745.909502)`sampled_softmax_loss`
	- [01:53:21](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6801.545984)weights和bias，相当于每个item embedding和bias
	- [01:53:37](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6817.969995)labels是正样本，`[batch_size,1]`
	- inputs: user embedding
	- num_classes: 整个词典的大小
	- [01:58:57](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=7137.121486) 所有负样本是共享的


- [02:06:49](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=7609.562727)`log_uniform_candidate_sampler`
	- 相当于内部自动进行`log_uniform`采样，所以调用的时候不用先采样再传入
	- [02:07:30](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=7650.815284)各item id的编号，必须按照item出现的次数，从大到小排序
	- ![[Pasted image 20220604160005.png |500]]

- [02:08:50](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=7730.924728)有可能自定义采样频率
	- ![[Pasted image 20220604160254.png |600]]


### 背景知识：重要性采样（Importance Sampling）
[58:26](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=3506.044089)Monte-Carlo求积分
![[Pasted image 20220604113536.png |400]]

[01:01:03](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=3663.432077) 重要性采样
- Monte-Carlo好是好，但是需要采样数量多才准确
- 能否有方法，**能够在采样数量一定的情况下，增加准确度，减少方差？其实就是重要区域多采，次要地区少采**
- ![[Pasted image 20220604114038.png |500]]

- [01:05:47](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=3947.858065) 基于重要性采样求期望 ^e31g3a
	- ![[Pasted image 20220604115008.png |1000]] ^vwozfz

## 负采样 vs. 重要性采样

- [01:43:40](https://www.bilibili.com/video/BV1KY4y1a7VL?spm_id_from=333.999.0.0#t=6220.489642) 对比
- ![[Pasted image 20220604153108.png | 500]]


