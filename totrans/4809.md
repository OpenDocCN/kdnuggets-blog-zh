# 遗传算法在 Python 中的实现

> 原文：[https://www.kdnuggets.com/2018/07/genetic-algorithm-implementation-python.html](https://www.kdnuggets.com/2018/07/genetic-algorithm-implementation-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

![图片](../Images/ab1f8e899e0f7dc04c652ffc0278bfbc.png)

本教程将基于一个简单的例子实现遗传算法优化技术，我们的目标是最大化一个方程的输出。教程使用十进制表示基因，单点交叉和均匀变异。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 相关工作

* * *

遗传算法（GA）的流程图如图 1 所示。GA 的每一步都有一些变体。

**图 1**

![](../Images/f7a61e594c8da119374bac0446628488.png)

例如，基因有不同类型的表示方式，如二进制、十进制、整数等。每种类型的处理方式不同。变异也有不同类型，如位翻转、交换、逆转、均匀、非均匀、高斯、缩小等。交叉操作也有不同的类型，如混合、单点、双点、均匀等。本教程不会实现所有这些变异和交叉类型，而是仅实现每一步中一种类型。教程使用了十进制表示基因，单点交叉和均匀变异。读者应对 GA 的工作原理有所了解。如果不了解，请阅读标题为“遗传算法优化简介”的文章，链接如下：

+   LinkedIn: [https://www.linkedin.com/pulse/introduction-optimization-genetic-algorithm-ahmed-gad/](https://www.linkedin.com/pulse/introduction-optimization-genetic-algorithm-ahmed-gad/)

+   KDnuggets: [https://www.kdnuggets.com/2018/03/introduction-optimization-with-genetic-algorithm.html](/2018/03/introduction-optimization-with-genetic-algorithm.html)

+   TowardsDataScience: [https://towardsdatascience.com/introduction-to-optimization-with-genetic-algorithm-2f5001d9964b](https://towardsdatascience.com/introduction-to-optimization-with-genetic-algorithm-2f5001d9964b)

+   SlideShare: [https://www.slideshare.net/AhmedGadFCIT/introduction-to-optimization-with-genetic-algorithm-ga](https://www.slideshare.net/AhmedGadFCIT/introduction-to-optimization-with-genetic-algorithm-ga)

教程开始时展示了我们将要实现的方程。方程如下所示：

**Y = w1x1 + w2x2 + w3x3 + w4x4 + w5x5 + w6x6**

方程有6个输入（x1到x6）和6个权重（w1到w6），输入值为（x1,x2,x3,x4,x5,x6）=(4,-2,7,5,11,1)。我们希望找到最大化该方程的参数（权重）。最大化该方程的想法似乎很简单。正输入应乘以尽可能大的正数，负数应乘以尽可能小的负数。但我们要实现的想法是如何让GA自己做到这一点，以便知道正权重与正输入配合使用，负权重与负输入配合使用。让我们开始实现GA。

首先，让我们创建一个包含6个输入的列表和一个保存权重数量的变量，如下所示：

```py

# Inputs of the equation.
equation_inputs = [4,-2,3.5,5,-11,-4.7]
# Number of the weights we are looking to optimize.
num_weights = 6

```

下一步是定义初始种群。根据权重的数量，种群中的每个染色体（解决方案或个体）将必定具有6个基因，每个基因对应一个权重。但问题是每个种群中有多少个解决方案？这个值没有固定的，我们可以选择适合我们问题的值。为了通用，我们可以在代码中保持它是可变的。接下来，我们创建一个变量来保存每个种群的解决方案数量，另一个变量保存种群的大小，最后一个变量保存实际的初始种群：

```py

import numpy

sol_per_pop = 8
# Defining the population size.
pop_size = (sol_per_pop,num_weights) # The population will have sol_per_pop chromosome where each chromosome has num_weights genes.
#Creating the initial population.
new_population = numpy.ram.uniform(low=-4.0, high=4.0, size=pop_size)
```

在导入numpy库后，我们可以使用numpy.random.uniform函数随机创建初始种群。根据选择的参数，它将具有形状（8, 6）。即8个染色体，每个染色体有6个基因，每个基因对应一个权重。运行此代码后，种群如下：

```py

[[-2.19134006 -2.88907857  2.02365737 -3.97346034  3.45160502  2.05773249]

 [ 2.12480298  2.97122243  3.60375452  3.78571392  0.28776565  3.5170347 ]

 [ 1.81098962  0.35130155  1.03049548 -0.33163294  3.52586421  2.53845644]

 [-0.63698911 -2.8638447   2.93392615 -1.40103767 -1.20313655  0.30567304]

 [-1.48998583 -1.53845766  1.11905299 -3.67541087  1.33225142  2.86073836]

 [ 1.14159503  2.88160332  1.74877772 -3.45854293  0.96125878  2.99178241]

 [ 1.96561297  0.51030292  0.52852716 -1.56909315 -2.35855588  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -0.72163167  0.7516408   0.00677938]]

```

请注意，它是随机生成的，因此每次运行时都会有所变化。

在准备好种群后，接下来是遵循图1中的流程图。根据适应度函数，我们将选择当前种群中最优秀的个体作为配对的父母。接下来，应用GA变体（交叉和变异）来产生下一代的后代，通过附加父母和后代来创建新种群，并重复这些步骤若干次迭代/代。下一段代码应用了这些步骤：

```py

import GA
num_generations = 5

num_parents_mating = 4
for generation in range(num_generations):
    # Measuring the fitness of each chromosome in the population.
    fitness = GA.cal_pop_fitness(equation_inputs, new_population)

    # Selecting the best parents in the population for mating.
    parents = GA.select_mating_pool(new_population, fitness, 
                                      num_parents_mating)

    # Generating next generation using crossover.
    offspring_crossover = GA.crossover(parents,
                                       offspring_size=(pop_size[0]-parents.shape[0], num_weights))

    # Adding some variations to the offsrping using mutation.
    offspring_mutation = GA.mutation(offspring_crossover)

    # Creating the new population based on the parents and offspring.
    new_population[0:parents.shape[0], :] = parents
    new_population[parents.shape[0]:, :] = offspring_mutation
```

当前的代数是5。为了在教程中展示所有代的结果，选择了较小的代数。有一个名为GA的模块，其中包含算法的实现。

第一步是使用GA.cal_pop_fitness函数找到种群中每个解决方案的适应度值。GA模块中该函数的实现如下：

```py

def cal_pop_fitness(equation_inputs, pop):
    # Calculating the fitness value of each solution in the current population.
    # The fitness function calculates the sum of products between each input and its corresponding weight.
    fitness = numpy.sum(pop*equation_inputs, axis=1)
    return fitness
```

适应度函数接受方程输入值（x1到x6）以及种群。适应度值计算为每个输入与其对应基因（权重）之间的乘积和（SOP）。根据每个种群中的解数量，将会有若干SOP。由于我们之前在名为sol_per_pop的变量中将解的数量设置为8，将会有8个SOP，如下所示：

```py

[-63.41070188  14.40299221 -42.22532674  18.24112489 -45.44363278 -37.00404311  15.99527402  17.0688537 ]
```

请注意，适应度值越高，解越好。

在计算所有解的适应度值后，接下来是根据下一个函数`GA.select_mating_pool`选择最佳的解作为交配池中的父母。该函数接受种群、适应度值和所需父母的数量。它返回选择的父母。其在GA模块中的实现如下：

```py

def select_mating_pool(pop, fitness, num_parents):
    # Selecting the best individuals in the current generation as parents for producing the offspring of the next generation.
    parents = numpy.empty((num_parents, pop.shape[1]))
    for parent_num in range(num_parents):
        max_fitness_idx = numpy.where(fitness == numpy.max(fitness))
        max_fitness_idx = max_fitness_idx[0][0]
        parents[parent_num, :] = pop[max_fitness_idx, :]
        fitness[max_fitness_idx] = -99999999999
    return parents
```

根据变量num_parents_mating中定义的所需父母数量，该函数创建一个空数组来保存它们，如下行所示：

```py

parents = numpy.empty((num_parents, pop.shape[1]))
```

在当前种群中循环，函数获取最高适应度值的索引，因为这是根据这一行选择的最佳解：

```py

max_fitness_idx = numpy.where(fitness == numpy.max(fitness))
```

这个索引用于使用这一行检索与该适应度值对应的解：

```py

parents[parent_num, :] = pop[max_fitness_idx, :]
```

为了避免再次选择这样的解，其适应度值被设置为一个非常小的值，**-99999999999**，以避免再次被选择。最终返回的是**parents**数组，根据我们的例子，它将如下所示：

```py

[[-0.63698911 -2.8638447   2.93392615 -1.40103767 -1.20313655  0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -0.72163167  0.7516408   0.00677938]

 [ 1.96561297  0.51030292  0.52852716 -1.56909315 -2.35855588  2.29682254]

 [ 2.12480298  2.97122243  3.60375452  3.78571392  0.28776565  3.5170347 ]]
```

请注意，这三个父母是在当前种群中适应度值最高的个体，分别为18.24112489、17.0688537、15.99527402和14.40299221。

下一步是使用这些选定的父母进行交配，以生成后代。交配从交叉操作开始，根据`GA.crossover`函数进行。该函数接受父母和后代大小。它使用后代大小来确定从这些父母那里产生的后代数量。该函数在GA模块中的实现如下：

```py

def crossover(parents, offspring_size):
    offspring = numpy.empty(offspring_size)
    # The point at which crossover takes place between two parents. Usually, it is at the center.
    crossover_point = numpy.uint8(offspring_size[1]/2)

    for k in range(offspring_size[0]):
        # Index of the first parent to mate.
        parent1_idx = k%parents.shape[0]
        # Index of the second parent to mate.
        parent2_idx = (k+1)%parents.shape[0]
        # The new offspring will have its first half of its genes taken from the first parent.
        offspring[k, 0:crossover_point] = parents[parent1_idx, 0:crossover_point]
        # The new offspring will have its second half of its genes taken from the second parent.
        offspring[k, crossover_point:] = parents[parent2_idx, crossover_point:]
```

这个函数首先根据后代大小创建一个空数组，如下行所示：

```py

offspring = numpy.empty(offspring_size)
```

由于我们使用单点交叉，我们需要指定交叉发生的点。该点被选定以将解分成两个相等的部分，根据这一行：

```py

crossover_point = numpy.uint8(offspring_size[1]/2)
```

然后我们需要选择两个父母进行交叉。这两个父母的索引根据这两行进行选择：

```py

parent1_idx = k%parents.shape[0]
parent2_idx = (k+1)%parents.shape[0]
```

父代的选择方式类似于环状。首先选择索引为 0 和 1 的父代产生两个后代。如果仍需产生更多后代，则选择父代 1 和父代 2 生成另外两个后代。如果需要更多的后代，则选择索引为 2 和 3 的父代。到索引 3 时，我们达到了最后一个父代。如果需要产生更多的后代，则选择索引为 3 的父代，并返回到索引为 0 的父代，以此类推。

对父代应用交叉操作后的解决方案存储在后代变量中，它们如下：

```py

[[-0.63698911 -2.8638447   2.93392615 -0.72163167  0.7516408   0.00677938]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.35855588  2.29682254]

 [ 1.96561297  0.51030292  0.52852716  3.78571392  0.28776565  3.5170347 ]

 [ 2.12480298  2.97122243  3.60375452 -1.40103767 -1.20313655  0.30567304]]
```

接下来是将第二种遗传算法变体——变异，应用到存储在后代变量中的交叉结果中，使用遗传算法模块中的变异函数。该函数接受交叉后的后代，并在应用均匀变异后返回。该函数的实现如下：

```py

def mutation(offspring_crossover):
    # Mutation changes a single gene in each offspring randomly.
    for idx in range(offspring_crossover.shape[0]):
        # The random value to be added to the gene.
        random_value = numpy.random.uniform(-1.0, 1.0, 1)
        offspring_crossover[idx, 4] = offspring_crossover[idx, 4] + random_value
```

它遍历每个后代，并根据这一行将一个均匀生成的随机数添加到范围 -1 到 1 之间：

```py

random_value = numpy.random.uniform(-1.0, 1.0, 1)
```

然后将随机数添加到后代的索引 4 的基因中，依据这一行：

```py

offspring_crossover[idx, 4] = offspring_crossover[idx, 4] + random_value
```

请注意，索引可以更改为任何其他索引。应用变异后的后代如下：

```py

[[-0.63698911 -2.8638447   2.93392615 -0.72163167  1.66083721  0.00677938]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.94513681  2.29682254]

 [ 1.96561297  0.51030292  0.52852716  3.78571392  0.45337472  3.5170347 ]

 [ 2.12480298  2.97122243  3.60375452 -1.40103767 -1.5781162   0.30567304]]
```

这些结果被添加到变量 offspring_crossover 中，并由函数返回。

到目前为止，我们成功地从 4 个选择的父代中产生了 4 个后代，我们准备创建下一代的新种群。

请注意，遗传算法是一种基于随机的优化技术。它通过对当前解决方案应用一些随机变化来尝试改进它们。由于这些变化是随机的，我们不能确定它们会产生更好的解决方案。因此，建议在新种群中保留之前的最佳解决方案（父代）。在最坏的情况下，如果所有新的后代都不如这些父代，我们将继续使用这些父代。结果是，我们可以保证新一代至少保留之前的好结果，而不会变得更差。新种群将从之前的父代中获得前 4 个解决方案。最后 4 个解决方案来自应用交叉和变异后的后代：

```py

new_population[0:parents.shape[0], :] = parents

new_population[parents.shape[0]:, :] = offspring_mutation
```

通过计算第一代所有解决方案（父代和后代）的适应度，其适应度如下：

```py

[ 18.24112489  17.0688537   15.99527402  14.40299221  -8.46075629  31.73289712   6.10307563  24.08733441]
```

之前的最高适应度是**18.24112489**，但现在是**31.7328971158**。这意味着随机变化朝着更好的解决方案移动。这是很棒的。但通过经过更多的代数，这些结果可以得到进一步的提升。以下是另外 4 代每一步的结果：

```py

Generation :  1

Fitness values:

[ 18.24112489  17.0688537   15.99527402  14.40299221  -8.46075629  31.73289712   6.10307563  24.08733441]

Selected parents:

[[ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.94513681  2.29682254]

 [ 2.12480298  2.97122243  3.60375452 -1.40103767 -1.5781162   0.30567304]

 [-0.63698911 -2.8638447   2.93392615 -1.40103767 -1.20313655  0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -0.72163167  0.7516408   0.00677938]]

Crossover result:

[[ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.5781162   0.30567304]

 [ 2.12480298  2.97122243  3.60375452 -1.40103767 -1.20313655  0.30567304]

 [-0.63698911 -2.8638447   2.93392615 -0.72163167  0.7516408   0.00677938]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.94513681  2.29682254]]

Mutation result:

[[ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.2392086   0.30567304]

 [ 2.12480298  2.97122243  3.60375452 -1.40103767 -0.38610586  0.30567304]

 [-0.63698911 -2.8638447   2.93392615 -0.72163167  1.33639943  0.00677938]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.13941727  2.29682254]]

Best result after generation 1 :  34.1663669207

Generation :  2

Fitness values:

[ 31.73289712  24.08733441  18.24112489  17.0688537   34.16636692  10.97522073  -4.89194068  22.86998223]

Selected Parents:

[[ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.2392086   0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.94513681  2.29682254]

 [ 2.12480298  2.97122243  3.60375452 -1.40103767 -1.5781162   0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.13941727  2.29682254]]

Crossover result:

[[ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.94513681  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.5781162   0.30567304]

 [ 2.12480298  2.97122243  3.60375452 -1.56909315 -1.13941727  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.2392086   0.30567304]]

Mutation result:

[[ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.20515009  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -0.73543721  0.30567304]

 [ 2.12480298  2.97122243  3.60375452 -1.56909315 -0.50581509  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.20089639  0.30567304]]

Best result after generation 2:  34.5930432629

Generation :  3

Fitness values:

[ 34.16636692  31.73289712  24.08733441  22.86998223  34.59304326  28.6248816    2.09334217  33.7449326 ]

Selected parents:

[[ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.20515009  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.2392086   0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.20089639  0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.94513681  2.29682254]]

Crossover result:

[[ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.2392086   0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.20089639  0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -1.94513681  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.20515009  2.29682254]]

Mutation result:

[[ 3.00912373 -2.745417    3.27131287 -1.40103767 -2.20744102  0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.16589294  0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.37553107  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.44124005  2.29682254]]

Best result after generation 3:  44.8169235189

Generation :  4

Fitness values

[ 34.59304326  34.16636692  33.7449326   31.73289712  44.81692352

  33.35989464  36.46723397  37.19003273]

Selected parents:

[[ 3.00912373 -2.745417    3.27131287 -1.40103767 -2.20744102  0.30567304]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.44124005  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.37553107  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.20515009  2.29682254]]

Crossover result:

[[ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.37553107  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.20515009  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -2.20744102  0.30567304]]

Mutation result:

[[ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.13382082  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.98105233  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.56909315 -2.27638584  2.29682254]

 [ 3.00912373 -2.745417    3.27131287 -1.40103767 -1.70558545  0.30567304]]

Best result after generation 4:  44.8169235189

```

在上述 5 代之后，最佳结果的适应度值现在为**44.8169235189**，相比之下，第一代后的最佳结果为**18.24112489**。

最佳解决方案具有以下权重：

```py

[3.00912373 -2.745417    3.27131287 -1.40103767 -2.20744102  0.30567304]
```

实现遗传算法的完整代码如下：

```py

import numpy
import GA

"""
The y=target is to maximize this equation ASAP:
    y = w1x1+w2x2+w3x3+w4x4+w5x5+6wx6
    where (x1,x2,x3,x4,x5,x6)=(4,-2,3.5,5,-11,-4.7)

    What are the best values for the 6 weights w1 to w6?
    We are going to use the genetic algorithm for the best possible values after a number of generations.
"""

# Inputs of the equation.
equation_inputs = [4,-2,3.5,5,-11,-4.7]

# Number of the weights we are looking to optimize.
num_weights = 6

"""
Genetic algorithm parameters:
    Mating pool size
    Population size
"""

sol_per_pop = 8
num_parents_mating = 4

# Defining the population size.
pop_size = (sol_per_pop,num_weights) # The population will have sol_per_pop chromosome where each chromosome has num_weights genes.

#Creating the initial population.
new_population = numpy.random.uniform(low=-4.0, high=4.0, size=pop_size)
print(new_population)

num_generations = 5

for generation in range(num_generations):
    print("Generation : ", generation)

    # Measing the fitness of each chromosome in the population.
    fitness = GA.cal_pop_fitness(equation_inputs, new_population)

    # Selecting the best parents in the population for mating.
    parents = GA.select_mating_pool(new_population, fitness, 
                                      num_parents_mating)

    # Generating next generation using crossover.
    offspring_crossover = GA.crossover(parents,
                                       offspring_size=(pop_size[0]-parents.shape[0], num_weights))

    # Adding some variations to the offsrping using mutation.
    offspring_mutation = GA.mutation(offspring_crossover)

    # Creating the new population based on the parents and offspring.
    new_population[0:parents.shape[0], :] = parents
    new_population[parents.shape[0]:, :] = offspring_mutation

    # The best result in the current iteration.
    print("Best result : ", numpy.max(numpy.sum(new_population*equation_inputs, axis=1)))

# Getting the best solution after iterating finishing all generations.
#At first, the fitness is calculated for each solution in the final generation.
fitness = GA.cal_pop_fitness(equation_inputs, new_population)

# Then return the index of that solution corresponding to the best fitness.
best_match_idx = numpy.where(fitness == numpy.max(fitness))

print("Best solution : ", new_population[best_match_idx, :])
print("Best solution fitness : ", fitness[best_match_idx])

```

遗传算法模块如下：

```py

import numpy

def cal_pop_fitness(equation_inputs, pop):
    # Calculating the fitness value of each solution in the current population.
    # The fitness function caulcuates the sum of products between each input and its corresponding weight.
    fitness = numpy.sum(pop*equation_inputs, axis=1)
    return fitness

def select_mating_pool(pop, fitness, num_parents):
    # Selecting the best individuals in the current generation as parents for producing the offspring of the next generation.
    parents = numpy.empty((num_parents, pop.shape[1]))
    for parent_num in range(num_parents):
        max_fitness_idx = numpy.where(fitness == numpy.max(fitness))
        max_fitness_idx = max_fitness_idx[0][0]
        parents[parent_num, :] = pop[max_fitness_idx, :]
        fitness[max_fitness_idx] = -99999999999
    return parents

def crossover(parents, offspring_size):
    offspring = numpy.empty(offspring_size)
    # The point at which crossover takes place between two parents. Usually it is at the center.
    crossover_point = numpy.uint8(offspring_size[1]/2)

    for k in range(offspring_size[0]):
        # Index of the first parent to mate.
        parent1_idx = k%parents.shape[0]
        # Index of the second parent to mate.
        parent2_idx = (k+1)%parents.shape[0]
        # The new offspring will have its first half of its genes taken from the first parent.
        offspring[k, 0:crossover_point] = parents[parent1_idx, 0:crossover_point]
        # The new offspring will have its second half of its genes taken from the second parent.
        offspring[k, crossover_point:] = parents[parent2_idx, crossover_point:]
    return offspring

def mutation(offspring_crossover):
    # Mutation changes a single gene in each offspring randomly.
    for idx in range(offspring_crossover.shape[0]):
        # The random value to be added to the gene.
        random_value = numpy.random.uniform(-1.0, 1.0, 1)
        offspring_crossover[idx, 4] = offspring_crossover[idx, 4] + random_value

```

**简历：[Ahmed Gad](https://www.linkedin.com/in/ahmedfgad/)** 于 2015 年 7 月获得埃及梅努菲亚大学计算机与信息学院（FCI）信息技术学士学位，并以优异成绩获得荣誉。因在学院中排名第一，他在 2015 年被推荐到埃及的一所学院担任助教，随后于 2016 年在他所在的学院担任助教和研究员。他目前的研究兴趣包括深度学习、机器学习、人工智能、数字信号处理和计算机视觉。

[原文](https://www.linkedin.com/pulse/genetic-algorithm-implementation-python-ahmed-gad/)。经授权转载。

**相关：**

+   [从完全连接网络逐步推导卷积神经网络](/2018/04/derivation-convolutional-neural-network-fully-connected-step-by-step.html)

+   [学习率在人工神经网络中是否有用？](/2018/01/learning-rate-useful-neural-network.html)

+   [通过正则化避免过拟合](/2018/02/avoid-overfitting-regularization.html)

### 更多相关话题

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么使 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，并通过找到目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [90 亿美元的人工智能失败，逐一检查](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功的数据科学家的五个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
