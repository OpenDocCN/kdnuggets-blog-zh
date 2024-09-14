# 精通新一代梯度提升

> 原文：[https://www.kdnuggets.com/2018/11/mastering-new-generation-gradient-boosting.html](https://www.kdnuggets.com/2018/11/mastering-new-generation-gradient-boosting.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：[Tal Peretz](https://www.linkedin.com/in/tal-per/)，数据科学家**

![Image](../Images/6f0021d10fa8e75d1f0681617209fb04.png)

Catboost

**梯度提升决策树**和随机森林是我最喜欢的用于表格异构数据集的机器学习模型。这些模型在*[Kaggle](https://www.kaggle.com/)* 竞赛中表现突出，并在业界广泛使用。

**Catboost**，这个新兴的模型，已经存在了一年多，现在已经对*XGBoost*、*LightGBM*和*H2O*构成威胁。

### 为什么选择 Catboost？

**更好的结果**

Catboost 在基准测试中取得了最佳结果，这很棒，但我不确定是否仅凭一小部分对数损失的改善就替换一个正在运行的生产模型（尤其是当进行基准测试的公司明显偏向 Catboost 时????）。

然而，当你查看**分类特征发挥重要作用**的数据集时，如*Amazon*和*Internet*数据集，这一改进变得显著而不可否认。

![](../Images/c0f56356b0efe76253f46b1b9e3e4db4.png)

GBDT 算法基准测试

**更快的预测**

尽管训练时间可能比其他 GBDT 实现要长，但根据 Yandex 基准测试，预测时间比其他库快 13 到 16 倍。

![](../Images/b878dfec55b45c7736bc70937ced22a9.png)

左侧：CPU，右侧：GPU

**功能齐全**

Catboost 的默认参数是比其他 GBDT 算法更好的起点。这对希望快速上手体验树集成或参与 Kaggle 竞赛的初学者来说是好消息。

然而，还有一些非常重要的参数我们必须讨论，我们马上会谈到这些。

![](../Images/1d04b9a46e27fdf79d1394772740d7c8.png)

使用默认参数的 GBDT 算法基准测试

Catboost 的一些更值得注意的进展包括特征交互、对象重要性和快照支持。

除了分类和回归，Catboost 开箱即用地支持**排名**。

**经过实战考验**

[Yandex](https://yandex.com/) 在排名、预测和推荐中严重依赖 Catboost。这个模型每月为超过 7000 万用户提供服务。

> CatBoost 是一种**基于决策树的梯度提升**算法。由 Yandex 的研究人员和工程师开发，它是广泛应用于公司排名任务、预测和推荐的[**MatrixNet 算法**](https://yandex.com/company/technologies/matrixnet/)的继任者。它具有通用性，可以应用于广泛的领域和各种问题。

![](../Images/c5da7f5f59cc6ccf130e81e835370235.png)

### 算法

**经典梯度提升**

![](../Images/5c4a3ead431e03d2e430b6f2cd022cf1.png)

Wikipedia 上的梯度提升

![](../Images/d9ca34b445477fccd614b5d7fa7e7ae8.png)

### Catboost 秘密配方

Catboost 引入了两个关键的算法进步——实现了 **有序提升**，这是一种基于排列的经典算法替代方案，以及一种用于 **处理分类特征** 的创新算法。

这两种技术都使用训练样本的随机排列来对抗所有现有梯度提升算法中存在的 *预测偏移* 造成的特殊类型的 *目标泄漏*。

### **分类特征处理**

**有序目标统计量**

大多数 GBDT 算法和 Kaggle 竞争者已经熟悉目标统计量（或目标均值编码）的使用。

这是一种简单而有效的方法，我们对每个分类特征进行编码，编码值是根据类别条件计算的预期目标 y 的估计。

事实证明，不加小心地应用这种编码（同一类别训练样本的 y 平均值）会导致目标泄漏。

为了对抗这种 *预测偏移*，CatBoost 使用了一种更有效的策略。它依赖于排序原则，并受到在线学习算法的启发，这些算法按时间顺序获取训练样本。在这种设置下，每个样本的 TS 值仅依赖于观察到的历史。

为了将这一思想适应标准的离线设置，Catboost 引入了一个人工“时间”——训练样本的随机排列 *σ1*。

然后，对于每个样本，它使用所有可用的“历史”来计算其目标统计量。

注意，仅使用一个随机排列，会导致前面的样本在目标统计量中具有比后续样本更高的方差。为此，CatBoost 对梯度提升的不同步骤使用不同的排列。

**One Hot 编码**

Catboost 对所有具有至多 *one_hot_max_size* 唯一值的特征使用 one-hot 编码。默认值为 2。

![](../Images/2f8ee6fadf0aa6b7e40160287f640d95.png)

Catboost 的秘密配方

### 有序提升

CatBoost 有两种选择树结构的模式，Ordered 和 Plain。 **Plain 模式** 对应于标准 GBDT 算法与有序目标统计量的结合。

在 **Ordered 模式** 提升中，我们对训练样本进行随机排列——*σ2*，并维持 n 个不同的支持模型——*M1, ... , Mn*，使得模型 *Mi* 仅使用排列中的前 *i* 个样本进行训练。

在每一步，为了获得 *j* -th 样本的残差，我们使用模型 *Mj−1*。

不幸的是，由于需要维护 n 个不同的模型，这个算法在大多数实际任务中不可行，这会使复杂性和内存需求增加 n 倍。Catboost 实现了该算法的一个修改版，基于梯度提升算法，使用所有模型共享的一个树结构来构建。

![](../Images/7245343ce5d8d1988a29e6935d988b25.png)

Catboost 有序提升与树构建

为了避免 *预测偏移*，Catboost 使用置换使得 *σ1* = *σ2*。这确保了目标-*yi* 既不用于训练 *Mi*，也不用于目标统计计算或梯度估计。

### 实践操作

对于本节，我们将使用 [*Amazon 数据集*](https://www.kaggle.com/c/amazon-employee-access-challenge/data)，因为它干净且强调类别特征。

![](../Images/54f9b2df0a33b9243c0edaeb7b2614e7.png)

数据集简述

### 调整 Catboost

**重要参数**

+   **cat_features** — 这个参数对于利用 Catboost 对类别特征的预处理是必须的，如果你自己编码类别特征而不传递列索引作为*cat_features*，你就错过了 Catboost 的精髓。*

+   ***one_hot_max_size**** — *如前所述，Catboost 对所有特征使用一热编码，前提是这些特征最多有 *one_hot_max_size* 个唯一值。在我们的案例中，类别特征有很多唯一值，因此我们不会使用一热编码，但根据数据集的不同，调整此参数可能是个好主意。*

+   ***learning_rate* & *n_estimators*** — 学习率越小，需要的*n_estimators*就越多。通常的方法是从相对较高的*learning_rate*开始，调整其他参数，然后在增加*n_estimators*的同时减少*learning_rate*。

+   ***max_depth**** — *基础树的深度*，*这个参数对训练时间有很大影响。*

+   ***subsample ****— *行的采样率，不能在* *Bayesian* *提升类型设置中使用。*

+   ***colsample_bylevel ****— *列的采样率。*

+   ***l2_leaf_reg ****— *L2 正则化系数。*

+   ***random_strength ****— *每个分裂都会得到一个分数，而 random_strength 会给分数添加一些随机性，这有助于减少过拟合。*

### 使用 Catboost 进行模型探索

除了特征重要性（这在 GBDT 模型中非常流行），Catboost 还提供了 **特征交互**和 **对象（行）重要性**

![](../Images/082c2a9b1ba4d6c8314ba8aa236e987b.png)

Catboost 的特征重要性![](../Images/56623055e6f9f321c7421840035723e3.png)

Catboost 的特征交互![](../Images/aee63f4ec198f40df446c77d26da3020.png)

Catboost 的对象重要性![](../Images/cc8f573706b8833248e07619ab3f075f.png)

[SHAP](https://catboost.ai/news/new-ways-to-explore-your-data) 值也可以用于其他集成方法

### 完整笔记本

查看一些有用的 Catboost 代码片段

Catboost Playground Notebook ### 结论

![](../Images/ef39987d7aca7ca0507cfb9e7cd44cb7.png)

Catboost与XGBoost（默认、贪婪和全面的参数搜索）

![](../Images/fc9fa159d967eadf6f7a1be280d62dca.png)

**要点**

+   Catboost的构建方法和特性类似于“较旧”一代的GBDT模型。

+   Catboost的优势在于其**类别特征预处理**、**预测时间**和**模型分析**。

+   Catboost的弱点在于其**训练和优化时间**。

+   不要忘记将***cat_features*** 参数传递给分类器对象。如果没有它，你就没有真正利用Catboost的力量。

+   尽管Catboost在默认参数下表现良好，但还有几个参数在调优时会显著提高结果。

**进一步阅读**

+   [Catboost文档](https://tech.yandex.com/catboost/doc/dg/concepts/about-docpage/)

+   [Catboost Github](https://github.com/catboost/catboost)

+   [Catboost官方网站](https://catboost.ai/)

+   我强烈推荐你深入阅读 [CatBoost: unbiased boosting with categorical features paper on arXiv](https://arxiv.org/abs/1706.09516)。

+   [Catboost playground notebook](https://gist.github.com/talperetz/6030f4e9997c249b09409dcf00e78f91)

+   [SHAP值](https://github.com/slundberg/shap)

非常感谢Catboost团队负责人 [Anna Veronika Dorogush](https://medium.com/@a.v.dorogush)。

如果你喜欢这篇文章，随时点击掌声按钮 ????????，如果你对即将发布的文章感兴趣，确保关注我

**中等: **[**https://medium.com/@talperetz24**](https://medium.com/@talperetz24) **推特:**[**https://twitter.com/talperetz24**](https://twitter.com/talperetz24) **领英:**[**https://www.linkedin.com/in/tal-per/**](https://www.linkedin.com/in/tal-per/)

每年，我都想提到 [DataHack](https://www.datahack.org.il/)- 最好的数据驱动黑客马拉松。今年 [דור פרץ](https://medium.com/@sdorperetz) 和我使用了Catboost进行我们的项目并赢得了第一名 ????。

**个人简介: [Tal Peretz](https://www.linkedin.com/in/tal-per/)** 是数据科学家、软件工程师和持续学习者。他热衷于利用数据、代码和算法解决具有高价值潜力的复杂问题。他特别喜欢构建和改进模型，将一与零区分开来。0|1

[原文](https://towardsdatascience.com/https-medium-com-talperetz24-mastering-the-new-generation-of-gradient-boosting-db04062a7ea2)。经许可转载。

**相关:**

+   [TensorFlow中的梯度提升与XGBoost](/2018/01/gradient-boosting-tensorflow-vs-xgboost.html)

+   [CatBoost与Light GBM与XGBoost](/2018/03/catboost-vs-light-gbm-vs-xgboost.html)

+   [直观的梯度提升集成学习指南](/2018/07/intuitive-ensemble-learning-guide-gradient-boosting.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 了解更多相关话题

+   [检索增强生成：信息检索与文本生成的交汇点](https://www.kdnuggets.com/retrieval-augmented-generation-where-information-retrieval-meets-text-generation)

+   [掌握季节性并提升业务成果的终极指南](https://www.kdnuggets.com/2023/08/media-mix-modeling-ultimate-guide-mastering-seasonality-boosting-business-results.html)

+   [提升机器学习算法：概述](https://www.kdnuggets.com/2022/07/boosting-machine-learning-algorithms-overview.html)

+   [Bark：终极音频生成模型](https://www.kdnuggets.com/2023/05/bark-ultimate-audio-generation-model.html)

+   [人工智能的未来：探索下一代生成模型](https://www.kdnuggets.com/2023/05/future-ai-exploring-next-generation-generative-models.html)

+   [揭开 Midjourney 5.2 的面纱：人工智能图像生成的飞跃](https://www.kdnuggets.com/2023/06/unveiling-midjourney-52-leap-forward.html)
