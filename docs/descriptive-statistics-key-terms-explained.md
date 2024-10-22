# 描述性统计关键术语解释

> 原文：[`www.kdnuggets.com/2017/05/descriptive-statistics-key-terms-explained.html`](https://www.kdnuggets.com/2017/05/descriptive-statistics-key-terms-explained.html)

尽管统计学是数据科学的核心工具集，但它常常被更为技术性的技能如编程所忽视。即使是机器学习算法，它们依赖于数学概念如代数和微积分——更不用说统计学了！——也常常在处理时高于所需的基础数学水平，这可能导致一些“数据科学家”对其职业的一个关键方面缺乏基本理解。

本文不会解决了解与不了解统计学绝对基础之间的差异。然而，如果你无法完全理解文中包含的基本描述性统计术语，你肯定缺乏建立在这些基础上构建更多坚实且有用的专业概念所需的基础知识。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供 IT 支持

* * *

![统计学标题](img/6f29d24dc50c36852ee81342902e085d.png)

这里是 15 个基本描述性统计关键术语的集合，以易于理解的语言进行解释。接下来是一个全面的示例，其中包含一些 Python 代码。请注意，作为基本介绍，本文中故意省略了术语的数学表示和描述。

# **1\. 描述性统计**

描述性统计是一组用于定量描述或总结数据集的统计工具。描述性统计旨在总结数据，因此可以与推断统计区分开来，后者在性质上更具预测性。

# **2\. 总体**

总体是一个被选中的个体或群体，代表了某个特定感兴趣群体的完整成员集合。

# **3\. 样本**

样本是从更大的总体中抽取的一个子集。如果这个抽样方式确保总体中的每个成员都有平等的选择机会，那么结果称为**随机样本**。

# **4\. 参数**

参数是从总体中生成的一个值。如果我拥有地球上所有人的数据，并计算出平均年龄，那么这个值就是一个参数。

# **5\. 统计量**

统计量是从样本中生成的一个值。如果我计算了地球上某一人群的平均年龄（更具可行性），这个值将是一个统计量。因此，统计学科的由来。

# **6\. 泛化能力**

泛化能力指的是基于从样本中收集的数据结果，对总体特征得出结论的能力。这种能力并不是自然而然存在的，而是高度依赖于样本收集的性质、样本大小以及各种其他因素。

# **7\. 分布**

分布是按一个变量的值从低到高排列的数据。这种排列及其特征，如形状和扩展，提供了关于基础样本的信息。

# **8\. 平均数**

平均数，与中位数和众数一起，是衡量集中趋势的三个主要度量之一，这三者共同评估分布的一个重要基本方面。作为变量值（或得分）分布的简单算术平均值，平均数提供了分布的单一、简洁的数值总结。平均数也可能是一般研究中最常遇到的统计量。总体平均数用 μ 表示，而样本平均数用 x̄ 表示。

# **9\. 中位数**

中位数是分布中位于第 50 百分位的得分，分隔了得分的前后 50%。中位数对将分布得分集合分成两半以及帮助识别分布的偏斜非常有用。

# **10\. 众数**

众数就是在分布中出现频率最高的得分。多峰指的是有多个众数的分布；双峰指的是有两个众数的分布。

# **11\. 偏斜**

当分布的一端的得分多于另一端时，会导致偏斜。当分布的得分更多集中在高端时，低端相对较少的得分会形成尾部，这种情况称为负偏斜。正偏斜是指分布在高端形成尾部。

通常，在负偏斜的分布中，我们会期望平均数小于中位数，而在正偏斜的分布中，我们会期望平均数大于中位数。

# **12\. 范围**

离散度的一个重要度量是范围，它是分布中最大值和最小值之间的差异。

# **13\. 方差**

方差是分布中得分离散的统计平均值。方差通常不会单独使用，但它可以作为描述性统计测量的一个有用计算步骤，如标准差。

# **14\. 标准差**

分布的标准差是个体分布分数与分布均值之间的平均偏差。标准差单独提供了一个良好的度量，衡量一个分布分数的分散程度。与均值一起考虑时，这两个度量可以很好地概述分数的分布。

# **15. 四分位距 (IQR)**

IQR 是划分第 75 百分位数和第 25 百分位数之间的分数差，即第三四分位数和第一四分位数。

下面是一个简单的 Python 脚本，用于计算上述讨论的大部分描述性统计数据，随后是一个示例。

```py
import numpy as np
import matplotlib.pyplot as plt
import scipy.stats

dist = np.array([ 1, 4, 5, 6, 8, 8, 9, 10, 10, 11, 11, 13, 13, 13, 14, 14, 15, 15, 15, 15 ])

print('Descriptive statistics for distribution:\n', dist)
print('Number of scores:', len(dist))
print('Number of unique scores:', len(np.unique(dist))
print('Sum:', sum(dist))
print('Min:', min(dist))
print('Max:', max(dist))
print('Range:', max(dist)-min(dist))
print('Mean:', np.mean(dist, axis=0))
print('Median:', np.median(dist, axis=0))
print('Mode:', scipy.stats.mode(dist)[0][0])
print('Variance:', np.var(dist, axis=0))
print('Standard deviation:', np.std(dist, axis=0))
print('1st quartile:', np.percentile(dist, 25))
print('3rd quartile:', np.percentile(dist, 75))
print('Distribution skew:', scipy.stats.skew(dist))

plt.hist(dist, bins=len(dist))
plt.yticks(np.arange(0, 6, 1.0))
plt.title('Histogram of distribution scores')
plt.show()
```

```py
Descriptive statistics for distribution:

[ 1  4  5  6  8  8  9 10 10 11 11 13 13 13 14 14 15 15 15 15]

Number of scores: 20
Number of unique scores: 11
Sum: 210
Min: 1
Max: 15
Range: 14
Mean: 10.5
Median: 11.0
Mode: 15
Variance: 16.15
Standard deviation: 4.01870625948
1st quartile: 8.0
3rd quartile: 14.0
Distribution skew: -0.714152479663
```

![直方图分布分数](img/aef8c81df19a924cd33afad8498d3447.png)

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是一位数据科学家和 KDnuggets 的主编，这是一个开创性的在线数据科学和机器学习资源。他的兴趣包括自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系。

### 了解更多此主题

+   [数据库关键术语解释](https://www.kdnuggets.com/2016/07/database-key-terms-explained.html)

+   [机器学习关键术语解释](https://www.kdnuggets.com/2016/05/machine-learning-key-terms-explained.html)

+   [深度学习关键术语解释](https://www.kdnuggets.com/2016/10/deep-learning-key-terms-explained.html)

+   [自然语言处理关键术语解释](https://www.kdnuggets.com/2017/02/natural-language-processing-key-terms-explained.html)

+   [生成式 AI 关键术语解释](https://www.kdnuggets.com/generative-ai-key-terms-explained)

+   [遗传算法关键术语解释](https://www.kdnuggets.com/2018/04/genetic-algorithm-key-terms-explained.html)
