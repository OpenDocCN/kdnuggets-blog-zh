# 比较、绘制和评估回归模型的简单Python包

> 原文：[https://www.kdnuggets.com/2020/11/simple-python-package-comparing-plotting-evaluating-regression-models.html](https://www.kdnuggets.com/2020/11/simple-python-package-comparing-plotting-evaluating-regression-models.html)

[评论](#comments)

**由 [Ajay Arunachalam](https://www.linkedin.com/in/ajay-arunachalam-4744581a/)，厄尔布鲁大学**

我一直相信将人工智能和机器学习民主化，并以这种方式传播知识，以满足更广泛的受众，充分利用人工智能的力量。与此相关的尝试是开发了Python包“**regressormetricgraphplot**”，旨在帮助用户通过单行代码绘制评估指标图，方便地比较不同广泛使用的回归模型指标。通过这个实用程序包，它还显著降低了从业者以业余的方式评估不同机器学习算法的门槛，将其应用于他们日常的预测回归问题中。

在我们深入了解包的详细信息之前，让我们以简单的术语理解一些基本概念。

一般来说，建模管道包括预处理阶段、拟合机器学习算法，然后进行评估。下图示例展示了集成学习的建模步骤。块A包括数据处理，如清洗、整理、聚合、衍生新特征、特征选择等。块B和C描述了集成学习，其中预处理后的数据输入到Layer-1的各个模型中，这些模型经过评估和调整。Layer-2的输入包括来自之前Layer-1的预测，然后使用投票集成方案得出最终预测。结果通过平均值结合起来。最后，块D展示了模型评估和结果解释。数据按（70:30的比例）分为训练数据和测试数据。使用了三个独立的机器学习算法，即线性回归、随机森林和XGBoost。所有模型都使用了调整过的参数，最后使用了**投票回归**模型。

![图](../Images/d0a644811eddf7c416c573c0e905fc2a.png)

建模管道集成学习示例

使用了不同的回归指标进行评估。让我们讨论每个指标的公式及其对应的简单解释。

![图片](../Images/b9f01e831ca1e4990d7cb8aa274439ec.png)

投票回归器是一个集成的元估计器，它在整个数据集上拟合基础回归器。然后，它将各个预测值取平均以形成最终预测，如下所示。

![图片](../Images/1f06efb9f79cd2db8bca3fef709aed4b.png)

### 入门

**终端安装**

```py
$ pip install regressormetricgraphplot
```

```py
$ git clone https://github.com/ajayarunachalam/RegressorMetricGraphPlot
$ cd RegressorMetricGraphPlot
$ python setup.py install
```

**笔记本**

```py
!git clone https://github.com/ajayarunachalam/RegressorMetricGraphPlot.git
cd RegressorMetricGraphPlot/
```

只需将行‘**from CompareModels import ***’替换为‘**from regressioncomparemetricplot import CompareModels**’

按照演示示例中的其余部分进行操作 [这里] — ([https://github.com/ajayarunachalam/RegressorMetricGraphPlot/blob/main/regressormetricgraphplot/demo.ipynb](https://github.com/ajayarunachalam/RegressorMetricGraphPlot/blob/main/regressormetricgraphplot/demo.ipynb))

**使用 Anaconda 安装**

如果你使用 Anaconda 安装了 Python，可以运行以下命令开始使用：

```py
# Clone the repository
git clone https://github.com/ajayarunachalam/RegressorMetricGraphPlot.git
cd RegressorMetricGraphPlot

# Create new conda environment with Python 3.6
conda create — new your-env-name python=3.6

# Activate the environment
conda activate your-env-name

# Install conda dependencies
conda install — yes — file conda_requirements.txt

# Instal pip dependencies
pip install requirements.txt
```

### 代码讲解

![帖子图片](../Images/9e0eb5211810e85c0bce31e30bfa4d30.png)

### 使用方法

```py
plot = CompareModels()
plot.add(model_name=“Linear Regression”, y_test=y_test, y_pred=y_pred)
plot.show(figsize=(10, 5))
```

![帖子图片](../Images/c7202ef4a1fc35eeea23d9786afb4931.png)

```py
# Metrics
CompareModels.R2AndRMSE(y_test=y_test, y_pred=y_pred)
```

![帖子图片](../Images/bba3241d685a395adf5c3f286db781a9.png)

### 完整演示

综合演示可以在 [Demo.ipynb](https://github.com/ajayarunachalam/RegressorMetricGraphPlot/blob/main/regressormetricgraphplot/demo.ipynb) 文件中找到。

### 联系方式

如果你想添加一些度量指标实现或示例，可以随意添加。你可以通过 [ajay.arunachalam08@gmail.com](mailto:ajay.arunachalam08@gmail.com) 联系我。

继续学习与分享知识!!!

**简介：[Ajay Arunachalam](https://www.linkedin.com/in/ajay-arunachalam-4744581a/)** ([个人网站](https://sites.google.com/site/ajayarunachalamprofile/)) 是瑞典厄勒布鲁大学应用自主传感器系统中心的人工智能博士后研究员。在此之前，他曾在 True Corporation 一家通讯集团担任数据科学家，处理 PB 级数据，构建和部署深度模型。他坚信，在我们完全接受 AI 的力量之前，AI 系统的透明度是当务之急。怀着这一理念，他一直致力于让 AI 普及化，并倾向于构建可解释的模型。他的兴趣在于应用人工智能、机器学习、深度学习、深度强化学习和自然语言处理，特别是学习好的表示。从他在实际问题上的经验来看，他完全承认找到好的表示是设计能够解决有趣且具有挑战性的实际问题的关键，这些问题超越了人类智能，并最终为我们解释复杂的数据。他设想的目标是学习能够从未标记和标记数据中学习特征表示的算法，可以在有无人工互动的情况下进行指导，并且在不同的抽象层次上，以便桥接低级数据和高级抽象概念之间的差距。

[原文](https://ajay-arunachalam08.medium.com/i-always-believe-in-democratizing-ai-and-machine-learning-and-spreading-the-knowledge-in-such-a-5343dab282aa)。经许可转载。

**相关：**

+   [机器学习中的模型评估指标](/2020/05/model-evaluation-metrics-machine-learning.html)

+   [PyCaret 2.1 上线：有什么新变化？](/2020/09/pycaret-21-new.html)

+   [KNN中最常用的距离度量及其使用场景](/2020/11/most-popular-distance-metrics-knn.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域。

* * *

### 更多相关话题

+   [如何使用 MLFlow 打包和分发机器学习模型](https://www.kdnuggets.com/2022/08/package-distribute-machine-learning-models-mlflow.html)

+   [数据科学中的绘图与数据可视化](https://www.kdnuggets.com/2022/06/plotting-data-visualization-data-science.html)

+   [5 个你可能不知道的 Pandas 绘图函数](https://www.kdnuggets.com/2023/02/5-pandas-plotting-functions-might-know.html)

+   [7 个快速数据可视化的 Pandas 绘图函数](https://www.kdnuggets.com/7-pandas-plotting-functions-for-quick-data-visualization)

+   [线性回归与逻辑回归的比较](https://www.kdnuggets.com/2022/11/comparing-linear-logistic-regression.html)

+   [超越准确性：使用 NLP 测试库评估和改进模型](https://www.kdnuggets.com/2023/04/john-snow-beyond-accuracy-nlp-test-library.html)
