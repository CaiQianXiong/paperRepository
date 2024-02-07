---
alias: MIMN
发布时间: 2019-05-24
出品方: 阿里妈妈
价值: ⭐⭐
环节: 精排
---
关键词:: #Reco/用户行为序列 

---

- 整体评价
	- 所谓基于Memory Network或者Neural Turing Machine建立的模型，没啥可看的，毕竟不怎么实用。
	- 唯一可取之处，就是将建模user interest的部分单独剥离出来，部署成UIC (user interest center)
	- 用所谓的Neural Turing Machine来更新user interest，在我看来是完全多此一举。
		- 这里建模的是long-term user interest，是比较稳定的，天级更新就可以了。完全没必要每来一个用户行为就更新。
		- 随着用户最新行为更新的，应该是short-term user interest，DIN之类就完全能够胜任了。
		- 文章中也说了， out-sync parameter update shows little difference on model performance

- From serving system view, we decouple the most resource-consuming part of user interest modeling from the entire model by designing a separate module named UIC (User Interest Center). 
	- UIC maintains the latest interest state for each user, whose update **depends on realtime user behavior trigger event, rather than on traffic request**. 也就是，用户用了新动作，UIC就更新。
	- Hence UIC is latency free.

- 无需存储，随时更新
	- The latest memory states represent user interests and are updated to TAIR for realtime CTR prediction. 
	- 估计TAIR就是一个KV的向量数据库，query时输入一个userid，返回一个代表user当前最新兴趣的向量
	- When receiving a new user behavior event, UIC calculates and updates the user interest representation to TAIR again. 只要有了新动作，TAIR中的向量就能得到随时更新。
		- MIMN proposes to model user interests in an **incremental** way, without need to store the whole user behavior sequence
	- In this way, user behavior data is not needed to be stored (换句话说，历史不用存储了，只存当前代表最新user interest的向量). The huge volume of longtime user behaviors could be reduced.
	- 但是问题是，如果只存最新的user interest embedding，如何解决time travel的问题❓

- 整体模型
	- ![[Pasted image 20220406220309.png | 500]]
	- 看着这个模型，MIMN并非作为一个auxilary task用自己的目标，也并非pre-trained
	- 而是MIMN与main ctrl model一起训练，优化CTR prediction目标
	- 然后在部署时，将MIMN剥离出来部署在UIC，而将main ctr部分部署在RTP
	- 但是，MIMN的速度真能赶得上online learning的要求吗？