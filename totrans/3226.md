# 使用机器学习预测和解释员工流失

> 原文：[https://www.kdnuggets.com/2017/10/machine-learning-predict-employee-attrition.html](https://www.kdnuggets.com/2017/10/machine-learning-predict-employee-attrition.html)

**作者：Matt Dancho，[BusinessScience.io](http://www.business-science.io)**

员工流失（attrition）是组织面临的主要成本之一，预测流失是许多组织人力资源（HR）的迫切需求。直到现在，主流的方法是使用逻辑回归或生存曲线来建模员工流失。然而，随着机器学习（ML）的进步，我们现在可以获得更好的预测性能和对关键特征与员工流失之间关联的更好解释。在这篇文章中，我们将解释如何使用H2O的自动化机器学习功能来开发一个预测模型，该模型在机器学习准确度方面与商业产品相当；我们还将解释如何应用新的LIME包，该包能够将复杂的黑箱机器学习模型分解为变量重要性图。

### 问题：员工流失

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT需求

* * *

组织面临着因员工流失而产生的巨大成本。其中一些成本是可见的，比如培训费用和从员工开始工作到成为生产力成员所需的时间。然而，最重要的成本是无形的。考虑到当一名高效的员工离职时所失去的：新产品创意、出色的项目管理或客户关系。随着机器学习和数据科学的发展，现在不仅可以预测员工流失，还可以理解影响流失的关键变量。

我们使用了来自[IBM Watson 网站](https://www.ibm.com/communities/analytics/watson-analytics-blog/hr-employee-attrition/)的HR员工流失数据集来测试几种高级机器学习技术。该数据集包括1470名员工（行）和35个特征（列），其中一部分员工已离开组织（Attrition = “Yes”）。请注意，根据IBM的说法，“这是由IBM数据科学家创建的虚构数据集”。

![](../Images/dce1932ae5799f084228a10996c92cb4.png)

### 解决方案：H2O 和 LIME

我们实施的解决方案同时使用了H2O进行自动化机器学习和LIME用于理解和分解复杂的黑箱模型。我们将概述分析的亮点，但感兴趣的读者可以查看[包含代码的完整解决方案](http://www.business-science.io/business/2017/09/18/hr_employee_attrition.html)。

**使用`h2o`包中的`h2o.automl()`进行机器学习**：这个函数通过测试许多高级算法（如随机森林、集成方法和深度学习）以及更传统的算法（如逻辑回归），将自动化机器学习提升到了一个新的水平。主要的收获是，我们现在可以轻松实现与商业算法和ML/AI软件相当（在某些情况下甚至更好）的预测性能。

领导者模型（在验证集上产生的最准确模型）在一个未在建模过程中出现的测试集上达到了惊人的88%准确率。此外，二分类分析的召回率（算法预测离职 = “是”而实际离职的次数）为62%，这意味着人力资源专业人员可以准确地识别出100名被认为有风险的员工中的62名。在人力资源背景下，召回率非常重要，因为我们不想错过有风险的员工，而62%的表现相当不错。

**使用`lime`包的特征（变量）重要性**：像深度学习这样的高级机器学习算法的问题在于，由于其复杂性，几乎无法理解算法。`lime`包的出现改变了这一切。`lime`的主要进展在于，通过局部递归分析模型，它可以提取出全球重复的特征重要性。这对我们来说意味着，`lime`为理解机器学习模型打开了大门，无论其复杂性如何。现在，最好的（通常非常复杂的）模型也可以被调查并潜在地理解其变量或特征如何影响模型。 

我们投入使用LIME的回报是这个特征重要性图。展示了前十个案例（观察）的前四个特征。绿色条表示特征支持模型结论，红色条则相反。LIME检测到加班、职位角色和培训时间是与模型预测相关的特征。

![](../Images/c4ed686719d0766182c1ac6867f583a4.png)

![](../Images/476084e7034212b76de66607a93547bc.png)

我们随后分析了关键特征，以了解这些特征是否与员工流失相关。对于像加班和职位这样的特征，方差似乎有关联。我们可以看到，流失=“否”的组有更低的加班员工比例。此外，销售代表、实验室技术员和人力资源等职位的流失百分比较高。

![](../Images/f15bd2c284041ab5a8b790a62c33098f.png)

![](../Images/6f39e0b3cf58978d806acd3de3e6b750.png)

### 结论

新的机器学习技术可以应用于业务应用，特别是预测分析。在这种情况下，我们使用了H2O和LIME来开发和解释非常准确地检测员工流失风险的复杂模型。H2O的`h2o.autoML`函数在对未见/未建模数据进行分类时表现良好，准确率约为88%。LIME帮助将H2O返回的复杂集成模型分解成与流失相关的关键特征。

**个人简介：Matt Dancho** 是 [Business Science](http://www.business-science.io) 的创始人，这是一家帮助组织将数据科学应用于业务应用的咨询公司。他是R包`tidyquant`和`timetk`的创作者，并且在业务和金融分析的数据科学领域工作了六年。Matt拥有商业和工程硕士学位，并在商业智能、数据挖掘、时间序列分析、统计和机器学习方面拥有丰富的经验。

**相关：**

+   [HR经理如何利用数据科学来管理公司人才](/2017/06/hr-managers-data-science-manage-talent.html)

+   [每位员工的收入：黄金比例还是红鲱鱼？](/2017/01/revenue-per-employee.html)

+   [构建预测流失模型的挑战](/2017/03/datascience-building-predictive-churn-model.html)

### 主题更多

+   [停止学习数据科学以寻找目标，找到目标去…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学学习统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个90亿美元的AI失败，审视](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功的数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
