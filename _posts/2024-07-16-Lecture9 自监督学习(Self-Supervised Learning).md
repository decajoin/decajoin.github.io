---
title: Lecture9 自监督学习(Self-Supervised Learning)
tags:
  - Hung-yi Lee ML 学习笔记
---

[课件下载 Lecture9](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/bert_v8.pdf)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708268813741-41d0485f-58b8-41ee-ad1c-c754f98bcdb9.png)

## BERT series

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706446434211-e641e4a5-eb16-4ffc-8097-831eff1f5613.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708352201477-414d6260-cd3f-4972-9842-561a5ee21ab4.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708352280917-a23ff906-3740-42ec-abd5-ebfcb634fe71.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708353060352-3ebea7c5-b838-4376-a45d-41976b4b99fc.png)<br />BERT 阶段训练是 Self-supervised Learning，但是在 Downstream 做 fine-tune 的时候是 Supervised Learning<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708353192878-e4be742a-5b86-40ea-b944-61a8f66fc1db.png)

### How to use BERT

#### Case1

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708353515833-503707a6-710a-455f-84c3-f7c1c226a0b9.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708353576895-14f29aed-df52-4171-a3f6-15e2ce4f7635.png)

#### Case2

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708353671388-6c21695a-b471-4716-abf6-4ce59ffb4da5.png)

#### Case3

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708353790709-e5efad5c-bfda-4b6e-b8de-d4df75b320b0.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708353805629-bc6c56f5-e2cd-4ec1-a4de-5901b0b5b162.png)

#### Case4

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708354341168-5059d74f-9a1f-4961-815c-b61d3e1ef8ae.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708354674728-3c168882-f18f-43f6-9d90-8f03ad9c3cc8.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708354734163-1ddeac5c-b29a-4ba0-9b78-0198ca881496.png)

#### seq2seq model

前面都是训练 BERT 做填空题，但是它也可以训练做 seq2seq model<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708434940884-655f9fcf-da96-4e3b-95fb-d8853125e8d1.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708435065046-949291ef-6269-4538-be58-3d0a4eedb364.png)

### Why does BERT work

#### Explanation1

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708440295141-a2ba0f10-1444-498f-a769-e3e137598eee.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708440522135-45f877a7-b6d7-440f-9dd3-ccefc8418363.png)<br />我们可以看见对于同一个”苹果“，苹果汁的”苹果“和苹果手机的”苹果“，BERT 的学习的 embedding 是不一样的的。看热力图可知，对于可以吃的”苹果“，BERT 算出的 embedding 都比较相似，而对于苹果公司的”苹果“，他们的 embedding 比较相似。所以我们可以说 BERT 好像能够理解词语的具体意思，知道”苹果“是可以吃的苹果还是苹果公司的苹果。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708440023001-3bd78f8d-3536-4b07-8da2-4a0e897e5a0c.png)<br /> ![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708441161973-eebb1fae-eba3-4577-9937-26599595071f.png)

#### Explanation2

在 explanation1 中说到 BERT 似乎是靠理解词语的意思工作，但是经过实验发现，事实可能不是那么简单。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708441277505-69b25525-61a6-4756-bcfc-ee81243ee07b.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708441755692-7bfcb722-c16f-4aa8-aebd-f80faefc6140.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708442081514-3a67f059-3d2c-480e-877d-b4802bc05514.png)

### Multi-lingual BERT

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708485499306-670ee321-36a3-41da-961c-fb04a6ce7ccb.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708485823353-24096f08-5854-434b-ad55-495f7cca185f.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708485899252-f715a0c8-9bd9-4a0e-87b7-cbfa4ab0c07d.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708486109049-4c384f34-5a1b-45fe-855f-75fbc85c732a.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708490226418-4c9ea425-6247-4e9e-87c8-e43ae0503263.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708493031097-3e50c9fa-2207-4f97-9db7-cf0fe6fcbf60.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708493124756-6df35808-0a2d-4c5c-9aed-06d62b224843.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708493148161-f0743e77-0e36-4b21-b30c-d54c2987540b.png)

## GPT series

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708495442426-dce90d23-2610-48b7-bcaa-ef736a0998b3.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708495675284-6e484433-6118-453e-a50e-6f280e381a3c.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708495704218-9c047b9f-6328-49fe-9713-76e41ece6f7b.png)
