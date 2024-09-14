# 自动化机器学习简介

> 原文：[https://www.kdnuggets.com/2021/09/introduction-automated-machine-learning.html](https://www.kdnuggets.com/2021/09/introduction-automated-machine-learning.html)

[评论](#comments)

![intro-to-automl-spc.png](../Images/7a5bcc2c1448536e9fd3c2a3f2fbbf22.png)

## 什么是 AutoML 系统？

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT

* * *

机器学习（ML）作为更广泛的人工智能（AI）领域的一个子领域，正在接管各种行业和业务领域。这包括零售、医疗、汽车、金融、娱乐等。随着在各类操作中的更广泛应用以及来自具有多样技能的劳动力，学习如何使用机器学习变得越来越重要。

由于**组织中不断增加的员工交叉层面**对 ML 使用的扩展，开发可供各种背景的业务专业人士使用的系统变得至关重要。这意味着这些系统**不能仅限于编码或编程导向**，像软件工程师或数据科学家使用的那些。

**这就是 AutoML 系统登场的地方。**

简而言之，AutoML 使**拥有有限 ML 专业知识**（以及编码经验）的开发人员能够训练出特定于他们**业务需求**的高质量模型。虽然从学术研究角度看，AutoML 还有其他方面（即搜索适用于给定数据类型的最佳 ML 算法并证明这些系统的理论属性）；但本文将重点讨论适用于日常业务和技术应用的 AutoML 系统。

## 为什么 AutoML 系统越来越受欢迎？

如前所述，各种背景和技能的专业人士正在进入数据科学和机器学习领域，以满足各自的业务或研发活动。**并不是所有人都有严谨的背景或正式的统计科学或机器学习理论培训**。

他们只需能够获取或摄取数据集，遵循完整的数据探索和模型训练工作流程，并为下游的 ML 应用生成训练好的模型。

如果他们不得不自己完成所有这些工作，将会更加**耗时**：

+   搜索所有可能的 ML 算法，

+   在训练数据集上评估它们，

+   以严格的方式检查验证集性能，

+   应用额外的判断（内存和CPU占用）以选择适合最终应用的最佳模型。

在很大程度上，AutoML系统为数据科学家或分析师完成了所有这些**繁重的工作**，并自动化或简化了生产就绪使用的高度优化训练机器学习模型的生成。

因此，这些系统在广泛使用数据科学的组织中越来越受欢迎，因为它们：

+   节省组织的人力成本和人数，因为一个相对精简的员工队伍可以利用这些系统来训练和优化大量的机器学习模型。

+   通过自动化模型训练和优化，以及所谓的数据科学中的枯燥方面（即数据摄取、解析、清洗和特征探索等）来减少错误的机会。

+   减少相对高性能标准下的机器学习驱动应用程序的市场推出时间。

就市场推出时间而言，有人可能会争辩说，AutoML系统生成的最终结果可能不如由专家ML工程师（使用手工特征工程或深度学习调优）设计的结果表现优异或优化得好。

然而，这种手动调整通常是一个耗时的过程，在大多数情况下，与其浪费时间去生产绝对最佳表现的模型，不如在较短时间内使模型达到生产准备状态对组织更为关键。AutoML系统帮助组织实现这一关键业务目标，因此越来越受欢迎并广泛采用。

现在组织可以简单地为其员工购买[定制化机器学习工作站](https://www.sabrepc.com/Deep-Learning-GPU-Workstations)，安装像AutoML这样的工具，以快速启动一个基于机器学习的系统，并继续进行AI准备生产的下一步，从而比以往更快地将成果推向市场。

## AutoML系统类型及一些著名示例

有许多不同类型的AutoML系统，因为它们满足数据科学或机器学习工作流程中不同任务的需求。

一些类型的AutoML系统包括：

+   用于自动化参数调优的AutoML（一个相对基础的类型）

+   非深度学习的AutoML，例如**Auto-Sklearn**。这类系统主要应用于数据预处理、自动特征分析、自动特征检测、自动特征选择和自动模型选择。

+   深度学习/神经网络的AutoML，包括专门为神经网络架构搜索（NAS）设计的系统，以及像**AutoKeras**这样构建在流行的深度学习框架之上的实用工具包。

### **Auto-Sklearn**

正如其名，Auto-Sklearn 是一个基于 scikit-learn 的自动化机器学习软件包。Auto-Sklearn 使数据科学家免于算法选择和超参数调整的任务。本质上，它自动搜索适合新机器学习数据集的学习算法，并优化其超参数。

它扩展了通过有效的全局优化配置通用机器学习框架的思想，这一思想在 [Auto-WEKA](http://www.automl.org/automl/autoweka/) 中被引入。为了提高泛化能力，Auto-Sklearn 在全局优化过程中构建了一个**所有模型的集成**。为了加速优化过程，Auto-Sklearn 使用**元学习**来识别相似的数据集，并利用**过去积累的知识**。

它包括特征工程方法，如 [独热编码](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html)、数字特征标准化和主成分分析（PCA）。该库核心使用 scikit-learn 估计器来处理分类和回归问题。

![图示](../Images/074d8c81b28f478488124e78f219a941.png)

*Auto-Sklearn 工作流程和图示*

**文档**：你可以在 [这里找到详细文档](https://automl.github.io/auto-sklearn/master/)。

**代码示例**：这里展示了一个使用 Auto-Sklearn 的基本分类任务代码示例。我们使用 scikit-learn 包中的内置数据集构建一个用于分类数字的 ML 模型。

```py
import autosklearn.classification
import sklearn.model_selection
import sklearn.datasets
import sklearn.metrics

if __name__ == "__main__":
    X, y = sklearn.datasets.load_digits(return_X_y=True)
    X_train, X_test, y_train, y_test = \
            sklearn.model_selection.train_test_split(X, y, random_state=1)
    automl = autosklearn.classification.AutoSklearnClassifier()
    automl.fit(X_train, y_train)
    y_hat = automl.predict(X_test)
    print("Accuracy score", sklearn.metrics.accuracy_score(y_test, y_hat))
```

**关于操作系统的说明**：Auto-Sklearn 强烈依赖于 Python 模块资源。此模块是 Python 的 Unix 特定服务的一部分，Windows 机器上不可用。因此，截至今天，无法在 Windows 机器上运行 auto-sklearn。

可能的解决方案：

+   Windows 10 bash shell（见 431 和 860 获取建议）

+   虚拟机

+   Docker 镜像

### MLBox

![](../Images/b596e74e3e590571adae1e059e8d9301.png)

MLBox 是一个强大的自动化机器学习 Python 库。根据官方文档，它提供以下功能：

+   快速阅读和分布式数据预处理/清理/格式化

+   高度可靠的特征选择和泄漏检测，以及准确的超参数优化

+   最先进的分类和回归预测模型（深度学习、堆叠、LightGBM）

+   带有模型解释的预测

+   MLBox 已在 Kaggle 上测试，并显示出良好的性能

+   流水线构建

### TPOT

![TPOT AutoML](../Images/958256004b67c4c801dfee6a22988536.png)

TPOT 是一个 Python 自动化机器学习工具，通过遗传编程优化机器学习流水线。它也构建在 scikit-learn 之上。

![图示](../Images/0507d7866679d6a5e34aff51a54166ce.png)

*示例 TPOT 流水线（来自 [https://epistasislab.github.io/tpot/](https://epistasislab.github.io/tpot/)）*

**文档**：这是 [文档链接](https://epistasislab.github.io/tpot/)。

**代码示例：**

```py
from tpot import TPOTClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
import numpy as np

iris = load_iris()
X_train, X_test, y_train, y_test = train_test_split(iris.data.astype(np.float64),
    iris.target.astype(np.float64), train_size=0.75, test_size=0.25, random_state=42)

tpot = TPOTClassifier(generations=5, population_size=50, verbosity=2, random_state=42)
tpot.fit(X_train, y_train)
print(tpot.score(X_test, y_test))
tpot.export('tpot_iris_pipeline.py') 
```

### **AutoKeras**

AutoKeras是由[德州A&M大学数据实验室](http://people.tamu.edu/~guangzhou92/Data_Lab/)开发的自动化机器学习开源软件库。基于深度学习框架**Keras**，AutoKeras提供了自动搜索深度学习模型架构和超参数的功能。

AutoKeras遵循经典的scikit-learn API设计，因此易于使用。该框架的目标是通过使用自动化的**神经网络架构搜索（NAS）**算法来简化机器学习实践和研究。

**文档**：这里是详细的[文档链接](https://autokeras.com/)。

**代码示例：** 这里是一个用于MNIST图像分类任务的示例代码。

```py
import numpy as np
import tensorflow as tf
from tensorflow.keras.datasets import mnist
import autokeras as ak

# Data loading
(x_train, y_train), (x_test, y_test) = mnist.load_data()

# Initialize the image classifier.
clf = ak.ImageClassifier(overwrite=True, max_trials=1)
# Feed the image classifier with training data.
clf.fit(x_train, y_train, epochs=10)

# Predict with the best model.
predicted_y = clf.predict(x_test)
print(predicted_y)

# Evaluate the best model with testing data.
print(clf.evaluate(x_test, y_test))
```

## AutoML系统及其重要性的总结

在本文中，我们首先解释了自动化机器学习框架或所谓的AutoML系统背后的核心思想。我们描述了它们的实用性以及为何它们在各种商业和技术组织中越来越受欢迎。我们还展示了一些突出的AutoML库和框架，并提供了相关的代码示例和架构图解。

了解这些强大的框架将对任何即将成为数据科学家的人员有益，因为它们将继续在能力和广泛应用方面发展。

请通过在下方留言告知我们您感兴趣的任何话题，或者随时通过[联系我们](https://www.sabrepc.com/Contact-Us)提出您的问题。

**简历：[Kevin Vu](https://www.kdnuggets.com/author/kevin-vu)** 管理Exxact Corp博客，并与许多才华横溢的作者合作，这些作者撰写有关深度学习不同方面的文章。

[原文](https://www.sabrepc.com/blog/Deep-Learning-and-AI/introduction-to-automl)。已获许可转载。

**相关：**

+   [如何创建AutoML管道优化沙箱](/2021/09/automl-pipeline-optimization-sandbox.html)

+   [使用FLAML + Ray Tune进行快速AutoML](/2021/09/fast-automl-flaml-ray-tune.html)

+   [前18名低代码和无代码机器学习平台](/2021/09/top-18-low-code-no-code-machine-learning-platforms.html)

### 更多相关话题

+   [使用Streamlit构建DIY自动化机器学习]（https://www.kdnuggets.com/2021/11/diy-automated-machine-learning-app.html）

+   [使用Python的自动化机器学习：案例研究](https://www.kdnuggets.com/2023/04/automated-machine-learning-python-case-study.html)

+   [使用Python的自动化机器学习：不同方法的比较…](https://www.kdnuggets.com/2023/03/automated-machine-learning-python-comparison-different-approaches.html)

+   [入门自动化文本摘要]（https://www.kdnuggets.com/2019/11/getting-started-automated-text-summarization.html）

+   [利用ChatGPT进行自动化数据清理和预处理](https://www.kdnuggets.com/2023/08/harnessing-chatgpt-automated-data-cleaning-preprocessing.html)

+   [AI-自动化网络安全：自动化什么？](https://www.kdnuggets.com/ai-automated-cybersecurity-what-to-automate)
