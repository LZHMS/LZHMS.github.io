---
title: The Basic Principles and Some Further Discussion of GANs
date: 2024-04-02 10:48:20
toc: true
tags:
    - Professional Knowledge
categories: knowledge
excerpt: This is a post about the basic principles and some further discussion of GANs.
---
## Maximum Likelihood Estimation
+ Given a data distribution $P_{data}(x)$
+ We have a distribution $P_G(x;\theta)$ parameterized by $\theta$ 
    + E.g. $P_G(x:\theta)$ is a Gaussian Mixture Model, $\theta$ are means and variances of the Gaussians
    + We want to find $\theta$ such that $P_G(x;\theta)$ close to $P_{data}(x)$
+ Sample $\{x^1, x^2,...,x^m\}$ from $P_{data}(x)$
+ We can compute $P_G(x^i;\theta)$
+ Likelihood of generating the samples
$$
L = \prod_{i=1}^m P_G(x^i;\theta)\tag{1}
$$

So we can just solve the optimal $\theta$:
$$
\theta^* = arg \max_{\theta}\prod_{i=1}^m P_G(x^i;\theta)\tag{2}
$$
$$
= arg \max_{\theta} \log\prod_{i=1}^m P_G(x^i;\theta)\tag{3}
$$
$$
=arg \max_{\theta}\sum_{i=1}^m \log P_G(x^i;\theta)\tag{4}
$$
$$
\approx \max_{\theta} E_{x\sim P_{data}}[\log P_G(x;\theta)]\tag{5}
$$
$$
=arg \max_{\theta}\int_x P_{data}(x)\log P_G(x;\theta)dx - \int_x P_{data}(x)\log P_{data}(x)dx\tag{6}
$$
$$
=arg \min_{\theta} KL(P_{data}(x)||P_G(x;\theta))\tag{7}
$$
+ In the equation (2), $\theta$ means the parameters of G(Generate) model and $x^i$ means the $i$-th sample from $P_{data}(x)$. So this equation expresses the probability of $P_G$ generating the samples from $P_{data}$.
+ In the equation (3) and (4), we use the logarithm to make the equation easier to calculate and it doesn't influence the optimal $\theta$.
+ Equation (5) is the approximate of summation results and it only has the difference with $\frac{1}{m}$.
+ Int te euqation (6), there is a extra term $\int_x P_{data}(x)\log P_{data}(x)dx$ which is the constant for $G$ network so it also doesn't influence the optimal $\theta$.

## Basic Idea of GAN
But the Generator $G$ is hard to be learned by maximum likelihood.
### Min-max GAN
Define a value function $V(G, D)$:
$$
G^* = arg \min_G \max_D V(G, D)\tag{8}
$$ 
+ Given G, the optimal $D^*$ maximizing:
$$
V = E_{x\sim P_{data}}[\log D(x)] + E_{x\sim P_G(x)}[\log (1-D(x))]\tag{9}
$$
$$
=\int_xP_{data}(x)\log D(x)dx + \int_xP_G(x)\log (1-D(x))dx\tag{10}
$$
$$
=\int_x[P_{data}(x)\log D(x) + P_G(x)\log (1-D(x))]dx\tag{11}
$$

+ Given $D$, the optimal $G^*$ minimizing:
$$
P_{data}(x)\log D(x) + P_G(x)\log (1-D(x))\tag{12}
$$

### Optimal $D^*$
If we maximum each $P_{data}(x)\log D(x) + P_G(x)\log (1-D(x))$ for any input $x$, there will be the optimal $D$.
+ Given $x$, the optimal $D^*$ maximizing:
$$
P_{data}(x)\log D(x) + P_G(x)\log (1-D(x))\tag{13}
$$
There we can view $P_{data}(x)$ as a constant value of $a$, and $P_G(x)$ as a constant value of $b$.
$D^*$ maximizing: $f(D) = a\log D + b\log (1-D)$. Solving it, $D^* = \frac{1}{a+b}$, that is
$$
D^* = \frac{P_{data}(x)}{P_{data}(x)+P_G(x)}\tag{14}
$$
+ Conduct Result
$$
V(G, D^*) = E_{x\sim P_{data}}[\log \frac{P_{data}(x)}{P_{data}(x)+P_G(x)}] + 
E_{x\sim P_G(x)}[\log \frac{P_{G}(x)}{P_{data}(x)+P_G(x)}]\tag{15}
$$
$$
=\int_x P_{data}(x)\log \frac{P_{data}(x)}{P_{data}(x)+P_G(x)}dx + \int_x P_G(x)\log \frac{P_{G}(x)}{P_{data}(x)+P_G(x)}dx\tag{16}
$$
$$
=\int_x P_{data}(x)\log \frac{\frac{P_{data}(x)}{2}}{\frac{P_{data}(x)+P_G(x)}{2}}dx + \int_x P_G(x)\log \frac{\frac{P_{G}(x)}{2}}{\frac{P_{data}(x)+P_G(x)}{2}}dx\tag{17}
$$
$$
=-2\log2+\int_x P_{data}(x)\log \frac{P_{data}(x)}{\frac{P_{data}(x)+P_G(x)}{2}}dx + \int_x P_G(x)\log \frac{P_{G}(x)}{\frac{P_{data}(x)+P_G(x)}{2}}dx\tag{18}
$$
$$
=-2\log2+KL(P_{data}(x)||\frac{P_{data}(x)+P_G(x)}{2}) + KL(P_G(x)||\frac{P_{data}(x)+P_G(x)}{2})\tag{19}
$$
$$
=-2\log2+2JSD(P_{data}(x)||P_G(x))\tag{20}
$$

### Optimal $G^*$
$$
G^* = arg \min_G \max_D V(G, D)
$$
When got the optimal $D^*$,
$$
\max_D V(G, D) = -2\log 2+2JSD(P_{data}(x)||P_G(x))\tag{21}
$$
So to minimize above equation, we just get the optimal G:
$$
P_G(x) = P_{data}(x)\tag{22}
$$

## Further Discussion
The adversarial idea is very innovative which seems there two bodies, one for making decisions and the other for checking the quality, will enhance each other alternately.

In some robust research domain, this idea will provide a new way to improve, like learning with noisy labels. 

## Reference
+ [Generative Adversarial Network](https://www.youtube.com/watch?v=0CKeqXl5IY0&t=5829s)