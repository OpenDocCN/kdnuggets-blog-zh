# 贝叶斯优化与高斯过程的直观理解

> 原文：[https://www.kdnuggets.com/2018/10/intuitions-behind-bayesian-optimization-gaussian-processes.html](https://www.kdnuggets.com/2018/10/intuitions-behind-bayesian-optimization-gaussian-processes.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者 [Charles Brecque](https://www.linkedin.com/in/charles-brecque-96768397/)，Mind Foundry**

在某些应用中，目标函数的评估成本高或困难。在这些情况下，一般方法是创建一个更简单的目标函数代理模型，该模型更便宜，并用它来解决优化问题。此外，由于评估目标函数的高成本，通常推荐使用迭代方法。迭代优化器通过在域的一系列点上请求函数的评估来工作。贝叶斯优化通过在可能的目标函数空间上结合先验模型，将贝叶斯方法添加到迭代优化器范式中。本文介绍了贝叶斯优化与高斯过程的基本概念和直观理解，并介绍了 [OPTaaS](https://mindfoundry.ai/optaas)，一个用于贝叶斯优化的 [API](https://optaas.mindfoundry.ai/)。

### **优化**

优化方法尝试在一个域*** ????***中找到一个输入*** x**** ，使得一个函数*** f***的值在*** ????***范围内达到最大（或最小）：

![](../Images/b59b492614c9a5bca2ae7228fdf70de5.png)

一般优化框架

实际上，函数 *f*代表需要优化的过程的结果，比如交易策略的整体盈利性、工厂生产线上的质量控制指标，或具有许多参数和超参数的数据科学管道的性能。

输入域*** ????***表示需要优化的过程的有效参数选择。这些可以是交易策略中使用的市场预测器、工厂过程中的原材料数量，或数据科学管道中机器学习模型的参数。描述输入域*** ????***以及函数*** f***的属性共同定义了优化问题。过程域*** ????***的有效输入可以是离散的、连续的、有约束的或这些的任意组合。同样，结果函数*** f***可能是凸的、可微分的、多模态的、嘈杂的、变化缓慢的，或具有其他许多重要属性。

在某些应用中，目标函数的评估代价高（计算上或经济上），或者评估困难（化学实验、石油钻探）。在这些情况下，一般的方法是创建一个目标函数 ***f*** 的更简单的替代模型 ***f ̂***，它更便宜进行评估，并将用来解决优化问题。

此外，由于评估目标函数的成本较高，通常推荐使用迭代方法。迭代优化器通过在领域*x*1, *x*2, … ∈ ***????*** 的一系列点上迭代请求函数 *f* 的评估来工作。通过这些评估，优化器能够构建函数***f***的图像。对于梯度下降算法，这个图像是局部的，但对于替代模型方法，这个图像是全局的。无论何时，或者在预分配的函数评估预算结束时，迭代优化器都能够给出对***x***真实值的最佳近似。

替代模型使用***N***个已知的***f***评估值进行训练：***F =(f1, f2,…,fN )*** 以及 ***XN =(x1,x2,…,xN)***。构建替代模型的方法有很多，如多项式插值、神经网络、支持向量机、随机森林和高斯过程。在 Mind Foundry，我们首选的方法是使用高斯过程进行回归。

### **高斯过程**

高斯过程（GP）提供了一类丰富且灵活的非参数统计模型，适用于可以是连续的、离散的、混合的或甚至是层次性的函数空间。此外，GP 不仅提供关于 ***f*** 可能值的信息，还重要的是提供关于该值的不确定性的信息。

高斯过程回归的想法是，对于某些点的观察值***FN***以及点***XN***，我们假设这些值对应于具有先验分布的多变量高斯过程的实现：

![](../Images/5303443f71691ebbf61550e8347bd162.png)

其中 ***KN*** 是一个 ***N*x*N*** 协方差矩阵，其系数以相关函数（或核）***Kmn =K(xm,xn,θ)*** 的形式表达。核的超参数 ***θ*** 根据最大似然原则进行校准。***KN*** 的选择旨在反映函数的先验假设，因此核的选择将对回归的正确性产生重大影响。图 2 给出了几种协方差函数的示例。

通过数学变换和使用条件概率规则，可以估计后验分布 *p*（*f N+1*|*FN*, *XN+1*），并将***f ̂N+1*** 表达为 KN 和 FN 的函数，并带有不确定性。这使我们能够从观察结果中构建一个概率性替代模型，如图 1 所示：

![](../Images/7b2c70b77d4c10ce68363611c7cb8429.png)

![](../Images/2336ca1258c09ad25c08b8459c5bce70.png)

### **贝叶斯优化**

贝叶斯优化是一类迭代优化方法，专注于一般优化设置，其中有描述????，但对*f*的属性了解有限。贝叶斯优化方法的特征包括两个方面：

+   代理模型***f ̂***，用于函数*f*，

+   和一个计算自代理函数的***获取函数***，用于指导下一个评估点的选择。

BO通过在可能目标函数*f*的空间中加入***先验模型***，将贝叶斯方法学添加到迭代优化器范式中。每次报告函数评估时更新该模型，贝叶斯优化过程保持目标函数*f*的后验模型。这个后验模型是函数*f*的代理***f ̂***。带有GP先验的贝叶斯优化程序的伪代码如下：

**初始化**：

+   在*f*上设置高斯过程先验

+   根据初始空间填充实验设计，在*n*0点观察*f*。

+   设置*n*为*n*0

**当***n* ≤ *N***时：

+   使用所有可用数据更新f的后验概率分布

+   确定获取函数在????上的最大值点***xn***，其中获取函数是使用当前后验分布计算的

+   观察*yn* = *f*(*xn*)

+   增量*n*

**结束时**

**返回**评估值最大的*f*(*x*)点，或后验均值最大的点。

一个标准的获取函数示例是***期望改进准则***（EI），对于在*x* ∈ ????的任何给定点，是*f*在*x*处相对于到目前为止看到的***f***最佳值的预期改进，前提是函数***f***在*x*处确实高于到目前为止看到的***f***最佳值：因此，如果我们正在寻找***f***的最大值，EI可以写为：

***E I*(*x*) = ????(max(*f*(*x*) − *f**, 0))**

其中*f**是目前看到的*f*的最大值。

获取函数的额外示例如下：

+   熵搜索，旨在最小化我们对最优值位置的不确定性

+   上置信界

+   期望损失准则

图3展示了代理的演变及其与获取函数的交互，随着每次迭代的进行，它对试图最小化的底层函数的知识不断改善。

![](../Images/8ea46119fdccc1852d95bce04f6ddc78.png)

### **用OPTaaS操作化BO**

OPTaaS 是一个通用的贝叶斯优化器，通过网络服务提供最佳的参数配置。它可以处理任何类型的参数，并且不需要了解底层过程、模型或数据。它要求客户端指定参数及其范围，并返回每个 OPTaaS 推荐的参数配置的准确性评分。OPTaaS 利用这些评分来建模底层系统，并更快地搜索最佳配置。

Mind Foundry 在 OPTaaS 中实现了一组代理模型和获取函数，根据所提供的参数的性质和数量，自动选择和配置这些模型和函数，如图 4 所示。这种选择基于深入的科学测试和研究，以确保 OPTaaS 总是做出最合适的选择。此外，Mind Foundry 能够为客户特定的问题设计定制的协方差函数，这将显著提高优化过程的速度和准确性。大多数 OPTaaS 用户需要优化复杂的过程，这些过程运行成本高，并且反馈有限。因此，OPTaaS 的 API 专注于提供一个简单的迭代优化器接口。然而，如果对优化的过程有更多信息，它总是可以被利用以更快地收敛到最优解。因此，OPTaaS 也支持传递关于领域 ??? 的信息，例如对输入的约束，以及对函数 *f* 的评估，例如噪声、梯度或部分完成的评估。此外，客户通常能够利用本地基础设施来分布优化搜索，OPTaaS 也可以请求批量评估。

![](../Images/8ca0fff42e26280d2942c825cbbd4c56.png)

优化过程如下：

1.  OPTaaS 向客户推荐一个配置

1.  客户在他们的机器上评估配置

1.  客户发送回一个评分（准确性、夏普比率、投资回报率，……）

1.  OPTaaS 使用评分来更新其代理模型，循环重复，直到达到最佳配置。

在整个过程中，OPTaaS 不会访问底层数据或模型。有关 OPTaaS 的更多信息，请参见本页底部。

### **团队与资源**

Mind Foundry 是牛津大学的衍生公司，由斯蒂芬·罗伯茨和迈克尔·奥斯本教授创立，他们在数据分析方面拥有 35 年的经验。Mind Foundry 团队由超过 30 名世界级机器学习研究人员和精英软件工程师组成，其中许多人曾在牛津大学担任博士后。此外，Mind Foundry 通过其衍生公司身份，享有对超过 30 位牛津大学机器学习博士的特权访问。Mind Foundry 是牛津大学的投资组合公司，其投资者包括 [牛津科学创新](https://www.oxfordsciencesinnovation.com/)、[牛津科技与创新基金](http://www.oxfordtechnology.com/)、[牛津大学创新基金](https://innovation.ox.ac.uk/award-details/university-oxford-isis-fund-uoif/) 和 [Parkwalk Advisors](http://parkwalkadvisors.com/)。

**文档**

教程：[https://tutorial.optaas.mindfoundry.ai](https://tutorial.optaas.mindfoundry.ai/)

API 文档：[https://optaas.mindfoundry.ai](https://optaas.mindfoundry.ai/)

**研究**

[http://www.robots.ox.ac.uk/~mosb/projects/project/2009/01/01/bayesopt/](http://www.robots.ox.ac.uk/~mosb/projects/project/2009/01/01/bayesopt/)

**参考文献**

Osborne, M.A. (2010). 贝叶斯高斯过程用于顺序预测、优化和积分（博士论文）。博士论文，牛津大学。

**演示：**charles.brecque@mindfoundry.ai

**简介：[Charles Brecque](https://www.linkedin.com/in/charles-brecque-96768397/)** 是 Mind Foundry 的产品经理，负责 OPTaaS，一种通过网络服务部署的通用贝叶斯优化器。Mind Foundry 是牛津大学的衍生公司，由斯蒂芬·罗伯茨和迈克尔·奥斯本教授创立，他们在高级数据分析方面拥有超过 35 年的经验。

[原文](https://towardsdatascience.com/the-intuitions-behind-bayesian-optimization-with-gaussian-processes-7e00fcc898a0)。经许可转载。

**相关：**

+   [从零开始展开朴素贝叶斯](/2018/09/unfolding-naive-bayes.html)

+   [数据科学家优化 101](/2018/08/optimization-101-data-scientists.html)

+   [初学者问：“在人工神经网络中使用多少隐藏层/神经元？”](/2018/07/beginners-ask-how-many-hidden-layers-neurons-neural-networks.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多相关内容

+   [高斯朴素贝叶斯解释](https://www.kdnuggets.com/2023/03/gaussian-naive-bayes-explained.html)

+   [ChatGPT 的工作原理：聊天机器人背后的模型](https://www.kdnuggets.com/2023/04/chatgpt-works-model-behind-bot.html)

+   [稳定扩散：生成式人工智能的基本直觉](https://www.kdnuggets.com/2023/06/stable-diffusion-basic-intuition-behind-generative-ai.html)

+   [开始使用 LLMOps：无缝互动的秘诀](https://www.kdnuggets.com/getting-started-with-llmops-the-secret-sauce-behind-seamless-interactions)

+   [基于大语言模型的自主智能体的成长](https://www.kdnuggets.com/the-growth-behind-llmbased-autonomous-agents)

+   [使用 AIMET 进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)
