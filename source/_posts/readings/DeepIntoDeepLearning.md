---
title: Deep Into Deep Learning
date: 2023-11-06 23:39:55
toc: true
index: false
tags:
	- Deep Learning
categories: readings
excerpt: This is an article about the course of Deep Into Deep Learning taught by Mu Li.
---
## 第一讲 线性回归

### 线性模型

$$
price = w_{area}\cdot area + w_{age} \cdot age + b
$$

### 基本概念

#### 小批量随机梯度下降(Minibatch Stochastic Gradient Descent)

为了加宽执行计算损失函数关于模型参数的导数，通常会在每次需要计算更新的时候随机抽取一小批样本。

#### 超参数(Hyperparameter)

可以调整但不在训练过程中更新的参数。

#### 调参(Hyperparameter Tuning)

选择超参数的过程

#### 泛化(Generalization)

寻找一组最佳超参数能够在未知的数据上实现较低的损失。

## 卷积神经网络

### 超参数

+ 填充

  + 无填充时
    输入图像 $Size=(n_h, n_w)$，卷积核 $Size=(k_h, k_w)$

  $$
  _h\times n_w  \Longrightarrow (n_h-k_h+1)\times(n_w-k_w+1)
  $$

  + 填充图像时
    假设填充 $p_h\times p_w$

  $$
  _h\times n_w  \Longrightarrow (n_h-k_h+p_h+1)\times(n_w-k_w+p_w+1)
  $$

  通常取 $p_h=k_h-1, p_w=k_w-1$, 此时输出 $n_h\times n_w$
  + $k_h = 2k+1, k\in Z$, 上下两侧填充 $p_h/2$
  + $k_h = 2k, k\in Z$, 上侧填充 $\lceil p_h/2\rceil$, 在下侧填充 $\lfloor p_h/2\rfloor$
+ 步幅
  高度和宽度的步幅分别设定为 $s_h, s_w$, 则输出图像形状:

  $$
  lfloor (n_h-k_h+p_h+s_h)/s_h\rfloor \times \lfloor(n_w-k_w+p_w+s_w)/s_w\rfloor
  $$

  + 如果 $p_h=k_h-1, p_w=k_w-1$, 则得到 $\lfloor(n_h+s_h-1)/s_h\rfloor \times \lfloor(n_w+s_w-1)/s_w\rfloor$
  + 如果 $n_h, n_w$ 可以被 $s_h, s_w$ 整除， 则得到 $(n_h/s_h)\times(n_w/s_w)$
+ 超参数说明

  + 一般用填充使得输入和输出的尺寸一致 $p_h=k_h-1, p_w=k_w-1$
  + 步幅等于1最好，提取信息最充分，但是模型的计算复杂度高，需要设置较多的网络层，通常步幅取 2 使得尺寸减半，降低计算复杂度。一般将步幅为 2 的网络层均匀插入到网络模型中，控制整体的复杂度。
  + 卷积核的边长一般取奇数，为了使得填充具有对称性，即 $p_h=k_h-1, p_w=k_w-1$，但对模型结果影响不大
  + 卷积核比较小时，可以增加网络层数来保证足够的感受野

## References

+ [李沐, 动手学深度学习](https://courses.d2l.ai/zh-v2/)
