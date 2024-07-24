---
title: Lecture13 领域自适应(Domain Adaptation)
tags:
 - Hung-yi Lee ML 学习笔记
---


[课件下载](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/da_v6.pdf)

## Domain Shift

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721361678664-591dbc75-297a-4db3-895a-4a03680dcd5d.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721361961147-a278c14b-62f9-4db7-9070-184403f8fa17.png)

## Domain Adaptation

Domain Adaptation依据我们对target domain的了解程度有不同的类型。

### Little but labeled

![](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721382997821-77ee13c2-af6e-4685-bc8b-cbdf2ed936f4.png)<br />对于少量有标注的target domain，可以在训练出来的model的基础上进行微调(fine-tune)，但是由于数据量很少要注意overfitting问题，只需要训练几个epoch就行。

### Large amount of unlabeled data

![](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721383190499-ea39514c-5a13-4709-907a-a24f1a0ff898.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721383218090-aa3be8f5-a905-4224-adfa-038d4a8d0969.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721383389588-fa0e3efd-07ea-4e8b-9e80-9a0768ffa01f.png)<br />对于这个类型的Domain Adaptaion基本思路是Source Domain和Target Domain经过Feature Extractor后得到的特征分布相似。就像上图中蓝色点和红色点的分布几乎没有什么明显的差别。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721383616826-bb3ad9bb-eaf1-42be-bc78-35faf870d5e2.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721384946800-6dd35110-1140-48a4-b77a-c373f03f1f15.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721387025804-b88e1d3c-4209-4f4c-961a-23204c13fa27.png)<br />领域分类器的工作是把源领域跟目标领域分开，根据特征提取器的特征，来判断数据是来自源领域还是目标领域，把源领域和目标领域的两组特征分开。而特征提取器要做的事情跟领域分类器相反想要骗过Discriminator，有点类似于GAN，其实起到的效果也是将两组特征分开，但是我们其实想要的是让Feature Extractor输出的两组特征没有明显的差异，所以这个方法并不是最好的方法，但是it works。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721447172927-277ff5b0-871a-49f0-b084-73a14b53c591.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721447206960-5a5fb099-1ece-4a23-a2dd-bf160e30cba0.png)<br />刚才这整套想法，有一个小小的问题。用蓝色的圆圈和三角形表示源领域上的两个类别，用正方形来表示目标领域上无类别标签的数据。可以找一个边界去把源领域上的两个类别分开。训练的目标是要让正方形的分布跟圆圈、三角形合起来的分布越接近越好。在图（a）所示的情况中，红色的点跟蓝色的点是挺对齐在一起的。在图（b）所示的情况中，红色的点跟蓝色的点是分布挺接近的。**虽然正方形的类别是未知的，但蓝色的圆圈跟蓝色的三角形的决策边界是已知的，应该让正方形远离决策边界。**因此两种情况相比，我们更希望在图（b）的情况发生，而避免让在图（a）的状况发生。

### little and unlabeled

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721447656563-47160f91-3dee-4f74-baca-5002cfc2b176.png)<br />**目标领域的数据不仅没有标签，而且还很少。**比如目标领域只有一张图片，也就无法跟源领域对齐。 可以参考测试时训练（Testing Time Training， TTT）这篇论文。

## Domain Generalization

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721448019959-e810031b-2fe8-4ec9-aedc-856855395df4.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1721448053874-ff54aae6-a72d-4544-861c-abdbf1fd6a19.png)<br />**对目标领域一无所知，并不是要适应到某一个特定的领域上的问题通常称为领域泛化。**领域泛化可又分成两种情况。一种情况是训练数据非常丰富，包含了各种不同的领域，测试数据只有一个领域。如图 13.10（a）所示，比如要做猫狗的分类器，训练数据里面有真实的猫跟狗的照片、素描的猫跟狗的照片、水彩画的猫跟狗的照片，期待因为训练数据有多个领域，模型可以学到如何弥平领域间的差异。当测试数据是卡通的猫跟狗时，模型也可以处理，具体细节可参考论文“Domain Generalization with Adversarial Feature Learning”。另外一种情况如图 13.10（b）所示，训练数据只有一个领域，而测试数据有多种不同的领域。虽然只有一个领域的数据，但可以想个数据增强的方法去产生多个领域的数据，具体可参考论文“Learningto Learn Single Domain Generalization”。