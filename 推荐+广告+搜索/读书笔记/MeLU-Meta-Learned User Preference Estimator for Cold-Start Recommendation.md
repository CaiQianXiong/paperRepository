---
alias: MeLU
发布时间: 2019-07-31
出品方: 
价值: ⭐
---

环节:: 
关键词:: #Reco/新用户冷启 

---
# MeLU: Meta-Learned User Preference Estimator for Cold-Start Recommendation
#价值/👎

## 严重置疑

我很不理解这种方式，**难道是每个用户算是一个task**，然后新用户来了之后，只需要少量数据，就能让“模型”在这个新用户的新任务上学习好？

> "We regard ==each task as estimating a user’s== preferences"
>
> "The MAML-based recommender system accounts for the fact that ==different users have different optimal parameters.=="
>


但是问题是，==meta-learning结束之后，究竟是把哪些参数学习到一个比较好的状态，从而能够快速适应这个新用户呢？==dense还是embedding?

* 如果是让dense适应新用户，我们不可能为每个用户单独训练+部署一套模型的，dense是共享的。其实除了dense，其余一些能共享embedding，也必须是共享的，不可能只偏向一个用户。
* 如果是让embedding适应新用户，新用户的问题就在于new user id embedding在训练的时候都是missing的，它们不可能在训练数据中出现的，meta-learning也是无法训练的呀


另外，还提出之前给新用户展现的都是popular items，这里提出meta-learning能有一个副产品，能够得到对新用户更具吸引力的**evidence candidates**。但是还是有疑问，这里所谓的new user是在训练数据中已经出现过的吗？既然已经出现过，还算新用户吗？❓❓❓


## 作法

"MAML"那一套

* **local updates** parameter θ to $θ_i$ by gradient $∇_θ L_i (f_θ ) $for each tasks i = 1, · · · ,T, 

  * where T is the number of sampled tasks
  * and $L_i (f_θ )$ denotes the **training loss** on task i with parameter θ.
* After local updates, for all sampled tasks, the algorithm globally updates parameter θ based on $L_i^′ (f_{θ_i} )$,

  * $L_i^′ (f_{θ_i} )$ is the **test loss** on task i with parameter $θ_i$

![image.png | 500](assets/image-20220110150203-csam2jo.png)

![image.png | 500](assets/image-20220110150303-5uebr1r.png)

算法特点是local updates时只更新dnn层（说是为了ensure the stability of learning，没有业务上的道理，只是trick），只有global updates才更新embedding+dnn全部参数。


**预测的时候更是离谱**，When recommending items to a user, MeLU conducts **local updates** （**难道dense哪些东西是每个用户一份？不同用户之间隔离不共享？？**）based on the user’s item-consumption history (也就是不适用于纯新用户) and calculates the **preferences for all items that the user has not rated** （拿dnn做全量召回？？）. After that, the model recommends top favored items to the user.
