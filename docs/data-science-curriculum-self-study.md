# 自学数据科学课程

> 原文：[`www.kdnuggets.com/2020/02/data-science-curriculum-self-study.html`](https://www.kdnuggets.com/2020/02/data-science-curriculum-self-study.html)

评论

![](img/29a357573d91d14f811985f816dcd053.png)

*照片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。*

作为数据科学教育者，许多对数据科学感兴趣的人联系我寻求如何进入数据科学领域的指导。本文将讨论建立数据科学必备技能所需学习的推荐主题。

这里提出的主题，如果深入学习，将提供开始从事数据科学所需的最基本背景。这个课程大纲也可以用于设计数据科学入门级的大学课程。

*请记住，仅仅通过课程获得的知识并不会使你成为数据科学家。课程学习必须配合一个顶点项目或实习。Kaggle 比赛可以用作顶点项目，因为它们提供了在真实数据科学项目上工作的机会。*

以下列表呈现了学习入门数据科学的基本主题。

### 1\. 数学基础

**（I）多变量微积分**

大多数机器学习模型是基于具有多个特征或预测变量的数据集构建的。因此，熟悉多变量微积分对于构建机器学习模型非常重要。你需要熟悉以下主题：

+   多变量函数

+   导数和梯度

+   步骤函数、Sigmoid 函数、Logit 函数、ReLU（整流线性单元）函数

+   代价函数

+   函数绘制

+   函数的最小值和最大值

**（II）线性代数**

线性代数是机器学习中最重要的数学技能。数据集表示为矩阵。线性代数用于数据预处理、数据转换和模型评估。你需要熟悉以下主题：

+   向量

+   矩阵

+   矩阵的转置

+   矩阵的逆

+   矩阵的行列式

+   点积

+   特征值

+   特征向量

**（III）优化方法**

大多数机器学习算法通过最小化目标函数来执行预测建模，从而学习必须应用于测试数据的权重以获得预测标签。你需要熟悉以下主题：

+   代价函数/目标函数

+   似然函数

+   误差函数

+   梯度下降算法及其变体（例如，随机梯度下降算法）

### 2\. 编程基础

Python 和 R 被认为是数据科学领域的顶级编程语言。你可以选择只专注于一种语言。Python 在工业界和学术培训项目中被广泛采用。作为初学者，建议你只专注于一种语言。

这里是一些需要掌握的 Python 和 R 基础主题：

+   基础 R 语法

+   R 编程的基础概念，如数据类型、向量运算、索引和数据框

+   如何在 R 中执行操作，包括排序、使用 dplyr 进行数据处理，以及使用 ggplot2 进行数据可视化

+   R studio

+   Python 的面向对象编程方面

+   Jupyter notebooks

+   能够使用 Python 库，如 NumPy、pylab、seaborn、matplotlib、pandas、scikit-learn、TensorFlow、PyTorch

### **3. 数据基础**

学习如何处理各种格式的数据，例如，CSV 文件、pdf 文件、文本文件等。学习如何清理数据、填补数据、缩放数据、导入和导出数据，以及从互联网抓取数据。一些相关的包包括 pandas、NumPy、pdf tools、stringr 等。此外，R 和 Python 包含多个内置数据集，可用于练习。学习数据转换和降维技术，如协方差矩阵图、主成分分析（PCA）和线性判别分析（LDA）。

### 4. 概率与统计基础

统计和概率用于特征可视化、数据预处理、特征转换、数据填补、降维、特征工程、模型评估等。以下是你需要熟悉的主题：

+   平均数

+   中位数

+   众数

+   标准差/方差

+   相关系数和协方差矩阵

+   概率分布（二项分布、泊松分布、正态分布）

+   p 值

+   贝叶斯定理（精确度、召回率、正预测值、负预测值、混淆矩阵、ROC 曲线）

+   A/B 测试

+   蒙特卡罗模拟

### 5. 数据可视化基础

学习良好数据可视化的基本组件。良好的数据可视化由几个组件组成，这些组件必须组合在一起才能产生最终产品。

a) **数据组件：** 决定如何可视化数据的一个重要第一步是了解数据的类型，例如，分类数据、离散数据、连续数据、时间序列数据等。

b) **几何组件：** 在这里你决定什么样的可视化适合你的数据，例如，散点图、折线图、柱状图、直方图、Q-Q 图、平滑密度图、箱线图、配对图、热图等。

c) **映射组件：** 在这里，你需要决定使用哪个变量作为 x 变量，哪个作为 y 变量。这一点尤为重要，特别是当你的数据集是多维的，具有多个特征时。

d) **尺度组件：** 在这里，你决定使用什么样的尺度，例如，线性尺度、对数尺度等。

e) **标签组件：** 这包括坐标轴标签、标题、图例、使用的字体大小等。

f) **伦理组件：** 在这里，你需要确保你的可视化展示了真实的故事。你需要在清理、总结、操控和生成数据可视化时保持意识，确保你的可视化不会误导或操控你的观众。

重要的数据可视化工具包括 Python 的 matplotlib 和 seaborn 包，以及 R 的 ggplot2 包。

### 6. 线性回归基础

学习简单和多重线性回归分析的基础知识。线性回归用于具有连续结果的监督学习。执行线性回归的一些工具如下所示：

Python：NumPy、pylab、sci-kit-learn

R：caret 包

### 7. 机器学习基础

**a) 监督学习（连续变量预测）**

+   基础回归

+   多重回归分析

+   正则化回归

**b) 监督学习（离散变量预测）**

+   逻辑回归分类器

+   支持向量机（SVM）分类器

+   K 最近邻（KNN）分类器

+   决策树分类器

+   随机森林分类器

+   朴素贝叶斯

**c) 无监督学习**

+   K 均值聚类算法

机器学习的 Python 工具：Scikit-learn、Pytorch、TensorFlow。

### 8. 时间序列分析基础

用于预测模型的时间依赖性场景，例如预测股票价格。分析时间序列数据有 3 种基本方法：

+   指数平滑

+   ARIMA（自回归积分滑动平均），是指数平滑的推广

+   GARCH（广义自回归条件异方差性），是一种用于分析方差的类似 ARIMA 的模型。

这三种技术可以在 Python 和 R 中实现。

### 9. 生产力工具基础

掌握如何使用基本的生产力工具，如 R studio、Jupyter notebook 和 GitHub 至关重要。对于 Python，Anaconda Python 是最佳的生产力工具。高级生产力工具，如 AWS 和 Azure，也是需要学习的重要工具。

### 10. 数据科学项目规划基础

学习如何规划项目的基础知识。在构建任何机器学习模型之前，重要的是仔细规划你希望模型实现的目标。在深入编写代码之前，重要的是了解待解决的问题、数据集的性质、要构建的模型类型、如何训练、测试和评估模型。项目规划和组织对提高数据科学项目的生产力至关重要。以下提供了一些项目规划和组织的资源。

### 数据科学自学的有用资源

[机器学习的基本数学技能](https://medium.com/towards-artificial-intelligence/4-math-skills-for-machine-learning-12bfbc959c92)

[3 个最佳数据科学 MOOC 专门化课程](https://medium.com/towards-artificial-intelligence/3-best-data-science-mooc-specializations-d58da382f628)

[进入数据科学的 5 个最佳学位](https://towardsdatascience.com/5-best-degrees-for-getting-into-data-science-c3eb067883b1)

[2020 年你应该开始数据科学之旅的 5 个理由](https://towardsdatascience.com/5-reasons-why-you-should-begin-your-data-science-journey-in-2020-2b4a0a5e4239)

[数据科学的理论基础——我应该关心还是仅仅专注于实际技能？](https://towardsdatascience.com/theoretical-foundations-of-data-science-should-i-care-or-simply-focus-on-hands-on-skills-c53fb0caba66)

[机器学习项目规划](https://towardsdatascience.com/machine-learning-project-planning-71bdb3a44349)

[如何组织你的数据科学项目](https://towardsdatascience.com/how-to-organize-your-data-science-project-dd6599cf000a)

[大规模数据科学项目的生产力工具](https://medium.com/towards-artificial-intelligence/productivity-tools-for-large-scale-data-science-projects-64810dfbb971)

[数据可视化的艺术——使用 Matplotlib 和 Ggplot2 进行天气数据可视化](https://medium.com/p/4d4b48b5b7c4?source=post_stats_page---------------------------)

[使用协方差矩阵图进行特征选择和降维](https://medium.com/towards-artificial-intelligence/feature-selection-and-dimensionality-reduction-using-covariance-matrix-plot-b4c7498abd07)

[数据科学 101——包含 R 和 Python 代码的简短课程](https://medium.com/towards-artificial-intelligence/data-science-101-a-short-course-on-medium-platform-with-r-and-python-code-included-3cdc9d489c6d)

[原文](https://medium.com/towards-artificial-intelligence/data-science-curriculum-bf3bb6805576)。已获许可转载。

**相关：**

+   [如何自学数据科学：实用指南](https://www.kdnuggets.com/2020/02/learn-data-science-guide.html)

+   [数据科学课程路线图](https://www.kdnuggets.com/2019/12/data-science-curriculum-roadmap.html)

+   [数据科学方法有什么问题？](https://www.kdnuggets.com/2019/07/whats-wrong-with-data-science.html)

### 更多相关话题

+   [停止学习数据科学以寻找目标，找到目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学最低要求：你需要知道的 10 项基本技能](https://www.kdnuggets.com/2020/10/data-science-minimum-10-essential-skills.html)

+   [KDnuggets™ 新闻 22:n06，2 月 9 日：数据科学编程…](https://www.kdnuggets.com/2022/n06.html)

+   [数据科学定义幽默：一些古怪的名言集锦](https://www.kdnuggets.com/2022/02/data-science-definition-humor.html)

+   [5 个数据科学项目，学习 5 项关键数据科学技能](https://www.kdnuggets.com/2022/03/5-data-science-projects-learn-5-critical-data-science-skills.html)

+   [KDnuggets 新闻，11 月 30 日：什么是切比雪夫定理及其应用](https://www.kdnuggets.com/2022/n46.html)
