# 轻松将LLMs集成到你的Scikit-learn工作流中，使用Scikit-LLM

> 原文：[https://www.kdnuggets.com/easily-integrate-llms-into-your-scikit-learn-workflow-with-scikit-llm](https://www.kdnuggets.com/easily-integrate-llms-into-your-scikit-learn-workflow-with-scikit-llm)

![轻松将LLMs集成到你的Scikit-learn工作流中，使用Scikit-LLM](../Images/7489bc85903354e9c06f7505fa5a3cf7.png)

由DALL-E 2生成的图像

文本分析任务已经存在了一段时间，因为需求始终存在。研究从简单的描述统计到文本分类和高级文本生成已经取得了很大进展。随着大型语言模型的加入，我们的工作任务变得更加轻松。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

Scikit-LLM是一个为文本分析活动开发的Python软件包，利用LLM的强大功能。这个软件包之所以突出，是因为我们可以将标准的Scikit-Learn管道与Scikit-LLM集成。

那么，这个软件包是关于什么的？它是如何工作的？让我们深入了解。

# Scikit-LLM

[Scikit-LLM](https://github.com/iryna-kondr/scikit-llm)是一个通过LLM增强文本数据分析任务的Python软件包。它由[Beatsbyte](https://beastbyte.ai/)开发，旨在帮助桥接标准的Scikit-Learn库和语言模型的强大功能。Scikit-LLM创建了类似于SKlearn库的API，因此我们使用起来不会太麻烦。

## 安装

要使用该软件包，我们需要安装它们。为此，你可以使用以下`code`。

```py
pip install scikit-llm
```

在撰写本文时，Scikit-LLM仅与一些OpenAI和GPT4ALL模型兼容。这就是为什么我们只会使用OpenAI模型的原因。不过，你可以通过首先安装组件来使用GPT4ALL模型。

```py
pip install scikit-llm[gpt4all]
```

安装后，你必须设置OpenAI密钥以访问LLM模型。

```py
from skllm.config import SKLLMConfig

SKLLMConfig.set_openai_key("<your_key>")
SKLLMConfig.set_openai_org("<your_organisation>")</your_organisation></your_key>
```

## 尝试使用Scikit-LLM

让我们在环境设置好后尝试一些Scikit-LLM的功能。LLMs的一项能力是执行文本分类而无需重新训练，这被称为Zero-Shot。然而，我们将首先使用样本数据尝试Few-Shot文本分类。

```py
from skllm import ZeroShotGPTClassifier
from skllm.datasets import get_classification_dataset

#label: Positive, Neutral, Negative
X, y = get_classification_dataset()

#Initiate the model with GPT-3.5
clf = ZeroShotGPTClassifier(openai_model="gpt-3.5-turbo")
clf.fit(X, y)
labels = clf.predict(X)
```

你只需在X变量中提供文本数据，在数据集中提供标签y。在这种情况下，标签包括情感，可能是Positive、Neutral或Negative。

如你所见，这个过程类似于使用 Scikit-Learn 包中的拟合方法。然而，我们已经知道 Zero-Shot 并不一定需要训练数据集。这就是为什么我们可以在没有训练数据的情况下提供标签。

```py
X, _ = get_classification_dataset()

clf = ZeroShotGPTClassifier()
clf.fit(None, ["positive", "negative", "neutral"])
labels = clf.predict(X)
```

这也可以扩展到多标签分类的情况，你可以在以下代码中看到。

```py
from skllm import MultiLabelZeroShotGPTClassifier
from skllm.datasets import get_multilabel_classification_dataset
X, _ = get_multilabel_classification_dataset()
candidate_labels = [
    "Quality",
    "Price",
    "Delivery",
    "Service",
    "Product Variety",
    "Customer Support",
    "Packaging",,
]
clf = MultiLabelZeroShotGPTClassifier(max_labels=4)
clf.fit(None, [candidate_labels])
labels = clf.predict(X)
```

Scikit-LLM 的神奇之处在于，它允许用户将 LLM 的力量扩展到典型的 Scikit-Learn 流水线中。

## Scikit-LLM 在 ML 流水线中的应用

在下一个示例中，我将展示如何将 Scikit-LLM 作为向量化器来初始化，并使用 XGBoost 作为模型分类器。我们还将把这些步骤封装到模型流水线中。

首先，我们将加载数据并启动标签编码器，将标签数据转换为数值。

```py
from sklearn.preprocessing import LabelEncoder

X, y = get_classification_dataset()

le = LabelEncoder()
y_train_enc = le.fit_transform(y_train)
y_test_enc = le.transform(y_test)
```

接下来，我们将定义一个流水线来执行向量化和模型拟合。我们可以使用以下代码来完成。

```py
from sklearn.pipeline import Pipeline
from xgboost import XGBClassifier
from skllm.preprocessing import GPTVectorizer

steps = [("GPT", GPTVectorizer()), ("Clf", XGBClassifier())]
clf = Pipeline(steps)

#Fitting the dataset
clf.fit(X_train, y_train_enc)
```

最后，我们可以使用以下代码来进行预测。

```py
pred_enc = clf.predict(X_test)
preds = le.inverse_transform(pred_enc)
```

如我们所见，我们可以在 Scikit-Learn 流水线下使用 Scikit-LLM 和 XGBoost。结合所有必要的包将使我们的预测更加强大。

使用 Scikit-LLM 还有各种任务可以完成，包括模型微调，我建议你查看文档以进一步了解。如果需要，你还可以使用 [GPT4ALL](https://gpt4all.io/index.html) 的开源模型。

# 结论

Scikit-LLM 是一个 Python 包，利用 LLM 强化 Scikit-Learn 文本数据分析任务。在这篇文章中，我们讨论了如何使用 Scikit-LLM 进行文本分类，并将其结合到机器学习流水线中。

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是一名数据科学助理经理和数据撰稿人。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 涉及各种 AI 和机器学习主题的撰写工作。

### 更多相关话题

+   [在你的笔记本电脑上轻松探索 LLM 与 openplayground](https://www.kdnuggets.com/2023/04/explore-llms-easily-laptop-openplayground.html)

+   [以无编码方式轻松抓取网站上的图像](https://www.kdnuggets.com/2022/06/octoparse-scrape-images-easily-websites-nocoding-way.html)

+   [通过 Scikit-learn 流水线简化你的机器学习工作流程](https://www.kdnuggets.com/streamline-your-machine-learning-workflow-with-scikit-learn-pipelines)

+   [7 个 GPT 助力提升你的数据科学工作流程](https://www.kdnuggets.com/7-gpts-to-help-improve-your-data-science-workflow)

+   [使用 RAPIDS cuDF 加速你的下一个数据科学工作流程](https://www.kdnuggets.com/2023/04/rapids-cudf-speed-next-data-science-workflow.html)

+   [谷歌的 5 门 MLOps 课程提升你的 ML 工作流程](https://www.kdnuggets.com/5-mlops-courses-from-google-to-level-up-your-ml-workflow)
