---
alias: ESCM2
发布时间: 2022-04-03
出品方: 阿里蚂蚁
价值: ⭐⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/Debias #Reco/多任务学习 #Reco/全链路优化 #ML/因果推断 
URL:: [zotero](zotero://open-pdf/0_9WYST65B/1)

---

好久没有写关于单篇文章的解读了。但是今天这一篇《ESCM2: Entire Space Counterfactual Multi-Task Model for Post-Click Conversion Rate Estimation》必须值得单独写一篇解读。原因有二：
- 文章非常有现实意义。这篇2022年的阿里新文，是关于“因果推断”、“反事实学习”在推荐模型中的应用，用于减轻建模一条转化链条的前后环节之间存在的bias，解决的确是推荐算法的一个痛点。
- 文章写得，真的不好懂。文章中“怎么做”、“为什么这样做”，还有一堆数学证明杂揉在一起，找人摸不清文章的网络和重点。顺着原文来读，思路极容易被打断。而且表达同样意思的公式，可能写了两遍，用了两种符号。有的重要结论，只用文字轻描淡写提了一句，而没有写成公式引起读者的注意。唉，可能作者也没办法，论文这东西就是八股文。

Causal Inference和Counterfactual Learning，和我们平常用到的Machine Learning理论上还是有所不同的，我也是一知半解，所以在本文中将我的疑问也列举出来，欢迎大家一起来讨论。

# 要解决的问题

阿里的这篇文章，应对的是CVR建模问题。在电商环境下，用户转化需要“先曝光->再点击->最后购买”，三个环节之间存在先后顺序之分，也蕴含着因果关系。

业务要求建模三个目标：
- CTR：从曝光到点击的概率
- CVR：从点击到购买的概率
- CTCVR：从曝光到购买的概率

## 中间环节单独建模的意义
这里需要指出的是，单独对中间环节建模（i.e., 这里的CVR）的意义：
- 理论上，我们只关心购买。如果CTCVR预测的是准确的，那么其实没必要单独建模CVR。以最终CTCVR排序即可。
- 但是，以上两个理论假设都不成立
	- 首先，CTCVR预测得肯定有误差
	- 其次，业务要求，使我们需要关心中间环节。比如广告场景下，不同的转化环节，不同的计费模式。
- 因此，这里必须把cvr, ctr, ctcvr都预测出来，再把三个分数加权成一个综合得分，再排序
	- ![[Pasted image 20220517071436.png |300]]
- 而在其他一些场景下，比如内容推荐场景下，就直接建模ctr和“完播”就行了，没再建模“点击后完播”的概率。当然，未来可以借鉴电商广告场景，把中间环节拎出来单独建模。

## 建模中间环节的Naive方法
写出loss来很容易
![[Pasted image 20220517072103.png | 400]]
- $r_{u,i}$是user 'u'对item 'i'是否转化的ground truth label
- $\hat{r}_{u,i}$ 是CVR model的预测结果
问题就在于$r_{u,i}$只在click=1的样本上（click space, 下图中的O）才能拿得到
![[Pasted image 20220517072932.png |400]]

因此，一种naive的方法，训练的时候，只拿click=1样本（click space, O）来训练CVR模型。存在两个问题：
- 问题1：训练空间与预测空间不匹配
	- 训练的时候，是拿所有click=1的样本来训练，<user, item>的匹配性已经非常高了。
	- 而预测的时候，只是经过召回、粗排的粗筛，<user, item>的匹配度还非常低。
	- 两个空间不匹配，训练时缺失的那部分样本，是由系统性的sample selection bias造成的，Missing Not At Random（MNAR）
- 问题2：只用click=1的样本，训练数据比较少

## ESMM方法的缺点

为了解决Naive方式以上两个问题，阿里提出了ESMM
![[Pasted image 20220517074359.png |450]]
- ESMM实际上是回避了单独建模CVR的问题
- 两个loss，一个ctr loss，另一个ctcvr loss，都是在整个“曝光样本空间”D上建模
- CVR= CTCVR / CTR

优点：CTR和CTCVR都是在全体曝光样本上建模的，因此两者相除的结果CVR也算是在全体曝光样本上建模的。因此，CVR模型训练与预测同分布（实际上训练是在曝光样本空间，预测是在召回样本空间，不过这点差异，大家都约定俗成，不予追究了），算是解决了sample selection bias的问题。

缺点：
- 一是Inherent Estimation Bias (IEB)，认为ESMM的CVR得分存在系统性的bias，普遍偏高
- 二是Potential Independence Priority (PIP)
	- 我理解成，算是另外一种反向sample selection bias
	- 因为CTR, CTCVR都是在全体曝光空间D上建模的，两者的商CVR也是建模在全体曝光空间上的，对于消除training/inference之间的gap当然是好事。
	- 但是CVR的含义毕竟是“点击后再转化”的概率，click->convert之间的因果关系就建模不出来了。

但是CVR=CTCVR/CTR这个公式是肯定不会有错的，那么以上两点偏差是从何而来的？我觉得是：**要让这个公式成立，需要理想化的条件，是现实无法满足的**。比如，训练CTCVR也是认为，如果最终click & convert=0，就代表着用户不喜欢。但是有可能是因为CTR模型预估得不准，把item排在不好的位置，让用户失去了click的机会，而实际上p(convert|click)应该是很高的。这也就是论文中所谓“**反事实**”的含义。

# Entire Space Counterfactual Multi-task Modeling (ESCM2)

ESCM采用了一种与ESMM完全不同的思路：
- CVR遵循其名称的本意，还是只拿click=1的数据来训练
- 但是采用一些因果推断、反事实学习的方法，在训练过程中就减轻Sample Selection Bais（SSB）、Missing Not At Random（MNAR）带来的负面影响。
- 最终得到的模型，尽管是只用click=1样本训练出来的，但是在全样本空间预测时，也能得到"unbiased"的结果。（unbiased是论文里宣称的效果，还用数学证明了。我觉得那是理论结论，现实中做不到，至多是降低bias）

整个ESCM都是围绕着如下公式开展的
![[Pasted image 20220517094213.png |400]]
- $L_{CTR}$在全量曝光样本D上建模ctr loss
- $L_{CTCVR}$是在click=1的样本空间O上建模“最终转化loss”
	- 用ESMM一样，用ctcvr=ctr\*cvr为预测值，用最终是否转化为label，计算binary cross-entropy loss。
	- <span style="color:red;font-weight:bold">论文中说“the global risk minimizer optimizes the risk of CTCVR estimation over 𝒟”</span>，~~我觉得是论文在这里写错了~~
	- 首先论文中图4“Global Risk Minimizer”下边就有“O”(代表click space)的字样
	- 论文里还说“The amount of training data for CTR task is significantly greater than that of CVR and CTCVR tasks by 1-2 order of magnitudes.”，所以ctcvr也应该是在click space中建模的
	- 还有建模ctcvr时，用到的ctcvr prediction是由predict ctr \* predict cvr相乘得到的。而predict cvr肯定是只由click=1的样本上训练得到的（这是本文区别ESMM的最大区别），所以ctcvr也应该是只在click=1的样本上建模。
- $L_{CVR}$到底在哪个空间建模，要区分具体是用IPS实现，还是DR实现。

## 用IPS实现CVR建模
The inverse propensity score (IPS) estimator weights each CVR error term $\delta_{u,i}$ with  $\frac{1}{q_{u,i}}$, the inverse of the **propensity score (CTR in our case)**, to <span style="color:orange;font-weight:bold">align the error distribution over the click space with that over the exposure space</span>. <span style="color:orange;font-weight:bold">在click空间中训练模型，修正后还是能够运用于整个空间，我觉得这才是这个方法的精髓</span>。
![[Pasted image 20220517094640.png |300]]
或者写得更具体一点
![[Pasted image 20220517094734.png |400]]
- $o_{u,i}$是用户是否点击的label。因此，尽管公式的计算范围是D（全体曝光样本），但是实际上是只拿点击样本($o_{u,i}$=1)建立loss
- $r_{u,i}$是user 'u'对item 'i'是否转化的ground truth label。毕竟只有click=1的样本上的$r_{u,i}$才是可信的
- $\hat{r}_{u,i}$ 是CVR tower的预测结果
- $\hat{q}_{u,i}=\hat{o}_{u,i}$ 都是ctr tower的预测值
- 然后，我们要建模的$L_{CVR}=R_{IPS}$。这也是这篇文章难懂的原因之一，同一个意思在不同地方用不同的符号来表达，也没有一个地方写清楚这个两个符号表达同样含义。

不要看原论文中的图4，在IPS实现中是压根没有中间那个计算Imputation Error的塔的。更准确的图是《LARGE-SCALE CAUSAL APPROACHES TO DEBIASING POST-CLICK CONVERSION RATE ESTIMATION WITH MULTI-TASK LEARNING》中的图4。注意右下角的“O”，说明CVR只在click=1的空间上建模。
![[Pasted image 20220517101543.png |400]]

对于以上公式的解读，我看到的解释都是：
- 如果ctr高，还转化了，是理所应当的，权重应该低一些
- 如果ctr低，最终反而转化了，是Counterfactual的，正好是对Sample Selection Bias的有力反击，所以权重应该大一些。

但是我始终有一个疑问，ctr高低来决定样本权重，难道不应该视样本的正负而定吗？
- ctr高，还转化了，没啥新鲜的，权重理应低一些
- ~~ctr高，但是未转化，这个非常非常counterfactual呀，权重理应更高呀！！！~~<span style="color:orange;font-weight:bold">其实ctr高，而cvr低，一点也不counter-factual</span>，算是正常情况。

另外，论文里说用这种IPS建模，尽管是在click=1的样本训练，但是在全体样本空间预测时得到的cvr也是unbiasd。我不太相信，我们是在debias，不是在unbias。cvr都是在点击样本上训练得到的，尽管以inverse ctr反向加权，但是点击样本中的ctr基本上都不低吧。

## 用DR实现CVR建模
IPS认为，只要$\hat{q}_{u,i}=\hat{o}_{u,i}$ 预测得准，在O(click space)上训练$L_{IPS}$得到的模型，就能在整个D(impression space)无偏预测。但是问题在于预测ctr总会有偏差。因此引入DR，DR introduces imputed error $\hat\delta_{u,i}$  to model the **prediction error for all events in D**, and corrects the error deviation $\hat{e}_{u,i}=\delta_{u,i}-\hat{\delta}_{u,i}$ for **clicked events**.

认为IPS中用predict ctr $\hat{o}_{u,i}$ 当propensity score，毕竟是预测值，偏差会高，因此采用如下Doubly Robust来计算$L_{CVR}$。
![[Pasted image 20220517102738.png |300]]
写得更具体一些
![[Pasted image 20220517102850.png |400]]
- 不同于IPS，DR是在整个曝光样本空间上建模
- $\hat{\delta}_{u,i}$  是<span style="color:orange;font-weight:bold">imputed error，全体曝光空间上的“推测cvr loss”</span>。
	- 因为只有click=1的样本才有是否转化的真实label，所以只有在click=1的样本上计算出来的cvr loss $\delta_{u,i}$ 才是真实可信的
	- 在除了click之外的剩余的曝光空间上，我们无法计算出真实的$\delta_{u,i}$。所以，我们<span style="color:orange;font-weight:bold">单独建立一个模型来模拟在click=0上的cvr loss</span>，就是这里的imputed error $\hat{\delta}_{u,i}$ 。再次发挥了<span style="color:cyan;font-weight:bold">DNN中的“无中生有、空手套白狼”的本事</span>。
- $\hat{e}_{u,i}=\delta_{u,i}-\hat{\delta}_{u,i}$
	- $\hat{\delta}_{u,i}$ 就是中间imputation tower的输出，是建模在全体曝光样本空间上的
	- $\delta_{u,i}$ 在click space上，根据predicted cvr和convert ground truth计算得到的真实cvr loss
	- 因此$\hat{e}_{u,i}$也**只针对click events进行修正**

Doubly Robust的意义，我看到的解释都是：起到一个双保险的作用，只要propensity score $\hat{q}_{u,i}=\hat{o}_{u,i}$，和imputed error  $\hat{\delta}_{u,i}$ ，有一个预测得准，就能起到debias的作用。$\hat{q}_{u,i}=\hat{o}_{u,i}$的准确性，是由ctr tower决定的，是被“是否点击”的ground truth label限制住的。但是谁来保证$\hat{\delta}_{u,i}$ 的准确性？

### 点击样本
如果是click样本，即$o_{u,i}=1$，再忽略分母$\hat{q}_{u,i}=\hat{o}_{u,i}$的影响，则$R_{DR}=\hat{\delta}_{u,i} + \hat{e}_{u,i}=\delta_{u,i}$，就是点击样本上的真实cvr loss。那么$R_{DR}$ 的准确性就由converted ground truth label给约束住了。而且还加上如下mse loss，减少真实cvr loss与imputed cvr loss之间的距离$\hat{e}_{u,i}=\delta_{u,i}-\hat{\delta}_{u,i}$。
![[Pasted image 20220517114401.png | 300]]

总之，对于click样本，采用如下公式计算cvr loss $L_{CVR}$，准确性是有保障的。
![[Pasted image 20220517114929.png]]

### 曝光未点击样本
但是对于click=0的曝光样本呢？整个cvr loss $L_{CVR}$就只剩下了imputed cvr loss $\hat{\delta}_{u,i}$。由于我们拿不到click=0的曝光样本上的“是否转化”的ground truth，$\hat{\delta}_{u,i}$本来就是由模型“无中生有”得到的，岂不是就没有了约束？比如这个模型可以让其中所有W、b都变成0，那么$\hat{\delta}_{u,i}$不也变成0了吗？

<span style="color:crimson;font-weight:bold">在我看来，还就真的没有对</span>$\hat{\delta}_{u,i}$<span style="color:crimson;font-weight:bold">的直接约束，对它的约束都是间接的</span>。因为$\hat{\delta}_{u,i}$对于click=0和click=1的样本，共享一套模型参数。而当click=1时的“是否转化”的ground truth label，把模型参数约束住了，那就不会出现W、b都变成0的情况，$\hat{\delta}_{u,i}$在click=0的样本上也不会无下限的小下去。或许这就是在论文里，称$L_{CVR}$这一项为regularizer的原因，毕竟对于regularizer来说，约束就是它自己。
``` ad-tip
title: 阿里没有上线DR方法

用NN来直接预测一个error，太奇葩。压根就没上线。
```

### 整体结构
论文中说，“ESCM2-DR: It **augments** ESCM2-IPS with an imputation tower”。所以，我觉得这里的$L_{CVR}$应该如下：（唉，这么重要的地方，论文作者为什么不给出一个直接明白的公式，非要让我们自己去猜，真是的）
![[Pasted image 20220517121137.png |500]]

![[Pasted image 20220517111449.png |500]]
- 左边的塔，在全体曝光样本上训练，得到ctr
	- 预测出来的ctr，喂入Empirical Risk Minimizer，计算$L_{CTR}$ 
	- ctr的倒数喂入Counterfactual Risk Minimizer，计算$L_{CVR}$
- 右边的塔，在点击样本上训练，得到cvr。
	- 由cvr，计算出点击样本上的真实cvr loss "$\delta_{u,i}$ "，喂入Counterfactual Risk Minimizer，参与计算$L_{CVR}$
	- ctr tower输出的predict ctr，和这里输出的predict cvr相乘，得到点击空间上的predict ctcvr，喂入Global Risk Minimizer计算$L_{CTCVR}$
- 中间的塔，在全量曝光样本上，预测输出imputed cvr loss $\hat{\delta}_{u,i}$ 
	- 中间的Counterfactual Risk Minimizer集合全体曝光上样本上的ctr和$\hat{\delta}_{u,i}$ ，还有点击样本上的真实cvr loss $\delta_{u,i}$，计算得到$L_{CVR}$

# 实现
其实阿里上线的只有IPS方法。
To construct ESCM2-IPS: 
- Construct a ESMM model with the CTR loss $L_{CTR}$ and CTCVR loss $L_{CTCVR}$ in the **entire space** in Equation 5. 
	- <span style="color:orange;font-weight:bold">CTCVR也在所有曝光样本上建模，是阿里团队的经验之谈</span>。当然也有好处，就是<span style="color:orange;font-weight:bold">未来预测时，可以直接使用，否则在click space上建模ctcvr在预测时也有sample selection bias问题</span>。
	- ![[Pasted image 20220602181138.png |400]]
	- 注意这里$\hat r_{u,i}$是CVR塔的预测值，如果CTCVR是在整个impression空间上建模的，也就需要$\hat r_{u,i}$也要能给impression but not click的数据上打分。这好像也与"IPS只在clicked数据上建模"的原理并不矛盾，**前代给在整个曝光上空间上进行，但是通过对loss加上mask，回代只在点击空间上进行**。
- Calculate the propensity score. In the CVR estimation task, a shortcut is to use the CTR estimate directly. 
	- small value of propensity scores (CTR estimates) leads to *large estimation variance and numerical error*. 
	- we set a threshold (e.g. 0.1 in our setting) to **clip the ctr estimate** to a reasonable range
- Calculate the CVR estimation error $L_{CVR}$ with 𝛿 and propensity score in the **clicked space**, following Equation 15. 
	- ![[Pasted image 20220602181450.png |400]]
	- The gradient from $L_{CVR}$ to propensity score (CTR estimate) ought to be truncated, i.e., **stop-gradient**.
- Calculate the learning objective of ESCM2 following Equation 27, and optimize it with stochastic gradient methods. 
	- ![[Pasted image 20220602181550.png |400]]
	- make every effort to **ensure an accurate CTR estimate**, as it has a significant impact on both the CVR and CTCVR estimates. 
	- For example, set the weight $\lambda_c$ to a small value (0.1-1 in our cases), and **enlarge the volume of CTR tower**.

# 讨论

## IPS就是后悔药
我的那篇文章的题目叫《推荐算法遇到了后悔药》，可惜行文比较匆忙，文中并没有对“后悔药是什么”进行详细阐述。

我说的这个“后悔药”就是IPS修正

我觉得ESCM2整篇文章的思想，是和ESMM完全不同的，ESMM是将ctr,ctcvr都拉到同一个D空间建模，从而推出两者的商也是在D空间上的，从而说cvr是无偏的。（当然你们文章中证明了esmm的这种假设是一个误解）

而我理解ESCM2的延续的不是ESMM的路线，而是naive方法
1. 我用naive方法，就只在click space上建立cvr模型
2. 但是听说naive方法建立起来的模型，有sample selection bias，我后悔了
3. 这时IPS就是后悔药，它的说明书上说，只要用inverse ctr修正一下，即使在click space上训练出来的模型，也能用于在整个D空间上预测。只要ctr预测准确，在D上的预测就是无偏的。

## CTCVR应该在哪里建模
如果我以上理解是正确的，那么我还是觉得CTCVR应该在O上建模，而不应该在D上建模。

首先，我声明，我这个没有实验根据，只是凭着我对这个算法的一些理解，而形成的我觉得make sense的观点。

第一，像你所说“需要和ESMM对齐”，我觉得完全没有必要呀。ESCM2和ESMM是完全两条不同的技术路线。ESCM2走的就是“click space训练并修正，在impression space上预测”的这条路线，为了保持路线的“纯洁性”，彻底和esmm划清界限，ctcvr也应该只在O（click space）上建模。

如果你们最后的排序公式要同时用到ctr, cvr, ctcvr, ctr是D空间上训练的，cvr是O上训练但是修正后的，你是担心ctcvr是在O上训练导致ctcvr有sample selection bias问题吗？那理论上，也应该是对ctcvr也拿IPS修正一下。

第二，第一条理由，我坚持的是理论上的纯洁性，接下来，我想说说数据上的纯洁性。如果按照你在appendix A implementation details上的说法，IPS只是双塔模型，CVR这个塔也要是喂入整个D空间上的训练数据的
- cvr这个塔必须用entire space D来训练，这样才能有cvr来参与在D上建模的CTCVR Loss
- 但是CVR loss本身只针对click space上来计算并优化

~~但是CVR这个塔中的参数，会被impression but not click的大量数据来带偏吧。从CTCVR对一些impression but not click样本上的loss，这些loss是不准确的，会污染到cvr tower的参数~~
并不矛盾，CVR塔<span style="color:orange;font-weight:bold">前代给在整个曝光上空间上进行，回代只在点击空间上进行</span>。

所以，从理论纯洁性与数据纯洁性两方面，我都觉得cvr和ctcvr只在click space上建模才make sense，不过可能需要在O上训练ctcvr也拿inverse ctr修正一番才能在inference时用得上。

# 结尾
本文和ESMM面向的问题是一样的，解决“转化链路多环节之间的bias”，但是与ESMM采用了完全不同的思路
- ESMM不直接优化CVR，而是在全体曝光样本上优化cvr loss和ctcvr loss，再用cvr=ctcvr/ctr来得到cvr，从而使训练和预测CVR的所使用的数据同分布，解决sample selection bias
- ESCM回归cvr的本质，只拿click数据上训练、优化cvr loss。但是通过Counterfactual方法debias，使只用click数据得到的模型，能够在面对全量数据预测时，产生所谓“unbias”的结果。

另外，论文中还有几点：
- 底层是一个MMOE
- “we implemented ESMM and ESCM2 with our internal C++ based deep learning framework, where **ESCM2 was built with the IPS regularizer** for its competitive offline performance and training efficiency.”。所以最终上线的是哪一种？$L_{CVR}$只使用了IPS，而没有使用DR？

