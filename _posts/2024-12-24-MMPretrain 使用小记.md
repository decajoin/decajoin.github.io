---
title: MMPretrain 使用小记
tags:
  - 小记
---

## 1. 环境配置

本机硬件信息如下

![image-20241224205934451](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/image-20241224205934451.png)

### 1.1. 安装 Pytorch

首先创建一个新的 Conda 环境

```bash
conda create --name torch python=3.10 -y
conda activate torch
```

本机 CUDA 版本查询结果是 11.7

```bash
nvcc --version
```

![image.png](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/1732612233419-bc6e2ed8-8c98-48d1-b084-99176a5d5b41.png)

安装原则是 Pytorch 的 CUDA 版本需要小于本机的 CUDA  版本

安装地址 https://pytorch.org/get-started/previous-versions/

```bash
# CUDA 11.6
conda install pytorch==1.13.1 torchvision==0.14.1 torchaudio==0.13.1 pytorch-cuda=11.6 -c pytorch -c nvidia
```

### 1.2. 安装 MMPretrain

官方文档 https://mmpretrain.readthedocs.io/en/latest/get_started.html#id2

```bash
git clone https://github.com/open-mmlab/mmpretrain.git
cd mmpretrain
pip install -U openmim && mim install -e .
```

进行验证安装

```bash
python demo/image_demo.py demo/demo.JPEG resnet18_8xb32_in1k --device cpu
```

如果结果如下则说明上述环境配置成功

![image-20241224210718311](https://yeyi0003.oss-cn-hangzhou.aliyuncs.com/image-20241224210718311.png)

如果验证安装时，提示 NumPy 版本不兼容问题，可以参考如下方法。

> NumPy 从 1.x 升级到 2.x 之后，可能会导致一些依赖于 NumPy 1.x 版本编译的模块不兼容，并在调用时崩溃，所以需要对其进行降级

```bash
pip install "numpy<2"
```

## 2. 学习小记

### 2.1. MMPretrain 文件结构

> mmpretrain
>
> -configs 配置文件
>
> -data 数据集存储路径
>
> -demo 入门案例
>
> -docs 中英文文档教程
>
> -mmpretrain 模块化代码
>
> -projects 工程实例
>
> -resources 图片、视频等静态资源
>
> -tests 组件维度测试脚本
>
> -tools 训练、测试、可视化等工具集
>
> -work_dirs \ 运行代码产生的文件存放地

### 2.2. 现有模型 + 现有数据集

```bash
python ./tools/train.py ./configs/resnet/resnet18_8xb16_cifar10.py
```

### 2.3. 现有模型 + 自定义数据集

1. 在 mmpretrain 目录下新建目录 data 用于存放自定义数据集
2. 在 configs 文件中选择目标模型
3. 运行命令

```bash
python ./tools/train.py ./configs/efficientnet/efficientnet-b0_8xb32_in1k.py（你选择的模型的配置文件的文件位置）
```

4. 得到一个 work_dirs 目录和对应模型的目录
5. 在模型目录中的配置文件中修改如下几处

> 1. batch_size 的大小
> 2. num_workers 的大小
> 3. data_root 设置为自定义数据集文件夹位置
> 4. type 设置为 CustomDataset
> 5. 注释掉 split 参数

6. 运行命令开始训练

```bash
python ./tools/train.py ./work_dirs/efficientnet-b0_8xb32_in1k/efficientnet-b0_8xb32_in1k.py（刚才修改的模型配置文件的文件位置）
```

### 2.4. 常用命令记录

```bash
# 训练
python ./tools/train.py work_dirs/davit-base_4xb256_in1k/davit-base_4xb256_in1k.py

# 绘制 loss 曲线
python tools/analysis_tools/analyze_logs.py plot_curve your_log_json  --out ./custom/results_loss.jpg

# 绘制 top-k 曲线
python tools/analysis_tools/analyze_logs.py plot_curve your_log_json --keys accuracy/top1 accuracy/top5  --legend top1 top5 --out ./custom/results.jpg

# 在同一图像内绘制两份日志文件对应的 top-1 准确率曲线图
python tools/analysis_tools/analyze_logs.py plot_curve log1.json log2.json --keys accuracy/top1 --legend exp1 exp2 --out ./custom/results_loss.jpg

# 统计训练时间
python tools/analysis_tools/analyze_logs.py cal_train_time  your_log_json
```

