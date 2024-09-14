# 使用遗传算法和Python进行特征减少

> 原文：[https://www.kdnuggets.com/2019/03/feature-reduction-genetic-algorithm-python.html](https://www.kdnuggets.com/2019/03/feature-reduction-genetic-algorithm-python.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](/2019/03/feature-reduction-genetic-algorithm-python.html?page=2#comments)![实用计算机视觉应用深度学习](../Images/1bf50c2f23d12814ce8634a5b1cee1e1.png)

### 介绍

在某些情况下，使用原始数据训练机器学习算法可能不是最佳选择。当算法使用原始数据进行训练时，它必须自行进行特征挖掘，以检测不同的群体。但这需要大量的数据才能自动进行特征挖掘。对于小型数据集，建议数据科学家自行进行特征挖掘，并仅告诉机器学习算法使用哪些特征集。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT工作

* * *

使用的特征集必须能代表数据样本，因此我们必须仔细选择最佳特征。数据科学家建议使用一些基于以往经验看来对表示数据样本有帮助的特征。一些特征可能在表示样本时证明了它们的鲁棒性，而其他特征则可能不行。

可能会有一些特征会影响训练模型的结果，要么通过降低分类问题的准确性，要么通过增加回归问题的误差。例如，特征向量中可能存在噪声元素，因此这些元素应被移除。特征向量中也可能包含两个或更多相关的元素，使用其中一个元素将代替其他元素。为了去除这些类型的元素，有两个有用的步骤，即**特征选择**和**特征减少**。本教程重点讲解**特征减少**。

假设有 3 个特征 F1、F2 和 F3，每个特征有 3 个特征元素。因此，特征向量的长度为 3x3=9。特征选择只选择特定类型的特征，并排除其他特征。例如，只选择 F1 和 F3 并去除 F3。此时，特征向量的长度从 9 减少为 6。在**特征缩减**中，可能会排除每个特征的特定元素。例如，这一步可能会去除 F3 的**第一个**和**第三个**元素，同时保留第二个元素。因此，特征向量的长度从 9 减少为 7。

在开始本教程之前，值得一提的是，它是我在 LinkedIn 个人资料中发布的 2 个之前教程的扩展。

第一个教程的标题是“**使用 NumPy 实现人工神经网络并对 Fruits360 图像数据集进行分类**”。它开始时从 Fruits360 数据集的 4 个类别中提取长度为 360 的特征向量。接着，使用 NumPy 从头构建一个人工神经网络（ANN）来对数据集进行分类。此教程可在此处找到：[https://www.linkedin.com/pulse/artificial-neural-network-implementation-using-numpy-fruits360-gad](https://www.linkedin.com/pulse/artificial-neural-network-implementation-using-numpy-fruits360-gad)。其 GitHub 项目可在此处找到：[https://github.com/ahmedfgad/NumPyANN](https://github.com/ahmedfgad/NumPyANN)。

第二个教程的标题是“**使用遗传算法优化人工神经网络**”。该教程构建并使用遗传算法来优化 ANN 参数，以提高分类准确率。此教程可在此处找到：[https://www.linkedin.com/pulse/artificial-neural-networks-optimization-using-genetic-ahmed-gad](https://www.linkedin.com/pulse/artificial-neural-networks-optimization-using-genetic-ahmed-gad)。其 GitHub 项目也可以在此处找到：[https://github.com/ahmedfgad/NeuralGenetic](https://github.com/ahmedfgad/NeuralGenetic)。

本教程讨论了如何使用遗传算法（GA）来缩减从 Fruits360 数据集中提取的长度为 360 的特征向量。本教程首先讨论需要遵循的步骤。然后，使用 Python 实现这些步骤，主要利用 NumPy 和 Sklearn。

本教程的实现可以在我的 GitHub 页面找到，链接如下：[https://github.com/ahmedfgad/FeatureReductionGenetic](https://github.com/ahmedfgad/FeatureReductionGenetic)。

遗传算法从一个初始种群开始，该种群由多个染色体（即解）组成，每个染色体都有一系列基因。通过适应度函数，遗传算法选择最佳解作为父代，用于创建新种群。新种群中的新解通过对父代应用两种操作来生成：交叉和变异。在将遗传算法应用于特定问题时，我们需要确定基因的表示、合适的适应度函数以及如何应用交叉和变异。让我们看看这些是如何工作的。

### 关于遗传算法的更多信息

你可以从我准备的以下资源中了解更多关于遗传算法的信息：

+   遗传算法优化简介

    +   [https://www.linkedin.com/pulse/introduction-optimization-genetic-algorithm-ahmed-gad/](https://www.linkedin.com/pulse/introduction-optimization-genetic-algorithm-ahmed-gad/)

    +   [/2018/03/introduction-optimization-with-genetic-algorithm.html](/2018/03/introduction-optimization-with-genetic-algorithm.html)

    +   [https://towardsdatascience.com/introduction-to-optimization-with-genetic-algorithm-2f5001d9964b](https://towardsdatascience.com/introduction-to-optimization-with-genetic-algorithm-2f5001d9964b)

+   遗传算法（GA）优化 - 步骤示例

    +   [https://www.slideshare.net/AhmedGadFCIT/genetic-algorithm-ga-optimization-stepbystep-example](https://www.slideshare.net/AhmedGadFCIT/genetic-algorithm-ga-optimization-stepbystep-example)

+   遗传算法在Python中的实现

    +   [https://www.linkedin.com/pulse/genetic-algorithm-implementation-python-ahmed-gad/](https://www.linkedin.com/pulse/genetic-algorithm-implementation-python-ahmed-gad/)

    +   [/2018/07/genetic-algorithm-implementation-python.html](/2018/07/genetic-algorithm-implementation-python.html)

    +   [https://towardsdatascience.com/genetic-algorithm-implementation-in-python-5ab67bb124a6](https://towardsdatascience.com/genetic-algorithm-implementation-in-python-5ab67bb124a6)

    +   [https://github.com/ahmedfgad/GeneticAlgorithmPython](https://github.com/ahmedfgad/GeneticAlgorithmPython)

我在2018年还写了一本书，其中一个章节讲述了遗传算法。这本书的标题是**《使用深度学习和卷积神经网络的实际计算机视觉应用》**，可以在这里的Springer上找到[https://www.springer.com/us/book/9781484241660](https://www.springer.com/us/book/9781484241660)。

### 染色体表示

遗传算法中的基因是染色体的构建块。首先，我们需要确定染色体中包含哪些基因。为此，考虑到每个可能影响结果的属性应视为一个基因。由于我们的任务目标是选择最佳的特征元素集合，因此每个特征元素可能会影响结果，因此每个特征元素都被视为一个基因。染色体将由所有基因（即所有特征元素）组成。因为有360个特征元素，所以将有360个基因。现在清楚的一点是染色体的长度是360。

确定了所选基因之后，接下来是确定基因的表示方式。有不同的表示方法，如十进制、二进制、浮点、字符串等。我们的目标是知道基因（即特征元素）是否在减少的特征集合中被选择。因此，分配给基因的值应反映其是否被选择。根据这个描述，很明显每个基因有2个可能的值。一个值表示基因被选择，另一个值表示基因未被选择。因此，二进制表示是最佳选择。当基因值为1时，它将被选择到减少的特征集合中。当值为0时，则会被忽略。

总结一下，染色体将由360个以二进制表示的基因组成。根据下图，特征向量与染色体之间存在一对一的映射关系。即染色体中的第一个基因与特征向量中的第一个元素相关联。当该基因的值为1时，这意味着特征向量中的第一个元素被选择。

![染色体表示](../Images/db95ac7af78596be8d5db825557827dc.png)

### 适应度函数

通过了解如何创建染色体，可以轻松地使用NumPy随机初始化初始种群。初始化后，选择父代。遗传算法基于**达尔文**的理论“**适者生存**”。即从当前种群中选择最佳解进行配对，以产生更好的解。通过保留好的解并淘汰坏的解，我们可以达到最优或次优解。

选择父代的标准是与每个解（即染色体）相关的适应度值。适应度值越高，解越好。适应度值是通过适应度函数计算得出的。那么，在我们的任务中，最佳的函数是什么呢？我们任务的目标是创建一个减少的特征向量，以提高分类准确率。因此，判断一个解是否好的标准是**分类准确率**。因此，适应度函数将返回一个指定每个解的分类准确率的数字。准确率越高，解越好。

为了返回分类准确率，必须有一个机器学习模型通过每个解返回的特征元素进行训练。我们将使用支持向量分类器（SVC）。

数据集被分为训练样本和测试样本。根据训练数据，SVC 将使用每个解在种群中选择的特征元素进行训练。训练完成后，将根据测试数据进行测试。

根据每个解的适应度值，我们可以选择其中最好的作为父代。这些父代被放在配对池中，以生成将成为下一代新种群成员的后代。这些后代是通过对选择的父代应用交叉和突变操作来创建的。接下来，我们将配置这些操作。

### 交叉和突变

根据适应度函数，我们可以在当前种群中筛选出最佳的解，这些解被称为父代。遗传算法假设将两个好的解配对会产生第三个更好的解。配对意味着从两个父代中交换一些基因。基因通过交叉操作进行交换。此操作可以通过不同方式应用。本教程使用单点交叉，其中一个点将染色体分开。点之前的基因来自一个解，点之后的基因来自另一个解。

仅仅通过应用交叉操作，所有基因都来自先前的父代。新的后代中没有引入新的基因。如果所有父代中存在坏基因，那么这个基因将被传递给后代。因此，应用突变操作以引入新的基因。对于基因的二进制表示，突变是通过翻转一些随机选择的基因的值来实现的。如果基因值是 1，则将其变为 0，反之亦然。

在生成后代后，我们可以创建下一代的新种群。这个种群包括了之前的父代以及后代。

目前，所有步骤都已讨论完毕。接下来是用 Python 实现这些步骤。请注意，我之前写了一篇标题为**《Python 中的遗传算法实现》**的教程，用于在 Python 中实现遗传算法，我将修改它的代码以适应我们的问题。最好先阅读一下。

### Python 实现

项目组织成两个文件。一个文件名为“GA.py”，其中包含遗传算法步骤的实现函数。另一个文件是主要文件，它只导入这个文件并在一个循环中调用其函数，该循环遍历各代。

主文件开始时会读取从Fruits360数据集中提取的特征，如下代码所示。这些特征被返回到**data_inputs**变量中。关于如何提取这些特征的详细信息，可以在教程开始时提到的两个教程中找到。文件还读取了与样本关联的类别标签，存储在**data_outputs**变量中。

一些样本被选为训练，其索引存储在**train_indices**变量中。类似地，测试样本的索引存储在**test_indices**变量中。

```py

import numpy

import GA

import pickle

import matplotlib.pyplot

f = open("dataset_features.pkl", "rb")

data_inputs = pickle.load(f)

f.close()

f = open("outputs.pkl", "rb")

data_outputs = pickle.load(f)

f.close()

num_samples = data_inputs.shape[0]

num_feature_elements = data_inputs.shape[1]

train_indices = numpy.arange(1, num_samples, 4)

test_indices = numpy.arange(0, num_samples, 4)

print("Number of training samples: ", train_indices.shape[0])

print("Number of test samples: ", test_indices.shape[0])

"""

Genetic algorithm parameters:

    Population size

    Mating pool size

    Number of mutations
"""

sol_per_pop = 8 # Population size.

num_parents_mating = 4 # Number of parents inside the mating pool.

num_mutations = 3 # Number of elements to mutate.

# Defining the population shape.
pop_shape = (sol_per_pop, num_feature_elements)

# Creating the initial population.
new_population = numpy.random.randint(low=0, high=2, size=pop_shape)
print(new_population.shape)

best_outputs = []
num_generations = 100

```

它初始化了遗传算法的所有参数。这包括每代种群中的解数量，根据**sol_per_pop**变量设置为8，后代数量，根据**num_parents_mating**变量设置为4，以及突变数量，根据**num_mutations**变量设置为3。之后，它在一个名为**new_population**的变量中随机创建初始种群。

有一个名为**best_outputs**的空列表，用于存储每次生成后的最佳结果。这有助于在完成所有代之后可视化遗传算法的进展。**num_generations**变量设置了代的数量为100。请注意，你可以更改这些参数，这可能会带来更好的结果。

### 了解更多此主题

+   [遗传算法关键术语解释](https://www.kdnuggets.com/2018/04/genetic-algorithm-key-terms-explained.html)

+   [使用遗传算法优化基因](https://www.kdnuggets.com/2022/04/optimizing-genes-genetic-algorithm.html)

+   [数据科学中的降维技术](https://www.kdnuggets.com/2022/09/dimensionality-reduction-techniques-data-science.html)

+   [Feature Store Summit 2022: 一场关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [Python中的遗传编程：背包问题](https://www.kdnuggets.com/2023/01/knapsack-problem-genetic-programming-python.html)

+   [理解和实现Python中的遗传算法](https://www.kdnuggets.com/understanding-and-implementing-genetic-algorithms-in-python)
