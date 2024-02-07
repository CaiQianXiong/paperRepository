---

excalidraw-plugin: parsed
alias: MetaEmbedding
发布时间: 2019-08-25
出品方: 院校
价值: ⭐⭐⭐⭐⭐
环节: 

---
关键词:: #Reco/新物料冷启 , #ML/MetaLearning 
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
此文的工作概述
- learn a ad id embedding initializer
- 喂进ad的contents，就能得到一个比较好的ad id embedding ^GFqX9wMP

背景与问题 ^w0yTZBK4

从meta learning的角度重新组织问题 ^Mb8UW5lV

每个ad当成一个task，第i个ad有它自己的模型 ^YuSysmXY

的参数有两部分 ^fazlvPze

-    是共享的 ^yzqaaWVx

- 每个ad id embedding是每个ad，也就是每个task所独有的 ^Ys2wOryv

训练过程 ^DEwl38if

第1步
用Da模拟某个old ad在它还是new ad时 ^QlOijqWz

对于每个old ad，从它的samples中提取两个batch ^BAuiDGJP

表示第i个old ad id的初值
是第i个old ad的属性u[i]的函数 ^kt4OzmZL

这里的aj表示，第i个ad的a部分训练数据中的第j条样本 ^0uglZ5Ga

在第i个ad的a部分数据上计算出一个loss ^YsA5s5U4

第2步
用Db来模拟某个old ad被warmup的过程 ^98vO4727

更新一步 ^BKekg2df

init embedding要为cold-start时的loss负责 ^GiXsSLte

init embedding也要为warmup时的loss负责,越快越好 ^PTe0VS5d

相同项可以提取出来 ^YwvJf9mD

是已经由old ad已经训练好的
在meta-learning阶段是固定不变的 ^uURfFcUN

两个loss是在不同batch计算出来的 ^ylodYEFt

[[李宏毅讲MAML#^95ew2y|MAML中是忽略了二阶导数的]] ^9ARZY5EK

Meta-Embedding Generator
既可以离线训练
也可以拿在线训练（当一部分new ad变成old ad） ^4rm0Qtmp

h函数如何设计 ^VmjBDezb

那些ad feature的embedding可以从base model REUSE
而且frozen ^6uMSKF3k

只有最后一层的dense才是h中的learnable weights 'w' ^EtA83XrT

base model:由old ad训练出来的CTR模型 ^VAr9jJgS

- 纯cold时，要求initial embedding就很好了
- warm up时，这样初始化的id embedding能够加速model fitting ^pRL1vO1O

[[李宏毅讲MAML#^ha2pmr|MAML不关心初值时的性能，只关心fine-tune后的性能]] ^vlKNUThf

与MAML不同之处 ^mPvYsbZN

这里认为初值对纯冷启非常重要，也要考虑这个loss ^q7Vr7esb

步长 ^azAQzjQZ

假如第二次采样到了相同的task
也就是相同的old item
old item id embedding不能继承上次训练的成果
而是重新初始化 ^xhIEAzQK


# Embedded files
50196a03bee4d4496fe7976aad53ce9e9ae42c1f: $$g_{[i]}$$
123dd9c81ac6298b4ccde0d490650c015c587621: $$g_{[i]}$$
23a1bc7641d864ae0baaa0372abc0782da8d4607: $$\theta$$
6dcba532962d580a42e2d77b86f0dbe83f86bcde: $$\theta$$
cc93ee00d7d2aee2b1ca672995fb24b11d95b300: [[Pasted Image 20220923120216_814.png]]
46d154dd73a96d032f541f275de7a8f9cba24acd: [[Pasted Image 20220923174009_943.png]]
07bcbf33bd04b3c721008e8418f87b760fb35a68: [[Pasted Image 20220923174318_999.png]]
645b400a35b29d37990b00fe8ee06005b917e643: [[Pasted Image 20220923174530_905.png]]
77002092a5f5ba70f823f62ae51d75c531bfaece: [[Pasted Image 20220923175222_863.png]]
9e220cbca45dbf89fb5e6cd3e2030a1c025e25a2: [[Pasted Image 20220923175747_886.png]]
1d40e642bf881d8fdbecf10a7ccc40562ab4b635: [[Pasted Image 20220923183952_212.png]]
add5b43517e6fcedb0fbacfc6c09f1e0f0654079: [[Pasted Image 20220923184134_074.png]]
683fae1520b961ce6f229b3a0736e9f9f67f7a23: [[Pasted Image 20220923184222_210.png]]
e997d7e5257473faf697bf895935a28d1763c746: [[Pasted Image 20220923184503_144.png]]
02ff8c953a7528a277a3c46b21d2bc605cf9fea7: [[Pasted Image 20220923202952_416.png]]
16c79c506882d0d277f0a9a2edf91bb227eeac06: [[Pasted Image 20220923203127_456.png]]
8fdbb935bae754b7960da89299acfbcce5402836: [[Pasted Image 20220923203158_340.png]]
2b2e9c3784c4fa59824ffb612451a1fd63830272: [[Pasted Image 20220923203716_389.png]]
3a6154c21cc52f0c162318c09f79756660ce0d68: [[Pasted Image 20220923203803_011.png]]
860dddd6470c26604a1ef027110297dba7edbe5a: [[Pasted Image 20220923203935_247.png]]
8c339f79949b0652feabef25f9eb0c262299aee4: [[Pasted Image 20220923204236_354.png]]
0125d9edc9d54291b8b2355b3826e5c953aae5c7: [[Pasted Image 20220923210058_378.png]]
bbdbb84376888ebd1554041f49ce5478e162fb28: [[Pasted Image 20220923210420_411.png]]
e699b1d10e5f2ba2cad95e3058d97fc66e14054f: [[Pasted Image 20220923210944_188.png]]
5cf6466307f2f9e253bc15ec95adca0d1a0fa2ad: [[Pasted Image 20220923211759_211.png]]

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/1.9.1",
	"elements": [
		{
			"type": "rectangle",
			"version": 64,
			"versionNonce": 235076813,
			"isDeleted": false,
			"id": "D3wQH6UU",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -716.2488731971152,
			"y": 631.9695068359403,
			"strokeColor": "white",
			"backgroundColor": "#e64980",
			"width": 344,
			"height": 106,
			"seed": 17376,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "xhIEAzQK"
				}
			],
			"updated": 1684216683775,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 152,
			"versionNonce": 1639939486,
			"isDeleted": false,
			"id": "w08Ggr4Y",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -286.98422475961524,
			"y": -487.18554687499454,
			"strokeColor": "transparent",
			"backgroundColor": "#4c6ef5",
			"width": 292,
			"height": 56,
			"seed": 90332,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "VAr9jJgS"
				},
				{
					"id": "_UgWSt4BURW8VKxjSRYIZ",
					"type": "arrow"
				},
				{
					"id": "64iqYoK-y6qqPZi7onOiY",
					"type": "arrow"
				}
			],
			"updated": 1664845375262,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "image",
			"version": 176,
			"versionNonce": 478645626,
			"isDeleted": false,
			"id": "eDRek0fZWni9e3cd7x4FE",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1227.423565453069,
			"y": 419.8871694711594,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 471.39523369842084,
			"height": 425.5945009773485,
			"seed": 1395420297,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "AykzGQsKyOcVAsZBx2pkd",
					"type": "arrow"
				}
			],
			"updated": 1664089086543,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "0125d9edc9d54291b8b2355b3826e5c953aae5c7",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 550,
			"versionNonce": 974597662,
			"isDeleted": false,
			"id": "D0nAunB6",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 217.2780949519232,
			"y": 628.277644230769,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 251,
			"height": 56,
			"seed": 84003,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "9ARZY5EK"
				},
				{
					"id": "5vFGV7cIAZcKvDwdrll6y",
					"type": "arrow"
				}
			],
			"updated": 1664845375265,
			"link": "",
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 344,
			"versionNonce": 1367849566,
			"isDeleted": false,
			"id": "qvaXprN3",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -510.830078125,
			"y": 31.26953125,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 130,
			"height": 102,
			"seed": 6899,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "GiXsSLte"
				}
			],
			"updated": 1664845375266,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "image",
			"version": 313,
			"versionNonce": 841665126,
			"isDeleted": false,
			"id": "ZwYyBbmbYa1PeaFtu_39L",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -740.01953125,
			"y": -809.93359375,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 78.61328125,
			"height": 78.61328125,
			"seed": 1125879610,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086543,
			"link": "https://github.com/Feiyang/MetaEmbedding",
			"locked": false,
			"status": "pending",
			"fileId": "cc93ee00d7d2aee2b1ca672995fb24b11d95b300",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 427,
			"versionNonce": 1921644282,
			"isDeleted": false,
			"id": "NAxc73aGKjDLTE5WmYH-o",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -885.41015625,
			"y": -660.7282169117648,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 448.58984375000006,
			"height": 143.1529354319853,
			"seed": 1027215817,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086543,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "46d154dd73a96d032f541f275de7a8f9cba24acd",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 253,
			"versionNonce": 1282444710,
			"isDeleted": false,
			"id": "w0yTZBK4",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -879.125,
			"y": -692.41796875,
			"strokeColor": "#2b8a3e",
			"backgroundColor": "transparent",
			"width": 100,
			"height": 24,
			"seed": 2074437545,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086543,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 4,
			"text": "背景与问题",
			"rawText": "背景与问题",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "背景与问题",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "image",
			"version": 419,
			"versionNonce": 1524959162,
			"isDeleted": false,
			"id": "lX4s9gxs5HlX8t9EO6XfA",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -407.8359375,
			"y": -656.0625,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 455.03906250000006,
			"height": 98.9508020525148,
			"seed": 1412178889,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "64iqYoK-y6qqPZi7onOiY",
					"type": "arrow"
				}
			],
			"updated": 1664089086543,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "07bcbf33bd04b3c721008e8418f87b760fb35a68",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 577,
			"versionNonce": 2006706406,
			"isDeleted": false,
			"id": "ewtnr5AZEfrQ_I7b6vOEd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -873.484375,
			"y": -509.859375,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 512.83203125,
			"height": 112.08604275823353,
			"seed": 1950643209,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "645b400a35b29d37990b00fe8ee06005b917e643",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 510,
			"versionNonce": 1327497338,
			"isDeleted": false,
			"id": "gyXngkkSLzDVitheankxa",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -892.80078125,
			"y": -693.40234375,
			"strokeColor": "#2b8a3e",
			"backgroundColor": "transparent",
			"width": 1027.95703125,
			"height": 572.26953125,
			"seed": 1283236169,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false
		},
		{
			"type": "line",
			"version": 292,
			"versionNonce": 859429926,
			"isDeleted": false,
			"id": "_3-VpCRhbvkiI6Br3Wo9O",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -870.82421875,
			"y": -386.84765625,
			"strokeColor": "#2b8a3e",
			"backgroundColor": "transparent",
			"width": 999.09765625,
			"height": 4.83203125,
			"seed": 1049214247,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
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
					999.09765625,
					4.83203125
				]
			]
		},
		{
			"type": "text",
			"version": 139,
			"versionNonce": 51808050,
			"isDeleted": false,
			"id": "Mb8UW5lV",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -865.1640625,
			"y": -371.40625,
			"strokeColor": "#2b8a3e",
			"backgroundColor": "transparent",
			"width": 263.58392333984375,
			"height": 19.2,
			"seed": 1478712137,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741350,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "从meta learning的角度重新组织问题",
			"rawText": "从meta learning的角度重新组织问题",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "从meta learning的角度重新组织问题",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 426,
			"versionNonce": 87517030,
			"isDeleted": false,
			"id": "HNLTrOPFucy1Nfdj5X6zd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -865.911233836207,
			"y": -339.1171875,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 388.0490301724138,
			"height": 169.9912669939577,
			"seed": 89366313,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "77002092a5f5ba70f823f62ae51d75c531bfaece",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 126,
			"versionNonce": 1155415278,
			"isDeleted": false,
			"id": "YuSysmXY",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -413.3984375,
			"y": -368.42578125,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 328.2879638671875,
			"height": 19.2,
			"seed": 197880231,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741350,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "每个ad当成一个task，第i个ad有它自己的模型",
			"rawText": "每个ad当成一个task，第i个ad有它自己的模型",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "每个ad当成一个task，第i个ad有它自己的模型",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 199,
			"versionNonce": 769755814,
			"isDeleted": false,
			"id": "IE4zizUo",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -83.07926657725982,
			"y": -366.94887986734716,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 24.56478315451963,
			"height": 15.632134734694311,
			"seed": 85813,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "123dd9c81ac6298b4ccde0d490650c015c587621",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 319,
			"versionNonce": 1120819898,
			"isDeleted": false,
			"id": "giIE8mCEutLAOZMT9mWpd",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -408.80973532725983,
			"y": -328.78872361734716,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 24.56478315451963,
			"height": 15.632134734694311,
			"seed": 129059433,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "50196a03bee4d4496fe7976aad53ce9e9ae42c1f",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 169,
			"versionNonce": 1541752294,
			"isDeleted": false,
			"id": "fazlvPze",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -375.484375,
			"y": -332.07421875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 112,
			"height": 19.2,
			"seed": 136651367,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "的参数有两部分",
			"rawText": "的参数有两部分",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "的参数有两部分",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 329,
			"versionNonce": 879162234,
			"isDeleted": false,
			"id": "I3GvzL1W",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -349.3639747048676,
			"y": -297.8479755414549,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 13.58732440973515,
			"height": 11.039701082909808,
			"seed": 63253,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "23a1bc7641d864ae0baaa0372abc0782da8d4607",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 177,
			"versionNonce": 753062130,
			"isDeleted": false,
			"id": "yzqaaWVx",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -370.07421875,
			"y": -302.3125,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 91.99995422363281,
			"height": 19.2,
			"seed": 1208850217,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741351,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "-    是共享的",
			"rawText": "-    是共享的",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "-    是共享的",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 215,
			"versionNonce": 606367534,
			"isDeleted": false,
			"id": "Ys2wOryv",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -373.4296875,
			"y": -276.734375,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 424.78387451171875,
			"height": 19.2,
			"seed": 911796167,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741351,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "- 每个ad id embedding是每个ad，也就是每个task所独有的",
			"rawText": "- 每个ad id embedding是每个ad，也就是每个task所独有的",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "- 每个ad id embedding是每个ad，也就是每个task所独有的",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 336,
			"versionNonce": 1653441638,
			"isDeleted": false,
			"id": "3mLBsqKNHkToA6t3UBW4-",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -378.1147564827128,
			"y": -256.71875,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 469.22803773271266,
			"height": 130.3026160912112,
			"seed": 1066297481,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "9e220cbca45dbf89fb5e6cd3e2030a1c025e25a2",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 267,
			"versionNonce": 2051529978,
			"isDeleted": false,
			"id": "DEwl38if",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1133.501953125,
			"y": 62.66796875,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 112,
			"height": 33.6,
			"seed": 1314811623,
			"groupIds": [
				"eitmGxJQTgboYEzmc8JTY"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086544,
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
			"version": 450,
			"versionNonce": 1912595122,
			"isDeleted": false,
			"id": "BAuiDGJP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1135.982421875,
			"y": 108.6328125,
			"strokeColor": "#000000",
			"backgroundColor": "#fab005",
			"width": 357.0399169921875,
			"height": 19.2,
			"seed": 404949481,
			"groupIds": [
				"eitmGxJQTgboYEzmc8JTY"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741352,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "对于每个old ad，从它的samples中提取两个batch",
			"rawText": "对于每个old ad，从它的samples中提取两个batch",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "对于每个old ad，从它的samples中提取两个batch",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 593,
			"versionNonce": 1039543738,
			"isDeleted": false,
			"id": "kLeoNbaiWXSjEIqN78fji",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1141.5939870845375,
			"y": 138.08203125,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 489.75609645953756,
			"height": 126.64843749999999,
			"seed": 43705129,
			"groupIds": [
				"eitmGxJQTgboYEzmc8JTY"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "ZyoX9nI0eNTlPy6PHlsiw",
					"type": "arrow"
				},
				{
					"id": "YJ94cV_pdT5GDE4qz4OFM",
					"type": "arrow"
				},
				{
					"id": "69xBTO_K9uy06NcB1Y_5X",
					"type": "arrow"
				},
				{
					"id": "AykzGQsKyOcVAsZBx2pkd",
					"type": "arrow"
				}
			],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "1d40e642bf881d8fdbecf10a7ccc40562ab4b635",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 971,
			"versionNonce": 2073869982,
			"isDeleted": false,
			"id": "ZeBlS-sl8LLc4e0NrUk6r",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -519.927734375,
			"y": 191.546875,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 169,
			"height": 79,
			"seed": 1117543401,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "98vO4727",
					"type": "text"
				}
			],
			"updated": 1664845375267,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 1091,
			"versionNonce": 889099431,
			"isDeleted": false,
			"id": "98vO4727",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -514.927734375,
			"y": 196.546875,
			"strokeColor": "#000000",
			"backgroundColor": "white",
			"width": 140,
			"height": 57.599999999999994,
			"seed": 148271911,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553869,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "第2步\n用Db来模拟某个old \nad被warmup的过程",
			"rawText": "第2步\n用Db来模拟某个old ad被warmup的过程",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "ZeBlS-sl8LLc4e0NrUk6r",
			"originalText": "第2步\n用Db来模拟某个old ad被warmup的过程",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "image",
			"version": 199,
			"versionNonce": 975099430,
			"isDeleted": false,
			"id": "YL9HTlWojUTANQCZI4Ecw",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -292.18154296875,
			"y": 186.765625,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 161.26943359375,
			"height": 62.62890625,
			"seed": 1553841609,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "02ff8c953a7528a277a3c46b21d2bc605cf9fea7",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 76,
			"versionNonce": 477988666,
			"isDeleted": false,
			"id": "BKekg2df",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -123.244140625,
			"y": 203.36328125,
			"strokeColor": "#862e9c",
			"backgroundColor": "#fab005",
			"width": 64,
			"height": 19.2,
			"seed": 625245769,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "更新一步",
			"rawText": "更新一步",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "更新一步",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 453,
			"versionNonce": 426582374,
			"isDeleted": false,
			"id": "do1ifvtmI-kBRL82yZkZh",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -299.458984375,
			"y": 246.95703125,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 411.546875,
			"height": 46.20500163185379,
			"seed": 312388263,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "16c79c506882d0d277f0a9a2edf91bb227eeac06",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 345,
			"versionNonce": 1484558330,
			"isDeleted": false,
			"id": "CgOv_roJ0iXZBEA5FFRkc",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -293.982421875,
			"y": 299.5,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 379.4296875,
			"height": 60.5875748502994,
			"seed": 1615839721,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "8fdbb935bae754b7960da89299acfbcce5402836",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 876,
			"versionNonce": 1560754910,
			"isDeleted": false,
			"id": "1SQJ3VXZ",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -517.806640625,
			"y": -47.015625,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 169,
			"height": 79,
			"seed": 27122,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "QlOijqWz"
				}
			],
			"updated": 1664845375268,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 972,
			"versionNonce": 257928521,
			"isDeleted": false,
			"id": "QlOijqWz",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -512.806640625,
			"y": -42.015625,
			"strokeColor": "#000000",
			"backgroundColor": "white",
			"width": 140.859375,
			"height": 57.599999999999994,
			"seed": 582888263,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553870,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "第1步\n用Da模拟某个old \nad在它还是new ad时",
			"rawText": "第1步\n用Da模拟某个old ad在它还是new ad时",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "1SQJ3VXZ",
			"originalText": "第1步\n用Da模拟某个old ad在它还是new ad时",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "image",
			"version": 237,
			"versionNonce": 1778430238,
			"isDeleted": false,
			"id": "QFONi6qaRTzEYZPFYdQrD",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -322.017578125,
			"y": -47.80078125,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 173.9278706395349,
			"height": 45.8828125,
			"seed": 219596423,
			"groupIds": [
				"2yk-ylQCiblpXZA8f_BoH"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664845795983,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "add5b43517e6fcedb0fbacfc6c09f1e0f0654079",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 378,
			"versionNonce": 947189698,
			"isDeleted": false,
			"id": "JS69_3vIAeytkBCz3YR61",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -293.50337751521624,
			"y": -22.11049102073492,
			"strokeColor": "white",
			"backgroundColor": "white",
			"width": 8.97399121661519,
			"height": 7.906949065452399,
			"seed": 123096359,
			"groupIds": [
				"2yk-ylQCiblpXZA8f_BoH"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1664845795983,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 467,
			"versionNonce": 1867046238,
			"isDeleted": false,
			"id": "iSSC0Z2zeiMZUymsRQms1",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -174.20514732183412,
			"y": -27.843758516533427,
			"strokeColor": "white",
			"backgroundColor": "white",
			"width": 8.97399121661519,
			"height": 7.906949065452399,
			"seed": 471240071,
			"groupIds": [
				"2yk-ylQCiblpXZA8f_BoH"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1664845795983,
			"link": null,
			"locked": false
		},
		{
			"type": "image",
			"version": 444,
			"versionNonce": 711662138,
			"isDeleted": false,
			"id": "-kF3D-aMZYbNhBtjtj0Yk",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -316.85742187499994,
			"y": 16.0703125,
			"strokeColor": "transparent",
			"backgroundColor": "white",
			"width": 482.20312499999994,
			"height": 45.86810213414634,
			"seed": 954722407,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086544,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "683fae1520b961ce6f229b3a0736e9f9f67f7a23",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 280,
			"versionNonce": 409622894,
			"isDeleted": false,
			"id": "kt4OzmZL",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -139.189453125,
			"y": -45.33984375,
			"strokeColor": "#862e9c",
			"backgroundColor": "white",
			"width": 219.45591735839844,
			"height": 38.4,
			"seed": 174525225,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741352,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "表示第i个old ad id的初值\n是第i个old ad的属性u[i]的函数",
			"rawText": "表示第i个old ad id的初值\n是第i个old ad的属性u[i]的函数",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "表示第i个old ad id的初值\n是第i个old ad的属性u[i]的函数",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "text",
			"version": 132,
			"versionNonce": 1024769138,
			"isDeleted": false,
			"id": "0uglZ5Ga",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -310.556640625,
			"y": 63.62890625,
			"strokeColor": "#862e9c",
			"backgroundColor": "white",
			"width": 386.63995361328125,
			"height": 19.2,
			"seed": 1437343879,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741353,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "这里的aj表示，第i个ad的a部分训练数据中的第j条样本",
			"rawText": "这里的aj表示，第i个ad的a部分训练数据中的第j条样本",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "这里的aj表示，第i个ad的a部分训练数据中的第j条样本",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "image",
			"version": 267,
			"versionNonce": 1842184614,
			"isDeleted": false,
			"id": "iTVArbjkfB3qeIgvIwCsf",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -330.185546875,
			"y": 94.42578125,
			"strokeColor": "transparent",
			"backgroundColor": "white",
			"width": 389.203125,
			"height": 63.322730654761905,
			"seed": 1571528583,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "e997d7e5257473faf697bf895935a28d1763c746",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 212,
			"versionNonce": 1492712366,
			"isDeleted": false,
			"id": "YsA5s5U4",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 63.291015625,
			"y": 110.00390625,
			"strokeColor": "#862e9c",
			"backgroundColor": "white",
			"width": 285.32794189453125,
			"height": 19.2,
			"seed": 425814087,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741353,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "在第i个ad的a部分数据上计算出一个loss",
			"rawText": "在第i个ad的a部分数据上计算出一个loss",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "在第i个ad的a部分数据上计算出一个loss",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "rectangle",
			"version": 307,
			"versionNonce": 1228561638,
			"isDeleted": false,
			"id": "xfRf6jthHb32G8PzaAzl7",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -526.873046875,
			"y": -63.14453125,
			"strokeColor": "#862e9c",
			"backgroundColor": "transparent",
			"width": 880.7265625,
			"height": 226.86328125,
			"seed": 1272694087,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "ZyoX9nI0eNTlPy6PHlsiw",
					"type": "arrow"
				}
			],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 383,
			"versionNonce": 1100341865,
			"isDeleted": false,
			"id": "GiXsSLte",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -505.830078125,
			"y": 36.26953125,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 118.1953125,
			"height": 76.8,
			"seed": 84476071,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553868,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "init \nembedding要为co\nld-\nstart时的loss负责",
			"rawText": "init embedding要为cold-start时的loss负责",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "qvaXprN3",
			"originalText": "init embedding要为cold-start时的loss负责",
			"lineHeight": 1.2,
			"baseline": 72
		},
		{
			"type": "rectangle",
			"version": 478,
			"versionNonce": 1549283102,
			"isDeleted": false,
			"id": "T3to1n0fuCPXsUHMbdL8F",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -517.896484375,
			"y": 270.84765625,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 130,
			"height": 125,
			"seed": 1189132361,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "PTe0VS5d",
					"type": "text"
				}
			],
			"updated": 1664845375269,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 554,
			"versionNonce": 1504929735,
			"isDeleted": false,
			"id": "PTe0VS5d",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -512.896484375,
			"y": 275.84765625,
			"strokeColor": "#a61e4d",
			"backgroundColor": "transparent",
			"width": 119.09375,
			"height": 76.8,
			"seed": 517770439,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553871,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "init \nembedding也要为\nwarmup时的loss\n负责,越快越好",
			"rawText": "init embedding也要为warmup时的loss负责,越快越好",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "T3to1n0fuCPXsUHMbdL8F",
			"originalText": "init embedding也要为warmup时的loss负责,越快越好",
			"lineHeight": 1.2,
			"baseline": 72
		},
		{
			"type": "rectangle",
			"version": 402,
			"versionNonce": 818469734,
			"isDeleted": false,
			"id": "6LdLFjM3-tvXoMdALFgsp",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -523.865234375,
			"y": 187.94140625,
			"strokeColor": "#862e9c",
			"backgroundColor": "transparent",
			"width": 641.53125,
			"height": 178.59765625,
			"seed": 2004282183,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "YJ94cV_pdT5GDE4qz4OFM",
					"type": "arrow"
				}
			],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 344,
			"versionNonce": 1095857658,
			"isDeleted": false,
			"id": "2GIdnJTdd9eTw_FZiJEkr",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -22.841796875,
			"y": 32.740234375,
			"strokeColor": "transparent",
			"backgroundColor": "#fa5252",
			"width": 15.3984375,
			"height": 22.769531249999986,
			"seed": 1603888295,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "PiuMHvEOYtLRhhP2IIKHf",
					"type": "arrow"
				},
				{
					"id": "XTU-3ZblzwfciLTz1COv0",
					"type": "arrow"
				}
			],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 507,
			"versionNonce": 1327791782,
			"isDeleted": false,
			"id": "g7w502VyURWtQge9QE0be",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -52.669921875,
			"y": 262.708984375,
			"strokeColor": "transparent",
			"backgroundColor": "#fa5252",
			"width": 15.3984375,
			"height": 22.769531249999986,
			"seed": 416628423,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "PiuMHvEOYtLRhhP2IIKHf",
					"type": "arrow"
				},
				{
					"id": "fk71vvwpUdpnDWGyt_7eC",
					"type": "arrow"
				}
			],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 241,
			"versionNonce": 1711315806,
			"isDeleted": false,
			"id": "sv1Xz58L",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 185.615234375,
			"y": 221.1953125,
			"strokeColor": "transparent",
			"backgroundColor": "#e64980",
			"width": 254,
			"height": 79,
			"seed": 25874,
			"groupIds": [
				"SowXirst3FV5MY55rwepe"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "uURfFcUN"
				}
			],
			"updated": 1664845375270,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "image",
			"version": 468,
			"versionNonce": 1084864998,
			"isDeleted": false,
			"id": "BFBPajdB",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 172.28641592013244,
			"y": 233.6442119585451,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 13.58732440973515,
			"height": 11.039701082909808,
			"seed": 53326,
			"groupIds": [
				"SowXirst3FV5MY55rwepe"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "XTU-3ZblzwfciLTz1COv0",
					"type": "arrow"
				},
				{
					"id": "fk71vvwpUdpnDWGyt_7eC",
					"type": "arrow"
				}
			],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "6dcba532962d580a42e2d77b86f0dbe83f86bcde",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 385,
			"versionNonce": 1533877289,
			"isDeleted": false,
			"id": "uURfFcUN",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 190.615234375,
			"y": 226.1953125,
			"strokeColor": "white",
			"backgroundColor": "#40c057",
			"width": 234.7353515625,
			"height": 38.71735537190082,
			"seed": 610656327,
			"groupIds": [
				"SowXirst3FV5MY55rwepe"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553872,
			"link": null,
			"locked": false,
			"fontSize": 16.132231404958677,
			"fontFamily": 4,
			"text": "是已经由old ad已经训练好的\n在meta-learning阶段是固定不变的",
			"rawText": "是已经由old ad已经训练好的\n在meta-learning阶段是固定不变的",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "sv1Xz58L",
			"originalText": "是已经由old ad已经训练好的\n在meta-learning阶段是固定不变的",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 384,
			"versionNonce": 476417318,
			"isDeleted": false,
			"id": "XTU-3ZblzwfciLTz1COv0",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -11.492245116090686,
			"y": 59.6875,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#e64980",
			"width": 188.35040420885872,
			"height": 168.91015625,
			"seed": 595856361,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "2GIdnJTdd9eTw_FZiJEkr",
				"focus": 0.6714904787951482,
				"gap": 4.177734375000014
			},
			"endBinding": {
				"elementId": "BFBPajdB",
				"focus": 0.7154427412413806,
				"gap": 3.06640625
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
					188.35040420885872,
					168.91015625
				]
			]
		},
		{
			"type": "arrow",
			"version": 464,
			"versionNonce": 1885454394,
			"isDeleted": false,
			"id": "fk71vvwpUdpnDWGyt_7eC",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -48.857421875,
			"y": 286.63671875,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#e64980",
			"width": 224.546875,
			"height": 42.56640625,
			"seed": 452694695,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "g7w502VyURWtQge9QE0be",
				"focus": 1.0859115927861867,
				"gap": 1.158203125
			},
			"endBinding": {
				"elementId": "BFBPajdB",
				"focus": -0.7098040823838176,
				"gap": 4.72265625
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
					181.73046875,
					7.31640625
				],
				[
					224.546875,
					-35.25
				]
			]
		},
		{
			"type": "arrow",
			"version": 131,
			"versionNonce": 264566886,
			"isDeleted": false,
			"id": "ZyoX9nI0eNTlPy6PHlsiw",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -630.939453125,
			"y": 143.16015625,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 99.44921875,
			"height": 65.7265625,
			"seed": 2073066791,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "kLeoNbaiWXSjEIqN78fji",
				"focus": 0.5214258027911342,
				"gap": 20.8984375
			},
			"endBinding": {
				"elementId": "xfRf6jthHb32G8PzaAzl7",
				"focus": 0.6599834204458483,
				"gap": 4.6171875
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
					99.44921875,
					-65.7265625
				]
			]
		},
		{
			"type": "arrow",
			"version": 117,
			"versionNonce": 1970383098,
			"isDeleted": false,
			"id": "YJ94cV_pdT5GDE4qz4OFM",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -634.865234375,
			"y": 221.97265625,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 93.96484375,
			"height": 20.51487941725742,
			"seed": 921976903,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "kLeoNbaiWXSjEIqN78fji",
				"focus": -0.3135237545599504,
				"gap": 16.97265625
			},
			"endBinding": {
				"elementId": "6LdLFjM3-tvXoMdALFgsp",
				"focus": -0.24476039983931433,
				"gap": 17.03515625
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
					93.96484375,
					20.51487941725742
				]
			]
		},
		{
			"type": "arrow",
			"version": 761,
			"versionNonce": 311753638,
			"isDeleted": false,
			"id": "69xBTO_K9uy06NcB1Y_5X",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": -774.3834741886208,
			"y": 276.66015625,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 447.67899702515905,
			"height": 199.21392159999306,
			"seed": 1395826535,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "kLeoNbaiWXSjEIqN78fji",
				"focus": 0.12089566930375088,
				"gap": 11.9296875
			},
			"endBinding": {
				"elementId": "QWlersifzOliykOWWLA8M",
				"focus": -0.12988703618821448,
				"gap": 9.59765625
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
					447.67899702515905,
					199.21392159999306
				]
			]
		},
		{
			"type": "rectangle",
			"version": 390,
			"versionNonce": 1824605155,
			"isDeleted": false,
			"id": "RM8zlgH4",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -820.6717247596152,
			"y": 522.6816406250055,
			"strokeColor": "transparent",
			"backgroundColor": "#228be6",
			"width": 361,
			"height": 88,
			"seed": 41455,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "4rm0Qtmp"
				}
			],
			"updated": 1684216683782,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 611,
			"versionNonce": 644210055,
			"isDeleted": false,
			"id": "9ARZY5EK",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 222.2780949519232,
			"y": 633.277644230769,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 238.35833740234375,
			"height": 38.72133891213389,
			"seed": 1387225289,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553866,
			"link": "[[李宏毅讲MAML#^95ew2y]]",
			"locked": false,
			"fontSize": 16.13389121338912,
			"fontFamily": 4,
			"text": "📍[[MAML中是忽略了二阶导数的\n]]",
			"rawText": "[[李宏毅讲MAML#^95ew2y|MAML中是忽略了二阶导数的]]",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "D0nAunB6",
			"originalText": "📍[[MAML中是忽略了二阶导数的]]",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "image",
			"version": 379,
			"versionNonce": 2101555834,
			"isDeleted": false,
			"id": "QbSZtQGXpq_GP3VynxrTW",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1236.1717247596152,
			"y": 852.4863281250055,
			"strokeColor": "transparent",
			"backgroundColor": "#fd7e14",
			"width": 709.6640625000001,
			"height": 190.72221679687505,
			"seed": 1987007367,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "bbdbb84376888ebd1554041f49ce5478e162fb28",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "arrow",
			"version": 95,
			"versionNonce": 1369098790,
			"isDeleted": false,
			"id": "AykzGQsKyOcVAsZBx2pkd",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -944.3006310096154,
			"y": 279.1816406250055,
			"strokeColor": "#a61e4d",
			"backgroundColor": "#fd7e14",
			"width": 18.29614716821027,
			"height": 128.07812500000006,
			"seed": 1225963879,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "kLeoNbaiWXSjEIqN78fji",
				"focus": 0.1436259711324952,
				"gap": 14.451171875005514
			},
			"endBinding": {
				"elementId": "eDRek0fZWni9e3cd7x4FE",
				"focus": -0.01154872837745504,
				"gap": 12.627403846153811
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
					-18.29614716821027,
					128.07812500000006
				]
			]
		},
		{
			"type": "text",
			"version": 586,
			"versionNonce": 849401575,
			"isDeleted": false,
			"id": "4rm0Qtmp",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -815.6717247596152,
			"y": 527.6816406250055,
			"strokeColor": "white",
			"backgroundColor": "#fd7e14",
			"width": 343.224365234375,
			"height": 57.93008595988538,
			"seed": 590529511,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553873,
			"link": null,
			"locked": false,
			"fontSize": 16.091690544412607,
			"fontFamily": 4,
			"text": "Meta-Embedding Generator\n既可以离线训练\n也可以拿在线训练（当一部分new ad变成old ad）",
			"rawText": "Meta-Embedding Generator\n既可以离线训练\n也可以拿在线训练（当一部分new ad变成old ad）",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "RM8zlgH4",
			"originalText": "Meta-Embedding Generator\n既可以离线训练\n也可以拿在线训练（当一部分new ad变成old ad）",
			"lineHeight": 1.2,
			"baseline": 53
		},
		{
			"type": "text",
			"version": 202,
			"versionNonce": 502216873,
			"isDeleted": false,
			"id": "VAr9jJgS",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -281.98422475961524,
			"y": -482.18554687499454,
			"strokeColor": "white",
			"backgroundColor": "#228be6",
			"width": 275.53125,
			"height": 19.2,
			"seed": 1495799017,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553864,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "base model:由old ad训练出来的CTR模型",
			"rawText": "base model:由old ad训练出来的CTR模型",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "w08Ggr4Y",
			"originalText": "base model:由old ad训练出来的CTR模型",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 290,
			"versionNonce": 1353622510,
			"isDeleted": false,
			"id": "_UgWSt4BURW8VKxjSRYIZ",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 296.42593149038476,
			"y": -227.23728098705124,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#4c6ef5",
			"width": 303.2291080538271,
			"height": 202.1279533879433,
			"seed": 1566989799,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684394740808,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "6uMSKF3k",
				"gap": 6.1640625,
				"focus": -0.810002872246136
			},
			"endBinding": {
				"elementId": "w08Ggr4Y",
				"gap": 1.8203125,
				"focus": -0.47575874625677217
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
					-303.2291080538271,
					-202.1279533879433
				]
			]
		},
		{
			"type": "arrow",
			"version": 166,
			"versionNonce": 1196422702,
			"isDeleted": false,
			"id": "64iqYoK-y6qqPZi7onOiY",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -100.3081100973844,
			"y": -500.34179687499454,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#4c6ef5",
			"width": 5.335704032222907,
			"height": 55.5078125,
			"seed": 110486375,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684394740809,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "w08Ggr4Y",
				"gap": 13.15625,
				"focus": 0.24696225159795454
			},
			"endBinding": {
				"elementId": "lX4s9gxs5HlX8t9EO6XfA",
				"gap": 1.2620885724907112,
				"focus": -0.38842325965563035
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
					5.335704032222907,
					-55.5078125
				]
			]
		},
		{
			"type": "image",
			"version": 318,
			"versionNonce": 1151782074,
			"isDeleted": false,
			"id": "uK2TqK-Ac0RwoqFL2iYJl",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 273.2771991790639,
			"y": -497.24414062499454,
			"strokeColor": "transparent",
			"backgroundColor": "#228be6",
			"width": 488.2844192216981,
			"height": 238.15098974309817,
			"seed": 343602503,
			"groupIds": [
				"bm6-BKiGkJA1u-CBHfqLI"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086545,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "e699b1d10e5f2ba2cad95e3058d97fc66e14054f",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "text",
			"version": 122,
			"versionNonce": 199777842,
			"isDeleted": false,
			"id": "VmjBDezb",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 310.00015024038476,
			"y": -529.2597656249945,
			"strokeColor": "#364fc7",
			"backgroundColor": "#228be6",
			"width": 131.75999450683594,
			"height": 24,
			"seed": 1901395177,
			"groupIds": [
				"bm6-BKiGkJA1u-CBHfqLI"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "bgvMJC2ZyJPnjunSf0vkH",
					"type": "arrow"
				}
			],
			"updated": 1684394741353,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 20,
			"fontFamily": 4,
			"text": "h函数如何设计",
			"rawText": "h函数如何设计",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "h函数如何设计",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 170,
			"versionNonce": 1274631662,
			"isDeleted": false,
			"id": "6uMSKF3k",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 302.58999399038476,
			"y": -235.22851562499454,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#228be6",
			"width": 397.8397521972656,
			"height": 38.4,
			"seed": 1230826503,
			"groupIds": [
				"bm6-BKiGkJA1u-CBHfqLI"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "GOoifiLh8H6L2M5elyXR6",
					"type": "arrow"
				},
				{
					"id": "_UgWSt4BURW8VKxjSRYIZ",
					"type": "arrow"
				}
			],
			"updated": 1684394741353,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "那些ad feature的embedding可以从base model REUSE\n而且frozen",
			"rawText": "那些ad feature的embedding可以从base model REUSE\n而且frozen",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "那些ad feature的embedding可以从base model REUSE\n而且frozen",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 206,
			"versionNonce": 1741260582,
			"isDeleted": false,
			"id": "GOoifiLh8H6L2M5elyXR6",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 349.83999399038476,
			"y": -326.21679687499454,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#228be6",
			"width": 26.19921875,
			"height": 89.22265625,
			"seed": 1118295143,
			"groupIds": [
				"bm6-BKiGkJA1u-CBHfqLI"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086546,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "6uMSKF3k",
				"focus": -0.5413607751248921,
				"gap": 1.765625
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
					26.19921875,
					89.22265625
				]
			]
		},
		{
			"type": "text",
			"version": 178,
			"versionNonce": 190957554,
			"isDeleted": false,
			"id": "EtA83XrT",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 296.66030649038476,
			"y": -180.60351562499454,
			"strokeColor": "#1864ab",
			"backgroundColor": "#228be6",
			"width": 390.4637756347656,
			"height": 19.2,
			"seed": 415514185,
			"groupIds": [
				"bm6-BKiGkJA1u-CBHfqLI"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "_k75iRkaA4RrmFGOLArPU",
					"type": "arrow"
				}
			],
			"updated": 1684394741354,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "只有最后一层的dense才是h中的learnable weights 'w'",
			"rawText": "只有最后一层的dense才是h中的learnable weights 'w'",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "只有最后一层的dense才是h中的learnable weights 'w'",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 245,
			"versionNonce": 10072678,
			"isDeleted": false,
			"id": "_k75iRkaA4RrmFGOLArPU",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 328.72280649038476,
			"y": -463.95507812499454,
			"strokeColor": "#1864ab",
			"backgroundColor": "#228be6",
			"width": 1.64453125,
			"height": 281.171875,
			"seed": 290051303,
			"groupIds": [
				"bm6-BKiGkJA1u-CBHfqLI"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086546,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "EtA83XrT",
				"focus": -0.8185980414238828,
				"gap": 2.1796875
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
					1.64453125,
					281.171875
				]
			]
		},
		{
			"type": "rectangle",
			"version": 219,
			"versionNonce": 886935290,
			"isDeleted": false,
			"id": "IBVFbQdtI_RBC7f49ioz7",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 268.12515024038476,
			"y": -532.0722656249945,
			"strokeColor": "#364fc7",
			"backgroundColor": "transparent",
			"width": 501.66796875,
			"height": 380.203125,
			"seed": 559163911,
			"groupIds": [
				"bm6-BKiGkJA1u-CBHfqLI"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1664089086546,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 84,
			"versionNonce": 1019377062,
			"isDeleted": false,
			"id": "Kudo-nEZgbqVmSMUoc5ol",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -252.27328725961524,
			"y": -42.42773437499454,
			"strokeColor": "#000000",
			"backgroundColor": "#e64980",
			"width": 41.66796875,
			"height": 35.94140625,
			"seed": 1953609767,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "bgvMJC2ZyJPnjunSf0vkH",
					"type": "arrow"
				}
			],
			"updated": 1664089086546,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "arrow",
			"version": 235,
			"versionNonce": 1057333178,
			"isDeleted": false,
			"id": "bgvMJC2ZyJPnjunSf0vkH",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -223.6413056120142,
			"y": -49.38867187499454,
			"strokeColor": "#000000",
			"backgroundColor": "#e64980",
			"width": 529.6951511105614,
			"height": 451.33596817690335,
			"seed": 91562631,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086546,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Kudo-nEZgbqVmSMUoc5ol",
				"focus": -0.5116899791052383,
				"gap": 6.9609375
			},
			"endBinding": {
				"elementId": "VmjBDezb",
				"focus": 0.6345672604959123,
				"gap": 4.234375
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
					529.6951511105614,
					-451.33596817690335
				]
			]
		},
		{
			"type": "image",
			"version": 208,
			"versionNonce": 1433456870,
			"isDeleted": false,
			"id": "C5Tbuj3MO41do-XHkxcvi",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -283.95681271852663,
			"y": 786.9888361150616,
			"strokeColor": "transparent",
			"backgroundColor": "#e64980",
			"width": 435.36268028846143,
			"height": 135.5620321856287,
			"seed": 665745641,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "FdQlDu3K_jBor3a0ODn3G",
					"type": "arrow"
				}
			],
			"updated": 1664089086546,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "5cf6466307f2f9e253bc15ec95adca0d1a0fa2ad",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "arrow",
			"version": 152,
			"versionNonce": 2068020006,
			"isDeleted": false,
			"id": "FdQlDu3K_jBor3a0ODn3G",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -122.6486073998077,
			"y": 494.91071111506164,
			"strokeColor": "#5c940d",
			"backgroundColor": "#82c91e",
			"width": 38.12964021127456,
			"height": 289.2109375000001,
			"seed": 1596654921,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664089086547,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "R4F7LSRJDTSH0Xf-kXuIB",
				"focus": -0.20019127792669208,
				"gap": 11.01171875
			},
			"endBinding": {
				"elementId": "C5Tbuj3MO41do-XHkxcvi",
				"focus": -0.45811555991419933,
				"gap": 2.867187500000057
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
					-38.12964021127456,
					289.2109375000001
				]
			]
		},
		{
			"type": "rectangle",
			"version": 42,
			"versionNonce": 2138823646,
			"isDeleted": false,
			"id": "ZN61Vzh8",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -602.9496011800652,
			"y": -768.7416326349385,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 456,
			"height": 79,
			"seed": 55110,
			"groupIds": [
				"AwriXaJQ4_2VgnIWZXX2O"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "pRL1vO1O"
				}
			],
			"updated": 1664845375271,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 654,
			"versionNonce": 712271918,
			"isDeleted": false,
			"id": "GFqX9wMP",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -598.8984375,
			"y": -833.82421875,
			"strokeColor": "#000000",
			"backgroundColor": "transparent",
			"width": 442.15985107421875,
			"height": 57.599999999999994,
			"seed": 683002810,
			"groupIds": [
				"AwriXaJQ4_2VgnIWZXX2O"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684394741354,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "此文的工作概述\n- learn a ad id embedding initializer\n- 喂进ad的contents，就能得到一个比较好的ad id embedding",
			"rawText": "此文的工作概述\n- learn a ad id embedding initializer\n- 喂进ad的contents，就能得到一个比较好的ad id embedding",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "此文的工作概述\n- learn a ad id embedding initializer\n- 喂进ad的contents，就能得到一个比较好的ad id embedding",
			"lineHeight": 1.2,
			"baseline": 54
		},
		{
			"type": "text",
			"version": 92,
			"versionNonce": 1705619209,
			"isDeleted": false,
			"id": "pRL1vO1O",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -597.9496011800652,
			"y": -763.7416326349385,
			"strokeColor": "#000000",
			"backgroundColor": "#82c91e",
			"width": 429.3934326171875,
			"height": 38.57297297297297,
			"seed": 109229223,
			"groupIds": [
				"AwriXaJQ4_2VgnIWZXX2O"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553875,
			"link": null,
			"locked": false,
			"fontSize": 16.07207207207207,
			"fontFamily": 4,
			"text": "- 纯cold时，要求initial embedding就很好了\n- warm up时，这样初始化的id embedding能够加速model fitting",
			"rawText": "- 纯cold时，要求initial embedding就很好了\n- warm up时，这样初始化的id embedding能够加速model fitting",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "ZN61Vzh8",
			"originalText": "- 纯cold时，要求initial embedding就很好了\n- warm up时，这样初始化的id embedding能够加速model fitting",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "rectangle",
			"version": 147,
			"versionNonce": 1108336237,
			"isDeleted": false,
			"id": "DskkTCHq",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 415.90737680288476,
			"y": 134.32888183594025,
			"strokeColor": "white",
			"backgroundColor": "#fa5252",
			"width": 379,
			"height": 33,
			"seed": 35956,
			"groupIds": [
				"tgV7ZqDC9-yLRIGcegWMD"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "q7Vr7esb"
				}
			],
			"updated": 1684216801505,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 374,
			"versionNonce": 238373603,
			"isDeleted": false,
			"id": "boELixbW",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 409.14272836538476,
			"y": 79.46169433594025,
			"strokeColor": "transparent",
			"backgroundColor": "#fab005",
			"width": 429,
			"height": 56,
			"seed": 9088,
			"groupIds": [
				"tgV7ZqDC9-yLRIGcegWMD"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "vlKNUThf"
				},
				{
					"id": "lTX-YPGpzzRQU7cyAPlCN",
					"type": "arrow"
				}
			],
			"updated": 1684216801505,
			"link": "",
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 607,
			"versionNonce": 858669257,
			"isDeleted": false,
			"id": "vlKNUThf",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 414.14272836538476,
			"y": 84.46169433594025,
			"strokeColor": "#000000",
			"backgroundColor": "#fab005",
			"width": 417.7882995605469,
			"height": 19.292086330935252,
			"seed": 1620523706,
			"groupIds": [
				"tgV7ZqDC9-yLRIGcegWMD"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553877,
			"link": "[[李宏毅讲MAML#^ha2pmr]]",
			"locked": false,
			"fontSize": 16.07673860911271,
			"fontFamily": 4,
			"text": "📍[[MAML不关心初值时的性能，只关心fine-tune后的性能]]",
			"rawText": "[[李宏毅讲MAML#^ha2pmr|MAML不关心初值时的性能，只关心fine-tune后的性能]]",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "boELixbW",
			"originalText": "📍[[MAML不关心初值时的性能，只关心fine-tune后的性能]]",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 138,
			"versionNonce": 864549298,
			"isDeleted": false,
			"id": "mPvYsbZN",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 420.86440805288476,
			"y": 57.05935058594025,
			"strokeColor": "#000000",
			"backgroundColor": "#fab005",
			"width": 127.1519775390625,
			"height": 19.2,
			"seed": 1348339174,
			"groupIds": [
				"tgV7ZqDC9-yLRIGcegWMD"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "lTX-YPGpzzRQU7cyAPlCN",
					"type": "arrow"
				}
			],
			"updated": 1684394741355,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "与MAML不同之处",
			"rawText": "与MAML不同之处",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "与MAML不同之处",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 280,
			"versionNonce": 1716740615,
			"isDeleted": false,
			"id": "q7Vr7esb",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 420.90737680288476,
			"y": 139.32888183594025,
			"strokeColor": "white",
			"backgroundColor": "#fab005",
			"width": 362.70257568359375,
			"height": 19.304632152588557,
			"seed": 486561850,
			"groupIds": [
				"tgV7ZqDC9-yLRIGcegWMD"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553876,
			"link": null,
			"locked": false,
			"fontSize": 16.087193460490465,
			"fontFamily": 4,
			"text": "这里认为初值对纯冷启非常重要，也要考虑这个loss",
			"rawText": "这里认为初值对纯冷启非常重要，也要考虑这个loss",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "DskkTCHq",
			"originalText": "这里认为初值对纯冷启非常重要，也要考虑这个loss",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "rectangle",
			"version": 346,
			"versionNonce": 1818792378,
			"isDeleted": false,
			"id": "3AYHjafOzZyR3H8v7S_KM",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -337.86215444711524,
			"y": 110.52810058594025,
			"strokeColor": "white",
			"backgroundColor": "#fd7e14",
			"width": 30.968749999999993,
			"height": 32.03125,
			"seed": 1514114170,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "lTX-YPGpzzRQU7cyAPlCN",
					"type": "arrow"
				}
			],
			"updated": 1664089255775,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "arrow",
			"version": 1029,
			"versionNonce": 1311687342,
			"isDeleted": false,
			"id": "lTX-YPGpzzRQU7cyAPlCN",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -299.70981069711524,
			"y": 127.29066906769125,
			"strokeColor": "#c92a2a",
			"backgroundColor": "#fd7e14",
			"width": 705.8046875,
			"height": 25.780396930818142,
			"seed": 694196454,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684394740821,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "3AYHjafOzZyR3H8v7S_KM",
				"gap": 7.18359375,
				"focus": 0.09498216457833489
			},
			"endBinding": {
				"elementId": "boELixbW",
				"gap": 3.0478515625,
				"focus": 0.387824066312411
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
					705.8046875,
					-25.780396930818142
				]
			]
		},
		{
			"type": "rectangle",
			"version": 12,
			"versionNonce": 1380595437,
			"isDeleted": false,
			"id": "LlpQ7hlK",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -261.45567908653857,
			"y": 401.59224759615387,
			"strokeColor": "transparent",
			"backgroundColor": "#fd7e14",
			"width": 257,
			"height": 49,
			"seed": 58557,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "ylodYEFt"
				}
			],
			"updated": 1684216683784,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "image",
			"version": 811,
			"versionNonce": 402481057,
			"isDeleted": false,
			"id": "IXOvvVofExRcvb0855t0m",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -297.41932091346166,
			"y": 449.5922475961539,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 207.32421875,
			"height": 34.39908562219731,
			"seed": 1984878951,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "CykqA3kS60XCjQ1MU_ppD",
					"type": "arrow"
				},
				{
					"id": "vlTkAyTJ7Tch_GOlcY301",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"status": "pending",
			"fileId": "2b2e9c3784c4fa59824ffb612451a1fd63830272",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 1014,
			"versionNonce": 2026022639,
			"isDeleted": false,
			"id": "2PH1vS2NjGQKvOFQ6G6N4",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -297.45838341346166,
			"y": 490.5688100961539,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 340.6328125,
			"height": 82.38130519701087,
			"seed": 98675433,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "3a6154c21cc52f0c162318c09f79756660ce0d68",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "image",
			"version": 942,
			"versionNonce": 1733477249,
			"isDeleted": false,
			"id": "pR_9aajQOIXEY1gy4oWob",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -300.11463341346166,
			"y": 608.5453725961539,
			"strokeColor": "transparent",
			"backgroundColor": "transparent",
			"width": 469.3125,
			"height": 73.11752717391305,
			"seed": 828339113,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "860dddd6470c26604a1ef027110297dba7edbe5a",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 716,
			"versionNonce": 2127425807,
			"isDeleted": false,
			"id": "e8E6GZ4pUmPf5pCVKI1As",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -60.93494591346166,
			"y": 499.4594350961539,
			"strokeColor": "transparent",
			"backgroundColor": "#fa5252",
			"width": 60.71875,
			"height": 75.16796874999999,
			"seed": 678427015,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "PiuMHvEOYtLRhhP2IIKHf",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 833,
			"versionNonce": 303082337,
			"isDeleted": false,
			"id": "l1oI-haKnq55fbJvcH51x",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -300.80213341346166,
			"y": 605.9574819711539,
			"strokeColor": "transparent",
			"backgroundColor": "#fa5252",
			"width": 60.71875,
			"height": 75.16796874999999,
			"seed": 1913263145,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "PiuMHvEOYtLRhhP2IIKHf",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "arrow",
			"version": 1913,
			"versionNonce": 1428879151,
			"isDeleted": false,
			"id": "PiuMHvEOYtLRhhP2IIKHf",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -69.63025841346166,
			"y": 570.1508413461539,
			"strokeColor": "#000000",
			"backgroundColor": "#fa5252",
			"width": 158.23046875,
			"height": 38.6015625,
			"seed": 1863543689,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "e8E6GZ4pUmPf5pCVKI1As",
				"focus": -0.5241057916485881,
				"gap": 8.6953125
			},
			"endBinding": {
				"elementId": "l1oI-haKnq55fbJvcH51x",
				"focus": -0.5423567436932546,
				"gap": 12.22265625
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
					-158.23046875,
					38.6015625
				]
			]
		},
		{
			"type": "rectangle",
			"version": 742,
			"versionNonce": 545691457,
			"isDeleted": false,
			"id": "sX-RqbP0Y2s6qbYINE3ja",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -137.40369591346166,
			"y": 607.3305288461539,
			"strokeColor": "transparent",
			"backgroundColor": "#228be6",
			"width": 110.54296875,
			"height": 70.6328125,
			"seed": 1673415399,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "7a2Ui42m6UWUTauEKrsli",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 922,
			"versionNonce": 37894479,
			"isDeleted": false,
			"id": "noI6cI1HxV_TtOrIe7T51",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 15.215444711538339,
			"y": 604.2758413461539,
			"strokeColor": "transparent",
			"backgroundColor": "#228be6",
			"width": 87.20703125000004,
			"height": 70.6328125,
			"seed": 1791857383,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "nOFM6JDBNpUyX5pCeQ-uu",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 705,
			"versionNonce": 1695811361,
			"isDeleted": false,
			"id": "YwvJf9mD",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -66.41932091346166,
			"y": 733.1703725961539,
			"strokeColor": "#000000",
			"backgroundColor": "#228be6",
			"width": 144,
			"height": 19.2,
			"seed": 1556078375,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "7a2Ui42m6UWUTauEKrsli",
					"type": "arrow"
				},
				{
					"id": "nOFM6JDBNpUyX5pCeQ-uu",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "相同项可以提取出来",
			"rawText": "相同项可以提取出来",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "相同项可以提取出来",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 1912,
			"versionNonce": 728512367,
			"isDeleted": false,
			"id": "7a2Ui42m6UWUTauEKrsli",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -75.87244591346166,
			"y": 687.9164663461539,
			"strokeColor": "#000000",
			"backgroundColor": "#228be6",
			"width": 28.34765625,
			"height": 40.9921875,
			"seed": 1413757609,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "sX-RqbP0Y2s6qbYINE3ja",
				"focus": 0.314274576393261,
				"gap": 9.953125
			},
			"endBinding": {
				"elementId": "YwvJf9mD",
				"focus": -0.5440891886376386,
				"gap": 4.26171875
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
					28.34765625,
					40.9921875
				]
			]
		},
		{
			"type": "arrow",
			"version": 1968,
			"versionNonce": 975731457,
			"isDeleted": false,
			"id": "nOFM6JDBNpUyX5pCeQ-uu",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 50.29769108888149,
			"y": 687.1781850961539,
			"strokeColor": "#000000",
			"backgroundColor": "#228be6",
			"width": 29.296047341232565,
			"height": 38.40234375,
			"seed": 1929631399,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "noI6cI1HxV_TtOrIe7T51",
				"focus": -0.39379864027929146,
				"gap": 12.26953125
			},
			"endBinding": {
				"elementId": "YwvJf9mD",
				"focus": 0.013920497936775513,
				"gap": 7.58984375
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
					-29.296047341232565,
					38.40234375
				]
			]
		},
		{
			"type": "rectangle",
			"version": 703,
			"versionNonce": 921792911,
			"isDeleted": false,
			"id": "IesFsceag-vqJ33Sm1hpx",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -8.399789663461661,
			"y": 627.6859975961539,
			"strokeColor": "transparent",
			"backgroundColor": "#40c057",
			"width": 15.5234375,
			"height": 28.125,
			"seed": 1664455753,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "Y_UWmcKuqoAoKEaI9w1YE",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "image",
			"version": 960,
			"versionNonce": 4420321,
			"isDeleted": false,
			"id": "byDgDCq0B1z5fy0pH91oy",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 142.67052283653834,
			"y": 492.4555288461539,
			"strokeColor": "transparent",
			"backgroundColor": "#228be6",
			"width": 172.30468749999997,
			"height": 62.12909405048076,
			"seed": 1149962247,
			"groupIds": [
				"UppjGIr1FHfvHMAlAXSDj",
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"status": "pending",
			"fileId": "8c339f79949b0652feabef25f9eb0c262299aee4",
			"scale": [
				1,
				1
			]
		},
		{
			"type": "rectangle",
			"version": 889,
			"versionNonce": 772433839,
			"isDeleted": false,
			"id": "Q-8vKGJSVH7yPUQF4rsp9",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 250.39708533653834,
			"y": 503.8344350961539,
			"strokeColor": "transparent",
			"backgroundColor": "#40c057",
			"width": 15.5234375,
			"height": 28.125,
			"seed": 1988696329,
			"groupIds": [
				"UppjGIr1FHfvHMAlAXSDj",
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "Y_UWmcKuqoAoKEaI9w1YE",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "arrow",
			"version": 2957,
			"versionNonce": 1542745793,
			"isDeleted": false,
			"id": "Y_UWmcKuqoAoKEaI9w1YE",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -3.724786027476739,
			"y": 622.8031850961539,
			"strokeColor": "#000000",
			"backgroundColor": "#40c057",
			"width": 263.3688866045477,
			"height": 88.05078125,
			"seed": 2053970695,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "IesFsceag-vqJ33Sm1hpx",
				"focus": -1.0926954118096728,
				"gap": 4.8828125
			},
			"endBinding": {
				"elementId": "Q-8vKGJSVH7yPUQF4rsp9",
				"focus": -0.8604180729692337,
				"gap": 2.79296875
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
					90.45780886401508,
					-60.01953125
				],
				[
					175.25468386401508,
					-51.44921875
				],
				[
					234.17265261401508,
					-61.3125
				],
				[
					263.3688866045477,
					-88.05078125
				]
			]
		},
		{
			"type": "rectangle",
			"version": 856,
			"versionNonce": 1591712207,
			"isDeleted": false,
			"id": "730rzKSZ2UGhuFkoQPtri",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -211.90760216346166,
			"y": 455.7602163461539,
			"strokeColor": "transparent",
			"backgroundColor": "#7950f2",
			"width": 15.5234375,
			"height": 28.125,
			"seed": 283326217,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "Y_UWmcKuqoAoKEaI9w1YE",
					"type": "arrow"
				},
				{
					"id": "CykqA3kS60XCjQ1MU_ppD",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 928,
			"versionNonce": 869985953,
			"isDeleted": false,
			"id": "Y8o6NUOOPK7qWEvZhP33J",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -104.98182091346166,
			"y": 452.6938100961539,
			"strokeColor": "transparent",
			"backgroundColor": "#7950f2",
			"width": 15.5234375,
			"height": 28.125,
			"seed": 1706420777,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "Y_UWmcKuqoAoKEaI9w1YE",
					"type": "arrow"
				},
				{
					"id": "vlTkAyTJ7Tch_GOlcY301",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 914,
			"versionNonce": 241814599,
			"isDeleted": false,
			"id": "ylodYEFt",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -256.45567908653857,
			"y": 406.59224759615387,
			"strokeColor": "#000000",
			"backgroundColor": "#7950f2",
			"width": 235.546875,
			"height": 19.2,
			"seed": 1870427047,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "CykqA3kS60XCjQ1MU_ppD",
					"type": "arrow"
				},
				{
					"id": "vlTkAyTJ7Tch_GOlcY301",
					"type": "arrow"
				}
			],
			"updated": 1684378553878,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			},
			"fontSize": 16,
			"fontFamily": 4,
			"text": "两个loss是在不同batch计算出来的",
			"rawText": "两个loss是在不同batch计算出来的",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "LlpQ7hlK",
			"originalText": "两个loss是在不同batch计算出来的",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 2096,
			"versionNonce": 1856402049,
			"isDeleted": false,
			"id": "CykqA3kS60XCjQ1MU_ppD",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -202.9202591867995,
			"y": 451.0375600961539,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "#7950f2",
			"width": 21.138554755461286,
			"height": 15.462970988079917,
			"seed": 1980935271,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "730rzKSZ2UGhuFkoQPtri",
				"focus": -0.906199958908535,
				"gap": 4.72265625
			},
			"endBinding": {
				"elementId": "IXOvvVofExRcvb0855t0m",
				"focus": 0.2413985065435075,
				"gap": 14.017658488079945
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
					21.138554755461286,
					-15.462970988079917
				]
			]
		},
		{
			"type": "arrow",
			"version": 2321,
			"versionNonce": 185643535,
			"isDeleted": false,
			"id": "vlTkAyTJ7Tch_GOlcY301",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -105.7411297877826,
			"y": 451.0781885840753,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "#7950f2",
			"width": 17.302415248135503,
			"height": 15.954690987921254,
			"seed": 138170535,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Y8o6NUOOPK7qWEvZhP33J",
				"focus": 0.3685649551104957,
				"gap": 1.6156215120786328
			},
			"endBinding": {
				"elementId": "IXOvvVofExRcvb0855t0m",
				"focus": -0.8589089230418397,
				"gap": 14.468749999999915
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
					-17.302415248135503,
					-15.954690987921254
				]
			]
		},
		{
			"type": "rectangle",
			"version": 791,
			"versionNonce": 1897298529,
			"isDeleted": false,
			"id": "QWlersifzOliykOWWLA8M",
			"fillStyle": "cross-hatch",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -317.10682091346166,
			"y": 404.3344350961539,
			"strokeColor": "#5f3dc4",
			"backgroundColor": "transparent",
			"width": 635.140625,
			"height": 351.84375,
			"seed": 90980007,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "69xBTO_K9uy06NcB1Y_5X",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "rectangle",
			"version": 493,
			"versionNonce": 762157103,
			"isDeleted": false,
			"id": "uZdRNI2A80bYbjy_tz3TY",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 2,
			"opacity": 100,
			"angle": 0,
			"x": 110.43224158653834,
			"y": 604.8578725961539,
			"strokeColor": "transparent",
			"backgroundColor": "#fd7e14",
			"width": 59.41796875,
			"height": 75.42578125,
			"seed": 636778183,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "5vFGV7cIAZcKvDwdrll6y",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "arrow",
			"version": 1913,
			"versionNonce": 2056061038,
			"isDeleted": false,
			"id": "5vFGV7cIAZcKvDwdrll6y",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 177.2528545673085,
			"y": 657.2050030311431,
			"strokeColor": "#000000",
			"backgroundColor": "#fab005",
			"width": 36.220853365384926,
			"height": 8.005411171730998,
			"seed": 1343207785,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1684394740811,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "uZdRNI2A80bYbjy_tz3TY",
				"gap": 7.402644230770193,
				"focus": 0.5159712658308377
			},
			"endBinding": {
				"elementId": "D0nAunB6",
				"gap": 3.8043870192298073,
				"focus": 0.6397204187856421
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
					36.220853365384926,
					-8.005411171730998
				]
			]
		},
		{
			"type": "rectangle",
			"version": 85,
			"versionNonce": 1535687247,
			"isDeleted": false,
			"id": "R4F7LSRJDTSH0Xf-kXuIB",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -132.2113199300652,
			"y": 453.84821111506164,
			"strokeColor": "transparent",
			"backgroundColor": "#82c91e",
			"width": 20.98046875,
			"height": 30.05078125,
			"seed": 403991977,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [
				{
					"id": "FdQlDu3K_jBor3a0ODn3G",
					"type": "arrow"
				}
			],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "text",
			"version": 139,
			"versionNonce": 171054625,
			"isDeleted": false,
			"id": "azAQzjQZ",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 213.61440805288476,
			"y": 538.4499755859401,
			"strokeColor": "#000000",
			"backgroundColor": "#fd7e14",
			"width": 32,
			"height": 19.2,
			"seed": 1941750319,
			"groupIds": [
				"Qr1GMKme4A2fET1LFafbQ"
			],
			"roundness": null,
			"boundElements": [],
			"updated": 1664097326323,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "步长",
			"rawText": "步长",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "步长",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 199,
			"versionNonce": 640817415,
			"isDeleted": false,
			"id": "xhIEAzQK",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -711.2488731971152,
			"y": 636.9695068359403,
			"strokeColor": "white",
			"backgroundColor": "#fd7e14",
			"width": 320.421875,
			"height": 76.8,
			"seed": 1286297353,
			"groupIds": [],
			"roundness": null,
			"boundElements": [],
			"updated": 1684378553862,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 4,
			"text": "假如第二次采样到了相同的task\n也就是相同的old item\nold item id embedding不能继承上次训练的成果\n而是重新初始化",
			"rawText": "假如第二次采样到了相同的task\n也就是相同的old item\nold item id embedding不能继承上次训练的成果\n而是重新初始化",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": "D3wQH6UU",
			"originalText": "假如第二次采样到了相同的task\n也就是相同的old item\nold item id embedding不能继承上次训练的成果\n而是重新初始化",
			"lineHeight": 1.2,
			"baseline": 72
		},
		{
			"type": "rectangle",
			"version": 220,
			"versionNonce": 1759960039,
			"isDeleted": false,
			"id": "okMuL9Tb6z3t6UlQzpqzI",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1146.8855919471152,
			"y": 619.6960693359403,
			"strokeColor": "#c92a2a",
			"backgroundColor": "transparent",
			"width": 344.55078125,
			"height": 28.56640625,
			"seed": 27993769,
			"groupIds": [],
			"roundness": null,
			"boundElements": [
				{
					"id": "kA-_OGcJpbJO3o4fP6afC",
					"type": "arrow"
				}
			],
			"updated": 1664760813030,
			"link": null,
			"locked": false,
			"customData": {
				"legacyTextWrap": true
			}
		},
		{
			"type": "arrow",
			"version": 118,
			"versionNonce": 1334888199,
			"isDeleted": false,
			"id": "kA-_OGcJpbJO3o4fP6afC",
			"fillStyle": "cross-hatch",
			"strokeWidth": 0.5,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -799.7684044471152,
			"y": 638.4109130859403,
			"strokeColor": "#a61e4d",
			"backgroundColor": "#e64980",
			"width": 87.26953125,
			"height": 34.84765625,
			"seed": 624900775,
			"groupIds": [],
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1664760818171,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "okMuL9Tb6z3t6UlQzpqzI",
				"focus": -0.7870582733101912,
				"gap": 2.56640625
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					87.26953125,
					34.84765625
				]
			]
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#a61e4d",
		"currentItemBackgroundColor": "#e64980",
		"currentItemFillStyle": "cross-hatch",
		"currentItemStrokeWidth": 0.5,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 4,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 1645.0144981971152,
		"scrollY": 836.7531494140597,
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