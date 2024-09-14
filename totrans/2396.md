# 使用 Eurybia 检测数据漂移以确保生产机器学习模型质量

> 原文：[https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

在本文的其余部分，我们将重点介绍使用 Eurybia 这一开源 Python 库进行的逐步数据漂移研究。检测数据漂移是确保生产环境中机器学习模型质量的重要步骤。这种检测大大有利于人工智能的长期可靠性。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

![使用 Eurybia 检测数据漂移以确保生产机器学习模型质量](../Images/15f602b6c3985943485046c1ad53411e.png)

Eurybia 演示，图像来源：作者

# 什么是数据漂移，为什么要关心？

机器学习模型通常在历史数据库中的数据上进行训练。然后，模型被部署在生产环境中。生产环境中的预测是基于新获得的数据进行的。

因此，用于训练模型的历史数据和用于预测结果的数据可能有所不同。这种数据集之间的差异被称为数据漂移。

生产环境中的机器学习模型有时会随着时间的推移出现性能下降。这种下降可能归因于操作使用或数据漂移（或两者）。

# 如何检测数据漂移？

为了检测数据漂移，可以使用几种方法，例如通过统计测试比较数据集特征的分布。

Python 库 Eurybia 使用了一种方法，该方法包括训练一个**二分类模型**（称为“*数据漂移分类器*”）。该模型试图预测样本是否属于训练/基线数据集或生产/当前数据集。

![使用 Eurybia 检测数据漂移以确保生产机器学习模型质量](../Images/2f5aee8dd8e202cad0e4a20476d24a70.png)

数据漂移分类器的工作原理，图像来源：作者

然后可以研究创建的**可解释性**“*数据漂移分类器*”。这是一大优势，因为可解释性提供了关于漂移的详细分析，并为进一步研究和理解数据漂移提供了强有力的理由。除了可解释性，“*数据漂移分类器*”的性能还提供了数据漂移的度量。

# 数据漂移：我们在寻找什么？ 

> 最显著的特征是什么变化了？

可以提取“*datadrift classifier*”中包含的每个特征的重要性。这有助于优先研究变化最大的特征。

> 数据漂移是否影响模型预测？

Eurybia 将数据漂移与部署模型的特征重要性进行对比。特征对部署模型越重要，特征的漂移问题就越严重。

> 每个特征的变化是什么？

可以观察基线数据集和当前数据集中每个特征的分布。

> 数据漂移的影响是什么？

Eurybia 根据基线数据集和生产数据集绘制预测概率分布图。我们可以通过 Jensen Shannon Divergence 测量这些分布之间的差异。

> 我们可以测量数据漂移吗？

Eurybia 提供了数据漂移的性能指标（“*datadrift classifier*”的 AUC 和预测概率分布的 Jensen Shannon Divergence），显著地允许随着时间跟踪。

# 使用 Eurybia 演示数据漂移检测

你可以通过 pip 安装该软件包：

```py
$pip install eurybia
```

## 让我们使用 Eurybia！

在本文的其余部分，我们将专注于使用 Eurybia 进行逐步的数据漂移研究。

我们将使用来自 [Kaggle](https://www.kaggle.com/c/house-prices-advanced-regression-techniques) 的著名“房价”数据集。数据将被拆分为两个不同的数据集。2006年的数据将用于训练一个回归模型，以预测房屋的销售价格。然后，将使用之前创建的模型预测2007年售出的房屋价格。

我们将尝试检测这个回归器的漂移，基于2006年与2007年数据集的数据漂移。

**让我们开始加载数据集：**

**构建监督模型：**

**并使用 Eurybia 检测数据漂移…**

**步骤 1 — 导入**

**步骤 2 — 初始化 SmartDrift 对象**

可选参数：

+   **“deployed_model”**：在此处指明用于提供预测的模型。该参数允许将数据漂移结果与预测模型结合，从而提供关于数据漂移对部署模型性能的潜在影响的线索。

+   “**Encoding**”：如果使用了特定的编码器来准备数据集，以便为 *“deployed_model”* 提供修改过的数据，则应在此处指明。Eurybia 将能够整合这些信息并返回原始数据格式。

**步骤 3 — 编译**

可选参数：

+   “**full_validation**”：False/True（默认值：False）。完整验证包括分析列之间的模式一致性。

+   “**date_compile_auc**”：指定保存结果时要标明的日期。当计算过去数据集的漂移时，这可能会很有用。

+   **“datadrift_file”**：包含数据漂移性能历史记录的 CSV 文件名。此参数在随时间评估漂移时很有用。

**步骤 4 — 生成 Html 报告**

可选参数：

+   “**title_description**”：简短的副标题用于描述报告。

+   “**project_info_file**”：yaml 文件的路径。该文件可能包含多个要添加到报告中的信息，如一般信息、数据集描述、数据准备或模型训练。

# 如何理解 Eurybia 的输出

Eurybia 旨在生成用于分析的 HTML 报告。HTML 报告中提供的所有图表也可以在 Jupyter notebooks 中查看。

![使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](../Images/427852ab9371adcd67d6dd98d14b9072.png)

Eurybia 报告索引，图片来源：作者

报告可以通过标签浏览，结构如下：

+   索引：主页

+   项目信息：报告背景和信息

+   一致性分析：突出显示两个数据集之间的差异

+   数据漂移：深入数据漂移分析

**Datadrift 分类器模型性能**

数据漂移检测方法基于模型分类器识别样本属于一个数据集还是另一个数据集的能力。为此，将目标 (0) 分配给基线数据集，将第二个目标 (1) 分配给当前数据集。训练一个分类模型 (catboost) 来预测这个目标。因此，数据漂移分类器的性能与两个数据集之间的差异相关。显著的差异将导致容易分类 (最终 AUC 接近 1)。类似的数据集将导致数据漂移分类器性能差 (最终 AUC 接近 0.5)。

![使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](../Images/c703373236fb724a2e4ef16220c26fb4.png)

Datadrift 分类器模型性能，图片来源：作者

AUC 越接近 0.5，数据漂移越少。AUC 越接近 1，数据漂移越多

**数据漂移中特征的重要性**

条形图表示每个包含在“*datadrift 分类器*”中的变量的特征重要性。

![使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](../Images/97cfbcc3bf0fba8ea857fdda97bfe75b.png)

数据漂移中特征的重要性，图片来源：作者

该图表突出显示了漂移最多的变量。这有助于优先考虑对特定变量的进一步深入研究。

## 特征重要性概述

Eurybia HTML 报告还包含一个散点图，描绘了每个特征的特征重要性，作为“*datadrift 分类器*”特征重要性的函数。这突出了数据漂移对已部署模型分类的实际重要性。

![使用 Eurybia 检测数据漂移以确保生产 ML 模型质量](../Images/95b89e12f85e4d74c8f7432f4367e844.png)

特征重要性概述，图片来源：作者

基于特征位置的图形解释：

+   左上角：对已部署模型重要且数据漂移较低的特征

+   左下角：对已部署模型重要性适中且数据漂移较低的特征

+   右下角：对部署模型重要性适中的特征但具有高数据漂移。这一特征可能需要你的关注。

+   右上角：对部署模型重要且数据漂移高的特征。这一特征需要你的关注。

## 单变量分析

单变量分析通过两个数据集的分布图形分析得到支持，便于研究对漂移检测最重要的特征。

![使用Eurybia检测数据漂移以确保生产ML模型质量](../Images/11439a554a0b7045156dd767c876d943.png)

单变量分析，图片来源：作者

在Eurybia报告中，通过下拉菜单，特征按照数据漂移分类中的重要性排序。对于分类特征，可能的值按两个数据集之间的降序差异进行排序。

**预测值的分布**

预测值的分布有助于可视化部署模型在基线数据集和当前数据集上的输出预测。分布差异可能反映数据漂移。

![使用Eurybia检测数据漂移以确保生产ML模型质量](../Images/cab662b411781e0011fe265abef16add.png)

预测值的分布，图片来源：作者

Jensen Shannon Divergence (JSD)。JSD衡量数据漂移对部署模型性能的影响。接近0的值表示数据分布相似，而接近1的值则通常显示数据分布不同，对部署模型性能有负面影响。

![使用Eurybia检测数据漂移以确保生产ML模型质量](../Images/14c671aba52f214fbb7443e9721f675a.png)

Jensen Shannon Divergence，图片来源：作者

**历史数据漂移**

Eurybia可以编译数据漂移情况，涵盖销售数据直到2010年。

![使用Eurybia检测数据漂移以确保生产ML模型质量](../Images/b557d064c4dcf148a9310d9946f089fe.png)

历史数据漂移，图片来源：作者

“Datadrift分类器AUC”和“Jensen Shannon Datadrift”提供了两个有用的数据漂移指标。

虽然AUC严格关注数据演变，JSD则评估数据演变对部署模型预测的影响。

换句话说，单一变量的明显漂移，如果该变量对部署模型的重要性较低，应导致高AUC但可能低JSD。

# 如果你想深入了解…

关于Eurybia不同用途的教程可以在 [**这里**](https://github.com/MAIF/eurybia/tree/master/tutorial)**找到**。

希望Eurybia在监控生产中的ML模型时能发挥作用。欢迎任何反馈和想法！[**Eurybia**](https://github.com/MAIF/eurybia)是开源的！请随时通过评论此帖或在[GitHub讨论区](https://github.com/MAIF/eurybia/discussions)来贡献。

[**托马斯·布歇**](https://medium.com/@thomas.bouche_2245)是MAIF的首席数据科学家。

### 更多相关话题

+   [使用MLOps管理生产环境中的模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)

+   [数据质量维度：用伟大的期望确保数据质量](https://www.kdnuggets.com/2023/03/data-quality-dimensions-assuring-data-quality-great-expectations.html)

+   [数据掩码：确保GDPR及其他法规合规性的核心](https://www.kdnuggets.com/2023/05/data-masking-core-ensuring-gdpr-regulatory-compliance-strategies.html)

+   [确保LLMs的可靠少量提示选择](https://www.kdnuggets.com/2023/07/ensuring-reliable-fewshot-prompt-selection-llms.html)

+   [将机器学习模型部署到云端生产环境](https://www.kdnuggets.com/deploying-your-ml-model-to-production-in-the-cloud)

+   [检测ChatGPT、GPT3和GPT2的5款免费工具](https://www.kdnuggets.com/2023/02/5-free-tools-detecting-chatgpt-gpt3-gpt2.html)
