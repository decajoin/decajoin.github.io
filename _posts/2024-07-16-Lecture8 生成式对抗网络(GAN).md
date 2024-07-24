---
title: Lecture8 生成式对抗网络(GAN)
tags:
  - Hung-yi Lee ML 学习笔记
---

[课件下载 Lecture8.pdf](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/gan_v10.pdf)

## GAN Overview

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706083657918-b78ef08d-2162-4aaf-a106-602940f1f9f4.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706083728295-4b28d2f2-0d03-4aab-a288-7e9f887c4a1c.png)<br />Generator<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706083794328-fa463b1c-0945-4811-a6f4-b605b85eddda.png)<br />Discriminator<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706083909300-ec7c2fb3-ef02-428a-af2a-eb4e765447b3.png)<br />Basic Idea of GAN<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706084261853-275cb8dc-9369-4608-a384-087cd0984ec5.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706084466777-ebda1db0-1f42-4500-9b0e-fee7e6d7a308.png)<br />Algorithm<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706084648590-1c012c9d-74bb-432a-9745-0cd211db0c5e.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706084714744-59a2c295-9ebe-4782-ac23-cbb5da760228.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706084736778-cf45c523-fb07-4bd5-a719-95ec4c35a817.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706174775403-063df4cf-db4f-4455-a7e5-31a2c3d28f11.png)

## Theory behind GAN

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706085193668-1297e69b-5460-4f39-a51a-5bebec589f8f.png)<br />由于我们不知道$P_G$和$P_{data}$的具体分布，所以不能直接计算两个分布的差异，但是我们可以在两个分布中采样，然后计算两个分布采样出的结果的 Divergence<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706085562014-92a238e9-cef0-48a3-81cc-a0d35bc19f11.png)<br />JS divergence 是描述两个概率分布的差异的一种方法，Discriminator 的目标是鉴别目标到底是真实数据还是 Generator 产生的数据，所以它要给真实数据一个较高的分数，给 Generator 生成的数据一个较低的分数，所以 Discriminator 要让真实数据和 Generator 得到的数据的 JS divergence 最大。但是 JS divergence 作为 loss function 的时候比较复杂，而数学证明$V(G, D)$与 JS divergence 之间具有一定的关系，所以在训练 Discriminator 的时候采用的是$V(G, D)$，我们只需要找到$max V(G, D)$<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706086930245-c138300c-a72a-40d1-b2b8-ee9b7b194cd3.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706087656422-785b6938-ec16-4baf-a0e8-1654ee84e14b.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706088134153-953330a4-7a2b-4fb4-b5fb-fbb4e0cfc542.png)<br />Other Divergence<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706088172018-59b63b71-653b-4453-90ad-51f29ca15244.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706088210387-76ffcad9-e86f-42bf-a177-b0cfdc632261.png)

## Tips for GAN

### The problem of JS divergence

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706088978101-81c37dd2-4ba0-4fc9-b1be-4c090e4ed29a.png)<br />JS divergence 并不适合用来衡量两个分布的差异，因为：

1. 考虑数据的特性，$P_{data}$和$P_G$都是高维空间的低维流形(拿生成动漫头像为例子，动漫头像就是处于一个$pixel \times pixel \times channel$这样高维度空间中的一小部分具有某些特征的空间，就是一个低维度镶嵌在一个高维度的一个特定空间，它不具有高维度空间的性质)，所以他们有 overlapped 的几率是很小很小的，几乎可以看作没有重叠，所以 JS divergence 很难进行下去。
2. 就算有一定的 overlapped 的部分，但是我们还要进行采样，如果我们的样本不是很多的话，也算没有重叠。

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706088658572-3652f714-6495-45d6-bcbb-7341735a760c.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706095488710-3545e45d-6403-40ee-bb9d-05bc03e410a0.png)

### Wasserstein distance

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706091356011-163c0a8f-35a0-41ef-920b-9a8f46f03fc7.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706097845869-73cd819c-8bb3-44cd-90a6-811a269d07b8.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706097871300-0d4fa85a-c53e-4428-b4d1-6be3435f8731.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706174004407-7dbe6348-bdfa-485f-a9ae-41925776d3ef.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706174298606-57383bdf-56b8-43a4-96f6-44487ebcbeec.png)

## Conditional Generation

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175328488-9a3e6c50-74b3-4c36-9cde-1e29a25d3d2c.png)<br />训练数据是成对的数据(text-image pairs)有图片和图片的文字描述<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175377684-709e35e6-7756-4060-83b6-e5537069473c.png)<br />Applications 除了 Text-to-image 之外还有......![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175504947-5d53295d-9403-4ab3-abc9-6b0b42283df2.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175643439-b9487d4d-92bf-4845-89ef-dbef37225565.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175623006-f4a3397d-cd68-4ef8-bc83-350668ebd348.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175666811-e503bdd3-35ab-493b-bacb-331f04e997cf.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175679031-fea26dd7-cb8c-4766-a134-7619595af2ff.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175708543-5ae296ae-83b4-4d21-a698-19637edf2ba9.png)

## Learning from Unpaired Data

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175854179-ff864cfd-a309-4c73-b1bb-8185c2654d8e.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706175898182-091da21f-eca2-4dcf-a6d1-f199ad2786ad.png)<br />Cycle GAN<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176169866-1630faf2-1e68-41a8-bc50-8baec83eb5d6.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176238453-ddeff542-fe5b-47be-bdee-a35114f91edb.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176280625-bd26fbc6-2c8b-49fa-8c85-8b7bb7cbe3ba.png)<br />StarGAN<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176337421-a50a704e-1a5b-4289-a087-64a7066614d4.png)<br />Other Unpaired Data Applications<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176425886-23dc9fd9-b166-4ac6-90aa-08a4de5902cf.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176435984-6c7714fe-10a2-49d9-9373-88e56f293665.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176455547-bc5b215b-5441-472d-9c62-52598f23803a.png)

## Evaluation of Generation

我们怎么评估 GAN 生成出来的图像质量呢？使用人力的话，耗费巨大不说而且难以让人信服。我们可以采用图像分类的神经网络，它的输出就是一个 distribution 的所有 class 的概率，如果某一个 class 的概率很高，远高于其他 class 的概率，我们就可以认为 GAN 生成的图像质量较高，反之说明 GAN 生成的图像质量较低。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176634585-99706696-a220-441f-879d-a36b3ab12d68.png)<br />但是，使用上述方法对 GAN 生成的图像进行判断的时候，会存在以下问题......

### Mode Collapse

如果 GAN 产生的图片质量确实很高，图像分类的结果也很明显，但是它翻来覆去就那几张图片，没有多样性，这个问题叫做 Mode Collapse。 <br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706176927950-ff7f1600-6486-47c0-8473-390f5832420f.png)

### Mode Dropping

如果 GAN 生成的图片虽然质量高而且有多样性，但是它只学习了样本的一部分特征，生成的图片在某一个特征上都一样(e.g. 下图中产生的图片肤色要么偏白，要么偏黄，在肤色在一个特征上所有生成的图片都差不多，这显然不是我们想要的，但是利用图像分类来评估 GAN 的结果是无法发现这个问题的)，这个问题叫做 Mode Deopping。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706177243381-b42d819b-639b-4263-94d2-c099e409bce6.png)

### **Inception Score**

当然，Mode Collapse 和 Mode Dropping 也不是没有办法解决，我们可以同时考虑生成图像的质量和生成图像的多样性，对生成的所有图像计算多样性，对生成的单个图像看图像分类 class 的概率。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706198718578-6608a7ff-c60e-40ec-90d8-8b0dc6c80d4b.png)

### FID

训练一个 CNN 网络，把真实图片和生成的图片都放入 CNN 网络，将它们的进入 softmax 前一层隐层中的分布拿出来计算距离，距离越小代表越相似<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1706200278752-02184819-727a-464f-bcbd-8a59bdc119af.png)
