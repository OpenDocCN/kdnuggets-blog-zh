# 5 个必试的精彩 Python 数据可视化库

> 原文：[https://www.kdnuggets.com/2021/09/5-awesome-data-visualization-libraries-python.html](https://www.kdnuggets.com/2021/09/5-awesome-data-visualization-libraries-python.html)

[comments](#comments)

**由 [Roja Achary](https://www.linkedin.com/in/roja-achary-b038b014a/) 提供，机器学习爱好者**

> * * *
> 
> ## 我们的前三大课程推荐
> ## 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。
> 
> ![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能
> 
> ![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT
> 
> * * *
> 
> “可视化的目的是洞察，而不是图片。”
> 
> ―本·施奈德曼

![Figure](../Images/320842b45341b2de64a76eb3e5b0b12d.png)

来源 – Venn 图

数据可视化是数据或信息的视觉呈现。数据可视化的目标是清晰有效地向读者传达数据或信息。通常，数据通过图表、信息图、图示、地图等形式进行可视化。

它有什么帮助？

+   识别趋势和异常值

+   在数据中讲述故事

+   强化论点或观点

+   突出数据集中的一个重要点

让我们深入了解它们。

### **所需库**

使用包管理器 [`pip`](https://pip.pypa.io/en/stable/) 安装以下库：

```py
pip install matplotlib
pip install seaborn
pip install plotnine
pip install plotly
pip install bokeh
```

### **Matplotlib**

![Image](../Images/0c38abaec646a68f0cae2b488e5f2bbb.png)

Matplotlib 是一个全面的库，用于在 Python 中创建静态、动画和互动可视化。大多数编码人员都从 Matplotlib 开始他们的数据可视化之旅。

特性：

+   它的设计类似于 MATLAB，因此在两者之间切换相对容易。

+   包含许多渲染后端。

+   它可以再现几乎所有的图形（虽然需要一点努力）。

+   已经存在超过十年，因此拥有庞大的用户基础。

**[10 分钟内快速掌握 Matplotlib 代码](https://github.com/rojaAchary/Data-Visualization-with-Python/tree/main/Matplotlib_plots)**

### **Seaborn**

![Image](../Images/ee58b7dcd6262abf5b6add8097b812a0.png)

Seaborn 利用 Matplotlib 的强大功能，用几行代码创建美丽的图表。关键区别在于 Seaborn 的默认样式和颜色调色板，设计得更具美学和现代感。由于 Seaborn 是建立在 Matplotlib 之上，因此你需要了解 Matplotlib 以调整 Seaborn 的默认设置。

特性：

+   内置主题用于样式化 matplotlib 图形

+   可视化单变量和双变量数据

+   拟合和可视化线性回归模型

+   绘制统计时间序列数据

+   Seaborn 与 NumPy 和 Pandas 数据结构兼容良好

+   它带有内置的主题用于样式化Matplotlib图形

**[10分钟内掌握Seaborn的快速代码](https://github.com/rojaAchary/Data-Visualization-with-Python/tree/main/Seaborn_Visz)**

### Plotnine

![图像](../Images/2c9aac217d25d13368eaaea55d1d4fbb.png)

Plotnine是Python中图形语法的实现，它基于ggplot2。该语法允许用户通过明确映射数据到构成图表的视觉对象来组合图表。

特性：

+   统计变换

+   刻度

+   坐标系统

+   方面

+   主题

**[10分钟内掌握Plotnine的快速代码](https://github.com/rojaAchary/Data-Visualization-with-Python/tree/main/Plotnine)**

### **Bokeh**

![图像](../Images/de031e0358af774429b45a8ad70772a2.png)

来源：Patrik Hlobil

Bokeh是一个现代网页浏览器的互动可视化库。它提供了优雅、简洁的多功能图形构建，并在大型或流数据集上提供高性能的交互。Bokeh可以帮助任何希望快速轻松创建互动图表、仪表板和数据应用的人。

特性：

+   灵活

+   互动

+   强大

+   高效

+   可共享

+   开源

**[10分钟内掌握Bokeh的快速代码](https://github.com/rojaAchary/Data-Visualization-with-Python/tree/main/Bokeh_tuts)**

### **Plotly**

![图像](../Images/d32a714fc81da42ef3bbc99a8ef3ccae.png)

plotly是一个互动的、开源的、基于浏览器的Python绘图库。构建在plotly.js之上，plotly.py是一个高级的、声明式的绘图库。plotly.js提供了30多种图表类型，包括科学图表、3D图形、统计图表、SVG地图、金融图表等。

特性：

+   图表，仪表板

+   文件导出，应用管理器

+   Kubernetes，认证

+   作业队列，快照引擎

+   嵌入，Python的大数据

**[10分钟内掌握Plotly的快速代码](https://github.com/rojaAchary/Data-Visualization-with-Python/tree/main/Plotly)**

**参考文献和帮助：**

+   [https://matplotlib.org/stable/tutorials/index.html](https://matplotlib.org/stable/tutorials/index.html)

+   [http://seaborn.pydata.org/index.html](http://seaborn.pydata.org/index.html)

+   [https://plotnine.readthedocs.io/en/stable/](https://plotnine.readthedocs.io/en/stable/)

+   [https://bokeh.org/](https://bokeh.org/)

+   [https://plotly.com/python/](https://plotly.com/python/)

**简历： [Roja Achary](https://www.linkedin.com/in/roja-achary-b038b014a/)** (**[Kaggle](https://www.kaggle.com/rojaachary), [GitHub](https://github.com/rojaAchary)**) 是一位机器学习爱好者和热情的学习者。她对人工智能、数据科学以及软件工程领域感兴趣，并且始终乐于进行有意义的合作。

**相关：**

+   [Python中的动画条形图竞赛](/2021/05/animated-race-bar-charts-python.html)

+   [可视化如何改变探索性数据分析](/2021/08/visualization-transforming-exploratory-data-analysis.html)

+   [直接使用 Pandas 获取交互式图表](/2021/06/interactive-plots-directly-pandas.html)

### 更多相关内容

+   [2024 年你必须尝试的 5 大最佳向量数据库](https://www.kdnuggets.com/the-5-best-vector-databases-you-must-try-in-2024)

+   [你必须尝试的 5 大 AI 编码助手](https://www.kdnuggets.com/top-5-ai-coding-assistants-you-must-try)

+   [2024 年你必须尝试的 7 个端到端 MLOps 平台](https://www.kdnuggets.com/7-end-to-end-mlops-platforms-you-must-try-in-2024)

+   [数据科学、数据可视化及……的 38 大 Python 库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)

+   [在神经网络之前你可以尝试的 10 件简单事](https://www.kdnuggets.com/2021/12/10-simple-things-try-neural-networks.html)

+   [2023 年你需要尝试的 5 个令人惊叹的免费 LLM 演练场](https://www.kdnuggets.com/5-amazing-free-llms-playgrounds-you-need-to-try-in-2023)
