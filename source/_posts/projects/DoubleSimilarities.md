---
title: Robust Similarity from Vision-Language Models for Learning with Noisy Labels
date: 2024-06-01 22:39:21
toc: true
tags:
    - Prompt Learning
    - Few Shots Learning
    - Noisy Label Learning
    - Visual Language Models
categories: projects
cover: https://github.com/LZHMS/picx-images-hosting/raw/master/EBlog/Paper/MutualDistance.5fkgzddyoi.webp
excerpt: This post introduces my current research project about how to construct two similarities for better filtering noisy samples and pseudo labels enhancement with the balance between model ability and feature structures.
---
## 研究背景

预训练-微调范式 (Pre-Training and Fine-Tuning(PT-FT)) 已经成为自然语言处理和多模态领域中的主流，针对视觉语言模型，通过提示学习微调预训练模型适配下游数据集已经被广泛地证明有非常好的泛化性能。然而对于许多下游任务场景，获取的数据往往具有很大的噪声，采用的人工标注和校正方法将会耗费大量时间成本，并且针对模型快速迁移应用的需求，我们旨在**探索一种对噪声鲁棒性更强并且采用少样本学习的鲁棒性提示学习机制，能够更好地微调视觉语言模型适配到下游数据集**。

## 研究领域

+ Noisy Label Learning
+ Few-shots Learning
+ Prompt Learning
+ Visual Language Models(VLMs)

## 研究基础

+ 数据：下游任务数据集存在噪声（标注错误）；
+ 模型：Visual Language Models (VLMs)；
+ 目标：在有噪音的下游任务上学习一个 Robust 的模型；
+ 方式：fine-tune pre-trained model，few-shots learning；

## 研究激励

+ 如何构造多个决策体，使得集成决策的策略能够缓解或避免单个模型的偏好作用；
+ 如何利用样本特征获取样本的潜在标签，辅助决策；

## Double Similarities Supervision For Filtering Noisy Samples

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/ZBlog/PDFImage/Framework_V1.361eyqxfdw.webp" alt="POMA Framework" />

### Prompt Similarities By Matrix Learners

+ **Step 1**：对每个类别构造 $m$ 个提示块形成一个提示矩阵；
+ **Step 2**：利用 frozen encoders 在多个决策体指导下获得每个样本的特征矩阵；
+ **Step 3**：集成所有决策体的决策获得每个样本的 prompt-similarity;

### Feature Similarities By Mutual Distance

+ **Step 1**：利用visual encoder 提取所有图像样本的特征；
+ **Step 2**：依据 noisy labels 对提取的特征进行分组；
+ **Step 3**：计算每个类别中各样本的相互距离矩阵，得到单个样本的 feature-distance;
+ **Step 4**：最小化相互距离，即最大化类间相似度，feature-similarity = - feature-distance;

### Robust Similarity Construction

样本提示相似度非常依赖于CLIP的预测能力，在迁移到下游任务初期，模型性能需要进一步提高，其标签预测可信度较低，而样本特征之间的关联性能直接反映噪声样本和干净样本的差别（相对于大多数干净样本的联合特征分布，噪声样本的特征分布显得较为独立，具有差异较大的均值和方差）。因此我们构建基于两者性能平衡的鲁棒性相似度，即在模型学习能力和样本特征潜在结构之间实现 trade-off：

$$
G_i = \alpha \cdot \tilde{y_i} + (1-\alpha) \cdot g_i, \ \ \ \ \ \ (i=1,...,D)\newline
\alpha=0.2\cdot e^{epoch/35}\sim(0.2, 0.8325)
$$

+ 训练初期，模型学习能力较弱，伪标签可信度较低，鲁棒性相似度主要来源于样本潜在结构形成的特征相似度；
+ 训练后期，模型学习能力渐渐提高，伪标签可信度较高，鲁棒性相似度主要来源于模型集成预测的提示相似度；

## How to run

### Requirements

Only for the purpose of verifying the model principles, we just used one GPU: RTX 2080-Ti and trained the prompt learner. The following codes is my constructed shell codes for running only once.

+ Screen 0

```bash
screen -S cuda0
cd scripts
bash train_exp0.sh ssdtd
```

### Shell Codes

We configure all experiments in a shell script so that it's very convenient to conduct _Validation Experiments_ and _Ablation Experiment_. After running experiments, the script immediately did result analysis.

```bash
#!/bin/bash

DATASET=$1
TAG=$2

# Experiments: Training for POMA
# Configuration
# --- dataset: Dtd
# --- prompt blocks m: 1 | 2 | 4 | 6
# --- noise rate: 0 | 12.5% | 25% | 50%
# --- backbone: Text: ViT-B/32-PT, Visual: RN50-PT
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 1 False True rn50_random_init${TAG} 0
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 1 False True rn50_random_init${TAG} 2
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 1 False True rn50_random_init${TAG} 4
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 1 False True rn50_random_init${TAG} 8

CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 2 False True rn50_random_init${TAG} 0
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 2 False True rn50_random_init${TAG} 2
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 2 False True rn50_random_init${TAG} 4
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 2 False True rn50_random_init${TAG} 8

CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 4 False True rn50_random_init${TAG} 0
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 4 False True rn50_random_init${TAG} 2
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 4 False True rn50_random_init${TAG} 4
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 4 False True rn50_random_init${TAG} 8

CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 6 False True rn50_random_init${TAG} 0
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 6 False True rn50_random_init${TAG} 2
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 6 False True rn50_random_init${TAG} 4
CUDA_VISIBLE_DEVICES=0 bash poma_train.sh ${DATASET} rn50_ep50 end 16 16 6 False True rn50_random_init${TAG} 8

# Experiments: Result Analysis for POMA
# Configuration
# --- Experiments: Training for POMA
CUDA_VISIBLE_DEVICES=0 bash parse_test.sh ${DATASET} rn50_ep50 end 16 16 1 False True rn50_random_init${TAG}
CUDA_VISIBLE_DEVICES=0 bash parse_test.sh ${DATASET} rn50_ep50 end 16 16 2 False True rn50_random_init${TAG}
CUDA_VISIBLE_DEVICES=0 bash parse_test.sh ${DATASET} rn50_ep50 end 16 16 4 False True rn50_random_init${TAG}
CUDA_VISIBLE_DEVICES=0 bash parse_test.sh ${DATASET} rn50_ep50 end 16 16 6 False True rn50_random_init${TAG}
```

## Results

### Abalation Study for POMA -- prompt blocks

+ dataset: Dtd
+ noise rate: 0 | 12.5% | 25% | 50%
+ backbone: Text: ViT-B/32-PT, Visual: RN50-PT

| Prompt Blocks |        Noise Rate        |        Noise Rate        |        Noise Rate        |        Noise Rate        |     MeanAcc     |
| :-----------: | :-----------------------: | :-----------------------: | :-----------------------: | :-----------------------: | :--------------: |
|              |             0             |           12.5%           |            25%            |            50%            |                  |
|     PTNL     |          62.86%          |          58.90%          |          53.62%          |          46.19%          |      55.39%      |
|       1       |      61.90% +- 1.29%      |      59.77% +- 1.02%      |      57.68% +- 0.76%      |      49.39% +- 0.31%      |      57.19%      |
|       2       |      62.73% +- 1.00%      |      60.92% +- 0.45%      |      59.65% +- 1.50%      |      49.84% +- 0.89%      |      58.28%      |
|       4       |      62.80% +- 0.51%      |      62.61% +- 0.91%      |      60.56% +- 0.41%      |      52.40% +- 1.10%      |      59.59%      |
|       6       | **63.95% +- 0.54%** | **62.77% +- 0.59%** | **61.17% +- 1.02%** | **53.74% +- 1.67%** | **60.41%** |

### Raw Materials

Model traning logs can be found in the `log.txt` under each experiment directory.

Parsing results can be found in the following files:

+ Dataset: Dtd
  + [POMA RN50_EP50_16SHOTS_1BLOCK ON Dtd](https://github.com/LZHMS/POMA/tree/main/output/ssdtd/POMA/1block_analysis.txt)
  + [POMA RN50_EP50_16SHOTS_2BLOCK ON Dtd](https://github.com/LZHMS/POMA/tree/main/output/ssdtd/POMA/2block_analysis.txt)
  + [POMA RN50_EP50_16SHOTS_4BLOCK ON Dtd](https://github.com/LZHMS/POMA/tree/main/output/ssdtd/POMA/4block_analysis.txt)
  + [POMA RN50_EP50_16SHOTS_6BLOCK ON Dtd](https://github.com/LZHMS/POMA/tree/main/output/ssdtd/POMA/6block_analysis.txt)

## Conclusions

+ prompt matrix 能够有效地缓解模型偏好的作用，能够进一步提高CLIP的在下游任务上的表现，并且在高噪声情况下性能提升更为明显；
+ 构建的鲁棒性相似度能够更好地结合模型特性和样本特征结构，实现更好的迁移性能和噪声鲁棒性。

## References

+ [C. Wu et al. Why Is Prompt Tuning for Vision-Language Models Robust to Noisy Labels? In ICCV, 2023](https://arxiv.org/abs/2307.11978)
+ [K. Zhou et al. Learning to Prompt for Vision-Language Models. In CVPR, 2021](https://arxiv.org/abs/2109.01134)
