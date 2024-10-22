# 在 Scikit-learn 中可视化你的混淆矩阵

> 原文：[`www.kdnuggets.com/2022/09/visualizing-confusion-matrix-scikitlearn.html`](https://www.kdnuggets.com/2022/09/visualizing-confusion-matrix-scikitlearn.html)

# 介绍

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 工作

* * *

有许多机器学习算法以标准软件的形式打包来训练你的数据。因此，训练和构建算法是微不足道的。但区分经验丰富的数据科学家和新手的关键在于他们如何评估机器学习模型的质量。

在模型开发阶段，你会在给定的数据上训练一组算法，评估它们的性能，并最终选择表现最佳的模型。然而，我们如何定义度量标准以从候选算法中选择最佳模型在你的机器学习解决方案成功中起着至关重要的作用。

# 混淆矩阵

当我们谈论混淆矩阵时，总是在分类问题的背景下。以二分类（两类问题）为例。这里我们有一个二元或两个状态的变量，称为目标变量。任务是根据一些属性或自变量来预测该状态。

我们从预测状态与实际状态之间的状态交互中得到的四个计数形成了我们的混淆矩阵。

![在 Scikit-learn 中可视化你的混淆矩阵](img/80496a6d5fd6005d32caf8dfcfb4c429.png)

来源：[`www.explorium.ai/wp-content/uploads/2019/07/Confusion-1024x733.png`](https://www.explorium.ai/wp-content/uploads/2019/07/Confusion-1024x733.png)

# 混淆矩阵的组成部分

四个象限被定义为真正负例（TN）、真正正例（TP）、假正例（FP）、假负例（FN）。如果你不熟悉这些术语，它们的名字让你感到困惑，那么请继续阅读，下面的部分将揭开这些术语的神秘面纱：

**真正负例：** 每当我们说真正时，这意味着我们的预测与实际情况相符。真正负例意味着预测和实际情况都是负类。

**真正正例：** 同样地，当预测和实际情况都是正类时，称为真正正例。

类似于上面解释的真实类别，其中实际值和预测值一致，假类别则表示预测与实际值不匹配，即实际值。这是数据科学家的关注点。从乐观的角度来看，这个假类别也是数据科学家识别算法模型或数据本身限制和问题的关键驱动因素。这个过程被称为错误分析，重点关注预测值与实际值的偏差。

**假阳性：** 当预测为正而实际为负时称为假阳性。

**假阴性：** 类似于假阳性，假阴性是指预测为负但实际为正。

混淆矩阵因此表示所有 TP、TN、FP 和 FN 实例的计数。

# 理解派生指标

混淆矩阵中的四个数字独立提供了对模型性能的细粒度理解，但数据科学家需要一个单一的度量来帮助他们评估整体模型性能。这有助于将机器学习问题表述为最小化问题。因此，从这四个度量中派生出一些关键评估指标，具体如下：

**准确率：** 准确率定义为所有正确预测实例占所有实例的比例。数学表达式为：

![在 Scikit-learn 中可视化你的混淆矩阵](img/b3b37e0b4a935a4677e9bf3835bd2ba3.png)

当数据存在类别不平衡时，这不是一个稳健的指标。类别不平衡是指一个类别主导了另一个类别。

**精确度**：精确度衡量正预测值与实际值匹配的比例。数学表达式为：

![在 Scikit-learn 中可视化你的混淆矩阵](img/aaad482096ac9eb4de9c34e78b702132.png)

**召回率**：召回率衡量正确识别的正实例的比例。数学表达式为：

![在 Scikit-learn 中可视化你的混淆矩阵](img/517cac0b5a0e20b37dfbaff317840789.png)

**F1 或 F-beta**：虽然精确度（Precision）尝试最小化假阳性（FP），召回率（Recall）尝试最小化假阴性（FN），F-1 指标在精确度和召回率之间保持平衡，其定义为两者的调和均值。

F-beta 是精确度和召回率的加权调和均值。数学表达式为：

![在 Scikit-learn 中可视化你的混淆矩阵](img/d23cb064c20d67e74989481cc3966cfe.png)

当 β 值小于 1 时，会降低精确度的影响，反之亦然。当 β=1 时，F-beta 变为 F1。

现在我们掌握了分类问题的指标。让我们选择一个数据集，训练一个模型，并使用混淆矩阵评估其性能。

# Scikit-learn 实现

我们将考虑[Kaggle 上的心脏病数据集](https://www.kaggle.com/datasets/johnsmith88/heart-disease-dataset)，以构建一个预测患者是否容易患心脏病的模型。因此，这是一个二分类问题，其中“心脏病”是类别 1，“无心脏病”是类别 0。

## 导入库

```py
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import confusion_matrix, ConfusionMatrixDisplay
```

## 读取数据

```py
df = pd.read_csv('heart.csv')
df.head()
```

## 拆分训练集和测试集

```py
X = df[['age', 'sex', 'cp', 'trestbps', 'chol', 'fbs', 'restecg', 'thalach', 'exang', 'oldpeak', 'slope', 'ca', 'thal']]
y = df['target']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=4
```

## 训练模型

```py
clf = DecisionTreeClassifier()
clf.fit(X_train, y_train)
```

## 评估预测

```py
y_pred = clf.predict(X_test)
cm = confusion_matrix(y_test, y_pred)
ConfusionMatrixDisplay(cm, clf.classes_).plot()
```

![在 Scikit-learn 中可视化你的混淆矩阵](img/a8ffabe58e9b0e4c2cae94f72cd30675.png)

# 解释混淆矩阵并计算衍生指标

从上面的混淆矩阵中，我们得到四个数字：

+   **真正例**：149（当预测标签和真实标签都为 1 时）

+   **真正例**：156（当预测标签和真实标签都为 1 时）

+   **假正例**：0（当预测标签和真实标签都为 1 时）

+   **假负例**：3（当预测标签和真实标签都为 1 时）

衍生指标是通过前一部分中解释的数学表达式计算得出的：

+   **准确率**：（156+149）/（156+149+0+3） = 99.03%

+   **精度**：149/(149+0) = 100%

+   **召回率**：149/(149+3) = 98.03%

+   **F1**：2*149/(2*149+0+3) = 99%

# 摘要

在这篇文章中，我们了解了混淆矩阵在分类算法中的使用及其重要性。然后，我们学习了混淆矩阵的四个指标以及如何计算衍生指标及其优缺点。此外，文章还通过一个二分类问题的示例说明了如何展示混淆矩阵。希望这篇文章能帮助你理解评估模型性能的各种指标及何时使用哪一项。

**[Vidhi Chugh](https://vidhi-chugh.medium.com/)**是一位获奖的 AI/ML 创新领袖和 AI 伦理学家。她在数据科学、产品和研究的交汇点工作，致力于提供业务价值和洞察。她是数据中心科学的倡导者，也是数据治理领域的领先专家，旨在构建值得信赖的 AI 解决方案。

### 更多相关主题

+   [傻瓜指南：精度、召回率和混淆矩阵](https://www.kdnuggets.com/2020/01/guide-precision-recall-confusion-matrix.html)

+   [混淆矩阵、精度和召回率解释](https://www.kdnuggets.com/2022/11/confusion-matrix-precision-recall-explained.html)

+   [KDnuggets 新闻，11 月 16 日：LinkedIn 如何使用机器学习 •…](https://www.kdnuggets.com/2022/n45.html)

+   [数据可视化：Statology 入门](https://www.kdnuggets.com/visualizing-data-statology-primer)

+   [Python 中的稀疏矩阵表示](https://www.kdnuggets.com/2020/05/sparse-matrix-representation-python.html)

+   [将文本文件转换为 TF-IDF 矩阵使用 tfidfvectorizer](https://www.kdnuggets.com/2022/09/convert-text-documents-tfidf-matrix-tfidfvectorizer.html)
