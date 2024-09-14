# 线性回归模型选择：平衡简洁性和复杂性

> 原文：[https://www.kdnuggets.com/2023/02/linear-regression-model-selection-balancing-simplicity-complexity.html](https://www.kdnuggets.com/2023/02/linear-regression-model-selection-balancing-simplicity-complexity.html)

![线性回归模型选择：平衡简洁性和复杂性](../Images/4aec1c8a060a8e79a81d4fca50744cef.png)

图片来源：[freepik](https://www.freepik.com/)

简单线性回归是最古老的预测建模类型之一。在简单线性回归中，我们有一个特征 (![Equation](../Images/fcb1f8de5e7672d9fe0c9a7da57d2750.png)) 和一个连续目标变量 (![Equation](../Images/92cee04f93ff75bc34f082e5efa945f5.png))。目标是找到一个数学函数来描述X和y之间的关系。最简单的形式是尝试一个线性（度数 = 1）关系，形式为 ![Equation](../Images/72a022503f9434f7a28bbadf6e04c726.png)，其中 ![Equation](../Images/8967ecc5e80780e956772991143d706f.png) 和 *a**1* 是需要确定的系数。一个二次模型（度数 = 2）的形式为 ![Equation](../Images/0abbfbe3a8a7fe16b9cad81e5a656cb9.png)，其中 ![Equation](../Images/a9b62396df36649ba189a47f11c3cf91.png)，![Equation](../Images/7e699bbbd6ec52fa38b17d03d4ea1b06.png) 和 ![Equation](../Images/dce1b24bf9f7a6b7e814e534f10ce7fa.png) 是需要确定的回归系数。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业之路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

假设我们有一个下图所示的数据集。

![线性回归模型选择：平衡简洁性和复杂性](../Images/fb30bd9f6870c76fac363b0e55678d15.png)

图片作者

我们的目标是执行回归分析，以量化*X*和*y*之间的关系，即*y = f(X)*。一旦获得这一关系，我们就可以为任何给定的*X*值预测新的*y*值。

首先，我们生成一个散点图来显示*X*和*y*之间的关系。

```py
import pandas as pd
import pylab
import matplotlib.pyplot as plt
import numpy as np

data = pd.read_csv("file.csv")
X = data.X.values
y = data.y.values

plt.scatter(X, y)
plt.xlabel('X')
plt.ylabel('y')
plt.show()
```

![线性回归模型选择：平衡简洁性和复杂性](../Images/787df4b5fc65fc3e2112aab792bb7e6b.png)

要对数据执行一次多项式拟合（度数 = 1），我们可以使用以下代码：

```py
degree = 1
model=pylab.polyfit(X,y,degree)
y_pred=pylab.polyval(model,X)
#calculating R-squared value
R2 = 1 - ((y-y_pred)**2).sum()/((y-y.mean())**2).sum()
```

通过将度数值更改为度数 = 2 和度数 = 10，我们可以对数据执行更高阶的多项式拟合。

下图展示了针对不同多项式拟合的数据所获得的原始值和预测值的图示。

![线性回归模型选择：简洁性与复杂性的平衡](../Images/12f5d8077dbe93188886ed35a4a195c2.png)

图片由作者提供

下表总结了不同模型的拟合优度得分（R2分数）：

从上图中，我们观察到以下几点：

![线性回归模型选择：简洁性与复杂性的平衡](../Images/d25b40416767d1b97c204f8aac6eb4ae.png)

+   线性模型（度数 = 1）过于简单，因此对数据的拟合不足，导致高偏差误差。

+   更高的多项式模型（度数 = 10）过于复杂，因此对数据的拟合过度，导致高方差误差。

+   二次模型（度数 = 2）似乎在简洁性和复杂性之间提供了正确的平衡。

# 结论

总之，我们展示了如何使用Python进行简单的线性回归。通常，任何度数的多项式都可以用来拟合数据。然而，在选择最终模型时，重要的是要找到简洁性和复杂性之间的正确平衡。一个过于简单的模型对数据拟合不足，导致高偏差误差。类似地，一个过于复杂的模型则过度拟合数据，导致高方差误差。应选择一个在简洁性和复杂性之间达到适当平衡的模型，因为这种模型在应用于新数据时会产生更低的误差。

**[本杰明·O·泰约](https://www.linkedin.com/in/benjamin-o-tayo-ph-d-a2717511/)** 是物理学家、数据科学教育者和作家，也是DataScienceHub的所有者。此前，本杰明曾在中欧大学、 grand canyon大学和匹兹堡州立大学教授工程学和物理学。

### 更多相关主题

+   [变压器的内存复杂度](https://www.kdnuggets.com/2022/12/memory-complexity-transformers.html)

+   [比较线性回归与逻辑回归](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets新闻 22:n12, 3月23日：最佳数据科学书籍…](https://www.kdnuggets.com/2022/n12.html)

+   [预测制作：Python中线性回归的初学者指南](https://www.kdnuggets.com/2023/06/making-predictions-beginner-guide-linear-regression-python.html)

+   [使用NumPy从头开始实现线性回归](https://www.kdnuggets.com/linear-regression-from-scratch-with-numpy)
