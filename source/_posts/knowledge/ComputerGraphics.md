---
title: 计算机图形学入门
date: 2024-10-13 16:35:44
toc: true
tags:
    - Computer Graphics
categories: knowledge
cover: https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241013164933.png
excerpt: This is a post about how to go into computer graphics.
---
## 基本概念
计算机图像学研究真实或者虚拟世界的**图形表示以及人机交互**，是涉及计算机、数学、物理等学科的交叉领域。
+ 核心内容：**建模**、**绘制**和**交互**。
+ 建模：在计算机中通过几何、图像、视频等形式构建和表示物体模型；
+ 绘制：通过可视媒介（图像、视频、全息等）呈现计算机中存储的物理模型；
+ 交互：通过计算机输入、输出设备，以有效的方式对计算机中的模型进行操作；

计算机图像学与计算机视觉/图像处理的关系
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241013164933.png" alt="计算机图像学与计算机视觉" width="70%"/>

## 发展历史
+ 上世纪50~60年代（1950-1969），阴极射线管（CRT）的出现促使了计算机图像学的兴起。阴极射线管，是一种用于显示系统的物理仪器，它是利用阴极电子枪发射电子，在阳极高压的作用下，射向荧光屏，使荧光粉发光，同时电子束在偏转磁场的作用下，作上下左右的移动来达到扫描的目的。
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241013165303.png" alt="阴极射线管中，电子束在磁场的作用下偏转" width="70%"/>

+ 1950年，第一个绘制出来的图形是由美国测绘员Ben Laposky通过操控模拟电子束在显示屏上呈现，这是最早的图形绘制设备，第一个通过阴极管绘制在计算机中的图形。
+ 1959年，美国通用和IBM公司发明了第一台工业CAD系统，帮助工程师进行车辆的设计，从此CAD逐渐发展起来。
+ 1960年，计算机图形学（Computer Graphics）是波音公司的工程师Fetter最早提出的。该概念主要用来描述飞机驾驶室模拟设计。
+ 1963年，MIT博士生的Ivan Sutherland （后获图灵奖，Coons奖（图形学领域终身成就奖））设计了Sketchpad，被公认为**计算机图形学的起源**。Sketchpad，赋予计算机图形处理的能力，使计算机拥有人类右脑的功能。
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241013165930.png" alt="Ivan Sutherland以及设计的Sketchpad" width="70%"/>
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241013170211.png" alt="人类左脑符号处理以及右脑图像处理的能力" width="70%"/>

## 研究问题
### 自然的三维交互技术
实现更灵活的三维交互输入：从传统鼠标、单点触摸、多点触摸的二维接触式交互方式转变为基于手势、姿态等的三维非接触式交互方式。
### 直观的三维编辑技术
实现艺术家模式的三维模型制作与编辑：通过画笔手绘等常见且直观的艺术创作手法，对三维模型进行建模或修改。
### 富有感情的机器人技术
实现具有人类相似情感表现的机器人：用人工的方法和技术赋予计算机或机器人以人类式的情感，使之在工作时具有表达、识别和理解喜乐哀怒，模仿、延伸和扩展人的情感的能力。
### 绘画辅助技术
学习和控制绘制过程以及理解绘制物的语义：通过直线、曲线、笔画、素描等不同形式的绘画工具，让计算机更加智能地辅助进行人工参与的绘画创作。
### 数学辅助技术
实现更便捷的数学符号和公式的输入、输出：通过图形方式辅助论文编辑、数学软件、数学手写板、数学推理等。
### 海量数据的可视化技术
海量数据的视觉感知：借助面向海量数据的可视化技术，从根本上改变人们表示、分析和理解海量复杂数据的方式。
### 虚拟导游技术
将虚拟角色融入吃、穿、住、用、行的虚拟现实场景：利用计算机提供的智能导游服务取代人力服务，将能大大提高服务质量。
### 高沉浸感技术
真实感的虚拟环境沉浸式体验和交互：以虚拟洞穴、虚拟办公室、VR眼镜等为代表的虚拟现实技术，需要使用户专注在当前的目标情境下感到愉悦和满足，而忘记真实世界的情境。
### 增强现实技术
叠加虚拟物体到真实世界：在屏幕上把虚拟世界套在现实世界并进行互动，使用户就不必在其他设备上查看相关信息，以便腾出双手进行其他任务。
### 混合现实技术
形成与真实世界无缝融合的高品质的完全沉浸式虚实融合的环境，通过交互构建现实世界、虚拟世界和用户之间的信息回路。

## 基础知识
### 图形与图像
+ 图形：由点、线、面等基本几何元素作为“图元”构成，通过建模、测量等方式获取；
+ 图像：由像素构成，通过照相、扫描等方式获取；

二维和三维图形需要通过光栅化转化为图像进行屏幕显示，光栅化是图形学中一个重要的概念，它将几何图形转化为像素点阵的过程。
+ 直线、圆：Bresenham算法
+ 面：扫描线算法（scan-line rasterization）

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241018094918.png" alt="图形流水线" width="70%"/>

### 计算机图形学系统
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241018152212.png" alt="计算机图形学系统" width="70%"/>

## References

<div style="border: 1px solid #ccc; padding: 15px; border-radius: 5px; box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);">
    <p>罗楠. 2024秋, 计算机图形学, 西安电子科技大学.</p>
    <p>
        <a href="https://mooc1.chaoxing.com/mooc-ans/course/245352860.html">
            https://mooc1.chaoxing.com/mooc-ans/course/245352860.html
        </a>
    </p>
</div>