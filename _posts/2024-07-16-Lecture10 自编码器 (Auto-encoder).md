---
title: Lecture10 自编码器 (Auto-encoder)
tags:
  - Hung-yi Lee ML 学习笔记
---

[课件下载 Lecture10](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/auto_v8.pdf)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708506864375-1590c3a2-f362-41f6-b1bf-2e1abaa8bb3e.png)

## Basic Idea of Auto-encoder

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708507938552-f8de326a-eb9c-4589-acdb-80d17b9832c1.png)<br />当然降维的方式还有很多，比如 PCA，t-SNE......<br />问题是为什么可以用一个 low dim vector 来表示原来的图像的特征呢？在下面的例子中，鞭子的多种多样的招法可以看作 high dim 数据，而他的头就可以看作 low dim vector，知道头的运动就可以知道鞭子的招法。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708508508064-56efeb0e-d810-435e-b5e5-936cf8313650.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708509235485-44f4e823-fcd1-4d97-bce5-f5678c96f54c.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708509169886-8c1b4c6b-67e6-4d07-8d83-e7cf3a0d50a9.png)<br />De-noising Auto-encoder<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708519217849-60a254ca-b82e-4134-a8f0-6abb8fc2659d.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708519298268-b4c7abb9-0c67-4ed1-a878-c5df743c0b7b.png)

## Feature Disentanglement

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708519902336-5d3dea2b-ecf0-4316-96ec-f4c34a53a476.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708520378942-a1cb477e-fedc-432a-b030-543e2aee8fdc.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708520552854-b0f614f0-ead7-4105-a7b1-94740a525f1e.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708520581334-84e55286-48f3-484d-8af1-1c16988d8d86.png)

## Discrete Latent Representation

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708521185337-e3af6568-d067-41be-a65f-5c9166cb0e15.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708521602683-3f3481b0-5fc1-4b58-9835-666c4e3d7ce8.png)

## More Applications

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708521789752-455ef2c5-a703-4165-9e3f-6c08d01478c1.png) <br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708521960006-8e9d4567-8d8c-4bee-b526-a09d1e716dab.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708522136998-00381f3f-8de5-4f14-b14d-aca4e5d28f45.png)<br />当然异常检测的方式有很多，这种的优势在于在有些任务中异常是很罕见的，导致训练数据中的异常数据难以收集或者异常数据比较少。使用 Auto-Encoder 的方式训练数据只需要正常的数据，而不需要提供异常的数据，同时也避免了数据的标注。<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708522153478-30815084-a725-4724-9a52-09d7d6583932.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708522460007-7a1077cb-bb59-447c-b7fb-a5116fb541f7.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1708522562664-2e943b84-6aff-444e-a2f8-f5e155df3698.png)<br />
