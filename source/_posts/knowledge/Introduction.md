---
title: Knowledge Introduction
date: 2023-11-11 19:01:01
toc: true
tags:
    - Life Knowledge
categories: knowledge
excerpt: An introduction of my knowledge list.
---

{% message color:info title:科学研究定义 size:default%}
+ 国家教育部对科学研究的定义：科学研究是指为了增进知识包括关于人类文化和社会的知识以及利用这些知识去发明新的技术而进行的系统的创造性工作;
+ 美国资源委员会对科学研究的定义：科学研究工作是科学领域中的检索和应用，包括对已有知识的整理、统计以及对数据的搜集、编辑和分析研究工作;
+ 《科研项目完全指南：从课题选择到报告撰写》：剑桥在线词典将“研究”定义为“对某一主题的详细探究，特别是为了发现（新）信息或达成（新）理解”;
+ 研究包括三个步骤：提出问题；收集用以回答问题的数据；给出问题的答案。
{% endmessage %}

## Introduction
These days I have been sentimental about the fact that I had learned so much knowledge but what's the really helpful to my current learning or research is not so directly accessible.

Until now, I have learned for at least fifteen years covering basic subjects from chinese, mathematics, english, physics, chemistry, biology, geography, history to politics and engineering subject of computer science. However, to be honest, I have forgotten a lot of them which is not so necessary or neccessay for my current learning. In other words, I cann't rethink them immediately when I engage in the related work. 

In my views, the sense of immediate recall is very important for research innovation which allows us to know what kind of knowledge there exists and how to reform the existed knowledge in new environment.

So I have decided to develop this habit of recording learned knowledge which has the potential in motivating my future learning or research.

## Knowledge List
### Mathematical Principles
#### 仿射变换(Affine Transformation)
线性模型:
$$
price = w_{area}\cdot area + w_{age} \cdot age + b
$$
仿射变换的特点是通过加权和对特征进行线性变换，并通过偏置项进行平移。
#### 非线性频率压缩
在滤波器设计中将整个模拟频率轴压缩到 $\pi/T$ 之间，使得 $H_a(s), s=j\Omega$ 压缩为 $\widehat{H_a}(s_1),s_1=j\Omega_1$, 可以利用正切变换实现频率压缩模型：
$$
\Omega = \frac{2}{T}\tan(\frac{1}{2}\Omega_1T)
$$
这个设计思想实质上利用了正切函数定义域有限、值域无限以及奇函数的性质；推而广之，这种设计可以实现特定的单值压缩方法，也可以实现值域的延展。

一些类似的函数特性，对数函数，指数函数分别适合于定义域、值域取值 $0 \sim 1$ 之间的情况，但是对目标域都有所限制，因此这些函数往往没有正切函数具有优良的特性。

## Research Innovation
### Learning With Noisy Labels
+ [Co-teaching: Robust Training of Deep Neural Networks with Extremely Noisy Labels](http://arxiv.org/abs/1804.06872)
    + Innovation: DNN的相互指导学习机制，两个模型分别动态地选取一些干净样本相互提供给对方进行学习，目的是过滤不同类型的噪声；
    + Learning Points: 直觉是同辈相互纠错的学习机制，而且训练中其样本选取的动态性值得一提；
+ [DivideMix: Learning with Noisy Labels as Semi-supervised Learning](http://arxiv.org/abs/2002.07394)
    + Innovation: 对噪声数据集进行划分，并同时学习两个模型进行相互指导、标签集成，以克服不同类型的噪声；
    + Learning Points: Mutual Learning 和 交互学习一定程度上可以增强模型鲁棒性；

## Project Habits
### Package Management
#### Miniconda Configuration
```bash
############ Conda Environment Installation ############

# Fetch the miniconda script
export HOME=$PWD
wget -q https://repo.anaconda.com/miniconda/Miniconda3-py37_4.12.0-Linux-x86_64.sh -O miniconda.sh
sh miniconda.sh -b -p $HOME/miniconda3
rm miniconda.sh
export PATH=$HOME/miniconda3/bin:$PATH

# Initialize conda
source $HOME/miniconda3/etc/profile.d/conda.sh
hash -r
conda config --set always_yes yes --set changeps1 yes

# Create new environment
conda create -n my_env python=3.8
conda activate my_env
```
