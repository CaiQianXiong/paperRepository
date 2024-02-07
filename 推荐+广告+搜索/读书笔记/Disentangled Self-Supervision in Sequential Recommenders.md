---
alias: 
发布时间: 2020-01-01
出品方: 阿里
价值: ⭐⭐
环节: 
---
关键词:: #ML/自监督学习

---
# Disentangled Self-Supervision in Sequential Recommenders

* 一般都是seq2item，就是拿之前的用户历史，预测next clicked，缺点是近视的。
* 这篇文章改成seq2seq，预测long-term future。但是seq2item --> seq2seq也是有难点的
  * 构建更长的future，肯定比a single next item要难, and inefficient。其中一个原因，很多预测是重复的，比如future sequence中的很多item都来源于user history所反映的同一个intention。
  * 即使构建了更长的future sequence，这其中所包含的用户的某些兴趣点intention，可能是由之前的用户行为历史所无法解释的

* 为了克服以上seq2seq带来的困难，文章提出两个创新
  * 为了解决[one by one predict的低效问题](siyuan://blocks/20211115182146-02u2wuf)，对比学习的方式，构建整个future sequence的表示，而不是为future sequence中的每个item单独表示
  * 为了解决[future intention可能与历史无关的问题](siyuan://blocks/20211115182146-707uu1d)，we can identify which portion of the future sequence is predictable from the earlier behaviors, then construct seq2seq training samples using only pairs of sub-sequences that involve a shared intention.

## Sequence-to-Sequence Self-Supervision

* our seq2seq training strategy asks the model to predict the representation of the future sub-sequence given the earlier sequence’s representations.
* This design avoids individually reconstructing all the behaviors in the future sequence and eases convergence of the seq2seq training process.
* The representation to be predicted effectively serves as a distilled pseudo behavior (e.g., clicking a pseudo item) in the vector space, which summarizes the main intention present in the future sequence.

### Contrastive Learning Loss

思路是与==百度的“孪生网络”非常相似，就是用户的一部分历史，应该与他自己的另一半历史，是相似的==。

* We construct the mini-batch B by **sampling** each of its element from the training set {(𝑢,𝑡) : 1 ≤ 𝑢 ≤ 𝑁, 1 ≤ 𝑡 ≤ 𝑇 𝑢 − 1} uniformly.
* Each training example (𝑢,𝑡) in the mini-batch B refers to an earlier sequence $x_{1:t}^{(u)}$ and its corresponding future sequence $x_{t+1:T_u}^{(u)}$。
	* 只不过，作者在embedding user sequence时还考虑了时间因素，所以认为$x_{1:t}^{(u)}$的向量表示，应该与reversed future sequence, $x_{T_u:t+1}^{(u)}$的向量表示，相似。
* 另外与常规作法不同的是，为了反映多兴趣，作者所使用的“user sequence --> vector”的映射函数，不是只返回一个向量，而是返回K个向量。
	* 为此，也将loss修正为，在第k个兴趣的隐向量空间内，用户前一部分历史的向量表示，与后一段历史的向量表示，应该相似。

![image.png | 500](assets/image-20211116205356-gt507e6.png)


这里还加了一个**小trick，尽快我认为是一个中看不中用的trick**，这里的user "u", t (前后历史的时间分裂点), k（兴趣）的组合太多了。作者认为，“if an earlier sequence x 1:𝑡 involves intention under latent (𝑢) category 𝑘 = 1 and 𝑘 = 3 while the future sequence x 𝑇 𝑢 :𝑡+1 involves intention under category 𝑘 = 1 and 𝑘 = 2, then we should use only L 𝑠2𝑠 (𝜽,𝑢,𝑡,𝑘 = 1), but not use 𝑘 = 2 and 𝑘 = 3, "。但是，**作者没有采用规则来限制要训练的样本，因此一点计算量也节省不下来**。而是采用了在loss上加mask这样稀奇古怪的方式： 这里的$\tau$代表top n小的loss, n是超参。也就是说we keep only the top 𝜆-percent of the sequenceto-sequence training samples that are considered to be of high confidence by the model.

![image.png | 400](assets/image-20211116205909-kuyx707.png)


### Total Loss

之前常用的是seq2item task，也就是利用之前的历史，预测next clicked。这里引入seq2seq, complement, not replace, the traditional seq2item loss. In other words, we minimize **both the seq2item loss and the seq2seq loss** when processing each mini-batch. 这和contrastive learning的常规作法也是符合的，**contrastive learning一般都是作为辅助训练的角色而存在的**。![image.png | 300](assets/image-20211116210647-8v9alex.png)

$L_{s2i}$是主训练任务

* $x_{1:t}^{(u)}$代表user "u"在t时间之前的历史序列
* $\phi_{\theta}^{k}$j是将用户历史压缩成一个兴趣向量。这里采用了多兴趣，所以输入一个用户历史，返回的不是一个向量，而是K个向量
* $h_{t+1}^u$j是用户在下一时刻t+1点击的item的embedding，来自一个D维的embedding matrix

![image.png | 400](assets/image-20211116203232-z95vzj0.png)


## Disentangled Sequence Encoding

* Our second core idea is to design a sequence encoder that can infer and disentangle the latent intentions reflected by a given sequence of behaviors.
* The disentangled encoder outputs multiple representations of a given sequence of behaviors, where each representation focuses on a distinct sub-sequence of the given sequence.
* Each of the multiple representations characterizes the user’s intention related to a different latent category.
* We can then construct seq2seq training samples using only pairs of sub-sequences whose intentions are relevant in that they involve the same latent category

让输入的一个sequence能够返回**multiple intention，最直观的想法就是使用multi-head transformer**，但是好像效果不是很好，**实验证明multi-head与single-head相比，并未带来显著提升**。

作者所采用的disentangle的方法是：

1. 只采用single-head transformer来处理sequence，将sequence中的每个clicked item映射成![image.png | 200](assets/image-20211117113958-3aobw7b.png)
2. **定义K个兴趣向量，成为trainable variable的一部分**
3. 计算第i个时刻的sequence item embedding到每个intention的权重（how likely the primary intention at position 𝑖 is related with the 𝑘 th latent category）

    ![image.png | 300](assets/image-20211117114428-nw0xrvi.png)
4. 再计算how likely the primary intention at position 𝑖 is important for predicting the user’s future intentions. 看着公式很复杂，<span style="color:orange;font-weight:bold">其实就相当于拿sequence的最后一个元素为query，对序列中所有元素做attention</span>。

    ![image.png | 300](assets/image-20211117114642-rx0h51j.png)
5. 最后用户序列$x_{1:t}^{(u)}$在第K个intention上的向量表示，就是序列中每个元素的向量表示，考虑了对第k个兴趣的相似度，再考虑了其对未来预测能力，之后的加权和

![image.png | 300](assets/image-20211117114830-62yhimt.png)


另外，常见的disentangle需要有regularization强迫每个用户学到的K个intention之间互不相同。考虑到[seq2seq contrastive loss中的分母](siyuan://blocks/20211116204511-k0myurs)已经考虑了其他K-1种兴趣，这里就不加了。