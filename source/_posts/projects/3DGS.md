---
title: 3D Gaussian Splatting 真实场景的光场图像渲染
date: 2024-08-02 15:43:30
toc: true
tags:
    - 3D Vision
    - 3DGS
categories: projects
cover: https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802170930.png
excerpt: 本文介绍如何利用 3D Gaussian Splatting(3DGS) 实现真实场景的光场图像渲染.
---
## 一、基本原理实现

​	3DGS 将稀疏的点云变成 3D 空间中的椭球体，每个椭球体拥有位置、颜色、不透明度、协方差（大小），当混合在一起时，可以产生从任何角度渲染的完整模型的可视化效果；

#### 整体框架

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240801111743.png" style="zoom:150%;" />

1. 通过 SfM 获取初始化稀疏点云（采样点）；
2. 基于初始化点云生成 3D 高斯椭球集；
3. 利用投影矩阵将 3D 高斯椭球投影的 2D 平面；
4. 进行场景渲染（分 tile 16*16）；
5. 计算 Loss 以及梯度回传；
6. 基于梯度自适应改变点云的分布方式；

## 二、构建场景数据

​	针对实际场景重建需要获取场景各个角度的图片，并且每张照片尽量保持一致的曝光。由于也可以录制视频提取单帧来获取图片，因此构建场景数据集主要通过以下两种方式：

+ 获取重建场景各个角度图片，尽量保持一致曝光，推荐100-1000张；

+ 利用 [FFMPEG](https://ffmpeg.org/download.html)工具构建真实场景数据集

  ​	使用的基本命令如下，`video_addr` 指的是实际视频文件地址，`FPS` 主要影响每秒采多少帧的图像：

```bash
  ffmpeg -i {video_addr} -qscale:v 1 -qmin 1 -vf fps={FPS} %04d.jpg
```

+ 构建的场景数据集
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802140442518.png" alt="Playroom Scene" width='70%'/>

+ 数据集目录结构

```apl
<dataset_name>
|---input
|   |---<image 0>
|   |---<image 1>
|   |---...
```

## 三、获取相机位姿

​	3DGS 需要利用稀疏点云作为输入进行建模，因此我们需要针对场景数据集建立点云。本项目中我们可以利用 [COLMAP](https://github.com/colmap/colmap/releases) 获取相机位姿建立点云，安装完成后在终端执行以下命令，完成相机位姿和点云的建立：

```python
python convert.py -s data/dataset_name
```
+ 可视化相机位姿与点云

  借助 COLMAP 工具我们可以对真实场景建立稀疏点云以及对应的相机位姿。

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802140901308.png" alt="Playroom场景的相机位姿" width='70%'/>

+ 数据集目录结构

```apl
<dataset_name>
|---input
|---distorted
|---images
|---sparse
|   |---0
|   |   |---cameras.bin
|   |   |---images.bin
|   |   |---points3D.bin
|---stereo
|---...
```

## 四、3DGS训练模型

​	针对构建好的真实场景数据以及 COLMAP 估计出的相机位姿，利用 3DGS 训练场景数据集对应的模型：

```python
python train.py -s data/playroom
```

+ 模型训练损失

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802142955471.png" alt="模型训练损失变化" width='70%'/>

+ 场景渲染对比

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802143038867.png" alt="DSC5623视图渲染对比" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802143110425.png" alt="DSC5683视图渲染对比" width='70%'/>

​	训练完成后，会得到如下目录结构，其中 `point_cloud.ply` 就是训练好的点云模型文件。

```apl
output
|---<Env_ID>
|   |---point_cloud
|   |   |---iteration_7000
|   |   |   |---point_cloud.ply
|   |   |---iteration_30000
|   |   |   |---point_cloud.ply
|   |---cameras.json
|   |---cfg_args
|   |---events.out...
|   |---input.ply
```

## 五、渲染真实场景
​	利用 SIBR Viewers 可以利用训练好的模型文件渲染实际场景，具体在终端执行以下命令：

```bash
./viewers/bin/SIBR_gaussianViewer_app -m <path to trained model>
```
+ 整体渲染效果

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802144305724.png" alt="Playroom Scene I" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802150102642.png" alt="Playroom Scene II" width='70%'/>

+ 不同缩放系数的影响
直观地感受到，从最开始的稀疏点云建立的椭球形模型不断优化，逐渐的渲染出整个实际场景。
<div class="justified-gallery">
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802145014411.png" alt="0.001" />
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802145055052.png" alt="0.108" />
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802145138578.png" alt="0.306" />
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802145525962.png" alt="0.863" />
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802145658495.png" alt="1.000" />
</div>

## 六、其他真实场景的渲染
从构建的数据集以及渲染结果得出，场景视角越多，渲染出的场景图像质量越高，对于最后一个教学楼数据集，仅从单个视角构建图像时，渲染的3D场景效果就非常差。因此，如果想要得到质量更高的渲染场景，可以从多个视角拍摄一段连续的视频，再从中提取帧构造场景数据集。
<div class="justified-gallery">
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802160556.png" alt="两个连通的房间" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802161256.png" alt="连通的房间其一" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802161356.png" alt="连通的房间其二" width='70%'/>
</div>

<div class="justified-gallery">
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802160942.png" alt="火车" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802170717.png" alt="场景渲染一" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802170227.png" alt="场景渲染二" width='70%'/>

</div>

<div class="justified-gallery">
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802161042.png" alt="卡车" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802170844.png" alt="场景渲染一" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/20240802165751.png" alt="场景渲染二" width='70%'/>
</div>

<div class="justified-gallery">
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802134315080.png" alt="教学楼一角" width='70%'/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/papers/xdu/image-20240802151136722.png" alt="场景渲染" width='70%'/>
</div>
