---
excalidraw-plugin: parsed
alias: PEPNet
发布时间: 2023-02-05
出品方: 快手
价值: ⭐⭐⭐⭐⭐
环节: 精排
---
关键词:: #Reco/多任务学习 , #Reco/多场景推荐 
URL::
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
PEPNet ^9Wv7NoJ8

Gate Neural Unit ^MwS3GjZI

EPNet ^yFytjg5v

根据domain specific特征生成的输入特征的权重
根据所处不同domain
增强一些特征，抑制另一些特征 ^7jrhtEQA

所谓的PPNet ^2p4xDnu7

结构和道理很简单
关键是谁来做裁判
生成哪些元素的权重 ^oeW6Lar2

对场景有强指示性的特征
domain-id或一些统计数据 ^Pdtce2Dg

说白了，就是不信任DNN
生怕在传输过程中将user id/item id这种最个性化的的信息忘掉
所以在每层都把这些信息重新补充进去 ^i1ihWsQz

user id, item id, author id
这些embedding的拼接 ^KrkoiHCe

Oep就是经过domain调制过的
底层输入 ^o3OWNYdC

用 ^xvYZNY7r

作为Gate的输入 ^FkaNTYhi

我还能理解，但是把Oep也接入，就有点太卷了 ^9rCIAGyo

stop gradient ^sQzSqj38

说明作者丝毫不信任DNN
恨不能把所有信息都直接接入最后一层 ^zIQ1za22

缺点是没有信息压缩，接入层大了，模型的参数也多了 ^W15N4quA

这个画法有误导性
每条输入的样本肯定只来自于一个domain
因此每次output也是只针对一个domain
图中好像会同时为多个domain生成output ^wEwzeju9

DNN的每层都用超级个性化的信息
增强或抑制一下 ^pfuoXvrl

和作者的异同 ^JkCFDMUz

相同点，我和作者都认为这种结构就是为了防止信息丢失
所以在每层都必须用超级个性化的prior information来增强 ^IvFKcDHh

不同点，我看不出这种增强和multi-task有什么关系
这种损失是由于DNN造成的，不是由于multi-task之间的数据不均衡造成的
换句话说，我不认为这种结构对于multi-task有什么帮助 ^bkmYbQXu

工程难点全在海量feature的embedding上 ^q3tuSdZf

用这么细粒度的特征
提升个性化同时
也增加了存储量与训练量 ^SMA8bbN8

需要优化的特征准入与逐出政策 ^rhhkX00L

比如根据id的更新频率来决定这些id在PS中的去留 ^J1FDR1wD

针对embedding与DNN也需要完全不同的训练策略 ^saSsxLOW

这里是指用不同的SGD与步长 ^IZEnQUVO

其实embedding与DNN在分布式训练中本来的策略就不相同 ^JPO0o1gt

embedding可以纯异步+PS模式
DNN权重需要同步+AllReduce模式 ^76WCjtUb

总结一下 ^8vnk0bay

模型有效果是因为使用了最最个性化的特征
比如各种id embedding ^bqt5tj0h

但是为了让这些id embedding发挥使用
才发明这样的类似LHUC结构 ^ptqtqstX

而不是相反，如果只是模仿了结构，但是喂入这种Gate Network当裁判的不是最最个性化的id embedding
就本末倒置、南辕北辙了 ^jgy7XXzU


# Embedded files
e3f797ca8e91551430e1486a1181b52eb88e2a77: $$O_{prior}$$
ae856eecfec83a08826fa839e8606df8f7779640: [[Pasted Image 20230516180801_459.png]]
8c4d416bbf55735bd62440f8e45c644b3a27fc01: [[Pasted Image 20230516182232_219.png]]
37d270e7dc1f76249bca9c7cbb5dcebcb3cfd0c4: [[Pasted Image 20230516182802_742.png]]
1b7b864322dcfeff5f19e9ee233850da1868ad5e: [[Pasted Image 20230516183304_926.png]]
5fb7e9abe73bf84fe136865bf1258b08b41ebf7d: [[Pasted Image 20230516183420_973.png]]
7f182f3926622d936d6caf46f124c95ebf99ffa5: [[Pasted Image 20230516200117_104.png]]
7986d9f48ede044be9ecf9b8e0455199ce0963dc: [[Pasted Image 20230516200421_922.png]]
9bee7ee34f66a316e5e1d076b4f5a44318512bef: [[Pasted Image 20230516200804_279.png]]

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.1",
	"elements": [
		{
			"type": "image",
			"version": 127,
			"versionNonce": 1560510150,
			"isDeleted": false,
			"id": "DO1SwxOjY-SRCzgKAyRjm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 424.1834124552463,
			"y": 47.8197853564219,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 357.92410714285717,
			"height": 79.70268777614139,
			"seed": 943786522,
			"groupIds": [
				"jNCrVSbtjE_6UaKDo8rJy"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "7f182f3926622d936d6caf46f124c95ebf99ffa5",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 151,
			"versionNonce": 989266125,
			"isDeleted": false,
			"id": "pfuoXvrl",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 478.9819615623892,
			"y": 129.33385706113484,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 243.2639923095703,
			"height": 38.4,
			"seed": 318348762,
			"groupIds": [
				"jNCrVSbtjE_6UaKDo8rJy"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133447,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "DNN的每层都用超级个性化的信息\n增强或抑制一下",
			"rawText": "DNN的每层都用超级个性化的信息\n增强或抑制一下",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "DNN的每层都用超级个性化的信息\n增强或抑制一下",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "text",
			"version": 146,
			"versionNonce": 1418722947,
			"isDeleted": false,
			"id": "9Wv7NoJ8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -486.12890625,
			"y": -265.1515625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 98.41993713378906,
			"height": 33.6,
			"seed": 382863898,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133448,
			"link": null,
			"locked": false,
			"fontSize": 28,
			"fontFamily": 4,
			"text": "PEPNet",
			"rawText": "PEPNet",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "PEPNet",
			"lineHeight": 1.2,
			"baseline": 26
		},
		{
			"type": "image",
			"version": 107,
			"versionNonce": 1271862618,
			"isDeleted": false,
			"id": "osCDa-gcs4_Hj89QxALWc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -520.4068708933041,
			"y": -223.59375,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 794.7434292866083,
			"height": 500,
			"seed": 255166470,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "ae856eecfec83a08826fa839e8606df8f7779640",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 112,
			"versionNonce": 1817904454,
			"isDeleted": false,
			"id": "hHOTOxbWXZ7spNP8MNrOe",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -514.9921875,
			"y": -87.6875,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fa5252",
			"width": 186.67578125,
			"height": 137.84375,
			"seed": 1393453594,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "fS4OUQfwetxVuQEkktARL",
					"type": "arrow"
				}
			],
			"updated": 1684239577041,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 349,
			"versionNonce": 1589074458,
			"isDeleted": false,
			"id": "2PI-2o01qEQuT8nP4PZFP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": -493.970703125,
			"y": 91.3515625,
			"strokeColor": "#364fc7",
			"backgroundColor": "#4c6ef5",
			"width": 743.88671875,
			"height": 184.36718750000003,
			"seed": 1366804422,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "Yo8ub9fLKrY2X8qVw-F8a",
					"type": "arrow"
				}
			],
			"updated": 1684239577041,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 224,
			"versionNonce": 244739206,
			"isDeleted": false,
			"id": "fS4OUQfwetxVuQEkktARL",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -515.9921875,
			"y": -35.527134175254346,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 171.4039005122031,
			"height": 73.31279993612394,
			"seed": 1417318278,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "hHOTOxbWXZ7spNP8MNrOe",
				"focus": 0.5250651443459473,
				"gap": 1
			},
			"endBinding": {
				"elementId": "MwS3GjZI",
				"focus": -0.5567360336461895,
				"gap": 7.971875000000011
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-171.4039005122031,
					73.31279993612394
				]
			]
		},
		{
			"type": "rectangle",
			"version": 549,
			"versionNonce": 498665178,
			"isDeleted": false,
			"id": "j3xr0wpEzqqSF_EaxvhK5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 30,
			"angle": 0,
			"x": 101.033203125,
			"y": -153.8828125,
			"strokeColor": "#364fc7",
			"backgroundColor": "#4c6ef5",
			"width": 146.64453125,
			"height": 178.75781250000003,
			"seed": 739171782,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 105,
			"versionNonce": 2069323565,
			"isDeleted": false,
			"id": "yFytjg5v",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -93.921875,
			"y": 355.1875,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 57.93995666503906,
			"height": 24,
			"seed": 1034624410,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "Yo8ub9fLKrY2X8qVw-F8a",
					"type": "arrow"
				}
			],
			"updated": 1684373133448,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "EPNet",
			"rawText": "EPNet",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "EPNet",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 248,
			"versionNonce": 291969946,
			"isDeleted": false,
			"id": "Yo8ub9fLKrY2X8qVw-F8a",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -55.91255913931788,
			"y": 280.53515625,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 3.3538067310000272,
			"height": 70.0078125,
			"seed": 572761222,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "2PI-2o01qEQuT8nP4PZFP",
				"focus": -0.1643963857033306,
				"gap": 4.81640625
			},
			"endBinding": {
				"elementId": "yFytjg5v",
				"focus": 0.44645747175920947,
				"gap": 4.64453125
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					3.3538067310000272,
					70.0078125
				]
			]
		},
		{
			"type": "text",
			"version": 62,
			"versionNonce": 263403043,
			"isDeleted": false,
			"id": "2p4xDnu7",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 484.5,
			"y": -219.12503396739132,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 117.53994750976562,
			"height": 24,
			"seed": 1197758278,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133448,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "所谓的PPNet",
			"rawText": "所谓的PPNet",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "所谓的PPNet",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 119,
			"versionNonce": 1625092493,
			"isDeleted": false,
			"id": "MwS3GjZI",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -753.887058423913,
			"y": 45.7575407608696,
			"strokeColor": "#000000",
			"backgroundColor": "#fa5252",
			"width": 159.79991149902344,
			"height": 24,
			"seed": 650474394,
			"groupIds": [
				"JzlEpGNjMH_gycU5u1NL5"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "fS4OUQfwetxVuQEkktARL",
					"type": "arrow"
				}
			],
			"updated": 1684373133449,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "Gate Neural Unit",
			"rawText": "Gate Neural Unit",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Gate Neural Unit",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 169,
			"versionNonce": 1557035459,
			"isDeleted": false,
			"id": "oeW6Lar2",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -746.862753826631,
			"y": 74.12102581522024,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 144,
			"height": 57.599999999999994,
			"seed": 144171910,
			"groupIds": [
				"JzlEpGNjMH_gycU5u1NL5"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "6l_FVtQlwMEYNGg4iysjK",
					"type": "arrow"
				}
			],
			"updated": 1684373133449,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "结构和道理很简单\n关键是谁来做裁判\n生成哪些元素的权重",
			"rawText": "结构和道理很简单\n关键是谁来做裁判\n生成哪些元素的权重",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "结构和道理很简单\n关键是谁来做裁判\n生成哪些元素的权重",
			"lineHeight": 1.2,
			"baseline": 54
		},
		{
			"type": "image",
			"version": 105,
			"versionNonce": 1362394394,
			"isDeleted": false,
			"id": "tdA5pBQP3vA7z6sHbZqKw",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -190.7139766527156,
			"y": 398.07822690217614,
			"strokeColor": "transparent",
			"backgroundColor": "#4c6ef5",
			"width": 259,
			"height": 45,
			"seed": 1877504922,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "8c4d416bbf55735bd62440f8e45c644b3a27fc01",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 83,
			"versionNonce": 801721125,
			"isDeleted": false,
			"id": "5jO5KMW19PLERlyZdEp5T",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 60,
			"angle": 0,
			"x": -17.525117957063458,
			"y": 398.2820312500022,
			"strokeColor": "transparent",
			"backgroundColor": "#4c6ef5",
			"width": 77.61209239130437,
			"height": 41.79347826086956,
			"seed": 1244610522,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "z0xmEmiXBqXfcc2hdW6pc",
					"type": "arrow"
				},
				{
					"id": "fosXhBxk67hFTilroLMPp",
					"type": "arrow"
				}
			],
			"updated": 1684306287614,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 121,
			"versionNonce": 2025928685,
			"isDeleted": false,
			"id": "Pdtce2Dg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 75.84444726032791,
			"y": 513.0850203804368,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 185.0719451904297,
			"height": 38.4,
			"seed": 797133978,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "z0xmEmiXBqXfcc2hdW6pc",
					"type": "arrow"
				}
			],
			"updated": 1684373133450,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "对场景有强指示性的特征\ndomain-id或一些统计数据",
			"rawText": "对场景有强指示性的特征\ndomain-id或一些统计数据",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "对场景有强指示性的特征\ndomain-id或一些统计数据",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 168,
			"versionNonce": 1008982214,
			"isDeleted": false,
			"id": "z0xmEmiXBqXfcc2hdW6pc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 34.0803652344175,
			"y": 443.5265964673935,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 71.50636485271848,
			"height": 63.20991847826082,
			"seed": 263347078,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "5jO5KMW19PLERlyZdEp5T",
				"focus": 0.23556196728316234,
				"gap": 3.4510869565217774
			},
			"endBinding": {
				"elementId": "Pdtce2Dg",
				"focus": -0.2966310711325657,
				"gap": 6.348505434782595
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					71.50636485271848,
					63.20991847826082
				]
			]
		},
		{
			"type": "rectangle",
			"version": 331,
			"versionNonce": 1798799002,
			"isDeleted": false,
			"id": "Dmei61vX1uehigKqSmD4D",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 60,
			"angle": 0,
			"x": -70.7129576309764,
			"y": 391.9844769021761,
			"strokeColor": "transparent",
			"backgroundColor": "#12b886",
			"width": 42.99932065217399,
			"height": 51.647418478260825,
			"seed": 174666438,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "6l_FVtQlwMEYNGg4iysjK",
					"type": "arrow"
				}
			],
			"updated": 1684239577041,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 869,
			"versionNonce": 620285958,
			"isDeleted": false,
			"id": "6l_FVtQlwMEYNGg4iysjK",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -75.35164758157947,
			"y": 450.0611696158752,
			"strokeColor": "#000000",
			"backgroundColor": "#12b886",
			"width": 622.6897747233108,
			"height": 312.81854053978583,
			"seed": 1812670938,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577041,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Dmei61vX1uehigKqSmD4D",
				"focus": -1.2472178089775103,
				"gap": 7.927989130435066
			},
			"endBinding": {
				"elementId": "oeW6Lar2",
				"focus": 0.3641028797974302,
				"gap": 5.521603260869142
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-587.8392312450501,
					-39.27574162674239
				],
				[
					-622.6897747233108,
					-312.81854053978583
				]
			]
		},
		{
			"type": "rectangle",
			"version": 494,
			"versionNonce": 1606458202,
			"isDeleted": false,
			"id": "SeQH_xGVxalrgzq43CaaX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 60,
			"angle": 0,
			"x": -186.31757719619418,
			"y": 394.6016644021762,
			"strokeColor": "transparent",
			"backgroundColor": "#fa5252",
			"width": 86.61005434782612,
			"height": 51.93953804347832,
			"seed": 587828186,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "We-eBjOs6Gj23xyP_1sAZ",
					"type": "arrow"
				}
			],
			"updated": 1684239577041,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 416,
			"versionNonce": 1856823110,
			"isDeleted": false,
			"id": "We-eBjOs6Gj23xyP_1sAZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -140.91160889992028,
			"y": 451.4409986413067,
			"strokeColor": "#000000",
			"backgroundColor": "#fa5252",
			"width": 3.683621588543815,
			"height": 54.91508152173907,
			"seed": 795113178,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "SeQH_xGVxalrgzq43CaaX",
				"focus": -0.092605981977685,
				"gap": 4.899796195652215
			},
			"endBinding": {
				"elementId": "7jrhtEQA",
				"focus": 0.16985102054039944,
				"gap": 10.589605978258533
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-3.683621588543815,
					54.91508152173907
				]
			]
		},
		{
			"type": "text",
			"version": 394,
			"versionNonce": 1493191011,
			"isDeleted": false,
			"id": "7jrhtEQA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -347.001698369565,
			"y": 516.9456861413043,
			"strokeColor": "#000000",
			"backgroundColor": "#4c6ef5",
			"width": 340.95989990234375,
			"height": 57.599999999999994,
			"seed": 1912751750,
			"groupIds": [
				"cuG_u5fzSbbcpsvAMrk5N"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "We-eBjOs6Gj23xyP_1sAZ",
					"type": "arrow"
				}
			],
			"updated": 1684373133450,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "根据domain specific特征生成的输入特征的权重\n根据所处不同domain\n增强一些特征，抑制另一些特征",
			"rawText": "根据domain specific特征生成的输入特征的权重\n根据所处不同domain\n增强一些特征，抑制另一些特征",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "根据domain specific特征生成的输入特征的权重\n根据所处不同domain\n增强一些特征，抑制另一些特征",
			"lineHeight": 1.2,
			"baseline": 54
		},
		{
			"type": "image",
			"version": 65,
			"versionNonce": 1570571910,
			"isDeleted": false,
			"id": "QwejacBgn4j5zf6I4Gopc",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -265.8647918701073,
			"y": 578.9029551630458,
			"strokeColor": "transparent",
			"backgroundColor": "#fa5252",
			"width": 246,
			"height": 46,
			"seed": 446027674,
			"groupIds": [
				"cuG_u5fzSbbcpsvAMrk5N"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "37d270e7dc1f76249bca9c7cbb5dcebcb3cfd0c4",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 86,
			"versionNonce": 1829614157,
			"isDeleted": false,
			"id": "i1ihWsQz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 410.79349617337436,
			"y": -192.55356657608542,
			"strokeColor": "#d9480f",
			"backgroundColor": "#fd7e14",
			"width": 444.4958801269531,
			"height": 57.599999999999994,
			"seed": 2047618202,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133451,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "说白了，就是不信任DNN\n生怕在传输过程中将user id/item id这种最个性化的的信息忘掉\n所以在每层都把这些信息重新补充进去",
			"rawText": "说白了，就是不信任DNN\n生怕在传输过程中将user id/item id这种最个性化的的信息忘掉\n所以在每层都把这些信息重新补充进去",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "说白了，就是不信任DNN\n生怕在传输过程中将user id/item id这种最个性化的的信息忘掉\n所以在每层都把这些信息重新补充进去",
			"lineHeight": 1.2,
			"baseline": 54
		},
		{
			"type": "image",
			"version": 283,
			"versionNonce": 579225030,
			"isDeleted": false,
			"id": "FEwVGGpe7PuCmkfZPX120",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 370.1005613907658,
			"y": -183.19894701086804,
			"strokeColor": "transparent",
			"backgroundColor": "#fd7e14",
			"width": 33.885869565217256,
			"height": 33.885869565217256,
			"seed": 951207814,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "1b7b864322dcfeff5f19e9ee233850da1868ad5e",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 58,
			"versionNonce": 1681809818,
			"isDeleted": false,
			"id": "p5vJSFSRReaHF113G2k7x",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 415.6433603038093,
			"y": -125.16090353260711,
			"strokeColor": "transparent",
			"backgroundColor": "#fd7e14",
			"width": 396,
			"height": 83,
			"seed": 1940587738,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "5fb7e9abe73bf84fe136865bf1258b08b41ebf7d",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 570,
			"versionNonce": 1317379334,
			"isDeleted": false,
			"id": "thvIqJDyzMLXZKQkB1zQ0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 60,
			"angle": 0,
			"x": 522.2557923690264,
			"y": -128.91056385869447,
			"strokeColor": "transparent",
			"backgroundColor": "#fd7e14",
			"width": 271.06997282608705,
			"height": 45.49252717391307,
			"seed": 1689393222,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "ZnZIssV4I4uA_lm-IRy6y",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 120,
			"versionNonce": 2039948547,
			"isDeleted": false,
			"id": "KrkoiHCe",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 879.9511048690259,
			"y": -121.54813179347627,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 193.98377990722656,
			"height": 38.4,
			"seed": 1125149146,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "ZnZIssV4I4uA_lm-IRy6y",
					"type": "arrow"
				},
				{
					"id": "Rz92Mr-EfHvcfCZ7lOIy_",
					"type": "arrow"
				}
			],
			"updated": 1684373133451,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "user id, item id, author id\n这些embedding的拼接",
			"rawText": "user id, item id, author id\n这些embedding的拼接",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "user id, item id, author id\n这些embedding的拼接",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 97,
			"versionNonce": 1192758342,
			"isDeleted": false,
			"id": "ZnZIssV4I4uA_lm-IRy6y",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 797.3933603038088,
			"y": -110.61144171200375,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 72.80230978260852,
			"height": 2.639930672011829,
			"seed": 1322514310,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "thvIqJDyzMLXZKQkB1zQ0",
				"focus": -0.3439180555053573,
				"gap": 4.067595108695173
			},
			"endBinding": {
				"elementId": "KrkoiHCe",
				"focus": 0.07714664684675948,
				"gap": 9.755434782608688
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					72.80230978260852,
					2.639930672011829
				]
			]
		},
		{
			"type": "rectangle",
			"version": 413,
			"versionNonce": 1601323802,
			"isDeleted": false,
			"id": "9glRnbBAikO3-470Mva7R",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 60,
			"angle": 0,
			"x": -266.6402674135833,
			"y": 579.0344089673936,
			"strokeColor": "transparent",
			"backgroundColor": "#12b886",
			"width": 42.99932065217399,
			"height": 51.647418478260825,
			"seed": 991030682,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "mRO9RRRX53R4szrBAIu4h",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 327,
			"versionNonce": 974820230,
			"isDeleted": false,
			"id": "mRO9RRRX53R4szrBAIu4h",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -218.75473752227896,
			"y": 595.0177649456546,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 980.1331484245268,
			"height": 634.932065217391,
			"seed": 873136710,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "o3OWNYdC"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "9glRnbBAikO3-470Mva7R",
				"focus": -0.345240132983255,
				"gap": 4.886209239130324
			},
			"endBinding": {
				"elementId": "istrCtP7AVVPykDNSV6rk",
				"focus": -0.26046930314316236,
				"gap": 1
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					888.4408967391307,
					-24.30706521739114
				],
				[
					980.1331484245268,
					-634.932065217391
				]
			]
		},
		{
			"type": "text",
			"version": 53,
			"versionNonce": 2033111002,
			"isDeleted": false,
			"id": "o3OWNYdC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 563.7741871709534,
			"y": 551.5106997282634,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 202.6484375,
			"height": 38.4,
			"seed": 963092870,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "Oep就是经过domain调制过的\n底层输入",
			"rawText": "Oep就是经过domain调制过的\n底层输入",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "mRO9RRRX53R4szrBAIu4h",
			"originalText": "Oep就是经过domain调制过的\n底层输入",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "rectangle",
			"version": 631,
			"versionNonce": 298463942,
			"isDeleted": false,
			"id": "istrCtP7AVVPykDNSV6rk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 60,
			"angle": 0,
			"x": 733.7775314994601,
			"y": -86.55322690217133,
			"strokeColor": "transparent",
			"backgroundColor": "#12b886",
			"width": 47.961956521739246,
			"height": 46.24999999999995,
			"seed": 36959258,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "mRO9RRRX53R4szrBAIu4h",
					"type": "arrow"
				},
				{
					"id": "G_vfKYmz3apzcpadGSx-_",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 763,
			"versionNonce": 1832633498,
			"isDeleted": false,
			"id": "y-4XSycDQ98mYrKlnxUOB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 60,
			"angle": 0,
			"x": 701.861770629895,
			"y": -75.73461277173615,
			"strokeColor": "transparent",
			"backgroundColor": "#228be6",
			"width": 21.450407608695873,
			"height": 27.68682065217387,
			"seed": 1868760858,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "mRO9RRRX53R4szrBAIu4h",
					"type": "arrow"
				},
				{
					"id": "zC5DWjNaaxDnVkelfKnUL",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 211,
			"versionNonce": 1788600493,
			"isDeleted": false,
			"id": "sQzSqj38",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 622.2506972603301,
			"y": 17.629857336959446,
			"strokeColor": "#000000",
			"backgroundColor": "#228be6",
			"width": 100.28790283203125,
			"height": 19.2,
			"seed": 1688440134,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "zC5DWjNaaxDnVkelfKnUL",
					"type": "arrow"
				}
			],
			"updated": 1684373133451,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "stop gradient",
			"rawText": "stop gradient",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "stop gradient",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 277,
			"versionNonce": 1699488090,
			"isDeleted": false,
			"id": "zC5DWjNaaxDnVkelfKnUL",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 708.807535949785,
			"y": -46.171093749997006,
			"strokeColor": "#000000",
			"backgroundColor": "#228be6",
			"width": 8.986970992281158,
			"height": 51.78668478260864,
			"seed": 1807264070,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "y-4XSycDQ98mYrKlnxUOB",
				"focus": 0.07820137899168257,
				"gap": 1.876698369565272
			},
			"endBinding": {
				"elementId": "sQzSqj38",
				"focus": 0.4569591072998777,
				"gap": 12.014266304347814
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-8.986970992281158,
					51.78668478260864
				]
			]
		},
		{
			"type": "text",
			"version": 63,
			"versionNonce": 2114238790,
			"isDeleted": false,
			"id": "xvYZNY7r",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 888.9021918255473,
			"y": 1.3499660326115759,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fd7e14",
			"width": 16,
			"height": 19.2,
			"seed": 153490758,
			"groupIds": [
				"dm8WLFeiIP6JZCW2tFmLJ"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "用",
			"rawText": "用",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "用",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 172,
			"versionNonce": 1555228186,
			"isDeleted": false,
			"id": "Dr49LZa5",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 906.06319726033,
			"y": 3.7901834239158916,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 44,
			"height": 18,
			"seed": 71223,
			"groupIds": [
				"dm8WLFeiIP6JZCW2tFmLJ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "G_vfKYmz3apzcpadGSx-_",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "e3f797ca8e91551430e1486a1181b52eb88e2a77",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 134,
			"versionNonce": 101813411,
			"isDeleted": false,
			"id": "FkaNTYhi",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 953.2771918255472,
			"y": 1.8356997282637622,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fd7e14",
			"width": 115.67997741699219,
			"height": 19.2,
			"seed": 794210310,
			"groupIds": [
				"dm8WLFeiIP6JZCW2tFmLJ"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133451,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "作为Gate的输入",
			"rawText": "作为Gate的输入",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "作为Gate的输入",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 147,
			"versionNonce": 1192575757,
			"isDeleted": false,
			"id": "9rCIAGyo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 882.0917298690256,
			"y": 29.539504076089827,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fd7e14",
			"width": 333.7599792480469,
			"height": 19.2,
			"seed": 1616570310,
			"groupIds": [
				"dm8WLFeiIP6JZCW2tFmLJ"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133452,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "我还能理解，但是把Oep也接入，就有点太卷了",
			"rawText": "我还能理解，但是把Oep也接入，就有点太卷了",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "我还能理解，但是把Oep也接入，就有点太卷了",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 156,
			"versionNonce": 1294966726,
			"isDeleted": false,
			"id": "zIQ1za22",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 903.4572189994606,
			"y": 68.94167798913332,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#228be6",
			"width": 272,
			"height": 38.4,
			"seed": 1805322266,
			"groupIds": [
				"dm8WLFeiIP6JZCW2tFmLJ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "lZTXRs-H9CemIu5H0GJLi",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "说明作者丝毫不信任DNN\n恨不能把所有信息都直接接入最后一层",
			"rawText": "说明作者丝毫不信任DNN\n恨不能把所有信息都直接接入最后一层",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "说明作者丝毫不信任DNN\n恨不能把所有信息都直接接入最后一层",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "text",
			"version": 130,
			"versionNonce": 384804762,
			"isDeleted": false,
			"id": "W15N4quA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 871.8064037820692,
			"y": 120.21545516304639,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#228be6",
			"width": 384,
			"height": 19.2,
			"seed": 926215578,
			"groupIds": [
				"dm8WLFeiIP6JZCW2tFmLJ"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "缺点是没有信息压缩，接入层大了，模型的参数也多了",
			"rawText": "缺点是没有信息压缩，接入层大了，模型的参数也多了",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "缺点是没有信息压缩，接入层大了，模型的参数也多了",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 70,
			"versionNonce": 1324594950,
			"isDeleted": false,
			"id": "G_vfKYmz3apzcpadGSx-_",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 785.2194472603301,
			"y": -55.170255360325655,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#228be6",
			"width": 112.72017094599312,
			"height": 51.42335795622065,
			"seed": 1097794438,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "istrCtP7AVVPykDNSV6rk",
				"focus": -0.12569683060255885,
				"gap": 3.479959239130608
			},
			"endBinding": {
				"elementId": "Dr49LZa5",
				"focus": 0.14680196782798338,
				"gap": 11.081521739130324
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					112.72017094599312,
					51.42335795622065
				]
			]
		},
		{
			"type": "rectangle",
			"version": 816,
			"versionNonce": 344695898,
			"isDeleted": false,
			"id": "pWslQHbfZvf1Yew6lfwQV",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -334.1521560005393,
			"y": -229.80050951086656,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 225.5061141304348,
			"height": 61.01562500000005,
			"seed": 1383717082,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "GXLddsV0dNw9XKrAuS5Pk",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 190,
			"versionNonce": 1377697859,
			"isDeleted": false,
			"id": "wEwzeju9",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -193.1229440440174,
			"y": -445.1418817934747,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#228be6",
			"width": 294.60791015625,
			"height": 76.8,
			"seed": 370220570,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "GXLddsV0dNw9XKrAuS5Pk",
					"type": "arrow"
				}
			],
			"updated": 1684373133453,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "这个画法有误导性\n每条输入的样本肯定只来自于一个domain\n因此每次output也是只针对一个domain\n图中好像会同时为多个domain生成output",
			"rawText": "这个画法有误导性\n每条输入的样本肯定只来自于一个domain\n因此每次output也是只针对一个domain\n图中好像会同时为多个domain生成output",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "这个画法有误导性\n每条输入的样本肯定只来自于一个domain\n因此每次output也是只针对一个domain\n图中好像会同时为多个domain生成output",
			"lineHeight": 1.2,
			"baseline": 73
		},
		{
			"type": "arrow",
			"version": 49,
			"versionNonce": 520751386,
			"isDeleted": false,
			"id": "GXLddsV0dNw9XKrAuS5Pk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -171.62304973371982,
			"y": -232.6758491847791,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 76.14979492085149,
			"height": 127.92459239130432,
			"seed": 795885638,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "pWslQHbfZvf1Yew6lfwQV",
				"focus": 0.22832471914556915,
				"gap": 2.875339673912549
			},
			"endBinding": {
				"elementId": "wEwzeju9",
				"focus": 0.1303908011169146,
				"gap": 7.741440217391272
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					76.14979492085149,
					-127.92459239130432
				]
			]
		},
		{
			"type": "image",
			"version": 115,
			"versionNonce": 894053766,
			"isDeleted": false,
			"id": "lB9X0ma9R0nYNt2Vw3B5z",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 849.9942383481034,
			"y": 330.8382811682776,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 573.2734375000001,
			"height": 211.72898958333337,
			"seed": 1132235462,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "lZTXRs-H9CemIu5H0GJLi",
					"type": "arrow"
				},
				{
					"id": "3ZpxRlsGZPwie365r1i9G",
					"type": "arrow"
				},
				{
					"id": "-9u5J3ky4ITcWlOnlQAJb",
					"type": "arrow"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "7986d9f48ede044be9ecf9b8e0455199ce0963dc",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "arrow",
			"version": 64,
			"versionNonce": 1037948378,
			"isDeleted": false,
			"id": "lZTXRs-H9CemIu5H0GJLi",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1025.650854238609,
			"y": 111.60016855998904,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 0.3305576563964223,
			"height": 218.18848760828857,
			"seed": 5972358,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "JkCFDMUz"
				}
			],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "zIQ1za22",
				"focus": 0.1015775231717438,
				"gap": 4.258490570855713
			},
			"endBinding": {
				"elementId": "lB9X0ma9R0nYNt2Vw3B5z",
				"focus": -0.38524646015890096,
				"gap": 1.0496249999999918
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					0.3305576563964223,
					218.18848760828857
				]
			]
		},
		{
			"type": "text",
			"version": 10,
			"versionNonce": 2012646598,
			"isDeleted": false,
			"id": "JkCFDMUz",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 977.8161330668072,
			"y": 211.09441236413332,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 96,
			"height": 19.2,
			"seed": 905744730,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577042,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "和作者的异同",
			"rawText": "和作者的异同",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "lZTXRs-H9CemIu5H0GJLi",
			"originalText": "和作者的异同",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 136,
			"versionNonce": 1727224173,
			"isDeleted": false,
			"id": "IvFKcDHh",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1483.5176758481034,
			"y": 351.7886561682776,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 415.74383544921875,
			"height": 38.4,
			"seed": 1739724634,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "3ZpxRlsGZPwie365r1i9G",
					"type": "arrow"
				}
			],
			"updated": 1684373133453,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "相同点，我和作者都认为这种结构就是为了防止信息丢失\n所以在每层都必须用超级个性化的prior information来增强",
			"rawText": "相同点，我和作者都认为这种结构就是为了防止信息丢失\n所以在每层都必须用超级个性化的prior information来增强",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "相同点，我和作者都认为这种结构就是为了防止信息丢失\n所以在每层都必须用超级个性化的prior information来增强",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 34,
			"versionNonce": 1012204550,
			"isDeleted": false,
			"id": "3ZpxRlsGZPwie365r1i9G",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1429.2793945981034,
			"y": 412.3433436682776,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 46.83203125,
			"height": 33.38671875,
			"seed": 89936198,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577043,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "lB9X0ma9R0nYNt2Vw3B5z",
				"focus": 0.5940213612321523,
				"gap": 6.01171875
			},
			"endBinding": {
				"elementId": "IvFKcDHh",
				"focus": 0.8692413818386499,
				"gap": 7.40625
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					46.83203125,
					-33.38671875
				]
			]
		},
		{
			"type": "arrow",
			"version": 18,
			"versionNonce": 577873754,
			"isDeleted": false,
			"id": "-9u5J3ky4ITcWlOnlQAJb",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1430.4434570981034,
			"y": 470.1011561682776,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 43.81640625,
			"height": 24.74609375,
			"seed": 1077282522,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577043,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "lB9X0ma9R0nYNt2Vw3B5z",
				"focus": -0.4950087766973326,
				"gap": 7.17578125
			},
			"endBinding": {
				"elementId": "bkmYbQXu",
				"focus": -0.8883812587234917,
				"gap": 5.39453125
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					43.81640625,
					24.74609375
				]
			]
		},
		{
			"type": "text",
			"version": 170,
			"versionNonce": 932904931,
			"isDeleted": false,
			"id": "bkmYbQXu",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1479.6543945981034,
			"y": 460.0503749182776,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 524.81591796875,
			"height": 57.599999999999994,
			"seed": 1701457926,
			"groupIds": [
				"2EJacBj5Qna1OahbZuDGk"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "-9u5J3ky4ITcWlOnlQAJb",
					"type": "arrow"
				}
			],
			"updated": 1684373133454,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "不同点，我看不出这种增强和multi-task有什么关系\n这种损失是由于DNN造成的，不是由于multi-task之间的数据不均衡造成的\n换句话说，我不认为这种结构对于multi-task有什么帮助",
			"rawText": "不同点，我看不出这种增强和multi-task有什么关系\n这种损失是由于DNN造成的，不是由于multi-task之间的数据不均衡造成的\n换句话说，我不认为这种结构对于multi-task有什么帮助",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "不同点，我看不出这种增强和multi-task有什么关系\n这种损失是由于DNN造成的，不是由于multi-task之间的数据不均衡造成的\n换句话说，我不认为这种结构对于multi-task有什么帮助",
			"lineHeight": 1.2,
			"baseline": 54
		},
		{
			"type": "image",
			"version": 242,
			"versionNonce": 657959962,
			"isDeleted": false,
			"id": "et7n6CigSt-A1_8TijsdW",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1564.5606445981034,
			"y": 519.9761561682776,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 62.52734375,
			"height": 62.52734375,
			"seed": 992676934,
			"groupIds": [
				"2EJacBj5Qna1OahbZuDGk"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577043,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "9bee7ee34f66a316e5e1d076b4f5a44318512bef",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "line",
			"version": 226,
			"versionNonce": 1249655430,
			"isDeleted": false,
			"id": "L3BRqFvBeByzOklkOiiQ3",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -526.6659179018966,
			"y": 667.5269374182776,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1461.703125,
			"height": 3.5390625,
			"seed": 1965194970,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239577043,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": null,
			"points": [
				[
					0,
					0
				],
				[
					1461.703125,
					-3.5390625
				]
			]
		},
		{
			"type": "text",
			"version": 189,
			"versionNonce": 1669309389,
			"isDeleted": false,
			"id": "q3tuSdZf",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -232.82867831856328,
			"y": 929.8179096404997,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 369.3398742675781,
			"height": 24,
			"seed": 739300166,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "Rz92Mr-EfHvcfCZ7lOIy_",
					"type": "arrow"
				},
				{
					"id": "ZRC7WPb_6IoBntyhoPFDy",
					"type": "arrow"
				},
				{
					"id": "8EGQcyUAp9iERcCJirDgs",
					"type": "arrow"
				}
			],
			"updated": 1684373133454,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "工程难点全在海量feature的embedding上",
			"rawText": "工程难点全在海量feature的embedding上",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "工程难点全在海量feature的embedding上",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 558,
			"versionNonce": 358545862,
			"isDeleted": false,
			"id": "Rz92Mr-EfHvcfCZ7lOIy_",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 929.1999675147699,
			"y": -77.23651744283332,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 784.1405492331934,
			"height": 1016.9053819444447,
			"seed": 1560542150,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "SMA8bbN8"
				}
			],
			"updated": 1684239577043,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "KrkoiHCe",
				"focus": 0.2978104398955341,
				"gap": 5.911614350642964
			},
			"endBinding": {
				"elementId": "q3tuSdZf",
				"focus": 0.9078448186495269,
				"gap": 8.548222332561693
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-548.4809027777777,
					896.6796875000001
				],
				[
					-784.1405492331934,
					1016.9053819444447
				]
			]
		},
		{
			"type": "text",
			"version": 124,
			"versionNonce": 796585370,
			"isDeleted": false,
			"id": "SMA8bbN8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 270.7190647369922,
			"y": 783.4431700571668,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 220,
			"height": 72,
			"seed": 1459406534,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239577043,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "用这么细粒度的特征\n提升个性化同时\n也增加了存储量与训练量",
			"rawText": "用这么细粒度的特征\n提升个性化同时\n也增加了存储量与训练量",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "Rz92Mr-EfHvcfCZ7lOIy_",
			"originalText": "用这么细粒度的特征\n提升个性化同时\n也增加了存储量与训练量",
			"lineHeight": 1.2,
			"baseline": 67
		},
		{
			"type": "text",
			"version": 109,
			"versionNonce": 818625306,
			"isDeleted": false,
			"id": "rhhkX00L",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 469.02502093357384,
			"y": 889.5299756127222,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 280,
			"height": 24,
			"seed": 1569007622,
			"groupIds": [
				"oTNrWYIuCI2z-bJ0Ib6rY"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239944359,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "需要优化的特征准入与逐出政策",
			"rawText": "需要优化的特征准入与逐出政策",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "需要优化的特征准入与逐出政策",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 125,
			"versionNonce": 1352076163,
			"isDeleted": false,
			"id": "J1FDR1wD",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 465.8739792669071,
			"y": 919.7166075571663,
			"strokeColor": "#364fc7",
			"backgroundColor": "transparent",
			"width": 438.2200012207031,
			"height": 24,
			"seed": 51624390,
			"groupIds": [
				"oTNrWYIuCI2z-bJ0Ib6rY"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "ZRC7WPb_6IoBntyhoPFDy",
					"type": "arrow"
				}
			],
			"updated": 1684373133455,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "比如根据id的更新频率来决定这些id在PS中的去留",
			"rawText": "比如根据id的更新频率来决定这些id在PS中的去留",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "比如根据id的更新频率来决定这些id在PS中的去留",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 95,
			"versionNonce": 1892917805,
			"isDeleted": false,
			"id": "saSsxLOW",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 294.3415273438302,
			"y": 1017.2593425999016,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 445.1999206542969,
			"height": 24,
			"seed": 956930650,
			"groupIds": [
				"dwTN6zvYjrN0dXHuo99k6"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133455,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "针对embedding与DNN也需要完全不同的训练策略",
			"rawText": "针对embedding与DNN也需要完全不同的训练策略",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "针对embedding与DNN也需要完全不同的训练策略",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 110,
			"versionNonce": 1901110051,
			"isDeleted": false,
			"id": "IZEnQUVO",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 337.85281206605237,
			"y": 1045.089203711013,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 261.94000244140625,
			"height": 24,
			"seed": 988621914,
			"groupIds": [
				"dwTN6zvYjrN0dXHuo99k6"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133455,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "这里是指用不同的SGD与步长",
			"rawText": "这里是指用不同的SGD与步长",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "这里是指用不同的SGD与步长",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 112,
			"versionNonce": 64541837,
			"isDeleted": false,
			"id": "JPO0o1gt",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 282.119305121608,
			"y": 1075.0023981554573,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 525.199951171875,
			"height": 24,
			"seed": 973817754,
			"groupIds": [
				"dwTN6zvYjrN0dXHuo99k6"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "8EGQcyUAp9iERcCJirDgs",
					"type": "arrow"
				}
			],
			"updated": 1684373133455,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "其实embedding与DNN在分布式训练中本来的策略就不相同",
			"rawText": "其实embedding与DNN在分布式训练中本来的策略就不相同",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "其实embedding与DNN在分布式训练中本来的策略就不相同",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 162,
			"versionNonce": 1647724227,
			"isDeleted": false,
			"id": "76WCjtUb",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 333.1739926216078,
			"y": 1104.7550023221238,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 308.71990966796875,
			"height": 48,
			"seed": 1573611462,
			"groupIds": [
				"dwTN6zvYjrN0dXHuo99k6"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684373133456,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "embedding可以纯异步+PS模式\nDNN权重需要同步+AllReduce模式",
			"rawText": "embedding可以纯异步+PS模式\nDNN权重需要同步+AllReduce模式",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "embedding可以纯异步+PS模式\nDNN权重需要同步+AllReduce模式",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "arrow",
			"version": 99,
			"versionNonce": 1829485254,
			"isDeleted": false,
			"id": "ZRC7WPb_6IoBntyhoPFDy",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 143.57930779254818,
			"y": 951.5901805538033,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 313.9179353632478,
			"height": 14.350019551935247,
			"seed": 1168122182,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239944359,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "q3tuSdZf",
				"focus": 0.9068050663819411,
				"gap": 7.068111843533359
			},
			"endBinding": {
				"elementId": "J1FDR1wD",
				"focus": 0.2214501089721559,
				"gap": 8.376736111111086
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					313.9179353632478,
					-14.350019551935247
				]
			]
		},
		{
			"type": "arrow",
			"version": 150,
			"versionNonce": 1448999174,
			"isDeleted": false,
			"id": "8EGQcyUAp9iERcCJirDgs",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 112.94628697343958,
			"y": 960.048347872922,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 160.10183759261278,
			"height": 131.08973452822784,
			"seed": 1539724038,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239945921,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "q3tuSdZf",
				"focus": -0.696996042094232,
				"gap": 6.230438232422273
			},
			"endBinding": {
				"elementId": "JPO0o1gt",
				"focus": -0.9980753331854413,
				"gap": 9.071180555555657
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					160.10183759261278,
					131.08973452822784
				]
			]
		},
		{
			"type": "text",
			"version": 23,
			"versionNonce": 716800282,
			"isDeleted": false,
			"id": "8vnk0bay",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 390.3504749934024,
			"y": -527.0604357120651,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 80,
			"height": 24,
			"seed": 711617222,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684239708076,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "总结一下",
			"rawText": "总结一下",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "总结一下",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 83,
			"versionNonce": 1353334662,
			"isDeleted": false,
			"id": "bqt5tj0h",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 616.3480711472486,
			"y": -620.5279837889882,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 380,
			"height": 48,
			"seed": 1741233734,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "GzARdkCotxr4PszHxI9vX",
					"type": "arrow"
				}
			],
			"updated": 1684239868555,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "模型有效果是因为使用了最最个性化的特征\n比如各种id embedding",
			"rawText": "模型有效果是因为使用了最最个性化的特征\n比如各种id embedding",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "模型有效果是因为使用了最最个性化的特征\n比如各种id embedding",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "text",
			"version": 77,
			"versionNonce": 944290541,
			"isDeleted": false,
			"id": "ptqtqstX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 631.9189846087869,
			"y": -516.7178876351421,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 344.87994384765625,
			"height": 48,
			"seed": 2133804570,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "A3J5J7W2yjrmFsQDnWd_0",
					"type": "arrow"
				}
			],
			"updated": 1684373133456,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "但是为了让这些id embedding发挥使用\n才发明这样的类似LHUC结构",
			"rawText": "但是为了让这些id embedding发挥使用\n才发明这样的类似LHUC结构",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "但是为了让这些id embedding发挥使用\n才发明这样的类似LHUC结构",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "rectangle",
			"version": 49,
			"versionNonce": 1380242630,
			"isDeleted": false,
			"id": "cZsK0lx06c7hCA3HyWszv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 373.90215768571,
			"y": -537.3488972505268,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 110.07211538461547,
			"height": 42.62019230769232,
			"seed": 115258822,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "GzARdkCotxr4PszHxI9vX",
					"type": "arrow"
				},
				{
					"id": "A3J5J7W2yjrmFsQDnWd_0",
					"type": "arrow"
				},
				{
					"id": "9OlWLGq4gCeEH2FqLIqwt",
					"type": "arrow"
				}
			],
			"updated": 1684239873774,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 199,
			"versionNonce": 1508275811,
			"isDeleted": false,
			"id": "jgy7XXzU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 587.6641769164793,
			"y": -431.2130799428344,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 933.9398803710938,
			"height": 48,
			"seed": 694064602,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "9OlWLGq4gCeEH2FqLIqwt",
					"type": "arrow"
				}
			],
			"updated": 1684373133457,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "而不是相反，如果只是模仿了结构，但是喂入这种Gate Network当裁判的不是最最个性化的id embedding\n就本末倒置、南辕北辙了",
			"rawText": "而不是相反，如果只是模仿了结构，但是喂入这种Gate Network当裁判的不是最最个性化的id embedding\n就本末倒置、南辕北辙了",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "而不是相反，如果只是模仿了结构，但是喂入这种Gate Network当裁判的不是最最个性化的id embedding\n就本末倒置、南辕北辙了",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "arrow",
			"version": 37,
			"versionNonce": 1570101018,
			"isDeleted": false,
			"id": "GzARdkCotxr4PszHxI9vX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 490.3504749934024,
			"y": -518.5207722505268,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 112.72836538461547,
			"height": 58.91225961538464,
			"seed": 684572102,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239868555,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "cZsK0lx06c7hCA3HyWszv",
				"focus": 0.5913927493366852,
				"gap": 6.376201923076906
			},
			"endBinding": {
				"elementId": "bqt5tj0h",
				"focus": 0.7067156763461585,
				"gap": 13.269230769230717
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					112.72836538461547,
					-58.91225961538464
				]
			]
		},
		{
			"type": "arrow",
			"version": 47,
			"versionNonce": 130485638,
			"isDeleted": false,
			"id": "A3J5J7W2yjrmFsQDnWd_0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 496.14374422417166,
			"y": -513.3825510966806,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 122.90865384615381,
			"height": 10.19831730769232,
			"seed": 364897242,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239871256,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "cZsK0lx06c7hCA3HyWszv",
				"focus": -0.112847204913141,
				"gap": 12.169471153846189
			},
			"endBinding": {
				"elementId": "ptqtqstX",
				"focus": -0.128156039010625,
				"gap": 12.866586538461434
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					122.90865384615381,
					10.19831730769232
				]
			]
		},
		{
			"type": "arrow",
			"version": 143,
			"versionNonce": 282595226,
			"isDeleted": false,
			"id": "9OlWLGq4gCeEH2FqLIqwt",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 494.6533596087869,
			"y": -502.2728647326636,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 88.4183619014741,
			"height": 68.38920253273739,
			"seed": 668429510,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684239875398,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "cZsK0lx06c7hCA3HyWszv",
				"focus": -0.5815572821228027,
				"gap": 10.679086538461434
			},
			"endBinding": {
				"elementId": "jgy7XXzU",
				"focus": -0.8776743491832613,
				"gap": 5.312499999999943
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					88.4183619014741,
					68.38920253273739
				]
			]
		},
		{
			"type": "rectangle",
			"version": 194,
			"versionNonce": 960262042,
			"isDeleted": false,
			"id": "9Au-UTUVmnXDaZZoJrAMU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 356.70864807032547,
			"y": -647.0303876351422,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 1176.9591346153852,
			"height": 284.81971153846166,
			"seed": 616249670,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1684239896509,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 73,
			"versionNonce": 824090245,
			"isDeleted": false,
			"id": "MoP2wMR3Lqrtcmb0J_vv8",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -462.1727623802248,
			"y": 223.22650076725483,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 101.32324218750006,
			"height": 50.7373046875,
			"seed": 1236476645,
			"groupIds": [],
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "fosXhBxk67hFTilroLMPp",
					"type": "arrow"
				}
			],
			"updated": 1684306287614,
			"link": null,
			"locked": false
		},
		{
			"type": "arrow",
			"version": 235,
			"versionNonce": 1587969195,
			"isDeleted": false,
			"id": "fosXhBxk67hFTilroLMPp",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 53.20809699477536,
			"y": 396.19525076725483,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 413.1787109375001,
			"height": 118.65234375,
			"seed": 1628856261,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684306304676,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "5jO5KMW19PLERlyZdEp5T",
				"focus": 0.8658835124738445,
				"gap": 2.0867804827473435
			},
			"endBinding": {
				"elementId": "MoP2wMR3Lqrtcmb0J_vv8",
				"focus": 0.8100068756321122,
				"gap": 3.5791015625
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-28.6865234375,
					-83.76953125
				],
				[
					-413.1787109375001,
					-118.65234375
				]
			]
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#000000",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 4,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 1061.3428944129048,
		"scrollY": 748.5830051565092,
		"zoom": {
			"value": 0.5
		},
		"currentItemRoundness": "round",
		"gridSize": null,
		"colorPalette": {},
		"currentStrokeOptions": null,
		"previousGridSize": null
	},
	"files": {}
}
```
%%