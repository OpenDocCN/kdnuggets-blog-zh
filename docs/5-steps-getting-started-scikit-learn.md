# 使用 Scikit-learn 的 5 个步骤入门

> 原文：[https://www.kdnuggets.com/5-steps-getting-started-scikit-learn](https://www.kdnuggets.com/5-steps-getting-started-scikit-learn)

![使用 Scikit-learn 的 5 个步骤入门](../Images/1d3ecbf678f59808a4d9eb7d90d3f39b.png)

# Scikit-learn 简介

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

学习如何使用 [Scikit-learn](https://scikit-learn.org/stable/) 时，我们必须显然了解机器学习的基本概念，因为 Scikit-learn 只是实现机器学习原理和相关任务的实用工具。机器学习是人工智能的一个子集，它使计算机能够从经验中学习和改进，而无需明确编程。算法使用训练数据通过发现模式和洞察来做出预测或决策。机器学习有三种主要类型：

+   监督学习 - 模型在标记数据上进行训练，学习将输入映射到输出

+   无监督学习 - 模型在未标记的数据中发现隐藏的模式和分组

+   强化学习 - 模型通过与环境互动进行学习，接受奖励和惩罚以鼓励最佳行为

正如你无疑知道的，机器学习驱动了现代社会的许多方面，生成了大量数据。随着数据可用性持续增长，机器学习的重要性也在提升。

Scikit-learn 是一个流行的开源 Python 机器学习库。其广泛使用的一些关键原因包括：

+   简单高效的数据分析和建模工具

+   对 Python 程序员友好，注重清晰性

+   基于 NumPy、SciPy 和 matplotlib，便于集成

+   用于分类、回归、聚类、降维等任务的广泛算法

本教程旨在提供一个逐步介绍如何使用 Scikit-learn（主要针对常见的监督学习任务），重点是通过大量实践示例入门。

# 步骤 1：使用 Scikit-learn 入门

## 安装和设置

为了安装和使用 Scikit-learn，你的系统必须有一个正常运行的 Python 安装。我们在这里不会涵盖这个内容，但会假设你此时已有一个正常运行的安装。

Scikit-learn 可以使用 pip，Python 的包管理器进行安装：

```py
pip install scikit-learn
```

这也将安装所有必需的依赖项，如 NumPy 和 SciPy。安装完成后，可以在你的 Python 脚本中如下导入 Scikit-learn：

```py
import sklearn
```

## 测试你的安装

安装完成后，你可以启动 Python 解释器并运行上述的导入命令。

```py
Python 3.10.11 (main, May 2 2023, 00:28:57) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sklearn
```

只要你没有看到任何错误信息，你现在就可以开始使用 Scikit-learn 了！

## 加载示例数据集

Scikit-learn 提供了多种示例数据集，我们可以用于测试和实验：

```py
from sklearn import datasets

iris = datasets.load_iris()
digits = datasets.load_digits()
```

数字数据集包含手写数字的图像及其标签。我们可以利用这些示例数据集来熟悉 Scikit-learn，然后再转向实际数据。

# 第2步：数据预处理

## 数据预处理的重要性

实际数据往往是不完整、不一致的，并且包含错误。数据预处理将原始数据转换为可用于机器学习的格式，是一个重要的步骤，可以影响后续模型的性能。

许多新手往往忽视了数据预处理，而是直接进入模型训练。然而，无论所用算法多么复杂，低质量的数据输入都会导致低质量的模型输出。像正确处理缺失数据、检测和移除异常值、特征编码和特征缩放这样的步骤，有助于提高模型的准确性。

数据预处理占据了机器学习项目中大量的时间和精力。老电脑科学格言“垃圾进，垃圾出”在这里非常适用。高质量的数据输入是高性能机器学习的前提。数据预处理步骤将原始数据转换为精炼的训练集，使机器学习算法能够有效地发现预测模式和洞察。

总之，数据预处理是任何机器学习工作流中不可或缺的一步，应给予充分关注和认真对待。

## 加载和理解数据

让我们使用 Scikit-learn 加载一个示例数据集进行演示：

```py
from sklearn.datasets import load_iris
iris_data = load_iris()
```

我们可以探索特征和目标值：

```py
print(iris_data.data[0]) # Feature values for first sample
print(iris_data.target[0]) # Target value for first sample
```

在继续之前，我们应当了解特征和目标的含义。

## 数据清洗

实际数据经常包含缺失、损坏或异常值。Scikit-learn 提供了处理这些问题的工具：

```py
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy='mean')  
imputed_data = imputer.fit_transform(iris_data.data)
```

插补器用均值替代缺失值，这是一种常见的——但不是唯一的——策略。这只是数据清洗的一种方法。

## 特征缩放

像支持向量机（SVM）和神经网络这样的算法对输入特征的尺度非常敏感。不一致的特征尺度可能导致这些算法对尺度较大的特征赋予过多的权重，从而影响模型的性能。因此，在训练这些算法之前，规范化或标准化特征以将其调整到类似的尺度是至关重要的。

```py
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaled_data = scaler.fit_transform(iris_data.data)
```

StandardScaler 将特征标准化为均值 0 和方差 1。还提供了其他缩放器。

## 数据可视化

我们还可以使用 matplotlib 对数据进行可视化，以获得更多见解：

```py
import matplotlib.pyplot as plt
plt.scatter(iris_data.data[:, 0], iris_data.data[:, 1], c=iris_data.target)
plt.xlabel('Sepal Length')
plt.ylabel('Sepal Width')
plt.show()
```

数据可视化在机器学习工作流中发挥着多个关键功能。它可以让你发现数据中的潜在模式和趋势，识别可能影响模型性能的异常值，并更深入地理解变量之间的关系。通过提前可视化数据，你可以在特征选择和模型训练阶段做出更明智的决策。

# 步骤 3：模型选择与训练

## Scikit-learn 算法概述

Scikit-learn 提供了多种有监督和无监督的算法：

+   分类：逻辑回归、SVM、朴素贝叶斯、决策树、随机森林

+   回归：线性回归、SVR、决策树、随机森林

+   聚类：k-均值、DBSCAN、凝聚聚类

以及其他许多。

## 选择算法

选择最合适的机器学习算法对于构建高质量模型至关重要。最佳算法取决于多个关键因素：

+   可用的训练数据的大小和类型。是小数据集还是大数据集？包含什么样的特征——图像、文本、数字？

+   可用的计算资源。算法在计算复杂性上有所不同。简单的线性模型训练速度比深度神经网络快。

+   我们想解决的具体问题是什么？我们是在进行分类、回归、聚类，还是更复杂的任务？

+   是否有任何特殊要求，比如对可解释性的需求。线性模型比黑箱方法更具可解释性。

+   期望的准确度/性能。有些算法在某些任务上的表现优于其他算法。

对于我们这个特定的分类鸢尾花问题，像逻辑回归或支持向量机这样的分类算法最为合适。这些算法可以根据提供的特征测量有效地对花朵进行分类。其他简单的算法可能无法提供足够的准确性。同时，像深度神经网络这样的复杂方法对于这个相对简单的数据集来说过于复杂。

随着我们不断训练模型，始终选择最适合我们当前问题的算法至关重要，基于上述考虑因素。可靠地选择合适的算法将确保我们开发出高质量的机器学习系统。

## 训练一个简单模型

让我们训练一个逻辑回归模型：

```py
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()
model.fit(scaled_data, iris_data.target)
```

就这些！模型已训练好，准备好进行评估和使用。

## 训练一个更复杂的模型

虽然像逻辑回归这样的简单线性模型通常可以提供不错的性能，但对于更复杂的数据集，我们可能需要利用更先进的算法。例如，集成方法结合了多个模型，使用诸如袋装（bagging）和提升（boosting）等技术，以提高整体预测准确性。举例来说，我们可以训练一个随机森林分类器，它汇聚了许多决策树：

```py
from sklearn.ensemble import RandomForestClassifier

rf_model = RandomForestClassifier(n_estimators=100) 
rf_model.fit(scaled_data, iris_data.target)
```

随机森林可以捕捉特征之间的非线性关系和复杂交互，从而比任何单一的决策树产生更准确的预测。我们还可以使用像 SVM、梯度提升树和神经网络这样的算法，以在具有挑战的数据集上获得进一步的性能提升。关键是要尝试不同的算法，超越简单的线性模型，以发挥它们的优势。

然而，请注意，无论是使用简单算法还是更复杂的算法进行模型训练，Scikit-learn 的语法都允许使用相同的方法，从而显著减少学习曲线。事实上，几乎所有使用该库的任务都可以通过 fit/transform/predict 模式来表达。

# 第 4 步：模型评估

## 评估的重要性

评估机器学习模型的性能是最终部署到生产环境之前一个绝对关键的步骤。全面评估模型建立了系统在部署后能够可靠运行的基本信任。同时，这也能识别需要改进的潜在领域，以提高模型的预测准确性和泛化能力。一个模型可能在其训练数据上表现得非常准确，但在实际数据上却可能表现不佳。这突显了在持出的测试集和新数据上测试模型的重要性，而不仅仅是训练数据。

我们必须模拟模型在部署后的表现。严格评估模型还提供了对可能的过拟合情况的见解，即模型记住了训练数据中的模式，但未能学习到对样本外预测有用的可泛化关系。检测到过拟合会促使采取适当的对策，如正则化和交叉验证。评估还允许比较多个候选模型，以选择表现最佳的选项。那些没有比简单基准模型提供足够提升的模型可能需要重新设计或完全替换。

总结来说，全面评估机器学习模型对于确保其可靠性和增加价值是不可或缺的。这不仅仅是一个可选的分析任务，而是模型开发工作流中一个 integral 的部分，能够部署真正有效的系统。因此，机器学习从业者应在考虑部署之前，投入大量精力在相关性能指标上正确评估他们的模型。

## 训练/测试拆分

我们将数据拆分以评估模型在新数据上的表现：

```py
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(scaled_data, iris_data.target)
```

按惯例，X 代表特征，y 代表目标变量。请注意，`y_test` 和 `iris_data.target` 是指代相同数据的不同方式。

## 评估指标

对于分类任务，关键指标包括：

+   准确率：正确预测的总体比例

+   精确率：实际为正的预测中正预测的比例

+   召回率：实际为正的预测中正预测的比例

可以通过 Scikit-learn 的分类报告来计算这些指标：

```py
from sklearn.metrics import classification_report

print(classification_report(y_test, model.predict(X_test)))
```

这让我们对模型性能有了深入了解。

# 步骤 5：提升性能

## 超参数调整

超参数是模型配置设置。调整它们可以提高性能：

```py
from sklearn.model_selection import GridSearchCV

params = {'C': [0.1, 1, 10]}
grid_search = GridSearchCV(model, params, cv=5)
grid_search.fit(scaled_data, iris_data.target)
```

这在不同的正则化强度下进行网格搜索，以优化模型的准确性。

## 交叉验证

交叉验证提供了更可靠的超参数评估：

```py
from sklearn.model_selection import cross_val_score

cross_val_scores = cross_val_score(model, scaled_data, iris_data.target, cv=5)
```

它将数据划分为 5 个折叠，并在每个折叠上评估性能。

## 集成方法

结合多个模型可以提升性能。为了演示这一点，让我们首先训练一个随机森林模型：

```py
from sklearn.ensemble import RandomForestClassifier

random_forest = RandomForestClassifier(n_estimators=100)
random_forest.fit(scaled_data, iris_data.target)
```

现在我们可以继续创建一个使用逻辑回归和随机森林模型的集成模型：

```py
from sklearn.ensemble import VotingClassifier

voting_clf = VotingClassifier(estimators=[('lr', model), ('rf', random_forest)])
voting_clf.fit(scaled_data, iris_data.target)
```

这个集成模型结合了我们之前训练的逻辑回归模型（称为 `lr`）和新定义的随机森林模型（称为 `rf`）。

## 模型堆叠与融合

更高级的集成技术如堆叠和融合构建一个元模型来组合多个基础模型。在单独训练基础模型之后，元模型学习如何最好地组合它们以获得最佳性能。这比简单的平均或投票集成提供了更多的灵活性。元学习者可以学习哪些模型在不同的数据段上表现最佳。使用多样化基础模型的堆叠和融合集成通常在许多机器学习任务中取得了最先进的结果。

```py
# Train base models
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC

rf = RandomForestClassifier()
svc = SVC()

rf.fit(X_train, y_train)
svc.fit(X_train, y_train)

# Make predictions to train meta-model
rf_predictions = rf.predict(X_test)
svc_predictions = svc.predict(X_test)

# Create dataset for meta-model
blender = np.vstack((rf_predictions, svc_predictions)).T
blender_target = y_test

# Fit meta-model on predictions
from sklearn.ensemble import GradientBoostingClassifier

gb = GradientBoostingClassifier()
gb.fit(blender, blender_target)

# Make final predictions
final_predictions = gb.predict(blender) 
```

这将分别训练一个随机森林和一个支持向量机（SVM）模型，然后在它们的预测结果上训练一个梯度提升树以产生最终输出。关键步骤是从基础模型生成测试集上的预测，然后使用这些预测作为输入特征来训练元模型。

# 继续前进

Scikit-learn 提供了一个广泛的 Python 机器学习工具包。在本教程中，我们介绍了使用 Scikit-learn 的完整机器学习工作流程——从安装库和理解其功能，到加载数据、训练模型、评估模型性能、调整超参数和编译集成模型。由于其设计良好的 API、算法的广度和与 PyData 堆栈的集成，Scikit-learn 已成为非常受欢迎的库。Sklearn 使用户能够快速高效地构建模型并生成预测，而不会陷入实现细节中。凭借这一坚实的基础，你现在可以使用 Scikit-learn 将机器学习实际应用于现实世界问题。下一步是识别适合使用机器学习技术的问题，并利用本教程中的技能提取价值。

当然，关于 Scikit-learn 以及机器学习的知识总是有更多的内容可以学习。该库实现了前沿算法，如神经网络、流形学习和深度学习，通过其估计器 API 提供支持。你可以通过研究这些方法的理论工作来不断提升你的能力。Scikit-learn 还与其他 Python 库如 Pandas 集成，以增加数据处理能力。此外，像 SageMaker 这样的产品提供了一个用于大规模操作 Scikit-learn 模型的生产平台。

本教程只是一个起点——Scikit-learn 是一个多功能工具包，将继续满足你在面对更高级挑战时的建模需求。关键是通过实践和完善你的技能来不断进步。对整个建模生命周期的实践经验是最好的老师。通过勤奋和创造力，Scikit-learn 提供了从各种数据中解锁深刻洞察的工具。

[**Matthew Mayo**](https://www.linkedin.com/in/mattmayo13/) ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 KDnuggets 的主编，Matthew 旨在使复杂的数据科学概念变得易于理解。他的职业兴趣包括自然语言处理、机器学习算法以及探索新兴的 AI。他致力于在数据科学社区中普及知识。Matthew 从 6 岁起就开始编程。

### 相关话题

+   [5 步骤开始使用 Python 数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [5 步骤开始使用 SQL](https://www.kdnuggets.com/5-steps-getting-started-with-sql)

+   [5 步骤开始使用 Google Cloud Platform](https://www.kdnuggets.com/5-steps-google-cloud-platform)

+   [5 步骤开始使用 PyTorch](https://www.kdnuggets.com/5-steps-getting-started-pytorch)

+   [5 步骤开始使用自动文本摘要](https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html)

+   [5 步骤开始清理数据](https://www.kdnuggets.com/2022/01/getting-started-cleaning-data.html)
