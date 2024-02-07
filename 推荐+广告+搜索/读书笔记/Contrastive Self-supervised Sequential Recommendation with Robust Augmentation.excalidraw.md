---

excalidraw-plugin: parsed
alias: CoSeRec
发布时间: 2021-08-14
出品方: Salesforce
价值: ⭐⭐⭐⭐⭐
环节: 

---
关键词:: #Reco/新用户冷启, #ML/自监督学习 
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
要解决的问题
- data sparsity
- 新用户的短序列，有噪音 ^IdTom3nS

augmentation难点
- 序列是discrete的，有些continuous的augmentation用不上
- 序列中的item都是correlated，随便shuffle就不像了
- 像crop或mask，有效长度变短，对cold-start不利 ^yjXWBxvA

提出substitute & insert
- 不破坏sequence中的item correlation
- 增加的都是correlated，增强augmentation成为positive的可信度
- 增加了用户序列的长度，对新用户冷启更友好 ^5aNshUMy

基础模型 ^rTzlNYO6

transformer ^kRVmKdrE

没那么简单，不能使用普通的transformer
否则在建模ht时要用到t+1及以后的item，造成信息泄漏 ^Esp60wBG

用户序列 ^zwS7tMNK

传统Augmentation方法 ^qo0NVJ1Z

crop ^RSAfYUJe

摘取一个连续子序列 ^0xRfe7ZE

mask ^LVd4zWmj

中间mask掉的item不是删除，而是用特殊占位符代替 ^1rwyBEFQ

reorder ^xFJHxGW6

shuffle一个子序列 ^ugFBV7Kl

新序列长度不变
只是部分内容改变了 ^PalalWIV

Substitute ^VM0qy6Wg

把原序列中的一些item有与其correlated的其他item来代替 ^IMHrYK9w

Insert ^vLneFqwo

将一些correlated items插入原序列 ^8C7mBkMZ

如何找到哪些correlated items ^WEVQ1XsQ

memory-based ^9I0Whloj

model-based ^DnUFWDas

item embedding可以直接从当前模型中获得 ^D7QRv9rr

hybrid method ^EPGrvqNr

两种correlation先normalize，才好相互比较 ^hL5qwP1u

不同序列长度
不同Augmentation ^1ybRYktq

short sequences are more sensitive to random item perturbations ^FvB4BrhA

mask在transformer中是如何处理的
当null vector??? ^7S4vNzm7

训练过程 ^kLN0ssUL

样本 ^lG032gxd

Loss ^4Piga9Bt

建模 ^V21X9QGN

N个user，每个user做2次augmentation ^BWVXU30K

通过transformer来建模 ^z3ZNkahO

为了下一步能够做dot-product，岂不是要求对一个序列的两次augmentation
必须产生相同的新序列长度，否则concatenate之后也不等长，也无法dot-product ^xDv2b5Gw

两种训练方式 ^2GuJyPDh

multi-task learning vs. pre-training ^eH3Tdver

作者的结论是MTL is better than two-stage pre-training ^o39AS8Iw

疑问 ^umshuzPp


# Embedded files
fb518626ebd3c6e0b8320a560222d7863635336b: [[Pasted Image 20220923083055_750.png]]
6fd59681d6d6099dba502fb6263414ed48f1ad80: [[Pasted Image 20220923083111_742.png]]
ca9363086a1916be1d704b86932edd7169bbe155: [[Pasted Image 20220923083126_746.png]]
1cdfd084de5477b5fea40c63485d534a0628015d: [[Pasted Image 20220923083213_003.png]]
49fa4eca77c44d019dfe77ab372c90eb975468f7: [[Pasted Image 20220923084607_054.png]]
e4cf27ff0c466869655cf549fbee812d5c0f531f: [[Pasted Image 20220923084832_886.png]]
3a8e1713e282262fa2030726a69786a660d15170: [[Pasted Image 20220923085747_037.png]]
f141cdff5b602dbdd0d19e7faa20bcad3fa4d65f: [[Pasted Image 20220923085925_069.png]]
9de3eb376ebcc92754874b93c52027a37365ce17: [[Pasted Image 20220923090130_169.png]]
2eec63fb8c149a7b0dc718ff00fdc4a0da9eed31: [[Pasted Image 20220923090440_067.png]]
f455f1acff3f992600d2fb392734d543cf777c0c: [[Pasted Image 20220923090527_107.png]]
62289774e60b8a34fc3b42aec1e606aa97104a72: [[Pasted Image 20220923091430_585.png]]
cbbbb930ffe5d693dcd7bc4bf576fbee164d9ac8: [[Pasted Image 20220923091536_371.png]]
da20f5bf23c71f1f9d77125bc7ea46971d3ac4fc: [[Pasted Image 20220923091658_000.png]]
011f6c70b0192aef54cb6c61924be501ef52e8a8: [[Pasted Image 20220923092631_528.png]]
8f192600599a13478579dd56be46bdeb9f717442: [[Pasted Image 20220923092941_967.png]]
5abfebe0ad3421e242a3885d093b8aa316a760f6: [[Pasted Image 20220923093251_235.png]]
0fb5486ff2a05141b606ae8230cfadc0b8498d03: [[Pasted Image 20220923094423_022.png]]
5819e8328620db8c63326cfc977183a126cc9b2d: [[Pasted Image 20220923094539_763.png]]
1fa972ab6a8cd20d1031ce268d4a508b757b4b96: [[Pasted Image 20220923094554_768.png]]
f20321899de28f1e9774b847fcb86c2c7ee7b8e4: [[Pasted Image 20220923094655_958.png]]
832064ea111778becc86367df2fdc022937e47eb: [[Pasted Image 20220923094921_891.png]]
9e10ce0a8a3ea452a5e0b1ebb04a6b8a9028443d: [[Pasted Image 20220923095420_156.png]]
6102c27eaa1b4e48cc6f8c085729c9962204dd95: [[Pasted Image 20220923095847_791.png]]
bceecb3d4f40647e620afcc94f5b1501ca785d64: [[Pasted Image 20220923100327_522.png]]

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.1",
	"elements": [
		{
			"type": "text",
			"version": 120,
			"versionNonce": 1924055842,
			"isDeleted": false,
			"id": "IdTom3nS",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -18.94921875,
			"y": -557.09765625,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 187.19998168945312,
			"height": 57.599999999999994,
			"seed": 830062566,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412882,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "要解决的问题\n- data sparsity\n- 新用户的短序列，有噪音",
			"rawText": "要解决的问题\n- data sparsity\n- 新用户的短序列，有噪音",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "要解决的问题\n- data sparsity\n- 新用户的短序列，有噪音",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "text",
			"version": 680,
			"versionNonce": 1726740478,
			"isDeleted": false,
			"id": "yjXWBxvA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 445.97265625,
			"y": -494.15234375,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 431.95172119140625,
			"height": 76.8,
			"seed": 869827898,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "in6d23KpvVofIZAd9d3JQ",
					"type": "arrow"
				}
			],
			"updated": 1707276412884,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "augmentation难点\n- 序列是discrete的，有些continuous的augmentation用不上\n- 序列中的item都是correlated，随便shuffle就不像了\n- 像crop或mask，有效长度变短，对cold-start不利",
			"rawText": "augmentation难点\n- 序列是discrete的，有些continuous的augmentation用不上\n- 序列中的item都是correlated，随便shuffle就不像了\n- 像crop或mask，有效长度变短，对cold-start不利",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "augmentation难点\n- 序列是discrete的，有些continuous的augmentation用不上\n- 序列中的item都是correlated，随便shuffle就不像了\n- 像crop或mask，有效长度变短，对cold-start不利",
			"lineHeight": 1.2,
			"baseline": 73
		},
		{
			"type": "text",
			"version": 789,
			"versionNonce": 1206783714,
			"isDeleted": false,
			"id": "5aNshUMy",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -410.578125,
			"y": 425.85095635775866,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 471.583740234375,
			"height": 76.8,
			"seed": 1325644602,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "iYndM2zwUrHaTsuq5YvIO",
					"type": "arrow"
				},
				{
					"id": "SLFVWYAJ4es8zC0R6Mj2m",
					"type": "arrow"
				}
			],
			"updated": 1707276412886,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "提出substitute & insert\n- 不破坏sequence中的item correlation\n- 增加的都是correlated，增强augmentation成为positive的可信度\n- 增加了用户序列的长度，对新用户冷启更友好",
			"rawText": "提出substitute & insert\n- 不破坏sequence中的item correlation\n- 增加的都是correlated，增强augmentation成为positive的可信度\n- 增加了用户序列的长度，对新用户冷启更友好",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "提出substitute & insert\n- 不破坏sequence中的item correlation\n- 增加的都是correlated，增强augmentation成为positive的可信度\n- 增加了用户序列的长度，对新用户冷启更友好",
			"lineHeight": 1.2,
			"baseline": 73
		},
		{
			"type": "image",
			"version": 322,
			"versionNonce": 363637875,
			"isDeleted": false,
			"id": "xbQ5Sr5_8FHimlqp1Dp_9",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -103.84179687500003,
			"y": -402.4338294719833,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 212.01077586206895,
			"height": 23.86876284539849,
			"seed": 26402234,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157546627,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "fb518626ebd3c6e0b8320a560222d7863635336b",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 518,
			"versionNonce": 1780009021,
			"isDeleted": false,
			"id": "J3LPhe0qbDzJ4n5klGUuB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -102.08209859913799,
			"y": -375.30559671336266,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 132.98589341692795,
			"height": 29.256896551724147,
			"seed": 678438822,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157546627,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "6fd59681d6d6099dba502fb6263414ed48f1ad80",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 359,
			"versionNonce": 1994696211,
			"isDeleted": false,
			"id": "f2Eg7aUP1QNdnocid27tm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -104.09536119860752,
			"y": -338.94717496269953,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 404.28825431034494,
			"height": 58.981045058538385,
			"seed": 473814458,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157546627,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "ca9363086a1916be1d704b86932edd7169bbe155",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 527,
			"versionNonce": 1403959453,
			"isDeleted": false,
			"id": "yNSuX4EhkJZwLfs__ZaQM",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -101.70421006944461,
			"y": -274.42165002152507,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 356.88393742913837,
			"height": 118.15751982451204,
			"seed": 405641082,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157546627,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "1cdfd084de5477b5fea40c63485d534a0628015d",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 249,
			"versionNonce": 812082040,
			"isDeleted": false,
			"id": "rTzlNYO6",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -103.42990451388908,
			"y": -435.8344104381913,
			"strokeColor": "#364fc7",
			"backgroundColor": "transparent",
			"width": 80,
			"height": 24,
			"seed": 1664825274,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672892343814,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "基础模型",
			"rawText": "基础模型",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "基础模型",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 351,
			"versionNonce": 15222846,
			"isDeleted": false,
			"id": "kRVmKdrE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 42.792751736110915,
			"y": -368.5101916881913,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 89.82391357421875,
			"height": 19.2,
			"seed": 1145791846,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412887,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "transformer",
			"rawText": "transformer",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "transformer",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 319,
			"versionNonce": 382591650,
			"isDeleted": false,
			"id": "Esp60wBG",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -102.42209201388908,
			"y": -147.0805041881913,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 391.8399353027344,
			"height": 38.4,
			"seed": 1246997222,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412889,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "没那么简单，不能使用普通的transformer\n否则在建模ht时要用到t+1及以后的item，造成信息泄漏",
			"rawText": "没那么简单，不能使用普通的transformer\n否则在建模ht时要用到t+1及以后的item，造成信息泄漏",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "没那么简单，不能使用普通的transformer\n否则在建模ht时要用到t+1及以后的item，造成信息泄漏",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "rectangle",
			"version": 358,
			"versionNonce": 1682240861,
			"isDeleted": false,
			"id": "vXhbn0ClTHYf1EYaITymX",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -111.32356770833354,
			"y": -441.1587416774289,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 417.515625,
			"height": 343.171875,
			"seed": 1944800570,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1664157546627,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 311,
			"versionNonce": 818507528,
			"isDeleted": false,
			"id": "zwS7tMNK",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 114.1803385416668,
			"y": -400.3638760470476,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 64,
			"height": 19.2,
			"seed": 1908997862,
			"groupIds": [
				"L-hL38v4S1FxlgHYzP_uN"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672892343814,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "用户序列",
			"rawText": "用户序列",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "用户序列",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 1991,
			"versionNonce": 65805693,
			"isDeleted": false,
			"id": "in6d23KpvVofIZAd9d3JQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 670.1476896239087,
			"y": 7.098851263303118,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 177.51416205234653,
			"height": 411.1649112944076,
			"seed": 1744501946,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507121,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Pzxu6BSRjPNjbvLcrwrTm",
				"gap": 8.559798998908661,
				"focus": 0.7983872350541527
			},
			"endBinding": {
				"elementId": "yjXWBxvA",
				"focus": 0.6193202919738271,
				"gap": 6.086283718895572
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
					53.563219960272704,
					-106.45769179702387
				],
				[
					49.87113550003704,
					-235.18433451225513
				],
				[
					-123.95094209207383,
					-411.1649112944076
				]
			]
		},
		{
			"type": "text",
			"version": 404,
			"versionNonce": 1869647998,
			"isDeleted": false,
			"id": "qo0NVJ1Z",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -105.5642509707261,
			"y": -69.67246979704777,
			"strokeColor": "#364fc7",
			"backgroundColor": "transparent",
			"width": 200.20547485351562,
			"height": 22.854759484290234,
			"seed": 2086120634,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412890,
			"link": null,
			"locked": false,
			"fontSize": 19.045632903575196,
			"fontFamily": 4,
			"text": "传统Augmentation方法",
			"rawText": "传统Augmentation方法",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "传统Augmentation方法",
			"lineHeight": 1.2,
			"baseline": 18
		},
		{
			"type": "text",
			"version": 310,
			"versionNonce": 1778204258,
			"isDeleted": false,
			"id": "RSAfYUJe",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -113.48105655187669,
			"y": -34.47758189238979,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 31.891586303710938,
			"height": 18.283807587432186,
			"seed": 1576635494,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412891,
			"link": null,
			"locked": false,
			"fontSize": 15.236506322860157,
			"fontFamily": 4,
			"text": "crop",
			"rawText": "crop",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "crop",
			"lineHeight": 1.2,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 423,
			"versionNonce": 776327358,
			"isDeleted": false,
			"id": "0xRfe7ZE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -70.63664885255801,
			"y": -33.06679426990276,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 137.06996154785156,
			"height": 18.283807587432186,
			"seed": 1619232422,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412892,
			"link": null,
			"locked": false,
			"fontSize": 15.236506322860157,
			"fontFamily": 4,
			"text": "摘取一个连续子序列",
			"rawText": "摘取一个连续子序列",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "摘取一个连续子序列",
			"lineHeight": 1.2,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 306,
			"versionNonce": 508240418,
			"isDeleted": false,
			"id": "LVd4zWmj",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -116.3555363326936,
			"y": -3.3423631977924515,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 36.71949768066406,
			"height": 18.283807587432186,
			"seed": 1384907770,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412892,
			"link": null,
			"locked": false,
			"fontSize": 15.236506322860157,
			"fontFamily": 4,
			"text": "mask",
			"rawText": "mask",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "mask",
			"lineHeight": 1.2,
			"baseline": 14
		},
		{
			"type": "image",
			"version": 803,
			"versionNonce": 663317523,
			"isDeleted": false,
			"id": "ldWilb7_0UAOMB4Xl7ief",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -74.49606229676948,
			"y": 2.933024049928502,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 372.5318356238956,
			"height": 53.38832039195956,
			"seed": 1571613178,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "q-P47lVRsOqRahi6vd2dT",
					"type": "arrow"
				},
				{
					"id": "9prczmU6YTTh_ujEEQ6zk",
					"type": "arrow"
				}
			],
			"updated": 1664157507121,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "49fa4eca77c44d019dfe77ab372c90eb975468f7",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 377,
			"versionNonce": 420022526,
			"isDeleted": false,
			"id": "1rwyBEFQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -56.74493725460913,
			"y": 63.4326673201916,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 356.275146484375,
			"height": 18.283807587432186,
			"seed": 871850790,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "9prczmU6YTTh_ujEEQ6zk",
					"type": "arrow"
				}
			],
			"updated": 1707276412893,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 15.236506322860157,
			"fontFamily": 4,
			"text": "中间mask掉的item不是删除，而是用特殊占位符代替",
			"rawText": "中间mask掉的item不是删除，而是用特殊占位符代替",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "中间mask掉的item不是删除，而是用特殊占位符代替",
			"lineHeight": 1.2,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 381,
			"versionNonce": 514231778,
			"isDeleted": false,
			"id": "xFJHxGW6",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -113.25690113568616,
			"y": 96.63976984562436,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 53.5029296875,
			"height": 18.283807587432186,
			"seed": 656902374,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412894,
			"link": null,
			"locked": false,
			"fontSize": 15.236506322860157,
			"fontFamily": 4,
			"text": "reorder",
			"rawText": "reorder",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "reorder",
			"lineHeight": 1.2,
			"baseline": 14
		},
		{
			"type": "text",
			"version": 346,
			"versionNonce": 1866152254,
			"isDeleted": false,
			"id": "ugFBV7Kl",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -43.740341037636696,
			"y": 99.38873912604274,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 125.40371704101562,
			"height": 18.28380758743219,
			"seed": 1284869286,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412894,
			"link": null,
			"locked": false,
			"fontSize": 15.236506322860158,
			"fontFamily": 4,
			"text": "shuffle一个子序列",
			"rawText": "shuffle一个子序列",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "shuffle一个子序列",
			"lineHeight": 1.2,
			"baseline": 14
		},
		{
			"type": "image",
			"version": 544,
			"versionNonce": 374719315,
			"isDeleted": false,
			"id": "aaG9tNr3fWQSkUV_cSO4D",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -56.59986309772651,
			"y": 124.63908212398579,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 425.3760272309638,
			"height": 40.68045166865766,
			"seed": 1911797626,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "ZHBtG3jsDOdrlw9L6HaEJ",
					"type": "arrow"
				}
			],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "e4cf27ff0c466869655cf549fbee812d5c0f531f",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 465,
			"versionNonce": 538870178,
			"isDeleted": false,
			"id": "PalalWIV",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 467.4749488657686,
			"y": 154.02961836835055,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 137.06996154785156,
			"height": 36.56761517486437,
			"seed": 1498133242,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "q-P47lVRsOqRahi6vd2dT",
					"type": "arrow"
				},
				{
					"id": "ZHBtG3jsDOdrlw9L6HaEJ",
					"type": "arrow"
				}
			],
			"updated": 1707276412895,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 15.236506322860157,
			"fontFamily": 4,
			"text": "新序列长度不变\n只是部分内容改变了",
			"rawText": "新序列长度不变\n只是部分内容改变了",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "新序列长度不变\n只是部分内容改变了",
			"lineHeight": 1.2,
			"baseline": 32
		},
		{
			"type": "arrow",
			"version": 1020,
			"versionNonce": 1946673395,
			"isDeleted": false,
			"id": "q-P47lVRsOqRahi6vd2dT",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 158.9891252034626,
			"y": 64.14046951284806,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 303.11596644702666,
			"height": 83.8972102958938,
			"seed": 2056688890,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "ldWilb7_0UAOMB4Xl7ief",
				"focus": 0.2740447644083567,
				"gap": 7.819125070959998
			},
			"endBinding": {
				"elementId": "PalalWIV",
				"focus": 0.1379388215517699,
				"gap": 5.991938559608684
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
					303.11596644702666,
					83.8972102958938
				]
			]
		},
		{
			"type": "arrow",
			"version": 985,
			"versionNonce": 429464509,
			"isDeleted": false,
			"id": "ZHBtG3jsDOdrlw9L6HaEJ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 375.38633789684144,
			"y": 159.98837770700413,
			"strokeColor": "#5c940d",
			"backgroundColor": "transparent",
			"width": 87.30488364197453,
			"height": 0.3424722476934417,
			"seed": 1269145126,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "aaG9tNr3fWQSkUV_cSO4D",
				"focus": 0.6681998743711797,
				"gap": 6.610173763604109
			},
			"endBinding": {
				"elementId": "PalalWIV",
				"focus": 0.6614664497554873,
				"gap": 4.783727326952658
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
					87.30488364197453,
					0.3424722476934417
				]
			]
		},
		{
			"type": "rectangle",
			"version": 571,
			"versionNonce": 1021659795,
			"isDeleted": false,
			"id": "Pzxu6BSRjPNjbvLcrwrTm",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -121.462890625,
			"y": -68.09379521977165,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 783.05078125,
			"height": 276.89305565741,
			"seed": 832071718,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "in6d23KpvVofIZAd9d3JQ",
					"type": "arrow"
				}
			],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "image",
			"version": 600,
			"versionNonce": 1725471773,
			"isDeleted": false,
			"id": "dM6Um9lLExtLxiv81sPja",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 377.5364112988466,
			"y": -52.14307766302741,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 216.68127277993258,
			"height": 130.00876366795956,
			"seed": 1672735526,
			"groupIds": [
				"ZTtmDYjTfmdgG_S0xDB1E"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "f141cdff5b602dbdd0d19e7faa20bcad3fa4d65f",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 474,
			"versionNonce": 94666803,
			"isDeleted": false,
			"id": "LxaB6aeM_rqmeZ4GqoG-l",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 132.798828125,
			"y": 325.8798426201822,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 421.27734375,
			"height": 46.44002214566929,
			"seed": 587658086,
			"groupIds": [
				"9qTpOMQwuvItWRKGmmGcw"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "iYndM2zwUrHaTsuq5YvIO",
					"type": "arrow"
				}
			],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "3a8e1713e282262fa2030726a69786a660d15170",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 275,
			"versionNonce": 1480417662,
			"isDeleted": false,
			"id": "IMHrYK9w",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 137.779296875,
			"y": 383.6728113701822,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 412.3998718261719,
			"height": 19.2,
			"seed": 1760947066,
			"groupIds": [
				"9qTpOMQwuvItWRKGmmGcw"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412896,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "把原序列中的一些item有与其correlated的其他item来代替",
			"rawText": "把原序列中的一些item有与其correlated的其他item来代替",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "把原序列中的一些item有与其correlated的其他item来代替",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 178,
			"versionNonce": 1575091554,
			"isDeleted": false,
			"id": "VM0qy6Wg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 140.82473734789627,
			"y": 277.9857034606401,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 127.06147766113281,
			"height": 31.996663278006324,
			"seed": 1652123770,
			"groupIds": [
				"9qTpOMQwuvItWRKGmmGcw"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412897,
			"link": null,
			"locked": false,
			"fontSize": 26.663886065005272,
			"fontFamily": 4,
			"text": "Substitute",
			"rawText": "Substitute",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Substitute",
			"lineHeight": 1.2,
			"baseline": 25
		},
		{
			"type": "image",
			"version": 181,
			"versionNonce": 1642854621,
			"isDeleted": false,
			"id": "Xv4c_peH5rOP22_B6hkGY",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 577.962890625,
			"y": 294.7665613701822,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 273.15625,
			"height": 99.80709134615385,
			"seed": 511218810,
			"groupIds": [
				"9qTpOMQwuvItWRKGmmGcw"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "9de3eb376ebcc92754874b93c52027a37365ce17",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 348,
			"versionNonce": 1733329342,
			"isDeleted": false,
			"id": "vLneFqwo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 185.64787997061248,
			"y": 450.46575718437515,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 72.91505432128906,
			"height": 31.996663278006324,
			"seed": 184693242,
			"groupIds": [
				"KgTNzdN0fRfOsheJ0quHP"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412897,
			"link": null,
			"locked": false,
			"fontSize": 26.663886065005272,
			"fontFamily": 4,
			"text": "Insert",
			"rawText": "Insert",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Insert",
			"lineHeight": 1.2,
			"baseline": 25
		},
		{
			"type": "image",
			"version": 681,
			"versionNonce": 387110205,
			"isDeleted": false,
			"id": "m5qwRXZigVvwd64-bQq8t",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 177.427734375,
			"y": 659.6220301201822,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 243.8111447704083,
			"height": 76.33703574281154,
			"seed": 687055910,
			"groupIds": [
				"KgTNzdN0fRfOsheJ0quHP"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "2eec63fb8c149a7b0dc718ff00fdc4a0da9eed31",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 235,
			"versionNonce": 1430257954,
			"isDeleted": false,
			"id": "8C7mBkMZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 288.677734375,
			"y": 451.5673426201822,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 250.4478759765625,
			"height": 19.2,
			"seed": 1333336890,
			"groupIds": [
				"KgTNzdN0fRfOsheJ0quHP"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412898,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "将一些correlated items插入原序列",
			"rawText": "将一些correlated items插入原序列",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "将一些correlated items插入原序列",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 393,
			"versionNonce": 1340024221,
			"isDeleted": false,
			"id": "CcTCdNVAWNvPXCjFdRZ9q",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 177.32091590447155,
			"y": 484.8407801201822,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 455.2786934705285,
			"height": 169.43806141263238,
			"seed": 1136177958,
			"groupIds": [
				"KgTNzdN0fRfOsheJ0quHP"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "SLFVWYAJ4es8zC0R6Mj2m",
					"type": "arrow"
				}
			],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "f455f1acff3f992600d2fb392734d543cf777c0c",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "arrow",
			"version": 106,
			"versionNonce": 2101694131,
			"isDeleted": false,
			"id": "iYndM2zwUrHaTsuq5YvIO",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 40.173828125,
			"y": 422.2978113701822,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 85.76953125,
			"height": 77.18359375,
			"seed": 1287080294,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "5aNshUMy",
				"focus": 0.6120076899846885,
				"gap": 3.553144987576445
			},
			"endBinding": {
				"elementId": "LxaB6aeM_rqmeZ4GqoG-l",
				"focus": 0.938595655901869,
				"gap": 6.85546875
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
					85.76953125,
					-77.18359375
				]
			]
		},
		{
			"type": "arrow",
			"version": 109,
			"versionNonce": 1602382333,
			"isDeleted": false,
			"id": "SLFVWYAJ4es8zC0R6Mj2m",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 64.138671875,
			"y": 495.8044453559497,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 101.39453124999997,
			"height": 23.843273486959447,
			"seed": 1252097786,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507122,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "5aNshUMy",
				"focus": -0.316824737881655,
				"gap": 13.716796875
			},
			"endBinding": {
				"elementId": "CcTCdNVAWNvPXCjFdRZ9q",
				"focus": -0.046220925035939345,
				"gap": 11.787712779471576
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
					101.39453124999997,
					23.843273486959447
				]
			]
		},
		{
			"type": "text",
			"version": 931,
			"versionNonce": 529897982,
			"isDeleted": false,
			"id": "WEVQ1XsQ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -516.521484375,
			"y": 994.2665613701822,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 273.05987548828125,
			"height": 24,
			"seed": 1483741030,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "iYndM2zwUrHaTsuq5YvIO",
					"type": "arrow"
				},
				{
					"id": "SLFVWYAJ4es8zC0R6Mj2m",
					"type": "arrow"
				},
				{
					"id": "luQFJUW19i68fk8Vc0t44",
					"type": "arrow"
				},
				{
					"id": "xj3MOSQdrcQRdYYSZz2XJ",
					"type": "arrow"
				},
				{
					"id": "yCyfFWPRK8YzHPW1c5QNR",
					"type": "arrow"
				}
			],
			"updated": 1707276412899,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 4,
			"text": "如何找到哪些correlated items",
			"rawText": "如何找到哪些correlated items",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "如何找到哪些correlated items",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "image",
			"version": 351,
			"versionNonce": 922624605,
			"isDeleted": false,
			"id": "QKRdsv7jcM3F43KKYNI7I",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -178.400390625,
			"y": 800.2509363701822,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 520.12109375,
			"height": 159.14152868470148,
			"seed": 541109562,
			"groupIds": [
				"qj05VrgKBxRRLSaDvYlL8"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "luQFJUW19i68fk8Vc0t44",
					"type": "arrow"
				}
			],
			"updated": 1664157507123,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "62289774e60b8a34fc3b42aec1e606aa97104a72",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 67,
			"versionNonce": 27757794,
			"isDeleted": false,
			"id": "9I0Whloj",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -172.384765625,
			"y": 773.0126551201822,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 108.68792724609375,
			"height": 19.2,
			"seed": 1094628154,
			"groupIds": [
				"qj05VrgKBxRRLSaDvYlL8"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412899,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "memory-based",
			"rawText": "memory-based",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "memory-based",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 185,
			"versionNonce": 723766973,
			"isDeleted": false,
			"id": "SjZFbfyiNvhqmNs7ZXiRN",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -124.935546875,
			"y": 1014.6923426201822,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 559.87109375,
			"height": 168.54539633010432,
			"seed": 1587509734,
			"groupIds": [
				"WOJf8_rOwE-jwfzHJLMAW"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "xj3MOSQdrcQRdYYSZz2XJ",
					"type": "arrow"
				}
			],
			"updated": 1664157507123,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "cbbbb930ffe5d693dcd7bc4bf576fbee164d9ac8",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 77,
			"versionNonce": 2014161470,
			"isDeleted": false,
			"id": "DnUFWDas",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -112.458984375,
			"y": 983.3173426201822,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 94.15992736816406,
			"height": 19.2,
			"seed": 72481658,
			"groupIds": [
				"WOJf8_rOwE-jwfzHJLMAW"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412900,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "model-based",
			"rawText": "model-based",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "model-based",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 151,
			"versionNonce": 413638818,
			"isDeleted": false,
			"id": "D7QRv9rr",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -14.373046875,
			"y": 981.4149988701822,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 310.2079162597656,
			"height": 19.2,
			"seed": 1155586874,
			"groupIds": [
				"WOJf8_rOwE-jwfzHJLMAW"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412900,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "item embedding可以直接从当前模型中获得",
			"rawText": "item embedding可以直接从当前模型中获得",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "item embedding可以直接从当前模型中获得",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 164,
			"versionNonce": 1198914717,
			"isDeleted": false,
			"id": "luQFJUW19i68fk8Vc0t44",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -323.373046875,
			"y": 991.4306238701822,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 123.7890625,
			"height": 133.01171875,
			"seed": 864749882,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1670448107549,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WEVQ1XsQ",
				"focus": 0.2780362937449078,
				"gap": 2.8359375
			},
			"endBinding": {
				"elementId": "QKRdsv7jcM3F43KKYNI7I",
				"focus": 0.9013774855194272,
				"gap": 21.18359375
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
					47.9921875,
					-75.666015625
				],
				[
					123.7890625,
					-133.01171875
				]
			]
		},
		{
			"type": "arrow",
			"version": 159,
			"versionNonce": 2081255133,
			"isDeleted": false,
			"id": "xj3MOSQdrcQRdYYSZz2XJ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -236.654296875,
			"y": 1011.5198449863219,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 91.18359375,
			"height": 39.95163244702803,
			"seed": 1652439482,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1670448120708,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WEVQ1XsQ",
				"focus": -0.8221844748565065,
				"gap": 9.8671875
			},
			"endBinding": {
				"elementId": "SjZFbfyiNvhqmNs7ZXiRN",
				"focus": -0.40669876773503394,
				"gap": 20.53515625
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
					42.349609375,
					23.944566223513903
				],
				[
					91.18359375,
					39.95163244702803
				]
			]
		},
		{
			"type": "arrow",
			"version": 145,
			"versionNonce": 1082048211,
			"isDeleted": false,
			"id": "yCyfFWPRK8YzHPW1c5QNR",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -386.6575369927266,
			"y": 1027.5634363701822,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 73.12742983728447,
			"height": 164.00390625,
			"seed": 407181178,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507123,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WEVQ1XsQ",
				"focus": 0.09899464656299327,
				"gap": 6.296875
			},
			"endBinding": {
				"elementId": "SVnL-O0JhLHojkQ4HRvIe",
				"focus": -0.21335819919785587,
				"gap": 19.066406249999886
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
					73.12742983728447,
					164.00390625
				]
			]
		},
		{
			"type": "image",
			"version": 319,
			"versionNonce": 451906525,
			"isDeleted": false,
			"id": "SVnL-O0JhLHojkQ4HRvIe",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -521.6660156249994,
			"y": 1210.6337488701822,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 647.7890624999994,
			"height": 141.0351562499999,
			"seed": 811831162,
			"groupIds": [
				"yX_QVB_Afa1Nsk7-JQ0T9"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "yCyfFWPRK8YzHPW1c5QNR",
					"type": "arrow"
				}
			],
			"updated": 1664157507123,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "da20f5bf23c71f1f9d77125bc7ea46971d3ac4fc",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 116,
			"versionNonce": 1603908222,
			"isDeleted": false,
			"id": "EPGrvqNr",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -500.541015625,
			"y": 1186.4267176201822,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 107.56791687011719,
			"height": 19.2,
			"seed": 1744289530,
			"groupIds": [
				"yX_QVB_Afa1Nsk7-JQ0T9"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412901,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "hybrid method",
			"rawText": "hybrid method",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "hybrid method",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 482,
			"versionNonce": 1173306466,
			"isDeleted": false,
			"id": "hL5qwP1u",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -224.763671875,
			"y": 1293.5165613701822,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 314.17584228515625,
			"height": 19.2,
			"seed": 376960698,
			"groupIds": [
				"yX_QVB_Afa1Nsk7-JQ0T9"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412902,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "两种correlation先normalize，才好相互比较",
			"rawText": "两种correlation先normalize，才好相互比较",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "两种correlation先normalize，才好相互比较",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 450,
			"versionNonce": 1051026963,
			"isDeleted": false,
			"id": "RZwvynsSG1jMXre5VidUY",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 742.3017578125002,
			"y": 60.766093185090995,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 65.71484375,
			"height": 65.71484375,
			"seed": 594174714,
			"groupIds": [
				"pVau3cuHjSvrmGQ8l5Vbk"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "9prczmU6YTTh_ujEEQ6zk",
					"type": "arrow"
				}
			],
			"updated": 1664157507123,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "5abfebe0ad3421e242a3885d093b8aa316a760f6",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 362,
			"versionNonce": 1090171582,
			"isDeleted": false,
			"id": "7S4vNzm7",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 817.0517578125002,
			"y": 86.996561935091,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 256.39990234375,
			"height": 38.4,
			"seed": 1004583398,
			"groupIds": [
				"pVau3cuHjSvrmGQ8l5Vbk"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412902,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "mask在transformer中是如何处理的\n当null vector???",
			"rawText": "mask在transformer中是如何处理的\n当null vector???",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "mask在transformer中是如何处理的\n当null vector???",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 658,
			"versionNonce": 1294040814,
			"isDeleted": false,
			"id": "9prczmU6YTTh_ujEEQ6zk",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 303.9072265625003,
			"y": 53.33390132231789,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 433.36718749999994,
			"height": 48.82507929782009,
			"seed": 317368230,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "umshuzPp"
				}
			],
			"updated": 1670504705507,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"startBinding": {
				"elementId": "1rwyBEFQ",
				"focus": -1.1932653754110225,
				"gap": 10.098765997873706
			},
			"endBinding": {
				"elementId": "RZwvynsSG1jMXre5VidUY",
				"focus": -0.1691415030416125,
				"gap": 5.02734375
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
					218.59319884417772,
					48.82507929782009
				],
				[
					433.36718749999994,
					46.34817143608422
				]
			]
		},
		{
			"type": "text",
			"version": 41,
			"versionNonce": 126600456,
			"isDeleted": false,
			"id": "umshuzPp",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 505.500425406678,
			"y": 91.15898062013798,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 32,
			"height": 19.2,
			"seed": 2059573938,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1672892343817,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "疑问",
			"rawText": "疑问",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "9prczmU6YTTh_ujEEQ6zk",
			"originalText": "疑问",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 174,
			"versionNonce": 1733124130,
			"isDeleted": false,
			"id": "kLN0ssUL",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -584.6904296874998,
			"y": 1824.781718185091,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 112,
			"height": 33.6,
			"seed": 2074263334,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412903,
			"link": null,
			"locked": false,
			"fontSize": 28,
			"fontFamily": 4,
			"text": "训练过程",
			"rawText": "训练过程",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "训练过程",
			"lineHeight": 1.2,
			"baseline": 26
		},
		{
			"type": "text",
			"version": 1156,
			"versionNonce": 114404094,
			"isDeleted": false,
			"id": "1ybRYktq",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -465.716796875,
			"y": 1482.2314051201822,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 170.29991149902344,
			"height": 48,
			"seed": 1564256294,
			"groupIds": [
				"QoEoJL--yfnBXhNNwKCIr"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "iYndM2zwUrHaTsuq5YvIO",
					"type": "arrow"
				},
				{
					"id": "SLFVWYAJ4es8zC0R6Mj2m",
					"type": "arrow"
				},
				{
					"id": "luQFJUW19i68fk8Vc0t44",
					"type": "arrow"
				},
				{
					"id": "xj3MOSQdrcQRdYYSZz2XJ",
					"type": "arrow"
				},
				{
					"id": "yCyfFWPRK8YzHPW1c5QNR",
					"type": "arrow"
				}
			],
			"updated": 1707276412904,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 4,
			"text": "不同序列长度\n不同Augmentation",
			"rawText": "不同序列长度\n不同Augmentation",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "不同序列长度\n不同Augmentation",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "text",
			"version": 102,
			"versionNonce": 1219562466,
			"isDeleted": false,
			"id": "FvB4BrhA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -264.5107421874998,
			"y": 1403.988749435091,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 495.3274230957031,
			"height": 19.2,
			"seed": 877762618,
			"groupIds": [
				"QoEoJL--yfnBXhNNwKCIr"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412905,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "short sequences are more sensitive to random item perturbations",
			"rawText": "short sequences are more sensitive to random item perturbations",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "short sequences are more sensitive to random item perturbations",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 412,
			"versionNonce": 1965955827,
			"isDeleted": false,
			"id": "BTmiJYfFEIlJW9z7tDV2w",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -268.5576171874998,
			"y": 1432.586405685091,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 482.37109375,
			"height": 60.97693284424379,
			"seed": 16101734,
			"groupIds": [
				"QoEoJL--yfnBXhNNwKCIr"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507123,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "011f6c70b0192aef54cb6c61924be501ef52e8a8",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 392,
			"versionNonce": 1351319997,
			"isDeleted": false,
			"id": "PGp53nlKFYwbvJ4xpjihT",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -266.2110332414214,
			"y": 1502.344218185091,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 394.9659160539216,
			"height": 128.7109375,
			"seed": 1920047994,
			"groupIds": [
				"QoEoJL--yfnBXhNNwKCIr"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "8f192600599a13478579dd56be46bdeb9f717442",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 202,
			"versionNonce": 1899250835,
			"isDeleted": false,
			"id": "dzoXGdYdD2NzD4yNQiHnd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -468.3193359374998,
			"y": 1400.383280685091,
			"strokeColor": "#1864ab",
			"backgroundColor": "transparent",
			"width": 701.6796875,
			"height": 251.71875,
			"seed": 2106172986,
			"groupIds": [
				"QoEoJL--yfnBXhNNwKCIr"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 73,
			"versionNonce": 1821647624,
			"isDeleted": false,
			"id": "lG032gxd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -313.7724609374998,
			"y": 1741.965311935091,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 40,
			"height": 24,
			"seed": 1715189478,
			"groupIds": [
				"ZesS0jBFMNrjSzeAtP-u9"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "d_F_Bn_2RQB0YeukGDcVo",
					"type": "arrow"
				}
			],
			"updated": 1672892343817,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 4,
			"text": "样本",
			"rawText": "样本",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "样本",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 89,
			"versionNonce": 530553662,
			"isDeleted": false,
			"id": "BWVXU30K",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -254.95214843749977,
			"y": 1710.359843185091,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 286.143798828125,
			"height": 19.2,
			"seed": 29687354,
			"groupIds": [
				"ZesS0jBFMNrjSzeAtP-u9"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412906,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "N个user，每个user做2次augmentation",
			"rawText": "N个user，每个user做2次augmentation",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "N个user，每个user做2次augmentation",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 330,
			"versionNonce": 857339517,
			"isDeleted": false,
			"id": "Z933egnJ4A_co7ipyUT8S",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -255.5178897938831,
			"y": 1736.164530685091,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 461.83136635638334,
			"height": 97.4823692458833,
			"seed": 1549419942,
			"groupIds": [
				"ZesS0jBFMNrjSzeAtP-u9"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "0fb5486ff2a05141b606ae8230cfadc0b8498d03",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 321,
			"versionNonce": 478444499,
			"isDeleted": false,
			"id": "Q7lqZm-RRIcwpcwLTNzp-",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -658.7930944991436,
			"y": 1867.914530685091,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 292.91853595890416,
			"height": 218.64062500000006,
			"seed": 1669138342,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "d_F_Bn_2RQB0YeukGDcVo",
					"type": "arrow"
				},
				{
					"id": "XhqLsAAZdKWI7j7HUz0G2",
					"type": "arrow"
				},
				{
					"id": "hGvmStuNT5uE7G9g6cvuT",
					"type": "arrow"
				},
				{
					"id": "wDWl_lE5BZWstLs_-6GQf",
					"type": "arrow"
				}
			],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "f20321899de28f1e9774b847fcb86c2c7ee7b8e4",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 370,
			"versionNonce": 53117448,
			"isDeleted": false,
			"id": "V21X9QGN",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -312.1982421874998,
			"y": 1902.644999435091,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 40,
			"height": 24,
			"seed": 217241126,
			"groupIds": [
				"fUTj8uzpUOJhTc83l6auk"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1672892343817,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "建模",
			"rawText": "建模",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "建模",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 85,
			"versionNonce": 1613316002,
			"isDeleted": false,
			"id": "z3ZNkahO",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -252.96386718749977,
			"y": 1844.605936935091,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 169.82391357421875,
			"height": 19.2,
			"seed": 509182438,
			"groupIds": [
				"fUTj8uzpUOJhTc83l6auk"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412907,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "通过transformer来建模",
			"rawText": "通过transformer来建模",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "通过transformer来建模",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 385,
			"versionNonce": 111295293,
			"isDeleted": false,
			"id": "41tTef0S_7skazGm_xAX2",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -252.37464021381558,
			"y": 1865.789530685091,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 511.9068667763158,
			"height": 43.61538326793722,
			"seed": 557723962,
			"groupIds": [
				"fUTj8uzpUOJhTc83l6auk"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "5819e8328620db8c63326cfc977183a126cc9b2d",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 486,
			"versionNonce": 1478484755,
			"isDeleted": false,
			"id": "MfPCEFRxSd8KCDoR0-0tC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -250.88574218749974,
			"y": 1914.312968185091,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 496.85156249999994,
			"height": 69.70753264925374,
			"seed": 264518822,
			"groupIds": [
				"fUTj8uzpUOJhTc83l6auk"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "XhqLsAAZdKWI7j7HUz0G2",
					"type": "arrow"
				},
				{
					"id": "qEkT0Vt12JGeO5O31gDm4",
					"type": "arrow"
				}
			],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "1fa972ab6a8cd20d1031ce268d4a508b757b4b96",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 253,
			"versionNonce": 29721470,
			"isDeleted": false,
			"id": "xDv2b5Gw",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -255.88574218749977,
			"y": 1992.699686935091,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 579.5198364257812,
			"height": 38.4,
			"seed": 509967206,
			"groupIds": [
				"fUTj8uzpUOJhTc83l6auk"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412908,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "为了下一步能够做dot-product，岂不是要求对一个序列的两次augmentation\n必须产生相同的新序列长度，否则concatenate之后也不等长，也无法dot-product",
			"rawText": "为了下一步能够做dot-product，岂不是要求对一个序列的两次augmentation\n必须产生相同的新序列长度，否则concatenate之后也不等长，也无法dot-product",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "为了下一步能够做dot-product，岂不是要求对一个序列的两次augmentation\n必须产生相同的新序列长度，否则concatenate之后也不等长，也无法dot-product",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "text",
			"version": 316,
			"versionNonce": 166001506,
			"isDeleted": false,
			"id": "4Piga9Bt",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -307.4560546874998,
			"y": 2105.043436935091,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 42.57997131347656,
			"height": 24,
			"seed": 1579607078,
			"groupIds": [
				"0m9kw7qrmsmpPN5qfhVo9"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "hGvmStuNT5uE7G9g6cvuT",
					"type": "arrow"
				}
			],
			"updated": 1707276412908,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 4,
			"text": "Loss",
			"rawText": "Loss",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "Loss",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "image",
			"version": 275,
			"versionNonce": 317739005,
			"isDeleted": false,
			"id": "wX3ndNEs7JCvsOwje1MK9",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -259.26245712652485,
			"y": 2060.422343185091,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 570.1189024390251,
			"height": 140.81250000000017,
			"seed": 1573500198,
			"groupIds": [
				"0m9kw7qrmsmpPN5qfhVo9"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "f1nt8QtPItq89SJ009qD4",
					"type": "arrow"
				}
			],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "832064ea111778becc86367df2fdc022937e47eb",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "arrow",
			"version": 83,
			"versionNonce": 251296339,
			"isDeleted": false,
			"id": "d_F_Bn_2RQB0YeukGDcVo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -349.5107421874998,
			"y": 1896.906718185091,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 57.1328125,
			"height": 114.2890625,
			"seed": 1318231782,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Q7lqZm-RRIcwpcwLTNzp-",
				"focus": 0.6099564773555598,
				"gap": 16.36381635273972
			},
			"endBinding": {
				"elementId": "lG032gxd",
				"focus": -0.5309597799909689,
				"gap": 13.65234375
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
					57.1328125,
					-114.2890625
				]
			]
		},
		{
			"type": "arrow",
			"version": 100,
			"versionNonce": 1337935965,
			"isDeleted": false,
			"id": "XhqLsAAZdKWI7j7HUz0G2",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -346.3740234374998,
			"y": 1956.453593185091,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 88.03515625,
			"height": 0,
			"seed": 667898362,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Q7lqZm-RRIcwpcwLTNzp-",
				"focus": -0.19009504752376177,
				"gap": 19.50053510273972
			},
			"endBinding": {
				"elementId": "MfPCEFRxSd8KCDoR0-0tC",
				"focus": -0.20906947638036097,
				"gap": 7.453125
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
					88.03515625,
					0
				]
			]
		},
		{
			"type": "arrow",
			"version": 85,
			"versionNonce": 83736563,
			"isDeleted": false,
			"id": "hGvmStuNT5uE7G9g6cvuT",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -356.1748046874998,
			"y": 2064.207499435091,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 46.02689322161359,
			"height": 36.36956286036593,
			"seed": 295239546,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Q7lqZm-RRIcwpcwLTNzp-",
				"focus": -0.16265902519332556,
				"gap": 9.699753852739718
			},
			"endBinding": {
				"elementId": "4Piga9Bt",
				"focus": 0.014902097573652165,
				"gap": 5.21484375
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
					46.02689322161359,
					36.36956286036593
				]
			]
		},
		{
			"type": "image",
			"version": 537,
			"versionNonce": 483407037,
			"isDeleted": false,
			"id": "dF3vfzO_IZZIYk_OVKHCU",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -208.13574218749977,
			"y": -574.026875564909,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 77.82812499999997,
			"height": 77.82812499999997,
			"seed": 1653842554,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": "https://github.com/YChen1993/CoSeRec",
			"locked": false,
			"status": "pending",
			"fileId": "9e10ce0a8a3ea452a5e0b1ebb04a6b8a9028443d",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "arrow",
			"version": 118,
			"versionNonce": 2131247507,
			"isDeleted": false,
			"id": "wDWl_lE5BZWstLs_-6GQf",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -475.66610557958495,
			"y": 2100.16564675652,
			"strokeColor": "#d9480f",
			"backgroundColor": "transparent",
			"width": 3.2867966242608873,
			"height": 146.375,
			"seed": 454246586,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507124,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Q7lqZm-RRIcwpcwLTNzp-",
				"focus": -0.22769754100717984,
				"gap": 13.610491071428896
			},
			"endBinding": {
				"elementId": "eH3Tdver",
				"focus": -0.7253801736917052,
				"gap": 7.21484375
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
					3.2867966242608873,
					146.375
				]
			]
		},
		{
			"type": "image",
			"version": 447,
			"versionNonce": 96769309,
			"isDeleted": false,
			"id": "lRB0uXOIUKqUe5lvjyqLn",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 392.7935801685844,
			"y": 1715.5211155065197,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 532.4320126488097,
			"height": 471.6093750000002,
			"seed": 1423206714,
			"groupIds": [
				"W2CNrQOL4Rpatx7RgCPyr"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507125,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "6102c27eaa1b4e48cc6f8c085729c9962204dd95",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 342,
			"versionNonce": 374380339,
			"isDeleted": false,
			"id": "2LoebvpmJpvnSeQQnZHOZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 443.8572892459656,
			"y": 1988.3570530065197,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 438.6796875,
			"height": 73.35156250000023,
			"seed": 1660176678,
			"groupIds": [
				"W2CNrQOL4Rpatx7RgCPyr"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "qEkT0Vt12JGeO5O31gDm4",
					"type": "arrow"
				}
			],
			"updated": 1664157507125,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 343,
			"versionNonce": 638445949,
			"isDeleted": false,
			"id": "3VXCevALI4qYiH4MB0XP-",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 436.5565079959656,
			"y": 2125.97814675652,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 464.2890625,
			"height": 38.3984375,
			"seed": 1074276710,
			"groupIds": [
				"W2CNrQOL4Rpatx7RgCPyr"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "f1nt8QtPItq89SJ009qD4",
					"type": "arrow"
				},
				{
					"id": "LJoPok95TWgf5iIkAKQ7-",
					"type": "arrow"
				}
			],
			"updated": 1664157507125,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "arrow",
			"version": 549,
			"versionNonce": 2034055379,
			"isDeleted": false,
			"id": "qEkT0Vt12JGeO5O31gDm4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 259.9822892459656,
			"y": 1950.9931933087987,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 182.87499999999994,
			"height": 55.168666795078025,
			"seed": 1649475238,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507125,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "MfPCEFRxSd8KCDoR0-0tC",
				"focus": -0.7047906387525541,
				"gap": 14.016468933465376
			},
			"endBinding": {
				"elementId": "2LoebvpmJpvnSeQQnZHOZ",
				"focus": -0.46283136742263614,
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
					182.87499999999994,
					55.168666795078025
				]
			]
		},
		{
			"type": "arrow",
			"version": 496,
			"versionNonce": 787329501,
			"isDeleted": false,
			"id": "f1nt8QtPItq89SJ009qD4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 312.79088299596566,
			"y": 2143.472115617066,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 116.11718749999994,
			"height": 10.00406312048517,
			"seed": 521589670,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507125,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "wX3ndNEs7JCvsOwje1MK9",
				"focus": -0.12722937961594658,
				"gap": 1.9344376834654327
			},
			"endBinding": {
				"elementId": "3VXCevALI4qYiH4MB0XP-",
				"focus": -0.7387357505052595,
				"gap": 7.6484375
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
					116.11718749999994,
					10.00406312048517
				]
			]
		},
		{
			"type": "arrow",
			"version": 1131,
			"versionNonce": 1672507310,
			"isDeleted": false,
			"id": "LJoPok95TWgf5iIkAKQ7-",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -108.37708575403443,
			"y": 2294.486585088977,
			"strokeColor": "#0b7285",
			"backgroundColor": "transparent",
			"width": 541.0078375629729,
			"height": 170.11610532072746,
			"seed": 2082415162,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1670504745125,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "3VXCevALI4qYiH4MB0XP-",
				"focus": 1.0264332434285939,
				"gap": 3.9257561870271047
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
					298.3931361607124,
					-49.37057321883867
				],
				[
					541.0078375629729,
					-170.11610532072746
				]
			]
		},
		{
			"type": "text",
			"version": 520,
			"versionNonce": 1399300104,
			"isDeleted": false,
			"id": "2GuJyPDh",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -647.7169295040344,
			"y": 2296.8140842565194,
			"strokeColor": "#e67700",
			"backgroundColor": "transparent",
			"width": 120,
			"height": 24,
			"seed": 1603766054,
			"groupIds": [
				"FHRQuon_xofKs3TS_Gk_b",
				"N3dqKh5dZ2M5cRM43AS2J"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "hGvmStuNT5uE7G9g6cvuT",
					"type": "arrow"
				}
			],
			"updated": 1672892343818,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 4,
			"text": "两种训练方式",
			"rawText": "两种训练方式",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "两种训练方式",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 131,
			"versionNonce": 797205438,
			"isDeleted": false,
			"id": "eH3Tdver",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -507.2364607540344,
			"y": 2253.75549050652,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 261.9037170410156,
			"height": 19.2,
			"seed": 1133472570,
			"groupIds": [
				"N3dqKh5dZ2M5cRM43AS2J"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "wDWl_lE5BZWstLs_-6GQf",
					"type": "arrow"
				}
			],
			"updated": 1707276412909,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "multi-task learning vs. pre-training",
			"rawText": "multi-task learning vs. pre-training",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "multi-task learning vs. pre-training",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 216,
			"versionNonce": 1144209186,
			"isDeleted": false,
			"id": "o39AS8Iw",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -515.5723982540344,
			"y": 2279.38830300652,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 408.5436706542969,
			"height": 19.2,
			"seed": 117102842,
			"groupIds": [
				"N3dqKh5dZ2M5cRM43AS2J"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1707276412910,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "作者的结论是MTL is better than two-stage pre-training",
			"rawText": "作者的结论是MTL is better than two-stage pre-training",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "作者的结论是MTL is better than two-stage pre-training",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 329,
			"versionNonce": 397272499,
			"isDeleted": false,
			"id": "nPgq6vnu-lfbG-KmN3OCg",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -519.8954079266582,
			"y": 2311.745166845805,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 361.73437499999994,
			"height": 78.54498245937961,
			"seed": 492907174,
			"groupIds": [
				"N3dqKh5dZ2M5cRM43AS2J"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507125,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "bceecb3d4f40647e620afcc94f5b1501ca785d64",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "line",
			"version": 126,
			"versionNonce": 1859306237,
			"isDeleted": false,
			"id": "xnxA6gEChxktFdNhM49Nt",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -580.6675433433161,
			"y": 1685.676558745138,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1344.08984375,
			"height": 2.1015625,
			"seed": 523261050,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507125,
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
					1344.08984375,
					2.1015625
				]
			]
		},
		{
			"type": "line",
			"version": 127,
			"versionNonce": 787265363,
			"isDeleted": false,
			"id": "yr5yKc7sXv9p76QRQ6co1",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -575.2730120933161,
			"y": 1385.067183745138,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1129.578125,
			"height": 1.24609375,
			"seed": 712531642,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507125,
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
					1129.578125,
					-1.24609375
				]
			]
		},
		{
			"type": "line",
			"version": 110,
			"versionNonce": 215702365,
			"isDeleted": false,
			"id": "A2zPPJZItfnqR9q5hQMKP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -563.9839495933161,
			"y": 770.000777495138,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 1161.35546875,
			"height": 4.85546875,
			"seed": 1171852666,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664157507125,
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
					1161.35546875,
					4.85546875
				]
			]
		},
		{
			"type": "line",
			"version": 273,
			"versionNonce": 353088125,
			"isDeleted": false,
			"id": "Qk-XdrT_1Fqx4qUr0cTgb",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -259.34332459332154,
			"y": 1838.8295535368047,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 473.5546875,
			"height": 1.1328125,
			"seed": 2045349757,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664174979045,
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
					473.5546875,
					1.1328125
				]
			]
		},
		{
			"type": "line",
			"version": 346,
			"versionNonce": 1748253149,
			"isDeleted": false,
			"id": "adwEF3dzi6LKe7sbSv6LG",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -248.26519959332154,
			"y": 2042.1381472868047,
			"strokeColor": "#087f5b",
			"backgroundColor": "transparent",
			"width": 473.5546875,
			"height": 1.1328125,
			"seed": 1076104189,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664174997948,
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
					473.5546875,
					1.1328125
				]
			]
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#5f3dc4",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "hachure",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "dashed",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 4,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 2868.037335009988,
		"scrollY": 851.8683631298618,
		"zoom": {
			"value": 0.30000000000000004
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