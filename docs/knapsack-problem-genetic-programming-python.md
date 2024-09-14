# Python 中的遗传编程：背包问题

> 原文：[`www.kdnuggets.com/2023/01/knapsack-problem-genetic-programming-python.html`](https://www.kdnuggets.com/2023/01/knapsack-problem-genetic-programming-python.html)

![使用遗传编程解决背包问题：Python 实现](img/7b463d16bed21ff7a97ece2d230b0111.png)

使用 DALL•E 创建的图像

> * * *
> 
> ## 我们的三大课程推荐
> ## 
> ![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。
> 
> ![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力
> 
> ![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT
> 
> * * *
> 
> **编辑注释：** 正如读者 Monem 在下面的评论中指出的，这是使用 **遗传算法** 来解决背包问题的一个例子，而不是遗传编程。我参与了文章的制作，并犯了一个非常基本的错误，一直延续到现在。为了透明起见，我保留了原文，并附上此更正说明。对引起的困惑表示歉意。

在这篇文章中，我们将探讨背包问题，这是计算机科学中的经典问题。我们将解释为什么使用传统计算方法很难解决这一问题，以及遗传编程如何帮助找到“足够好”的解决方案。之后，我们将查看一个 Python 实现的具体解决方案，并亲自测试一下。

# 背包问题

背包问题可以用来说明解决复杂计算问题的困难。在最简单的形式中，给定一个一定容量的背包、一组物品及其大小和价值，并要求在不超过容量的情况下最大化放入背包的物品的价值。背包问题可以有多种表述方式，但一般认为使用传统算法解决是一个困难的问题。

背包问题的难点在于它是一个 NP 完全问题，这意味着没有已知的解决方案可以保证全局最优解。因此，该问题难以处理，无法使用传统方法快速解决。解决背包问题的最佳已知算法涉及使用暴力搜索或启发式方法，这些方法可能会有较长的运行时间，并且可能无法保证最优解。

# 遗传编程

然而，遗传编程可以提供一种寻找背包问题解决方案的替代方法。遗传编程是一种使用进化算法来搜索复杂问题解决方案的技术。通过使用遗传编程，可以快速找到对给定问题“足够好”的解决方案。它还可以用来优化和改进解决方案。

在遗传编程中，一组可能的解决方案（或初始生成）被随机生成，然后根据一组标准进行评估。那些最符合标准的解决方案将被选中，并应用遗传变异以创建新的解决方案变体（或后续生成）。然后评估这些新一代变体，并重复这个过程，直到找到令人满意的解决方案。该过程会重复，直到找到最佳的或“足够好”的解决方案。

使用遗传编程解决背包问题的优势在于，可以快速找到足够好的解决方案，而不需要穷举所有可能的解决方案。这使得它比传统算法更高效，能够更快地找到解决方案。

# 解释代码

现在我们已经了解了背包问题是什么，遗传编程是什么，以及为什么我们要使用后者来尝试解决前者，让我们来看一下上述描述的 Python 实现。

我们将逐一了解重要函数，然后再查看整体程序。

该程序使用 Python 实现，未使用任何第三方库。

## 生成随机种群

此函数生成给定大小的随机种群。它使用 for 循环遍历给定的大小，并为每次迭代创建一个染色体。这个染色体是由 0 和 1 组成的列表，使用 random.choice()函数生成。然后将染色体附加到种群列表中。最后，函数打印出一条消息并返回种群列表。此函数对于创建遗传算法的个体种群非常有用。

```py
def generate_population(size):
	population = []
	for _ in range(size):
		genes = [0, 1]
		chromosome = []
		for _ in range(len(items)):
			chromosome.append(random.choice(genes))
		population.append(chromosome)
	print("Generated a random population of size", size)
	return population
```

## 计算染色体适应度

此函数用于计算染色体的适应度。它以染色体作为参数，并对其进行迭代。如果染色体在给定索引处的值为 1，它就将对应项的重量和值分别添加到总重量和总值中。如果总重量超过最大重量，则适应度设置为 0\。否则，适应度设置为总值。该函数在遗传算法中用于确定给定染色体的适应度。

```py
def calculate_fitness(chromosome):
	total_weight = 0
	total_value = 0
	for i in range(len(chromosome)):
		if chromosome[i] == 1:
			total_weight += items[i][0]
			total_value += items[i][1]
	if total_weight > max_weight:
		return 0
	else:
		return total_value
```

## 选择染色体

这个函数用于从一个种群中选择两个染色体进行交叉操作。它首先使用 `calculate_fitness()` 函数计算种群中每个染色体的适应度值。然后，它通过将每个值除以所有适应度值的总和来对适应度值进行归一化。最后，它使用 `random.choices()` 函数根据归一化的适应度值随机选择两个染色体。然后，这两个选择的染色体作为交叉操作的父代染色体返回。

```py
def select_chromosomes(population):
	fitness_values = []
	for chromosome in population:
		fitness_values.append(calculate_fitness(chromosome))

	fitness_values = [float(i)/sum(fitness_values) for i in fitness_values]

	parent1 = random.choices(population, weights=fitness_values, k=1)[0]
	parent2 = random.choices(population, weights=fitness_values, k=1)[0]

	print("Selected two chromosomes for crossover")
	return parent1, parent2
```

## 执行交叉操作

这个函数在两个染色体之间执行交叉操作。它接受两个父代染色体作为输入，并随机选择一个交叉点。然后，通过在交叉点结合两个父代染色体来创建两个子代染色体。第一个子代染色体由第一个父代染色体的前半部分和第二个父代染色体的后半部分组成。第二个子代染色体由第二个父代染色体的前半部分和第一个父代染色体的后半部分组成。最后，这两个子代染色体作为输出返回。

```py
def crossover(parent1, parent2):
	crossover_point = random.randint(0, len(items)-1)
	child1 = parent1[0:crossover_point] + parent2[crossover_point:]
	child2 = parent2[0:crossover_point] + parent1[crossover_point:]

	print("Performed crossover between two chromosomes")
	return child1, child2
```

## 执行突变

这个函数对染色体进行突变。它接受一个染色体作为参数，并使用 `random` 模块生成一个介于 0 和染色体长度之间的随机数。如果突变点的值是 0，它将被更改为 1；如果是 1，它将被更改为 0。然后，函数打印一条消息并返回突变后的染色体。这个函数可以用来模拟生物种群中的基因突变。

```py
def mutate(chromosome):
	mutation_point = random.randint(0, len(items)-1)
	if chromosome[mutation_point] == 0:
		chromosome[mutation_point] = 1
	else:
		chromosome[mutation_point] = 0
	print("Performed mutation on a chromosome")
	return chromosome
```

## 获取最佳染色体

这个函数接受一个染色体种群并返回种群中最好的染色体。它首先为种群中每个染色体创建一个适应度值的列表。然后，找到最大适应度值及其在列表中的对应索引。最后，返回最大适应度值索引处的染色体。这个函数用于从染色体种群中找到最佳染色体，以便进行进一步的操作。

```py
def get_best(population):
	fitness_values = []
	for chromosome in population:
		fitness_values.append(calculate_fitness(chromosome))

	max_value = max(fitness_values)
	max_index = fitness_values.index(max_value)
	return population[max_index]
```

## 控制循环

这段代码正在执行一种进化算法，以进化一组染色体。它首先循环指定的代数次数。对于每一代，从种群中选择两个染色体，然后对它们进行交叉以生成两个新的染色体。接着，这两个新的染色体会以给定的概率进行突变。如果随机生成的概率高于预定的阈值，则新的染色体会经历随机的基因突变。最后，旧的种群会被新的种群替代，新种群由这两个新染色体和旧种群中的剩余染色体组成。

```py
for _ in range(generations):
	# select two chromosomes for crossover
	parent1, parent2 = select_chromosomes(population)

	# perform crossover to generate two new chromosomes
	child1, child2 = crossover(parent1, parent2)

	# perform mutation on the two new chromosomes
	if random.uniform(0, 1) < mutation_probability:
		child1 = mutate(child1)
	if random.uniform(0, 1) < mutation_probability:
		child2 = mutate(child2)

	# replace the old population with the new population
	population = [child1, child2] + population[2:]
```

# 完整的 Python 实现

如果我们将上述函数和控制循环，加上一个项目列表以及一些参数和控制台输出，我们将得到以下完整的 Python 实现。

请注意，所有参数都是为了简便而硬编码的；不过，只需稍加修改即可接受命令行参数或请求用户输入，包括可用项目的数量、价值和重量。

```py
import random

# function to generate a random population
def generate_population(size):
	population = []
	for _ in range(size):
		genes = [0, 1]
		chromosome = []
		for _ in range(len(items)):
			chromosome.append(random.choice(genes))
		population.append(chromosome)
	print("Generated a random population of size", size)
	return population

# function to calculate the fitness of a chromosome
def calculate_fitness(chromosome):
	total_weight = 0
	total_value = 0
	for i in range(len(chromosome)):
		if chromosome[i] == 1:
			total_weight += items[i][0]
			total_value += items[i][1]
	if total_weight > max_weight:
		return 0
	else:
		return total_value

# function to select two chromosomes for crossover
def select_chromosomes(population):
	fitness_values = []
	for chromosome in population:
		fitness_values.append(calculate_fitness(chromosome))

	fitness_values = [float(i)/sum(fitness_values) for i in fitness_values]

	parent1 = random.choices(population, weights=fitness_values, k=1)[0]
	parent2 = random.choices(population, weights=fitness_values, k=1)[0]

	print("Selected two chromosomes for crossover")
	return parent1, parent2

# function to perform crossover between two chromosomes
def crossover(parent1, parent2):
	crossover_point = random.randint(0, len(items)-1)
	child1 = parent1[0:crossover_point] + parent2[crossover_point:]
	child2 = parent2[0:crossover_point] + parent1[crossover_point:]

	print("Performed crossover between two chromosomes")
	return child1, child2

# function to perform mutation on a chromosome
def mutate(chromosome):
	mutation_point = random.randint(0, len(items)-1)
	if chromosome[mutation_point] == 0:
		chromosome[mutation_point] = 1
	else:
		chromosome[mutation_point] = 0
	print("Performed mutation on a chromosome")
	return chromosome

# function to get the best chromosome from the population
def get_best(population):
	fitness_values = []
	for chromosome in population:
		fitness_values.append(calculate_fitness(chromosome))

	max_value = max(fitness_values)
	max_index = fitness_values.index(max_value)
	return population[max_index]

# items that can be put in the knapsack
items = [
		[1, 2],
		[2, 4],
		[3, 4],
		[4, 5],
		[5, 7],
		[6, 9]
	]

# print available items
print("Available items:\n", items)

# parameters for genetic algorithm
max_weight = 10
population_size = 10
mutation_probability = 0.2
generations = 10

print("\nGenetic algorithm parameters:")
print("Max weight:", max_weight)
print("Population:", population_size)
print("Mutation probability:", mutation_probability)
print("Generations:", generations, "\n")
print("Performing genetic evolution:")

# generate a random population
population = generate_population(population_size)

# evolve the population for specified number of generations
for _ in range(generations):
	# select two chromosomes for crossover
	parent1, parent2 = select_chromosomes(population)

	# perform crossover to generate two new chromosomes
	child1, child2 = crossover(parent1, parent2)

	# perform mutation on the two new chromosomes
	if random.uniform(0, 1) < mutation_probability:
		child1 = mutate(child1)
	if random.uniform(0, 1) < mutation_probability:
		child2 = mutate(child2)

	# replace the old population with the new population
	population = [child1, child2] + population[2:]

# get the best chromosome from the population
best = get_best(population)

# get the weight and value of the best solution
total_weight = 0
total_value = 0
for i in range(len(best)):
	if best[i] == 1:
		total_weight += items[i][0]
		total_value += items[i][1]

# print the best solution
print("\nThe best solution:")
print("Weight:", total_weight)
print("Value:", total_value)
```

将上述代码保存到文件`knapsack_ga.py`中，然后通过输入`python knapsack_ga.py`来运行它。

一个示例输出如下：

```py
Available items:
[[1, 2], [2, 4], [3, 4], [4, 5], [5, 7], [6, 9]]

Genetic algorithm parameters:
Max weight: 10
Population: 10
Mutation probability: 0.2
Generations: 10 

Performing genetic evolution:
Generated a random population of size 10
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Performed mutation on a chromosome
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Performed mutation on a chromosome
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Performed mutation on a chromosome
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Selected two chromosomes for crossover
Performed crossover between two chromosomes
Performed mutation on a chromosome

The best solution:
Weight: 10
Value: 14
```

就这样，你现在知道如何使用遗传编程解决背包问题了。通过一点创造力，上述脚本可以被修改以解决各种计算复杂的问题，获得最佳的“足够好”的解决方案。

感谢阅读！

*本文的部分内容是在 GPT-3 的协助下进行规划和/或编写的。*

**[马修·梅奥](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家，也是 KDnuggets 的主编，这是一个开创性的在线数据科学和机器学习资源。他的兴趣包括自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。马修拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系到。

### 更多相关主题

+   [用 Python 理解和实现遗传算法](https://www.kdnuggets.com/understanding-and-implementing-genetic-algorithms-in-python)

+   [遗传算法关键术语解释](https://www.kdnuggets.com/2018/04/genetic-algorithm-key-terms-explained.html)

+   [使用遗传算法优化基因](https://www.kdnuggets.com/2022/04/optimizing-genes-genetic-algorithm.html)

+   [现实世界中 NLP 应用的范围：一种不同的…](https://www.kdnuggets.com/2022/03/different-solution-problem-range-nlp-applications-real-world.html)

+   [梯度消失问题：原因、后果及解决方案](https://www.kdnuggets.com/2022/02/vanishing-gradient-problem.html)

+   [今天 90%的代码是为了防止失败，这就是问题所在](https://www.kdnuggets.com/2022/07/90-today-code-written-prevent-failure-problem.html)
