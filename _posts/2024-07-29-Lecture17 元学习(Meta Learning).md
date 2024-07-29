---
title: Lecture17 元学习(Meta Learning)
tags:
  - Hung-yi Lee ML 学习笔记
---

[课件下载 Lecture17](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/meta_v3.pdf)

## What is Meta Learning?

元学习从字面的意思就是“学习”的“学习”，也就是学习如何学习。大部分的深度学习就是在不断的调整超参数，或者在决定网络架构，改变学习率等等。实际上没有什么好方法来调这些超参，今天工业界最常拿来解决调整超参数的方法是买很多张 GPU，然后一次训练多个模型，有的训练不起来、训练效果比较差的话就输入掉，最后只看那些可以训练的比较好的模型会得到什么样的性能。所以在业界做实验的时候往往就是一次开几张 GPU，这些 GPU 跑多组不同的超参数，看看哪一组超参数可以得到最好的结果。但是在学术界我们通常没有那么多张 GPU，通常需要凭着经验和直觉定义可能效果比较好的超参数，然后看看这些超参数会不会得到好的结果。但是这样的方法往往会花费很多时间，因为需要不断的去调整这些超参数。**所以我们就会想办法让机器自己去调整这些超参数，机器自己学习一个最优的模型和网络架构**，然后得到好的结果，元学习就这样诞生了。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722159550841-46fed579-a4c7-44c6-bda6-24049dea54c2.png)

## Meta Learning Steps

### Meta Learning – Step 1

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722161274645-afc07052-b6a3-4e20-a1e2-6dafaf443ef4.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722161360386-64f7a38c-3f60-4678-8e55-99d027b7a9f2.png)<br />我们期待它们是可以通过学习算法被学出来的，而不是像机器学习一样我们人为设定的。我们把这些在学习算法里面想要它自学的东西统称为 $\phi$，在机器学习里面我们是用来 $\theta$ 代表一个函数里面我们要学的东西。接下来，我们都将学习算法写为 $F_\phi$，代表这个学习算法里面有些是未知的参数。对于不同的元学习的方法，它会想办法去学模型不同的成分，当去学不同模型成分的时候，我们就有了不同的元学习的方法。

### Meta Learning – Step 2

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722164693365-aa62c3c8-d202-405d-b70c-044769e0c100.png)<br />第二步是定义 Meta Learning 的 loss，我们使用$L(\phi)$来表示用$\phi$作参数的模型的性能。在机器学习里面，损失函数来自于训练数据，那在元学习里面，我们收集的是训练任务。举例来说，假设今天想要训练一个二分类的分类器，来分辩苹果和橘子（任务一），以及分别脚踏车和车（任务二），以上每一个任务里面我们都会有分训练数据和测试数据。<br />我们评价一个学习算法的好坏，是看其在某一个任务里面使用训练数据学习得到的算法的好坏。比如，任务一是分辨苹果和橘子，我们就把任务一里的训练数据拿出来输入给这个学习算法，从而学出一个分类。我们使用 $f_{\theta^1}$ 来代表这个是任务一的分类，分辨苹果和橘子。如果这个分类是好的，那就代表我们模型是好的，反之如果这个分类是不好的，就代表说这个学习算法是不好的。对于不好的学习算法（测试数据中表现不好的），我们就会给它比较大的损失$L(\phi)$。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722217364642-99d97ba5-a107-4d6f-b14b-b15ae382dfea.png)<br />我们的元学习的目标是学习怎么得到一个二元分类器，输入的是各种训练数据集，输出的是对应二元分类器的模型参数，所以对于一个元学习模型，肯定不止一个任务。我们需要考虑多个任务的学习表现来评估元学习模型的$L(\phi)$。所以$L(\phi)=\sum_{n=1}^N l^n$可以更全面评估这个元学习模型的表现情况。

### Meta Learning – Step 3

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722217798936-955ac5be-38c8-4c3b-9cdc-6062e0d4c5f2.png)<br />第三步就是使用算法求$\phi$让$L(\phi)$最小，如果可以计算梯度就可以使用我们的 Gradient Descent，如果梯度无法计算就是用 RL 硬 train 一发也可以。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722217986511-131c3067-8a8b-42fc-b699-ca43635b5cf4.png)

## ML v.s. Meta

### Goal

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722218272206-ac3ad812-a4fe-4816-b0f8-a328768c9891.png)

### Training Data

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722218376570-6c9a73e6-6c88-4499-b259-8dfd61a81b42.png)<br />Machine Learning 中的训练资料是训练集，Meta Learning 中的训练资料是训练“任务”，这个“任务”中既有训练集又有测试集。这个很容易搞混，所以在文献中，我们会把任务里面的训练数据叫做支持(support)，把测试数据叫做查询(query)。在元学习里面，我们是拿查询来进行训练，而在机器学习里面，我们是拿支持来进行训练。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722257598850-1ad5768b-db04-48a3-b6fa-e5ae8fed07d3.png)

### Loss

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722218923076-51413d12-bef1-49a2-9edb-1e11cf28ee46.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722257655070-4385bdc9-43cb-4c5a-9179-b92df8f0b609.png)<br />对于训练的过程两者也有一些差异，元学习的训练需要算$l^n$，也就是计算每一个小任务的损失函数，在这个过程中，我们需要做一次单一任务的训练 + 测试，也就是一个 Episode。现在假设我们的优化算法中，要找一个$\phi$让$L(\phi)$最小，那做这件事情的时候，我们需要算这个$L$很多次，也就是跨任务的训练包含了很多次的单一任务的训练 + 测试。这个是非常复杂的且耗时的。<br />所以在文献中，在 Learning to initialize 这个领域，往往把跨任务训练叫做外循环，把单一任务训练叫做内循环。这是因为在跨任务训练里要跑好几次单一任务训练，所以跨任务训练是外循环，单一任务训练是内循环。不过，外循环和内循环这些称呼通常只有在 Learning to initialize 的工作中才会有，其他时候通常也不会这样叫。

## Applications

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722259161197-e62febed-b7c9-4d7c-ac7b-190806aa66d2.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722259213324-9256d3ef-805c-48df-9b72-0088512977c3.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1722259223283-f9055ded-b7bc-41bc-a38a-a606aa00ef08.png)<br />那要怎么去找一系列的 N 类别 K 样例下的任务呢？在文章中最常见的一种做法是使用 Omniglot 当做基准， Omniglot 是一个手写的数据集，它有 1623 个不同的字符，每一个字符有 20 个样例。那有这些字符以后呢，我们就可以去制造 N 类别 K 样例下的分类。比如我们从 Omniglot 里面选出 20 个字符，然后每一个字符就只取一个样例，这样就得到一个 20 类别 1 样例的分类任务。如果我们把这个任务当做训练数据，那我们就可以让机器学习到 20 类别 1 样例的分类算法。如果我们把这个任务当做测试数据，那我们就可以测试机器在 20 类别 1 样例的分类任务上的表现。同理，我们可以制造出 20 类别 5 样例的分类任务，这个任务里面每一个类别都有 5 个样例，然后我们可以把这个任务当做训练数据，让机器学习到 20 类别 5 样例的分类算法。<br />在使用 Omniglot 的时候，我们会把字符分成两半，一半是拿来制造训练任务的字符，另外一半是拿来制造测试任务的字符。如果我们要去制造一个 N 类别 K 样例的任务，那么就是从这些训练任务的字符里面先随机采样 N 个字符，然后这 N 个字符每个字符再去采样 K 个样例，集合起来就得到一个训练的任务。对于测试的任务，就从这些测试的字符里面拿出 N 个字符，然后每个字符采样 K 个样例，你就得到一个 N 类别 K 样例下的测试任务。这样我们就可以把 Omniglot 当做一个基准，然后在这个基准上面测试不同的元学习算法。<br />总之，元学习不是只能用非常简单的任务，今天在学界已经开始把元学习推向更复杂的任务。
