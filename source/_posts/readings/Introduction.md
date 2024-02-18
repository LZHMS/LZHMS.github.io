---
title: Introduction of Deep Reinforcement Learning
date: 2023-09-24 22:10:51
toc: true
tags:
    - Deep Reinforcement Learning 
categories: readings
excerpt: Reading notes about introduction to the theory of Deep Reinforcement Learning.
---
## Reading Notes about the book *Deep Reinforcement Learning* written by Aske Plaat

Recently, I have been reading the book *Deep Reinforcement Learning* written by Aske Plaat. This book is a good introduction to the theory of Deep Reinforcement Learning. And it is very inspiring when I learn the theory of Deep Reinforcement Learning.

### Introduction of Deep Reinforcement Learning

#### Reinforcement Learning

The author defines reinforcement learning as a field in which an agent learns by interacting with an environment. So my understanding is that there is an target environment which provides feedback signals for the action the agent had made. It is a form of active learning and has a certain level of autonomy.

#### Definition of Deep Reinforcement Learning

As the book defines it, "Deep reinforcement learning is the combination of deep learning and reinforcement learning." Compared with deep learning which is about approximating functions, this learning method is about learning from feedback and will utimately find a solution by trial and error.
About their application, there is a table as shown below:

|        Dataset Feature        |     Low-Dimensional States     |   High-Dimensional States   |
| :---------------------------: | :----------------------------: | :-------------------------: |
|        Static Dataset        |  classic supervised learning  |  deep supervised learning  |
| Agent/Environment Interaction | tabular reinforcement learning | deep reinforcement learning |

In my opinion, deep reinforcement learning combines the features between deep learning and reinforcement learning so it will be also used for large, complex, high-dimensional environments.

What the most important part of reinforcement learning is all about learning from success as well as from mistakes. The author inspired me that we can apply the human learning process or mechanism to the Artificial Intelligence. Just from this point, success is so good for summarizing the skills and mistakes is also good for accumulating experience.

#### Sequntial Decision Problems

For me, this topic is so fresh and interesting. "Many real world problems can be modeled as a sequence of decisions.", as the author described. We can view many daily problems as a sequence of decisions. Just like an algorithm to some problems, it has specific steps. But the difference is that the steps in sequntial decision problems are dynamic and algorithm is determined. Sequential decision steps will also have dynamic choices cooresponding to its environment which is a mirror image of the real world. It has two types of applications: robotic problems and games.

+ Robotic problems
  Previously, all actions that a robot should take can be pre-programmed step-by-step by a programmer in meticulous detail. But this mode will be a disadvantage when the environments are slightly challenging. To fix this problem, we need the robot to be able to respond more flexible to different conditions. In this way, an adaptive program is needed which is just about today's main topic.

#### Four Related Fileds

Reinfocement is a field of interdisciplinary integration involving psychology, mathematics, engineering and biology.

+ Psychology
  Reinforcement is also known as *leaning by conditioning*. Just to be simple, it is the process of learning by trial and error. That is continuous practice makes learning become a memeory of the body.
+ Mathematics
  Discrete optimization and graph theory are the basics of formulization of reinforcement learning. And we all kown it is the Mathematical formalizations that develops efficient planning and optimization algorithms. Symbolic reasoning and continuous optimization also play an important role in reinforcement.
+ Engineering
  The filed of reinforcement learning is better known as optimal control. That is, applying reinforcement learning to the engineering has beening a creative way for optimal problems.
+ Biology
  Nature-inspired optimization algorithms have been developed in artificial intelligence. It is worth mentioning connectionist AI.

  + Mathematical logic and engineering approach intelligence
    It is a top-down deductive process with intelligence following deductively from theory.
  + Connnectionism approach intelligence
    It is a bottom-up fashion and will forming intelligence out of many low level interactions. That is, intelligence follows inductively from parctice.

#### Reinforcement Learning Paradigm

+ Basic components:
  + Dataset: produced dynamically.
  + Agent: doess the learning of the policy.
  + Environment: provides feedback.
  + Goal: maximize the long term accumulated expected reward.

There is a table showing the differences between reinforcement learning and supervised learning.

|   Concept   |  Supervised Learning  |     Reinforcement Learning     |
| :---------: | :--------------------: | :----------------------------: |
| Inputs$x$ | Full dataset of states |  Partial(One state at a time)  |
| Labels$y$ |  Full(correct action)  | Partial(Numeric action reward) |

### Reference

+ [Aske Plaat, &#34;Deep Reinforcement Learning,&#34; Springer Nature Singapore, 2022](https://arxiv.org/abs/2201.02135)
