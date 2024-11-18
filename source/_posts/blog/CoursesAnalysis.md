---
title: Courses Analysis Tutorial Using MicroSoft Power BI
date: 2024-10-30 18:20:10
toc: true
tags:
    - Project Tools
categories: blog
cover: https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241030185104.png
excerpt: A post about how to visualize courses scores better using the tool of MicroSoft Power BI.
---
## 准备工作
### 数据准备
针对自己本科阶段所有学习科目，应当导出或制作对应每个科目的属性以及成绩，将其汇总程 Excel 表格。具体的属性可以见以下打印的 PDF 文件，Excel 表格无法直接展示，附在随后。

<iframe src="/pdfjs/web/viewer.html?file=/pdf/blog/MyCoursesAnalysis.pdf" style='width:100%;height:800px'></iframe>

+ [MyCourses.xlsx](../../excel/MyCourses.xlsx)

### 西电校友
对于各位西电计科学子，博文也提供了可以直接导出个人所有必修以及限选科目成绩的jupyter notebook（因包含个人信息，完成科目成绩可以私下联系我），详见下文具体步骤。
主要计算每门课程的平均分、最高分、最低分以及排名，可以将其替换为自己的ID，以及置换自己对应的专业限选课，然后计算即可。对于其他排名，诸如学年排名、推免综合排名等部分，这个因人而异，需要自行添加。

#### Jupyter Notebook Codes
+ Load Datasets

```python
import pandas as pd
import numpy as np

BaseCourses = pd.read_excel("Courses.xlsx", sheet_name="6Semester")
print(BaseCourses.head())

MyCourses = pd.read_excel("Courses.xlsx", sheet_name="Courses")
MyCourses.set_index(MyCourses.columns[0], inplace=True)
print(MyCourses.head())

EmbeddedCourses = pd.read_excel("Courses.xlsx", sheet_name="Embedded")
print(EmbeddedCourses.head())
```
+ Calculate Scores

```python
# calculate the average, min, max score
MyCourses['单科排名'] = MyCourses['单科排名'].astype(str)
for col in BaseCourses.columns[3:]:
    MyCourses.loc[col, '平均分'] = round(BaseCourses[col].mean(), 2)
    MyCourses.loc[col, '最低分'] = BaseCourses[col].min()
    MyCourses.loc[col, '最高分'] = BaseCourses[col].max()
    
    ranking = BaseCourses[col].rank(method='min', ascending=False).astype(int)
    rank_rate_percentage = (ranking.iloc[0] / len(BaseCourses))
    MyCourses.loc[col, '单科排名'] = f"{ranking[0]}/{len(BaseCourses)}={rank_rate_percentage* 100:.2f}%"
    MyCourses.loc[col, '百分比'] = round(1 - rank_rate_percentage, 2)

# 专业限选课
EmbeddedColumns = ['嵌入式程序设计', '嵌入式应用综合设计', 'SOC微体系结构设计', '数字信号处理', '自主可控嵌入式系统']
for col in EmbeddedColumns:
    MyCourses.loc[col, '平均分'] = round(EmbeddedCourses[col].mean(), 2)
    MyCourses.loc[col, '最低分'] = EmbeddedCourses[col].min()
    MyCourses.loc[col, '最高分'] = EmbeddedCourses[col].max()
    
    ranking = EmbeddedCourses[col].rank(method='min', ascending=False).astype(int)
    rank_rate_percentage = (ranking.iloc[0] / len(EmbeddedCourses))
    MyCourses.loc[col, '单科排名'] = f"{ranking[0]}/{len(EmbeddedCourses)}={rank_rate_percentage * 100:.2f}%"
    MyCourses.loc[col, '百分比'] = round(1 - rank_rate_percentage, 2)
```
+ Save to Excel

```python
MyCourses.reset_index(drop=False, inplace=True)
print(MyCourses.head())

try:
    with pd.ExcelWriter("MyCourses.xlsx", mode='a') as writer:
        MyCourses.to_excel(writer, sheet_name="MyCourses", index=False)
except Exception as e:
    MyCourses.to_excel("MyCourses.xlsx", sheet_name="MyCourses", index=False)
```

### Power BI Desktop 安装
Microsoft Power BI 官网提供了多种安装方式，但是比较推荐的安装方式还是 Power BI Desktop，详情可见[下载页面](https://www.microsoft.com/zh-cn/download/details.aspx?id=58494)。
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241030195819.png" alt="Power BI Desktop Installation" width="70%"/>

## 数据导入
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-30%20200104.png" alt="导入Excel数据表以及导入属性列" width="70%"/>

## 可视化图块
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-10-30%20201015.png" alt="一个可视化图块的举例" width="70%"/>
Power BI 可以对数据做汇总分析，然后重构成新的统计量，对应每一个可视化图块，都可以对选中的数据自动绘制图表。最终的报表可以由多个可视化图块组成，共同展示相应的统计信息。而且基于同一份数据，不同图块能够相互关联，交互地展示同一份子数据。

具体的模块配置比较简单，对照着示例基本很快可以完成一份报表的绘制，因此具体用法就不在此阐述了。

## 发布到博客网站
重点想要分享的还是怎样把我们做好的报表整个地发布到博客网站上，可以展示给其他人阅览。
### 注册 Power BI 服务账号
企业账号以及部分地区可以免费享有 Power BI 服务，尤其是公布到微软服务器上。但是在国内的话，不会提供相应的服务，具体需要购买相应的方案。
比较简单高效的方法是，获取一个国外或企业账号，进行注册。这里可以通过淘宝渠道购买一个，基本8元左右。
另外有的采用 Azure 账户的方式，但似乎已经失效，具体可以参考知乎文章[最新版-PowerBI账户注册（无需企业邮箱！！！）](https://zhuanlan.zhihu.com/p/337692884)。

### 发布到 Power BI 服务
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241030202419.png" alt="本机发布到微软 Power BI 服务" width="70%"/>
本机上制作好的报表需要先发布到 Power BI，如上图所示。然后需要在 Power BI 服务上对应保存本机报表的工作区中，将报表生成网页嵌入代码，具体操作如下：
<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241030202720.png" alt="生成网页嵌入代码" width="70%"/>

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/learning/20241030202826.png" alt="获取HTML代码" width="70%"/>
最后将生成的HTML代码放置在博客网页中就可以了。

## 效果展示
放置一个做好的报表的网页链接，具体效果可以见下文。

<iframe title="CoursesAnalysis" width="960" height="630" src="https://app.powerbi.com/view?r=eyJrIjoiZDliMWUxM2UtZmI0OC00ZDA1LTg3ZjYtMjcxNTQyNzgzNDI5IiwidCI6IjZmMGJiNzJmLTUzNzctNGRkZi05MzZhLWI2YzcyYmYyMWFlMiIsImMiOjF9&pageName=9574150788369b9e0a53" frameborder="0" allowFullScreen="true"></iframe>

## 资源汇总
+ [MyCourses.xlsx](../../excel/MyCourses.xlsx)
+ [CoursesAnalysis.pbix](../../files/blog/CoursesAnalysis.pbix)

## Reference
<div style="border: 1px solid #ccc; padding: 15px; border-radius: 5px; box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);">
    <p>Chenlai Qian, bachelor in Xidian University and master in Southeast University.
    <p>
        <a href="https://levitate-qian.github.io/about/">
            https://levitate-qian.github.io/about/
        </a>
    </p>
</div>
