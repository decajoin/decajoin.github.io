---
title: Lecture14 强化学习(Reinforcement Learning)
tags:
  - Hung-yi Lee ML 学习笔记
---
[课件下载 Lecture14](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/drl_v5.pdf)

## What is RL?

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721718841206-23dd0d42-0d17-4330-9e7b-615ca4b7e83a.png)

### Example for RL

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721718997386-9d8de76e-22de-4f51-9bf8-4b9026f01bba.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721719063813-db03e965-9e76-4c07-8614-18a0866af1b5.png)

### Three steps in ML

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721719462230-6db65dd1-22ac-46d7-9b8c-7444e2accbfa.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721720599073-c282d8a6-5731-4652-a9a7-f1db9c7fcacd.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721794717863-076af447-77c9-46b4-a486-a13a87753e33.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721719695074-95139c5e-2cd3-41b4-94b6-9acaca9b477e.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721720090530-eb156de3-8e56-4990-9819-2443e26ab364.png)

## Policy Gradient

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721720440185-7b19cd64-1c32-43cf-9f30-a1acb58d23cc.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721721452974-066b3641-fe35-41e4-b9d5-20d43eef6f62.png)<br />关键就是怎么来定义这个$A$

### Version0

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721721919489-c98a19a0-e2d2-4b9c-8578-4b6e2799a201.png)<br />最简单的评价方式是，假设在某一个状态$s1$，模型执行$a1$，得到奖励$a1$。如果奖励是正的，代表该动作是好的；如果奖励是负的，代表该动作是不好的。<br />但是这样的评价方式是存在问题的，因为每一个当下的 Action 或多或少都会影响后续整个游戏的发展，但是这个方式只看到了执行这个 Action 得到的 Reward 没有考虑执行之后的后续 Reward 的获得情况，所以说这个模型是一个短视(Short-sighted)的模型。<br />比如在游戏《Space invader》里面，飞船要先左右移动一下进行瞄准，射击才会得到分数。而左右移动是没有任何奖励的，其得到的奖励是零。只有射击才会得到奖励，但是并不代表左右移动是不重要的，先需要左右移动进行瞄准，射击才会有效果，所以有时候我们会牺牲一些近期的奖励，而换取更长期的奖励。如果使用版本 0，左移和右移的奖励为 0，开火的奖励为正，模型会觉得只有开火是对的，它会一直开火。

### Version1

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721727039013-c2772d80-6d26-419a-a556-9200678ee099.png)<br />把未来所有的奖励加起来即可得到累积奖励$G$，用其来评估一个动作的好坏，$G_t$是从时间点$t$开始，$r_t$一直加到$r_N$。$a1$的好坏不是取决于$r1$，而是取决于$a1$之后所有发生的事情，即执行完$a1$以后所有得到的奖励$G1$， $A1$等于$G1$。使用累积奖励可以解决 Version0 遇到的问题，因为可能右移移动以后进行瞄准，接下来开火就有打中外星人。因此右移也有累积奖励，虽然右移没有立即的奖励。假设$a1$是右移， $r1$可能是 0，但接下来可能会因为右移才能打到外星人，累积的奖励就会正的，因此右移也是一个好的动作。<br />但是版本 1 也有问题，假设游戏非常长，最开始的 Action 对后续 Reward 的影响其实相当小了，所以把$r_N$归功于$a1$是不太合适的。

### Version2

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721727543618-8c09527e-5641-4d40-b818-c0a577307b69.png)<br />Version2 中引入一个折扣因子$\gamma$，$\gamma$会设置一个小于 1 的值，越后面的 Reward 与前面的 Action 的关系就越小，因为$\gamma$乘了非常多次。所以加入折扣因子可以把给离$a1$比较近的奖励比较大的权重，给离$a1$比较远的那些奖励比较小的权重。因此新的$A$等于 $G1^{'}$，离采取的动作越远的奖励，$\gamma$就被乘越多次，它对$G1^{'}$的影响就越小。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721728121854-b1b128e9-e3a4-4bc0-a80f-bb0552db7b45.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721728152110-c879e4a6-ecdc-4bd1-a139-524fc71668c3.png)

### Version3

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721728229031-ff319ce8-3a8f-42ec-982a-140f22f48cb5.png)<br />因为好或坏是相对的，假设在游戏里面，我们每次采取一个行动的时候，最低分预设是 10 分，其实得到 10 分的奖励算是差的，奖励是相对的。用$G^{'}$来表示评估标准会有一个问题：假设游戏里面，**可能永远都是拿到正的分数**，每一个动作都会给出正的分数，只是大小的不同， $G^{'}$ 算出来都会是正的，有些动作其实是不好的，但是我们仍然会鼓励模型去采取这些动作。因此我们需要做一下标准化，最简单的方法是把所有的$G^{'}$都减掉一个基线（baseline）$b$，让$G^{'}$有正有负，特别高的$G^{'}$让它是正的，特别低的$G^{'}$让它是负的。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721728592398-6d42d237-8ba7-45ab-add6-8abc8770d908.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721791961750-7a948b7f-b9f3-4a06-98ba-d79368f90c5d.png)<br />在一般的训练中，收集数据都是在训练之前，比如有一堆数据，用这堆数据拿来做训练，更新模型很多次，最后得到一个收敛的参数，拿这个参数来做测试。**但在强化学习不同，其在训练迭代的过程中收集数据。**<br />在强化学习中，收集到一批数据在这批数据完成训练之后就需要重新收集，如果要更新 400 次参数就需要收集 400 次数据，非常耗时间。<br />如何理解呢，举个《棋魂》的例子，进藤光跟佐为下棋，进藤光下了小马步飞（棋子斜放一格叫做小马步飞，斜放好几格叫做大马步飞）。下完棋以后，佐为让进藤光这种情况不要下小马步飞，而是要下大马步飞。如果大马步飞有 100 手，小马步飞只有 99 手。之前走小马步飞是对的，因为小马步飞的后续比较容易预测，也比较不容易出错，大马步飞的下法会比较复杂。但进藤光假设想要变强的话，他应该要学习下大马步飞，或者是进藤光变得比较强以后，他应该要下大马步飞。同样是下小马步飞，对不同棋力的棋士来说，其作用是不一样的。对于比较弱的进藤光，下小马步飞是对的，因为这样比较不容易出错，但对于已经变强的进藤光来说，下大马步飞比较好。因此同一个动作，对于模型的不同训练阶段而言，它的好是不一样的。<br />如果收集数据的模型跟被训练的模型是同一个模型，当模型更新以后，就要重新去收集数据，这就是强化学习非常花时间的原因， 异策略学习（off-policy learning） 可以解决该问题。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721794498141-fbc59666-9f3c-4982-9fa2-462a548b8b82.png)

### Actor-Critic

与 Environment 交互的网络是 Actor，Critic 的工作是用来评估一个 Actor 的好坏。Actor 是看见 Observation 给出下一步的 Action，Critic 是看见 Observation 给出接下来得到的 Reward。但是 Critic 有不同的变形，有些是看见 Observation 和 Actor 采取的 Action 给出 Reward。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721800263664-c6e5f955-736b-43cf-b260-1ceceb8db492.png)<br />训练 Critic 的常用方法有两种，蒙特卡洛(Monte-Carlo)和时序差分(Temporal-Difference)，训练时，Actor 会跟 Environment 进行很多轮互动得到一些游戏的记录，看到的游戏画面$s_a$，得到的累计奖励为$G_a^{'}$；看到的游戏画面$s_b$，得到的累计奖励为$G_b^{'}$。

#### Monte-Carlo

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721800885449-06f408d9-a725-4970-bafa-4844f79b5609.png)<br />使用蒙特卡洛就是让 Critic 得到的$V^{\theta}(s_x)$与$G_x^{'}$越接近越好。使用这个方法训练需要完成整局游戏才能得到最后的累计奖励$G_x^{'}$，但是有些游戏可能一局的时间很长，或者根本就不会结束，那采取这个方法就不太合适了。

#### Temporal-Difference

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721802315925-7b482804-24de-40d7-846f-787a8b33c329.png)<br />使用时序差分可以不玩完整局游戏，根据一部分参数就可以当做训练资料，根据图中的公式可知，$V^{\theta}(s_t) = \gamma V^{\theta}(s_{t+1})+r_t$，我们虽然不知道$V^{\theta}(s_t)$和$V^{\theta}(s_{t+1})$的具体值，但是我们知道他们相减等于$r_t$，所以我们训练的目标就是让$V^{\theta}(s_t) - \gamma V^{\theta}(s_{t+1})$与$r_t$越接近越好。

#### MC v.s. TD

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721806142868-03eac7b2-021d-49ed-839d-8effab0f2eb4.png)<br />图中某个 Actor 跟环境互动，玩了某个游戏八次的记录。为了简化计算，假设这些游戏都非常简单，一到两个回合就结束了。比如 Actor 第一次玩游戏的时候，它先看到画面$s_a$，得到奖励 0，接下来看到画面$s_b$，得到奖励 0，游戏结束。接下来有连续六场游戏，都是看到画面$s_b$，得到奖励 1 就结束了。最后一场游戏，看到画面$s_b$，得到奖励 0 就结束了。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721806471420-d2036fd8-c72a-455e-aae9-52e07e037487.png)

#### Tip of Actor-Critic

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721808178601-89e7ae1a-9e88-4494-9fdd-1e5c53dfcbac.png)<br />Actor-Critic 有一个训练的技巧。 Actor 和 Critic 都是一个网络，Actor 网络的输入是一个游戏画面，其输出是每一个动作的分数 Critic 的输入是游戏画面，输出是一个数值，代表接下来会得到的累积奖励。两个网络，它们的输入是一样的东西，所以这两个网络应该有部分的参数可以共用，尤其假设输入又是一个非常复杂的东西，比如说游戏画面，前面几层应该都需要是卷积神经网络。所以 Actor 和 Critic 可以共用前面几个层，所以在实践的时候往往会这样设计 Actor-Critic。

### Version3.5

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721806725990-e90e1cf7-bfc3-4970-83fe-c65f188089e2.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721807174054-6021a2ac-6c65-48f9-b19f-b8b0f859a07d.png)<br />使用$V^{\theta}(s_x)$作为 Version3 中的 baseline，这样做有什么好处呢？$V^{\theta}(s_x)$是有 Critic 得到的对当前 Observation 的期望 Reward，如果 Actor 采取的 Action 计算得到的 Reward 高于期望 Reward 就得到$A > 0$，反之则$A < 0$。<br />但是这个 Version 有个小问题，$G_t^{'}$是在$s_t$这个画面下，执行$a_t$以后，接下来会得到的累积奖励，是一个采样的结果，而$V^{\theta}(s_x)$是一个平均的结果。用一个采样的结果减去一个平均的结果其实是不太准确的，因为这个采样结果可能特别好或者特别坏，随机性比较大，所以我们要使用平均减去平均。

### Version4

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721807590075-fc1578a7-3e91-44e3-a5c0-e1c8aea84266.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721808127880-4c54d3ab-0616-451a-a4d3-ecbc50b646a6.png)
