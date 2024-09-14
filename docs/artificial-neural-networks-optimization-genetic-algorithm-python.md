# 使用遗传算法和 Python 优化人工神经网络

> 原文：[`www.kdnuggets.com/2019/03/artificial-neural-networks-optimization-genetic-algorithm-python.html/2`](https://www.kdnuggets.com/2019/03/artificial-neural-networks-optimization-genetic-algorithm-python.html/2)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

### 完整的 Python 实现

这个项目的 Python 实现包括三个 Python 文件：

1.  **GA.py** 用于实现 GA 函数。

1.  **ANN.py** 用于实现 ANN 函数。

1.  第三个文件用于通过多个代调用这些函数。这是项目的主要文件。

### 主项目文件实现

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

第三个文件是主要文件，因为它连接了所有函数。它读取特征和类标签文件，根据标准差过滤特征，创建 ANN 架构，生成初始解，通过计算所有解的适应度值、选择最佳父代、应用交叉和变异，最后创建新种群来循环多个代。其实现如下。该文件定义了 GA 参数，如每个种群的解的数量、选择的父代数量、变异百分比和代数。你可以尝试不同的值。

```py

import numpy

import GA

import pickle

import ANN

import matplotlib.pyplot

f = open("dataset_features.pkl", "rb")

data_inputs2 = pickle.load(f)

f.close()

features_STDs = numpy.std(a=data_inputs2, axis=0)

data_inputs = data_inputs2[:, features_STDs>50]

f = open("outputs.pkl", "rb")

data_outputs = pickle.load(f)

f.close()

#Genetic algorithm parameters:

#    Mating Pool Size (Number of Parents)

#    Population Size

#    Number of Generations

#    Mutation Percent

sol_per_pop = 8

num_parents_mating = 4

num_generations = 1000

mutation_percent = 10

#Creating the initial population.

initial_pop_weights = []

for curr_sol in numpy.arange(0, sol_per_pop):

    HL1_neurons = 150

    input_HL1_weights = numpy.random.uniform(low=-0.1, high=0.1,

                                             size=(data_inputs.shape[1], HL1_neurons))

    HL2_neurons = 60

    HL1_HL2_weights = numpy.random.uniform(low=-0.1, high=0.1,

                                             size=(HL1_neurons, HL2_neurons))

    output_neurons = 4

    HL2_output_weights = numpy.random.uniform(low=-0.1, high=0.1,

                                              size=(HL2_neurons, output_neurons))

    initial_pop_weights.append(numpy.array([input_HL1_weights,

                                                HL1_HL2_weights,

                                                HL2_output_weights]))

pop_weights_mat = numpy.array(initial_pop_weights)

pop_weights_vector = GA.mat_to_vector(pop_weights_mat)

best_outputs = []

accuracies = numpy.empty(shape=(num_generations))

for generation in range(num_generations):

    print("Generation : ", generation)

    # converting the solutions from being vectors to matrices.

    pop_weights_mat = GA.vector_to_mat(pop_weights_vector,

                                       pop_weights_mat)

    # Measuring the fitness of each chromosome in the population.

    fitness = ANN.fitness(pop_weights_mat,

                          data_inputs,

                          data_outputs,

                          activation="sigmoid")

    accuracies[generation] = fitness[0]

    print("Fitness")

    print(fitness)

    # Selecting the best parents in the population for mating.

    parents = GA.select_mating_pool(pop_weights_vector,

                                    fitness.copy(),

                                    num_parents_mating)

    print("Parents")

    print(parents)

    # Generating next generation using crossover.

    offspring_crossover = GA.crossover(parents,

                                       offspring_size=(pop_weights_vector.shape[0]-parents.shape[0], pop_weights_vector.shape[1]))

    print("Crossover")

    print(offspring_crossover)

    # Adding some variations to the offsrping using mutation.

    offspring_mutation = GA.mutation(offspring_crossover,

                                     mutation_percent=mutation_percent)

    print("Mutation")

    print(offspring_mutation)

    # Creating the new population based on the parents and offspring.

    pop_weights_vector[0:parents.shape[0], :] = parents

    pop_weights_vector[parents.shape[0]:, :] = offspring_mutation

pop_weights_mat = GA.vector_to_mat(pop_weights_vector, pop_weights_mat)

best_weights = pop_weights_mat [0, :]

acc, predictions = ANN.predict_outputs(best_weights, data_inputs, data_outputs, activation="sigmoid")

print("Accuracy of the best solution is : ", acc)

matplotlib.pyplot.plot(accuracies, linewidth=5, color="black")

matplotlib.pyplot.xlabel("Iteration", fontsize=20)

matplotlib.pyplot.ylabel("Fitness", fontsize=20)

matplotlib.pyplot.xticks(numpy.arange(0, num_generations+1, 100), fontsize=15)

matplotlib.pyplot.yticks(numpy.arange(0, 101, 5), fontsize=15)

f = open("weights_"+str(num_generations)+"_iterations_"+str(mutation_percent)+"%_mutation.pkl", "wb")

pickle.dump(pop_weights_mat, f)

f.close()

```

基于 1,000 代，文件末尾使用**Matplotlib**可视化库创建了一个图表，显示每代准确率的变化。它显示在下一个图中。

![遗传算法准确性](img/64fe9af88fc4f028af9430b41dc84444.png)

经过 1,000 次迭代后，准确率超过 97%。相比之下，如果不使用优化技术，则准确率为 45%。这说明结果可能不佳并非因为模型或数据有问题，而是因为没有使用优化技术。当然，使用不同的参数值，例如 10,000 代，可能会提高准确率。在该文件的末尾，它将参数以矩阵形式保存到磁盘以备后用。

### GA.py 实现

**GA.py** 文件的实现如下所示。请注意，**mutation()** 函数接受**mutation_percent** 参数，定义了要随机更改其值的基因数量。主文件中设置为 10%。该文件还包含两个新函数 **mat_to_vector()** 和 **vector_to_mat()**。

```py

import numpy

import random

# Converting each solution from matrix to vector.

def mat_to_vector(mat_pop_weights):

    pop_weights_vector = []

    for sol_idx in range(mat_pop_weights.shape[0]):

        curr_vector = []

        for layer_idx in range(mat_pop_weights.shape[1]):

            vector_weights = numpy.reshape(mat_pop_weights[sol_idx, layer_idx], newshape=(mat_pop_weights[sol_idx, layer_idx].size))

            curr_vector.extend(vector_weights)

        pop_weights_vector.append(curr_vector)

    return numpy.array(pop_weights_vector)

# Converting each solution from vector to matrix.

def vector_to_mat(vector_pop_weights, mat_pop_weights):

    mat_weights = []

    for sol_idx in range(mat_pop_weights.shape[0]):

        start = 0

        end = 0

        for layer_idx in range(mat_pop_weights.shape[1]):

            end = end + mat_pop_weights[sol_idx, layer_idx].size

            curr_vector = vector_pop_weights[sol_idx, start:end]

            mat_layer_weights = numpy.reshape(curr_vector, newshape=(mat_pop_weights[sol_idx, layer_idx].shape))

            mat_weights.append(mat_layer_weights)

            start = end

    return numpy.reshape(mat_weights, newshape=mat_pop_weights.shape)

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

    return offspring

def mutation(offspring_crossover, mutation_percent):

    num_mutations = numpy.uint8((mutation_percent*offspring_crossover.shape[1])/100)

    mutation_indices = numpy.array(random.sample(range(0, offspring_crossover.shape[1]), num_mutations))

    # Mutation changes a single gene in each offspring randomly.

    for idx in range(offspring_crossover.shape[0]):

        # The random value to be added to the gene.

        random_value = numpy.random.uniform(-1.0, 1.0, 1)

        offspring_crossover[idx, mutation_indices] = offspring_crossover[idx, mutation_indices] + random_value

    return offspring_crossover

```

### ANN.py 实现

最终，**ANN.py** 根据以下代码实现。它包含激活函数（sigmoid 和 ReLU）的实现，以及**fitness()** 和 **predict_outputs()** 函数以计算准确性。

```py

import numpy

def sigmoid(inpt):

    return 1.0 / (1.0 + numpy.exp(-1 * inpt))

def relu(inpt):

    result = inpt

    result[inpt < 0] = 0

    return result

def predict_outputs(weights_mat, data_inputs, data_outputs, activation="relu"):

    predictions = numpy.zeros(shape=(data_inputs.shape[0]))

    for sample_idx in range(data_inputs.shape[0]):

        r1 = data_inputs[sample_idx, :]

        for curr_weights in weights_mat:

            r1 = numpy.matmul(a=r1, b=curr_weights)

            if activation == "relu":

                r1 = relu(r1)

            elif activation == "sigmoid":

                r1 = sigmoid(r1)

        predicted_label = numpy.where(r1 == numpy.max(r1))[0][0]

        predictions[sample_idx] = predicted_label

    correct_predictions = numpy.where(predictions == data_outputs)[0].size

    accuracy = (correct_predictions / data_outputs.size) * 100

    return accuracy, predictions

def fitness(weights_mat, data_inputs, data_outputs, activation="relu"):

    accuracy = numpy.empty(shape=(weights_mat.shape[0]))

    for sol_idx in range(weights_mat.shape[0]):

        curr_sol_mat = weights_mat[sol_idx, :]

        accuracy[sol_idx], _ = predict_outputs(curr_sol_mat, data_inputs, data_outputs, activation=activation)

    return accuracy

```

### 联系作者

+   电子邮件: [ahmed.f.gad@gmail.com](https://mailto:ahmed.f.gad@gmail.com)

+   LinkedIn: [`linkedin.com/in/ahmedfgad/`](https://linkedin.com/in/ahmedfgad/)

+   KDnuggets: [`www.kdnuggets.com/author/ahmed-gad`](https://www.kdnuggets.com/author/ahmed-gad)

+   YouTube: [`youtube.com/AhmedGadFCIT`](https://youtube.com/AhmedGadFCIT)

+   TowardsDataScience: [`towardsdatascience.com/@ahmedfgad`](https://towardsdatascience.com/@ahmedfgad)

+   GitHub: [`github.com/ahmedfgad`](https://github.com/ahmedfgad)

[原文](https://www.linkedin.com/pulse/artificial-neural-networks-optimization-using-genetic-ahmed-gad/)。经许可转载。

**个人简介: [Ahmed Gad](https://www.linkedin.com/in/ahmedfgad/)** 于 2015 年 7 月获得埃及梅努非亚大学计算机与信息学院（FCI）信息技术优秀荣誉学士学位。因在学院中排名第一，他于 2015 年被推荐到埃及某学院担任助教，并于 2016 年在其学院担任助教及研究员。他目前的研究兴趣包括深度学习、机器学习、人工智能、数字信号处理和计算机视觉。

**相关：**

+   使用 NumPy 和图像分类的人工神经网络实现

+   Python 中的遗传算法实现

+   学习率在人工神经网络中是否有用？

### 更多相关内容

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [建立一个坚实的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三款 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [成为一名优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)
dnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [是什么让 Python 成为初创企业的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [成为伟大的数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)
