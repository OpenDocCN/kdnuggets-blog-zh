# 如何在现实世界中解决机器学习问题

> 原文：[https://www.kdnuggets.com/2021/09/solve-machine-learning-problems-real-world.html](https://www.kdnuggets.com/2021/09/solve-machine-learning-problems-real-world.html)

[评论](#comments)

**由 [Pau Labarta Bajo](https://www.linkedin.com/in/pau-labarta-bajo-4432074b/)，数学家和数据科学家**。

![](../Images/6a20df68ee56f752143d201f851ed063.png)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

所以，你想成为一名专业的机器学习工程师？是否被诱惑去参加（又一个）机器学习的在线课程，以获得那第一份工作？

机器学习的在线课程和 Kaggle 风格的比赛是学习机器学习基础知识的极佳资源。然而，机器学习工程师的日常工作需要额外的技能，这些技能在课程中无法掌握。

在这篇文章中，我将给你4个建议，帮助你在现实世界中解决机器学习问题。我在 [Toptal](https://www.toptal.com/Bxdpg6/worlds-top-talent) 作为自由职业机器学习工程师工作时（经历了一番艰难的学习过程）学到了这些建议。顺便提一下，如果你也希望以自由职业的方式工作（强烈推荐！），你可以查看我 [关于如何成为自由职业数据科学家的博客文章](http://datamachines.xyz/2021/05/18/how-to-become-a-freelance-data-scientist/)。

![](../Images/120eed67351db4fadcfb3354a9029092.png)

*照片由 [Valdemaras D.](https://www.pexels.com/@valdemaras-d-784301?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 提供，来源于 [Pexels](https://www.pexels.com/photo/two-people-on-mountain-cliff-1647962/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)。*

## 机器学习课程与实践之间的差距

完成许多机器学习的在线课程看起来似乎是一个安全的学习路径。你跟随卷积网络课程中的代码，自己实现，然后大功告成！你就成了计算机视觉的专家！

好吧，你没有。

你还缺少4项关键技能，以在专业环境中构建成功的机器学习解决方案。

让我们开始吧！

### 1\. 首先理解业务问题，然后将其框架化为机器学习问题。

当你参加在线课程或参与 Kaggle 比赛时，你不需要定义要解决的机器学习问题。

你会被告知要解决什么问题（例如，预测房价），以及如何衡量你离良好解决方案的距离（例如，模型预测值与实际价格的均方误差）。他们还会给你所有数据，告诉你特征是什么，目标指标是什么。

有了这些信息后，你会直接进入解决方案空间。你会迅速探索数据并开始训练一个又一个模型，希望每次提交后都能在公共排行榜上爬升几个名次。技术型人才，比如软件和机器学习工程师，喜欢构建东西。我也包括在这个群体中。即使在理解需要解决的问题之前，我们也会这样做。我们知道工具，手指很灵活，所以我们在理解眼前的问题（即WHAT）之前，迅速跳入解决方案空间（即HOW）。

当你作为专业的数据科学家或机器学习工程师工作时，你需要在构建任何模型之前考虑几个问题。我总是在每个项目开始时提出3个问题：

+   **管理层希望改善的业务结果是什么？是否有明确的指标，还是我需要找出可以简化工作的替代指标？**

    在项目开始时与你所有相关的利益相关者沟通是至关重要的。他们通常比你拥有更多的业务背景，可以大大帮助你理解你需要瞄准的目标。在行业中，为正确的问题构建一个尚可的解决方案比为错误的问题构建一个出色的解决方案要好。学术研究往往正好相反。

    先回答这个第一个问题，你将知道你机器学习问题的目标指标。

+   **目前是否有正在生产环境中解决此问题的解决方案，例如其他模型或一些基于规则的启发式方法？**

    如果有，这就是你必须超越的基准，以便产生业务影响。否则，你可以通过实施非机器学习解决方案获得快速胜利。有时你可以实现一个简单而快速的启发式方法，这已经会带来影响。在行业中，今天一个尚可的解决方案比两个月后的一个出色解决方案要好。

    回答第二个问题，你将理解模型的表现需要达到什么程度才能产生影响。

+   **这个模型是作为黑箱预测器使用吗？还是我们打算将它作为帮助人类做出更好决策的工具？**

    创建黑箱解决方案比创建可解释的解决方案要容易。例如，如果你想构建一个比特币交易机器人，你只关心它产生的预计利润。你会回测它的表现，看看这个策略是否给你带来价值。你的计划是部署机器人，监控它的每日表现，并在它让你亏钱时关闭它。你并不试图通过查看模型来了解市场。另一方面，如果你创建一个模型来帮助医生提高诊断能力，你需要创建一个可以轻松向他们解释的模型。否则，那95%的预测准确率将没有用处。

    回答第三个问题，你将知道是否需要额外花时间处理可解释性，还是可以完全专注于最大化准确性。

回答这三个问题，你将明白你需要解决的机器学习问题是什么。

### 2\. 专注于获取更多更好的数据

在在线课程和Kaggle竞赛中，组织者会提供所有数据。实际上，所有参与者使用相同的数据，彼此竞争谁的模型更好。重点在于模型，而不是数据。

在你的工作中，恰恰相反。数据是你拥有的最有价值的资产，它决定了机器学习项目的成功与否。获取更多更好的数据来提高模型性能是最有效的策略。

这意味着两件事：

1.  **你需要（多多）与数据工程师沟通。**

    他们知道每一条数据的位置。他们可以帮助你获取这些数据并用它生成有用的特征。此外，他们还可以构建数据摄取管道，以添加可以提高模型性能的第三方数据。保持良好健康的关系，偶尔喝杯啤酒，你的工作会变得更轻松，轻松很多。

1.  **你需要精通SQL。**

    访问数据的最通用语言是SQL，因此你需要精通SQL。这在你工作于数据不发达的环境中（如初创公司）尤为重要。掌握SQL可以让你快速构建、扩展和修复模型的训练数据。除非你在一个超级发达的科技公司（如Facebook、Uber及类似公司）拥有内部功能库，否则你会花费大量时间编写SQL。所以最好熟练掌握。

机器学习模型是软件（例如，从简单的逻辑回归到庞大的变换器）和数据（是的，字母大写）的结合。数据决定了项目的成功与否，而不是模型。

### 3\. 结构化你的代码

![](../Images/cbde8c0eec35912fe01ad79956600325.png)

*照片由 [Igor Starkov](https://www.pexels.com/@igor-starkov-233202?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 提供，来自 [Pexels](https://www.pexels.com/photo/photo-of-people-on-building-under-construction-1117452/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)。*

Jupyter notebooks 非常适合快速原型设计和测试想法。它们在开发阶段的快速迭代中表现优秀。Python 是一种为快速迭代而设计的语言，Jupyter notebooks 是完美的配对。

然而，notebooks 很快就会变得拥挤且难以管理。

当你只训练一次模型并将其提交到竞赛或在线课程时，这不是问题。然而，当你在现实世界中开发 ML 解决方案时，你需要做的不仅仅是训练一次模型。

有两个重要的方面你可能会遗漏：

1.  **你必须部署你的模型并使其对公司其他部门可访问。**

    不易部署的模型没有价值。在工业界，一个可以轻松部署的普通模型比一个没有人知道如何部署的最新庞大 Transformer 更有用。

1.  **你必须重新训练模型以避免概念漂移。**

    现实世界中的数据会随时间变化。无论你今天训练了什么模型，它在几天、几周或几个月后（取决于底层数据的变化速度）都会过时。在工业界，一个使用最新数据训练的普通模型比一个使用昔日数据训练的出色模型更好。

我强烈建议从一开始就打包你的 Python 代码。对我来说，效果较好的目录结构如下：

```py
my-ml-package
├── README.md
├── data
│   ├── test.csv
│   ├── train.csv
│   └── validation.csv
├── models
├── notebooks
│   └── my_notebook.ipynb
├── poetry.lock
├── pyproject.toml
├── queries
└── src
    ├── __init__.py
    ├── inference.py
    ├── data.py
    ├── features.py
    └── train.py

```

[Poetry](https://python-poetry.org/) 是我最喜欢的 Python 打包工具。只需 3 条命令，你就可以生成大部分这样的文件夹结构。

```py
$ poetry new my-ml-package
$ cd my-ml-package
$ poetry install

```

我喜欢为所有 ML 项目中的常见元素保持单独的目录：*data*、*queries*、*Jupyter notebooks* 和由训练脚本生成的 *serialized models*：

```py
$ mkdir data queries notebooks models

```

我建议添加一个 `.gitignore` 文件，以排除 `data` 和 `models` 目录，因为它们包含可能非常大的文件。

对于 `src`/ 中的源代码，我喜欢保持简单：

+   `**data.py**` 是生成训练数据的脚本，通常通过查询 SQL 类型的数据库来完成。拥有一种干净且可重复的方式来生成训练数据非常重要，否则你会浪费时间试图理解不同训练集之间的数据不一致性。

+   `**features.py**` 包含大多数模型所需的特征预处理和工程。这包括填补缺失值、对分类变量进行编码、添加现有变量的变换等。我喜欢使用并推荐 [scikit-learn 数据集转换 API](https://scikit-learn.org/stable/data_transforms.html)。

+   `**train.py**` 是用于训练的脚本，它将数据拆分为训练集、验证集、测试集，并拟合一个 ML 模型，可能还会进行超参数优化。最终模型作为一个工件保存在 `models/` 下。

+   `**inference.py**` 是一个 Flask 或 FastAPI 应用，它将你的模型封装为一个 REST API。

当你将代码结构化为Python包时，你的Jupyter笔记本中不会包含大量的函数声明。相反，这些函数在`src`中定义，你可以通过像`from src.train import train`这样的语句将它们加载到笔记本中。

更重要的是，清晰的代码结构意味着与帮助你的DevOps人员关系更健康，以及更快地发布你的工作。双赢。

### 4. 开始时避免深度学习

现在，我们经常将机器学习和深度学习作为同义词使用。但它们并不是。尤其是在你处理真实世界项目时。

目前，深度学习模型在每个AI领域都是最先进的（SOTA）。但你不需要最先进的技术来解决大多数商业问题。

除非你处理的是计算机视觉问题，在这种情况下深度学习是最佳选择，否则请不要一开始就使用深度学习模型。

通常，你开始一个机器学习项目，拟合你的第一个模型，比如逻辑回归，然后你发现模型的表现不够好，无法结束项目。你认为应该尝试更复杂的模型，而神经网络（即深度学习）是最佳候选者。经过一些谷歌搜索，你找到了一段适合你数据的Keras/PyTorch代码。你复制并粘贴它，然后尝试用你的数据训练它。

你会失败。为什么？神经网络不是即插即用的解决方案。它们正好相反。它们有成千上万/百万个参数，并且非常灵活，这使得它们在第一次尝试时有些棘手。如果你花费大量时间，你会让它们发挥作用，但你需要投入过多的时间。

有很多现成的解决方案，如著名的[XGBoost](https://xgboost.readthedocs.io/en/latest/)模型，对于许多问题（特别是表格数据）效果非常好。在进入深度学习领域之前，尝试这些方法。

![](../Images/cead50aa78dd5effd26d18212b140482.png)

## 结论

专业机器学习工程师的工作比你在任何在线课程中学到的要复杂得多。

我很乐意帮助你成为其中的一员，所以如果你想了解更多，请[订阅 *datamachines* 通讯](https://datamachines.xyz/subscribe/)或查看我的[博客](http://datamachines.xyz/blog/)。

[原文](http://datamachines.xyz/2021/08/13/how-to-solve-ml-problems-in-the-real-world/)。经许可转载。

**简介：**[Pau Labarta Bajo](http://datamachines.xyz/) 是一位数学家和数据科学家，拥有超过10年的经验，处理了各种问题的数字和模型，包括金融交易、移动游戏、在线购物和医疗保健。

**相关：**

+   [Google研究总监的数据科学学习建议](https://www.kdnuggets.com/2021/07/google-advice-learning-data-science.html)

+   [高绩效数据科学家的五种思维方式](https://www.kdnuggets.com/2021/06/five-types-thinking-data-scientist.html)

+   [跟踪数据科学进展的检查表](https://www.kdnuggets.com/2021/05/checklist-data-science-progress.html)

### 更多相关主题

+   [数据科学项目：助你解决现实世界问题](https://www.kdnuggets.com/2022/11/data-science-projects-help-solve-real-world-problems.html)

+   [Kaggle竞赛对现实世界问题有用吗？](https://www.kdnuggets.com/are-kaggle-competitions-useful-for-real-world-problems)

+   [想用数据技能解决全球性问题？这里有解决方案……](https://www.kdnuggets.com/2022/04/jhu-want-data-skills-solve-global-problems.html)

+   [现实世界中NLP应用的范围：不同的……](https://www.kdnuggets.com/2022/03/different-solution-problem-range-nlp-applications-real-world.html)

+   [克服现实世界场景中的数据不平衡挑战](https://www.kdnuggets.com/2023/07/overcoming-imbalanced-data-challenges-realworld-scenarios.html)

+   [揭开现实世界决策树的神秘面纱](https://www.kdnuggets.com/demystifying-decision-trees-for-the-real-world)
