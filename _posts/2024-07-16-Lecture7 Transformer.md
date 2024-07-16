---
title: Lecture7 Transformer
tags:
 - 学习笔记
---

[课件下载Lecture7.pdf](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/seq2seq_v9.pdf)

## Applications

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705853801971-94d76a9f-c5df-4dd8-9a28-60cb8b7ed3fe.png)

### Speech Translation

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705884577972-d7a6fdfa-4321-4b53-b2fb-46e531c3069b.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705884566699-2262531e-d7e0-4c3a-bb4f-758f7a8a05bf.png)

### Text-to-Speech(TTS) Synthesis

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705884673076-c290bff4-528e-4df8-9b7a-5d44f878369f.png)

### Chatbot

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705884713182-6d6209d0-d3a7-45d8-ad6f-851fa889dbb9.png)

### Natural Language Processing

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705884930109-ffa7246d-9438-42cd-bd72-65774786b9b2.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705885115929-55fa4005-3cc9-44bc-9017-2606bed0a89e.png)

### **Syntactic Parsing**

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705885155862-cbd8d830-0321-4770-bd82-8495527cf4d4.png)

### Multi-label Classification

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705886068719-1b0288de-290d-4238-803e-7bce4554ebc6.png)

### Object Detection

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705886110760-3d24dc49-e55e-49be-ba5a-411d0548b965.png)

## Transformer

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705886483200-6e6ff588-508e-4f9d-bd79-8f9859f61035.png)

### Encoder

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705886689706-2d8e630e-5bd0-438b-bf32-6c8cab8172a6.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705887140313-7ffef62e-899e-400e-adbb-01ddba5198bf.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705887186496-c023f504-be7d-4512-97d3-42c22b8d204d.png)

### Decoder

#### Decoder – Autoregressive (AT)

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705887487281-2dc728d0-b541-429c-9605-21f743d5775f.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705887573349-d507f7dc-9a3b-47ba-b8ac-417e09b500ca.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705887591355-ebf750d8-2004-45cc-b40d-9132b954f31f.png)Masked Self-attention就是计算$\alpha ^ {'}$时只考虑现输入和以前的输入，因为Decoder的时候$\alpha_1,\alpha_2,\alpha_3,\alpha_4$一个一个产生的，所以在计算$\alpha^{'}$时只能考虑现输入和以前的输入<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705888388589-07d96645-4c8d-4e87-a5b6-c52874f25f62.png)<br /> ![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705889815797-adf8c8af-4b64-4e73-acda-a453724fb34e.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705889865075-f566b45a-7aa7-49a1-8ae4-185f655f06fb.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705889896431-5bd9da63-2ffe-438f-b9db-dc7929bc97a6.png)

#### Decoder – Non-autoregressive (NAT)

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705890323307-c5041270-008a-48fc-873e-50532f7fcbb1.png)

### Cross attention

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705890541821-be68ae5e-b194-450f-8028-19d006f673f9.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705891080740-448f37d1-139d-423c-a830-15421797c77d.png)<br /> ![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705891120345-ee967e98-a211-42c5-a32b-287cbd8fc5cf.png)

## Training

### 训练过程

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705905422634-82cbb80d-1ae9-407c-b21a-5407c08c2a2f.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705905944691-20129c74-0e9c-4cc7-941d-9f255f406efd.png)

### 训练的Tips

#### Copy Mechanism

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705909632255-4f4276bd-97c0-4ef4-9b8a-2044ce444687.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705910481543-d111252a-bc5c-4b63-b29b-0e68f8ffa581.png)

#### Guided Attention

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705924806752-1113e169-5a17-4500-a26b-c1f3e76b925d.png)<br />我们训练的时候强制让它从左到右计算Attention weights<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705924834736-16ba8d6a-596b-46bc-b804-1a5dfec590ce.png)

#### Beam Search

Beam Search可以找到分数最高的路径<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705925931367-14a1c84f-f94d-4626-a7ca-73f02c6ac7ad.png)<br />而有一些需要机器具有一定的创造性的任务Beam Search就不一定有好的效果了，（e.g. sentence completion, TTS）,这时候我们就期待Decoder具有一定的随机性<br /> ![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705926784021-0946b39d-d50c-4ec0-83d6-b6a9e53fb7b0.png)

#### Scheduled Sampling

由于测试的时候Decoder是看着它自己的前一个输出然后输出后一个，如果Decoder输出了一个错误的答案，那么Decoder很有可能就是一步错，步步错，因为我们训练的时候Decoder看到的都是完全正确的情况，所以为了解决这个问题，我们可以在训练的时候就加入一些错误，反而能得到好的训练结果<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705927860597-0999a11d-aa5d-43f6-94bd-68d93fbc24fe.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705928027038-c3f1c83e-9078-404e-8ff0-f026fdabd577.png)