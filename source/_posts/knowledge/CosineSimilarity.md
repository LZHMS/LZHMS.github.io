---
title: Mathematic Principles about Cosine Similarity of High-dimensional Vectors
date: 2024-04-22 18:25:19
toc: true
tags:
    - Professional Knowledge
categories: knowledge
excerpt: This is a post about the mathematic principles about cosine similarity of high-dimensional vectors.
---
## Introduction
In machine learning, we often need to measure the distance between the high-dimensional vectors and using the distance to cluster, do re-labeling, or evaluate samples. Although there are many other alternative methods, the cosine similarity is the most used and more efficient.

## Two-dimensional Vectors
When I encountered this theory, it's so fantastic because it is just extended from one conclusion that I derived in the high school. Let's firstly take the original conclusion as an example.
In the following figure, there are three points $A$, $B$, $P$ in the two-dimensional space which form two vectors $\overrightarrow{PA}$, $\overrightarrow{PB}$.
<img src="https://github.com/LZHMS/picx-images-hosting/raw/master/EBlog/Paper/P1.54xk04anla.webp" alt="" width="40%"/>

We know, the vector $\overrightarrow{AB}$ could be represented as $\overrightarrow{PB} - \overrightarrow{PA}$. In the high school, we use this characteristic to calculate the cosine value between the two vectors. That is, 
$$
\cos \theta = \frac{\overrightarrow{PA}^2 + \overrightarrow{PB}^2 - \overrightarrow{AB}^2}{2|\overrightarrow{PA}||\overrightarrow{PB}|}
$$
$$
\overrightarrow{AB}^2 = \overrightarrow{PA}^2 + \overrightarrow{PB}^2 - 2\overrightarrow{PA} \cdot \overrightarrow{PB}
$$
$$
\cos \theta = \frac{\overrightarrow{PA} \cdot \overrightarrow{PB}}{|\overrightarrow{PA}||\overrightarrow{PB}|}
$$
If we denote the $\overrightarrow{PA}=(x_1, y_1)$, $\overrightarrow{PB}=(x_2, y_2)$, then we can get the more concrete formula.
$$
\cos \theta = \frac{x_1x_2 + y_1y_2}{\sqrt{(x_1^2 + y_1^2)}\sqrt{(x_2^2 + y_2^2)}}
$$
## Similarity Analysis
But why is it suitable for evaluating the similarity between two vectors?
This method using the cosine value of $\theta$ to measure the similarity between the two vectors. If the cosine value is close to 1, it means that the two vectors are parallel to each other. If the cosine value is close to 0, it means that the two vectors are perpendicular to each other.If the cosine value is close to -1, it means that the two vectors are opposite to each other.
This method just balance the angle or direction of vectors. The more closer the direction is, the more similar the two vectors are.

Do you put forward another question? If we have two vectors with the same direction and different modulus and we have another two vectors with the same direction and same modulus, the two situations will both get the cosine value of 1. But actually, the latter is more similar in our opinion so this is a little drawback.

## High-dimensional Vectors
Now, let's extend this conclusion to the high-dimensional vectors. The only difference is just the dimensionalities which are very very large but the principles are still consistent.
If $x\in R^n$, $y\in R^n$, then we can get the cosine similarity.
$$
\cos \theta = \frac{x^T\cdot y}{|x||y|}=\frac{\sum_{i=1}^n x_iy_i}{\sqrt{\sum_{i=1}^n x_i^2}\sqrt{\sum_{i=1}^n y_i^2}}
$$
## Conclusion
The cosine similarity is a very useful method to measure the similarity between the high-dimensional vectors. But it has its application limitations. For some situations, it doesn't work well or the principles are not desired.

Just taking my exploration, I want the similarity between samples to be higher when they have more overlapping features. So this situation is not suitable for the cosine similarity. In a nutshell, using the cosine similarity need to consider your application scenarios.
