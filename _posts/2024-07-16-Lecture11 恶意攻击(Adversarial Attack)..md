---
title: Lecture11 恶意攻击(Adversarial Attack)
tags:
 - Hung-yi Lee ML 学习笔记
---

[课件下载Lecture11.pdf](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/attack_v3.pdf)

## How to Attack

### Example

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720941735214-db081882-fcac-41f5-99e0-258c0d3d20bd.png)

### Algorithm

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720942377334-27ccf032-2078-42b7-a56e-1b04bfd75475.png)

选定$d$的表达方式不一定是唯一的，下图是在影像辨识系统中选择`L-infinity`作为两张图之间的差异，但是对于声音信号就不一定是`L2-norm`或`L-infinity`，要根据具体情况来选择，这也是一个需要`Domain knowledge`的问题。

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720939340355-8a78e899-cff0-4a71-9a8f-df48a216a62b.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720942826389-7547aec6-7401-4350-8555-d892c86e5a05.png)

### Fast Gradient Sign Method(FGSM)

是一个简单的`Gradient Descent`算法，它将参数更新的迭代取消，只用一次就找到参数，主要的变化在计算`Gradient`上，引入$sign$来计算。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720945014304-64881684-7286-4688-92b7-a5ee62658e9d.png)

## Black Box Attack

### White Box v.s. Black Box

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720947889607-9a56276b-1c14-40db-8951-bd647cbca348.png)

### How?

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720948096016-737f18df-cde0-49dd-9af4-4ee971296dbc.png)

对于没有训练数据的情况可以准备部分图片输入调用模型得到对应的结果，然后将图片和输出的结果作为一个pair来训练Proxy Network。

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720948233803-d9e49036-69a0-49c4-80b9-6d438b7554d4.png)

### Why?

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720948483596-9c5f464a-bdcd-49f2-8699-7c41cce14b10.png)

## One pixel attack

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720948997516-7728f2a8-8fb0-448a-bfa1-42b94cdaf117.png)

## Universal Adversarial Attack


用同一个noise给多张图片处理，结果网络的判断结果都发生了变化

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720949085578-9d2691b6-5945-42bf-8c9a-7a8fd68dd5df.png)

## Defense

### Passive Defense

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720949479797-978440f3-ed9d-49a4-8486-a794b8954d79.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720949687999-f32873ba-b719-4130-b311-826fa66a370e.png)

Passive Defense基本上都是利用一些操作将Attack淡化，使之不被网络察觉，从而失效。但是上述几种方法都有致命的缺点，就是如果攻击方知道了你的操作就会在生成攻击的时候考虑你的Defense让其失去作用。比如，在训练之前就进行模糊操作，训练出的Attack就可以让模糊Defense失效。

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720950051387-9f65256f-841e-4fda-991d-926048eef9b8.png)

为了解决上述的问题，就可以采用Randomization的方式进行操作。输入一张图片对它进行随机的处理（缩放，模糊，移动...）然后再进行判断。本质还是一个filter，但是这是一个对图片随机进行变化的filter，可能对于Attack就有更好的防御作用。


### Proactive Defense

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1720950313107-8b40a30d-32d8-4323-8c36-381533630e31.png)

主动防御就是自己训练攻击模型对网络进行攻击，然后将攻击图片进行正确标注然后加入训练数据集进行训练，就可以起到Defense的作用。（当然，这个也是一种数据增强的方式，即使没有人要对你进行Attack也可以使用这个方法增强模型的鲁棒性）
