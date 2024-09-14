# 使用开源工具实现自动化机器学习系统

> 原文：[https://www.kdnuggets.com/2018/10/implementing-automated-machine-learning-open-source-path.html](https://www.kdnuggets.com/2018/10/implementing-automated-machine-learning-open-source-path.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

我之前写过一些关于自动化机器学习的内容。我不会重复已经涵盖的介绍性材料，但如果有兴趣了解主要点，以及实际操作的尝试，请随意浏览这些文章，然后再继续。

+   [自动化机器学习的当前状态](/2017/01/current-state-automated-machine-learning.html)

+   [自动化机器学习与自动化数据科学](/2018/07/automated-machine-learning-vs-automated-data-science.html)

+   [使用AutoML与TPOT生成机器学习管道](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-4.html)

在我看来，机器学习的实践归结为两个主要的任务，因此在一个有限的实际定义中，我们可以将**自动化机器学习**的核心视为：

1.  自动化特征工程和/或选择

1.  自动化超参数调优和架构搜索

从语义上讲，机器学习模型的训练，虽然这些自动化步骤的结果，但在自动化机器学习过程中是偶然的，而自动化步骤如模型评估和模型选择则是辅助的。见图1。

![Image](../Images/ace43dee5f8c068270a2ce2717540b9c.png)

**图1.** 自动化机器学习过程（核心自动化过程用红色标出，辅助过程用黄色标出）

理论上讲，这一切都很好。但如果你想实现自己的自动化机器学习管道，或在上述两个主要任务方面自动化机器学习管道的特定方面怎么办？

请放心，不需要重新发明轮子；自动化机器学习作为一个学科，可能尚未完全形成，但也绝非完全未开发。请参见下面的开源工具样本，它们可以帮助你完善自己的自动化机器学习管道。

请记住，下面的第一组（超参数调优和架构搜索）通常被广泛认为是“自动化机器学习工具”。但是，请注意，超参数调优和架构搜索工具可以且经常执行某种类型的特征选择。有一套强大的工具只提供自动化特征工程和/或选择（注意你对自动化的定义在这些特定情况下可能不完全一致），因此也提供了这些工具的样本。

### 自动化超参数调优与架构搜索

**[Auto-Keras](https://github.com/jhfjhfj1/autokeras)**

> Auto-Keras 是一个用于自动化机器学习（AutoML）的开源软件库。它由德克萨斯 A&M 大学的 DATA Lab 和社区贡献者开发。AutoML 的最终目标是为数据科学或机器学习背景有限的领域专家提供易于访问的深度学习工具。Auto-Keras 提供了自动搜索深度学习模型架构和超参数的功能。

**[auto-sklearn](https://github.com/automl/auto-sklearn)**

> auto-sklearn 是一个自动化机器学习工具包，是 scikit-learn 估计器的直接替代品。auto-sklearn 使机器学习用户摆脱算法选择和超参数调优的困扰。它利用了贝叶斯优化、元学习和集成构建方面的最新优势。通过阅读我们在 NIPS 2015 上发表的论文，可以进一步了解 auto-sklearn 背后的技术。

**[MLBox](https://github.com/AxeldeRomblay/MLBox)**

> MLBox 是一个强大的自动化机器学习 Python 库。它提供了以下功能：
> 
> +   快速阅读和分布式数据预处理/清理/格式化
> +   
> +   高度可靠的特征选择和泄漏检测
> +   
> +   高维空间中准确的超参数优化
> +   
> +   最先进的分类和回归预测模型（深度学习、堆叠、LightGBM 等）
> +   
> +   带有模型解释的预测

**[TPOT](https://github.com/EpistasisLab/tpot)**

> 一个优化机器学习管道的 Python 自动化机器学习工具，使用遗传编程。将 TPOT 视为你的数据科学助手。TPOT 是一个使用遗传编程优化机器学习管道的 Python 自动化机器学习工具。TPOT 通过智能探索数千个可能的管道，来自动化机器学习中最繁琐的部分，以找到最适合你数据的管道。

### 自动化特征工程/选择

**[Featuretools](https://github.com/Featuretools/featuretools)**

> Featuretools 是一个用于自动化特征工程的 Python 库。Featuretools 可以与你已经使用的工具一起工作来构建机器学习管道。你可以加载 pandas 数据框，并在手动操作所需时间的一小部分内自动创建有意义的特征。

**[mlxtend](https://github.com/rasbt/mlxtend)**

> 一个用于 Python 数据分析和机器学习库的扩展和辅助模块库。

[进一步了解](/2018/06/step-forward-feature-selection-python.html)：

> 那么，我们如何在 Python 中执行前向特征选择呢？Sebastian Raschka 的 mlxtend 库包括一个实现（Sequential Feature Selector），因此我们将使用它来演示。无需多言，你应该在继续之前安装 mlxtend（检查 Github 仓库）。

请注意，这仅仅是可用 Python 自动化机器学习工具的一个样本。除了这里列出的开源工具（以及 Python 生态系统之外），还有许多专有和托管选项可供使用，这些可能在不久的将来需要自己独立的调查文章。

**相关：**

+   [自动化机器学习的现状](https://www.kdnuggets.com/2017/01/current-state-automated-machine-learning.html)

+   [自动化机器学习与自动化数据科学](//2018/07/automated-machine-learning-vs-automated-data-science.html)

+   [使用 AutoML 生成机器学习管道与 TPOT](/2018/01/managing-machine-learning-workflows-scikit-learn-pipelines-part-4.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关话题

+   [封闭源 VS 开源图像标注](https://www.kdnuggets.com/closed-source-vs-open-source-image-annotation)

+   [实施推荐系统于商业中的十个关键课程](https://www.kdnuggets.com/2022/07/ten-key-lessons-implementing-recommendation-systems-business.html)

+   [Chip Huyen 分享实施 ML 系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)

+   [2023 年十大开源数据科学工具的比较概述](https://www.kdnuggets.com/a-comparative-overview-of-the-top-10-open-source-data-science-tools-in-2023)

+   [5 款最佳端到端开源 MLOps 工具](https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools)
