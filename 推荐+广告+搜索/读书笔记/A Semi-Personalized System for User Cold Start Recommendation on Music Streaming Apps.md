---
alias:
发布时间: 2021-01-01
出品方: Deezer
价值: ⭐⭐⭐⭐
---

环节:: 
关键词:: #Reco/新用户冷启 

---
# A Semi-Personalized System for User Cold Start Recommendation on Music Streaming Apps

Deezer（一家法国在线音乐公司） #价值/💡

基础模型是一个collaborative filtering算法，就是一堆warm user和一堆music track之间的矩阵分解，得到user embedding and track embedding。

addressing the following problem: 
* given an existing latent model for collaborative filtering learning an embedding space from a set of **warm users**,
* how can we effectively **include new cold users into this same space**, **by the end of their registration day(意味着已经有过一些行为)** on Deezer?

## Related Work

* Various approaches aim at clustering warm users, and subsequently assign cold users to existing clusters by leveraging these metadata (e.g., demographic information (such as the age or country of the user))
* app弹窗问卷调查新用户兴趣，
* use bandit algorithms to assign cold users to warm user segments
* 还利用社交找相似用户

## 步骤1：先得到warm user embedding

* 就是常规作法，有了warm user embedding
  * 可以通过ALS，获得warm user embedding
  * 也可以通过先通过word2vec学习出item embedding，再average获得user embedding
  * 注意，**这里获得user embedding的时候，纯粹是把id-->embedding硬学出来，没使用任何side information**。

* 通过k-means聚成1000类
	* 每类用中心embedding来代替
	* 预先计算好各cluster中最受欢迎的item
	* 注意，<span style="color:orange;font-weight:bold">聚类不是必需的，和第2步的预测没关系。预测的并非“某个用户群体”的embedding</span>。用户聚类只不过是为了u2u2i时，给cold user分一个user segmentation才用得上。


## 步骤2：预测cold user的embedding

![image.png | 700](assets/image-20220119163450-jetjmhw.png)

训练一个DNN能够预测cold user embedding

* "data retrieved from user-item interactions, **limiting to interactions at registration day** (if any)."。**也就是只用新用户的数据来训练的模型**💡。
* 模型左边比较普通，就是pooled user action list那一类的。
  * 除了pooled interacted item embedding，还有album, artist等粒度，不过也属于常规操作
* 右边的比较特别，图中的country embedding和age embedding不是拿用户的country/age去embedding矩阵去查来的向量
  * ==country embedding是同一个国家所有warm user embedding的平均==
  * ==age embedding是同年龄所有warm user embedding的平均==
* 中间就是ReLU+BatchNormalization，没啥新鲜的
* 最后一层拿预测出来的向量，与第一步得到的warm user embedding做MSE


怎么理解这个模型，
* 这个模型的样本必须是warm user，因为只有他们才在第一步中得到了embedding
* 但是所使用的特征，必须是这些warm user在注册第一天的数据
* 注意特征里还用到了同国家/同年龄的warm user embedding的平均，作为group embedding
* 所以这个模型是一个“**用户成熟**”模型💡💡💡，**我们知道用户所在群体的embedding，再加上第1天的一些行为，预测用户成熟后的embedding是什么样？**

## 步骤3：U2U2I预测

有了预测出来的cold user embedding，当然可以直接在向量空间里进行KNN，提到要推荐的item。

但是效果不好，所以只能u2u2i，“each cold user is assigned to the warm cluster whose centroid is the closest w.r.t. the predicted embedding vector of this cold user. We subsequently recommend the pre-computed most popular tracks among warm users from the cluster.”