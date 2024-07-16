---
title: Lecture6 注意力机制(Self-attention)
tags:
 - 学习笔记
---

[课件下载Lecture6.pdf](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/self_v7.pdf)

## Input

一段文字，语音，图这些都可以看作一组长度不定的vector作为Self-attention的输入<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705653965970-ef33f7f3-ea6e-40d4-bfba-8c7830d9e417.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705654136271-597faf93-ebcb-47c5-8e68-0af02c88135f.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705654150904-a8921268-3c75-4200-98f5-fa9e35b79dbe.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705654172064-b076b01d-ac86-4099-b269-0cd296455e46.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705654186823-bce3dc8b-2b35-4c6d-98c7-203840d8c759.png)

## Output

### N to N

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705654629390-0aa48689-4113-4732-b5f7-02b964670a31.png)

### N to 1

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705654654445-cdb0cd5a-a5d8-4e81-945d-c2264b0922c2.png)

### N to N'

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705654743665-610c5b05-cd95-4370-b11a-193a647e9fc1.png)

## Self-attention

### Background

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705655325610-f3ae8dc3-d348-4c6f-b1ed-86966c1c66d5.png)

### Framework

可以采用一层的Self-attention也可以使用多层的Self-attention<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705657511090-6f6181c3-96c2-441d-a0b6-bbbc99db77de.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705655538762-fe839649-b27e-47de-928b-7f131aef50de.png)

### Algorithm

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705656445255-18afb761-18f4-4aac-89eb-0d5fe766baae.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705657118398-d631a650-e72e-458c-945a-49b5c5da7662.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705657367036-f7d1b916-b135-48df-9456-5b041b001a7c.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705657932567-6fd3b0d4-057d-4b2d-b9dd-0047e1ce8164.png)

### Matrix representation

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705658337729-a61c4f05-1f9f-4ba7-baab-1657ddb71e09.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705658380884-9aa21564-eea4-4b1e-977c-47683b07ddb2.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705658392469-5e9d14d5-93cc-4b64-8c2e-485eb3e1aa14.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705658479100-2a406016-b456-413a-baa1-eb56cfcaffd4.png)

## **Multi-head Self-attention**

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705658896901-5f88f0ca-f513-4728-9121-97025ae70b75.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705658923941-385c1165-0d57-458f-b752-52968eb8d505.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705658943537-3a988db0-4dda-4659-b1cd-d6a5293674e6.png)

## Positional Encoding

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665015634-e2a4e561-2af1-42f9-bfa0-cde30847eb0c.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665058173-27cb947e-a5e0-4bb3-ac11-755486801751.png)

## Applications

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665108010-911540cd-66f8-47ab-b4d9-94ea1ecf0a09.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665115230-4907b335-c732-4b64-9ea6-df7eb8841503.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665129372-ae5de3d0-f624-4197-8342-ae501fe13f76.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665150372-6b7c9888-604a-4994-a00d-c1c7dc7d8c60.png)

## Postscript

### Self-attention和CNN的关系

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665315664-41209c12-6790-49de-ac12-1c0ebbe5953b.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665575016-333765ba-371f-479b-8895-22e3a68da619.png)

### Self-attention和RNN的关系

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705665947584-b18d8fb7-a264-491d-81b6-934971fc9e08.png)

### Self-attention for Graph

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705666110522-4f8bc291-8129-4f49-81e5-86f97b587765.png)

<br />

<br />