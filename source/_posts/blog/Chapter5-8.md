---
title: Chapter 5 to 8
date: 2023-09-04 21:09:32
toc: true
tags:
	- Machine Learning
	- Computer Vision
categories: blog
excerpt: This is an article about Deep Learning Practice from Chapter 5 to 8.
---
### Chapter 5 Logistic Regression

Formal definitions: for some data samples $\{<\boldsymbol{x}^{(n)}, y^{(n)}>\}_{n=1}^N$, we can get a predictive model by statistical analysis. The model we have trained will output a preferable prediction value:

$$
y=f(\boldsymbol{x})
$$

for a given test data $\boldsymbol{x}=\{x_1, x_2, \cdots, x_n\}$.

#### Linear Regression

Define: $f(\cdot)$ is regression model in linear combinations

$$
y = f(\boldsymbol{x})=\boldsymbol{w}_T\boldsymbol{x}+b\\
=w_1x_1+\cdots+w_nx_n+b
$$

Optimization goal：

$$
\min\limits_{w,\ b} \ |f(\boldsymbol{x}^{(n)})-y^{(n)}|
$$

Loss Function( Mean squared error)：

$$
E = \sum\limits_n[y^{(n)}-(\boldsymbol{w}^T\boldsymbol{x}^{(n)}+b)]^2
$$

Assumption of the selection error function: The data sample points satisfy the normal distribution with the predicted values of the selected linear regression model as the mean, which is converted to a parametric estimation problem.
Define the conditional probability, which represents the probability value of the model prediction output as $y$ given the independent variables:

$$
p(y|\boldsymbol{x})\thicksim N(\boldsymbol{w}^T\boldsymbol{x}+b, \sigma^2)
$$

Derive the likelihood function of the conditional probability:

$$
p(y|\boldsymbol{x}) :\ \ \boldsymbol{L}(\boldsymbol{w}, b)=\prod\limits_n\frac{1}{\sqrt{2\pi}\sigma}\exp(-\frac{1}{2\sigma^2}(y^{(n)}-\boldsymbol{w}^T\boldsymbol{x}^{(n)}-b)^2)
$$

$$
\Rightarrow \ln\boldsymbol{L}(\boldsymbol{w}, b)=n\ln(\frac{1}{\sqrt{2\pi}\sigma})+\ln(\prod\limits_n\exp(-\frac{1}{2\sigma^2}(y^{(n)}-\boldsymbol{w}^T\boldsymbol{x}^{(n)}-b)^2)
$$

$$
=n\ln(\frac{1}{\sqrt{2\pi}\sigma})-\frac{1}{2\sigma^2}\sum\limits_n(y^{(n)}-\boldsymbol{w}^T\boldsymbol{x}^{(n)}-b)^2
$$

$$
\frac{\partial{\ln\boldsymbol{L}(\boldsymbol{w}, b)}}{\partial{\boldsymbol{w}}}=0, \ \ \frac{\partial{\ln\boldsymbol{L}(\boldsymbol{w}, b)}}{\partial{b}}=0
$$

$$
\Rightarrow \boldsymbol{w}, b=\underset{\boldsymbol{w},\ b}{\arg \max}\ \ \boldsymbol{L}(\boldsymbol{w}, b)
$$

$$
=\underset{\boldsymbol{w},\ b}{\arg \min}\ \ \sum\limits_n(y^{(n)}-\boldsymbol{w}^T\boldsymbol{x}^{(n)}-b)^2
$$

If matrix operations are used, the error function is represented in vector form:

$$
\boldsymbol{Y}=\left[y^{(1)},y^{(2)},\cdots,y^{(n)}\right]',\ \boldsymbol{w}=(w_1, w_2, \cdots, w_n)
$$

$$
\boldsymbol{X}=\left[\boldsymbol{x}_1, \boldsymbol{x}_2,\cdots, \boldsymbol{x}_m\right],\ \boldsymbol{x_i}=(x_i^{(1)}, x_i^{(2)}, \cdots, x_i^{(n)})'
$$

$$
\boldsymbol{b}=\left[b_1, b_2, \cdots, b_n\right]', \ \ b_1=b_2=\cdots=b_n
$$

$$
\Longrightarrow \boldsymbol{E}=(\boldsymbol{Y}-\boldsymbol{X}\boldsymbol{w}^T-\boldsymbol{b})^T(\boldsymbol{Y}-\boldsymbol{X}\boldsymbol{w}^T-\boldsymbol{b})
$$

if we combine data samples weights $w$ with parameters $b$, that is:

$$
\boldsymbol{w}=(w_1, w_2, \cdots, w_n, b)
$$

$$
\boldsymbol{X}=\left[\boldsymbol{x}_1, \boldsymbol{x}_2,\cdots, \boldsymbol{x}_m,\boldsymbol{1}\right],\ \boldsymbol{x_i}=(x_i^{(1)}, x_i^{(2)}, \cdots, x_i^{(n)})',\ \boldsymbol{1} = [1,1,\cdots,1]'
$$

$$
\Longrightarrow \boldsymbol{E}=(\boldsymbol{Y}-\boldsymbol{X}\boldsymbol{w}^T)^T(\boldsymbol{Y}-\boldsymbol{X}\boldsymbol{w}^T)
$$

$$
\Longrightarrow\frac{\partial{E}}{\partial{\boldsymbol{w}}}=2\boldsymbol{X}^T(\boldsymbol{X}\boldsymbol{w}^T-\boldsymbol{Y})=0
$$

$$
\Longrightarrow\boldsymbol{w}^T=(\boldsymbol{X}^T\boldsymbol{X})^{-1}\boldsymbol{X}^T\boldsymbol{Y}
$$

$$
\Longrightarrow y=\boldsymbol{x}\boldsymbol{w}^T=\boldsymbol{x}(\boldsymbol{X}^T\boldsymbol{X})^{-1}\boldsymbol{X}^T\boldsymbol{Y}
$$

#### Logistic Regression

If there exists a nonlinear relationship between random variables ${x_i}(i=1,2,\cdots,n)$ and $y$, a generalized linear regression model can be defined by a nonlinear transformation $g(\cdot)$:

$$
y\mathop{\longrightarrow}\limits^{g(\cdot)} g(y);\ \boldsymbol{x}\mathop{\longrightarrow}\limits^{f(\cdot)} g(y)
$$

$$
f(\boldsymbol{x})=g(y)\Rightarrow y=g^{-1}(f(\boldsymbol{x}))
$$

【Example of binary classification】
In binary classification tasks, the goal is to fit a separating hyperplane.

$$
y=g^{-1}(f(\boldsymbol{x}))=0,\ f(\boldsymbol{x})<0\ \ |\ \ 
1,\ f(\boldsymbol{x})>0.
$$

To ensure differentiability of $g(\cdot)$, a function $\sigma(\cdot)$ is chosen to approximate the discontinuous step function, such as the logistic function:

$$
y=\sigma(f(\boldsymbol{x}))=\frac{1}{1+\exp(-\boldsymbol{w}^T\boldsymbol{x}-b)}
$$

$$
p(y=1|\boldsymbol{x})=\sigma(f(\boldsymbol{x}))
$$

$$
p(y=0|\boldsymbol{x})=1-\sigma(f(\boldsymbol{x}))
$$

$$
g^{-1}(x)=\sigma(x)\Rightarrow \sigma(g(y))=y=\frac{1}{1+e^{-f(\boldsymbol{x})}}=\frac{1}{1+e^{-g(y)}}
$$

$$
\Longrightarrow g(y)=\log\frac{y}{1-y}=\log\frac{p(y=1|\boldsymbol{x})}{p(y=0|\boldsymbol{x})}=f(\boldsymbol{x})=\boldsymbol{w}^T\boldsymbol{x}+b
$$

Parameter estimation: Maximum likelihood estimation is used to estimate the conditional probability $p(y|\boldsymbol{x};\boldsymbol{w},b)$.
Likelihood Function：

$$
\boldsymbol{L}(\boldsymbol{w}, b)=\prod\limits_n[\sigma(f(\boldsymbol{x}^{(n)}))]^{y^{(n)}}[1-\sigma(f(\boldsymbol{x}^{(n)}))]^{1-y^{(n)}}
$$

$$
\Longrightarrow \ln\boldsymbol{L}(\boldsymbol{w}, b)=\ln(\prod\limits_n[\sigma(f(\boldsymbol{x}^{(n)}))]^{y^{(n)}}[1-\sigma(f(\boldsymbol{x}^{(n)}))]^{1-y^{(n)}})
$$

$$
=\sum\limits_n[y^{(n)}\log(\sigma(f(\boldsymbol{x}^{(n)})))+(1-y^{(n)})\log(1-\sigma(f(\boldsymbol{x}^{(n)})))]
$$

#### Pytorch Logistic Regression

This is a demo for Logistic Regression Learning. Firstly, we need to prepare corresponding environment by importing some libraries.

```python
import numpy as np
from torch.distributions import MultivariateNormal
import torch
import torch.nn as nn
import matplotlib.pyplot as plt
```

##### 【Prepare Data】

We set Normal Distribution's mean vector and covariance matrix.

```python
mu1 = torch.ones(2) * -3   # two variables
mu2 = torch.ones(2) * 3
sigma1 = torch.eye(2) * 0.5
sigma2 = torch.eye(2) * 2

# set Normal Distributions Object
m1 = MultivariateNormal(mu1, sigma1)
m2 = MultivariateNormal(mu2, sigma2)
# sampling
x1 = m1.sample((100,))
x2 = m2.sample((100,))

# set label value
y = torch.zeros((200, 1))
y[100:] = 1

# combine and scramble samples
x = torch.cat([x1, x2], dim=0)
idx = np.random.permutation(len(x))
x = x[idx]
y = y[idx]

# visualization
plt.scatter(x1.numpy()[:,0], x1.numpy()[:,1])
plt.scatter(x2.numpy()[:,0], x2.numpy()[:,1])
plt.show()
```

#### 【Prepare Pytorch Linear Model】

In the class `torch.nn`, `Linear` object achieves $y=x\boldsymbol{x}^T+b$.

```python
# set features number
D_in, D_out = 2, 1
linear = nn.Linear(D_in, D_out, bias=True)
output = linear(x)

print(x.shape, linear.weight.shape, linear.bias.shape, output.shape)
```

##### 【Pytorch Activation Function】

Logistic Regression used for binary classification problem will ultilize `torch.nn.Sigmoid()` fanction to map the result which linear model have calculated to $0\thicksim 1$.

```python
sigmoid = nn.Sigmoid()
scores = sigmoid(output)
```

##### 【Loss Function】

Logistic Regression uses cross-entropy as its loss function. `Torch.nn` provides many standard loss function and we can directly use `torch.nn.BCELoss` to calculate binary cross-entropy loss.

```python
loss = nn.BCELoss()
print(loss(sigmoid(output), y))
```

##### 【Reconstruct our model】

In Pytorch, we can inherit `nn.Module` to build our own model, but what we need to notice is that `forward()` method must be overwritten by subclasses.

```python
class LogisticRegression(nn.Module):
	def __init__(self, D_in):
		super(LogisticRegression, self).__init__()
		self.linear = nn.Linear(D_in, 1)
		self.sigmoid = nn.Sigmoid()
	def format(self, x):
		x = self.linear(x)
		output = self.sigmoid(x)
		return output

lr_model = LogisticRegression(2)
loss = nn.BCELoss()
loss(lr_model(x), y)
```

##### 【Optimization Algorithm】

Logistic regression typically optimizes the objective function using gradient descent. Pytorch's `torch.optim` package implements most commonly used optimization algorithms.

```python
from torch import optim

optimizer = optim.SGD(lr_model.parameters(), lr=0.03)
```

After constructing optimizer, we can train the model iteratively. There are two main steps here, one is calling the `backward()` method of the loss function to calculate the model gradient, the other is calling the `step()` method of the optimizer to update the model parameters. It should be noted that we need to call the `zero_grad()` method of the optimizer to clear out parameters' gradient firstly.

```python
batch_size = 10
iters = 10
for _ in range(iters):
	for i in range(int(lex(x)/batch_size)):
		input = x[i*batch_size:(i+1)*batch_size]
		target = y[i*batch_size:(i+1)*batch_size]
		optimizer.zero_grad()
		output = lr_model(input)
		l = loss(output, target)
		l.backward()
		optimizer.step()
```

### Chapter 6 Neural Network Basics

#### I. Basic characteristics of neural networks

+ Connectionist model based on statistics
+ Have the basic unit for processing signals
+ Processing units are connected in parallel with each other
+ There are the connection weights between processing units

##### 【Neuron】

$$
out=f(w_1x_1+w_2x_2+\cdots+w_nx_n+b)
$$

A neuron receives a set of tensors as input: $\boldsymbol{x}=\{x_1,x_2,\cdots,x_n\}^T$, connection weights $\boldsymbol{w}=\{w_1,w_2,\cdots,w_n\}$, then performs a weighted summation：

$$
sum=\sum_iw_ix_i=\boldsymbol{w}\boldsymbol{x}
$$

Sometimes, the weighted summation of neurons is also accompanied by a constant term $b$ as a bias:

$$
sum=\boldsymbol{w}\boldsymbol{x}+b
$$

Activation Function $f(\cdot)$ is applied to the input weighted $sum$ to produce the output of the neuron; if the order of $sum$ is greater than 1, then $f(\cdot)$ is applied to each element of $sum$ .

##### 【Activation Function】

+ $softmax()\ \ function$
  It is suitable for multivariate classification problems, and the function is to normalize n scalars representing n classes respectively to obtain the probability distribution of these n classes.

$$
softmax(x_i)=\frac{\exp(x_i)}{\sum_j\exp(x_j)}
$$

+ $sigmoid()\ \ function$
  It is also usually expressed as $logitic()\ \ function$, suitable for binary classification problems, and is a binary version of $softmax$.

$$
\sigma(x) = \frac{1}{1+\exp(-x)}
$$

that is, for two classes: $x_1, x_2$, there is the equation:

$$
softmax(x_1)=\frac{\exp(x_1)}{\exp(x_1)+\exp(x_2)}
=\frac{1}{1+\exp(x_2-x_1)}
$$

$$
P(x_1=1)=sigmoid(x_1)=\frac{1}{1+\exp(-x_1)}(x_2=0)
$$

+ $Tanh()\ \ function$
  It is a variant of $logistic()\ \ function$.

$$
tanh(x) = \frac{2\sigma(x)-1}{2\sigma^2(x)-2\sigma(x)+1}
$$

+ $ReLU()\ \ function(Rectified\ \ linear\ \ unit)$
  This function only has the half of the range which is active and it can effectively avoid the problem of vanishing gradient.

$$
ReLU(x) = \max(0,x)
$$

##### 【Output Layer】

The output of the activation function is just the output of the neuron. A neuron can have multiple outputs $o_1, o_2,\cdots,o_m$, but corresponding to different activation functions $f_1,f_2,\cdots,f_m$.

##### 【Neural Network】

A neural network is a directed graph and a computational graph.

#### II. Perceptron

##### 【Monolayer Perceptron】

Consider there is a neuron, which has two inputs $x_1,x_2$ with weights $w_1,w_2$. And we use symbolic function as activation function:

$$
f(x) = sgn(x)=-1,\ x<0\ |\ 1,\ x\geq 0
$$

In training process, weights will be updated according to the following method:

$$
w'\leftarrow + \alpha \cdot (y-o)\cdot x
$$

About the above parameters, $o$ means activation value, $y$ is expected objective value, $\alpha$ is learning rate.

##### 【Multi-Layer Perceptron, MLP】

We can take an example to specifically understand how different the MLP is. Supposing that we choose XOR function as model's activation function:

$$
f(x_1,x_2)=0,\ x_1=x_2\ \ |\ \ 1,\ x_1\neq x_2
$$

And in this case, as the equation described, we just need two inputs to get our activate value. But how can we achieve XOR operation by using some neurons? As mentioned above, Monolayer Perception can fit a hyperplane $y=ax_1+bx_2$. And this is suitable for linearly separable problems. That's because our mapping relationship is linear. So if we use one more layer, there are maybe good changes in mapping relationship between inputs and outputs.
Now we set two hidden layer neurons $h_1, h_2$, and then we can get new equations:

$$
h_1=w_{11}x_1+w_{21}x_2,\ \ h_2=w_{12}x_1+w_{22}x_2
$$

$$
y=0,\ h_1=h_2\ \ |\ \ 1,\ h_1\neq h_2
$$

#### III. BP Neural Network

Define: Back Propagation(BP) algorithm propagates the error backward from the output layer to the front layer, and uses the error of the latter layer to estimate the error of the previous layer.

##### 【Gradient Descent】

In order to ensure the error back propagation, we utilize gradient descent algorithm to search in the weight space int the direction of the fastest error decline.

$$
w\leftarrow w+\Delta w
$$

$$
\Delta w=-\alpha\triangledown Loss(w)=-\alpha\frac{\partial{Loss}}{\partial{w}}
$$

Commonly used loss functions

+ Mean Squared Error, MSE

$$
Loss(o, y)=\frac{1}{n}\sum\limits_{i=1}^n|o_i-y_i|^2
$$

+ Cross Entropy, CE

$$
Loss(x_i)=-\log(\frac{\exp(x_i)}{\sum_j\exp(x_j)})
$$

##### 【Back Propagation】

The key to backpropagating the error is to use the chain rule for partial derivatives. Take the following neural network as example to demonstrate.

$$
o=f_3(w_6\cdot f_2(w_5\cdot f_1(w_1\cdot i_1+w_2\cdot i_2)+w_3\cdot i_3)+w_4\cdot i_4)
$$

In gradient descent, in order to calculate $\Delta w_k$, we need to utilize the chain rule to get $\frac{\partial{Loss}}{\partial{w_k}}$. For example, to calculate $\frac{\partial{Loss}}{\partial{w_1}}$:

$$
\frac{\partial{Loss}}{\partial{w_1}}=\frac{\partial{Loss}}{\partial{f_3}}\frac{\partial{f_3}}{\partial{f_2}}\frac{\partial{f_2}}{\partial{f_1}}\frac{\partial{f_1}}{\partial{w_1}}
$$

##### 【Dropout Regularization】

As a kind of regularization method, dropout will reduce overfitting in neural network by avoiding features co-adaptations.

```python
import torch.nn as nn
import torch
# set coding environments

p, count, iters, shape = 0.5, 0., 50, (5, 5)
dropout = nn.Dropout(p=p)
dropout.train()

for _ in range(iters):
    activations = torch.rand(shape) + 1e-5
    output = dropout(activations)
    count += torch.sum(output == activations *( 1/(1-p)))

print("In training mode Dropout influenced {} neurons".format(1-float(count)/(activations.nelement() * iters)))

count = 0
dropout.eval()

for _ in range(iters):
    activations = torch.rand(shape) + 1e-5
    output = dropout(activations)
    count += torch.sum(output == activations)

print("In evaling mode Dropout influenced {} neurons".format(1-float(count)/(activations.nelement() * iters)))
>>> In training mode Dropout influenced 0.4952 neurons
>>> In evaling mode Dropout influenced 0.0 neurons
```

##### 【Batch Normalization】

+ Internal Covariate Shift
  When training neural network, we always need to normalize input data to speed up the training process. However, learning algorithms for example SGD will continuously change the parameters in network and thus the distribution of activations in hidden layer will also change.
+ Batch Normalization Functions
  + Accelerate training
  + High learning rate ability
  + Regularization
+ Batch Normalization Achievement
  For a specific activation $x^{(k)}$, we can use it to demonstrate how Batch Normalization works.
  Now define: current batch has $m$ activations that is $\beta$:

$$
\beta = (x_1,x_2,\cdots,x_m)
$$

Firstly, we need to calculate mean and variance of $\beta$

$$
\mu_{\beta}=\frac{1}{m}\sum\limits_{i=1}^mx_i,\ \delta^2_{\beta} = \frac{1}{m}\sum\limits_{i=1}^m(x_i-\mu_{\beta})^2
$$

Secondly, it's necessary to normalize $\beta$ using calculated mean $\mu_{\beta}$ and variance $\delta^2_{\beta}$:

$$
\widehat{x_i}=\frac{x_i-\mu_{\beta}}{\delta_{\beta}^2+\xi}\sim N(0, 1)
$$

And $\xi = 1\times10^{-5}$ is used to avoid zero division.

+ Batch Normalization Affine Mapping
  Some hidden layers need data that is not standardized distribution, so Batch Normalization provides affine mapping $y_i=\gamma \widehat{x}_i+\beta$ for standard variables $x_i$ to restore expression ability in neural network. And these parameters will be trained with network original weights.
  In training process, calculates the mean and variance of the moving average:

$$
running_{mean} = (1-momentum)\times running_{mean} + momentum\times \mu_{\beta}
$$

$$
running_{var} = (1-momentum)\times running_{var} + momentum\times \delta^2_{\beta}
$$

After model training, we can get trained two parameters $\beta$ and $\gamma$ and two variables `running_mean` and `running_var`. If we use this model to do inference, we need to do the following transformation:

$$
y = \frac{\gamma}{\sqrt{running_{var}}+\xi}\cdot x+(\beta-\frac{\gamma}{\sqrt{running_{var}}+\xi}\cdot running_{mean})
$$

+ Batch Normalization Usage
  In Pytorch, `torch.nn.BatchNorm1d` achieved Batch Normalization function, also used as a typical layer in neural network. It has two critical parameters: `num_features` determines the number of features and `affine` determines whether Batch Normalization uses affine mapping or not.

```python
import torch
from torch import nn
# prepare running environments

m = nn.BatchNorm1d(num_features=5, affine=False)
print("BEFORE:")
print("running_mean:", m.running_mean)
print("running_var:", m.running_var)

for _ in range(100):
    input = torch.randn(20, 5)
    output = m(input)

print("AFTER:")
print("running_mean:", m.running_mean)
print("running_var:", m.running_var)

m.eval()
for _ in range(100):
    input = torch.randn(20, 5)
    output = m(input)

print("EVAL:")
print("running_mean:", m.running_mean)
print("running_var:", m.running_var)

BEFORE:
running_mean: tensor([0., 0., 0., 0., 0.])
running_var: tensor([1., 1., 1., 1., 1.])
AFTER:
running_mean: tensor([-0.0585, -0.0522, -0.0177,  0.0318,  0.0267])
running_var: tensor([1.0350, 1.0406, 0.9752, 1.0325, 0.9902])
EVAL:
running_mean: tensor([-0.0585, -0.0522, -0.0177,  0.0318,  0.0267])
running_var: tensor([1.0350, 1.0406, 0.9752, 1.0325, 0.9902])
```

In Batch Normalization, two parameters of affine mapping $\gamma$ and $\beta$ are called weight and bias. When not using affine, these parameters will be set `None`.

```python
print("no affine, gamma:", m.weight)
print("no affine, beta:", m.bias)

m_affine = nn.BatchNorm1d(num_features=5, affine=True)

print("with affine, gamma:", m_affine.weight, type(m_affine.weight))
print("with affine, beta:", m_affine.bias, type(m_affine.bias))
BEFORE:
running_mean: tensor([0., 0., 0., 0., 0.])
running_var: tensor([1., 1., 1., 1., 1.])
AFTER:
running_mean: tensor([-0.0585, -0.0522, -0.0177,  0.0318,  0.0267])
running_var: tensor([1.0350, 1.0406, 0.9752, 1.0325, 0.9902])
EVAL:
running_mean: tensor([-0.0585, -0.0522, -0.0177,  0.0318,  0.0267])
running_var: tensor([1.0350, 1.0406, 0.9752, 1.0325, 0.9902])
>>> no affine, gamma: None
>>> no affine, beta: None
>>> with affine, gamma: Parameter containing:
tensor([1., 1., 1., 1., 1.], requires_grad=True) <class 'torch.nn.parameter.Parameter'>
>>> with affine, beta: Parameter containing:
tensor([0., 0., 0., 0., 0.], requires_grad=True) <class 'torch.nn.parameter.Parameter'>
```

As the results described, the types of `m_affine.weight` and `m_affine.bias` are both `Parameter`. That means they will be particapated in model training process but the types of `running_mean` and `running_var` are both `Tensor` which is called `buffer` updated and saved as intermediate variables.

### Chapter 7 Convolutional Neural Network and Computer Vision

#### 7.1 Convolutional Neural Network Basic Ideas

##### I. Local Connections

For traditional BP neural networks, previous layer usually connects with next layer by global connections.
Suppose that there is a former layer with $M$ nodes and a latter layer with $N$ nodes, the network will increase more $M\times N$ weights. So it results in calculation cost and memory cost with the complexity of $O(M\times N)=O(n^2)$.
On the other hand, local connections mode only connects nearby nodes between layers, based on the knowledge that only combination of some local pixels presents some features together. If we limit the connections to $C$ nearby nodes in space, the connected weights will be reduced to $C\times N$. In that mode, the complexity of calculation and memory will be also reduced to $O(C\times N)=O(N)$.

##### II. Parameters Sharing

In image processing, we hold the bilief that the features of image have locality. Because local features don't have similarity, if we use specific weights for different local features it will not get better performance. And consider different images has great difference in the structure, we share connected weights between nodes with different local features.
According to this mode, we can further reduce the number of connected weights to $C$ with the complexity of $O(C)$.

#### 7.2 Convolutional Operation

Discrete convolution operation satisfies above properties which are local connections and  parameters sharing.
Define continuous convolution operation $f*g$:

$$
(f*g)(t)=\int_{-\infty}^{\infty}f(\tau)g(t-\tau)d\tau
$$

And one-dimensional discrete convolution operation:

$$
(f*g)(x)=\sum\limits_if(i)g(x-i)
$$

+ Multiple Convolutional Kernels
  CNN usually uses multiple convolutional kernels to extract features for better performance.
+ Multiple channels convolution
+ Boundary Filling
  In order to avoid convolutional feature collapse, we usually fill the bounds of the input tensor to make the center of the convolution kernel can start scanning from the boundary. In this way, the size of the input tensor and output tensor of the convolution operation does not change.

#### 7.3 Classic Network Structure

##### I. VGG Network

【Features】

+ Replace big size kernels with multiple size of $3\times 3$ kernels
+ Same receptive field of the convolutional kernels but deeper network structure
+ Reduce model's parameters using size of $3\times 3$ kernels

##### II. InceptionNet

##### III. ResNet
