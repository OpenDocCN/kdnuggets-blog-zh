# 在 Python 和 R 中评估预测模型的业务价值

> 原文：[https://www.kdnuggets.com/2018/10/evaluating-business-value-predictive-models-modelplotpy.html](https://www.kdnuggets.com/2018/10/evaluating-business-value-predictive-models-modelplotpy.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Jurriaan Nagelkerke](https://www.linkedin.com/in/jnagelkerke/)、数据科学顾问，和 [Pieter Marcus](https://www.linkedin.com/in/pieter-marcus/)、数据科学家**

### 为什么 ROC 曲线不适合向商业人士解释您的模型

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 摘要

在本博客中，我们解释了四种最有价值的评估图，以评估预测模型的业务价值。这些图包括累积增益、累积提升、响应和累积响应。由于这些可视化图形在大多数流行的模型构建包或 R 和 Python 模块中未包含，我们展示了如何使用我们的 modelplotpy Python 模块和我们的 [modelplotr R 包（喜欢 R 吗？在这里阅读有关 modelplotr 的全部内容！）](https://modelplot.github.io/intro_modelplotr.html) 为您自己的预测模型轻松创建这些图。这将帮助您将模型的业务价值以通俗易懂的方式向非技术人员解释。

### 介绍

![ ](../Images/83c84da9830ba3d49ae3aba258c106a6.png)

> ‘…正如我们在这张 ROC 图上清楚看到的，模型在 0.2 的值下的灵敏度相对于 1 减去特异性是相当高的！对吗？…’。

如果您的商业同事在您展示您出色的预测模型时还没有离开，当您开始这样讲话时，他们肯定会彻底走开。为什么？因为 ROC 曲线不容易快速解释，并且难以转化为观众提出的业务问题的答案。而这些业务问题正是您构建模型的初衷！

什么是业务问题？我们构建各种监督分类问题。例如预测模型，以选择数据集中最佳的记录，这些记录可以是客户、潜在客户、项目、事件……例如：你想知道你的活跃客户中哪一位最可能流失；你需要选择那些最有可能对优惠做出回应的潜在客户；你必须识别出具有高风险的欺诈交易。在你的展示中，你的听众主要关注于回答类似*你的模型是否能帮助我们接触到目标受众？使用你的模型我们有多大的改善？我们的活动预期回应如何？*这样的问题。

在我们构建模型的过程中，我们应该已经关注于验证模型的表现。通常，我们通过在一部分记录上训练模型参数，并在保留集或外部验证集上测试性能来实现这一点。我们查看一些性能指标，如 ROC 曲线和 AUC 值。这些图表和统计数据在模型构建和优化过程中非常有帮助，可以检查模型是否过拟合或欠拟合以及哪些参数在测试数据上表现最佳。然而，这些统计数据在评估你开发的模型的业务价值方面并不那么有价值。

ROC 曲线在解释模型的业务价值方面并不十分有用，因为很难向业务人员解释‘曲线下面积'、‘特异性'或‘灵敏度'的含义。另一个重要原因是这些统计数据和图表在业务会议中无用，因为它们无法帮助决定如何应用预测模型：我们应该基于模型选择多少比例的记录？是否仅选择最好的 10% 的案例？还是在 30% 时停止？或者继续直到选择 70%？……这是你希望与你的业务同事*共同*决定的，以最好地匹配他们的业务计划和活动目标。我们即将介绍的四个图表——累计收益、累计提升、响应和累计响应——在我们看来是最适合这个目的的。

### 让 modelplotpy 在你的机器上运行

让我们从安装**modelplotpy**模块开始。本模块已经可以从[Github](https://github.com/modelplot/modelplotpy)获得，但目前无法通过[Python Package Index (PyPI) 仓库](https://pypi.org/)安装。

对于一些用户，我们发现 GitHub 需要首先安装在你的机器上。如果你还没有在机器上安装 GitHub，可以从[这里](https://desktop.github.com/)下载。安装 GitHub 后，建议重启计算机，然后继续本教程。

Modelplotpy 必须通过命令行安装。在 macOS 或 Linux 操作的机器上打开终端，在 Windows 机器上搜索 cmd.exe 并按回车键。复制并粘贴下面的命令，然后按回车键。

```py
pip install git+https://github.com/modelplot/modelplotpy.git

```

一旦成功安装 modelplotpy，就可以打开 Python 并将其加载到工作目录中，继续本教程！本教程使用 Python 3.6.4 编写，可能在 Python 2.x 中无法完全运行。

```py
import modelplotpy as mp

```

现在我们准备使用 modelplotpy。在另一篇文章中，我们将详细介绍我们在 R 和 Python 中的 modelplot 包及其所有功能。在这里，我们将重点介绍如何使用 modelplotpy 进行商业示例。

### 示例：来自 *sklearn* 的预测模型在 *银行营销数据集* 上

开始工作吧！在介绍图表时，我们将展示如何用一个商业示例使用它们。这个示例基于一个公开可用的数据集，称为银行营销数据集。它是最受欢迎的数据集之一，提供在 [UCI 机器学习库](https://archive.ics.uci.edu/ml/index.php) 上。数据集来自葡萄牙的一家银行，处理一个经常被提问的市场营销问题：客户是否获得了定期存款这一金融产品。共有 4 个数据集，我们使用的是 **bank-additional-full.csv**。它包含了 41,188 名客户的信息和 21 列数据。

为了说明如何使用 modelplotpy，假设我们在这家银行工作，我们的市场营销同事要求我们帮助选择最有可能响应定期存款优惠的客户。为此，我们将开发一个预测模型并创建图表，以便与市场营销同事讨论结果。由于我们要展示如何构建图表，而不是如何构建完美的模型，我们将在示例中使用这六个列。以下是我们使用的数据的简要描述：

1.  **y**：客户是否订阅了定期存款？

1.  **duration**：最后一次联系的持续时间，以秒为单位（数字型）

1.  **campaign**：在本次活动中为该客户进行的联系次数

1.  **pdays**：客户上次从之前的活动中联系后经过的天数

1.  **previous**：在本次活动之前为该客户进行的联系次数（数字型）

1.  **euribor3m**：欧元区 3 个月利率

让我们加载数据并快速查看一下：

```py
import io
import os
import requests
import zipfile
import numpy as np
import pandas as pd
import warnings
warnings.filterwarnings('ignore')

#r = requests.get("https://archive.ics.uci.edu/ml/machine-learning-databases/00222/bank-additional.zip")
# we encountered that the source at uci.edu is not always available, therefore we made a copy to our repos.
r = requests.get('https://modelplot.github.io/img/bank-additional.zip')
z = zipfile.ZipFile(io.BytesIO(r.content))
# You can change the path, currently the data is written to the working directory
path = os.getcwd()
z.extractall(path)
bank = pd.read_csv(path + "/bank-additional/bank-additional-full.csv", sep = ';')

# select the 6 columns
bank = bank[['y', 'duration', 'campaign', 'pdays', 'previous', 'euribor3m']]
# rename target class value 'yes' for better interpretation
bank.y[bank.y == 'yes'] = 'term deposit'

# dimensions of the data
print(bank.shape)

# show the first rows of the dataset
print(bank.head())

```

```py
(41188, 6)
    y  duration  campaign  pdays  previous  euribor3m
0  no       261         1    999         0      4.857
1  no       149         1    999         0      4.857
2  no       226         1    999         0      4.857
3  no       151         1    999         0      4.857
4  no       307         1    999         0      4.857

```

在这些数据上，我们应用了一些来自 [**sklearn 模块**](http://scikit-learn.org/) 的预测建模技术。这个知名模块是许多预测建模技术的封装器，如逻辑回归、随机森林等。让我们训练几个模型并用图表进行评估。

```py
# to create predictive models
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression

# define target vector y
y = bank.y
# define feature matrix X
X = bank.drop('y', axis = 1)

# Create the necessary datasets to build models
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.3, random_state = 2018)

# Instantiate a few classification models
clf_rf = RandomForestClassifier().fit(X_train, y_train)
clf_mult = LogisticRegression(multi_class = 'multinomial', solver = 'newton-cg').fit(X_train, y_train)

```

```py
C:\Users\nagelk000\PycharmProjects\testen_matplotpy\venv\lib\site-packages\sklearn\ensemble\weight_boosting.py:29: DeprecationWarning: numpy.core.umath_tests is an internal NumPy module and should not be imported. It will be removed in a future NumPy release.
  from numpy.core.umath_tests import inner1d

```

在另一篇文章中，我们将详细介绍我们在R和Python中的modelplot包及其所有功能。现在，我们重点解释如何向我们的营销同事展示我们的预测模型如何帮助他们选择客户进行定期存款活动。

```py
obj = mp.modelplotpy(feature_data = [X_train, X_test]
                     , label_data = [y_train, y_test]
                     , dataset_labels = ['train data', 'test data']
                     , models = [clf_rf, clf_mult]
                     , model_labels = ['random forest', 'multinomial logit']
                     )

# transform data generated with prepare_scores_and_deciles into aggregated data for chosen plotting scope 
ps = obj.plotting_scope(select_model_label = ['random forest'], select_dataset_label = ['test data'])

```

```py
No comparison specified! Single evaluation line will be plotted
The label with smallest class is ['term deposit']
Target value term deposit plotted for dataset test data and model random forest.

```

刚刚发生了什么？在**modelplotpy**中实例化了一个类，**plotting_scope**函数指定了你想展示的图表范围。一般来说，**modelplotpy**类中可以应用3种方法（函数），但你不必指定它们，因为它们是链式调用的。这些函数是：

+   **prepare_scores_and_deciles**：根据客户获得定期存款的概率对训练数据集和测试数据集中的客户进行评分

+   **aggregate_over_deciles**：将所有分数聚合到十分位数并计算展示的信息

+   **plotting_scope**允许你指定分析的范围。

在第二行代码中，我们指定了分析的范围。由于我们未指定“scope”参数，因此选择了默认值 - *不进行比较*。正如输出所示，你可以使用**modelplotpy**从多个角度评估你的模型：

+   仅解释一个模型（默认值）

+   比较不同数据集的模型性能

+   比较不同模型的性能

+   比较不同目标类的性能

在这里，我们将简单地评估 - 从业务角度 - 一个选定模型在一个目标类的选定数据集中的表现。我们确实为一些参数指定了值，以便专注于**随机森林**模型在**测试数据**上的表现。目标类的默认值是**定期存款**；由于我们想专注于那些确实进行定期存款的客户，这个默认值是完美的。

### 让我们介绍收益、提升和（累积）响应图。

在我们提供更多代码和输出之前，让我们让你熟悉我们强烈推荐用于评估预测模型商业价值的图表。尽管每个图表从不同角度揭示了模型的商业价值，但它们都使用相同的数据：

+   目标类的预测概率

+   基于此预测概率的等大小组

+   这些组中实际观察到的目标类观察值的数量

将数据划分为10个等大小的组并称这些组为十分位数是一种常见做法。在一个数据集中，属于前10%具有最高模型概率的观察值在该数据集的十分位数1中；下一个10%高模型概率的组是十分位数2，最后10%具有最低目标类模型概率的观察值属于十分位数10。

我们的四个图表中的每一个都将十分位数放在x轴上，另一个指标放在y轴上。十分位数从左到右绘制，因此具有最高模型概率的观察值在图表的左侧。这会产生如下图表：

![ ](../Images/f0d5193245d460e1640096864514008c.png)

现在既然每个图的横轴已经明确，我们可以更详细地探讨每个图的纵轴上的指标。对于每个图，我们将从业务角度简要说明该图提供的洞察。之后，我们将其应用于我们的银行数据，并展示modelplotpy的一些有趣功能，帮助你向他人解释你出色的预测模型的价值。

**1\. 累计收益图**

累计收益图——通常称为“收益图”——帮助你回答以下问题：

***当我们应用模型并选择最佳的X分位时，我们可以期望目标类观察值的百分比是多少？***

因此，累计收益图可视化了如果你决定选择直到第X分位的目标类成员的百分比。这是一个非常重要的业务问题，因为在大多数情况下，你想使用预测模型来针对一个**子集**的观察值——客户、潜在客户、案例等——而不是针对所有案例。由于我们不会总是构建完美的模型，我们会遗漏一些潜力。这是完全可以接受的，因为如果我们不愿意接受这一点，我们不应该使用模型。或者构建一个完美的模型，对所有实际目标类成员的概率为100%，对所有不属于目标类的案件的概率为0%。然而，如果你是这样一个精英，你根本不需要这些图，或者你应该仔细检查你的模型——也许你在作弊？

因此，我们必须接受会有一些损失。*你在给定分位时选择的目标类成员的百分比*，这就是累计收益图告诉你的。图中有两条参考线来告诉你模型的好坏：*随机模型线*和*精英模型线*。随机模型线告诉你在完全没有模型的情况下，你期望选择的实际目标类的比例。这条垂直线从原点（0%案件时，你只能拥有0%目标类成员）到右上角（100%案件时，你拥有100%目标类成员）。这是你模型表现的最低水平；如果你接近这个水平，那么你的模型不比抛硬币好多少。精英模型是你模型能做到的上限。它从原点开始，并尽可能陡峭地上升到100%。如果不到10%的案件属于目标类别，这意味着它从原点急剧上升到第1分位值和100%的累计收益，并在所有其他分位中保持不变，因为这是一个累计指标。你的模型总是会在这两条参考线之间移动——越接近精英模型越好——看起来是这样的：

![ ](../Images/773c69670ffd4dd5c2dcc0fa00a8cb38.png)

回到我们的业务示例。我们能从预测模型的前20%中选择多少定期存款买家？让我们找出答案！为了生成累积收益图，我们可以简单地调用函数**plot_cumgains()**：

```py
# plot the cumulative gains plot
mp.plot_cumgains(ps)

```

```py
The cumulative gains plot is saved in C:\Users\nagelk000\AppData\Local\Temp\intro_modelplotpy.ipynb/Cumulative gains plot.png

```

![](../Images/1d22b8efa88d457540009a4782eb926f.png)

```py
<Figure size 432x288 with 0 Axes>
```

```py
<matplotlib.axes._subplots.AxesSubplot at 0x1ca44358>
```

我们只需指定带有plotting_scope输出的输入。不过，还有几个参数可以自定义图表。如果我们想强调模型在某一点的表现，可以在图表中添加高亮，并在图表下方添加一些解释文本。不过这两项都是可选的：

```py
# plot the cumulative gains plot and annotate the plot at decile = 2
mp.plot_cumgains(ps, highlight_decile = 2)

```

```py
When we select 20% with the highest probability according to model random forest, this selection holds 80% of all term deposit cases in dataset test data.
The cumulative gains plot is saved in C:\Users\nagelk000\AppData\Local\Temp\intro_modelplotpy.ipynb/Cumulative gains plot.png

```

![](../Images/6aa9908c17e07b026f90b89554194d05.png)

```py
<Figure size 432x288 with 0 Axes>
```

```py
<matplotlib.axes._subplots.AxesSubplot at 0x20596198>
```

我们的`highlight_decile`参数在第2百分位数的图表上添加了一些指导元素，并在图表下方添加了一个文本框，文字解释了第2百分位数的图表。这个解释也会打印到控制台。我们的简单模型——仅使用了6个预测变量——似乎很好的挑选出了有意购买定期存款的客户。*当我们根据随机森林选择前20%的客户时，这些选择占测试数据中所有定期存款案例的79%。* 如果模型完美，我们会选择100%，因为测试集中购买定期存款的客户不到20%。随机选择只会包含20%的定期存款客户。这比随机选择好多少，就要看第2号图表了！

### 更多相关主题

+   [超越准确性：使用NLP测试库评估和改进模型](https://www.kdnuggets.com/2023/04/john-snow-beyond-accuracy-nlp-test-library.html)

+   [评估文档相似性计算方法](https://www.kdnuggets.com/evaluating-methods-for-calculating-document-similarity)

+   [机器学习未能为我的业务创造价值。为什么？](https://www.kdnuggets.com/2021/12/machine-learning-produce-value-business.html)

+   [KDnuggets™ 新闻 22:n01, 1月5日：跟踪和可视化的3种工具…](https://www.kdnuggets.com/2022/n01.html)

+   [构建预测模型：Python中的逻辑回归](https://www.kdnuggets.com/building-predictive-models-logistic-regression-in-python)

+   [将数据管理与数据讲故事结合起来以产生价值](https://www.kdnuggets.com/combining-data-management-and-data-storytelling-to-generate-value)
