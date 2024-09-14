# 理解集中趋势

> 原文：[https://www.kdnuggets.com/2023/05/understanding-central-tendency.html](https://www.kdnuggets.com/2023/05/understanding-central-tendency.html)

![理解集中趋势](../Images/1438fe1a1be0d1472d333fbb2bb3d5a1.png)

图片由编辑提供

集中趋势是数据围绕一个特征值分布的属性。在数据科学和统计学中，两个最重要的集中趋势度量是均值和中位数。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

# 均值

对于具有 *N* 个观察值的数据集，均值通过将所有数据值相加然后除以 *N* 来计算。均值计算简单，但对数据集中离群值的存在非常敏感。

# 中位数

中位数是一个重要的集中趋势度量，不容易受到离群值的影响。数据集的中位数可以通过对数据集进行排序，然后确定中间值来确定，使得数据集的50%值小于中位数，50%值大于中位数。

# 计算数据集的均值和中位数

为了说明集中趋势的概念，我们计算了两个数据集的均值和中位数。第一个数据集是一个没有离群值的样本数据集，第二个数据集是一个包含离群值的样本数据集。

```py
import numpy as np
import matplotlib.pyplot as plt

# generate some random data
np.random.seed(1)
data1 = np.random.uniform(0,10, 1000)
data2 = np.append(data1, np.linspace(150,200,100))
data2 = np.append(data2, np.linspace(15,25,10))
data = list([data1, data2])
fig, ax = plt.subplots()

# build a box plot
ax.boxplot(data)
ax.set_ylim(0,25)
xticklabels=['sample data', 'sample data with outliers']
ax.set_xticklabels(xticklabels)

# add horizontal grid lines
ax.yaxis.grid(True)

# show the plot
plt.show() 
```

![理解集中趋势](../Images/4512783ea60e2a6d9462ab060a0411ab.png)

箱线图显示了有和没有离群值的样本数据。小的空心圆点表示离群值。图片由作者提供。

```py
# mean and median of sample data with no outliers
np.mean(data1)

>>> 5.006045994559051

np.median(data1)

>>> 5.075008116147119

# mean and median of sample data with outliers
np.mean(data2)

>>> 20.455897292395537

np.median(data2)

>>> 5.565300519330409
```

我们观察到，第二个数据集中离群值的存在导致均值从5.006增加到20.45，而中位数从5.075到5.565的变化相对较小。这表明中位数是一个稳健的集中趋势度量，因为它不容易受到数据集中离群值的影响。

# 总结

总结来说，我们回顾了计算集中趋势的两个最重要的指标。均值计算简单，但对数据集中离群值的存在非常敏感。中位数是一个稳健的集中趋势度量，并且不易受到离群值的影响。

**[本杰明·O·塔约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是一位物理学家、数据科学教育者和作家，同时也是 DataScienceHub 的拥有者。此前，本杰明曾在中央俄克拉荷马大学、大峡谷大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关内容

+   [免费麻省理工学院微积分课程：理解深度学习的关键](https://www.kdnuggets.com/2020/07/free-mit-courses-calculus-key-deep-learning.html)

+   [3分钟理解偏差-方差权衡](https://www.kdnuggets.com/2020/09/understanding-bias-variance-trade-off-3-minutes.html)

+   [理解贝叶斯定理的三种方法将提升你的数据科学水平](https://www.kdnuggets.com/2022/06/3-ways-understanding-bayes-theorem-improve-data-science.html)

+   [通过实现理解：决策树](https://www.kdnuggets.com/2023/02/understanding-implementing-decision-tree.html)

+   [理解 Python 中的迭代器与可迭代对象](https://www.kdnuggets.com/2022/01/understanding-iterables-iterators-python.html)

+   [人工智能中的智能体环境理解](https://www.kdnuggets.com/2022/05/understanding-agent-environment-ai.html)
