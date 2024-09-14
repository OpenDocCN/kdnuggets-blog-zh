# 使用一行代码进行统计和可视化探索性数据分析

> 原文：[https://www.kdnuggets.com/2020/09/statistical-visual-exploratory-data-analysis-one-line-code.html](https://www.kdnuggets.com/2020/09/statistical-visual-exploratory-data-analysis-one-line-code.html)

[评论](#comments)

**由[布伦达·哈利](https://twitter.com/brendahali)，市场数据专家**

![图片](../Images/05c95cdf3eaec35493337cdf9694deab.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

在我看来，探索性数据分析（EDA）是新数据集机器学习建模中最重要的部分。如果EDA执行不当，我们可能会用“未清理”的数据开始建模，这就像雪球一样，一开始小，之后变得越来越大，问题也越来越严重。

### 优秀探索性数据分析的基本元素

探索性数据分析可以根据你的需求深入进行，但基本分析需要包含以下元素：

+   首值和末值

+   数据集形状（#行和#列）

+   数据/变量类型

+   缺失值和空值

+   重复值

+   描述性统计（均值、最小值、最大值）

+   变量分布

+   相关性

我喜欢手动进行EDA以更好地了解我的数据，但几个月前，[Adi Bronshtein](https://medium.com/u/c82c464daf80?source=post_page-----9953638ea9d0--------------------------------) 向我介绍了Pandas Profiling。由于处理时间较长，我通常在想要快速探索小数据集时使用它，希望它也能加快你的EDA进程。

### 开始使用Pandas Profiling

在这个演示中，我将对[NASA的陨石降落数据集](https://data.nasa.gov/Space-Science/Meteorite-Landings/gh4g-9sfh)进行EDA。

你已经运行了吗？

***瞧，这很简单！***

![帖子图片](../Images/5b7cf4f5d14ffce9a0b9befac28f6b31.png)

现在有趣的部分开始了。

在他们的文档中了解更多关于Pandas Profiling的信息：[https://pandas-profiling.github.io/pandas-profiling/docs/](https://pandas-profiling.github.io/pandas-profiling/docs/)

你喜欢这篇文章吗？你可能会想查看[***最佳免费数据科学电子书***](https://towardsdatascience.com/the-best-free-data-science-ebooks-b671691e5231)***。***

**简介：[布伦达·哈利](https://twitter.com/brendahali)** (**[领英](https://www.linkedin.com/in/brenda-hali/)**) 是一位驻华盛顿特区的市场数据专家。她对女性在技术和数据领域的参与充满热情。

[原文](https://towardsdatascience.com/statistical-and-visual-exploratory-data-analysis-with-one-line-of-code-9953638ea9d0)。已获许可转载。

**相关：**

+   [在 3 分钟内理解偏差-方差权衡](/2020/09/understanding-bias-variance-trade-off-3-minutes.html)

+   [增强版的探索性数据分析](/2020/07/exploratory-data-analysis-steroids.html)

+   [用 D-Tale 让你的 Pandas 数据框活起来](/2020/08/bring-pandas-dataframes-life-d-tale.html)

### 更多相关话题

+   [关于掌握 SQL、Python、数据清洗、数据整理及探索性数据分析的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [针对非结构化数据的探索性数据分析技术](https://www.kdnuggets.com/2023/05/exploratory-data-analysis-techniques-unstructured-data.html)

+   [数据科学家必备的探索性数据分析指南](https://www.kdnuggets.com/2023/06/data-scientist-essential-guide-exploratory-data-analysis.html)

+   [掌握探索性数据分析的 7 个步骤](https://www.kdnuggets.com/7-steps-to-mastering-exploratory-data-analysis)

+   [数据编排：生成性 AI 成功与失败的分界线](https://www.kdnuggets.com/2024/07/astronomer/data-orchestration-the-dividing-line-between-generative-ai-success-and-failure)

+   [视觉 ChatGPT：微软将 ChatGPT 和 VFMs 结合起来](https://www.kdnuggets.com/2023/03/visual-chatgpt-microsoft-combine-chatgpt-vfms.html)
