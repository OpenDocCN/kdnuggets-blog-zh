# 解释“黑箱”机器学习模型：SHAP的实际应用

> 原文：[https://www.kdnuggets.com/2020/05/explaining-blackbox-machine-learning-models-practical-application-shap.html](https://www.kdnuggets.com/2020/05/explaining-blackbox-machine-learning-models-practical-application-shap.html)

[评论](#comments)

**由[Norman Niemer](https://www.linkedin.com/in/normanniemer/)，首席数据科学家**

### 动机

GBM模型经过实际测试证明是强大的模型，但由于缺乏可解释性而受到影响。通常，数据科学家会查看变量重要性图，但它们不足以解释模型的工作原理。为了最大限度地提高模型用户的接受度，使用SHAP值来回答常见的可解释性问题，并建立对模型的信任。

在这篇文章中，我们将对一个简单的数据集训练一个GBM模型，并学习如何解释模型的工作原理。这里的目标不是解释数学如何运作，而是向非技术用户解释输入变量如何与输出变量相关，以及如何进行预测。

我们使用的数据集是[ISLR](http://faculty.marshall.usc.edu/gareth-james/ISL/)提供的广告数据集，你可以在[d6t github](http://tiny.cc/d6t-blog-20200426-shapley)上获取所用的代码。

### 设置

```py
# processing
import d6tflow, d6tpipe

import pandas as pd
import numpy as np
import pathlib

# viz
import seaborn as sns
import plotly.express as px
import matplotlib.pyplot as plt
import matplotlib

# modeling
import statsmodels.api as sm
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from sklearn.inspection import plot_partial_dependence
from sklearn.model_selection import cross_validate
import lightgbm
import shap
```

### 首先，构建高效的数据科学工作流程

如在[数据科学家常犯的10个编码错误](/2019/04/top-10-coding-mistakes-data-scientists.html)中解释的，为了构建一个高效的数据科学工作流程，我们将使用文件管理器[d6tpipe](https://github.com/d6t/d6tpipe)和工作流管理器[d6tflow](https://github.com/d6t/d6tflow)。

```py
api = d6tpipe.APIClient()
pipe = d6tpipe.Pipe(api, 'intro-stat-learning')
pipe.pull()
```

```py
# preprocessing workflow

class TaskProcessRawData(d6tflow.tasks.TaskPqPandas):

    def run(self):
        df = pd.read_csv(pipe.dirpath/'Advertising.csv', usecols=[1,2,3,4])
        self.save(df)

@d6tflow.requires(TaskProcessRawData)
class TaskFeatures(d6tflow.tasks.TaskPqPandas):

    def run(self):
        df = self.inputLoad()
        df['target']=df['sales']
        df['radio']=-df['radio'] # force negative relationship
        df['tv_radio']=df['TV']*df['radio'] # interaction effect
        self.save(df)

print('preprocessing workflow:')
d6tflow.preview([TaskFeatures()]) 
```

```py
preprocessing workflow:

└─--[TaskFeatures-{} (COMPLETE)]
   └─--[TaskProcessRawData-{} (COMPLETE)] 
```

### 其次，构建模型前不要没有直觉关于它应该做什么

如在[数据科学家常犯的10个统计错误](/2019/06/statistics-mistakes-data-scientists.html)中解释的，你需要形成对模型应如何工作的经济直觉。

广告数据集展示了销售额与电视、广播和报纸广告支出的关系。通过查看散点图，我们可以看出电视和广播是有效的营销渠道，因为销售额与这些渠道的支出密切相关。但请注意，广播有负面影响，而电视有正面影响（注意我们强制了广播的负相关关系）。报纸似乎只有边际影响。最后，电视和广播之间似乎存在交互效应。

```py
df_train = TaskFeatures().outputLoad()
cfg_col_X = ['TV', 'radio', 'newspaper'] # base features
cfg_col_X_interact =  cfg_col_X+['tv_radio'] # includes interaction variable
cfg_col_Y = 'target'
df_trainX, df_trainY = df_train[cfg_col_X], df_train[cfg_col_Y]
```

```py
dfp = df_train.melt(id_vars=cfg_col_Y,value_vars=cfg_col_X)
sns.lmplot(x="value", y="target", col="variable", data=dfp, sharex=False);
```

![图示](../Images/91e1202f811974d4cc6613b24e9ab0dd.png)

### 现在训练一个“黑箱”机器学习模型

GBM模型经过实际测试证明是强大的模型，但由于缺乏可解释性而受到影响。让我们训练模型，看看如何解释它。我们将使用[LightGBM](https://lightgbm.readthedocs.io/en/latest/)。

```py
m_lgbm = lightgbm.LGBMRegressor()
m_lgbm.fit(df_trainX,df_trainY)
```

### 为什么变量重要性图不起作用

正如其名，变量重要性图告诉你输入和输出变量之间关系的强度。但为了信任一个模型，用户通常希望了解更多：影响是正向还是负向，是线性的还是非线性的？是否存在有效或无效的情况？他们希望看到模型是否符合他们的经济直觉。

在我们的例子中，重要性图没有显示出我们确定的报纸支出与负相关关系。这可能会让模型用户感到困惑，他们可能仅凭这一图表不会信任模型。

```py
lightgbm.plot_importance(m_lgbm); 
```

![图](../Images/cf2f0e726a61dfb0d37f84d0cce47f6f.png)

### 用SHAP解释模型

[SHAP](https://github.com/slundberg/shap) 使你能够看到每个输入对输出变量的方向性影响，这为模型用户提供了关于模型工作原理的重要直觉。

如你所见，模型确实显示了电视支出较高时销售额较高，而收音机支出较高时销售额较低。这与我们的直觉一致，并使模型用户相信你的模型是正确的。

```py
explainer = shap.TreeExplainer(m_lgbm, df_trainX)
shap_values = explainer.shap_values(df_trainX)
```

```py
shap.summary_plot(shap_values, df_trainX, plot_size=(12,6))
```

![图](../Images/90d1d1e5395c7e6ac4014d342b1669c4.png)

你也可以在散点图中调查“拟合”值，以更详细地可视化输入和输出变量之间的关系。这对于更复杂的关系和理解互动效应非常有用。

在这种情况下，关系是线性的，并且电视和收音机之间存在互动（你可以通过运行OLS并包含`tv_radio`变量来确认）。

```py
for col in cfg_col_X:
    shap.dependence_plot(col, shap_values, df_trainX)
```

![图](../Images/e409ed864fa73d1b3f9b9b98b80281bc.png)![图](../Images/317eb2b070625a1bcba2a0af0e4309d2.png)![图](../Images/85da95a70c69aa54c8ae1501fe42825a.png)

### 那么部分依赖图呢？

这些方法也有效。它们的优点在于其输出与实际目标变量的尺度一致。这不同于Shapley值，它展示了相对于平均预测的边际影响。

```py
matplotlib.rcParams['figure.figsize'] = (12,6)
plot_partial_dependence(m_lgbm, df_trainX, range(df_trainX.shape[1])) 
```

![图](../Images/b153753d6f8030c2bb65b39391790078.png)

### 解释最新的预测

这是实践中常遇到的另一个问题：你已经训练了模型，并使用最新数据进行预测。模型用户不仅想知道预测值，还想知道为什么你的模型做出这个预测。SHAP可以解释单个预测，并解释每个输入变量如何影响总体预测。

在这里，我们可以看到最新的预测值是[13.18]。电视提高了预测值，而收音机降低了预测值。报纸的影响几乎可以忽略。这与我们的预期一致。

```py
shap.force_plot(explainer.expected_value, shap_values[-1,:], df_trainX.iloc[-1,:],matplotlib=True)
```

![图](../Images/b13cde286009eddb8396686e749ff3d4.png)

### 哪里可以了解更多？

理解SHAP

+   [https://github.com/slundberg/shap](https://github.com/slundberg/shap)

+   [https://christophm.github.io/interpretable-ml-book/shap.html](https://christophm.github.io/interpretable-ml-book/shap.html)

构建高效的数据科学工作流

+   [https://github.com/d6t/d6t-python/blob/master/blogs/effective-datasci-workflows.rst](https://github.com/d6t/d6t-python/blob/master/blogs/effective-datasci-workflows.rst)

+   [https://github.com/d6t/d6tflow-template](https://github.com/d6t/d6tflow-template)

使用 Databolt 加速数据科学

+   [https://github.com/d6t/d6t-python](https://github.com/d6t/d6t-python)

+   [https://www.databolt.tech/](https://www.databolt.tech/)

**简介: [诺曼·尼默](https://www.linkedin.com/in/normanniemer/)** 是一家大型资产管理公司的首席数据科学家，他提供数据驱动的投资见解。他拥有哥伦比亚大学的金融工程硕士学位以及伦敦 Cass 商学院的银行与金融学士学位。

[原始](https://github.com/d6t/d6t-python/blob/master/blogs/blog-20200426-shapley.ipynb)。经授权转载。

**相关:**

+   [数据科学家常犯的 10 大编码错误](/2019/04/top-10-coding-mistakes-data-scientists.html)

+   [数据科学家常犯的 10 大统计错误](/2019/06/statistics-mistakes-data-scientists.html)

+   [你的机器学习代码可能糟糕的 4 个原因](/2019/02/4-reasons-machine-learning-code-probably-bad.html)

* * *

## 我们的 3 大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 方面的组织

* * *

### 更多相关话题

+   [SHAP: 在 Python 中解释任何机器学习模型](https://www.kdnuggets.com/2022/11/shap-explain-machine-learning-model-python.html)

+   [使用 SHAP 值解释机器学习模型](https://www.kdnuggets.com/2023/08/shap-values-model-interpretability-machine-learning.html)

+   [解释可解释的 AI 用于对话](https://www.kdnuggets.com/2022/10/explaining-explainable-ai-conversations.html)

+   [扩散与去噪: 解释文本到图像生成 AI](https://www.kdnuggets.com/diffusion-and-denoising-explaining-text-to-image-generative-ai)

+   [Feature Store 峰会 2023: 实用策略部署 ML…](https://www.kdnuggets.com/2023/09/hopsworks-feature-store-summit-2023-practical-strategies-deploying-ml-models-production-environments)

+   [数据科学的基础数学: 特征向量及其在 PCA 中的应用](https://www.kdnuggets.com/2022/06/essential-math-data-science-eigenvectors-application-pca.html)
