# 事物并不总是正态的：一些“其他”分布

> 原文：[https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html](https://www.kdnuggets.com/2023/01/things-arent-always-normal-distributions.html)

![事物并不总是正态的：一些“其他”分布](../Images/e8e915fc0bc88a4b78d301f99865041b.png)

图片来源：Unsplash

## 关键要点

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行IT管理

* * *

概率分布在数据科学和统计学中扮演着重要角色。尽管正态（高斯）分布是最流行的概率分布，但在数据科学中也可以使用其他概率分布：

+   伽马分布用于建模代表事件之间时间间隔的连续变量

+   贝塔分布用于建模代表比例或概率的连续变量

+   伯努利分布用于建模二元结果

概率分布是描述随机变量行为的数学函数。在数据科学和机器学习中，概率分布通常用于描述数据集的潜在分布，预测未来事件，并评估机器学习模型的性能。例如，高斯分布是一种参数分布，它依赖于两个变量，即***均值***和***标准差***。因此，一旦均值和标准差参数已知，就可以创建一个正态分布的数据集。举例来说，下面的代码创建了一个包含1000个值的数据集，这些值服从均值为0、标准差为0.1的正态分布。

```py
import numpy as np

import matplotlib.pyplot as plt

mu, sigma = 0, 0.1 # mean and standard deviation

s = np.random.normal(mu, sigma, 1000)

count, bins, ignored = plt.hist(s, 30, density = True)
plt.plot(bins, 1/(sigma * np.sqrt(2 * np.pi)) *
               np.exp( - (bins - mu)**2 / (2 * sigma**2) ),
         linewidth=2, color='r')
plt.show() 
```

![事物并不总是正态的：一些“其他”分布](../Images/951af5be836a4f7367bd65d00596e3f3.png)

图1\. 高斯分布的可视化。

我们还可以绘制不同均值和标准差组合的正态分布，如下所示：

```py
import numpy as np
import scipy.stats as stats 
import matplotlib.pyplot as plt

#define three Gamma distributions
x = np.linspace(-10, 10, 101)
y1 = stats.norm.pdf(x, 0, 2)
y2 = stats.norm.pdf(x, 0, 4)
y3 = stats.norm.pdf(x, 2, 2)

#add lines for each distribution
plt.plot(x, y1, label='mu=0, sigma=2')
plt.plot(x, y2, label='mu=0, sigma=4')
plt.plot(x, y3, label='mu=4, sigma=1')

#add legend
plt.legend()

#display plot
plt.show() 
```

![事物并不总是正态的：一些“其他”分布](../Images/2c38083001f78690f1a0605d2749e6ca.png)

图2\. 高斯分布在不同均值和标准差下的可视化。

概率分布在数据科学和机器学习中非常重要，因为它们提供了一种量化和分析不确定性的方法，而不确定性是许多现实世界过程的固有部分。它们在统计推断中也发挥着关键作用，统计推断是使用数据对总体或过程进行推测的过程。

在本文中，我们将解释三种用于机器学习的概率分布，即 ***伽马分布***、***贝塔分布*** 和 ***伯努利分布***。

# 伽马分布

伽马分布是一种连续概率分布，常用于建模在以恒定速率发生的过程中事件之间的时间。它由形状参数（k）和率参数（ϴ）来定义，其概率密度函数（PDF）定义为

![事情并不总是正常的：一些](../Images/2bb077d8e916af5c625091ca78a4bbb6.png)

其中 Γ(k) 是伽马函数，ϴ 是尺度参数，k 是形状参数。

![事情并不总是正常的：一些](../Images/bd31bfb1d28bc1739d5b6d4ed8095217.png)

图 3\. 伽马分布的可视化。

伽马分布通常用于建模表示事件之间时间间隔的连续变量的分布。例如，它可以用于建模顾客到达商店的时间间隔，或者设备故障的时间间隔。

**代码示例：** 在 Python 中，可以使用 ***scipy.stats*** 模块中的“gamma”函数生成伽马分布。例如，下面的代码将生成一个具有伽马分布的随机变量 x，并绘制该分布的概率密度函数。k 和 theta 参数分别指定伽马分布的形状参数和率参数。

```py
import numpy as np
import scipy.stats as stats 
import matplotlib.pyplot as plt

#define three Gamma distributions
x = np.linspace(0, 40, 100)
y1 = stats.gamma.pdf(x, a=5, scale=3)
y2 = stats.gamma.pdf(x, a=2, scale=5)
y3 = stats.gamma.pdf(x, a=4, scale=2)

#add lines for each distribution
plt.plot(x, y1, label='shape=5, scale=3')
plt.plot(x, y2, label='shape=2, scale=5')
plt.plot(x, y3, label='shape=4, scale=2')

#add legend
plt.legend()

#display plot
plt.show() 
```

![事情并不总是正常的：一些](../Images/99e7c7af450e303ced6d5c93bd8e76a2.png)

图 4\. 不同形状和尺度参数的伽马分布的可视化。

除了生成随机变量外，***scipy.stats*** 模块还提供了从数据中估计伽马分布参数的函数、检验拟合优度的函数，以及使用伽马分布进行统计检验的函数。这些函数对分析被认为遵循伽马分布的数据很有用。

# 贝塔分布

贝塔分布是一种定义在区间 [0, 1] 上的连续概率分布。它常用于建模比例或概率，并由两个形状参数定义，通常记作 α 和 β。贝塔分布的概率密度函数（PDF）定义为

![事情并不总是正常的：一些](../Images/56ea52569dfbed850de934c761b95c30.png)

PDF 也可以表示为

![事情并不总是正常的：一些](../Images/9b8dd6a46a75d5f83bd67b7a63a75e74.png)

其中

![事情并不总是正常的：一些](../Images/b7e0946c49628ab87328c9ae041fa49e.png)

是贝塔函数。

![事情并不总是正常的：一些](../Images/ab4b468ef3b34c897e27da46002d8e7b.png)

图 5\. 贝塔分布的可视化。

Beta 分布常用于建模代表比例或概率的连续变量的分布。例如，它可以用来建模在某些营销努力下客户进行购买的概率，或机器学习模型做出正确预测的概率。

**代码示例**：在 Python 中，可以使用 ***scipy.stats*** 模块中的 "beta" 函数生成 Beta 分布。例如：

```py
import numpy as np
import matplotlib.pyplot as plt
from scipy.stats import beta

# Set the shape paremeters
a, b = 80, 10

# Generate the value between
x = np.linspace(beta.ppf(0.01, a, b),beta.ppf(0.99, a, b), 100)

# Plot the beta distribution
plt.figure(figsize=(7,7))
plt.xlim(0.7, 1)
plt.plot(x, beta.pdf(x, a, b), 'r-')
plt.title('Beta Distribution', fontsize='15')
plt.xlabel('Values of Random Variable X (0, 1)', fontsize='15')
plt.ylabel('Probability', fontsize='15')
plt.show()
```

这将生成一个具有 Beta 分布的随机变量 *x* 并绘制该分布的概率密度函数。*a* 和 *b* 参数分别指定 Beta 分布的形状参数。

***scipy.stats*** 模块具有从数据中估计 Beta 分布参数、评估拟合优度以及使用 Beta 分布进行统计测试的函数，除此之外，还可以生成随机变量。

# 伯努利分布

伯努利分布是一种离散概率分布，描述了单个二元事件的结果，例如抛硬币。它由一个参数 *p* 特征，该参数是事件发生的概率。伯努利分布的概率质量函数定义为

![事物并不总是正常：一些](../Images/655ba2132da305c1f2a8b20aab8ea6d9.png)

其中 n 是 0 或 1，代表事件的结果。

![事物并不总是正常：一些](../Images/d3b4d48b0ff4e8f1825a14b045864f49.png)

图 6\. 伯努利分布的可视化。

该分布通常用于建模二元结果的概率，例如客户进行购买的概率或机器学习模型做出正确预测的概率。

**代码示例**：在 Python 中，可以使用 ***scipy.stats*** 模块中的 "Bernoulli" 函数生成伯努利分布。例如：

```py
from scipy.stats import bernoulli
import seaborn as sb

data_bern = bernoulli.rvs(size=1000,p=0.6)
ax = sb.distplot(data_bern,
                  kde=True,
                  color='crimson',
                  hist_kws={"linewidth": 25,'alpha':1})
ax.set(xlabel='Bernouli', ylabel='Frequency')
```

这将生成一个具有伯努利分布的随机变量 *x* 并绘制该分布的概率质量函数。*p* 参数指定事件发生的概率。

除了生成随机变量，***scipy.stats*** 模块还提供了从数据中估计伯努利分布概率参数、测试拟合优度以及使用伯努利分布进行统计测试的函数。在评估可能遵循伯努利分布的数据时，这些函数可能会很有用。

总之，Gamma 分布用于建模表示事件间时间间隔的连续变量，Beta 分布用于建模表示比例或概率的连续变量，而 Bernoulli 分布用于建模二元结果。了解这些概率分布背后的概念对于你的机器学习旅程非常有帮助，因为它们能帮助你建模解决数据科学和机器学习中的各种问题。

**[本杰明·O·塔约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是一位物理学家、数据科学教育者和作家，也是 DataScienceHub 的所有者。之前，本杰明曾在中央俄克拉荷马大学、大峡谷大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关内容

+   [为什么我们将永远需要人类来训练 AI——有时需要实时训练](https://www.kdnuggets.com/2021/12/why-we-need-humans-training-ai.html)

+   [持续学习：AI 如何防止数据泄露](https://www.kdnuggets.com/2023/07/always-learning-ai-prevents-data-breaches.html)

+   [MLOps 思维模式：始终保持生产就绪](https://www.kdnuggets.com/2023/07/mlops-mindset-always-productionready.html)

+   [StarCoder：你一直想要的编码助手](https://www.kdnuggets.com/2023/05/starcoder-coding-assistant-always-wanted.html)

+   [区分数据科学家与其他职业的5个特点](https://www.kdnuggets.com/2021/11/5-things-set-data-scientist-apart-other-professions.html)

+   [正态分布综合指南](https://www.kdnuggets.com/2022/06/comprehensive-guide-normal-distribution.html)
