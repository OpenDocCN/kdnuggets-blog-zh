# 数据科学家职位薪资分析

> 原文：[`www.kdnuggets.com/2023/04/data-scientist-job-salaries-analysis.html`](https://www.kdnuggets.com/2023/04/data-scientist-job-salaries-analysis.html)

![数据科学家职位薪资分析](img/5f5902bc6ca66594be2b921fe349e805.png)

图片来源：[Tima Miroshnichenko](https://www.pexels.com/photo/a-laptop-near-the-dollars-and-papers-on-a-wooden-table-6693655/)

数据科学和机器学习在运动、艺术、空间、医学、医疗保健等多个领域越来越受到关注。了解这些数据科学家在全球不同地区的薪资和就业现状将会很有启发性。

* * *

## 我们的前三课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT 工作

* * *

数据集下载自 Kaggle（链接见下方），我们将对数据进行探索性分析和可视化。[`www.kaggle.com/datasets/ruchi798/data-science-job-salaries`](https://www.kaggle.com/datasets/ruchi798/data-science-job-salaries)

数据集根据经验水平分为以下几类：

1.  EN: 入门级

1.  MI: 中级

1.  SE: 高级

1.  EX: 高级管理层

数据集根据就业类型分为以下几类：

1.  FT: 全职

1.  PT: 兼职

1.  CT: 合同制

1.  FL: 自由职业者

数据集根据公司规模分为以下几类：

1.  S: 小型

1.  M: 中级

1.  L: 大型

## 探索性分析与可视化

在本节中，我们将对给定的数据集进行探索性数据分析和可视化。以下项目在议程上：

1.  经验水平的分布

1.  工作类型的分布

1.  基于经验水平的数据科学职位薪资比较

1.  基于就业类型的数据科学职位薪资比较

1.  基于经验水平和公司规模的薪资比较

1.  比较全球数据科学家的薪资

1.  平均薪资与货币的关系

1.  平均薪资与地点的关系

1.  前 10 名数据科学职位

1.  远程工作状态与时间的关系

数据集中，14.5%的成员为应届生，而大部分名额由高级工程师填补，占 46.1%。

在 2020 至 2022 年期间，62.8%的成员因 Covid-19 危机转为居家工作模式。稍后，我们将看到这一趋势回归正常。

自然可以看出，经验越丰富，薪资也越高。然而，在最高的执行级别，薪资的变动幅度明显大于其他级别。

看起来合同制工作在所有类型的工作中收入最高，尽管其薪资变动幅度也很大。一个有趣的观察是，自由职业者的收入高于兼职工作者，但其薪资变动几乎是成比例的。

在之前的‘经验水平’与‘薪资’图表中添加‘公司规模’这一维度可以揭示更多信息。高级职位的薪资平均与执行级别薪资相符。此外，小公司的高级职位薪资平均几乎与相应公司规模的执行级别薪资一致。

通过对薪资列求和，我们得到的数据非常偏向于美国。这可能是因为许多数据科学职位都在美国创建，数据主要在美国收集，或者数据收集表单可能是英文的，并且该表单可能在非英语国家流传。然而，为了线性化数据，我们将对薪资列进行 log10 转换，并使用这些缩放值来绘制地图的颜色。

薪资总和可能不是一个正确的比较指标，因为某些国家的条目可能比其他国家更多。因此我们绘制了保持 log10 缩放的平均薪资图。这能更好地反映全球薪资情况。

可以观察到大多数数据科学职位在美国，并且美国的薪资也最高。加拿大（CA）、日本（JP）、德国（DE）、英国（GB）、西班牙（ES）、法国（FR）、希腊（GR）和印度（IN）在职位薪资和数量方面依次排名（日本除外）。

将平均薪资作为货币的函数进行分析显示，薪资最高的是以美元支付，其次是瑞士法郎和新加坡元。这个图表受到特定货币价值的严重影响，因为图表左侧的大多数货币相对于美元具有较高的价值。

公司的位置在确定平均薪资方面也起着至关重要的作用。根据平均薪资绘制了前 10 个国家。

可以观察到数据科学家是最常见的职位，其次是数据工程师和数据分析师。

由于 Covid-19 危机，大多数工作转向了在家办公模式，但随着疫苗的推出，一切开始恢复正常。

## 推论与结论

对给定的数据科学职位薪资数据集进行了详细的数据分析。可以得出以下结论：

1.  数据科学是几乎所有行业中最受欢迎和新兴的领域之一，如医疗保健、体育、艺术等。

1.  探索了全球数据科学家的平均薪资变动情况。

1.  薪资在不同雇佣类型（如合同制、全职等）之间的变化非常关键。

1.  随着经验的增长，薪资的变化呈上升曲线。

1.  由于新冠疫情危机，工作环境从在家工作转回到正常状态。

## 参考文献与未来工作

所有有用的链接如下：

1.  [`www.kaggle.com/datasets/ruchi798/data-science-job-salaries`](https://jovian.ai/outlink?url=https%3A%2F%2Fwww.kaggle.com%2Fdatasets%2Fruchi798%2Fdata-science-job-salaries)

1.  [`jovian.ai/`](https://jovian.ai/)

1.  [`plotly.com/python/choropleth-maps/`](https://jovian.ai/outlink?url=https%3A%2F%2Fplotly.com%2Fpython%2Fchoropleth-maps%2F)

1.  [`plotly.com/`](https://jovian.ai/outlink?url=https%3A%2F%2Fplotly.com%2F)

**[Nikhil Purao](https://www.linkedin.com/in/nikhilpurao-2233/)** 目前在印度理工学院古瓦哈提分校攻读技术硕士学位，专注于数据与决策科学。作为一名人工智能爱好者，他热衷于利用先进的分析技术和人工智能推动业务增长和改善结果。通过学习，他对该领域最新的工具和技术有了深入了解，并致力于保持在这一激动人心的学科前沿。无论是从复杂数据集中挖掘关键见解，还是开发前沿解决方案，他总是渴望迎接新挑战并与他人合作以取得成功。

[原文](https://medium.com/@nikhilpurao1998/data-scientist-job-salaries-analysis-f153717e2dbf)。经许可转载。

### 更多相关话题

+   [2022 年数据科学领域的热门职位与薪资](https://www.kdnuggets.com/2022/05/top-jobs-salaries-data-science-2022.html)

+   [2023 年数据科学家薪资](https://www.kdnuggets.com/2023/07/2023-data-scientists-salaries.html)

+   [数据分析中的职位趋势：用于职位趋势分析的自然语言处理](https://www.kdnuggets.com/job-trends-in-data-analytics-nlp-for-job-trend-analysis)

+   [数据科学职位头衔导航：数据分析师与数据科学家…](https://www.kdnuggets.com/navigating-data-science-job-titles-data-analyst-vs-data-scientist-vs-data-engineer)

+   [我如何获得我的第一个数据科学家职位](https://www.kdnuggets.com/2023/02/got-first-job-data-scientist.html)

+   [如何在没有工作经验的情况下获得第一个数据科学职位](https://www.kdnuggets.com/2021/02/first-job-data-science-without-work-experience.html)
