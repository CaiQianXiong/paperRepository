---
alias: DropoutNet
发布时间: 2022-03-08
出品方: 阿里
价值: ⭐⭐
环节: 
---
关键词:: #Reco/新物料冷启 #Reco/新用户冷启 
URL:: [冷启动推荐模型DropoutNet深度解析与改进 - 知乎专栏·「智能推荐」](https://zhuanlan.zhihu.com/p/474671484?utm_source=wechat_session&utm_medium=social&utm_oi=43686972882944&utm_campaign=shareopn)

---

> [!INFO] 整体评价
> 价值不大，和原来的DropoutNet关系已经不大了
> 说白了，就是普通的双塔模型，只不过在接入层时加入了Dropout，将一些后验指标drop掉
> **但是，如果不像经典Dropout那样，用带后验指标的模型进行蒸馏，**
> **干脆只用新用户/物料特征建立一个模型不就完了吗**


# 冷启动一般思路
- “泛快迁少”原则
	- 泛：泛化至人群，或有相似attribute的物料
	- 快：bandits算法之类的
	- 迁：transfer learning
	- 少：少样本学习，比如meta-learning这种在reco不靠谱的东西

# 传统的DropoutNet模型
DropoutNetwork分两步走
## 第1步：预训练
第一步是预训练，肯定是在老用户&老物料的样本上，用了更多的后验数据，得到
- 每个用户的兴趣向量U和
- 每个物料的向量V
## 第2步：DropoutNet
![[Pasted image 20220427095516.png | 500]]
DropoutNet借鉴了Denoising Autoencoder的思想，
- 即训练模型接受被corrupted的输入来重建原始的输入，
- **模型是要使得在输入被corrupted的情况下学习到的用户向量与物品向量的相关性分，尽可能接近输入在没有被corrupted的情况下学习到的用户向量与物品向量的相关性分**。
- 这里的corrupted就是指对[[冷启动推荐模型DropoutNet深度解析与改进#第1步：预训练 | “真实”user兴趣向量和“真实”item向量]]的dropout

训练时
- 正常训练，不dropout，![[Pasted image 20220427095944.png | 400]]
- 针对新用户冷启，dropout $U_u$, ![[Pasted image 20220427100111.png | 400]]
- 针对新物料冷启，dropout $V_v$, ![[Pasted image 20220427100205.png | 400]]
![[Pasted image 20220427100236.png | 500]]

# “挂羊头”的改进DropoutNet
> [!hint] 挂羊头，卖狗肉
> - 说的好听，是为了解决传统DropoutNet中需要预训练步骤的不便
> - 其实就是普通双塔模型，加上一些dropout，再考虑hard negative/easy negative

![[Pasted image 20220427100821.png | 600]]

## 样本策略 #Reco/样本策略 
**不再需要提供用于和物品的embedding向量作为监督信号**，取而代之，我们使用用户与物品的交互行为作为监督信号。
- 正样本：如果用户点击了某物品，则该用户与物品构成正样本；
- **Hard Negative**：那些展现给用户但却没有被点击的商品构建成负样本。通过损失函数的设计，可以使模型学习到正样本的用户与物品向量表示的相似度尽可能高，负样本的用户与物品的向量表示的相似度尽可能低
- Sampling Easy Negative: 使用了in-batch sampling

### In-batch Sampling
另一种更讨巧的实现方式是从当前mini-batch中采样。因为训练数据需要全局混洗（shuffle）之后再用来训练模型，**这样每个mini-batch中的样本集都是随机采样得到的，当我们从mini-batch中采样负样本时，理论上相当于是对全局样本进行了负采样**。这种方式实现起来比较简单，本文就是采用这种在线采样的方法。

具体地，训练过程中，
- 用户、物品特征执行完网络的forward阶段后，得到了用户embedding、物品embedding，
- 接下来我们通过对物品embedding矩阵（batch_size * embedding_size）做一个按行偏移操作（row-wise roll），<span style="color:orange;font-weight:bold">把矩阵的行（对应物品embedding）整体向下移动 N 行，被移出矩阵的N行再重现插入到矩阵最前面的N行，相当于是在一个循环队列中依次往一个方向移动了N步</span> 💡。这样就得到了一个负样本的用户物品pair
- 重复上述操作M次就得到了M个负样本pair

[in-batch negative sampling的代码实现](https://github.com/alibaba/EasyRec/blob/master/easy_rec/python/loss/softmax_loss_with_negative_mining.py)

## Loss设计
应该是基于in-batch sampling制造出的postive pair, hard/easy negative pair，可以在其中套用不同的loss函数

### Support Vector Guided Softmax Loss
我们参考了论文《Support Vector Guided Softmax Loss for Face Recognition》的思路，在实现softmax损失函数过程中引入了最大间隔和支持向量的做法，通过在训练过程中使用“**削弱正确**”和“**放大错误**”的方式，<span style="color:orange;font-weight:bold">强迫模型在训练时挑战更加困难的任务，使得模型更高鲁棒</span>，在预测阶段可以轻松做出正确判断。

[support_vector_guided_softmax_loss](https://github.com/alibaba/EasyRec/blob/master/easy_rec/python/loss/softmax_loss_with_negative_mining.py)

### Pairwise L2R Loss
[pairwise_loss.py](https://github.com/alibaba/EasyRec/blob/master/easy_rec/python/loss/pairwise_loss.py)
