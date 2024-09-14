# 如何在 Python 中构建自己的逻辑回归模型

> 原文：[https://www.kdnuggets.com/2019/10/build-logistic-regression-model-python.html](https://www.kdnuggets.com/2019/10/build-logistic-regression-model-python.html)

[评论](#comments)

这个算法的名称可能会有些混淆，因为逻辑回归机器学习算法用于分类任务而非回归问题。这里的“回归”名称意味着将线性模型拟合到特征空间中。该算法将逻辑函数应用于特征的线性组合，以预测基于预测变量的分类因变量的结果。逻辑回归算法有助于估计基于给定预测变量的分类因变量的特定水平的概率。

假设你想预测明天在多伦多是否会下雨。这里的预测结果不是一个连续的数字，因为要么会下雨，要么不会下雨，因此线性回归不能应用。这里的结果变量是多个类别之一，使用逻辑回归会有所帮助。

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 事务

* * *

### 逻辑回归的应用

+   逻辑回归算法在流行病学领域中用于识别疾病的风险因素，并据此规划预防措施。

+   用于预测候选人是否会赢得政治选举，或预测选民是否会投票给某位候选人。

+   用于天气预报，以预测降雨的概率。

+   用于信用评分系统中的风险管理，以预测账户的违约情况。

### 环境和工具

1.  [numpy](https://numpy.org/)

1.  [matplotlib](https://matplotlib.org/)

### 代码在哪里？

话不多说，让我们开始代码吧。完整项目可以在 [这里](https://github.com/abhinavsagar/Machine-learning-tutorials/blob/master/logistic%20regression/logistic_regression.py) 找到。

让我们从加载库和依赖项开始。

```py
import numpy as np
import matplotlib.pyplot as plt
```

第一个函数用于定义 sigmoid 激活函数。sigmoid 函数的图像如下所示：

![图](../Images/9052a633e687e3cf8857f3300053d52d.png)

sigmoid 函数

```py
def sigmoid(scores): 
   return 1 / (1 + np.exp(-scores))
```

sigmoid 函数表示如下：

![](../Images/b0ce537c506a545ebe03b193e995d5cc.png)

Sigmoid 函数也称为逻辑函数，给出一个“S”形曲线，可以将任何实值数映射到 0 和 1 之间。如果曲线趋向于正无穷大，预测的 y 将变为 1，如果曲线趋向于负无穷大，预测的 y 将变为 0。如果 sigmoid 函数的输出大于 0.5，我们可以将结果分类为 1 或 yes；如果小于 0.5，我们可以将其分类为 0 或 no。

下一个函数用于返回对数似然度值。与此函数相关的参数包括特征向量、目标值和模型的权重。

对数似然度顾名思义是似然度的自然对数。反过来，给定一个样本和一个参数化的分布族（即，按参数索引的分布集合），似然度是一个将每个参数与观察到给定样本的概率关联的函数。

```py
def log_likelihood(features, target, weights): 
   scores = np.dot(features, weights)  
   ll = np.sum(target * scores - np.log(1 + np.exp(scores)))  
   return ll
```

下一个函数用于创建逻辑回归模型。与该函数相关的参数包括特征向量、目标值、训练步骤数、学习率以及一个默认设置为 false 的添加截距的参数。

首先使用特征向量分配权重。接下来使用特征向量和权重向量的点积计算分数。通过对分数应用 sigmoid 函数来找到预测值。现在可以计算误差，即目标值与预测值之间的差异。这个误差用于找出梯度，梯度是转置特征向量和误差的点积。可以通过将学习率乘以梯度并加到旧权重上来计算新的权重。

```py
def logistic_regression(features, target, num_steps, learning_rate, add_intercept=False):
    if add_intercept: 
       intercept = np.ones((features.shape[0], 1))  
       features = np.hstack((intercept, features))     weights = np.zeros(features.shape[1])

    for step in range(num_steps):  
      scores = np.dot(features, weights) 
      predictions = sigmoid(scores)  
      output_error_signal = target - predictions      
      gradient = np.dot(features.T, output_error_signal)    
      weights += learning_rate * gradient  

      if step % 10000 == 0:           
        print(log_likelihood(features, target, weights)) 

return weights
```

`**random()**` 函数用于在 Python 中生成随机数。Seed 函数用于保存随机函数的状态，以便在相同机器或不同机器上的多次执行代码时生成相同的随机数。选择的 seed 值为 10，数据点为 10000。

多元正态分布是将一维正态分布推广到更高维度的概念。这样的分布由其均值和协方差矩阵指定。

```py
np.random.seed(10)
num_observations = 10000 x1 = np.random.multivariate_normal([0, 0], [[1, 0.5], [0.5, 1]], num_observations)
x2 = np.random.multivariate_normal([1, 4], [[1, 0.5], [0.5, 1]], num_observations)
```

`hstack` 用于水平附加数据，而 `vstack` 用于垂直附加数据。首先使用 `vstack` 来根据特征分隔数据点，然后使用 `hstack` 来根据标签分隔数据点。

```py
simulated_separableish_features = np.vstack((x1, x2)).astype(np.float32)simulated_labels = np.hstack((np.zeros(num_observations), np.ones(num_observations)))
```

通过使用 scatter 函数绘制分隔的数据点来可视化结果，其中 alpha blending 值选择为 0.3。混合值范围从 0（透明）到 1（不透明）。

```py
plt.figure(figsize=(10, 8))plt.scatter(simulated_separableish_features[:, 0],   simulated_separableish_features[:, 1], c=simulated_labels,    alpha=0.3,)

plt.show()
```

### 结果

![图](../Images/1d6f43f3168a908d86c3d3c4483e9ec3.png)

样本数据点的分类

### 结论

总结一下，我展示了如何从零开始用 Python 构建一个逻辑回归模型。逻辑回归是一种广泛使用的监督学习技术。它是统计学家、研究人员和数据科学家在预测分析中的最佳工具之一。它具有几个优点，比如它是一个稳健的算法，因为自变量不需要具有相等的方差或正态分布，不假设因变量和自变量之间的线性关系，因此也可以处理非线性效应，并且它们也更易于检查且复杂度较低。

### 参考文献/进一步阅读

[**逻辑回归 — 详细概述**]

逻辑回归在二十世纪初被用于生物科学中。后来它被用于许多社会…](https://towardsdatascience.com/logistic-regression-detailed-overview-46c4da4303bc?source=post_page-----de8d2d8feae5----------------------)

[**提高你的逻辑回归模型的技巧 | Zopa 博客**]

当我们在 Zopa 创建信用风险评估或欺诈预防机器学习模型时，我们使用各种…](https://blog.zopa.com/2017/07/20/tips-honing-logistic-regression-models/?source=post_page-----de8d2d8feae5----------------------)

[**逻辑回归：概念与应用 | 博客 | 无量纲**]

通过这篇文章，我们尝试理解逻辑回归的概念及其应用。我们将…](https://dimensionless.in/logistic-regression-concept-application/?source=post_page-----de8d2d8feae5----------------------)

### 在离开之前

相应的源代码可以在这里找到。

[**abhinavsagar/机器学习教程**]

目前你无法执行该操作。你已在另一个标签页或窗口中登录。你在另一个标签页或…](https://github.com/abhinavsagar/Machine-learning-tutorials/blob/master/logistic%20regression/logistic_regression.py?source=post_page-----de8d2d8feae5----------------------)

### 联系方式

如果你想及时了解我的最新文章和项目，请[在 Medium 上关注我](https://medium.com/@abhinav.sagar)。以下是我的一些联系信息：

+   [个人网站](https://abhinavsagar.github.io/)

+   [Linkedin](https://in.linkedin.com/in/abhinavsagar4)

+   [Medium 个人资料](https://medium.com/@abhinav.sagar)

+   [GitHub](https://github.com/abhinavsagar)

+   [Kaggle](https://www.kaggle.com/abhinavsagar)

祝阅读愉快，学习愉快，编码愉快。

**个人简介：[Abhinav Sagar](https://www.linkedin.com/in/abhinavsagar4)** 是 VIT Vellore 的大四本科生。他对数据科学、机器学习及其在现实问题中的应用感兴趣。

[原文](https://towardsdatascience.com/how-to-build-your-own-logistic-regression-model-in-python-de8d2d8feae5)。转载已获许可。

**相关内容：**

+   [**逻辑回归：简明技术概述**](/2019/01/logistic-regression-concise-technical-overview.html)

+   [用于乳腺癌分类的卷积神经网络](/2019/10/convolutional-neural-network-breast-cancer-classification.html)

+   [如何使用Flask轻松部署机器学习模型](/2019/10/easily-deploy-machine-learning-models-using-flask.html)

### 相关主题

+   [构建预测模型：Python中的逻辑回归](https://www.kdnuggets.com/building-predictive-models-logistic-regression-in-python)

+   [LangChain 101：构建你自己的GPT驱动应用](https://www.kdnuggets.com/2023/04/langchain-101-build-gptpowered-applications.html)

+   [使用LlamaIndex构建你自己的PandasAI](https://www.kdnuggets.com/build-your-own-pandasai-with-llamaindex)

+   [线性回归与逻辑回归的比较](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [逻辑回归概述](https://www.kdnuggets.com/2022/02/overview-logistic-regression.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)
