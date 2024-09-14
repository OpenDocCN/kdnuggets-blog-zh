# 如何生成合成表格数据集

> 原文：[https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

![如何生成合成表格数据集](../Images/a1d5f83ff8498849deb3ba47907a9ccc.png)

作者提供的图片

企业经常遇到这样的问题：他们没有足够的真实数据，或者由于隐私问题不能使用实际数据。这时，合成数据生成技术就派上用场了。研究人员和数据科学家利用合成数据来开发新产品、提升机器学习模型的性能、替代敏感数据，并节省数据获取成本。更多内容请阅读 [终极合成数据指南](https://research.aimultiple.com/synthetic-data/)。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT

* * *

合成数据被应用于医疗保健、自驾车、金融行业、维持高水平隐私以及研究目的 - [数据科学探索](https://towardsdatascience.com/synthetic-data-key-benefits-types-generation-methods-and-challenges-11b0ad304b55)。去年，整个Kaggle的 [表格玩法系列](https://www.kaggle.com/c/tabular-playground-series-apr-2021) 数据集是使用CTGANs开发的。Kaggle团队利用来自各种比赛的旧数据集生成了人工数据集。这是Kaggle的一个聪明举动，因为它使作弊变得困难。在这篇博客中，我们将学习使用 [SDV](https://sdv.dev/SDV/#) 的Python库生成表格数据的各种方法。

# CTGANs

[CTGAN](https://arxiv.org/abs/1907.00503) 使用几种 [基于GAN](https://machinelearningmastery.com/tour-of-generative-adversarial-network-models/) 的方法从原始数据中学习，并生成高度真实的表格数据。为了生成合成表格数据，我们将使用开源Python库中的条件生成对抗网络，如 [CTGAN](https://pypi.org/project/ctgan/) 和合成数据库 ([SDV](https://sdv.dev/))。SDV允许数据科学家从单表、关系数据和时间序列中学习和生成数据集。它是各种表格数据的一站式解决方案。

通过几行代码，我们的深度学习模型可以学习并生成样本表格数据。在下面的示例中，我们初始化了模型，在真实数据上训练了它，保存了模型，然后生成了 200 个样本。这只是开始，我们还将学习生成人工数据的各种方法，然后评估结果。

![如何生成合成表格数据集](../Images/7d9b8b958865d9e407e605cacfe3e8ff.png)

图片来源于作者

# 教程

在本教程中，我们将使用 [Food Demand Forecasting | Kaggle](https://www.kaggle.com/kannanaikkal/food-demand-forecasting)（根据 [DbCL](https://opendatacommons.org/licenses/dbcl/1-0/) 许可）数据集来创建合成数据集。我们将尝试一个基础模型、自定义模型，最后评估结果。这些结果将帮助我们确定生成数据的质量。我们将使用 [Deepnote](https://deepnote.com/) 云笔记本生成结果。

## CTGANSynthesizer

首先，我们需要通过使用 `pip install ctgan` 安装 CTGAN 库，然后使用 Pandas 加载我们的数据集。我们正在打乱数据集，以获得真正随机的两千个样本。我们使用较少的样本量来减少训练时间。

```py
from ctgan import CTGANSynthesizer
import pandas as pd

data = pd.read_csv("/work/food-demand-forecasting/train.csv")\
                    .sample(frac=1).reset_index(drop=True)[0:2000]
```

在开始模型训练之前，让我们观察一下我们的实际数据集，以便我们可以看到真实数据和人工数据之间的差异。

```py
data.head()
```

![如何生成合成表格数据集](../Images/3c690c93a910e65edacf933f3f52d126.png)

为了训练我们的 CTGAN 模型，我们需要提供离散列的列表、批量大小和训练轮数。API 类似于 [Scikit-learn](https://scikit-learn.org/)，我们用超参数初始化模型，然后使用 `.fit()` 训练模型。在成功训练模型后，我们将保存模型以重现类似的结果。通过 `ctgan.sample(2000)` 我们将生成 2k 个样本。

```py
discrete_columns = ['week',
                    'Center_id',
                    'Meal_id',
                    'Emailer_for_promotion',
                    'homepage_featured']
ctgan = CTGANSynthesizer(batch_size=50,epochs=5,verbose=False)
ctgan.fit(data,discrete_columns)
ctgan.save('ctgan-food-demand.pkl')
samples = ctgan.sample(2000)
samples.head()
```

正如我们所观察到的，输出与原始数据非常相似。结果是可以接受的。

![如何生成合成表格数据集](../Images/1a26cf80589b30adf5411e35c8776e27.png)

## SDV

在这一部分，我们将使用 [SDV](https://sdv.dev/SDV/) 库生成数据。SDV 提供了多个单表模型，并且还提供了更多功能来进行数据生成实验。

首先，我们将通过提供主键和浮动特征的小数位数来初始化模型。然后，我们将训练模型并生成 200 个样本。

```py
from sdv.tabular import CTGAN

model = CTGAN(primary_key='id',rounding=2)
model.fit(data)
model.save("sdv-ctgan-food-demand.pkl")
new_data = model.sample(200)
new_data.head()
```

正如我们所看到的，id 列以 0 开始，整体数据看起来很干净。为了完全理解我们的合成数据，我们需要使用相似度度量来评估数据集，并比较数据分布。

![如何生成合成表格数据集](../Images/3c192f11c664fe8dc1686b606d09ee60.png)

## 评估

我们将使用 SDV evaluate 函数来分析真实数据集与虚假数据集之间的相似度。此函数显示了所有相似度指标的汇总结果，范围从 0 到 1，其中 0 最差，1 最理想。在我们的情况下，相似度指标为**（0.34）**，这表示较差，我们需要在超参数优化上投入更多工作，以获得更好的结果。

```py
from sdv.evaluation import evaluate
evaluate(new_data, data)
>>>> 0.34091406940696406
```

[TableEvaluator](https://pypi.org/project/table-evaluator/) 用于评估合成数据集与真实数据集之间的相似度。它专门用于分析基于 GAN 的表格模型的性能。通过编写几行代码，我们可以比较两个数据集在绝对对数均值、所有特征的分布、相关矩阵和前两个 [PCA](https://www.keboola.com/blog/pca-machine-learning) 组件上的表现。

```py
from table_evaluator import load_data, TableEvaluator
table_evaluator = TableEvaluator(data, new_data)
table_evaluator.visual_evaluation()
```

通过观察所有可视化结果，我们可以得出结论：数据特征的分布在某种程度上是相似的，但主成分分析的分布与真实数据完全不同。

![如何生成合成表格数据集](../Images/e37e1772e4003f50070cb3afa1fb5c5f.png)

![如何生成合成表格数据集](../Images/264bb0bb3ad5633d3e2be96acd065cb4.png)

![如何生成合成表格数据集](../Images/540039a27ab33660f9a3b8781baf32f3.png)

## 自定义模型

我们来创建一个自定义模型，通过改变训练轮数、批量大小、生成器维度和判别器维度来改善相似度指标。

```py
model = CTGAN(
        epochs=500,
        batch_size=100,
        generator_dim=(256, 256, 256),
        discriminator_dim=(256, 256, 256)
             )
model.fit(data)
model.save("manual-CTGAN.pkl")
```

我们还可以通过创建常量来定制生成的数据集。假设我们想为*center_id 161\.* 生成数据。我们将首先创建一个“条件”字典，并将其传递给下述样本函数。

```py
conditions = {"center_id": 161}
model.sample(5, conditions=conditions)
```

![如何生成合成表格数据集](../Images/889ae70d040607bc4d50ba13fcff4852.png)

通过轻量级超参数优化，我们已实现了更好的相似度评分**（0.53）**，如下所示。如果您的生成数据集的评分在 0.6 到 0.7 之间，那么您的数据集已准备好用于生产。

```py
new_data = model.sample(2000)
evaluate(new_data, data)
>>>> 0.517249739944206
```

# SDV 用于关系数据和时间序列

我们还可以使用 SDV 库生成关系数据集，使用 `sdv.relational.HMA1` 和时间序列数据，使用 `sdv.timeseries.PAR`。API 的工作原理类似于 CTGAN 模型，我们只需训练模型，然后生成**N**个样本。

## 关系数据

[层次建模算法](https://sdv.dev/SDV/user_guides/relational/hma1.html) 是一种允许递归遍历关系数据集并在所有表格上应用表格模型的算法。通过这种方式，模型学习所有表格中的字段如何相关。

## 时间序列

[概率自回归模型](https://sdv.dev/SDV/user_guides/timeseries/par.html#what-is-par) 允许学习多类型、多变量的时间序列数据，然后生成具有与学习数据相同格式和属性的新合成数据。

# 结论

我使用 CTGAN 来提升我的机器学习模型性能，并评估模型在未见数据集上的准确性。这也帮助我处理不平衡的类数据集。如果你有有限的训练数据，并且希望在不投入大量资金的情况下开发人工智能产品，那么你应该考虑生成合成数据集。合成数据集不仅限于表格数据，我们现在可以使用 GAN 生成图像、音频和文本数据集。创建人工数据是一个不断发展的领域，未来它将超过真实数据集。

在这篇博客中，我们了解了 CTGAN 以及如何使用 SDV 库生成表格数据集。我们还了解到，我们可以使用类似的 API 生成关系数据和时间序列数据。希望你喜欢这个简短的教程，如果你对调试代码有任何问题，请在下方评论。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一名认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络为患有心理疾病的学生构建人工智能产品。

### 更多相关内容

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)

+   [数据驱动的人工智能与表格数据](https://www.kdnuggets.com/2022/09/datacentric-ai-tabular-data.html)

+   [使用 ChatGPT 生成被动收入的 4 种方法](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

+   [使用 Google MusicLM 从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

+   [3 种使用稳定扩散生成超真实面孔的方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [将数据管理与数据讲述结合生成价值](https://www.kdnuggets.com/combining-data-management-and-data-storytelling-to-generate-value)
