---
title: Lecture5 卷积神经网络(CNN)
tags:
 - Hung-yi Lee ML 学习笔记
---


[课件下载Lecture5.pdf](https://speech.ee.ntu.edu.tw/~hylee/ml/ml2021-course-data/cnn_v4.pdf)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705580904785-0e9ce279-c2ca-4765-a97f-59aeb417e492.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705581061254-ce72c4f8-7da4-477a-b081-8cc77b55bc16.png)

## Receptive Field
![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705581783916-40c3e3fa-654b-4dfb-ae4f-47a29d2f580d.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705581914383-25b0128f-9080-435a-94a4-eb1152d50ea5.png)
## Parameter Sharing
Same patterns(e.g. 鸟嘴)可以在different regions出现，但是不可能每个receptive field都加一个Same patterns detector，所以我们就需要用到Parameter Sharing<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705582875054-c4b1c6af-0952-4af5-a2a5-5c885d51258e.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705591239893-7c5ab191-ace4-4a91-ba36-8a9b2ba95cac.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705591930957-cfc5ec26-e572-4717-a67b-3f471e607e06.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705592055449-a9b97c7a-ca2d-4c06-b161-2636acf769ef.png)
## Convolutional Layer
![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705592279534-08dc30d1-2cb3-4b89-849b-7f78e4642ddf.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705596988046-b1701fa6-37f2-46e9-b804-c3c44f4fa82a.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705597035922-4ecae8e2-169d-4c1c-ada0-ba7d642b2e58.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705597150492-a86be064-5c53-463a-b408-3c9f259ac1ef.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705597285299-54d7fb53-712d-4faa-af90-1d1c1312eb24.png)<br /> ![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600280392-c24dae2c-620c-4e93-b347-117d13156063.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600335950-50f269b4-46c3-4cad-a386-95666a3ac002.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600359496-cf158d18-affe-4d79-a741-1662f3fa2533.png)
## Pooling
![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600390298-aa6a609f-2624-41da-905c-3d72fb4ad5da.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600507214-5e6f2cc8-3fcf-4b94-bffb-743a377cb553.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600523464-5568af2d-8d37-4095-bf7d-223cf70d112d.png)
## 总结
### CNN框架
![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600598132-811b183b-9e33-4eeb-a222-cfcc8f6cf49f.png)
### CNN应用
![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600708597-069bf5ad-a82b-4da1-923f-6bbadc21fbec.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600723345-039a270b-5bb9-4929-ad9d-0108bce93836.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600836313-d5da1f65-6557-47ac-b23f-4c7b8dbbcd0a.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705600866619-b1c90856-1f8c-45d8-b72d-034084b68bc7.png)<br />![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1705601017557-e16bbd23-f633-4508-9dbf-c44323abce01.png)

