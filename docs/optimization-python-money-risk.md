# 使用 Python 优化：如何以最小的风险赚取最多的利润？

> 原文：[https://www.kdnuggets.com/2019/06/optimization-python-money-risk.html](https://www.kdnuggets.com/2019/06/optimization-python-money-risk.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

### 介绍

现代数据科学和分析企业的主要目标之一是[为商业和技术公司解决复杂的优化问题](https://www.quora.com/Is-optimization-related-to-data-science-And-how)以最大化他们的利润。

在我的文章“[使用 Python 的线性规划和离散优化](https://towardsdatascience.com/linear-programming-and-discrete-optimization-with-python-using-pulp-449f3c5f6e99)”中，我们涉及了基本的离散优化概念，并介绍了一个用于解决此类问题的[Python 库 PuLP](https://pythonhosted.org/PuLP/)。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

尽管[线性规划 (LP) 问题](https://www.analyticsvidhya.com/blog/2017/02/lintroductory-guide-on-linear-programming-explained-in-simple-english/)仅由线性目标函数和约束定义，但它可以应用于从医疗保健到经济学、商业到军事等各种不同领域的惊人广泛的问题。

在这篇文章中，我们展示了一个使用 Python 编程的线性规划的惊人应用——***在最小化相关风险的同时最大化股市投资组合的预期利润***。

听起来有趣吗？请继续阅读。

### 如何在股市中最大化利润并最小化风险？

1990 年诺贝尔经济学奖颁给了[哈里·马科维茨](https://en.wikipedia.org/wiki/Harry_Markowitz)，他因其著名的[现代投资组合理论 (MPT)](https://www.investopedia.com/terms/m/modernportfoliotheory.asp)而受到认可。该理论的原始论文早在1952年就已发表。

![](../Images/d506899d2253d2512edd071a9d356993.png)

**来源**：AZ Quotes

这里的关键字是**平衡**。

一个好的平衡投资组合必须提供**保护**（最小化风险）和**机会**（最大化利润）。

当涉及到最小化和最大化等概念时，将问题用[**数学优化理论**](https://en.wikipedia.org/wiki/Mathematical_optimization)来表述是很自然的。

> 基本思想相当简单，根植于人类固有的厌恶风险的天性中。

一般来说，股市统计数据显示，较高的风险与更高回报的概率相关，而较低的风险与较低回报的概率相关。

[MPT假设投资者是厌恶风险的](https://www.investopedia.com/ask/answers/041615/how-risk-aversion-measured-modern-portfolio-theory-mpt.asp)，**即在两个投资组合提供相同预期回报的情况下，投资者将更偏好于风险较小的那个**。想一想，只有在高风险股票具有较高回报概率时，你才会选择它们。

**但如何量化风险**？这确实是一个模糊的概念，对不同的人可能有不同的含义。然而，在普遍接受的经济理论中，[股票价格的变动性（波动性）（在固定时间范围内定义）等同于风险](https://www.investopedia.com/ask/answers/041415/variance-good-or-bad-stock-investors.asp)。

> 因此，核心优化问题是最小化风险，同时确保一定的利润回报。或者，在保持风险低于某个阈值的情况下最大化利润。

![](../Images/6c736d82561c89a9ad169724c194efc2.png)

### 一个示例问题

在本文中，我们将展示投资组合优化问题的一个非常简化的版本，该版本可以转化为LP框架，并通过简单的Python脚本高效解决。

目标是展示这些优化求解器在解决复杂现实问题中的能力和可能性。

我们使用了三只股票的24个月股价（月均值）——微软、Visa、沃尔玛。这些数据较旧，但完美展示了这个过程。

![](../Images/720c6452c7fc5a56df3b8e439965c5a6.png)

图：三家公司在某个24个月期间的月度股价。

**如何定义回报**？我们可以通过从当前月份的平均股价中减去前一个月的平均股价，并除以前一个月的价格来简单地计算滚动的月度回报。

![](../Images/26abe6ba36d4edba2b60e21f78df92b4.png)

回报如图所示，

![](../Images/eadd3e7e8a284178646ab64be317dc36.png)

### **优化模型**

股票的回报是一个不确定的量。我们可以将其建模为**随机向量**。

![](../Images/ade5c0830ca773d434fd0773c4b0cff4.png)

投资组合也可以建模为一个向量。

![](../Images/81315a9e4f4e079587e91afd738532a5.png)

因此，某一投资组合的回报由这些向量的内积给出，它是一个随机变量。百万美元的问题是：

*我们如何比较随机变量（对应不同投资组合）以选择“最佳”投资组合？*

按照 Markowitz 模型，我们可以将问题表述为，

> 给定固定金额的钱（比如 $1000），我们应该如何在三只股票中进行投资，以便 (a) 一个月的期望回报至少达到给定的阈值，并且 (b) 最小化投资组合回报的风险（方差）。

我们不能投资负的数量。这是**非负性约束**，

![](../Images/18af0c8e415bdce8996bc1c00c925ccb.png)

假设没有交易成本，总投资受限于现有资金，

![](../Images/c90c073d616b61282f9f595b6cb8971c.png)

投资回报，

![](../Images/24f1fa970ddda72ec17322791cb68e30.png)

但这是一个**随机变量**。所以，我们必须使用**期望数量**，

![](../Images/df02ca87f2c93adc19718e04ee812c58.png)

假设我们想要一个**最小期望回报**。因此，

![](../Images/e1a5c32e6e9072b2b9c767d985b2e411.png)

现在，为了建模风险，我们必须计算**方差**，

![](../Images/7e04ba36ef1dba66aae8e917c90c3d1b.png)

总结起来，最终的优化模型是，

![](../Images/005e68756eb67762c7558693a27c7601.png)

接下来，我们展示使用流行的 Python 库来制定和解决这个问题是多么简单。

### 使用 Python 来解决优化问题：CVXPY

我们将使用的库是叫做[**CVXPY**](https://www.cvxpy.org/index.html)的。它是一个嵌入 Python 的凸优化问题建模语言。它允许你以**自然的方式表达你的问题**，遵循数学模型，而不是求解器要求的限制性标准形式。

[**完整代码见此 Jupyter notebook**](https://github.com/tirthajyoti/Optimization-Python/blob/master/Portfolio_optimization.ipynb)。这里，我只展示核心代码片段。

为了设置必要的数据，关键是从每月价格的数据表中计算回报矩阵。代码如下，

```py
import numpy as np
import pandas as pd
from cvxpy import *

mp = pd.read_csv("monthly_prices.csv",index_col=0)
mr = pd.DataFrame()

# compute monthly returns
for s in mp.columns:
    date = mp.index[0]
    pr0 = mp[s][date] 
    for t in range(1,len(mp.index)):
        date = mp.index[t]
        pr1 = mp[s][date]
        ret = (pr1-pr0)/pr0
        mr.set_value(date,s,ret)
        pr0 = pr1

```

现在，如果你将原始数据表和回报表并排查看，它看起来如下，

![](../Images/72f14cb1e6b69aa49a982d959f57536d.png)

接下来，我们简单地从这个回报矩阵中计算均值（期望）回报和协方差矩阵，

```py
# Mean return
r = np.asarray(np.mean(return_data, axis=1))

# Covariance matrix
C = np.asmatrix(np.cov(return_data))

```

之后，CVXPY 允许按照我们上面构建的数学模型简单地设置问题，

```py
# Get symbols
symbols = mr.columns

# Number of variables
n = len(symbols)

# The variables vector
x = Variable(n)

# The minimum return
req_return = 0.02

# The return
ret = r.T*x

# The risk in xT.Q.x format
risk = quad_form(x, C)

# The core problem definition with the Problem class from CVXPY
prob = Problem(Minimize(risk), [sum(x)==1, ret >= req_return, x >= 0])

```

注意使用 CVXPY 框架中非常有用的类，如**quad_form()** 和 **Problem()**。

完成了！

我们可以编写一个简单的代码来解决**问题**并展示确保最小回报为 2% 的最佳投资数量，同时将风险保持在最低。

```py
try:
    prob.solve()
    print ("Optimal portfolio")
    print ("----------------------")
    for s in range(len(symbols)):
       print (" Investment in {} : {}% of the portfolio".format(symbols[s],round(100*x.value[s],2)))
    print ("----------------------")
    print ("Exp ret = {}%".format(round(100*ret.value,2)))
    print ("Expected risk    = {}%".format(round(100*risk.value**0.5,2)))
except:
    print ("Error")

```

最终结果为，

![](../Images/e30f99b940ad33ec5704135a5d11488f.png)

### 扩展问题

不用多说，我们模型的设置和简化假设可能会让这个问题看起来比实际更简单。但一旦你理解了解决这种优化问题的基本逻辑和机制，你就可以将其扩展到多种场景中，

+   数以百计的股票，更长的时间跨度数据

+   多重风险/收益比率和阈值

+   最小化风险或最大化回报（或两者兼得）

+   一组公司的共同投资

+   二选一的场景——投资于可口可乐或百事可乐，但不能同时投资于两者

你需要构建更复杂的矩阵和更长的约束列表，使用指示变量将其转化为一个 [混合整数问题](https://www.solver.com/integer-constraint-programming) **-** 但所有这些问题都被像CVXPY这样的包本质上支持。

[查看CVXPY包的示例页面](https://www.cvxpy.org/examples/index.html) 以了解使用该框架可以解决的优化问题的广度。

### 摘要

在这篇文章中，我们讨论了如何利用经典经济理论中的关键概念来制定一个简单的股票市场投资优化问题。

为了说明，我们以三家公司每月平均股价的样本数据集为例，展示了如何使用NumPy、Pandas等基础Python数据科学库以及一个叫做CVXPY的优化框架快速建立线性规划模型。

> 对这些灵活且强大的包有工作知识，对未来的数据科学家来说具有极大的价值，因为在科学、技术和商业问题的各个方面都需要解决优化问题。

鼓励读者尝试更复杂的投资问题版本，以便享受乐趣和学习。

[原文](https://towardsdatascience.com/optimization-with-python-how-to-make-the-most-amount-of-money-with-the-least-amount-of-risk-1ebebf5b2f29)。转载许可。

**简介：** [Tirthajyoti Sarkar](https://www.linkedin.com/in/tirthajyoti-sarkar-2127aa7/) 是ON Semiconductor的高级首席工程师，负责基于深度学习/机器学习的设计自动化项目。

**相关内容：**

+   [定量金融与风险管理中的数据分析模型](https://www.kdnuggets.com/2016/12/data-analytics-models-quantitative-finance-risk-management.html)

+   [优化如何工作](https://www.kdnuggets.com/2019/04/how-optimization-works.html)

+   [使用PuLP进行线性编程和离散优化](https://www.kdnuggets.com/2019/05/linear-programming-discrete-optimization-python-pulp.html "使用PuLP进行线性编程和离散优化")

### 更多相关内容

+   [每位数据科学家至少犯过一次的错误](https://www.kdnuggets.com/2022/09/mistake-every-data-scientist-made-least.html)

+   [谁将从生成性AI的金矿中赚钱？](https://www.kdnuggets.com/2023/08/make-money-generative-ai-gold-rush.html)

+   [利用ChatGPT和AI赚钱的3种方法](https://www.kdnuggets.com/3-ways-to-make-money-with-chatgpt-and-ai)

+   [超参数优化：10个顶尖的Python库](https://www.kdnuggets.com/2023/01/hyperparameter-optimization-10-top-python-libraries.html)

+   [AI/ML 模型的风险管理框架](https://www.kdnuggets.com/2022/03/risk-management-framework-aiml-models.html)

+   [TPOT 的机器学习管道优化](https://www.kdnuggets.com/2021/05/machine-learning-pipeline-optimization-tpot.html)
