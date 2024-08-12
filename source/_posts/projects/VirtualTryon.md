---
title: 2D Virtual Try-on Based on Deep Learning
date: 2023-10-05 14:38:42
toc: true
tags:
    - Computer Vision
categories: projects
cover: https://cdn.statically.io/gh/LZHMS/picx-images-hosting@master/Profile/model.4ax0n6qbtbs0.webp
excerpt: This post is the introduction about my project of 2D Virtual Try-on Based on Deep Learning.
---
{% dplayer "url=https://lzhms.oss-cn-hangzhou.aliyuncs.com/videos/projects/2DVirtualTryon.mp4" "autoplay=false" "preload=metadata" "hotkey=true" "lang=en"%}

### 项目介绍

本项目主要面向第 $14$ 届全国服务外包创新创业比赛 $A16$ 赛道虚拟试衣赛题，采用 $2D$ 虚拟试衣技术依托于 $VITON$ 开源数据集训练 $DNN$ 网络并着重进行工程化落地应用；项目选用了前沿顶刊论文的 $PFAFN$ 模型，在此基础上对模型进行优化改进，实现了模型压缩和推理加速并使用 $OpenVINO$ 框架进行部署应用，出色地完成了赛题的要求。

![项目示例](https://cdn.statically.io/gh/LZHMS/picx-images-hosting@master/Profile/examples.4u074u4fgio0.webp)

### 项目开发环境

|   开发平台   |   版本   |      开发工具      |  版本  |
| :-----------: | :------: | :----------------: | :----: |
|    Pycharm    | 2022.3.2 | Visual Studio Code | 1.80.1 |
| Visual Studio |  17.5.5  |                    |        |

|     开发环境     |   版本   |   开发环境   |  版本  |
| :---------------: | :------: | :-----------: | :----: |
| neural-compressor |  2.2.1  |     nncf     | 2.5.0 |
|       numpy       |  1.23.4  |     onnx     | 1.14.0 |
|   opencv-python   | 4.7.0.72 |  onnxruntime  | 1.15.1 |
|     openvino     | 2022.3.0 |    pandas    | 1.3.5 |
|    pytorch-fid    |  0.3.0  |     rembg     | 2.0.50 |
|      pytorch      |  2.0.0  | torch-pruning | 1.1.9 |
|   intel-openmp   | 2021.4.0 |              |        |

### 模型结构介绍

本项目基于 $PFAFN$ 模型重新设计各个网络模块，具体结构如下图所示：

![DNN网络结构](https://cdn.statically.io/gh/LZHMS/picx-images-hosting@master/Profile/model.4ax0n6qbtbs0.webp)

### 项目工程化落地

为了满足赛题方的要求，本项目开展了工程化落地部分，主要分为两个部分，模型训练和模型剪枝量化。项目工程化部署总图如下所示：
![项目工程化部署总图](https://cdn.statically.io/gh/LZHMS/picx-images-hosting@master/Profile/project.1dom5gtegs2o.webp)

### 项目详细技术文档

<iframe src="/pdfjs/web/viewer.html?file=/pdf/projects/详细技术文档.pdf" style='width:100%;height:800px'></iframe>

#### 实验结果：通道剪枝

+ Clothe Warp Module

|          Metrics          | GFLOPs | Para(M) | SIZE(MB) | Total SIZE(MB) | Compresion Ratio |  FID  | FID Loss |
| :-----------------------: | :----: | :-----: | :------: | :------------: | :--------------: | :---: | :------: |
|      Original Module      |  6.63  |  9.37  |   35.8   |     112.0     |     100.00%     | 8.906 |  0.00%  |
| Ratio=0.2 with FineTuning |  5.23  |  7.28  |   27.6   |     88.69     |      79.19%      | 9.013 |  1.20%  |
| Ratio=0.3 with FineTuning |  4.40  |  6.48  |   24.8   |     65.73     |      58.69%      | 9.113 |  2.32%  |
| Ratio=0.4 with FineTuning |  3.79  |  5.61  |   20.4   |     40.97     |      36.58%      | 9.304 |  4.47%  |
| Ratio=0.5 with FineTuning |  3.42  |  4.55  |   16.8   |     35.47     |      31.67%      | 9.977 |  12.03%  |

+ Image Generation Module

|          Metrics          | GFLOPs | Para(M) | SIZE(MB) | Total SIZE(MB) | Compresion Ratio |  FID  | FID Loss |
| :------------------------: | :----: | :-----: | :------: | :------------: | :--------------: | :----: | :------: |
|      Original Module      | 21.93 |  43.90  |   167   |      167      |     100.00%     | 8.906 |  0.00%  |
| Ratio=0.2 with FineTuning | 16.54 |  35.02  |  112.3  |     112.3     |      67.25%      | 9.212 |  3.44%  |
| Ratio=0.25 with FineTuning | 15.45 |  31.93  |  94.39  |     94.39     |      56.52%      | 9.405 |  5.60%  |
| Ratio=0.3 with FineTuning | 13.90 |  29.89  |  80.25  |     80.25     |      48.05%      | 9.679 |  8.68%  |
| Ratio=0.35 with FineTuning | 12.78 |  27.31  |  73.49  |     73.49     |      44.01%      | 9.835 |  10.43%  |
| Ratio=0.4 with FineTuning | 11.20 |  26.12  |  68.52  |     68.52     |      41.03%      | 10.527 |  18.20%  |

+ 最优剪枝方案

| Model | Original Model | Sparsity | Pruned Model |  FID  | FPS |
| :---: | :------------: | :------: | :----------: | :---: | :--: |
|  CWM  |     112MB     |   40%   |   40.97MB   | 9.504 | 2.92 |
|  IGM  |     167MB     |   25%   |   94.39MB   | 9.504 | 2.92 |

#### 实验结果：量化感知训练

|    Optimization    | CPU-FID | GPU-FID | Original Model | Quantized Model |
| :----------------: | :-----: | :-----: | :------------: | :-------------: |
|    Unquantized    |  9.504  |  9.483  |    135.36MB    |    135.36MB    |
|    Quantize CWM    |  9.783  |  9.701  |    40.97MB    |     10.85MB     |
|    Quantize IGM    | 10.382 | 10.249 |    94.39MB    |     24.10MB     |
| Quantize CWM & IGM | 11.503 | 11.379 |    135.36MB    |     34.95MB     |

#### 实验结果：`img2col` 优化加速

|   Runtimes   | CorrTorch(s) | Img2Col(s) |  FPS  | Acceleration Rate |
| :----------: | :----------: | :--------: | :---: | :---------------: |
|    n=1000    |   147.8491   |  94.7902  | 10.81 |      1.5598      |
|   n=10000   |  1489.1325  |  927.4293  | 10.77 |      1.6057      |
| Average Time |    0.1488    |   0.029   | 10.79 |      1.6017      |

### 参考文献

+ Y. Ge, Y. Song, R. Zhang, C. Ge, W. Liu, and P. Luo, "Parser-Free Virtual Try-on via Distilling Appearance Flows," arXiv preprint arXiv:2103.04559, 2021.
+ Y. Cheng, D. Wang, P. Zhou and T. Zhang, "Model Compression and Acceleration for Deep
  Neural Networks: The Principles, Progress, and Challenges," in IEEE Signal Processing Magazine,
  vol. 35, no. 1, pp. 126-136, Jan. 2018, doi: 10.1109/MSP.2017.2765695.
+ [PyTorch Quantization Aware Training](https://leimao.github.io/blog/PyTorch-Quantization-Aware-Training/)
