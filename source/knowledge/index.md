---
title: Knowledge
date: 2023-11-11 19:01:01
toc: true
---
## Introduction
These days I have been sentimental about the fact that I had learned so much knowledge but what's the really helpful to my current learning or research is not so directly accessible.

Until now, I have learned for at least fifteen years covering basic subjects from chinese, mathematics, english, physics, chemistry, biology, geography, history to politics and engineering subject of computer science. However, to be honest, I have forgotten a lot of them which is not so necessary or neccessay for my current learning. In other words, I cann't rethink them immediately when I engage in the related work. 

In my views, the sense of immediate recall is very important for research innovation which allows us to know what kind of knowledge there exists and how to reform the existed knowledge in new environment.

So I have decided to develop this habit of recording learned knowledge which has the potential in motivating my future learning or research.

## Knowledge List
### 仿射变换(Affine Transformation)
线性模型:
$$
price = w_{area}\cdot area + w_{age} \cdot age + b
$$
仿射变换的特点是通过加权和对特征进行线性变换，并通过偏置项进行平移。
### 非线性频率压缩
在滤波器设计中将整个模拟频率轴压缩到 $\pi/T$ 之间，使得 $H_a(s), s=j\Omega$ 压缩为 $\widehat{H_a}(s_1),s_1=j\Omega_1$, 可以利用正切变换实现频率压缩模型：
$$
\Omega = \frac{2}{T}\tan(\frac{1}{2}\Omega_1T)
$$
这个设计思想实质上利用了正切函数定义域有限、值域无限以及奇函数的性质；推而广之，这种设计可以实现特定的单值压缩方法，也可以实现值域的延展。

一些类似的函数特性，对数函数，指数函数分别适合于定义域、值域取值 $0 \sim 1$ 之间的情况，但是对目标域都有所限制，因此这些函数往往没有正切函数具有优良的特性。

