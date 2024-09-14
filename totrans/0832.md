# 数据科学和分析师的统计学基础

> 原文：[https://www.kdnuggets.com/2023/08/fundamentals-statistics-data-scientists-analysts.html](https://www.kdnuggets.com/2023/08/fundamentals-statistics-data-scientists-analysts.html)

![数据科学和分析师的统计学基础](../Images/d481985c660a55be739320d17203bf66.png)

图片来源 编辑

正如英国数学家**卡尔·皮尔逊**曾经指出的，**统计学**是科学的语法，这一点在计算机与信息科学、物理科学和生物科学中尤为重要。当你开始你的**数据科学**或**数据分析**之旅时，拥有统计知识将帮助你更好地利用数据洞察。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

> “统计学是科学的语法。” **卡尔·皮尔逊**

统计学在数据科学和数据分析中的重要性不可低估。统计学提供了寻找结构和深入数据洞察的工具和方法。统计学和数学都喜爱事实而厌恶猜测。了解这两个重要学科的基础知识将让你在解决商业问题和做出数据驱动的决策时更加批判性和创造性。在本文中，我将涵盖数据科学和数据分析的以下统计学主题：

```py
**- Random variables

- Probability distribution functions (PDFs)

- Mean, Variance, Standard Deviation

- Covariance and Correlation 

- Bayes Theorem

- Linear Regression and Ordinary Least Squares (OLS)

- Gauss-Markov Theorem

- Parameter properties (Bias, Consistency, Efficiency)

- Confidence intervals

- Hypothesis testing

- Statistical significance 

- Type I & Type II Errors

- Statistical tests (Student's t-test, F-test)

- p-value and its limitations

- Inferential Statistics 

- Central Limit Theorem & Law of Large Numbers

- Dimensionality reduction techniques (PCA, FA)**
```

*如果你没有任何统计学知识，并且希望从零开始识别和学习基本的统计概念，以准备你的求职面试，那么这篇文章适合你。这篇文章也适合任何希望刷新统计学知识的人。*

# 在开始之前，欢迎来到LunarTech！

欢迎来到[**LunarTech.ai**](http://lunartech.ai/)，在这里我们理解在数据科学和人工智能领域中求职策略的力量。我们深入探讨了在竞争激烈的求职过程中所需的战术和策略。不论是定义职业目标、定制申请材料，还是利用求职网站和建立网络，我们的见解为你提供了获得梦想工作的指导。

正在为数据科学面试做准备吗？不用担心！我们将揭秘面试过程的复杂性，为你提供必要的知识和准备，以增加成功的机会。从初步的电话筛选到技术评估、技术面试和行为面试，我们都将一一讲解。

在[LunarTech.ai](http://lunartech.ai/)上，我们不仅仅停留在理论层面。我们是你在技术和数据科学领域取得卓越成功的跳板。我们的全面学习旅程量身定制，能够无缝融入你的生活方式，让你在个人和职业承诺之间取得完美平衡，同时掌握前沿技能。我们致力于你的职业发展，包括职位推荐、专家简历制作和面试准备，你将成为一个行业准备就绪的精英。

今天就加入我们有抱负的社区，开始一起踏上这段激动人心的数据科学旅程。与[LunarTech.ai](http://lunartech.ai/)一起，未来一片光明，你掌握着开启无限机会的钥匙。

# 随机变量

随机变量的概念是许多统计学概念的基石。虽然它的正式数学定义可能难以理解，但简而言之，**随机变量**是将随机过程的结果（如抛硬币或掷骰子）映射到数字的一种方式。例如，我们可以通过随机变量X来定义抛硬币的随机过程，如果结果是*正面*，则X取值为1；如果结果是*反面*，则X取值为0。

![统计学基础](../Images/3324b6942add771aa3a150c5797de250.png)

在这个例子中，我们有一个抛硬币的随机过程，这个实验可以产生***两个*** ***可能的结果***：{0,1}。所有可能结果的集合称为实验的***样本空间***。每次重复随机过程时，称为***事件***。在这个例子中，抛硬币得到反面是一个事件。这个事件发生的概率称为该事件的***概率***。事件的概率是随机变量取特定值x的可能性，可以用P(x)描述。在抛硬币的例子中，得到正面或反面的可能性相同，即0.5或50%。因此我们有如下设置：

![统计学基础](../Images/6a937f9fed26e06c00983fe79610958f.png)

在这个例子中，事件的概率只能取值于[0,1]范围内。

> 统计学在数据科学和数据分析中的重要性不容低估。统计学提供了寻找结构和深入数据洞察的工具和方法。

# 均值、方差、标准差

为了理解均值、方差及许多其他统计主题的概念，了解***总体***和***样本***的概念非常重要。**总体**是所有观察值（个体、对象、事件或过程）的集合，通常非常大且多样，而***样本***是从总体中抽取的一个子集，理想情况下它真实地代表了总体。

![数据科学家和分析师的统计学基础](../Images/64762a6771ba522873abdba3769fe436.png)

图片来源：作者

由于对整个总体进行实验要么不可能，要么成本过高，研究人员或分析师在实验或试验中使用样本而不是整个总体。为了确保实验结果可靠且适用于整个总体，样本需要真实地代表总体。也就是说，样本需要没有偏倚。为此，可以使用统计抽样技术，如[随机抽样、系统抽样、聚类抽样、加权抽样和分层抽样](https://github.com/TatevKaren/mathematics-statistics-for-data-science/tree/main/Sampling%20Techniques)。

## 均值

均值，也称为平均值，是一个有限数集的中心值。假设数据中的随机变量X具有以下值：

![数据科学家和分析师的统计学基础](../Images/7d8ac42015b031b9d6759a324dcd6c2b.png)

其中N是样本集中的观察值或数据点的数量，也即数据频率。然后，***样本均值***由**?**定义，通常用于近似***总体均值***，可以表示如下：

![数据科学家和分析师的统计学基础](../Images/e7524e67326f0ecf3aa350ec481c2e16.png)

均值也被称为***期望***，通常由**E**()或带顶线的随机变量定义。例如，随机变量X和Y的期望，即**E**(X)和**E**(Y)，可以表示如下：

![数据科学家和分析师的统计学基础](../Images/6932f3ff34266d5ba6da012197aa47c7.png)

```py
import numpy as np
import math
x = np.array([1,3,5,6])
mean_x = np.mean(x)
# in case the data contains Nan values
x_nan = np.array([1,3,5,6, math.nan])
mean_x_nan = np.nanmean(x_nan)
```

## 方差

方差度量数据点与平均值的分散程度，等于数据值与平均值（均值）之间差异的平方和。此外，***总体方差***可以表示如下：

![数据科学家和分析师的统计学基础](../Images/aef2b54c252e0b432cc23ac55fa0e7d5.png)

```py
x = np.array([1,3,5,6])
variance_x = np.var(x)

# here you need to specify the degrees of freedom (df) max number of logically independent data points that have freedom to vary
x_nan = np.array([1,3,5,6, math.nan])
mean_x_nan = np.nanvar(x_nan, ddof = 1)
```

要推导不同常见概率分布函数的期望和方差，[请查看这个Github仓库](https://github.com/TatevKaren/mathematics-statistics-for-data-science/tree/main/Deriving%20Expectation%20and%20Variances%20of%20Densities)。

## 标准差

标准差仅是方差的平方根，衡量数据偏离其均值的程度。由***sigma***定义的标准差可以表示如下：

![数据科学家和分析师的统计学基础](../Images/0421cc53478be66985f2e49fbf464ad0.png)

标准差通常优于方差，因为它与数据点具有相同的单位，这意味着你可以更容易地解释它。

```py
x = np.array([1,3,5,6])
variance_x = np.std(x)

x_nan = np.array([1,3,5,6, math.nan])
mean_x_nan = np.nanstd(x_nan, ddof = 1)
```

## 协方差

协方差是衡量两个随机变量联合变异性的指标，描述这两个变量之间的关系。它被定义为两个随机变量偏离其均值的乘积的期望值。两个随机变量X和Z之间的协方差可以通过以下表达式描述，其中**E**(X)和**E**(Z)分别表示X和Z的均值。

![数据科学家和分析师的统计学基础](../Images/d3083c8ab0ca95749eee908dbb0a92a4.png)

协方差可以取负值、正值或0。协方差的正值表示两个随机变量倾向于朝相同方向变化，而负值则表明这些变量在相反方向变化。最后，值为0意味着它们不会一起变化。

```py
x = np.array([1,3,5,6])
y = np.array([-2,-4,-5,-6])
#this will return the covariance matrix of x,y containing x_variance, y_variance on diagonal elements and covariance of x,y
cov_xy = np.cov(x,y)
```

## 相关性

相关性也是一种关系测量，它衡量两个变量之间线性关系的强度和方向。如果检测到相关性，则意味着两个目标变量的值之间存在关系或模式。两个随机变量X和Z之间的相关性等于这两个变量之间的协方差，除以这些变量的标准差乘积，可以通过以下表达式描述。

![数据科学家和分析师的统计学基础](../Images/b998c007c8071b20e9c274bcd38d3d9f.png)

相关系数的值范围在-1和1之间。请记住，变量与自身的相关性总是1，即 **Cor(X, X) = 1**。在解释相关性时，还要记住不要将其与***因果关系***混淆，因为相关性并不意味着因果关系。即使两个变量之间存在相关性，也不能得出一个变量导致另一个变量变化的结论。这种关系可能是偶然的，或者可能有第三个因素导致这两个变量的变化。

```py
x = np.array([1,3,5,6])
y = np.array([-2,-4,-5,-6])
corr = np.corrcoef(x,y)
```

# 概率分布函数

描述所有可能值、样本空间以及随机变量在给定范围内（介于最小值和最大值之间）可以取的相应概率的函数称为***概率分布函数 (pdf)***或概率密度。每个pdf需要满足以下两个标准：

![数据科学家和分析师的统计学基础](../Images/c8981273b9815e149643c5cae1bfb38c.png)

第一个标准指出所有概率应在[0,1]范围内，第二个标准则指出所有可能概率的总和应等于1。

概率函数通常分为两类：***离散型***和***连续型***。离散分布函数描述了具有***可数***样本空间的随机过程，例如投掷硬币的例子只有两种可能结果。连续分布函数描述了具有***连续***样本空间的随机过程。离散分布函数的例子包括[Bernoulli](https://en.wikipedia.org/wiki/Bernoulli_distribution)、[Binomial](https://en.wikipedia.org/wiki/Binomial_distribution)、[Poisson](https://en.wikipedia.org/wiki/Poisson_distribution)、[离散均匀](https://en.wikipedia.org/wiki/Discrete_uniform_distribution)。连续分布函数的例子包括[正态](https://en.wikipedia.org/wiki/Normal_distribution)、[连续均匀](https://en.wikipedia.org/wiki/Continuous_uniform_distribution)、[柯西](https://en.wikipedia.org/wiki/Cauchy_distribution)。

## 二项分布

[二项分布](https://brilliant.org/wiki/binomial-distribution/)是**n**次独立实验中成功次数的离散概率分布，每次实验的结果是：***成功***（概率为**p**）或***失败***（概率为**q** = 1 ? p）。假设一个随机变量X遵循二项分布，则在n次独立试验中观察到***k***次成功的概率可以由以下概率密度函数表示：

![统计学基础](../Images/f37d90ac17d3bef473905b164bc4ab65.png)

二项分布在分析重复独立实验的结果时非常有用，特别是当关注于在特定错误率下达到某一特定阈值的概率时。

**二项分布的均值与方差**

![统计学基础](../Images/793a3286af7d4bdf202f1658b68f77d7.png)

下图展示了一个二项分布的例子，其中独立试验的次数为8，每次试验的成功概率为16%。

![统计学基础](../Images/e3d45a5d2488199dc5f0d5b4485b3f06.png)

图片来源：作者

```py
# Random Generation of 1000 independent Binomial samples
import numpy as np
n = 8
p = 0.16
N = 1000
X = np.random.binomial(n,p,N)
# Histogram of Binomial distribution
import matplotlib.pyplot as plt
counts, bins, ignored = plt.hist(X, 20, density = True, rwidth = 0.7, color = 'purple')
plt.title("Binomial distribution with p = 0.16 n = 8")
plt.xlabel("Number of successes")
plt.ylabel("Probability")
plt.show()
```

## 泊松分布

[泊松分布](https://brilliant.org/wiki/poisson-distribution/)是指定时间段内事件发生次数的离散概率分布，给定该时间段内事件的平均发生次数。假设一个随机变量X遵循泊松分布，则在指定时间段内观察到***k***次事件的概率可以由以下概率函数表示：

![数据科学家和分析师的统计学基础](../Images/f70e05fed5b7f88a2618eb69aedfe7fb.png)

其中，***e*** 是 [***欧拉数***](https://brilliant.org/wiki/eulers-number/)，***?*** lambda 是***到达率参数***，是 X 的期望值。泊松分布函数因其在建模计数事件方面的使用而广受欢迎。

**泊松分布均值与方差**

![数据科学家和分析师的统计学基础](../Images/177c3326333a1ea215d2fac1b8fa3c7d.png)

例如，泊松分布可以用于建模在晚上 7 点到 10 点之间到达商店的顾客数量，或在晚上 11 点到 12 点之间到达急诊室的患者数量。下图展示了一个泊松分布的示例，我们计算网站访问者的数量，其中到达率 lambda 假设为每 7 分钟到达一次。

![数据科学家和分析师的统计学基础](../Images/259c67e9c69f061cda23eeb3c974fa80.png)

图片来源：作者

```py
# Random Generation of 1000 independent Poisson samples
import numpy as np
lambda_ = 7
N = 1000
X = np.random.poisson(lambda_,N)

# Histogram of Poisson distribution
import matplotlib.pyplot as plt
counts, bins, ignored = plt.hist(X, 50, density = True, color = 'purple')
plt.title("Randomly generating from Poisson Distribution with lambda = 7")
plt.xlabel("Number of visitors")
plt.ylabel("Probability")
plt.show()
```

## 正态分布

[正态概率分布](https://brilliant.org/wiki/normal-distribution/)是实值随机变量的连续概率分布。正态分布，也称为***高斯分布***，可以说是最常用的分布函数之一，广泛用于社会科学和自然科学的建模，例如，用于建模人们的身高或考试成绩。假设随机变量 X 遵循正态分布，则其概率密度函数可以表示如下。

![数据科学家和分析师的统计学基础](../Images/eb61bbf7565bdebf02e8b31368740443.png)

其中，参数 **?** (mu) 是分布的均值，也称为***位置参数***，参数 **?** (sigma) 是分布的标准差，也称为 *尺度参数*。数字 [**?**](https://www.mathsisfun.com/numbers/pi.html) (π) 是一个数学常数，约等于 3.14。

**正态分布均值与方差**

![数据科学家和分析师的统计学基础](../Images/8eeef4566971b3d28383dfa5a4030557.png)

下图展示了一个均值为 0 (**? = 0**) 和标准差为 1 (**? = 1**) 的正态分布示例，这被称为** *标准正态* **分布，具有 *对称性*。

![数据科学家和分析师的统计学基础](../Images/cb1dc399818525fd2138fe78b5ded2a8.png)

图片来源：作者

```py
# Random Generation of 1000 independent Normal samples
import numpy as np
mu = 0
sigma = 1
N = 1000
X = np.random.normal(mu,sigma,N)

# Population distribution
from scipy.stats import norm
x_values = np.arange(-5,5,0.01)
y_values = norm.pdf(x_values)
#Sample histogram with Population distribution
import matplotlib.pyplot as plt
counts, bins, ignored = plt.hist(X, 30, density = True,color = 'purple',label = 'Sampling Distribution')
plt.plot(x_values,y_values, color = 'y',linewidth = 2.5,label = 'Population Distribution')
plt.title("Randomly generating 1000 obs from Normal distribution mu = 0 sigma = 1")
plt.ylabel("Probability")
plt.legend()
plt.show()
```

# 贝叶斯定理

贝叶斯定理，或称为***贝叶斯法则***，可以说是概率与统计中最强大的规则，得名于著名的英国统计学家和哲学家托马斯·贝叶斯。

![数据科学家和分析师的统计学基础](../Images/cc4ec7fc1ab6ea7e73fc278fa386763c.png)

图片来源：[维基百科](https://en.wikipedia.org/wiki/Thomas_Bayes)

贝叶斯定理是一个强大的概率法则，它将***主观性***的概念引入了统计学和数学这个以事实为基础的领域。它描述了一个事件的概率，基于可能与该事件相关的***条件***的先验信息。例如，如果已知感染冠状病毒或Covid-19的风险随着年龄的增长而增加，那么贝叶斯定理允许根据年龄来更准确地确定某一已知年龄个体的风险，而不是简单地假设该个体是整个群体中的一个普通成员。

***条件概率***在贝叶斯理论中扮演着核心角色，它是指在另一个事件已经发生的情况下，某事件发生的概率。贝叶斯定理可以用以下表达式来描述，其中X和Y分别代表事件X和Y：

![统计学基础：数据科学家和分析师](../Images/47128d968029d6b488250bf093237ff5.png)

+   *Pr* (X|Y)：事件X在事件或条件Y已发生或为真的情况下发生的概率

+   *Pr* (Y|X)：事件Y在事件或条件X已发生或为真的情况下发生的概率

+   *Pr* (X) 和 *Pr* (Y)：分别是观察到事件X和Y的概率

在前面的例子中，感染冠状病毒（事件X）在某一特定年龄的条件下的概率是*Pr* (X|Y)，它等于在感染冠状病毒的条件下处于某一特定年龄的概率*Pr* (Y|X)，乘以感染冠状病毒的概率*Pr* (X)，然后除以处于某一特定年龄的概率*Pr* (Y)。

# 线性回归

之前介绍了变量间的因果关系，即一个变量对另一个变量有直接影响。当两个变量之间的关系是线性时，线性回归是一种统计方法，可以帮助建模自变量***独立变量***对另一个变量***因变量***值的影响。

因变量通常被称为***响应变量***或***解释变量***，**而自变量则通常被称为***回归变量***或***解释性变量***。当线性回归模型基于一个自变量时，该模型称为***简单线性回归***；当模型基于多个自变量时，它被称为***多重线性回归***。***简单线性回归***可以用以下表达式来描述：

![统计学基础：数据科学家和分析师](../Images/528bd5cb2a5c166f2b2cd8b1d917d145.png)

其中**Y**是因变量，**X**是数据中的自变量，**?0**是未知且恒定的截距，**?1**是未知且恒定的斜率系数或与变量X对应的参数。最后，**u**是模型在估计Y值时产生的误差项。线性回归的主要思想是找到通过一组配对（X, Y）数据的最佳拟合直线，***回归线***。线性回归应用的一个例子是建模*鳍长*对企鹅*体重*的影响，见下图。

![数据科学家和分析师的统计学基础](../Images/5a6e0ebe9c5ce46729a9aff019179334.png)

图片来源：作者

```py
# R code for the graph
install.packages("ggplot2")
install.packages("palmerpenguins")
library(palmerpenguins)
library(ggplot2)
View(data(penguins))
ggplot(data = penguins, aes(x = flipper_length_mm,y = body_mass_g))+
  geom_smooth(method = "lm", se = FALSE, color = 'purple')+
  geom_point()+
  labs(x="Flipper Length (mm)",y="Body Mass (g)")
```

三个自变量的多重线性回归可以通过以下表达式描述：

![数据科学家和分析师的统计学基础](../Images/319809a077aa03220960cdc5e5067133.png)

## 普通最小二乘法

普通最小二乘法（OLS）是一种用于估计线性回归模型中未知参数（如?0和?1）的方法。该模型基于***最小二乘法***的原则，该原则最小化观察到的因变量与其通过自变量的线性函数预测值之间的平方和，这些预测值通常称为***拟合值***。因变量Y的真实值和预测值之间的差异称为***残差***，OLS的工作就是最小化残差的平方和。这个优化问题导致了对未知参数?0和?1的以下OLS估计，这些估计也被称为***系数估计***。

![数据科学家和分析师的统计学基础](../Images/6325872184f467d2b2dcb9d6e9c5898b.png)

一旦这些简单线性回归模型的参数被估计，响应变量的***拟合值***可以通过以下方法计算：

![数据科学家和分析师的统计学基础](../Images/bd855af7372593e6f90f292b818618ab.png)

## 标准误差

估计误差项或***残差***可以通过以下方法确定：

![数据科学家和分析师的统计学基础](../Images/b210ac11afcaa334007128e98a98561e.png)

重要的是要记住误差项和残差之间的区别。误差项是无法观察到的，而残差是从数据中计算得出的。OLS估计每个观察值的误差项，但并不估计实际的误差项。因此，真实的误差方差仍然未知。此外，这些估计受到抽样不确定性的影响。这意味着我们永远无法从样本数据中确定这些参数的确切估计值，即真实值。然而，我们可以通过以下方式计算***样本残差方差***来进行估计。

![数据科学家和分析师的统计基础](../Images/cb73cb5751eede7cce34abf2c3151f87.png)

这个样本残差方差的估计值有助于估计参数估计的方差，通常表达如下：

![数据科学家和分析师的统计基础](../Images/ca99a0bcc7df57a3b9fa1b186f5f793c.png)

该方差项的平方根称为**标准误差**，它是评估参数估计准确性的关键组成部分。它用于计算检验统计量和置信区间。标准误差可以表达为：

![数据科学家和分析师的统计基础](../Images/c647c1cfe247f168506aff7b8bf84343.png)

> 重要的是要记住误差项和残差之间的区别。误差项是无法观察的，而残差是从数据中计算出来的。

## OLS 假设

OLS 估计方法做出了以下假设，满足这些假设才能获得可靠的预测结果：

**A1: 线性**假设声明模型在参数上是线性的。

**A2:** **随机** **样本**假设声明样本中的所有观察值都是随机选择的。

**A3: 外生性**假设声明自变量与误差项不相关。

**A4: 同方差性**假设声明所有误差项的方差是恒定的。

**A5: 无完美多重共线性**假设声明所有自变量都不是常数，并且自变量之间不存在精确的线性关系。

```py
def runOLS(Y,X):

   # OLS esyimation Y = Xb + e --> beta_hat = (X'X)^-1(X'Y)
   beta_hat = np.dot(np.linalg.inv(np.dot(np.transpose(X), X)), np.dot(np.transpose(X), Y))

   # OLS prediction
   Y_hat = np.dot(X,beta_hat)
   residuals = Y-Y_hat
   RSS = np.sum(np.square(residuals))
   sigma_squared_hat = RSS/(N-2)
   TSS = np.sum(np.square(Y-np.repeat(Y.mean(),len(Y))))
   MSE = sigma_squared_hat
   RMSE = np.sqrt(MSE)
   R_squared = (TSS-RSS)/TSS

   # Standard error of estimates:square root of estimate's variance
   var_beta_hat = np.linalg.inv(np.dot(np.transpose(X),X))*sigma_squared_hat

   SE = []
   t_stats = []
   p_values = []
   CI_s = []

   for i in range(len(beta)):
       #standard errors
       SE_i = np.sqrt(var_beta_hat[i,i])
       SE.append(np.round(SE_i,3))

        #t-statistics
        t_stat = np.round(beta_hat[i,0]/SE_i,3)
        t_stats.append(t_stat)

        #p-value of t-stat p[|t_stat| >= t-treshhold two sided] 
        p_value = t.sf(np.abs(t_stat),N-2) * 2
        p_values.append(np.round(p_value,3))

        #Confidence intervals = beta_hat -+ margin_of_error
        t_critical = t.ppf(q =1-0.05/2, df = N-2)
        margin_of_error = t_critical*SE_i
        CI = [np.round(beta_hat[i,0]-margin_of_error,3), np.round(beta_hat[i,0]+margin_of_error,3)]
        CI_s.append(CI)
        return(beta_hat, SE, t_stats, p_values,CI_s, 
               MSE, RMSE, R_squared)
```

# 参数性质

> 在假设满足 OLS 标准 A1 — A5 的情况下，OLS 系数估计量 β0 和 β1 是**最优线性无偏估计量**和**一致的**。
> 
> **高斯-马尔科夫定理**

该定理突出了 OLS 估计的性质，其中术语***最优线性无偏估计量***表示***最佳线性无偏估计量***。

## 偏差

估计量的**偏差**是其期望值与被估计参数的真实值之间的差异，可以表示如下：

![数据科学家和分析师的统计基础](../Images/49e9b6baa1cc3add6f92fbf86184700b.png)

当我们说估计量是***无偏的***时，我们的意思是偏差等于零，这意味着估计量的期望值等于真实的参数值，即：

![数据科学家和分析师的统计基础](../Images/5cbe5d12bcae8afa86a7463f36c5740a.png)

无偏性并不保证通过任何特定样本得到的估计值等于或接近?。它的意思是，如果从总体中***重复***抽取随机样本并每次计算估计值，那么这些估计值的平均值将等于或非常接近β。

## 效率

高斯-马尔可夫定理中的***最佳***一词与估计量的方差相关，并称为***效率***。*一个参数可以有多个估计量，但具有最低方差的那个被称为高效的**。**

## 一致性

一致性这一术语与***样本大小***和***收敛***这两个术语密切相关。如果估计量随着样本大小变得非常大而收敛于真实参数，那么该估计量被称为一致的，即：

![数据科学家和分析师统计学基础](../Images/643cd54573319672cabd9518fd86403d.png)

> 在假设OLS标准A1 — A5得到满足的情况下，OLS系数β0和β1的估计量是**BLUE**和**一致的**。
> 
> **高斯-马尔可夫定理**

所有这些性质都符合高斯-马尔可夫定理总结的OLS估计量。换句话说，OLS估计量具有最小的方差，它们是无偏的，参数线性的，并且是一致的。这些性质可以通过使用之前做出的OLS假设来进行数学证明。

# 置信区间

置信区间是包含真实总体参数的范围，其具有一定的预设概率，称为实验的***置信水平***，并且是通过使用样本结果和**误差范围**获得的。

## 误差范围

误差范围是样本结果与如果使用整个总体时结果之间的差异。

## 置信水平

置信水平描述了实验结果的确定性水平。例如，95%的置信水平意味着如果将相同的实验重复进行100次，那么其中95次的试验将会得到类似的结果。请注意，置信水平在实验开始之前就已经定义，因为它会影响实验结束时误差范围的大小。

## OLS估计量的置信区间

如前所述，简单线性回归的OLS估计量，即截距？0和斜率系数？1，是受采样不确定性的影响。然而，我们可以构建这些参数的置信区间，这些置信区间将在95%的所有样本中包含这些参数的真实值。即，95%置信区间可以解释为：

+   置信区间是一组值，在这些值中，假设检验在5%的水平上不能被拒绝。

+   置信区间有95%的机会包含真实值？。

OLS估计的95%置信区间可以如下构建：

![数据科学家和分析师统计学基础](../Images/38fa1db8673542963aafbfdeab828fca.png)

这个值基于参数估计、该估计的标准误差，以及代表5%拒绝规则的误差边际值1.96。这个值是通过使用[正态分布表](https://www.google.com/url?sa=i&url=https%3A%2F%2Ffreakonometrics.hypotheses.org%2F9404&psig=AOvVaw2IcJrhGrWbt9504WTCWBwW&ust=1618940099743000&cd=vfe&ved=0CAIQjRxqFwoTCOjR4v7rivACFQAAAAAdAAAAABAI)来确定的，这将在本文稍后讨论。与此同时，下图展示了95% CI的概念：

![数据科学家和分析师的统计学基础](../Images/ff27f468c8b352ac1bb32cc464fab061.png)

图片来源：[维基百科](https://en.wikipedia.org/wiki/Standard_deviation#/media/File:Standard_deviation_diagram.svg)

注意，置信区间还依赖于样本大小，因为它是通过基于样本大小的标准误差计算得出的。

> 置信水平在实验开始之前定义，因为它会影响实验结束时误差边际的大小。

# 统计假设检验

在统计学中，假设检验是一种测试实验或调查结果是否有意义的方法。基本上，是通过计算结果偶然出现的可能性来检验获得的结果是否有效。如果结果是偶然的，那么结果和实验都不可靠。假设检验是***统计推断***的一部分。

## 原假设与备择假设

首先，你需要确定要测试的论点，然后需要制定***原假设***和***备择假设***。**测试可以有两个可能的结果，基于统计结果你可以拒绝或接受所陈述的假设。作为经验法则，统计学家通常将需要拒绝的假设版本或表述放在原假设下，而接受和期望的版本放在备择假设下。*

## 统计显著性

让我们来看一下之前提到的例子，其中使用线性回归模型调查企鹅的*鳍片长度*（自变量）是否对*体重*（因变量）有影响。我们可以用以下统计表达式来制定这个模型：

![数据科学家和分析师的统计学基础](../Images/ef406ba8f83820bb548baf65882f6a4f.png)

然后，一旦OLS估计了系数，我们可以制定以下原假设和备择假设来测试鳍片长度对体重的** *统计学显著性* **影响：

![数据科学家和分析师的统计学基础](../Images/adfb148e34182cf51da062e279e4a3e2.png)

其中 H0 和 H1 分别代表原假设和备择假设。拒绝原假设意味着*鳍长*的每单位增加对*体重*有直接影响。考虑到参数估计 ?1 描述了独立变量*鳍长*对因变量*体重*的影响。这一假设可以重新表述如下：

![数据科学家和分析师的统计学基础](../Images/cc79759497d7f05ee2583b631f5b3848.png)

其中 H0 表示参数估计 ?1 等于 0，即*鳍长*对*体重*的影响是***统计上不显著***，而 H0 表示参数估计 ?1 不等于 0，暗示*鳍长*对*体重*的影响是***统计上显著***。

## 第一类错误和第二类错误

在进行统计假设检验时，需要考虑两种概念性错误：第一类错误和第二类错误。第一类错误发生在错误拒绝了原假设时，而第二类错误发生在错误未拒绝原假设时。一个混淆[矩阵](https://www.dataschool.io/simple-guide-to-confusion-matrix-terminology/)可以帮助清晰地可视化这两种错误的严重性。

> 作为经验法则，统计学家倾向于将假设的版本放在*原假设*下，这需要被拒绝，而可接受和期望的版本则陈述在*备择假设*下。

# 统计测试

一旦原假设和备择假设被陈述并且测试假设被定义，下一步是确定适当的统计测试并计算***测试统计量***。是否拒绝原假设可以通过将测试统计量与** *临界值* **进行比较来决定。这种比较显示了观察到的测试统计量是否比定义的临界值更极端，并且可能有两种结果：

+   测试统计量比临界值更极端时？可以拒绝原假设。

+   测试统计量不如临界值极端时？不能拒绝原假设。

临界值是基于预先指定的***显著性水平* ?**（通常选择为5%）和检验统计量所遵循的概率分布类型。临界值将概率分布曲线下的区域分为***拒绝区域***和***非拒绝区域***。有许多统计检验用于检验各种假设。统计检验的示例包括[学生t检验](https://en.wikipedia.org/wiki/Student%27s_t-test)、[F检验](https://en.wikipedia.org/wiki/F-test)、[卡方检验](https://en.wikipedia.org/wiki/Chi-squared_test)、[Durbin-Hausman-Wu内生性检验](https://www.stata.com/support/faqs/statistics/durbin-wu-hausman-test/)、[怀特异方差检验](https://en.wikipedia.org/wiki/White_test#:~:text=In%20statistics%2C%20the%20White%20test,by%20Halbert%20White%20in%201980.)。在本文中，我们将介绍这两种统计检验。

> 当错误地拒绝了原假设时，发生的是I型错误，而当错误地未拒绝原假设时，发生的是II型错误。

## 学生t检验

最简单和最流行的统计检验之一是学生t检验。它可以用于检验各种假设，特别是在处理假设时，主要关注的是寻找***单个变量***的统计显著效应。*t检验的统计量遵循[***学生t分布***](https://en.wikipedia.org/wiki/Student%27s_t-distribution)，可以按如下方式确定：

![数据科学家和分析师的统计学基础](../Images/587575f5de0ec8b6fb54b88c2ad87e81.png)

其中，分子中的h0是与之进行参数估计检验的值。因此，t检验统计量等于参数估计值减去假设值，然后除以系数估计的标准误。在之前提到的假设中，我们想要检验鳍长是否对体重有统计显著影响。此检验可以使用t检验进行，在这种情况下，h0等于0，因为斜率系数估计值是与值0进行检验的。

t检验有两个版本：***双侧t检验***和***单侧t检验***。你需要使用哪一种检验版本完全取决于你想要检验的假设。

双侧** *双尾t检验* **可以在假设检验*相等*与*不相等*关系时使用，类似于以下示例：

![数据科学家和分析师的统计学基础](../Images/06ebc78cdf8bbc84a867d5506bfddb7a.png)

双侧t检验有** *两个拒绝区域***，如下图所示：

![数据科学家和分析师的统计学基础](../Images/e22e9197e47fd0ba5ac5e396e5d2504c.png)

图片来源：[*Hartmann, K., Krois, J., Waske, B. (2018)：E-Learning Project SOGA: Statistics and Geospatial Data Analysis. Earth Sciences Department, Freie Universität Berlin*](https://www.geo.fu-berlin.de/en/v/soga/Basics-of-statistics/Hypothesis-Tests/Introduction-to-Hypothesis-Testing/Critical-Value-and-the-p-Value-Approach/index.html)

&nbsp

在这种 t 检验版本中，如果计算得到的 t 统计量太小或太大，则原假设被拒绝。

![数据科学家和分析师的统计学基础](../Images/86f71ad67a356adbdd43c72dd58f5948.png)

在这里，检验统计量与基于样本大小和选择的显著性水平的临界值进行比较。要确定确切的临界点值，可以使用[双侧 t 分布表](https://www.google.com/search?q=t-table+two+sided&client=safari&rls=en&sxsrf=ALeKk01KSlU3EEtBeMcXPuh13ud42kRCWw%3A1618592162824&tbm=isch&ictx=1&fir=ZGAb8l8KaBNJiM%252CZaqfSsY36WrUvM%252C_&vet=1&usg=AI4_-kSaUb_tv_3EBZQRhYaQVYYaJ1uBHQ&sa=X&ved=2ahUKEwjBtZrXnYPwAhWHgv0HHQPmASUQ9QF6BAgSEAE&biw=1981&bih=1044#imgrc=ZGAb8l8KaBNJiM)。

单侧或***单尾 t 检验***可以在假设检验*正向/负向*与*负向/正向*关系时使用，这类似于以下示例：

![数据科学家和分析师的统计学基础](../Images/6023ee462d8ae1addebe71ee2e413d57.png)

单侧 t 检验有一个***单一***的**拒绝区域**，**根据假设的方向，拒绝区域要么在左侧，要么在右侧，如下图所示：**

![数据科学家和分析师的统计学基础](../Images/5a203710504154ed893dfdf5b9b2a0f1.png)

图片来源：[*Hartmann, K., Krois, J., Waske, B. (2018)：E-Learning Project SOGA: Statistics and Geospatial Data Analysis. Earth Sciences Department, Freie Universität Berlin*](https://www.geo.fu-berlin.de/en/v/soga/Basics-of-statistics/Hypothesis-Tests/Introduction-to-Hypothesis-Testing/Critical-Value-and-the-p-Value-Approach/index.html)

在这种 t 检验版本中，如果计算得到的 t 统计量小于/大于临界值，则原假设被拒绝。

![数据科学家和分析师的统计学基础](../Images/f7a0144d365cdb2196b557b6bef94db2.png)

## F 检验

F 检验是另一种非常流行的统计检验，通常用于检验假设的*a****多个变量的联合统计显著性****。* 这种情况发生在你想检验多个自变量是否对因变量有统计学显著影响时。以下是一个可以通过 F 检验测试的统计假设示例：

![数据科学家和分析师的统计学基础](../Images/fbc0eb1db97a83aeee324ad8a43c6c22.png)

在原假设中，这三个变量对应的系数在统计上是联合不显著的，而备择假设中这三个变量在统计上是联合显著的。F检验的统计量遵循[F分布](https://en.wikipedia.org/wiki/F-distribution)，可以如下确定：

![数据科学家和分析师的统计基础](../Images/af793b8cf157d9d60b6ea78b0174224f.png)

其中，SSRrestricted是*限制模型*的**残差平方和**，即从数据中排除被原假设认为不显著的目标变量的模型；SSRunrestricted是*非限制模型*的残差平方和，即包含所有变量的模型，q表示在原假设下被联合测试为不显著的变量数量，N是样本量，k是非限制模型中的变量总数。SSR值在运行OLS回归后与参数估计一起提供，F统计量也是如此。以下是MLR模型输出的示例，其中标记了SSR和F统计量值。

![数据科学家和分析师的统计基础](../Images/49829bccc3f5b199826809dc36ff469c.png)

图片来源：[ Stock and Whatson](https://www.uio.no/studier/emner/sv/oekonomi/ECON4150/v18/lecture7_ols_multiple_regressors_hypothesis_tests.pdf)

F检验有**一个拒绝区域**，如下图所示：

![数据科学家和分析师的统计基础](../Images/5ac78a76d47b5b2683b991738f426636.png)

图片来源：[*密歇根大学*](https://www.statisticshowto.com/probability-and-statistics/f-statistic-value-test/)

如果计算的F统计量大于临界值，则可以拒绝原假设，这表明自变量在统计上是联合显著的。拒绝规则可以表示为：

![数据科学家和分析师的统计基础](../Images/b3e5f96af4f08fc272c08bb1119222e1.png)

# p值

另一种快速判断是否拒绝或支持原假设的方法是使用***p值***。p值是原假设下条件发生的概率。换句话说，p值是在原假设为真的情况下，观察到至少与检验统计量一样极端的结果的概率。p值越小，证据越强，建议可以拒绝原假设。

*p*-值的解释取决于选择的显著性水平。通常，1%、5%或10%的显著性水平用于解释p值。因此，除了使用t检验和F检验外，还可以使用这些检验统计量的p值来检验相同的假设。

下图展示了一个包含两个独立变量的OLS回归的示例输出。在此表中，t检验的p值测试了*class_size*变量参数估计的统计显著性，F检验的p值测试了*class_size*和*el_pct*变量参数估计的联合统计显著性，这些值被下划线标出。

![数据科学家和分析师的统计学基础](../Images/552f815ae74d8c19686206b9c540a0a3.png)

图片来源：[Stock and Whatson](https://www.uio.no/studier/emner/sv/oekonomi/ECON4150/v18/lecture7_ols_multiple_regressors_hypothesis_tests.pdf)

对于*class_size*变量的p值是0.011，当将此值与显著性水平1%或0.01、5%或0.05、10%或0.1进行比较时，可以得出以下结论：

+   0.011 > 0.01？t检验的零假设不能在1%的显著性水平下被拒绝。

+   0.011 < 0.05？t检验的零假设可以在5%的显著性水平下被拒绝。

+   0.011 < 0.10？t检验的零假设可以在10%的显著性水平下被拒绝。

所以，这个p值表明*class_size*变量的系数在5%和10%的显著性水平下是统计上显著的。对应于F检验的p值是0.0000，由于0小于所有三个截止值；0.01、0.05、0.10，我们可以得出结论，在这三种情况下都可以拒绝F检验的零假设。这表明*class_size*和*el_pct*变量的系数在1%、5%和10%显著性水平下共同具有统计学显著性。

## p值的局限性

尽管使用p值有许多好处，但它也有局限性**。**即p值取决于关联的强度和样本大小。如果效应的大小很小且统计上不显著，p值可能仍然显示***显著影响***，因为样本量很大。相反，如果样本量很小，一个效应可能很大，但仍然未能满足p<0.01、0.05或0.10的标准。

# 推论统计

推论统计使用样本数据对样本数据来源的总体做出合理的判断。它用于调查样本中变量之间的关系，并对这些变量如何与更大总体相关做出预测。

***大数法则 (LLN)***和***中心极限定理 (CLM)***在推论统计中具有重要作用，因为它们表明当数据量足够大时，实验结果无论原始总体分布的形状如何都能保持稳定。收集的数据越多，统计推断就越准确，从而生成的参数估计也越准确。

## 大数法则 (LLN)

假设**X1, X2, . . . , Xn**都是具有相同底层分布的独立随机变量，也称为独立同分布或i.i.d，其中所有的X具有相同的均值**?**和标准差**?**。随着样本数量的增加，所有X的平均值等于均值?的概率趋近于1。大数法则可以总结如下：

![数据科学和分析师的统计学基础](../Images/83dd325451ddc400712f84289a24bd9d.png)

## 中心极限定理（CLM）

假设**X1, X2, . . . , Xn**都是具有相同底层分布的独立随机变量，也称为独立同分布或i.i.d，其中所有的X具有相同的均值**?**和标准差**?**。随着样本量的增加，X的概率分布***在分布上收敛***于均值为**?**和方差为**?**平方的正态分布。中心极限定理可以总结如下：

![数据科学和分析师的统计学基础](../Images/fe3351c1c7395d7ce30440689669f3a6.png)

换句话说，当你有一个均值为?和标准差为?的总体，并且从这个总体中进行足够大的有放回随机抽样时，样本均值的分布将近似呈正态分布。

# 降维技术

降维是将数据从一个***高维空间***转换到一个***低维空间***，以便在这个低维表示中尽可能保留原始数据的有意义特性。

随着大数据的流行，这些降维技术的需求也增加了，这些技术可以减少不必要的数据和特征。流行的降维技术包括[主成分分析](https://builtin.com/data-science/step-step-explanation-principal-component-analysis)、[因子分析](https://en.wikipedia.org/wiki/Factor_analysis)、[典型相关分析](https://en.wikipedia.org/wiki/Canonical_correlation)和[随机森林](https://towardsdatascience.com/understanding-random-forest-58381e0602d2)。

## 主成分分析（PCA）

主成分分析（PCA）是一种降维技术，通常用于减少大数据集的维度，通过将大量变量转换为较少的变量集，同时仍保留原始大数据集中的大部分信息或变异。

假设我们有一个数据X，包含p个变量；X1, X2, …., Xp，具有***特征向量*** e1, …, ep 和 ***特征值*** ?1,…, ?p。特征值表示某一数据字段在总方差中所解释的方差。PCA的核心思想是创建新的（独立的）变量，称为主成分，它们是现有变量的线性组合。第i*个*主成分可以表示如下：

![数据科学与分析的统计基础](../Images/205398b1011b22a59b65953ecdfb2c36.png)

然后使用**肘部规则**或[**凯泽规则**](https://docs.displayr.com/wiki/Kaiser_Rule)，你可以确定最佳总结数据的主成分数量，而不会丢失过多信息。同样，查看***总方差比例（PRTV）*** **被每个主成分解释的比例也很重要，以决定是否有必要包含或排除它。第i*个*主成分的PRTV可以通过以下特征值计算：

![数据科学与分析的统计基础](../Images/c3e51300ce63864e1aae03a05b2a3a15.png)

## 肘部规则

肘部规则或肘部方法是一种启发式方法，用于确定PCA结果中的最佳主成分数量。这种方法的核心思想是将*解释的方差*作为主成分数量的函数绘制，并选择曲线的肘部作为最佳主成分的数量。以下是一个这样的散点图示例，其中PRTV（Y轴）与主成分数量（X轴）绘制。肘部对应于X轴值2，这表明最佳主成分的数量为2。

![数据科学与分析的统计基础](../Images/bd0053cd0dd490b12766da029b5773e6.png)

图片来源：[多变量统计Github](https://raw.githubusercontent.com/TatevKaren/Multivariate-Statistics/main/Elbow_rule_%25varc_explained.png)

## 因子分析（FA）

因子分析（FA）是另一种降维的统计方法。它是最常用的相互依赖技术之一，当相关变量集表现出系统性的相互依赖且目标是找出创建共同性的潜在因子时使用。假设我们有一个数据X，包含p个变量；X1, X2, …., Xp。FA模型可以表示如下：

![数据科学与分析的统计基础](../Images/5baf5518c565bfa78ab2313023f8929c.png)

其中X是一个[p x N]的p变量和N观测值的矩阵，µ是[p x N]的总体均值矩阵，A是[p x k]的共同***因子负荷矩阵***，F [k x N]是共同因子的矩阵，u [pxN]是特定因子的矩阵。换句话说，因子模型是一系列多重回归，通过不可观测的共同因子fi预测每一个变量Xi：

![数据科学家和分析师统计基础](../Images/90a98a161c579e57686e572a34cc0306.png)

每个变量都有自己的 k 个公共因子，这些因子通过因子载荷矩阵与单个观察值相关联：在因子分析中，***因子*** 是为了 ***最大化*** 组间方差而计算的，同时 ***最小化组内方差***。它们被称为因子，因为它们将潜在变量分组。与 PCA 不同，在 FA 中数据需要标准化，因为 FA 假设数据集遵循正态分布。

**[Tatev Karen Aslanyan](https://www.linkedin.com/in/tatev-karen-aslanyan/)** 是一位经验丰富的全栈数据科学家，专注于机器学习和人工智能。她还是 [LunarTech](https://lunartech.ai/course-overview/) 的联合创始人，该平台提供在线技术教育，并且是终极数据科学训练营的创建者。Tatev Karen 拥有经济计量学和管理科学的本科及硕士学位，在机器学习和人工智能领域取得了显著成长，专注于推荐系统和自然语言处理，并且有科学研究和发表的论文作为支持。经过五年的教学，Tatev 现在将她的热情投入到 LunarTech，帮助塑造数据科学的未来。

[原文](https://medium.com/towards-data-science/fundamentals-of-statistics-for-data-scientists-and-data-analysts-69d93a05aae7)。经许可转载。

### 相关主题

+   [数据分析师与数据科学家有什么区别？](https://www.kdnuggets.com/2022/03/difference-data-analysts-data-scientists.html)

+   [数据分析师的 SQL 和 Python 面试问题](https://www.kdnuggets.com/2023/02/sql-python-interview-questions-data-analysts.html)

+   [人工智能与数据分析师：影响分析未来的 6 大局限](https://www.kdnuggets.com/ai-vs-data-analysts-top-6-limitations-impacting-the-future-of-analytics)

+   [数据分析师必备工具全指南](https://www.kdnuggets.com/a-comprehensive-guide-to-essential-tools-for-data-analysts)

+   [使用 BigQuery ML 简化数据分析师的机器学习](https://www.kdnuggets.com/machine-learning-made-simple-for-data-analysts-with-bigquery-ml)

+   [世界需要更多网络安全分析师！](https://www.kdnuggets.com/the-world-needs-more-cyber-security-analysts)
