# 数据科学家应该了解的5个统计悖论

> 原文：[https://www.kdnuggets.com/2023/02/5-statistical-paradoxes-data-scientists-know.html](https://www.kdnuggets.com/2023/02/5-statistical-paradoxes-data-scientists-know.html)

![数据科学家应了解的5个统计悖论](../Images/562301ccf1afe501fc699b8a9778ba7d.png)

图片由作者提供

# 简介

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

作为数据科学家，我们依赖统计分析来从数据中挖掘不同变量之间关系的信息，以回答问题，这将帮助企业和个人做出正确决策。然而，一些统计现象可能会违反直觉，可能导致分析中的悖论和偏差，这将破坏我们的分析。

我将向你解释的这些悖论易于理解，不包括复杂的公式。

在本文中，我们将探讨数据科学家应了解的5个统计悖论：准确性悖论、假阳性悖论、赌徒谬论、辛普森悖论和伯克森悖论。

这些悖论中的每一个都可能是导致你分析结果不可靠的潜在原因。

![数据科学家应了解的5个统计悖论](../Images/ab597d5545e0e90fe48acd142317dcda.png)

图片由作者提供

我们将讨论这些悖论的定义和现实生活中的例子，以说明这些悖论如何在现实数据分析中发生。理解这些悖论将帮助你消除可能影响可靠统计分析的障碍。

那么，事不宜迟，让我们深入探讨准确性悖论的世界。

# 准确性悖论

![数据科学家应了解的5个统计悖论](../Images/7e5cb497243a668808380deaf7e831d0.png)

图片由作者提供

准确性显示出在分类时准确性不是一个好的评价指标。

假设你正在分析一个包含1000个患者指标的数据集。你想检测一种罕见的疾病，这种疾病最终会在5%的人群中显现。所以总体上，你必须在1000人中找到50个人。

即使你总是说这些人没有疾病，你的准确率也会是95%。而你的模型在这个群体中不能捕捉到一个生病的人。（0/50）

## 数字数据集

让我们通过一个著名的数字数据集的例子来解释这一点。

该数据集包含从0到9的手写数字。

![5 个数据科学家应知道的统计悖论](../Images/f83f8067555a3e7802796201698e1671.png)

图片由作者提供

这是一个简单的多标签分类任务，但也可以被解读为图像识别，因为数字是以图像的形式呈现的。

现在我们将加载这些数据集，并重新塑造数据集以应用机器学习模型。我跳过了这些部分的解释，因为你可能也对这些部分很熟悉。如果不熟悉，可以尝试搜索数字数据集或MNIST数据集。MNIST数据集也包含相同类型的数据，但形状比这个更大。

好的，我们继续。

现在我们尝试预测数字是否为6。为此，我们将定义一个预测不是6的分类器。让我们查看这个分类器的交叉验证分数。

```py
from sklearn import datasets
from sklearn.model_selection import train_test_split
from sklearn.model_selection import cross_val_score
from sklearn.base import BaseEstimator
import numpy as np

digits = datasets.load_digits()
n_samples = len(digits.images)
data = digits.images.reshape((n_samples, -1))
x_train, x_test, y_train, y_test = train_test_split(
    data, digits.target, test_size=0.5, shuffle=False
)
y_train_6 = y_train == 6

from sklearn.base import BaseEstimator

class DumbClassifier(BaseEstimator):
    def fit(self, X, y=None):
        pass

    def predict(self, X):
        return np.zeros((len(X), 1), dtype=bool)

dumb_clf = DumbClassifier()

cross_val_score(dumb_clf, x_train, y_train_6, cv=3, scoring="accuracy") 
```

结果将如下所示。

![5 个数据科学家应知道的统计悖论](../Images/9b625149f8b42f496b2269390400afc6.png)

这意味着什么？这意味着即使你创建一个从不估计6的估计器并将其放入你的模型中，准确率也可能超过90%。为什么？因为我们的数据集中存在其他9个数字。所以如果你说这个数字不是6，你会有9/10的准确率。

这表明选择评估指标非常重要。如果你想评估分类任务，准确率不是一个好的选择。你应该选择精确度或召回率。

那些是什么？它们出现在假阳性悖论中，所以请继续阅读。

# 假阳性悖论

![5 个数据科学家应知道的统计悖论](../Images/00f8a4860e442a84ed155a9360c6e82b.png)

图片由作者提供

现在，假阳性悖论是一种统计现象，当我们测试稀有事件或条件的存在时，可能会出现这种现象。

它也被称为“基本率谬误”或“基本率忽视”。

这个悖论意味着在测试稀有事件时，假阳性结果会多于正结果。

让我们看一个数据科学的例子。

## 欺诈检测

![5 个数据科学家应知道的统计悖论](../Images/a53755e8783272ec93fd6fdbf792ddd4.png)

图片由作者提供

想象一下你正在开发一个机器学习模型来检测欺诈性信用卡交易。你使用的数据集包括大量正常（非欺诈性）交易和少量欺诈交易。然而，当你在现实世界中部署模型时，你发现它产生了大量的假阳性。

经过进一步调查，你会发现现实世界中欺诈交易的发生率远低于训练数据集中的发生率。

假设1/10,000的交易是欺诈的，并且假设测试也有5%的假阳性率。

TP = 1/10,000

FP = 10,000*(100-40)/100*0,05 = 499,95 / 9,999

那么当发现一个欺诈交易时，它真的有可能是欺诈交易吗？

P = 1/500,95 =0,001996

结果接近0.2%。这意味着当事件被标记为欺诈时，实际为欺诈事件的概率仅为0.2%。

这就是**假阳性悖论**。

下面是如何在Python代码中实现它。

```py
import pandas as pd
import numpy as np

# Number of normal transactions
normal_count = 9999

# Number of fraudulent transactions
true_positive = 1

# Number of normal transactions flagged as fraudulent by the model
false_positives = 499.95

# Number of fraudulent transactions flagged as normal by the model
false_negatives = 0

# Calculate precision
precision = (true_positive) / true_positive + false_positives
print(f"Precision: {precision:.2f}")

# Calculate recall
recall = (fraud_count) / fraud_count + false_negatives
print(f"Recall: {recall:.2f}")

# Calculate accuracy
accuracy = (
    normal_count - false_positives + fraud_count - false_negatives
) / (normal_count + fraud_count)
print(f"Accuracy: {accuracy:.2f}") 
```

你可以看到，召回率非常高，但精确度却很低。

![5 个数据科学家应该了解的统计悖论](../Images/a6ffddff783cb2ffd84fa7200f126b97.png)

要理解为什么系统会这样做，让我解释一下精确度/召回率和精确度/召回率权衡。

召回率（真正率）也称为敏感度。你应该首先找到正面案例，并计算其中真正例的比例。

召回率 = TP / (TP + FP)

精确度是正向预测的准确率。

精确度 = TP / (TP + FN)

假设你希望有一个分类器来进行情感分析，并预测评论是正面的还是负面的。你可能希望有一个召回率高的分类器（它能够正确识别大量的正面或负面评论）。然而，为了获得更高的召回率，你应该能接受较低的精确度（错误分类正面评论），因为删除负面评论比偶尔删除一些正面评论更为重要。

另一方面，如果你想构建一个垃圾邮件分类器，你可能希望有一个精确度高的分类器。它能准确识别高比例的垃圾邮件，但偶尔会漏掉一些垃圾邮件，因为保持重要邮件更为重要。

现在，在我们的案例中，为了找到欺诈交易，你牺牲了许多非欺诈的错误。然而，如果你这样做，你也必须采取预防措施，例如在银行系统中。当他们检测到欺诈交易时，他们开始进行进一步调查，以确保绝对准确。

通常，当进行超过预设限额的交易时，他们会向你的手机或电子邮件发送消息以进行进一步确认等。

如果你允许你的模型有**假阴性**，那么你的召回率将会很低。然而，如果你允许你的模型有**假阳性**，你的精确度将会低。

作为数据科学家，你应该调整你的模型或添加一个步骤进行进一步调查，因为可能会有很多**假阳性**。

# 赌徒谬误

![5 个数据科学家应该了解的统计悖论](../Images/060b9846e420e7211b1a203ecb0997e1.png)

图片由作者提供

**赌徒谬误**，也称为**蒙特卡洛谬误**，是一种错误的信念，即如果某事件发生的频率高于其正常概率，它将在随后的试验中更频繁地发生。

让我们来看一个数据科学领域的例子。

## 客户流失

![5 个数据科学家应该了解的统计悖论](../Images/7e5c9e2ea97ba717445d90018296629f.png)

图片由作者提供

设想一下，你正在构建一个机器学习模型来预测客户是否会流失，基于他们的历史行为。

现在，你已经收集了许多不同类型的数据，包括与服务互动的客户数量、客户成为客户的时间长度、客户提出的投诉数量等等。

在这一点上，你可能会觉得长期使用服务的客户流失的可能性较小，因为他们在过去对服务表现出了承诺。

然而，这仍然是一个赌徒谬误的例子，因为客户流失的概率不受他们成为客户的时间长度的影响。

流失概率是由多种因素决定的，包括服务质量、客户对服务的满意度等。

因此，如果你构建一个机器学习模型，请特别注意不要创建包含客户时间长度的列，并试图用它来解释模型。此时，你应该意识到这可能会因为赌徒谬误而毁坏你的模型。

现在，这只是一个概念性的例子。让我们通过一个硬币抛掷的例子来解释这一点。

首先让我们看看硬币抛掷概率的变化。你可能会觉得如果硬币多次出现正面，未来出现正面的可能性会减少。这实际上是赌徒谬误的一个很好的例子。

正如你所见，在开始时，可能性有所波动。然而，当翻转次数增加时，得到正面的概率将趋于0.5。

```py
import random
import matplotlib.pyplot as plt

# Set up the plot
plt.xlabel("Flip Number")
plt.ylabel("Probability of Heads")

# Initialize variables
num_flips = 1000
num_heads = 0
probabilities = []

# Simulate the coin flips
for i in range(num_flips):
    if (
        random.random() > 0.5
    ):  # random() generates a random float between 0 and 1
        num_heads += 1
    probability = num_heads / (i + 1)  # Calculate the probability of heads
    probabilities.append(probability)  # Record the probability
# Plot the results
plt.plot(probabilities)
plt.show()
```

现在，让我们看看输出结果。

![5个数据科学家应该知道的统计悖论](../Images/15aaa2df1535b4163ffe9b66968f3a7e.png)

作者提供的图片

显然，概率会随时间波动，但最终它会趋向于0.5。

这个例子展示了赌徒谬误，因为之前翻转的结果不会影响任何一次翻转中得到正面的概率。概率始终保持在50%，无论过去发生了什么。

# 辛普森悖论

![5个数据科学家应该知道的统计悖论](../Images/d2be3dd08034f53b63d44c5aeaf234a2.png)

图片由[Roland Steinmann](https://pixabay.com/users/rollstein-13853955/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7041935)提供，来源于[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=7041935)

这个悖论发生在当数据被汇总时，两变量之间的关系似乎发生了变化。

现在，为了解释这个悖论，我们来使用seaborn中的内置数据集tips。

## 提示

![5个数据科学家应该知道的统计悖论](../Images/0de06f07e11116533b3bd5bacdc25a7f.png)

作者提供的图片

为了解释辛普森悖论，我们将使用tips数据集计算女性和男性在午餐时间和整体的平均小费。tips数据集包含客户在餐馆给出的提示数据，如总小费、性别、日期、时间等。

小费数据集是顾客在餐馆给小费的数据集合。它包括小费金额、顾客性别、星期几和一天中的时间等信息。该数据集可用于分析顾客的小费行为并识别数据中的趋势。

```py
import seaborn as sns

# Load the tips dataset
tips = sns.load_dataset("tips")

# Calculate the tip percentage for men and women at lunch
men_lunch_tip_pct = (
    tips[(tips["sex"] == "Male") & (tips["time"] == "Lunch")]["tip"].mean()
    / tips[(tips["sex"] == "Male") & (tips["time"] == "Lunch")][
        "total_bill"
    ].mean()
)
women_lunch_tip_pct = (
    tips[(tips["sex"] == "Female") & (tips["time"] == "Lunch")]["tip"].mean()
    / tips[(tips["sex"] == "Female") & (tips["time"] == "Lunch")][
        "total_bill"
    ].mean()
)

# Calculate the overall tip percentage for men and women
men_tip_pct = (
    tips[tips["sex"] == "Male"]["tip"].mean()
    / tips[tips["sex"] == "Male"]["total_bill"].mean()
)
women_tip_pct = (
    tips[tips["sex"] == "Female"]["tip"].mean()
    / tips[tips["sex"] == "Female"]["total_bill"].mean()
)

# Create a data frame with the average tip percentages
data = {
    "Lunch": [men_lunch_tip_pct, women_lunch_tip_pct],
    "Overall": [men_tip_pct, women_tip_pct],
}
index = ["Men", "Women"]
df = pd.DataFrame(data, index=index)
df 
```

好的，这就是我们的数据框。

![5 个数据科学家应了解的统计悖论](../Images/aa1619317f018492aa5e4be7d85713f8.png)

如我们所见，午餐时男性和女性的平均小费更高。然而，当数据汇总时，均值发生了变化。

让我们看一下柱状图，以查看变化。

```py
import matplotlib.pyplot as plt

# Set the group labels
labels = ["Lunch", "Overall"]

# Set the bar heights
men_heights = [men_lunch_tip_pct, men_tip_pct]
women_heights = [women_lunch_tip_pct, women_tip_pct]

# Create a figure with two subplots
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 5))

# Create the bar plot
ax1.bar(labels, men_heights, width=0.5, label="Men")
ax1.bar(labels, women_heights, width=0.3, label="Women")
ax1.set_title("Average Tip Percentage by Gender (Bar Plot)")
ax1.set_xlabel("Group")
ax1.set_ylabel("Average Tip Percentage")
ax1.legend()

# Create the line plot
ax2.plot(labels, men_heights, label="Men")
ax2.plot(labels, women_heights, label="Women")
ax2.set_title("Average Tip Percentage by Gender (Line Plot)")
ax2.set_xlabel("Group")
ax2.set_ylabel("Average Tip Percentage")
ax2.legend()

# Show the plot
plt.show() 
```

这是输出结果。

![5 个数据科学家应了解的统计悖论](../Images/e33083b2b5729ad54d7f5aa8a5ecd6cf.png)

图片来源：作者

现在，如你所见，数据汇总后平均值发生了变化。突然间，你会看到总体上女性的小费比男性更多。

## 有什么陷阱？

在观察子集版本的趋势并从中提取意义时，要小心不要忘记检查这一趋势是否仍然适用于整个数据集。因为如你所见，特殊情况下可能并非如此。这可能导致数据科学家做出误判，从而做出糟糕的（商业）决策。

# 伯克森悖论

伯克森悖论是一种统计悖论，当两个变量在数据中相互关联时，但当数据被子集化或分组时，这种相关性没有被观察到或发生了变化。

简单来说，伯克森悖论是指在数据的不同子组中，相关性看起来有所不同。

现在，让我们通过分析 Iris 数据集来深入了解。

## Iris 数据集

![5 个数据科学家应了解的统计悖论](../Images/6c45311109b9ca66517b029f522e5ae5.png)

图片来源：作者

Iris 数据集是机器学习和统计学中常用的数据集。它包含了不同鸢尾花观测的数据，包括花瓣和萼片的长度和宽度以及观察到的花卉种类。

在这里，我们将绘制两个图表，显示萼片长度和宽度之间的关系。但在第二个图表中，我们将物种过滤为 Setosa。

```py
import seaborn as sns
import matplotlib.pyplot as plt
from scipy.stats import linregress

# Load the iris data set
df = sns.load_dataset("iris")

# Subset the data to only include setosa species
df_s = df[df["species"] == "setosa"]

# Create a figure with two subplots
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(10, 5))

# Plot the relationship between sepal length and width.
slope, intercept, r_value, p_value, std_err = linregress(
    df["sepal_length"], df["sepal_width"]
)
ax1.scatter(df["sepal_length"], df["sepal_width"])
ax1.plot(
    df["sepal_length"],
    intercept + slope * df["sepal_length"],
    "r",
    label="fitted line",
)
ax1.set_xlabel("Sepal Length")
ax1.set_ylabel("Sepal Width")
ax1.set_title("Sepal Length and Width")
ax1.legend([f"R^2 = {r_value:.3f}"])

# Plot the relationship between setosa sepal length and width for setosa.
slope, intercept, r_value, p_value, std_err = linregress(
    df_s["sepal_length"], df_s["sepal_width"]
)
ax2.scatter(df_s["sepal_length"], df_s["sepal_width"])
ax2.plot(
    df_s["sepal_length"],
    intercept + slope * df_s["sepal_length"],
    "r",
    label="fitted line",
)
ax2.set_xlabel("Setosa Sepal Length")
ax2.set_ylabel("Setosa Sepal Width")
ax2.set_title("Setosa Sepal Length and Width ")
ax2.legend([f"R^2 = {r_value:.3f}"])

# Show the plot
plt.show() 
```

你可以看到萼片长度与 Setosa 物种之间的变化。实际上，它显示了与其他物种不同的相关性。

![5 个数据科学家应了解的统计悖论](../Images/596dda1398e2941cbdb0a0f11cbc5f06.png)

图片来源：作者

此外，你可以在第一个图中看到 Setosa 的不同相关性。

在第二个图表中，你可以看到萼片宽度和萼片长度之间的相关性发生了变化。当分析所有数据集时，显示了萼片长度增加时，萼片宽度减少。然而，如果我们开始分析选择 Setosa 物种的数据，相关性现在为正，并且显示萼片宽度增加时，萼片长度也增加。

```py
import seaborn as sns
import matplotlib.pyplot as plt
from scipy.stats import linregress

# Load the tips data set
df = sns.load_dataset("iris")

# Subset the data to only include setosa species
df_s = df[df["species"] == "setosa"]

# Create a figure with two subplots
fig, ax1 = plt.subplots(figsize=(5, 5))

# Plot the relationship between sepal length and width.
slope, intercept, r_value_1, p_value, std_err = linregress(
    df["sepal_length"], df["sepal_width"]
)
ax1.scatter(df["sepal_length"], df["sepal_width"], color="blue")
ax1.plot(
    df["sepal_length"],
    intercept + slope * df["sepal_length"],
    "b",
    label="fitted line",
)

# Plot the relationship between setosa sepal length and width for setosa.
slope, intercept, r_value_2, p_value, std_err = linregress(
    df_s["sepal_length"], df_s["sepal_width"]
)
ax1.scatter(df_s["sepal_length"], df_s["sepal_width"], color="red")
ax1.plot(
    df_s["sepal_length"],
    intercept + slope * df_s["sepal_length"],
    "r",
    label="fitted line",
)

ax1.set_xlabel("Sepal Length")
ax1.set_ylabel("Sepal Width")
ax1.set_title("Sepal Length and Width")
ax1.legend([f"R = {r_value_1:.3f}"]) 
```

这里是图表。

![5 个数据科学家应了解的统计悖论](../Images/af9b8193b2d5bb1aa2229c678a8512c4.png)

图片来源：作者

你可以看到，从分析 setosa 开始并将其花萼宽度和长度的相关性进行概括，可能会导致你根据分析得出错误的结论。

# 结论

在这篇文章中，我们探讨了数据科学家应当了解的五个统计学悖论，以便进行准确的分析。假设你发现数据集中存在某种趋势，表明花萼长度增加时，花萼宽度也增加。然而，当你查看整个数据集时，情况实际上完全相反。

或者你可能通过查看准确度来评估你的分类模型。你会发现，即使是完全无操作的模型也能实现超过 90% 的准确率。如果你试图通过准确度来评估模型并进行分析，考虑一下你可能会犯多少错误。

通过理解这些悖论，我们可以采取措施避免常见的陷阱，并提高统计分析的可靠性。用健康的怀疑态度来对待数据分析，避免分析中的潜在悖论和局限性也是很重要的。

总之，这些悖论对于数据科学家在高水平分析时非常重要，因为意识到这些悖论可以提高分析的准确性和可靠性。我们还推荐这份 “[统计学备忘单](https://www.stratascratch.com/blog/a-comprehensive-statistics-cheat-sheet-for-data-science-interviews/?utm_source=blog&utm_medium=click&utm_campaign=kdn+statistical+paradoxes)”，它可以帮助你理解统计学和概率的重要术语和方程，并帮助你准备下一次数据科学面试。

感谢阅读！

**[Nate Rosidi](https://www.stratascratch.com)** 是一位数据科学家，专注于产品策略。他还是一名兼职教授，教授分析学，并且是 [StrataScratch](https://www.stratascratch.com/)，一个帮助数据科学家准备面试的平台，提供来自顶级公司的真实面试问题。可以通过 [Twitter: StrataScratch](https://twitter.com/StrataScratch) 或 [LinkedIn](https://www.linkedin.com/in/nathanrosidi/) 与他联系。

### 相关主题

+   [KDnuggets 新闻，4 月 13 日：数据科学家应知的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [2022 年数据科学家应该知道的 Python 库](https://www.kdnuggets.com/2022/04/python-libraries-data-scientists-know-2022.html)

+   [数据科学家应该了解的 OpenUSD](https://www.kdnuggets.com/what-data-scientists-should-know-about-openusd)

+   [KDnuggets™ 新闻 22:n03，1 月 19 日：对 13 个数据的深度分析…](https://www.kdnuggets.com/2022/n03.html)

+   [KDnuggets 新闻，5 月 25 日：每个…都应该知道的 6 种 Python 机器学习工具](https://www.kdnuggets.com/2022/n21.html)

+   [数据科学家为何应使用 LightGBM 的 3 个理由](https://www.kdnuggets.com/2022/01/data-scientists-reasons-lightgbm.html)
