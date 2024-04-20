---
title: 一个深入研究的实验分析示例——启示与探索
date: 2024-04-20 17:02:13
toc: true
tags:
    - Project Habits
categories: blog
excerpt: 深入研究
---
## 引言
通过从事前三个月的研究工作，我逐渐发现，在研究的过程中，最最重要的是，需要非常清楚每一个步骤/环节中结果如何、表现怎样，有没有按照我们的预期进行。这是我们回溯实验结果、分析数据特点、模型特性以及方法有效性的衡量标准，最终得出一条我个人目前的研究路线：发现存在的问题、分析内在的特征然后提出有效的解决方案才是高效的研究路线。

## 实验探究一——探究同一类别样本的互相似度分布差异
### 实验设置
+ 计算单一样本相似度向量的平均值、最值、方差等分布参数；
+ 观察 TP 样本与 FP 样本，TP 样本与 TN 样本之间差异情况；

### 实验现象
#### 样本分布特性
```python
Testing the similarities distribution of CLS_SIM
index: 0  nlabel: 0  tlabel: 0  min_sim: 0.4758  mean_sim: 0.7277  std_sim: 0.1334  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0074.jpg
[1.         0.81201172 0.75390625 0.8671875  0.75244141 0.87597656
 0.56884766 0.70263672 0.75244141 0.4831543  0.47583008 0.68701172
 0.66796875 0.70703125 0.73535156 0.80126953]
index: 1  nlabel: 0  tlabel: 0  min_sim: 0.5483  mean_sim: 0.7607  std_sim: 0.1154  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0078.jpg
[0.81201172 1.         0.70654297 0.87353516 0.75195312 0.83007812
 0.61865234 0.70410156 0.75732422 0.55224609 0.54833984 0.80908203
 0.80322266 0.73583984 0.78271484 0.88476562]
index: 2  nlabel: 0  tlabel: 0  min_sim: 0.5356  mean_sim: 0.7162  std_sim: 0.1057  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0138.jpg
[0.75390625 0.70654297 1.         0.72607422 0.59667969 0.71728516
 0.58007812 0.79736328 0.80371094 0.53564453 0.65869141 0.68457031
 0.6484375  0.75097656 0.79785156 0.70117188]
index: 3  nlabel: 0  tlabel: 0  min_sim: 0.5234  mean_sim: 0.7680  std_sim: 0.1165  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0059.jpg
[0.8671875  0.87353516 0.72607422 1.         0.80126953 0.90673828
 0.64306641 0.72070312 0.74365234 0.60400391 0.5234375  0.72851562
 0.72900391 0.77539062 0.76806641 0.87695312]
index: 4  nlabel: 0  tlabel: 0  min_sim: 0.4858  mean_sim: 0.6780  std_sim: 0.1206  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0041.jpg
[0.75244141 0.75195312 0.59667969 0.80126953 1.         0.73876953
 0.54443359 0.64111328 0.671875   0.50927734 0.48583984 0.64453125
 0.63574219 0.64941406 0.68261719 0.7421875 ]
index: 5  nlabel: 0  tlabel: 0  min_sim: 0.4875  mean_sim: 0.7469  std_sim: 0.1298  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0055.jpg
[0.87597656 0.83007812 0.71728516 0.90673828 0.73876953 1.
 0.57470703 0.77099609 0.74658203 0.53076172 0.48754883 0.72998047
 0.70361328 0.75097656 0.74560547 0.84130859]
index: 6  nlabel: 0  tlabel: 39  min_sim: 0.4878  mean_sim: 0.6113  std_sim: 0.1139  ip: /home/zhli/projects/PTNL/data/dtd/images/striped/striped_0122.jpg
[0.56884766 0.61865234 0.58007812 0.64306641 0.54443359 0.57470703
 1.         0.55371094 0.61279297 0.63964844 0.48779297 0.52099609
 0.50976562 0.67724609 0.671875   0.57763672]
index: 7  nlabel: 0  tlabel: 0  min_sim: 0.5537  mean_sim: 0.7301  std_sim: 0.1030  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0113.jpg
[0.70263672 0.70410156 0.79736328 0.72070312 0.64111328 0.77099609
 0.55371094 1.         0.78710938 0.58007812 0.64208984 0.73779297
 0.68359375 0.83789062 0.78076172 0.7421875 ]
index: 8  nlabel: 0  tlabel: 0  min_sim: 0.5571  mean_sim: 0.7348  std_sim: 0.1003  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0141.jpg
[0.75244141 0.75732422 0.80371094 0.74365234 0.671875   0.74658203
 0.61279297 0.78710938 1.         0.55712891 0.65283203 0.67626953
 0.64257812 0.75732422 0.84619141 0.74902344]
index: 9  nlabel: 0  tlabel: 7  min_sim: 0.4702  mean_sim: 0.5970  std_sim: 0.1245  ip: /home/zhli/projects/PTNL/data/dtd/images/cracked/cracked_0102.jpg
[0.4831543  0.55224609 0.53564453 0.60400391 0.50927734 0.53076172
 0.63964844 0.58007812 0.55712891 1.         0.57177734 0.52539062
 0.47021484 0.73925781 0.68701172 0.56689453]
index: 10  nlabel: 0  tlabel: 0  min_sim: 0.4758  mean_sim: 0.5924  std_sim: 0.1239  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0117.jpg
[0.47583008 0.54833984 0.65869141 0.5234375  0.48583984 0.48754883
 0.48779297 0.64208984 0.65283203 0.57177734 1.         0.54980469
 0.56298828 0.65332031 0.65429688 0.5234375 ]
index: 11  nlabel: 0  tlabel: 0  min_sim: 0.5210  mean_sim: 0.7170  std_sim: 0.1238  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0081.jpg
[0.68701172 0.80908203 0.68457031 0.72851562 0.64453125 0.72998047
 0.52099609 0.73779297 0.67626953 0.52539062 0.54980469 1.
 0.88818359 0.73046875 0.72167969 0.83740234]
index: 12  nlabel: 0  tlabel: 0  min_sim: 0.4702  mean_sim: 0.7017  std_sim: 0.1324  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0036.jpg
[0.66796875 0.80322266 0.6484375  0.72900391 0.63574219 0.70361328
 0.50976562 0.68359375 0.64257812 0.47021484 0.56298828 0.88818359
 1.         0.72460938 0.70263672 0.85400391]
index: 13  nlabel: 0  tlabel: 0  min_sim: 0.6494  mean_sim: 0.7557  std_sim: 0.0811  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0123.jpg
[0.70703125 0.73583984 0.75097656 0.77539062 0.64941406 0.75097656
 0.67724609 0.83789062 0.75732422 0.73925781 0.65332031 0.73046875
 0.72460938 1.         0.82958984 0.77197266]
index: 14  nlabel: 0  tlabel: 0  min_sim: 0.6543  mean_sim: 0.7605  std_sim: 0.0822  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0133.jpg
[0.73535156 0.78271484 0.79785156 0.76806641 0.68261719 0.74560547
 0.671875   0.78076172 0.84619141 0.68701172 0.65429688 0.72167969
 0.70263672 0.82958984 1.         0.76171875]
index: 15  nlabel: 0  tlabel: 0  min_sim: 0.5234  mean_sim: 0.7645  std_sim: 0.1226  ip: /home/zhli/projects/PTNL/data/dtd/images/banded/banded_0077.jpg
[0.80126953 0.88476562 0.70117188 0.87695312 0.7421875  0.84130859
 0.57763672 0.7421875  0.74902344 0.56689453 0.5234375  0.83740234
 0.85400391 0.77197266 0.76171875 1.        ]
```
这是我对混杂噪声的 banded 少样本数据集的特点分析，目的是寻找一种更加高效的方法筛选出该类别中干净的样本和噪声的样本，于是我对提出的方法其作用的有效性进行探究，挖掘了该类别的互相似度特征分布，得出以下的分析：
+ 干净样本与噪声样本对比：从各个样本的相似度分布可以看出，干净样本大都具有较高的平均相似度值，噪声样本 index=6, 9 的平均相似度较低；
+ FP 样本：其中 index=10 的干净样本与 index=9 的噪声样本具有相近的平均相似度，这将导致 FP 样本的出现；
+ 相似度向量：干净样本的各相似度向量均普遍较高，但仍存在较低的相似度值；噪声样本的各相似度值均普遍较低；

#### 样本对比
+ TP Sample of _banded_ Class: index 5
<img src="https://github.com/LZHMS/picx-images-hosting/raw/master/EBlog/Paper/image.39kz4idz45.webp" alt="image" />

+ FP Sample of _banded_ Class: index 10
<img src="https://github.com/LZHMS/picx-images-hosting/raw/master/EBlog/Paper/image.1756ggcov4.webp" alt="image" />

+ TN Sample of _striped_ Class: index 6
<img src="https://github.com/LZHMS/picx-images-hosting/raw/master/EBlog/Paper/image.8dwnts6h0j.webp" alt="image" />

+ TN Sample of _Cracked_ Class: index 8
<img src="https://github.com/LZHMS/picx-images-hosting/raw/master/EBlog/Paper/image.3ye8oiwcyn.webp" alt="image" />

当我们的模型推理特别奇怪的时候，一定需要深入的观察到底推理的有多奇怪。
+ 对比 TP 样本和 FP 样本，我发现确实大部分 TP 样本和该 FP 样本，是有一部分差异的：直观地观察，TP 样本更加平整，没有类似这种衣服的折皱以及其他的边角，这可能是由于 Visual Encoder 结构提出特征的能力特别强大，细微的特征也进一步地被反映了出来；
+ 对比 TP 样本和 TN 样本，只有 index=6 的样本有些相似，因此具有较高的相似度值，另外的 TN 样本倒是区别挺大，也是合理的。
因此进一步分析观察，我们的模型其实推理也并无特别奇怪，似乎也都在合理之中。

### 实验结论
这一步骤特别重要，我们深入地探究推理的细节、数据特征分布，就是为了明细每一步地性能，以及该如何在哪个环节或哪些结果上做改进，从而进一步提升模型性能。
从上面的实验结果分析，其实也是基于计算出的样本数据特征：
+ 仅仅依靠样本平均相似度是不足以支撑筛选噪声标签的（因为我们之前的方法是依据平均平均相似度的）；
+ 可以选定 Top-K 个较优相似度进一步增强噪声样本与干净样本的特征距离，这是因为我们发现干净样本的高相似度值居多，相反，噪声样本的低相似度值居多；

## 总结
当然，这只是一个简单的分析，目的是想跟大家分享一个心得，从发现问题到分析问题这一步骤，该从哪里着手研究。我的最大的感触是一定要深入研究过程，发现问题所在，分析推理结果，才能针对问题提出更有效的 Idea。
