# 使用置信区间

> 原文：[https://www.kdnuggets.com/2023/04/working-confidence-intervals.html](https://www.kdnuggets.com/2023/04/working-confidence-intervals.html)

![使用置信区间](../Images/52760898b716931a277c6f663ebeda9e.png)

图片由编辑提供

在数据科学和统计学中，置信区间对于量化数据集的不确定性非常有用。65%置信区间表示数据值落在均值的一标准差范围内。95%置信区间表示数据值分布在均值的两标准差范围内。置信区间也可以估计为四分位数范围，它表示介于第25百分位数和第75百分位数之间的数据值，第50百分位数表示均值或中位数。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

在这篇文章中，我们演示了如何使用身高数据集来计算置信区间。身高数据集包含男性和女性的身高数据。

# 身高的概率分布可视化

首先，我们生成男性和女性身高的概率分布。

```py
# import necessary libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# obtain dataset
df = pd.read_csv('https://raw.githubusercontent.com/bot13956/Bayes_theorem/master/heights.csv')

# plot probability distribution of heights
sns.kdeplot(df[df.sex=='Female']['height'], label='Female')
sns.kdeplot(df[df.sex=='Male']['height'], label = 'Male')
plt.xlabel('height (inch)')
plt.title('probability distribution of Male and Female heights')
plt.legend()
plt.show()
```

![使用置信区间](../Images/ea3e135efbfad7dbbb2d2f0e8ad1a85d.png)

男性和女性身高的概率分布 | 图片由作者提供。

从上图可以观察到，男性的平均身高高于女性。

# 置信区间的计算

下面的代码展示了如何计算男性和女性身高的95%置信区间。

```py
# calculate confidence intervals for male heights
mu_male = np.mean(df[df.sex=='Male']['height'])
mu_male

>>> 69.31475494143555

std_male = np.std(df[df.sex=='Male']['height'])
std_male

>>> 3.608799452913512

conf_int_male = [mu_male - 2*std_male, mu_male + 2*std_male]
conf_int_male

>>> [65.70595548852204, 72.92355439434907]

# calculate confidence intervals for female heights
mu_female = np.mean(df[df.sex=='Female']['height'])
mu_female

>>> 64.93942425064515

std_female = np.std(df[df.sex=='Female']['height'])
std_female

>>> 3.752747269853828

conf_int_female = [mu_female - 2*std_female, mu_female + 2*std_female]
conf_int_female

>>> [57.43392971093749, 72.4449187903528]
```

# 使用箱线图的置信区间

另一种估计置信区间的方法是使用四分位数范围。可以使用箱线图来可视化四分位数范围，如下所示。

```py
# generate boxplot
data = list([df[df.sex=='Male']['height'],   
             df[df.sex=='Female']['height']])

fig, ax = plt.subplots()
ax.boxplot(data)
ax.set_ylabel('height (inch)')
xticklabels=['Male', 'Female']
ax.set_xticklabels(xticklabels)
ax.yaxis.grid(True)
plt.show()
```

![使用置信区间](../Images/905ebc0f7d3ba011ff388bb33af2d470.png)

箱线图展示了四分位数范围。| 图片由作者提供。

箱线图显示了四分位数范围，须状线表示数据的最小值和最大值（不包括离群值）。圆圈表示离群值。橙色线为中位数。从图中可以看出，男性身高的四分位数范围为[67英寸，72英寸]。女性身高的四分位数范围为[63英寸，67英寸]。男性身高的中位数为68英寸，而女性身高的中位数为65英寸。

# 总结

总结来说，置信区间在量化数据集中的不确定性方面非常有用。95%的置信区间表示数据值分布在均值的两个标准差范围内。置信区间也可以估计为四分位数范围，表示数据值介于第25百分位数和第75百分位数之间，其中第50百分位数表示均值或中位数值。

**[本杰明·O·泰约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是一位物理学家、数据科学教育者和作家，同时也是DataScienceHub的拥有者。之前，本杰明曾在中央俄克拉荷马大学、Grand Canyon大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关主题

+   [寻求模型信心：你能信任黑箱吗？](https://www.kdnuggets.com/the-quest-for-model-confidence-can-you-trust-a-black-box)

+   [与大数据工作：工具和技术](https://www.kdnuggets.com/working-with-big-data-tools-and-techniques)

+   [Python中SQLite数据库的使用指南](https://www.kdnuggets.com/a-guide-to-working-with-sqlite-databases-in-python)

+   [在机器学习模型中处理稀疏特征](https://www.kdnuggets.com/2021/01/sparse-features-machine-learning-models.html)

+   [在实际应用中使深度学习有效：数据中心课程](https://www.kdnuggets.com/2022/04/corise-deep-learning-wild-data-centric-course.html)

+   [远程工作的数据科学家必备的6种软技能](https://www.kdnuggets.com/2022/05/6-soft-skills-data-scientists-working-remotely.html)
