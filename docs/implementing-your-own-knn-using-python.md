# 实现你自己的 k-最近邻算法使用 Python

> 原文：[https://www.kdnuggets.com/2016/01/implementing-your-own-knn-using-python.html](https://www.kdnuggets.com/2016/01/implementing-your-own-knn-using-python.html)

**作者：Natasha Latysheva**。

*编辑注*：Natasha 活跃于 [剑桥编程学院](https://cambridgecoding.com/)，该学院将于 2016 年 2 月 20-21 日举办 [Python 数据科学训练营](https://cambridgecoding.com/datascience-bootcamp)，在这里你可以学习针对现实世界问题的最先进机器学习技术。

在机器学习中，你可能经常希望构建分类器，将事物分类到某些类别中，这些类别是基于一组相关值。例如，可以根据来自先前患者的数据为患者提供诊断。分类可能涉及在类别之间构造高度非线性的边界，如下图所示的红色、绿色和蓝色类别 [下面](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm)：

![kNN 边界](../Images/4322b5f9ae66be35375b65bec3b6c05f.png)

许多算法已经被开发用于自动分类，常见的算法包括随机森林、支持向量机、朴素贝叶斯分类器和多种类型的神经网络。为了了解分类的工作原理，我们以一个简单的分类算法——k-最近邻（kNN）为例，并在 Python 2 中从头开始构建它。如果你刚刚开始学习 Python，可以使用主要的 [命令式编程风格](http://latentflip.com/imperative-vs-declarative/)，而不是使用 [lambda 函数](http://www.secnetix.de/olli/Python/lambda_functions.hawk) 和 [列表推导式](http://www.secnetix.de/olli/Python/list_comprehensions.hawk) 的声明式/函数式风格，以保持简单。在这里，我们将介绍后一种方法。kNN 通过将新的实例与最相似的案例分组来进行分类。在这里，你将使用 kNN 处理流行的（尽管是理想化的）鸢尾花数据集，该数据集包括三种鸢尾花的花卉测量数据。我们的任务是根据花卉测量数据预测花卉的物种标签。由于你将基于一组已知的正确分类来构建预测器，因此 kNN 是一种监督式机器学习（虽然有些令人困惑的是，在 kNN 中没有显式的训练阶段；请参见 [懒惰学习](https://en.wikipedia.org/wiki/Lazy_learning)）。kNN 任务可以分解为编写 3 个主要功能：

1\. 计算任何两点之间的距离

2\. 基于这些成对距离找到最近邻

3\. 基于最近邻列表对类别标签进行多数投票

以下图中的步骤提供了你在代码中需要完成任务的高层次概述。

[![kNN 算法](../Images/39dbb98036b866c0b324bcb95cf9762c.png)](https://cambridgecoding.files.wordpress.com/2016/01/knn2.jpg)

**算法**

简而言之，你将构建一个脚本，对于每个需要分类的输入，搜索整个训练集中的 k 个最相似的实例。然后，通过多数投票总结最相似实例的类别标签，并将其作为测试案例的预测结果返回。

完整的代码在文章的最后。现在，让我们分别查看不同部分并解释它们的功能。

加载数据并拆分为训练集和测试集。为了快速上手，你将使用一些辅助函数：虽然我们可以自己[下载鸢尾花数据](https://archive.ics.uci.edu/ml/datasets/Iris)并使用[csv.reader](https://docs.python.org/2/library/csv.html)加载它，你也可以直接从 scikit-learn 快速获取鸢尾花数据。此外，你可以使用 train_test_split 函数进行 60/40 的训练/测试拆分，但你也可以自己随机分配行（请参见此类型的实现）。在机器学习中，训练/测试拆分用于减少过拟合——在完整数据集上训练模型往往会导致模型过拟合数据的噪声和特性，而不是实际的底层趋势。你只在训练集上进行任何类型的模型调整（例如，选择邻居的数量 k）——测试集作为一个独立的、未触及的数据集，用于测试最终模型的性能。

```py
from sklearn.datasets import load_iris
from sklearn import cross_validation
import numpy as np

# load dataset and partition in training and testing sets
iris = load_iris()
X_train, X_test, y_train, y_test = cross_validation.train_test_split(iris.data, iris.target, test_size=0.4, random_state=1)

# reformat train/test datasets for convenience
train = np.array(zip(X_train,y_train))
test = np.array(zip(X_test, y_test))

```

这是鸢尾花数据集、数据拆分以及索引的简要指南。

[![拆分鸢尾花数据集](../Images/8099def34ac1f68142c2841020df09ca.png)](https://cambridgecoding.files.wordpress.com/2016/01/knn3.jpg)

### 更多相关内容

+   [从理论到实践：构建一个 k-最近邻分类器](https://www.kdnuggets.com/2023/06/theory-practice-building-knearest-neighbors-classifier.html)

+   [分类的最近邻](https://www.kdnuggets.com/2022/04/nearest-neighbors-classification.html)

+   [Scikit-learn 中的 k-最近邻](https://www.kdnuggets.com/2022/07/knearest-neighbors-scikitlearn.html)

+   [LangChain 101：构建你自己的 GPT 驱动应用](https://www.kdnuggets.com/2023/04/langchain-101-build-gptpowered-applications.html)

+   [使用 LlamaIndex 构建你自己的 PandasAI](https://www.kdnuggets.com/build-your-own-pandasai-with-llamaindex)

+   [使用 ChatGPT 的 GPTs 创建你自己的 GPT](https://www.kdnuggets.com/make-your-own-gpts-with-chatgpts-gpts)
��理想情况下，可以通过查看哪个值产生最准确的预测来优化k（参见 [交叉验证](http://scikit-learn.org/stable/modules/cross_validation.html)）。

对kNN的一个很好的概述可以在 [这里](https://saravananthirumuruganathan.wordpress.com/2010/05/17/a-detailed-introduction-to-k-nearest-neighbor-knn-algorithm/) 阅读。一个更深入的实现，包括加权和搜索树，见 [这里](http://opencv-python-tutroals.readthedocs.org/en/latest/py_tutorials/py_ml/py_knn/py_knn_understanding/py_knn_understanding.html)。

**完整脚本**

完整的脚本如下：

```py
from sklearn.datasets import load_iris
from sklearn import cross_validation
from sklearn.metrics import classification_report, accuracy_score
from operator import itemgetter
import numpy as np
import math
from collections import Counter

# 1) given two data points, calculate the euclidean distance between them
def get_distance(data1, data2):
    points = zip(data1, data2)
    diffs_squared_distance = [pow(a - b, 2) for (a, b) in points]
    return math.sqrt(sum(diffs_squared_distance))

# 2) given a training set and a test instance, use getDistance to calculate all pairwise distances
def get_neighbours(training_set, test_instance, k):
    distances = [_get_tuple_distance(training_instance, test_instance) for training_instance in training_set]
    # index 1 is the calculated distance between training_instance and test_instance
    sorted_distances = sorted(distances, key=itemgetter(1))
    # extract only training instances
    sorted_training_instances = [tuple[0] for tuple in sorted_distances]
    # select first k elements
    return sorted_training_instances[:k]

def _get_tuple_distance(training_instance, test_instance):
    return (training_instance, get_distance(test_instance, training_instance[0]))

# 3) given an array of nearest neighbours for a test case, tally up their classes to vote on test case class
def get_majority_vote(neighbours):
    # index 1 is the class
    classes = [neighbour[1] for neighbour in neighbours]
    count = Counter(classes)
    return count.most_common()[0][0] 

# setting up main executable method
def main():

    # load the data and create the training and test sets
    # random_state = 1 is just a seed to permit reproducibility of the train/test split
    iris = load_iris()
    X_train, X_test, y_train, y_test = cross_validation.train_test_split(iris.data, iris.target, test_size=0.4, random_state=1)

    # reformat train/test datasets for convenience
    train = np.array(zip(X_train,y_train))
    test = np.array(zip(X_test, y_test))

    # generate predictions
    predictions = []

    # let's arbitrarily set k equal to 5, meaning that to predict the class of new instances,
    k = 5

    # for each instance in the test set, get nearest neighbours and majority vote on predicted class
    for x in range(len(X_test)):

            print 'Classifying test instance number ' + str(x) + ":",
            neighbours = get_neighbours(training_set=train, test_instance=test[x][0], k=5)
            majority_vote = get_majority_vote(neighbours)
            predictions.append(majority_vote)
            print 'Predicted label=' + str(majority_vote) + ', Actual label=' + str(test[x][1])

    # summarize performance of the classification
    print '\nThe overall accuracy of the model is: ' + str(accuracy_score(y_test, predictions)) + "\n"
    report = classification_report(y_test, predictions, target_names = iris.target_names)
    print 'A detailed classification report: \n\n' + report

if __name__ == "__main__":
    main()

```

**想了解更多？查看我们的两天数据科学训练营**：

[https://cambridgecoding.com/datascience-bootcamp](https://cambridgecoding.com/datascience-bootcamp)

**简介：[Natasha Latysheva](http://blog.cambridgecoding.com/author/natlat/)** 是MRC分子生物学实验室的计算生物学博士生。她的研究集中于癌症基因组学、统计网络分析和蛋白质结构。更广泛地说，她的研究兴趣包括数据密集型分子生物学、机器学习（特别是深度学习）和数据科学。

[原文](http://blog.cambridgecoding.com/2016/01/16/machine-learning-under-the-hood-writing-your-own-k-nearest-neighbour-algorithm/)。经允许转载。

**相关：**

+   [Scikit-learn 和 Python 堆栈教程：简介，分类器实现](/2016/01/scikit-learn-tutorials-introduction-classifiers.html)

+   [7 Steps to Mastering Machine Learning With Python](/2015/11/seven-steps-machine-learning-python.html)

+   [7 Steps to Understanding Deep Learning](/2016/01/seven-steps-deep-learning.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

### 更多相关话题

+   [从理论到实践：构建 k-最近邻分类器](https://www.kdnuggets.com/2023/06/theory-practice-building-knearest-neighbors-classifier.html)

+   [用于分类的最近邻](https://www.kdnuggets.com/2022/04/nearest-neighbors-classification.html)

+   [Scikit-learn 中的 K 最近邻](https://www.kdnuggets.com/2022/07/knearest-neighbors-scikitlearn.html)

+   [LangChain 101：构建你自己的 GPT 驱动应用](https://www.kdnuggets.com/2023/04/langchain-101-build-gptpowered-applications.html)

+   [使用 LlamaIndex 构建你自己的 PandasAI](https://www.kdnuggets.com/build-your-own-pandasai-with-llamaindex)

+   [使用 ChatGPT 的 GPTs 创建你自己的 GPT](https://www.kdnuggets.com/make-your-own-gpts-with-chatgpts-gpts)
