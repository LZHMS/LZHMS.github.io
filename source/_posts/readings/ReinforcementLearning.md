---
title: Reinforcement Learning
date: 2023-11-21 20:34:26
toc: true
tags:
    - Reinforcement Learning
categories: readings
excerpt: My notes about reinforcement learning based on the course of Mathematical Foundations of Reinforcement Learning taught by Shiyu Zhao in Westlake University.
---
## I. Introduction

Inspiration of AlphaGo Story:

+ Machine can beat human people physically and intelligently.
+ A new era for reinforcement learning and artificial intelligence:

{% message color:info size:default%}
The ultimate goal of reinforcement learning is to find the optimal policy.
{% endmessage %}

## II. Basic Concepts

+ *State*: The status of agent with respect to the environment.
+ *State Space*: the set of all states.

$$
S=\{s_i\}
$$

+ *Action*: For each state, there are some actions: $a_1, a_2, \cdots, a_n$.
+ *Action Space of A State*: the set of all possible actions of a state.

$$
A(s_i)=\{a_i\}
$$

+ *State Transition*: when taking an action, the agent may move from one state to another.

$$
s_1\overset{a_1}{\longrightarrow} s_2
$$

+ *Forbidden Area*: the forbidden area is accessible but with penalty or inaccessible.
+ *Tabular Representation of State Transition*: using a table to describe the state transition.(*Deterministic Situation*)
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/RL/image.6dg2aezlho40.webp" width="70%" />
+ *State Transition Probability*: use probability to describe state transition.

<center>$p(s_2|s_1, a_2)=1$</center>
<center>$p(s_i|s_1, a_2)=0\ \ \forall i\neq 2$</center>

+ *Policy*: tells the agent what actions to take at a state.

  + *Deterministic Policy*

  <center>$\pi(a_1|s_1)=0$</center>
    <center>$\pi(a_2|s_1)=1$</center>
    <center>$\pi(a_3|s_1)=0$</center>
    <center>$\vdots$</center>
    <center>$\pi(a_{n-1}|s_1)=0$</center>
    <center>$\pi(a_n|s_1)=0$</center>
  
  + *Stochastic Policy*
    <center>$\pi(a_1|s_1)=0$</center>
    <center>$\pi(a_2|s_1)=0.5$</center>
    <center>$\pi(a_3|s_1)=0.5$</center>
    <center>$\vdots$</center>
    <center>$\pi(a_{n-1}|s_1)=0$</center>
    <center>$\pi(a_n|s_1)=0$</center>
+ *Tabular Representation of A Policy*
  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/RL/image.44koh66xciy0.webp" width="70%" />
+ *Reward*: a real number we get after taking an action.(*Human-machine Interface*)

  + *Tabular Representation of Reward Transition*

  <img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/RL/image.3wz8u0b5ql20.webp" width="70%" />

  + *Stochastic Reward Transition*: conditional probability

  <center>$p(r=-1|s_1, a_1)=0.5$</center>
    <center>$p(r\neq-1|s_1, a_1)=0.5$</center>
+ *Trajectory*: a state-action-reward chain

  $$
  s_1\mathop{\longrightarrow}\limits_{r=0}^{a_2}s_2\mathop{\longrightarrow}\limits_{r=0}^{a_3}s_3\cdots s_{n-1}\mathop{\longrightarrow}\limits_{r=1}^{a_{n-1}}s_n
  $$

  + *Return of A Trajectory*: the sum of all the rewards

  <center>$Return=0+0+0+\cdots+1$</center>

  + *Discounted Return*: the sum of all the rewards multiplied by discount factor $\gamma\in[0, 1]$

    <center>discounted return = $r_1+\gamma r_2+\gamma^2 r_3+\cdots+\gamma^{n-1} r_n$</center>

    + the sum becomes finite
    + balance the far and near future rewards
+ *Episode*
  When interacting with the environment following a policy, the agent may stop
  at some terminal states. The resulting trajectory is called an episode (or a
  trial).
  {% message color:info %}
  **_episodic tasks_**: tasks with episodes which has finite trajectories.
  **_continuing tasks_**: tasks without terminal states, meaning the interaction with the environment will never end.
  {% endmessage %}

  + Convert episodic tasks to continuing tasks
    + Treat the target state as a special absorbing state. Once the agent reaches an absorbing state, it will never leave. The consequent rewards $r = 0$.
    + Treat the target state as a normal state with a policy. The agent can still leave the target state and gain $r = +1$ when entering the target state.

## References

+ [Mathematical Foundations of Reinforcement Learning](https://github.com/MathFoundationRL/Book-Mathmatical-Foundation-of-Reinforcement-Learning)
