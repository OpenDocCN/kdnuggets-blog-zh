# 使用直方图探索数据分布

> 原文：[https://www.kdnuggets.com/2023/05/exploring-data-distributions-histograms.html](https://www.kdnuggets.com/2023/05/exploring-data-distributions-histograms.html)

![使用直方图探索数据分布](../Images/f26cd301002228f46e823712f476de40.png)

图片来自 Bing 图像生成器

直方图是一种数据可视化工具，在数据科学和统计学中广泛用于探索数据的分布。要创建直方图，首先将感兴趣的特征值分组到区间中，然后统计每个区间内的数据条目总数，这些数值表示计数。直方图是数据值（自变量）和计数（因变量）的图示。通常，被绘制的特征表示水平轴，而计数表示垂直轴。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在IT方面

* * *

# 使用直方图探索男性和女性身高数据分布

为了说明如何使用直方图探索数据分布，我们将使用 [heights 数据集](https://github.com/bot13956/Bayes_theorem/blob/master/heights.csv)。该数据集包含男性和女性的身高数据。

```py
# import necessary libraries 
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# obtain dataset
df = pd.read_csv('https://raw.githubusercontent.com/bot13956/Bayes_theorem/master/heights.csv')

# display head of dataset
pd.head() 
```

![使用直方图探索数据分布](../Images/1b63f1cbfdfac7627e53d27392102498.png)

[heights](https://github.com/bot13956/Bayes_theorem/blob/master/heights.csv) 数据集的头部信息显示了男性和女性的身高（以英寸为单位）。图片由作者提供。

## 所有身高的直方图

我们可以使用以下代码绘制所有身高的分布。

```py
sns.histplot(data = df, x="height")

plt.show()
```

![使用直方图探索数据分布](../Images/8e33669bb392070ffd910a22d8045187.png)

直方图显示了数据集中所有身高的分布。图片由作者提供。

## 显示男性和女性身高类别的直方图

由于数据集是分类的，我们可以生成如下所示的男性和女性身高分布的直方图。

```py
sns.histplot(data=df, x="height", hue="sex")

plt.show()
```

![使用直方图探索数据分布](../Images/12e10d96f0a64161f5b187073b98e10b.png)

显示男性和女性身高分布的直方图。图片由作者提供。

## 男性和女性身高的独立直方图

我们可以绘制男性和女性身高的独立直方图，如下所示。

```py
sns.histplot(data = df[df.sex=='Male']['height'], color='blue')

plt.show()
```

![使用直方图探索数据分布](../Images/d73d68f6976a0edafc710dc62002946c.png)

显示男性身高分布的直方图。图片由作者提供。

```py
sns.histplot(data = df[df.sex=='Female']['height'], color='orange')

plt.show()
```

![使用直方图探索数据分布](../Images/a02f430334a604c64f1690b843d122d8.png)

显示女性身高分布的直方图。图片由作者提供。

## 带有核密度估计图的直方图

可以添加核密度估计（KDE）图以平滑直方图，并估计数据的概率分布。

```py
sns.histplot(data = df, x = 'height', KDE = 'True')

plt.show()
```

![使用直方图探索数据分布](../Images/8ae440d0ea1ffa8ed96eceb13c84d527.png)

带有 KDE 图的全部身高数据直方图。图片由作者提供。

```py
sns.histplot(data=df, x="height", hue="sex", KDE = 'True')

plt.show()
```

![使用直方图探索数据分布](../Images/d370688d11721e36d2e7ea9b7bf5d706.png)

带有核密度估计图的男性和女性身高分布直方图。图片由作者提供。

显然，我们从上图中可以观察到，身高数据是双峰的，分别对应男性和女性类别。

# 摘要

总结来说，我们回顾了使用直方图探索数据分布的方法。使用身高数据集，我们展示了为数据集中的每个类别生成直方图的重要性。我们还展示了如何使用核密度估计图（KDE）平滑直方图，以产生近似的连续分布曲线。

**[本杰明·O·泰约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是一位物理学家、数据科学教育者和作家，同时也是 DataScienceHub 的所有者。此前，本杰明曾在中央俄克拉荷马大学、大峡谷大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关内容

+   [使用 Seaborn 创建美丽的直方图](https://www.kdnuggets.com/2023/01/creating-beautiful-histograms-seaborn.html)

+   [事情并非总是正常的：一些“其他”分布](https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html)

+   [优化数据存储：探索 SQL 中的数据类型和规范化](https://www.kdnuggets.com/optimizing-data-storage-exploring-data-types-and-normalization-in-sql)

+   [探索数据网格：数据架构中的范式转变](https://www.kdnuggets.com/exploring-data-mesh-a-paradigm-shift-in-data-architecture)

+   [导航数据革命：探索数据科学和机器学习中的蓬勃趋势](https://www.kdnuggets.com/navigating-the-data-revolution-exploring-the-booming-trends-in-data-science-and-machine-learning)

+   [使用 Python 探索数据清洗技术](https://www.kdnuggets.com/2023/04/exploring-data-cleaning-techniques-python.html)
