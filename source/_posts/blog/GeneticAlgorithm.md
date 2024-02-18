---
title: Genetic Algorithm 
date: 2024-01-28 20:26:16
toc: true
tags:
    - Mathematical Modeling
categories: blog
excerpt: 关于遗传算法的原理推导与实际应用。
---
### 算法简介
遗传算法（Genetic Algorithm，GA）是一种基于自然选择和遗传操作的随机全局搜索优化算法。它通过模拟自然选择和遗传中发生的复制、交叉(crossover)和变异(mutation)等现象，从任一初始种群（父代）开始，通过随机选择、交叉和变异操作，产生更具有生存优势的子代，使群体不断向搜索空间最优的方向进化，最后收敛到一群最适应环境的个体，从而求得问题的最佳解。

达尔文进化论保留了种群的个体性状，而遗传算法则保留了针对给定问题的候选解集合(即individuals)。这些候选解经过迭代评估 (evaluate)，生成子代解。更优的解有更大的机会被选择，并将其特征传递给下一代候选解集合。
### 相关概念
#### 染色体(Chromosome)/位串(Bit String)
个体的表示形式, 对应于遗传学中的染色体. 一条染色体表示为一个二进制串，其中每个位代表一个基因，表征染色体上是否存在该基因。
#### 基因(Gene)
基因是染色体中的元素，用于表示个体的特征，用 0 和 1 表示其是否存在于一条染色体上。
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/Gene.439bh5eejr60.svg" alt="Gene" />

#### 特征值(Eigenvalue)
在用串表示整数时，基因的特征值与二进制数的权一致，即二进制串的位权一致。

#### 基因型(Genotype)
+ 进化理论：通过基因型表征繁殖和突变，基因型是组成染色体的一组基因的集合。
+ 遗传算法：每个个体都由代表基因集合的染色体构成, 对应于位串(个体均为单染色体型)

#### 表现型(Phenotype)
生物体的基因型在特定环境下的表现特征, 对应于GA中的位串解码后的参数.

#### 适应度(Fitness)
各个个体对环境的适应程度叫做适应度(fitness)。在算法的每次迭代中，会使用适应度函数/目标函数对个体进行度量评估，得到其适应度值。

#### 种群 (Population)
遗传算法保持大量的个体 (individuals) —— 针对当前问题的候选解集合。由于每个个体都由染色体表示，因此这些种族的个体 (individuals) 可以看作是染色体集合：
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/Population.343grcq783m0.svg" alt="Population" />

#### 遗传算子(Genetic Operators)
+ 选择(Selection): 选择操作从种群中概率选择适应度值最高的个体作为父代，生成子代
+ 交叉(Crossover): 交叉操作将父代的两条染色体进行交叉，生成子代
随机地将选择的双亲样本的部分染色体互换(交叉)，以生成后代的两个新染色体，也称为基因重组
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/Cross.2azih1pg6xq8.svg" alt="Crossover" />

+ 变异(Mutation): 变异操作将父代的一条染色体进行变异，生成子代。
突变操作的目的是定期随机更新种群，将新模式引入染色体以便探索求解空间的未知区域，避免陷入局部最优。
<img src="https://cdn.jsdelivr.net/gh/LZHMS/picx-images-hosting@master/EBlog/Learning/Mutation.24c1r3y0ddsw.svg" alt="Mutation" />

### 算法原理
#### 染色体编码
+ 编码：原问题的解到基因型的映射，即将问题的可行解从其解空间转换到遗传算法的搜索空间
	+ 二进制数编码方案：染色体上的基因序列是由二进制表示的
	若参数 $U\in [U_1, U_2]$, 表示为长度 $k$ 的位串, 产生 $2^k$ 个不同基因型
	<center>$000\cdots 000=0\rightarrow U_1$</center>
	<center>$000\cdots 001=1\rightarrow U_1+\delta$</center>
	<center>$000\cdots 010=2\rightarrow U_1+2\delta$</center>
	<center>$\vdots$</center>
	<center>$111\cdots 111=2^k-1\rightarrow U_2$</center>
	其中，$\delta = \frac{U_2-U_1}{2^k-1}$
+ 解码：将染色体上的基因序列转换为原问题的可行解
$$
U = U_1 + (\sum^k_{i=1}b_i\cdot2^{i-1})\cdot\frac{U_2-U_1}{2^k-1}
$$
其中，$(\sum^k_{i=1}b_i\cdot2^{i-1})$表示将基因位串按权展开，进一步将其转换为原可行解

#### 初始种群
在遗传算法中, 需要随机初始化一个待进化种群 $P_0$, 并配置参数: 最大进化代数$T$，群体大小 $M$, 交叉概率 $P_c$, 变异概率 $P_m$.

#### 适应度尺度变换
在算法迭代过程中，利用适应度函数计算出每个个体的适应度值，但是由于其相对于原问题的目标函数可能存在群体间适应度相当而造成的竞争减弱，导致种群收敛于局部最优解。

因此，需要对适应度值进行尺度变换，以增强种群间的竞争能力，常用的经典方法有: **线性尺度变换**、**乘幂尺度变换**以及**指数尺度变换**

+ 线性尺度变换
$$
F' = aF+b
$$
其中，$a$为缩放系数，$b$为平移系数，$F$为变换前适应度值，$F'$为变换后适应度值。
+ 乘幂尺度变换
$$
F' = F^k
$$
其中, $k$ 为幂次，$F$为变换前适应度值，$F'$为变换后适应度值。
+ 指数尺度变换
$$
F' = e^{-\beta F}
$$
其中，$\beta$ 的大小决定了适应度尺度变换的强弱.

#### 选择操作
选择操作从旧群体中以一定概率选择优良个体组成新的种群，以繁殖得到下一代个体。个体被选中的概率跟适应度值有关，个体适应度值越高，被选中的概率越大。
$$
P_i = \frac{F_i}{\sum^M_{j=1}F_j}
$$

#### 交叉操作
交叉操作是指从种群中随机选择两个个体，依概率对两个染色体进行交换组合，把父串基因序列遗传给子串，从而产生新的个体。
+ 单点交叉算子: 该算子在配对的染色体中随机的选择一个交叉位置，然后在该交叉位置对配对的染色体进行基因位变换

#### 变异操作
为了防止遗传算法在优化过程中陷入局部最优解，在搜索过程中需要对个体进行变异，以探索新的解空间。
+ 单点变异算子：对基因序列中某一个位进行变异，随机变异为进制中其他一位

#### 终止条件
+ 算法已迭代到最大代数，主要用于限制运行时间和计算资源
+ 种群个体没有明显的改进，当代种群最佳适应度值与父代种群最佳适应度值相比，其差异小于某个阈值，则算法可以停止

### 应用实践
#### Configure Running Environment
```python
# prepare corresponding environment
import numpy as np
```
#### Parameters for Genetic Algorithm
+ Setting the crossover probability to 1 ensures adequate evolution of the population
+ In general, variation is less likely to occur, so a variation rate of 0.005 is set

```python
# Set parameters for Genetic Algorithm
GENE_SIZE = 48    # Gene length
POP_SIZE = 200     # Population size
CROSSOVER_RATE = 1   # Crossover rate
MUTATION_RATE = 0.005   # Mutation rate
N_GENERATIONS = 50   # Maximum generations

# Set parameters for optimization problems
X_BOUND = [-3, 3]
Y_BOUND = [-3, 3]
```
#### Objective Function
$$
F(x, y) = 3\times (1-x)^2\times e^{-[x^2+(y+1)^2]}-10\times (\frac{x}{5}-x^3-y^5)\times e^{-x^2-y^2}-\frac{1}{3^{e^{-(x+1)^2-y^2}}}
$$

```python
# define object function
def F(x, y):
	return 3*(1-x)**2*np.exp(-(x**2)-(y+1)**2)- 10*(x/5 - x**3 - y**5)*np.exp(-x**2-y**2)- 1/3**np.exp(-(x+1)**2 - y**2)
```
#### Coding Strategy
+ Parameters Demonstrate
    + `pop` represents the population matrix: 
        + A row represents a binary code represents DNA
        + The number of rows of the matrix is the number of populations
        + Odd columns represent X
        + Even columns represent Y
        
```python
def TranslateDNA(pop):
	
	x_pop = pop[:,1::2]
	y_pop = pop[:,::2]
	Gene_Size = GENE_SIZE / 2
	# pop:(POP_SIZE, GENE_SIZE)*(GENE_SIZE,1) --> (POP_SIZE,1)
	x = x_pop.dot(2**np.arange(Gene_Size)[::-1])/float(2**Gene_Size-1)*(X_BOUND[1]-X_BOUND[0])+X_BOUND[0]
	y = y_pop.dot(2**np.arange(Gene_Size)[::-1])/float(2**Gene_Size-1)*(Y_BOUND[1]-Y_BOUND[0])+Y_BOUND[0]
	return x, y
```

#### Crossover and Mutation
```python
def Mutation(child, Mutation_Rate=0.003):
	# Mutation with Mutation_Rate probability
	if np.random.rand() < Mutation_Rate:
		# Randomly generate an mutation location
		mutate_point = np.random.randint(0, GENE_SIZE)
		# Inverts the binary bit of the mutation point
		child[mutate_point] = child[mutate_point] ^ 1

def CrossoverMutation(pop, Crossover_Rate=0.8):
	new_pop = []

	# Traverse each individual in the population, taking that individual as the father
	for father in pop:		
		child = father		# The child first gets all the genes of the father
		
		# Crossover occurs with a certain probability when producing offspring
		if np.random.rand() < Crossover_Rate:
			# Another individual is selected in the population and that individual is taken as the mother	
			mother = pop[np.random.randint(POP_SIZE)]
			# Randomly generate an intersection
			cross_points = np.random.randint(low=0, high=GENE_SIZE)
			# The child gets the mother's genes located behind the intersection
			child[cross_points:] = mother[cross_points:]
		
		# Each child has a certain chance of mutating
		Mutation(child, MUTATION_RATE)
		new_pop.append(child)

	return new_pop
```
#### Calculate Fitness of New Population
+ Subtracting the minimum fitness is to prevent negative fitness
    + In this way, wa can gurantee the range of fitness is `[0, np.max(pred) - np.min(pred)]`
+ Add a small number to prevent the appearance of fitness to 0

```python
def GetFitness(pop): 
    x, y = TranslateDNA(pop)
    pred = F(x, y)   # Calculate objective value
    return (pred - np.min(pred)) + 1e-3
```
#### Select New Population
```python
# nature selection according to pop's fitness
def SelectPop(pop, fitness):
    idx = np.random.choice(np.arange(POP_SIZE), size=POP_SIZE, replace=True,
                           p=(fitness) / (fitness.sum()) )
    return pop[idx]
```

#### Print Results
```python
def Print_Info(pop):
	fitness = GetFitness(pop)
	max_fitness_index = np.argmax(fitness)
	print("Max_Fitness:", fitness[max_fitness_index])
	x, y = TranslateDNA(pop)
	print("Optimal gene:", pop[max_fitness_index])
	print("(x, y):", (x[max_fitness_index], y[max_fitness_index]))
	print("Optimal value:", F(x[max_fitness_index], y[max_fitness_index]))
```

#### Run GA
```python
def GeneticAlgorithm():
	# initial population
	pop = np.random.randint(2, size=(POP_SIZE, GENE_SIZE))
	
	for _ in range(N_GENERATIONS):
		pop = np.array(CrossoverMutation(pop, CROSSOVER_RATE))
		fitness = GetFitness(pop)
		pop = SelectPop(pop, fitness)
	
	Print_Info(pop)

if __name__ == '__main__':
    GeneticAlgorithm()
```
#### Running Results
```
Max_Fitness: 0.08738358838357263
Optimal gene: [1 0 0 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 0 1 0 1 0 1 1 0 1 0 1 0 0 0 1 0
 0 1 0 0 0 1 0 1 0 0 0]
(x, y): (-0.028632940568503074, 1.499641925075169)
Optimal value: 7.052501227315164
```
### References
+ [遗传算法(Genetic Algorithm, GA)详解与实现](https://zhuanlan.zhihu.com/p/436453994)