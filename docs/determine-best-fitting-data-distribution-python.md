# 如何使用Python确定最佳拟合数据分布

> 原文：[https://www.kdnuggets.com/2021/09/determine-best-fitting-data-distribution-python.html](https://www.kdnuggets.com/2021/09/determine-best-fitting-data-distribution-python.html)

有时你在分析之前就知道数据的最佳拟合分布或概率密度函数；但更多时候，你并不知道。数据采样、建模和分析的方法可以根据数据的分布而有所不同，因此确定最佳的理论分布可以成为数据探索过程中的一个重要步骤。

这时，[distfit](https://github.com/erdogant/distfit) 就派上用场了。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行IT工作

* * *

> `distfit` 是一个用于通过残差平方和（RSS）和假设检验，对89种单变量分布进行概率密度拟合的Python包。概率密度拟合是将概率分布拟合到一系列数据上，这些数据涉及对变量现象的重复测量。`distfit` 为89种不同的分布中的每一种打分，以评估与经验分布的拟合情况，并返回得分最高的分布。

本质上，我们可以将数据传递给distfit，它会基于RSS指标，确定数据最适合哪种概率分布，这在它尝试将数据拟合到89种不同的分布后进行。然后，我们可以查看经验数据与最佳拟合分布的可视化叠加图。我们还可以查看过程的摘要，并绘制最佳拟合结果。

让我们看看如何使用distfit来完成这些任务，并了解使用它是多么简单。

首先，我们将生成一些数据；初始化distfit模型；并将数据拟合到模型中。这是distfit分布拟合过程的核心。

```py
import numpy as np
from distfit import distfit

# Generate 10000 normal distribution samples with mean 0, std dev of 3 
X = np.random.normal(0, 3, 10000)

# Initialize distfit
dist = distfit()

# Determine best-fitting probability distribution for data
dist.fit_transform(X)
```

而distfit正在默默工作：

```py
[distfit] >fit..
[distfit] >transform..
[distfit] >[norm      ] [0.00 sec] [RSS: 0.0004974] [loc=-0.002 scale=3.003]
[distfit] >[expon     ] [0.00 sec] [RSS: 0.1595665] [loc=-12.659 scale=12.657]
[distfit] >[pareto    ] [0.99 sec] [RSS: 0.1550162] [loc=-7033448.845 scale=7033436.186]
[distfit] >[dweibull  ] [0.28 sec] [RSS: 0.0027705] [loc=-0.001 scale=2.570]
[distfit] >[t         ] [0.25 sec] [RSS: 0.0004996] [loc=-0.002 scale=2.994]
[distfit] >[genextreme] [0.64 sec] [RSS: 0.0010127] [loc=-1.105 scale=3.007]
[distfit] >[gamma     ] [0.39 sec] [RSS: 0.0005046] [loc=-268.356 scale=0.034]
[distfit] >[lognorm   ] [0.84 sec] [RSS: 0.0005159] [loc=-227.504 scale=227.485]
[distfit] >[beta      ] [0.33 sec] [RSS: 0.0005016] [loc=-2746819.537 scale=2747059.862]
[distfit] >[uniform   ] [0.00 sec] [RSS: 0.1103102] [loc=-12.659 scale=22.811]
[distfit] >[loggamma  ] [0.73 sec] [RSS: 0.0005070] [loc=-554.304 scale=83.400]
[distfit] >Compute confidence interval [parametric]
```

一旦完成，我们可以通过几种不同的方式检查结果。首先，让我们看看生成的摘要。

```py
# Print summary of evaluated distributions
print(dist.summary)
```

```py
         distr        score  LLE          loc        scale  \
0         norm  0.000497419  NaN  -0.00231781      3.00297   
1            t  0.000499624  NaN  -0.00210365      2.99368   
2         beta  0.000501588  NaN -2.74682e+06  2.74706e+06   
3        gamma  0.000504569  NaN     -268.356    0.0336241   
4     loggamma  0.000506987  NaN     -554.304      83.3997   
5      lognorm   0.00051591  NaN     -227.504      227.485   
6   genextreme   0.00101271  NaN     -1.10495      3.00708   
7     dweibull   0.00277053  NaN  -0.00114977      2.56974   
8      uniform      0.11031  NaN      -12.659      22.8107   
9       pareto     0.155016  NaN -7.03345e+06  7.03344e+06   
10       expon     0.159567  NaN      -12.659      12.6567   

                                       arg  
0                                       ()  
1                     (323.7785925192121,)  
2   (73202573.87887828, 6404.720016859684)  
3                     (7981.006169767873,)  
4                     (770.4368441223046,)  
5                  (0.013180038142300822,)  
6                   (0.25551845624380576,)  
7                    (1.2640245435189867,)  
8                                       ()  
9                     (524920.1083231748,)  
10                                      () 
```

基于RSS（即上述摘要中的“得分”），并且记住最低的RSS将提供最佳拟合，我们的数据最适合正态分布。

这可能看起来是一个不言而喻的结论，鉴于我们从正态分布中抽样，但事实并非如此。鉴于众多分布之间的相似性，对重新抽样数据的连续运行使用正态分布，显示出得到t、gamma、beta、log-normal或log-gamma等最佳拟合分布是一样容易的（也许更容易？），尤其是在相对较小的样本量下。

然后我们可以将最佳拟合分布的结果与我们的实证数据进行绘图。

```py
# Plot results
dist.plot()
```

![图片](../Images/db9c30d03bd63d72fde3699ac6becd53.png)

最终，我们可以绘制最佳拟合总结，直观地比较分布拟合的情况。

![图片](../Images/ac00a4049e01d5b7db1f6ca935b5c6d9.png)

从以上内容可以看出几个最佳拟合分布相对的优劣。

除了分布拟合，distfit还有其他应用场景：

> `distfit`函数有许多应用场景。首先是确定数据的最佳理论分布。这可以将数万个数据点简化为3个浮动参数。另一个应用是异常值检测。可以使用正常状态来确定零分布。显著偏离的新的数据点可以被标记为异常值，可能具有兴趣。零分布也可以通过随机化/置换方法生成。在这种情况下，如果新数据点与随机性显著偏离，将被标记出来。

你可以在[distfit文档](https://erdogant.github.io/distfit/pages/html/Examples.html)中找到这些应用场景的示例。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家及KDnuggets的主编，KDnuggets是开创性的在线数据科学和机器学习资源。他的兴趣在于自然语言处理、算法设计与优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过editor1 at kdnuggets[dot]com联系。

### 更多相关主题

+   [正态分布综合指南](https://www.kdnuggets.com/2022/06/comprehensive-guide-normal-distribution.html)

+   [数据科学的5个Python最佳实践](https://www.kdnuggets.com/5-python-best-practices-for-data-science)

+   [最佳Python课程：分析总结](https://www.kdnuggets.com/2022/01/best-python-courses-analysis-summary.html)

+   [构建生成式AI应用的最佳Python工具备忘单](https://www.kdnuggets.com/2023/08/best-python-tools-generative-ai-cheat-sheet.html)

+   [6个最佳免费在线Python课程，提升你的职业生涯](https://www.kdnuggets.com/2022/11/corise-6-best-free-online-courses-python-boost-career.html)

+   [8种最佳 Python 图像处理工具](https://www.kdnuggets.com/2022/11/8-best-python-image-manipulation-tools.html)
