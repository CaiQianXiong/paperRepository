---
alias: 
发布时间: 2021-01-01
出品方: 腾讯
价值: ⭐
环节: 
---
关键词:: #Reco/辅助训练 #ML/自监督学习 #Reco/用户行为序列 

---
# U-BERT: Pre-training User Representations for Improved Recommendation 

推荐任务属于review-enhanced item recommendation。不怎么样的一篇文章，故弄玄虚👎。
* 其实main task就是拿user+review来预测对item rating，最后用的还是mse loss
* 之前以为是一个transfer learning的问题，但是看完，完全没有transfer的影子，也没有source domain/target domain的区别。只不过是在main task之外，额外增加了两个auxilary loss。

## Main Task

**单侧attention**

1. user embedding + domain embedding，![image.png | 200](assets/image-20211205143355-i07hdbx.png)
2. user的每一篇review，都经过一遍multi-layer bert，应该得到的也是一个sequence embedding
3. 因为user的每一篇都成为了一个sequence embedding，user多篇review进行row-wise concatenation（**？？？难道reviewA的第一个词与reviewB的第一个词的embedding拼起来**），![image.png | 200](assets/image-20211205143741-724x9l5.png)
4. user单侧做一个attend，拿user基本信息对review做attend，![image.png | 300](assets/image-20211205143830-ctbtcje.png)
5. item侧也做同样的事情，比如拿item id对属于这个item的多篇review，做attend

**双侧attention**

user review对所有item review做attention，item review对所有user review做attention。尽管目前还有细节没说清楚，比如review都是sequence embedding，但是我觉得并不重要，反正都是套路。

![image.png | 500](assets/image-20211205144151-ffsdu1f.png)

有一个所谓的matching kernel

![image.png | 300](assets/image-20211205144220-jaroj2p.png)

"row-wise max-pooling to fuse the matching information at all positions"，可能是沿“每个词”的维度做的max-pooling

![image.png | 200](assets/image-20211205144258-kjdgvwd.png)


最后计算，和real rating之间的mse

## 辅助目标预训练

针对每一条<user, review>样本，预测两个辅助目标

1. 和普通bert类似，也是预测masked word。但是有两点改进

    1. 不是像原来bert那样的self-attention，而是拿user embedding+domain embedding对review做target attention
    2. 要mask并预测的word，来自一个预先定义好的opinion word vocabulary
2. 每个review是带有rating的，拿<user, review>来预测rating。和main task中预测rating不同，这时不考虑item特征。