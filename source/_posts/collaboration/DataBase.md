---
title: 数据库系统概论
date: 2023-10-19 20:10:12
toc: true
tags:
    - Collaboration Project
categories: collaboration
cover: https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/covers/xdu/wp2347580-database-wallpapers.jpg
excerpt: 计科院数据库系统概论课程学习笔记
---
## 第一讲 数据库系统概述

### 数据库基本概念

{% message color:info title:数据库(DataBase,DB) %}

+ 概念: 长期存储在计算机内、有组织、可共享的数据集合
+ 特点: 永久存储、有组织、可共享
  {% endmessage %}

{% message color:info title:数据库管理系统(DataBaseManagementSystem,DBMS) %}

+ 概念: 专门用于管理数据库的软件
+ 组成: 相互关联的数据集合、访问数据的程序
  {% endmessage %}

{% message color:info title:数据库系统(DataBaseSystem,DBS) %}

+ 概念: 引入数据库之后的计算机系统
+ 特点: DBS = DB + OS + DBMS + App + DBA +Users
  {% endmessage %}

### 数据库发展阶段

- 人工管理阶段
  - 数据不保存
  - 用户/应用程序管理数据
  - 数据不共享，不独立
  - 数据无结构
- 文件系统阶段
  - 数据可以长期保存
  - 文件系统管理数据
  - 数据共享性差，冗余度大
  - 物理独立性好，逻辑独立性差
  - 记录内有结构，整体无结构
- 数据库系统阶段
  - 数据可以永久保存
  - 数据由DBMS管理
  - 数据共享性高，冗余度小
  - 具有高度的物理独立性，较好的逻辑独立性
  - 统一数据模型，整体结构化

数据库管理系统采用*外模式-模式-内模式*三级模式，*外模式/模式*和*模式/内模式*两级映像结构来实现的。

### 数据模型
- 是数据及其联系在计算机中的表示和组织形式的描述
- 组成三要素: 数据结构、数据操纵、数据完整性约束
- 数据库模型
  - 概念模型
    - E-R图
  - 逻辑模型

    - 层次模型
      - 树状结构: 每个节点是基本单位称为记录，记录之间的联系以树形结构存储
      - 特点: 只能处理一对多联系，无法处理多对多联系
    - 网状模型
      - 网状结构(有向图): 记录之间的联系用连线表达，联系必须标注名称
      - 特点: 将多对多联系转换为多个一对多联系
    - 关系模型
      - 实体和联系都作为数据文件存储
  - 物理模型

### 数据库系统结构

- 数据**逻辑独立性**：由**外模式/模式**映像保证（当模式改变，仅修改映像，即可保证外模式不变）
- 数据**物理独立性**：由**模式/内模式**映像保证（当内模式改变，仅修改映像，即可保证模式不变）

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/Untitled-3.5x31v922ayk0.webp" width="70%"/>

## 第二讲 关系模型

### 关系数据结构

关系模式: $R(U, D, Dom(), F)$ (简称: $R(U)$)
其中，$R$ 表示关系名，$U$ 表示属性集，$D$ 表示关系的域，$Dom$ 表示属性到域上的映射关系，$F$ 表示数据依赖

### 关系代数

+ 关系代数(Relational Algebra)是**过程化**的查询语言，是以**集合**为基础的运算表达式

#### 传统集合运算

+ 并(Union): From the row angle

<center> $R \cup S$={$t|t\in R\vee t\in S$}</center>

+ 差(Difference): From the row angle

<center>$R - S$={$t|t\in R\wedge t\notin S$}</center>

+ 交(Intersection): From the row angle

<center>$R \cap S$ = {$t|t\in R\wedge t\in S$}=$R-(R-S)$</center>

+ 广义笛卡尔积(Cartesian Product): From the row angle

<center>$R \times S$ = {$\widehat{t_rt_s}|t_r\in R\wedge t_s\in S$}</center>

注: R:$(k_1, n)$, S:$(k_2,m)\Longrightarrow$R$\times$S:$(k_1+k_2, n+m)$

#### 专门关系运算

+ 选择(Selection): From the row angle

<center>$\sigma_F(R)$ ={$t|t\in R\wedge F(t)=True$}</center>

+ 投影(Projection): From the column angle

<center>$\pi_A(R)$ = {$t[A]|t\in R$}</center>
注: 选择出原关系中某些属性列，为避免重复，还可能会取消某些元组

+ 连接(Join): From the cross angle

<center>$R\underset{A\theta B}{\bowtie} S$ = {$t_r\cup t_s|t_r\in R\wedge t_s\in S\wedge t_r[A]\theta t_s[B]$}</center>

+ Solution Steps For $\theta$ Join:
  + Step 1: 确定结果中的属性列
  + Step 2: 确定参与比较的属性列
  + Step 3: 逐一取R中的元组分别和S中与其符合条件的元组进行拼接
+ 等值连接(Equi-Join): $\theta$ is "="

<center>$R\underset{A=B}{\bowtie} S$ = {$t_r\cup t_s|t_r\in R\wedge t_s\in S\wedge t_r[A]=t_s[B]$}</center>

+ 自然连接(Natural Join): $\theta$ is "=" and $As = Bs$ which combines As and Bs columns avoiding repeated attributes(As, Bs means a column or multiple columns)

<center>$R\bowtie S$ = {$t_r\cup t_s - t_s[B]|t_r\in R\wedge t_s\in S\wedge t_r[B]=t_s[B]$}</center>

#### Practices

+ Used Tables

  + S Table = S(Sno, Sname, Ssex, Sage, Sdept)
    |  Sno  | Sname | Ssex | Sage | Sdept |
    | :---: | :---: | :--: | :--: | :---: |
    | 95001 | 李勇 |  男  |  20  |  CS  |
    | 95002 | 刘晨 |  女  |  18  |  IS  |
    | 95003 | 王敏 |  女  |  18  |  MA  |
    | 95004 | 张立 |  男  |  19  |  IS  |
  + SC Table = SC(Sno, Cno, Grade)
    |  Sno  | Cno | Grade |
    | :---: | :-: | :---: |
    | 95001 | c1 |  92  |
    | 95001 | c2 |  65  |
    | 95001 | c4 |  88  |
    | 95002 | c2 |  90  |
    | 95002 | c5 |  73  |
+ SC$\times$SC Table

  | SC1.Sno | SC1.Cno | SC1.Grade | SC2.Sno | SC2.Cno | SC2.Grade |
  | :-----: | :-----: | :-------: | :-----: | :-----: | :-------: |
+ Problems

  + 查询选修了 $C_2$ 和 $C_4$ 课程的学生学号

  $$
  pi_1(\sigma_{1=4\wedge 2='c2'\wedge 5='c4'}(SC\times SC))
  $$

  + 查询不学 $C_2$ 课程的学生学号

  $$
  pi_{sno}(S)-\pi_{cno}(\sigma_{cno='c2'}(SC))
  $$

1. 关系模型由**关系数据结构**、**关系操作集合**和**关系完整性约束**组成
2. 关系数据结构：单一的结构类型即关系，表示现实世界的实体以及实体间的联系
3. 关系操作集合：查询、插入、删除、修改操作
4. 关系完整性约束：实体完整性、参照完整性、用户定义完整性约束
5. 关系数据库语言的共同特点：非过程化的集合操作语言
6. 关系数据语言：关系代数语言、关系演算语言、SQL


## 第三讲 数据库完整性

数据库完整性包括实体完整性、参照完整性和用户定义完整性。

### 实体完整性

+ CREATE TABLE 中用 PRIMARY KEY 定义关系模型的实体完整性
+ 单属性构成的码： 定义为列级约束条件/定义为表级约束条件
+ 多个属性构成的码： 定义为表级约束条件

### References

+ [BitHachi&#39;s Blog](https://blog.csdn.net/weixin_43914604/article/details/105359554)

## 第三讲 SQL概述
+ [SQL概述及数据定义——BitHachi&#39;s Blog](https://bithachi.blog.csdn.net/article/details/105217410)
+ [SQL之数据查询——BitHachi&#39;s Blog](https://bithachi.blog.csdn.net/article/details/105171740)
+ [SQL之基本表更新——BitHachi&#39;s Blog](https://bithachi.blog.csdn.net/article/details/105243896)

## 第六讲 关系数据理论之规范化

### 存在的问题

关系模式中*属性间存在某些依赖关系*导致*插入异常、删除异常、更新异常以及数据冗余*的问题

### 数据依赖

定义: 关系属性与属性之间的一种约束关系，即两个列或列组之间的约束，主要包含函数依赖与多值依赖。

#### 函数依赖 (Functional Dependency, FD)

定义: 对于任意关系 $r\in R(U)$, $r$ 中不可能存在两个元组在 $X$ 上的属性值相等，而在 $Y$ 上的属性值不等。

+ $X\rightarrow Y$: $X$ 函数确定 $Y$ 或 $Y$ 函数依赖于 $X$
+ Notes
  + 函数依赖指 $R$ 的所有关系实例均要满足的约束条件
  + 函数依赖属于语义范畴概念，只能根据数据的语义来确定函数依赖
+ 特殊函数依赖
  + 非平凡的函数依赖：$X\rightarrow Y$ 且 $Y\nsubseteq X$
  + 平凡的函数依赖：$X\rightarrow Y$ 且 $Y\subseteq X$
  + 相互决定: $X\rightarrow Y$ 且 $Y\rightarrow X$, denotes $X\leftrightarrow Y$
  + $Y$ 不函数依赖于 $X$: $X \nrightarrow Y$
  + 完全函数依赖：$X\rightarrow Y$ 且 $ \forall X' \subset X, X' \nrightarrow Y$, denotes $X\mathop{\longrightarrow}\limits^F Y$
  + 部分函数依赖：$X\rightarrow Y$ 且 $Y$ 不完全函数依赖于 $X$, denotes $X\mathop{\longrightarrow}\limits^P Y$
  + 传递函数依赖：$X\rightarrow Y, Y\rightarrow Z$ with conditions $Y\nsubseteq X, Y\nrightarrow X$，则 $X\rightarrow Z$, denotes $X\mathop{\longrightarrow}\limits^T Y$
    + 如果 $Y\rightarrow X$ 即 $X\leftrightarrow Y$，则 $Z$ 直接依赖于 $X$
    + 如果 $Y\subseteq X$, 则 $X\mathop{\longrightarrow}\limits^P Z$
+ 候选码(Candidate Key)
  + For $K$ in $R<U, F>$, satisfy $K\mathop{\longrightarrow}\limits^F U$
  + 主码(Primary Key) 为选定的一个候选码
  + 性质
    + 决定性：$K\rightarrow U$
    + 最小性: $\nexists K'\subset K$ let $K'\rightarrow U$
  + 主属性(Prime Attribute): 所有候选码中出现的属性
  + 非主属性(Nonprime Attribute): 不出现在任何候选码中的属性
  + 全码(All Key): 由关系模式的所有属性构成码
  + 外码(Foreign Key): $X$ 并非是 $R$ 的码，而是另外一个关系模式的码

### 规范化

#### 规范化设计

关系表的规范化设计就是要尽可能地减少关系表中列或者列组之间的依赖关系，即函数依赖

#### 范式(Normal Form, NF)

+ **Defination 1**: 表示关系表的规范程度状态
+ **Defination 2**: 表示符合某一种级别的关系模式的集合

#### 第一范式(First Normal Form, 1NF)

+ **Defination**: 关系模式 $R$ 的所有属性都是*不可分的基本数据项*，denotes $R\in 1NF$
+ 不满足第一范式的数据库模式不是关系数据库

#### 第二范式(Second Normal Form, 2NF)

+ **Defination**: $R\in 1NF$ 并且每一个非主属性都完全函数依赖于 $R$ 的任一候选码, denotes $R\in 2NF$
+ Notes
  + 不存在非主属性对码的部分依赖
  + 不属于 $2NF$ 关系模式问题：插入异常、删除异常、数据冗余大、修改异常

#### 第三范式(Third Normal Form, 3NF)

+ **Defination**: $R<U, F>$ 中不存在码 $X$、属性组 $Y$ 及非主属性 $Z(Z\nsubseteq Y)$ 使得 $X\rightarrow Y(Y\nrightarrow X), Y\rightarrow Z$, denotes $R\in 3NF$
+ Notes
  + If $Z\subseteq Y$, then when $X\rightarrow Y$, get $X\rightarrow Z$
  + 不存在非主属性对码的传递依赖
  + 不属于 $3NF$ 关系模式问题：插入异常、删除异常、数据冗余大、修改异常

#### 修正第三范式(Boyce Codd Normal Form, BCNF)

+ **Defination**: $R\in 1NF$, for any $X\rightarrow Y(Y\nsubseteq X)$ and $X$ 必包含码, denotes $R\in BCNF$
+ Notes
  + 每一个函数依赖的决定因素都包含码

## Handwritten Notes

<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page1.51yc1mwj6wc0.png"/>
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page2.5zhwap4c8nw0.webp" />
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page3.6sisp43v1pk0.webp" />
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page4.23jsf44s5qow.webp" />
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page5.41nf6qoealc0.webp" />
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page6.330yrzif8iw0.webp" />
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page7.39y9yuxk8gg0.webp" />
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page8.2wdw8hhua8u0.webp" />
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Courses/数据库原理笔记_Page9.3ehx9lj0jui0.webp" />

## 数据库系统期末复习笔记
<iframe src="/pdfjs/web/viewer.html?file=/pdf/collaboration/DatabaseNotes.pdf" style='width:100%;height:800px'></iframe>

## 数据库系统课程实验报告
<iframe src="/pdfjs/web/viewer.html?file=/pdf/collaboration/DatabaseSystem.pdf" style='width:100%;height:800px'></iframe>

## Contributors

+ [Zhihao Li](https://lzhms.github.io/)
+ [Changrong You](https://cryoushiwo.github.io/)
