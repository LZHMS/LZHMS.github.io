---
title: Chapter 1 to 4
date: 2023-09-01 21:09:32
toc: true
tags:
	- Machine Learning
	- Computer Vision
categories: blog
excerpt: This is an article about Deep Learning Practice from Chapter 1 to 4.
---
### Chapter 1—Introduction of Deep Learning

#### Background

In 1981, neurobiologist David Hubel discovered the mechanism of information processing in the visual system, demonstrating that the visual cortex of the brain is hierarchical. His contribution is mainly two, one is that he believes that recognizing visual functions, one is abstraction, and the other is iteration.
Abstraction is the abstraction of very concrete, figurative elements, that is, primitive light pixels and other information, to form meaningful concepts. These meaningful concepts will iterate upwards and become more abstract concepts that people can perceive.
Thus, for computers, it needs to simulate the process of abstraction and recursive iteration.

#### Modern Deep Learning

Convolutional neural networks(CNN) simulate this process with convolutional layers that are usually stacked.
The lower convolutional layers can extract local features of the images, such as corners, edges, lines, and so on. The higher convolutional layers are able to learn more complex features from the lower convolutional layers to achieve classification and recognition of the images.

#### Reinforcement Learning

Reinforcement Learning mainly includes 4 elements: agent, state, action, reward.

+ Features

> There are no supervisors, only a feedback signal.
> Feedback is delayed and not generated immediately.
> Reinforcement learning is sequential learning, and time has an important meaning in reinforcement learning.
> The behavior of the agent will affect all future decisions.

### Chapter 2—Deep Learning Framework

#### Caffe

Caffe(Convolutional Architecture for Fast Feature Embedding): mainly used in video and image processing.
The official website of Caffe is `http://caffe.berkeleyvision.org/`

#### TensorFlow

TensorFlow is an open-source database that uses data flow graphs for numerical computation. Nodes represent mathematical operations in the graph, and the lines in the graph represent multidimensional arrays of data, that is, tensors, that are related to each other between nodes.
Computation Graph in TensorFlow:

+ The leaf node or start node is always a tensor.
+ Tensors cannot appear as non-leaf nodes.
+ Computational graphs always express complex operations in a hierarchical order.

#### Pytorch

The biggest advantage of PyTorch is that the neural network built is dynamic, while TensorFlow and Caffe are both static neural network structures.
The design of PyTorch follows the three levels of abstraction from low to high, $tensor\rightarrow variable(autograd)\rightarrow nn.Module$, representing high-dimensional arrays, automatic derivation, and neural networks.

### Chapter 3—Machine Learning Basics

#### Basic Concepts

+ Loss Function: $L(y, \hat{y})$ is a measure of model error.
+ Training Error: average error on the training set.
+ Generalization Error: average error on the test set.

If we unilaterally pursue the minimization of training errors, it will lead to an increase in the complexity of model parameters, resulting in overfitting of the model.
To prevent overfitting:

+ Validate set tuning parameters

The selection of parameters (i.e. parameter tuning) must be carried out on a dataset independent of the training and testing sets, and such a dataset used for model tuning is called a development or validation set.

+ Loss function for regularization

Regularization is added to the optimization objective to punish the complexity of redundancy.

$$
\mathop{min}\limits_{h}L(\boldsymbol{y},\boldsymbol{\hat{y}};\boldsymbol{\theta})+\lambda\cdot J(\boldsymbol{\theta})
$$

#### Supervised Learning

Supervised learning is mainly applicable to two main types of problems: regression and classification.

+ Classification

Model evaluation metrics:

+ Balanced problems:     $Accuracy = \frac{k}{D}$
+ Non-balanced problems:       $F-Metric$

> Non-balanced problems
> Define the class that is a minority of the sample as a positive class and the class that is a majority of the sample as a negative class

【Predictions】

+ Predict a positive sample as a positive class(true positive, TP)
+ Predict a negtive sample as a positive class(false positive, FP)
+ Predict a positive sample as a negtive class(flase negtive, FN)
+ Predict a negtive sample as a negtive class(true negtive, TN)

$$
Define\ \ recall:\ 
R = \frac{|TP|}{|TP|+|FN|}
$$

The recall rate measures the rate of correct detection by the model among all positive samples, so it also becomes the recall rate.

$$
Define\ \ precision:\ P = \frac{|TP|}{|TP|+|FP|}
$$

The accuracy rate measures the percentage of all samples predicted by the model to be positive, and is therefore also called the accuracy rate.
F-Metric reconciles the average between recall and precision.

$$
F_{\alpha}=\frac{(1+\alpha^2)RP}{R+\alpha^2P}
$$

$$
if\ \alpha=1\Longrightarrow F_1=\frac{2RP}{R+P}
$$

### Chapter 4—Pytorch Deep Learning

#### Pytorch Tensor Features

+ Tensor can use GPU for calculation
+ In the calculation, it can be automatically added to the calculation diagram as a node, and it can be automatically differentiated.

#### Tensor Object

##### Basic Operations

```python
>>> import torch
>>> import numpy as np
>>> print('torch.Tensor default format:{}'.format(torch.Tensor(1).dtype))
torch.Tensor default format:torch.float32    # single point float
>>> print('torch.tensor default format:{}'.format(torch.tensor(1).dtype))
torch.tensor default format:torch.int64    # 64 bits integer
# Create tensor by list
>>> a = torch.tensor([[1,2,3],[3,4,5]],dtype=torch.float64)
>>> print(a)
tensor([[1., 2., 3.],
        [3., 4., 5.]], dtype=torch.float64)
# Create tensor by ndarray
>>> b = torch.tensor(np.array([[1,2,3],[3,4,5]]),dtype=torch.uint8)
>>> print(b)
tensor([[1, 2, 3],
        [3, 4, 5]], dtype=torch.uint8)
 
# Set tensor device
>>> cuda0 = torch.device('cuda:0')
>>> c = torch.ones((2,2),device=cuda0)
>>> print(c)                       
tensor([[1., 1.],
        [1., 1.]], device='cuda:0')
# copy tensor to CPU
>>> c = c.to('cpu', torch.double)
>>> print(c.device)
cpu

# tensors multiply
>>> a = torch.tensor([[1,2],[3,4]])
>>> b = torch.tensor([[1,2],[3,4]]) 
>>> c = a*b        # multiply by corresponding element
>>> print(c)
tensor([[ 1,  4],
        [ 9, 16]])
>>> c = torch.mm(a, b)   # multiply by metrixs
>>> print(c)
tensor([[ 7, 10],
        [15, 22]])
   
# Special functions
>>> a = torch.tensor([[1,2],[3,4]])
# Discard too small or too large elements in matrix
>>> torch.clamp(a,min=2,max=3)
tensor([[2, 2],
        [3, 3]])
# Rounds to the nearest integer
>>> a=torch.tensor([[-1.2, -1.5, 0.3], [-0.8, 7.4, 1.2]])
>>> torch.round(a)
tensor([[-1., -2.,  0.],
        [-1.,  7.,  1.]])
# Hyperbolic tangent function, mapping function values to (0,1)
>>> torch.tanh(a)
tensor([[-0.8337, -0.9051,  0.2913],
        [-0.6640,  1.0000,  0.8337]])
   
# Create tensors
>>> print(torch.arange(5))
tensor([0, 1, 2, 3, 4])
>>> print(torch.arange(1,10,2))   # the third parameter is foot length
tensor([1, 3, 5, 7, 9])
>>> print(torch.linspace(0, 5, 10))  # the third parameter is elements number
tensor([0.0000, 0.5556, 1.1111, 1.6667, 2.2222, 2.7778, 3.3333, 3.8889, 4.4444, 5.0000])
>>> print(torch.ones(3,3))
tensor([[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]])
>>> print(torch.zeros((3,3), dtype=torch.uint8))
tensor([[0, 0, 0],
        [0, 0, 0],
        [0, 0, 0]], dtype=torch.uint8)

# Random numbers
# uniform distribution of [0,1]
>>> torch.rand(3,3)
tensor([[0.7228, 0.4286, 0.5304],
        [0.4195, 0.0597, 0.5446],
        [0.0225, 0.4951, 0.5704]])
# elements that are sampled satisfy normal distribution
>>> torch.randn(3,3)
tensor([[-0.5142,  0.6429, -1.8848],
        [-0.7895, -0.5115,  0.0638],
        [ 1.7370, -2.2135,  0.1514]])
# uniform distribution of {a,...,b} and not including b
>>> torch.randint(0,9,(3,3))
tensor([[4, 6, 6],
        [3, 7, 6],
        [2, 7, 4]])
```

##### Indexes and Slices

```python
>>> a = torch.arange(9).view(3,3) 
>>> a[2,2]    # basic index
tensor(8)
>>> a[1:, :-1]   # slice
tensor([[3, 4],
        [6, 7]])
>>> a[::2]    # slice with foot length
tensor([[0, 1, 2],
        [6, 7, 8]])
>>> rows=[0,1]
>>> cols=[2,2]
>>> a[rows,cols]   # integer index
tensor([2, 5])
>>> index=a>4    # bool index
>>> print(index)
tensor([[False, False, False],
        [False, False,  True],
        [ True,  True,  True]])
>>> print(a[index])
tensor([5, 6, 7, 8])
# torch.nonzero return index matrix that corresponding value is not zero
>>> a = torch.randint(0,2,(3,3))
>>> print(a)
tensor([[1, 0, 1],
        [1, 1, 0],
        [0, 1, 0]])
>>> index=torch.nonzero(a)
>>> print(index)
tensor([[0, 0],
        [0, 2],
        [1, 0],
        [1, 1],
        [2, 1]])
>>> [[i,j,a[i, j]] for i, j in index]
[[tensor(0), tensor(0), tensor(1)], 
[tensor(0), tensor(2), tensor(1)], 
[tensor(1), tensor(0), tensor(1)], 
[tensor(1), tensor(1), tensor(1)], 
[tensor(2), tensor(1), tensor(1)]]
# torch.where(condition, x, y) if condition is true then return corresponding element in x else return the corresponding element in y. x.shape()=y.shape()
>>> x = torch.randn(3,2)
>>> y = torch.ones(3,2)
>>> print(x)
tensor([[-1.2620, -1.9865],
        [ 1.1022,  0.8017],
        [-0.5744, -0.4387]])
>>> print(torch.where(x>0,x,y))
tensor([[1.0000, 1.0000],
        [1.1022, 0.8017],
        [1.0000, 1.0000]])
```

##### Tensor Transformation, Splicing and Splitting

```python
>>> a = torch.rand(1,2,3,4,5)
>>> print("the number of elements:", a.nelement())  
the number of elements: 120
>>> print("the number of axises:", a.ndimension())
the number of dimensions: 5 
>>> print("matrix dimension:", a.size(), a.shape)   
matrix dimension: torch.Size([1, 2, 3, 4, 5]) torch.Size([1, 2, 3, 4, 5])  

# Tensor Reshape
# For Tensor.view, physical storage for tensors must be continuous
>>> a = torch.rand(1,2,3,4,5)
>>> b = a.view(2*3,4*5)
>>> print(b.shape)
torch.Size([6, 20])
>>> c = a.reshape(-1)  # if set -1 in some dimension, it will be calculated automatically
>>> print(c.shape)
torch.Size([120])
>>> d = a.reshape(2*3, -1)
>>> print(d.shape)
torch.Size([6, 20])
# discard some axises whose dimension is 1
>>> b = torch.squeeze(a)
>>> print(b.shape)
torch.Size([2, 3, 4, 5])
# add 1 dimension at axis 0
>>> torch.unsqueeze(b,0).shape
torch.Size([1, 2, 3, 4, 5])

# Tensor Transpose
# 2 dimensions matrix
>>> >>> b = torch.tensor([2,3])
>>> b
tensor([2, 3])
>>> print(torch.t(b))
tensor([2, 3])
>>> b = torch.tensor([[2,3]])   # transpose the outer matrix
>>> print(torch.t(b))
tensor([[2],
        [3]])
>>> print(torch.t(b[0]))
tensor([2, 3])
>>> print(torch.transpose(b, 1, 0))
tensor([[2],
        [3]])
# high latitude matrix
>>> a = torch.rand((1,224,224,3))
>>> print(a.shape) 
torch.Size([1, 224, 224, 3])
>>> b = a.permute(0,2,3,1)
>>> print(b.shape)
torch.Size([1, 224, 3, 224])

# Tensor splicing
# torch.cat will splice matrices on the existed axis
>>> a = torch.randn(2,3)
>>> b = torch.randn(3,3)
>>> c = torch.cat((a, b))    # input (a, b) must be tuple because there are other attributes
>>> d = torch.cat((b, b, b), dim=1)
>>> print(c.shape)
torch.Size([5, 3])
>>> print(d.shape) 
torch.Size([3, 9])
# torch.stack will splice metrices on a new axis
>>> c = torch.stack((b,b), dim=1) 
>>> d = torch.stack((b,b), dim=0) 
>>> print(c.shape)
torch.Size([3, 2, 3])
>>> print(d.shape)
torch.Size([2, 3, 3])

# Tensor Splitting
# torch.split input every matrix's size to be split
>>> a = torch.randn(10, 3)
>>> >>> for x in torch.split(a, [1,2,3,4], dim=0):
...  print(x.shape) 
... 
torch.Size([1, 3])
torch.Size([2, 3])
torch.Size([3, 3])
torch.Size([4, 3])
>>> for x in torch.split(a, 4, dim=0):    # also can be an integer   
...  print(x.shape)
... 
torch.Size([4, 3])
torch.Size([4, 3])
torch.Size([2, 3])
# torch.chunk input matrix total number to be split
>>> for x in torch.chunk(a, 4, dim=0):     
...  print(x.shape)
... 
torch.Size([3, 3])
torch.Size([3, 3])
torch.Size([3, 3])
torch.Size([1, 3])
```

##### Tensor Reduction

```python
>>> a = torch.tensor([1,2],[3,4])
>>> print("global maximum value:", torch.max(a))  
global maximum value: tensor(4)
>>> print("colomn  maximum value:", torch.max(a, dim=0))
colomn  maximum value: torch.return_types.max(
values=tensor([3, 4]),
indices=tensor([1, 1]))
>>> print("colomn  accumulation value:", torch.cumsum(a, dim=0))
colomn  accumulation value: tensor([[1, 2],
        [4, 6]])
>>> print("row multiplication value:", torch.cumprod(a, dim=1))
row multiplication value: tensor([[ 1,  2],
        [ 3, 12]])
>>> a = torch.Tensor([[1,2], [3,4]])   # data type to floating point
>>> a.mean(), a.median(), a.std()
(tensor(2.5000), tensor(2.), tensor(1.2910))
# calculate with the axis
>>> a.mean(dim=0), a.median(dim=0), a.std(dim=0)
(tensor([2., 3.]), torch.return_types.median(   
values=tensor([1., 2.]),
indices=tensor([0, 0])), tensor([1.4142, 1.4142]))
>>> a = torch.randint(0,3,(3,3))
>>> print(a)
tensor([[2, 0, 1], 
        [1, 2, 2], 
        [1, 0, 1]])
# torchl.unique function find unique elements in the matrix
>>> print(torch.unique(a))
tensor([0, 1, 2])

# Pytorch Tensor Automatic Differentiation
>>> x = torch.arange(9).view(3, -1)
>>> x.requires_grad
False
>>> x = torch.rand(3, 3, requires_grad=True)
>>> print(x)
tensor([[0.9214, 0.6373, 0.4736],
        [0.4541, 0.8828, 0.4526],
        [0.2727, 0.8647, 0.0488]], requires_grad=True)
>>> w = torch.ones(3,3,requires_grad=True)
>>> print(w)
tensor([[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]], requires_grad=True)
>>> y = torch.mm(w,x)
>>> print(y)
tensor([[1.6481, 2.3848, 0.9750],
        [1.6481, 2.3848, 0.9750],
        [1.6481, 2.3848, 0.9750]], grad_fn=<MmBackward0>)
# cancle tensor's automatic differentiation
>>> detached_y = y.detach()
>>> print(detached_y)
tensor([[1.6481, 2.3848, 0.9750],
        [1.6481, 2.3848, 0.9750],
        [1.6481, 2.3848, 0.9750]])
>>> yy = torch.mean(y)
>>> yy.backward()        # grad backward
>>> print(y.grad)
None
>>> print(w.grad)
tensor([[0.2258, 0.1988, 0.1318],
        [0.2258, 0.1988, 0.1318],
        [0.2258, 0.1988, 0.1318]])
>>> print(x.grad) 
tensor([[0.3333, 0.3333, 0.3333],
        [0.3333, 0.3333, 0.3333],
        [0.3333, 0.3333, 0.3333]])
# with torch.no_grad() includes code snippets that do not compute differentitation
>>> y =torch.sum(torch.mm(w,x)) 
>>> print(y.requires_grad)
True
>>> with torch.no_grad():
...  y = torch.sum(torch.mm(w,x))
...  print(y.requires_grad)
... 
False
```
