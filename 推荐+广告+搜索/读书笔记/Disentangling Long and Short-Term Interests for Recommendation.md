---
alias: 
发布时间: 2022-02-26
出品方: 快手
价值: ⭐⭐
环节: 
---
关键词:: #Reco/用户行为序列 

---
整篇文章的价值不大：
1. 所谓的contrastive learning，完全就是一个噱头。正宗constrastive learning的精确在于data augmentation，也就是怎么基于原样本变形出相似样本和不同样本。这篇文章完全没有样本变形这个过程。
2. 文章提出的问题是确实存在的，也就是要将stable long-term user interest和dynamic short-term user interest区分开。但是使用的方式是完全多此一举、画蛇添足的。使用了非常复杂的模型来模拟一个非常简单的所谓proxy，那这个简单的proxy就是模型拟合的上限，既然如此，为什么不直接使用这个proxy。
3. 看实验数据集，淘宝的数据集的用户行为序列平均长度是39.85，快手数据集的用户行为序列平均长度是245。说实在的，这点历史都谈不上long-term，对于长历史的刻画远远不如SIM那么硬核、那么贴近实际。
4. 一看作者就知道，不过是学校为了发文章而灌水的水文。

另外，永远不要和我说，文章的确取得了收益。
- 如果是在标准数据集上取得收益，就是打kaggle比赛那样，也未必有实战价值。最典型的事件就是，对于netlix prize的winner solution，netflix只付了奖金，但是并未在自己的系统中加以实现。一个静态的小数据集的效果，完全不能反映线上动态、海量数据集上的实际情况。
- 如果是在自己公司的线上环境取得收益，恭喜你，公司应该给你升职加薪，但是未必有学术价值。有时改个bug都能取得收益。更何况，大家环境不同，论文中所提出的方法或许只适用于贵公司，换个环境，就有可能“水土不服”。
- 以上两点，其实也凸显出我们做算法的尴尬处境。每年发那么多论文，但是我们很难评判每篇论文的价值。
- 我们作为局外人看论文，其实真的不必太在意实验指标提升了多少。只看原理是否make sense，只有原理make sense，我们才会复现的必要，否则就是瞎耽误功夫。

# 提出问题
- 首先，文章提出的问题是确实存在的，也就是要将stable long-term user interest和dynamic short-term user interest区分开。比如，我个人长期、稳定的兴趣爱好是经济、金融、历史、军事，但是也不妨碍我时不时地点击一些八卦、娱乐新闻。为此，作者独立建模用户的长期兴趣与短期兴趣，的确非常有必要。
- 两类兴趣的重要性也应该是个性化的。

# 模型方法
## 拆分建模
把整个建模过程拆解成：
1. 建模用户长期兴趣
2. 建模用户短期动态兴趣
3. 结合长短期兴趣进行CTR预估
![[Pasted image 20220403102452.png | 300]]
![[Pasted image 20220403102603.png | 300]]
拆解本身并不稀奇，另外，作者的点在于：
1. 增加了辅助任务，使$U_l$和$U_s$有区分度
2. $U_l$和$U_s$对$Y^{t}$ 的贡献度是个性化的

### 用户长期兴趣
用户的长期兴趣$U_l=f_1(U)$。
- <span style="color:blue;font-weight:bold">U是所有用户特征，包括user_id还有user history</span>
- f1是一个attention模型。
	- 拿user_id的embedding当query![[Pasted image 20220403104019.png | 100]]，
	- 去和user history sequence ($x_j^u$是user u的历史中的第j个action item)做attention。
		- ![[Pasted image 20220403104111.png | 200]]
		- ![[Pasted image 20220403104135.png | 150]]
	- 非常奇怪呀，一般的作法是拿target item当query，去和user history sequence做attention，目的是为了从整个用户历史中突出与target item相关部分历史。
	- 但是拿user_id去query user history，给自己交互过的每个历史item不同权重，意义在哪里呢？看看用户更喜欢user history中的那一个？用“观看完成度”不就行了？或者用户参与越高（不仅点击了，还点赞了；不仅点赞了，还购买了......），权重就越大好。


### 用户短期兴趣
用户短期兴趣$U_s^t=f_2(U_s^{t-1},V_{t-1},Y_{t-1},U)$
- $U_s^t$是用户在t时刻的短期兴趣，它和$U_s^{t-1}$相关，看来作者是准备使用RNN模型
- $V_{t-1},Y_{t-1}$是t-1时刻接触的item，和动作（e.g., 是否点击）
- <span style="color:blue;font-weight:bold">U前边说了，是user profile，包括user_id和整个user history</span>
- query本身就很奇怪，是拿GRU最后输出当成query![[Pasted image 20220403104611.png | 200]]
- 对谁做attention呢？还是自己，也就是说，attention双方都来自user history。

- 其实就是先把sequence喂入RNN
- RNN的输出当成keys/values
- RNN的最后一步的输出当成query
- 在如此的query/keys/values上做一次attention
![[Pasted image 20220403104845.png | 200]] ![[Pasted image 20220403104907.png | 100]]

### CTR预估
没见论文中说，但是应该包括target item对user history 'U'的attention

## Disentangle
贡献点之一就是要让$U_l$和$U_s$有区分度。
- 但是“区分度”这个目标本身都有问题，比如历史较短的用户，他的长期历史本来就应该和短期历史相同。文中也说，如果兴趣历史太短，就没必要disentagle了。
- 也不会有label来区分长期兴趣、短期兴趣。

所使用的方法就是所谓的contrastive learning。实际上不是，因为constrative learning的核心是data augmentation。
- 第1步，为长短期兴趣构建锚点。二者形式相同，只不过long term anchor用的是全部历史的mean pooling，而short term anchor用的是近期历史（多少算近期也是超参）的mean pooling
![[Pasted image 20220403111706.png | 300]]
- 第2步，构建Learning To Rank的关系对，即用户的长（短）期兴趣应该与长（短）期锚点更接近，而与短（长）期锚点相距较远。
![[Pasted image 20220403143102.png | 200]]
- 第3步，如何描述这种关系对，LTR的loss都可以用上
![[Pasted image 20220403143352.png | 300]]
- 第4步，当成一个辅助loss，和main loss一同优化

但是问题是，❓❓❓❓❓❓❓❓❓
- <span style="color:red;font-weight:bold">为什么不直接用long term anchor（全部历史的pooling）来描述长期历史？</span>
- <span style="color:red;font-weight:bold">为什么不直接用short term anchor（近期历史的pooling）来描述短期历史？</span>
- 你或许要说，只是简单pooling比较简单，信息损失大？那么既然如此，用来做锚点就合适吗？


## 不同兴趣的个性化权重
权重是由user history和target item来决定
![[Pasted image 20220403144110.png | 300]]
- 把用户历史用GRU压缩成$h_t^u$
- 将所有信息，$h_t^u$，target item "$E(x_{t+1}^u)$"，长期兴趣$u_l^t$，短期兴趣$u_s^t$，都当成输入来决定长短期兴趣的权重

# 实验
这篇文章与SIM的思路完全不同。对于长期历史，<span style="color:darkred;font-weight:bold">考虑线上infer的时间成本，必须有一个过程是将long history给简化下来，否则train/infer都来不及</span>。这篇文章完全没有这个过程。
![[Pasted image 20220403144542.png | 500]]
看了实验数据才发现，数据中所谓的长历史不过是在250左右，远远达不到像SIM那么长的量级。

<span style="color:red;background-color:black;font-weight:bold">这篇文章与其说是在建模长短期历史，不如说是都在“短期”历史中建模“稳定兴趣”和“近期动态兴趣”。</span>
