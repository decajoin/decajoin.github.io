---
title: Lecture15 终身学习(Life Long Learning)
tags:
  - Hung-yi Lee ML 学习笔记
---

[课件下载 Lecture15 .pdf](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/life_v2.pdf)

## Catastrophic Forgetting

### Example1

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721967675538-bc17e874-6067-4010-b50e-0f841d591555.png)<br />一个数字辨识模型在学习 Task1 的资料后对 Task1 和 Task2 的正确率分别是 90% 和 96%，但是学完 Task1 后再学习 Task2 的资料后对 Task1 的正确率却下降到 80%，这就是 Catastrophic Forgetting 现象。那有没有可能是这个网络层数太少，结点太少而无法同时学好两种 Task 呢？<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721968046053-134fe226-a1f5-4769-b659-bb250832d2b4.png)<br />把 Task1 和 Task2 的资料混在一起进行训练，训练结果是 Task1 和 Task2 的正确率分别是 89% 和 98% 说明这个简单的模型是有能力同时训练好 Task1 和 Task2，但是先训练 Task1 后训练 Task2 就会出现 Catastrophic Forgetting 现象。

### Example2

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721968536429-7f585a16-260e-49aa-b45d-917202c2ae98.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721968553687-bd945cea-2536-4552-9a9b-e2f02813b8c8.png)<br />上图是依次学习 20 个 QA 任务和一次学习 20 个 QA 任务的结果图。可以看到，一开始我们没有学过任务五，所以准确率是零。当开始学第五个任务之后，准确率达到百分百，然而开始学下一个任务的时候，准确率就开始暴跌，即学习新的任务之后一下子就完全忘了前面的任务。可能模型本身就没有学习那么多任务的能力，但其实不是的。当模型同时学二十个任务的时候，我们会发现机器是有潜力能够学习多个任务的，当然这里第十九个任务可能有点难，模型的准确率非常低。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721968824710-b08a416b-dbdf-41ed-a65a-7ca7ab0a5a57.png)<br />当模型依次学习多个任务的时候，它好像一个上下两端都接有水龙头的池子，新的任务从上面水龙头进来，旧的东西就从下面水龙头那里冲掉了，永远学不会多个技能，这种情况称为灾难性遗忘(Catastrophic Forgetting) 。我们知道即使是人类也可能会有遗忘的时候，而这里在遗忘前面我们加上了灾难性这个形容词，意在强调模型的这个遗忘不是一般的遗忘，而是特别严重程度的遗忘。<br />模型是能够同时学多个任务的，这种学习方式称为多任务学习(Multitask Learning)，既然可以多任务学习，那为什么还需要终身学习呢？问题就在于假如我们要训练一千个任务，在训练第一千个的时候，还需要把前面九百九十九个的资料放在一起进行训练，这个是不符合实际的。<br />那我们不能每个任务都分别用一个模型呢？其实这样做会有一些问题，首先这样做可能会产生很多个模型，这样对机器存储也是一个挑战。另外不同任务之间可能是有共通的，从任务 A 学到的数据也可能在学习任务 B 的时候有所参考。这个和前面讲到的迁移学习有点像，但是迁移学习是从任务 A 学到的数据在任务 B 上可以发挥作用，我们在意的是模型在任务 A 学习到的东西能不能对任务 B 有所帮助，只在乎任务 B 做得如何。而终身学习更注重的是在解决完任务 B 之后能不能再回头解决任务 A。

### Why?

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721983424192-726e9b11-70fe-4198-af95-1d81aec461b1.png)<br />蓝色的部分是 loss 较小的部分，可以看出两个 Task 的 loss 小的区域是不一样的，学会 Task1 之后在学 Task2 可以看出参数向 Task2 loss 小的区域移动，但是移动后的区域再用到 Task1 上效果就不好了，出现了 Catastrophic Forgetting 现象。

## Evaluation

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721982600758-a0489490-a5ef-4b59-bf70-af8ed58091bd.png)<br />Accuracy 就是指学完所有 Task 之后，所有任务的平均正确率。Backward Transfer 是指学完所有 Task 跟最初学某个 Task 的正确率的平均下降率，通常是负数。

## Solution

终身学习的 Solution 主要分为三个方向。

### Selective Synaptic Plasticity

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721984059273-8225a26f-e82c-40b2-b902-fa7ca815e899.png)<br />基本的思想就是每一个参数对我们过去学过的任务的重要性程度是不同的，因此在学习新的任务时，我们尽量不要动那些对过去任务很重要的参数，而是去学一些其他的对新任务比较重要的参数。假设 $\theta^b$ 是前一个任务学出来的参数，在选择性的突触可塑性这一解法中会给每一个参数 $\theta^b$ 赋予一个系数 $b_i$，这个系数代表着对应的参数对过去的任务到底重不重要，也称作“守卫”。<br />这里 $b_i = 0$ 时，表示我们并不关心学习新任务时的参数需要跟过去的参数有什么联系，即一般的训练，这种情况下就容易发生遗忘。另一种极端就是当 $b_i$ 趋近于无穷大时，即模型参数不肯在新任务上发生妥协，此时可能不会忘记旧的任务，但是也很有可能学不好新的任务，这种情况我们称作不妥协。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721984690745-1dbbd8ec-95f4-4e26-adea-fb7317410658.png)

#### EWC

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721984747455-70477696-b813-48fd-9aa9-7301aa420c80.png)<br />蓝色是 $b_i = 0$ 的结果，绿色是 $b_i = 1$ 的结果，红色是采用 EWC 算法计算每个参数的 $b_i$ 得到的结果。分析结果可以知道，蓝色出现了灾难性遗忘先前学到的 TaskA 和 TaskB 在后续都出现了明显的正确率下降。绿色由于每个 $b_i$ 都设置为 1 所以在后续两个任务上都 train 不起来，正确率一直无法提高。红色使用 EWC 算法为每个参数都计算出适合的 $b_i$，结果上没有出现灾难性遗忘而且后续的 Task 的正确率都还比较高。<br />EWC 算法的具体细节请参考论文。

#### GEM

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721985531671-03338c79-f801-458d-8c6e-fdb519d1bb22.png)<br />除了在参数上做限制，还可以在梯度的更新方向上做限制，它在计算当前任务梯度方向的同时，也会回去算历史任务对应的梯度方向，然后把两个梯度进行向量求和，得出实际的梯度方向，这样持续更新就能尽可能接近一个不会陷入灾难性遗忘的最优解了。这就是梯度回合记忆(Gradient Episodic Memory， GEM)。<br />但是这类方法需要把过去的数据同时存储下来，其实这跟终身学习的初衷有些违背了，因为终身学习本身就是希望不需要依赖过去的数据。但由于这里只是存储梯度这些少量的信息，像前面讲的选择性的突触可塑性方法也需要存储一些历史模型信息，所以在实际操作过程中也还能接受。

### Additional Neural Resource Allocation

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721987885742-ff662f1b-473a-40d6-b0cf-2cb545576005.png)<br />这个算法大概实现就是每次学一个新任务就增加结点的数量，这当然就没有灾难性遗忘的问题了，学到 Task 越多，模型就越大。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721985794609-26b37fa3-f1a2-4688-a126-e0e013baa540.png)<br />这个算法的大概实现就是首先定义一个很大的 Network，里面有很多结点，训练 Task1 的时候使用其中一部分的结点，训练 Task2 的时候再使用另外一部分结点......是 Progressive Neural Networks 反过来。<br />他们俩结合就是 CPG 算法。

### Memory Reply

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721988165353-1d8f8d33-7f67-432e-b39f-0da839952e75.png)<br />这个算法的核心就是在 Task1 训练我们的模型的同时，训练一个 Generator 用来生成 Task1 的资料，接下来训练 Task2 时，我们就用 Task1 的 Generator 产生的资料和 Task2 的资料混合训练 Task2 和对应的 Generator，这个 Generator 就可以产生 Task1 和 Task2 的资料......
