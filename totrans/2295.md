# 机器学习中的预处理重要性

> 原文：[https://www.kdnuggets.com/2023/02/importance-preprocessing-machine-learning.html](https://www.kdnuggets.com/2023/02/importance-preprocessing-machine-learning.html)

![机器学习中预处理的重要性](../Images/51bbbce6173821c391abc769ce351c28.png)

由 [DeepMind](https://unsplash.com/@deepmind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来自 [Unsplash](https://unsplash.com/photos/rXy5Zlmw3qY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

很明显，开发新模型或算法的 ML 团队期望模型在测试数据上的性能是最优的。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您所在组织的 IT。

* * *

但很多时候这种情况并不会发生。

**原因可能有很多，但主要的罪魁祸首是：**

1.  数据不足

1.  数据质量差

1.  过拟合

1.  欠拟合

1.  算法选择不当

1.  超参数调优

1.  数据集中的偏差

上述列表并不详尽。

在这篇文章中，我们将讨论可以解决上述多个问题的过程，并且 ML 团队在执行时要非常注意。

这是数据的预处理。

机器学习社区普遍接受预处理数据是 ML 工作流中的重要一步，并且它可以提高模型的性能。

许多研究和文章已经显示了在机器学习中预处理数据的重要性，例如：

> “Bezdek 等人（1984 年）的研究发现，预处理数据可以使几种聚类算法的准确性提高多达 50%。”
> 
> “Chollet（2018 年）的研究发现，数据预处理技术，如数据归一化和数据增强，可以提高深度学习模型的性能。”

还值得一提的是，预处理技术不仅对提高模型的性能很重要，而且对使模型更具解释性和鲁棒性也很重要。

例如，处理缺失值、去除异常值和缩放数据可以帮助防止过拟合，从而使模型对新数据的泛化能力更强。

无论如何，重要的是要注意，所需的具体预处理技术和预处理的程度将取决于数据的性质和算法的具体要求。

还要记住，在某些情况下，数据预处理可能不是必要的，甚至可能对模型的性能产生负面影响。

在将数据应用于机器学习 (ML) 算法之前的预处理是 ML 工作流程中的一个关键步骤。

这一步有助于确保数据处于算法可以理解的格式，并且没有错误或异常值，这些都可能对模型性能产生负面影响。

在本文中，我们将讨论数据预处理的一些优势，并提供一个如何使用流行的 Python 库 Pandas 预处理数据的代码示例。

# 数据预处理的优势

数据预处理的主要优势之一是它有助于提高模型的准确性。通过清理和格式化数据，我们可以确保算法仅考虑相关信息，而不受任何无关或不正确数据的影响。

这可以导致一个更准确、更稳健的模型。

数据预处理的另一个优势是，它可以帮助减少训练模型所需的时间和资源。通过移除无关或冗余的数据，我们可以减少算法需要处理的数据量，这可以大大减少训练模型所需的时间和资源。

数据预处理还可以帮助防止过拟合。过拟合发生在模型在一个过于特定的数据集上进行训练，因此在训练数据上表现良好，但在新的、未见过的数据上表现较差。

通过预处理数据并移除无关或冗余的信息，我们可以帮助减少过拟合的风险，并提高模型对新数据的泛化能力。

数据预处理还可以提高模型的可解释性。通过清理和格式化数据，我们可以更容易理解不同变量之间的关系及其如何影响模型的预测。

这可以帮助我们更好地理解模型的行为，并做出更明智的决定来改进它。

## 示例

现在，让我们看一个使用 Pandas 进行数据预处理的示例。我们将使用一个包含关于葡萄酒质量信息的数据集。该数据集有几个特征，如酒精含量、氯化物、密度等，还有一个目标变量，即葡萄酒的质量。

```py
import pandas as pd

# Load the data
data = pd.read_csv("winequality.csv")

# Check for missing values
print(data.isnull().sum())

# Drop rows with missing values
data = data.dropna()

# Check for duplicate rows
print(data.duplicated().sum())
# Drop duplicate rows
data = data.drop_duplicates()

# Check for outliers
Q1 = data.quantile(0.25)
Q3 = data.quantile(0.75)
IQR = Q3 - Q1
data = data[
    ~((data < (Q1 - 1.5 * IQR)) | (data > (Q3 + 1.5 * IQR))).any(axis=1)
]

# Scale the data
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
data_scaled = scaler.fit_transform(data)

# Split the data into training and testing sets
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    data_scaled, data["quality"], test_size=0.2, random_state=42
)
```

在这个示例中，我们首先使用 Pandas 的 `read_csv` 函数加载数据，然后使用 `isnull` 函数检查缺失值。接着，我们使用 `dropna` 函数删除含有缺失值的行。

接下来，我们使用 `duplicated` 函数检查重复的行，并使用 `drop_duplicates` 函数将其删除。

我们然后使用四分位距 (IQR) 方法检查异常值，该方法计算第一四分位数和第三四分位数之间的差异。任何超出 1.5 倍 IQR 的数据点都被认为是异常值，并从数据集中移除。

在处理了缺失值、重复行和异常值之后，我们使用来自sklearn.preprocessing库的StandardScaler函数对数据进行缩放。缩放数据很重要，因为它有助于确保所有变量都在相同的尺度上，这对于大多数机器学习算法正常运行是必要的。

最后，我们使用来自sklearn.model_selection库的train_test_split函数将数据分成训练集和测试集。这个步骤对于评估模型在未见数据上的表现是必要的。

# 如果我忽略了这一步会怎样？

在将数据应用于机器学习算法之前不进行预处理可能会带来几个负面后果。一些主要问题包括：

1.  模型性能差：如果数据没有被正确清理和格式化，算法可能无法正确理解数据，这可能导致模型性能差。这可能是由于数据集中存在缺失值、异常值或未被移除的无关数据造成的。

1.  过拟合：如果数据集没有经过清理和预处理，可能会包含无关或冗余的信息，从而导致过拟合。过拟合发生在模型在一个过于特定的数据集上进行训练时，因此它在训练数据上表现良好，但在新的、未见的数据上表现较差。

1.  更长的训练时间：不进行数据预处理可能导致训练时间延长，因为算法可能需要处理比必要更多的数据，这可能非常耗时。

1.  理解模型的难度：如果数据没有经过预处理，可能会很难理解不同变量之间的关系以及它们如何影响模型的预测。这会使识别模型中的错误或改进领域变得更加困难。

1.  偏倚的结果：如果数据没有经过预处理，它可能包含错误或偏差，这可能导致不公平或不准确的结果。例如，如果数据中包含缺失值，算法可能在处理一个偏差的数据样本，这可能导致错误的结论。

一般来说，不进行数据预处理可能导致模型的准确性降低、可解释性差以及操作困难。数据预处理是机器学习工作流中的一个重要步骤，不应被忽略。

# 结论

总之，在将数据应用于机器学习算法之前进行预处理是机器学习工作流中的一个关键步骤。它有助于提高准确性，减少训练模型所需的时间和资源，防止过拟合，并提高模型的可解释性。

上述代码示例演示了如何使用流行的Python库Pandas进行数据预处理，但还有许多其他库可用于数据预处理，例如NumPy和Scikit-learn，可以根据项目的具体需求进行使用。

**[Sumit Singh](https://www.linkedin.com/in/sumit-singh-68b07a25/)** 是一位致力于数据中心人工智能的连续创业者。他共同创立了下一代训练数据平台[Labellerr](https://www.labellerr.com)。Labellerr的平台使AI-ML团队能够轻松自动化他们的数据准备流程。

### 更多相关主题

+   [机器学习不像你的大脑 第6部分：精确突触权重的重要性](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-6-importance-precise-synapse-weights-ability-set-quickly.html)

+   [机器学习中可重复性的重要性](https://www.kdnuggets.com/2023/06/importance-reproducibility-machine-learning.html)

+   [庆祝对数据隐私重要性的认识](https://www.kdnuggets.com/2022/01/celebrating-awareness-importance-data-privacy.html)

+   [数据科学中实验设计的重要性](https://www.kdnuggets.com/2022/08/importance-experiment-design-data-science.html)

+   [数据科学中概率的重要性](https://www.kdnuggets.com/2023/02/importance-probability-data-science.html)

+   [数据科学中数据清洗的重要性](https://www.kdnuggets.com/2023/08/importance-data-cleaning-data-science.html)
