# 在Power BI中使用PyCaret进行机器学习

> 原文：[https://www.kdnuggets.com/2020/05/machine-learning-power-bi-pycaret.html](https://www.kdnuggets.com/2020/05/machine-learning-power-bi-pycaret.html)

[评论](#comments)

**由 [Moez Ali](https://www.linkedin.com/in/profile-moez/), PyCaret的创始人兼作者**

![图](../Images/3f5a4a9efcba98dab013359395156933.png)

机器学习与商业智能相结合

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的IT工作

* * *

### **PyCaret 1.0.0**

上周我们宣布了[PyCaret](https://www.pycaret.org/)，这是一个开源的Python机器学习库，用于在**低代码**环境中训练和部署机器学习模型。在我们的[上一篇文章](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)中，我们演示了如何在Jupyter Notebook中使用PyCaret来训练和部署Python中的机器学习模型。

在这篇文章中，我们展示了如何在[Power BI](https://powerbi.microsoft.com/en-us/)中集成**逐步教程**，从而使分析师和数据科学家能够在其仪表板和报告中添加机器学习层，而无需额外的许可证或软件费用。PyCaret是一个开源的**免费使用**的Python库，提供了广泛的功能，专门用于在Power BI中使用。

在本文结束时，您将学习如何在Power BI中实现以下内容：

+   **聚类**— 将具有相似特征的数据点分组。

+   **异常检测**— 识别数据中的稀有观察值/异常值。

+   **自然语言处理**— 通过主题建模分析文本数据。

+   **关联规则挖掘**— 发现数据中的有趣关系。

+   **分类**— 预测二元（1或0）的类别标签。

+   **回归**— 预测连续值，如销售额、价格等

> “PyCaret通过提供**免费、开源和低代码**的机器学习解决方案，正在使机器学习和高级分析变得更加普及，适用于业务分析师、领域专家、普通数据科学家和经验丰富的数据科学家。”

### Microsoft Power BI

Power BI 是一个业务分析解决方案，可以让你可视化数据并在组织内分享洞察，或将其嵌入到你的应用或网站中。在本教程中，我们将使用 [Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) 通过将 PyCaret 库导入 Power BI 来进行机器学习。

### 在开始之前

如果你以前使用过 Python，那么你可能已经在电脑上安装了 Anaconda Distribution。如果没有，[点击这里](https://www.anaconda.com/distribution/)下载带有 Python 3.7 或更高版本的 Anaconda Distribution。

![图](../Images/163b0f04e24139d2b34cd6ee02ab9391.png)

[https://www.anaconda.com/distribution/](https://www.anaconda.com/distribution/)

### 环境设置

在我们开始使用 PyCaret 的机器学习功能之前，我们必须创建一个虚拟环境并安装 pycaret。这是一个三步过程：

✅ **步骤 1 — 创建一个 anaconda 环境**

从开始菜单打开**Anaconda 提示符**并运行以下代码：

```py
conda create --name **myenv** python=3.6
```

![图](../Images/3bd6efcfa61251f8c40a82ef295b22e6.png)

Anaconda 提示符 — 创建环境

✅ **步骤 2 — 安装 PyCaret**

在 Anaconda 提示符中运行以下代码：

```py
conda activate **myenv**
pip install pycaret
```

安装可能需要 10 – 15 分钟。

✅ **步骤 3 — 在 Power BI 中设置 Python 目录**

创建的虚拟环境必须与 Power BI 关联。这可以通过 Power BI Desktop 中的全局设置完成（文件 → 选项 → 全局 → Python 脚本）。Anaconda 环境默认安装在：

C:\Users\***username***\AppData\Local\Continuum\anaconda3\envs\myenv

![图](../Images/e3fdb4e0cc8d5ace7873bd23993fd46d.png)

文件 → 选项 → 全局 → Python 脚本

### ???? 示例 1 — Power BI 中的聚类

聚类是一种机器学习技术，它将具有相似特征的数据点分组。这些分组对于探索数据、识别模式和分析数据子集非常有用。聚类的一些常见商业应用包括：

✔ 客户细分用于市场营销。

✔ 客户购买行为分析用于促销和折扣。

✔ 确定流行病爆发（如 COVID-19）中的地理聚类。

在本教程中，我们将使用**‘jewellery.csv’**文件，该文件可在 PyCaret 的 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/jewellery.csv)中找到。你可以使用 Web 连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

**CSV 文件链接：**[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/jewellery.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/jewellery.csv)

![图](../Images/5edc725e8cd55d215ef4c071db06b9a1.png)

Power BI Desktop → 获取数据 → 其他 → 网络![图](../Images/ff97b38f2726e72632995c3d0f8be0bc.png)

*来自 jewellery.csv 的样本数据点*

### **K-Means 聚类**

为了训练一个聚类模型，我们将在 Power Query 编辑器中执行 Python 脚本（Power Query 编辑器 → 转换 → 运行 Python 脚本）。

![图示](../Images/4b25a7feba15824c05ba67abecc26150.png)

Power Query 编辑器中的功能区

运行以下代码作为 Python 脚本：

```py
from **pycaret.clustering** import *****
dataset = **get_clusters**(data = dataset)
```

![图示](../Images/7f86b0448df2bda96c4cc68ef0543abf.png)

Power Query 编辑器（转换 → 运行 Python 脚本）

### **输出：**

![图示](../Images/9a864907193881d33e531198caf91ed8.png)

集群结果（代码执行后）![图示](../Images/4146b95c76aa1ba15323aa20d9d16f89.png)

最终输出（点击表格后）

一个新的列**‘Cluster’**包含标签，并附加到原始表格中。

一旦应用查询（Power Query 编辑器 → 首页 → 关闭并应用），你可以在 Power BI 中可视化集群，如下所示：

![](../Images/d14f4b5e3aa036b0135d90551263d569.png)

默认情况下，PyCaret 训练一个**K-Means**集群模型，设置为 4 个集群（*即表格中的所有数据点被分类为 4 组*）。默认值可以轻松更改：

+   要更改集群数量，可以在**get_clusters( )**函数中使用***num_clusters***参数。

+   要更改模型类型，请在**get_clusters( )**中使用***model***参数。

查看以下训练 K-Modes 模型并设置 6 个集群的示例代码：

```py
from **pycaret.clustering** import *
dataset = **get_clusters**(dataset, model = 'kmodes', num_clusters = 6)
```

PyCaret 提供了 9 种现成的集群算法：

![](../Images/899f72d5dd7f2a4f89b2e0e40fd3c9d8.png)

所有训练集群模型所需的预处理任务，如[缺失值填补](https://pycaret.org/missing-values/)（如果表格中有任何缺失或*空*值）、[归一化](https://www.pycaret.org/normalization)或[独热编码](https://pycaret.org/one-hot-encoding/)，这些任务都会在训练集群模型之前自动完成。[点击这里](https://www.pycaret.org/preprocessing)了解更多关于 PyCaret 预处理功能的信息。

???? 在此示例中，我们使用了**get_clusters( )**函数来为原始表格分配集群标签。每次查询刷新时，集群都会重新计算。另一种实现方法是使用**predict_model( )**函数来使用**预训练模型**在 Python 或 Power BI 中预测集群标签（*见下面的示例 5，了解如何在 Power BI 环境中训练机器学习模型*）。

???? 如果你想学习如何使用 Jupyter Notebook 在 Python 中训练集群模型，请参见我们的[Clustering 101 初学者教程](https://www.pycaret.org/clu101)。*（无需编码背景）*

### ???? 示例 2 — Power BI 中的异常检测

异常检测是一种用于识别**稀有项**、**事件**或**观察**的机器学习技术，通过检查表格中与大多数行显著不同的行来实现。通常，异常项会转化为某种问题，如银行欺诈、结构缺陷、医疗问题或错误。异常检测的一些常见业务用例包括：

✔ 使用金融数据进行欺诈检测（信用卡、保险等）。

✔ 入侵检测（系统安全，恶意软件）或监控网络流量的激增和下降。

✔ 识别数据集中的多变量异常值。

在本教程中，我们将使用 PyCaret 的 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/anomaly.csv) 上提供的 **‘anomaly.csv’** 文件。您可以使用 Web 连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

**链接到 csv 文件：** [https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/anomaly.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/anomaly.csv)

![图](../Images/471fa1bd43faf4234bb72f168b363b6e.png)

*来自 anomaly.csv 的示例数据点*

### K-最近邻异常检测器

类似于聚类，我们将从 Power Query 编辑器（转换 → 运行 Python 脚本）运行 Python 脚本来训练异常检测模型。运行以下代码作为 Python 脚本：

```py
from **pycaret.anomaly** import *****
dataset = **get_outliers**(data = dataset)
```

![图](../Images/3a0f137e42ed28bad716d735c66bf2d2.png)

Power Query 编辑器（转换 → 运行 Python 脚本）

### **输出：**

![图](../Images/9a864907193881d33e531198caf91ed8.png)

异常检测结果（代码执行后）![图](../Images/e158a6347c29c8bcc68a298b983b4051.png)

最终输出（点击表格后）

原始表格附加了两个新列。标签（1 = 异常，0 = 正常）和分数（高分的数据点被归类为异常）。

一旦应用查询，您可以在 Power BI 中以以下方式可视化异常检测结果：

![](../Images/1f19ffddcfedac07e646344f23f5b6f9.png)

默认情况下，PyCaret 使用 5% 的分数（即表格中总行数的 5% 将被标记为异常）训练 **K-最近邻异常检测器**。默认值可以很容易地更改：

+   要更改分数值，您可以使用 ***fraction*** 参数在 **get_outliers()** 函数中。

+   要更改模型类型，请使用 ***model*** 参数在 **get_outliers()** 中。

请参阅以下代码以训练 **Isolation Forest** 模型，分数值为 0.1：

```py
from **pycaret.anomaly** import *
dataset = **get_outliers**(dataset, model = 'iforest', fraction = 0.1)
```

PyCaret 中有超过 10 种现成的异常检测算法：

![](../Images/60f99e849a1c82050db0cbb2700f2311.png)

所有必要的预处理任务，如 [缺失值填补](https://pycaret.org/missing-values/)（如果表格中有缺失或 *空* 值），或 [归一化](https://www.pycaret.org/normalization)，或 [独热编码](https://pycaret.org/one-hot-encoding/)，都将在训练异常检测模型之前自动执行。 [点击这里](https://www.pycaret.org/preprocessing) 了解更多关于 PyCaret 的预处理功能。

???? 在这个示例中，我们使用了**get_outliers()**函数为分析分配异常值标签和分数。每次刷新查询时，异常值会被重新计算。另一种实现方式是使用**predict_model()**函数，通过Python或Power BI中的预训练模型预测异常值（*请参见下面示例5，了解如何在Power BI环境中训练机器学习模型*）。

???? 如果你想学习如何使用Jupyter Notebook在Python中训练异常检测器，请参见我们的[异常检测101初学者教程](https://www.pycaret.org/ano101)。*（无需编码背景）*

### ???? 示例3 — 自然语言处理

分析文本数据的技术有很多，其中**主题建模**是一种流行的方法。主题模型是一种统计模型，用于发现文档集合中的抽象主题。主题建模是一种常用的文本挖掘工具，用于发现文本数据中的隐藏语义结构。

在本教程中，我们将使用PyCaret的**‘kiva.csv’**文件，该文件可以在[github存储库](https://github.com/pycaret/pycaret/blob/master/datasets/kiva.csv)中找到。你可以通过Web连接器加载数据。（Power BI Desktop → 获取数据 → 来自Web）。

**CSV文件链接：**[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/kiva.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/kiva.csv)

### **潜在狄利克雷分配**

在Power Query编辑器中作为Python脚本运行以下代码：

```py
from **pycaret.nlp** import *****
dataset = **get_topics**(data = dataset, text = 'en')
```

![图像](../Images/702fe793104164afbd7fe70ed7ed5321.png)

Power Query编辑器（转换 → 运行Python脚本）

**‘en’**是表**‘kiva’**中包含文本的列名。

### 输出：

![图像](../Images/9a864907193881d33e531198caf91ed8.png)

主题建模结果（代码执行后）![图像](../Images/b94268f345e68e888a64c3205e01d7e4.png)

最终输出（点击表格后）

一旦代码执行完成，新列会附加到原始表格中，包括主题的权重和主导主题。在Power BI中，有多种方法可视化主题模型的输出。请参见下面的示例：

![](../Images/62689e5bc6adca6e229d5b3480a0898c.png)

默认情况下，PyCaret训练一个包含4个主题的潜在狄利克雷分配模型。默认值可以轻松更改：

+   要更改主题数量，可以在**get_topics()**函数中使用***num_topics***参数。

+   要更改模型类型，请使用***model***参数，在**get_topics()**中。

请参见示例代码以训练一个**非负矩阵分解模型**，主题数量为10：

```py
from **pycaret.nlp** import *
dataset = **get_topics**(dataset, 'en', model = 'nmf', num_topics = 10)
```

PyCaret提供了用于主题建模的现成算法：

![](../Images/2939021fb3471a663f521e5373968128.png)

### ???? 示例4— Power BI中的关联规则挖掘

关联规则挖掘是一种**基于规则的机器学习**技术，用于发现数据库中变量之间的有趣关系。它旨在使用有趣度的度量来识别强规则。一些常见的关联规则挖掘业务用例包括：

✔ 市场篮子分析以了解经常一起购买的商品。

✔ 医疗诊断以帮助医生确定根据因素和症状的疾病发生概率。

在本教程中，我们将使用 PyCaret 的**‘france.csv’**文件，该文件可在 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/france.csv) 上找到。你可以使用网络连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

**csv 文件链接：**[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/france.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/france.csv)

![图示](../Images/bf78797a41f6371b4558a21e94597fcc.png)

*来自 france.csv 的示例数据点*

### Apriori 算法

到目前为止，所有 PyCaret 功能都在 Power Query 编辑器（转换 → 运行 Python 脚本）中作为 Python 脚本执行。运行以下代码以使用 Apriori 算法训练关联规则模型：

```py
from **pycaret.arules** import *
dataset = **get_rules**(dataset, transaction_id = 'InvoiceNo', item_id = 'Description')
```

![图示](../Images/0b92da4796521d4ed23ae963558ca542.png)

Power Query 编辑器（转换 → 运行 Python 脚本）

**‘InvoiceNo’** 是包含交易 ID 的列，**‘Description’** 包含感兴趣的变量，即产品名称。

### **输出：**

![图示](../Images/9a864907193881d33e531198caf91ed8.png)

关联规则挖掘结果（代码执行后）![图示](../Images/b90d6b13f0e3251dd796f4f77a0cb442.png)

最终输出（点击表格后）

它返回一个包含前提和结论的表格，并附有相关指标，如支持度、置信度、提升度等。[点击这里](https://www.pycaret.org/association-rule) 了解更多关于 PyCaret 中的关联规则挖掘的信息。

### ???? 示例 5 — Power BI 中的分类

分类是一种监督学习技术，用于预测类别**分类标签**（也称为二元变量）。分类的一些常见业务用例包括：

✔ 预测客户贷款/信用卡违约。

✔ 预测客户流失（客户是否会留下或离开）

✔ 预测患者结果（患者是否有疾病）

在本教程中，我们将使用 PyCaret 的**‘employee.csv’**文件，该文件可在 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/employee.csv) 上找到。你可以使用网络连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

**csv 文件链接：**[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/employee.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/employee.csv)

**目标：**表**‘employee’**包含了公司中 15,000 名在职员工的信息，例如在公司的工作时间、平均每月工作小时、晋升历史、部门等。基于这些列（也称为机器学习术语中的*特征*），目标是预测员工是否会离开公司，由**‘left’**列表示（1 表示离开，0 表示不离开）。

与聚类、异常检测和 NLP 示例不同，它们属于无监督机器学习的范畴，分类是一种**有监督**技术，因此它被分为两个部分：

### **第1部分：在 Power BI 中训练分类模型**

第一步是创建一个 Power Query 编辑器中的表**‘employee’**的副本，用于训练模型。

![图示](../Images/12d72c5edb34e2a1b1adc63f6852db3a.png)

Power Query 编辑器 → 右键点击‘employee’ → 复制

在新创建的副本表**‘employee (model training)’**中运行以下代码以训练分类模型：

```py
# import classification module and setup environmentfrom **pycaret.classification** import *****
clf1 = **setup**(dataset, target = 'left', silent = True)# train and save xgboost modelxgboost = **create_model**('xgboost', verbose = False)
final_xgboost = **finalize_model**(xgboost)
**save_model**(final_xgboost, 'C:/Users/*username*/xgboost_powerbi')
```

![图示](../Images/ff70a0c29e6419e5655f0a35f125fbcc.png)

Power Query 编辑器（转换 → 运行 Python 脚本）

### 输出：

该脚本的输出将是一个**pickle 文件**，保存于定义的位置。pickle 文件包含了整个数据转换管道以及训练好的模型对象。

???? 另一个选择是在 Jupyter Notebook 中训练模型，而不是 Power BI。在这种情况下，Power BI 将仅用于在前端生成预测，使用的是在 Jupyter Notebook 中预训练的模型，该模型将作为 pickle 文件导入 Power BI（请参见下面的第2部分）。要了解有关在 Python 中使用 PyCaret 的更多信息，[点击这里](https://www.pycaret.org/tutorial)。

???? 如果你想学习如何在 Jupyter Notebook 中使用 Python 训练分类模型，请参见我们的[二分类 101 初学者教程](https://www.pycaret.org/clf101)。*(无需编程背景)。*

PyCaret 提供了 18 种现成的分类算法：

![](../Images/0a844304999676480f56c42c145ec52f.png)

### 第2部分：使用训练好的模型生成预测

现在我们可以使用训练好的模型对原始**‘employee’**表进行预测，判断员工是否会离开公司（1或0）以及概率百分比。运行以下代码作为 Python 脚本以生成预测结果：

```py
from **pycaret.classification** import *****
xgboost = **load_model**('c:/users/*username*/xgboost_powerbi')
dataset = **predict_model**(xgboost, data = dataset)
```

### 输出：

![图示](../Images/9a864907193881d33e531198caf91ed8.png)

分类预测（代码执行后）![图示](../Images/e1ffb5e9076607e39ac8a591a04da35f.png)

最终输出（点击表格后）

原始表中附加了两个新列。**‘Label’** 列表示预测结果，**‘Score’** 列表示结果的概率。

在此示例中，我们对用于训练模型的相同数据进行了预测，仅用于演示目的。在实际情况下，**‘Left’** 列是实际结果，在预测时是未知的。

在本教程中，我们训练了**极端梯度提升**（**‘xgboost’**）模型并用其生成了预测。我们这样做只是为了简化。实际上，你可以使用PyCaret来预测任何类型的模型或模型链。

PyCaret的**predict_model( )**函数可以与使用PyCaret创建的pickle文件无缝协作，因为它包含了整个转换管道以及训练好的模型对象。 [点击这里](https://www.pycaret.org/predict-model)了解更多关于**predict_model**函数的信息。

???? 所有训练分类模型所需的预处理任务，例如[缺失值填补](https://pycaret.org/missing-values/)（如果表格中有任何缺失或*空*值），或[独热编码](https://pycaret.org/one-hot-encoding/)，或[目标编码](https://www.pycaret.org/one-hot-encoding)，这些任务都会在训练模型之前自动执行。 [点击这里](https://www.pycaret.org/preprocessing)了解更多关于PyCaret的预处理功能。

### ???? 示例6——Power BI中的回归

**回归**是一种监督机器学习技术，用于根据过去的数据及其相应的过去结果，以最佳方式预测连续结果。与用于预测二元结果如是或否（1或0）的分类不同，回归用于预测连续值如销售、价格、数量等。

在本教程中，我们将使用**‘boston.csv’**文件，该文件可以在PyCaret的[github存储库](https://github.com/pycaret/pycaret/blob/master/datasets/boston.csv)中找到。你可以使用Web连接器加载数据。（Power BI Desktop → 获取数据 → 从Web）。

**CSV文件链接：** [https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/boston.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/boston.csv)

**目标：** 表**‘boston’**包含了506栋波士顿房屋的信息，如平均房间数、财产税率、人口等。根据这些列（在机器学习术语中也称为*特征*），目标是预测房屋的中位数值，由列**‘medv’**表示。

### 第1部分：在Power BI中训练回归模型

第一步是创建**‘boston’**表的副本，在Power Query编辑器中用于训练模型。

在新的重复表中运行以下代码作为Python脚本：

```py
# import regression module and setup environmentfrom **pycaret.regression** import *****
clf1 = **setup**(dataset, target = 'medv', silent = True)# train and save catboost modelcatboost = **create_model**('catboost', verbose = False)
final_catboost = **finalize_model**(catboost)
**save_model**(final_catboost, 'C:/Users/*username*/catboost_powerbi')
```

### 输出：

此脚本的输出将是保存在指定位置的**pickle文件**。pickle文件包含整个数据转换管道以及训练好的模型对象。

PyCaret中有20多种现成的回归算法：

![](../Images/05cae57d061b18adfa6fbdba17dec7a8.png)

### 第2部分：使用训练好的模型生成预测

现在我们可以使用训练好的模型来预测房屋的中位数值。在原始表**‘boston’**中运行以下代码作为Python脚本：

```py
from **pycaret.classification** import *****
xgboost = **load_model**('c:/users/*username*/xgboost_powerbi')
dataset = **predict_model**(xgboost, data = dataset)
```

### 输出：

![图示](../Images/9a864907193881d33e531198caf91ed8.png)

回归预测（代码执行后）![图示](../Images/87868f1bf6988f82d052fd39e65428a3.png)

最终输出（点击表格后）

新添加了一个包含预测结果的列**‘Label’**到原始表格中。

在这个示例中，我们在同一数据上进行了预测，这些数据用于训练模型仅为演示目的。在实际应用中，**‘medv’**列是实际结果，在预测时是未知的。

???? 所有训练回归模型所需的预处理任务，如[缺失值插补](https://pycaret.org/missing-values/)（如果表格中有任何缺失或*空*值）、[独热编码](https://pycaret.org/one-hot-encoding/)、或[目标变换](https://pycaret.org/transform-target/)，在训练模型之前都会自动完成。[点击这里](https://www.pycaret.org/preprocessing)了解更多关于PyCaret预处理功能的信息。

### 下一个教程

在**使用PyCaret进行Power BI中的机器学习**系列的下一个教程中，我们将深入探讨PyCaret中的高级预处理功能。我们还将了解如何在Power BI中实现机器学习解决方案，并利用[PyCaret](https://www.pycaret.org/)在Power BI前端的强大功能。

如果你想了解更多，请保持关注。

在我们的[Linkedin](https://www.linkedin.com/company/pycaret/)页面关注我们，并订阅我们的[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)频道。

### 另见：

初级Python笔记本：

+   [聚类](https://www.pycaret.org/clu101)

+   [异常检测](https://www.pycaret.org/anom101)

+   [自然语言处理](https://www.pycaret.org/nlp101)

+   [关联规则挖掘](https://www.pycaret.org/arul101)

+   [回归](https://www.pycaret.org/reg101)

+   [分类](https://www.pycaret.org/clf101)

### 开发计划中有什么？

我们正在积极改进PyCaret。我们未来的开发计划包括一个新的**时间序列预测**模块，与**TensorFlow**的集成，以及PyCaret可扩展性的重大改进。如果你愿意分享你的反馈并帮助我们进一步改进，你可以在网站上[填写此表格](https://www.pycaret.org/feedback)或在我们的[Github](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面留言。

### 想了解特定模块吗？

从1.0.0首次发布开始，PyCaret提供以下模块供使用。点击下面的链接查看文档和Python中的示例。

+   [分类](https://www.pycaret.org/classification)

+   [回归](https://www.pycaret.org/regression)

+   [聚类](https://www.pycaret.org/clustering)

+   [异常检测](https://www.pycaret.org/anomaly-detection)

+   [自然语言处理](https://www.pycaret.org/nlp)

+   [关联规则挖掘](https://www.pycaret.org/association-rules)

### 重要链接

+   [用户指南 / 文档](https://www.pycaret.org/guide)

+   [Github 仓库](https://www.github.com/pycaret/pycaret)

+   [安装 PyCaret](https://www.pycaret.org/install)

+   [笔记本教程](https://www.pycaret.org/tutorial)

+   [在 PyCaret 中贡献](https://www.pycaret.org/contribute)

如果你喜欢 PyCaret，请在我们的 [github 仓库](https://www.github.com/pycaret/pycaret) 上给我们 ⭐️。

在 Medium 上关注我：[https://medium.com/@moez_62905/](https://medium.com/@moez_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

**简介：[Moez Ali](https://www.linkedin.com/in/profile-moez/)** 是一名数据科学家，同时是 PyCaret 的创始人和作者。

[原文](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a)。已获许可转载。

**相关：**

+   [宣布 PyCaret 1.0.0](/2020/04/announcing-pycaret.html)

+   [机器学习堆栈中的关键缺失部分](/2020/04/missing-part-machine-learning-stack.html)

+   [通过公共 API 共享你的机器学习模型](/2020/02/sharing-machine-learning-models-common-api.html)

### 更多相关主题

+   [宣布 PyCaret 3.0：开源、低代码的 Python 机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [PyCaret 的二分类介绍](https://www.kdnuggets.com/2021/12/introduction-binary-classification-pycaret.html)

+   [PyCaret 的 Python 聚类介绍](https://www.kdnuggets.com/2021/12/introduction-clustering-python-pycaret.html)

+   [入门 PyCaret](https://www.kdnuggets.com/2022/11/getting-started-pycaret.html)

+   [解锁 AI 的力量 - KDnuggets 和 Machine… 的特别发布](https://www.kdnuggets.com/2023/07/mlm-unlock-power-ai-special-release-kdnuggets-machine-learning-mastery.html)

+   [利用数据的力量的 3 个步骤](https://www.kdnuggets.com/2022/05/3-steps-harnessing-power-data.html)
