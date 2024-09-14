# Jupyter Notebook 上的数据科学项目的 5 个免费模板

> 原文：[`www.kdnuggets.com/5-free-templates-for-data-science-projects-on-jupyter-notebook`](https://www.kdnuggets.com/5-free-templates-for-data-science-projects-on-jupyter-notebook)

![Jupyter Notebook 上的数据科学项目的 5 个免费模板](img/e6943732a00200cbfe1b55d90bcd1e55.png)

图片由作者使用 DALL·E 3 生成

对于许多专业的数据科学家来说，Jupyter Notebook 已成为他们的主要工作环境。即使对我来说，它也是我进行任何数据科学实验和工作流时的首选地方。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

作为数据科学家的工作环境，Jupyter Notebook 是一个独特的 IDE，因为代码可以在每个单元格中独立执行。同时，作者可以解释每个单元格。这种区别使得笔记本可以被其他人重用，并成为项目模板。

在本文中，我们将讨论五个用于在 Jupyter Notebook 上构建数据科学项目的免费模板。那么，这些 Jupyter Notebook 模板是什么呢？让我们深入了解一下。

## 1\. Python 数据科学项目的 Cookiecutter 模板

我们讨论的第一个模板不一定是我们可以直接填写的完整代码项目。我们将讨论的模板不仅仅是 Jupyter Notebook，还有支持 Jupyter Notebook 的完整项目。我们会涉及到 [AWS 的 Python 数据科学项目](https://github.com/aws-samples/python-data-science-template)。

这个模板创建了一个完整的数据科学项目结构，准备好用于你的实际项目。使用 [Cookiecutter CLI](https://github.com/cookiecutter/cookiecutter)，你可以生成数据科学项目的目录结构，类似于下面的结构。

```py
|-- bin/
|-- notebooks                    # A directory to place all notebooks files.
|   |-- *.ipynb
|   `-- my_nb_path.py            # Imported by *.ipynb to treat src/ as PYTHONPATH
|-- requirements/
|-- src
|   |-- my_custom_module         # Your custom module
|   |-- my_nb_color.py           # Imported by *.ipynb to colorize their outputs
|   `-- source_dir               # Additional codes such as SageMaker source dir
|-- tests/                       # Unit tests
|-- MANIFEST.in                  # Required by setup.py (if module name specified)
|-- setup.py                     # To pip install your Python module (if module name specified)

# These sample configuration files are auto-generated too:
|-- .editorconfig                # Sample editor config (for IDE / editor that supports this)
|-- .gitattributes               # Sample .gitattributes
|-- .gitleaks.toml               # Sample Gitleaks config (if pre_commit is advanced)
|-- .gitignore                   # Sample .gitignore
|-- .pre-commit-config.yaml      # Sample precommit hooks
|-- LICENSE                      # Boilperplate (auto-generated)
|-- README.md                    # Template for you to customize
|-- pyproject.toml               # Sample configurations for Python toolchains
`-- tox.ini                      # Sample configurations for Python toolchains 
```

如果你对模板如何应用于实际项目感兴趣，可以查看上面的这个 [强化学习能源存储用例](https://github.com/aws-samples/sagemaker-rl-energy-storage-system)。

## 2\. Coen Meintjes 的数据科学笔记本模板

接下来我们要讨论的 Jupyter 笔记本模板是[Coen Meintjes 的模板](https://github.com/CoenMeintjes/data_science_notebook_templates/tree/main)。这是一个从数据探索到模型评估的基本 Jupyter 笔记本合集。它不是一个针对特定项目的模板；实际上，它主要由基本代码组成，没有更多内容。但我会说它是好的。为什么呢？

这是一个基础模板，每个人都可以用来进行各种项目，仅需进行少量调整。你可以使用这个 Jupyter 笔记本模板来开发任何项目创意。此外，这里的模板深入解释了笔记本中的许多过程，因此任何初学者或专业人士都可以从中受益。

## 3\. Yusuf Cinarci 的数据科学项目

让我们来看一个更具项目针对性的模板，[Yusuf Cinarci 的《数据科学项目 Jupyter 笔记本模板》](https://github.com/SUKHMAN-SINGH-1612/Data-Science-Projects/tree/main)。在这些模板中，你可以用它们来开发简单的项目，适用于你的投资组合或任何商业需求。

你可以从许多项目模板中进行选择。从简单的 Python 工资数据探索，到假新闻检测和电影推荐系统的开发，选择众多。这些笔记本非常适合那些希望轻松启动项目的你。

我喜欢 Yusuf Cinarci 模板合集的地方在于，它们并不过于复杂，因此初学者在学习数据科学时可以直接开始他们的项目。然而，许多项目都是针对初学者的，如果你在寻找数据科学项目，可能会发现它们的项目比较少。

## 4\. Sukman Singh 的数据科学项目

如果你需要一个更复杂的项目 Jupyter 笔记本模板，那么[Sukman Singh 的《数据科学项目 Jupyter 笔记本模板》](https://github.com/SUKHMAN-SINGH-1612/Data-Science-Projects/tree/main)可能适合你。它非常适合那些希望轻松开发预测模型但需要创意灵感的人。

该模板合集包含许多数据科学项目模板，包括客户流失预测、贷款审批预测和索赔欺诈项目。这些是标准的商业项目，你可以用来丰富你的数据项目组合。

该项目可能看起来单一，但你也可以扩展它。它是一个你可以用来开发项目的模板，并使用你认为适合商业问题的其他数据集。

## 5\. Jupyter Naas 的精彩笔记本

最后，我们将讨论[Jupyter Naas 的精彩笔记本](https://github.com/jupyter-naas/awesome-notebooks/tree/master/YouTube)。精彩笔记本是 Jupyter Naas 推出的一个项目，旨在创建最大规模的生产就绪 Jupyter 笔记本模板目录。从中你可以选择丰富的免费 Jupyter 笔记本模板。

该项目包含许多具有特定使用案例的专业 Jupyter Notebook 模板。从人工智能开发到分析商业漏斗，再到 YouTube 视频下载，有很多模板可供选择。

许多模板要求你理解如何作为数据科学家工作，因此学习如何将 Python 作为数据科学家的工具将帮助你使用这些模板。一旦你明白你的需求，这些模板集合将帮助你的工作。

## 结论

Jupyter Notebook 是许多专业数据科学家使用的环境，因为许多数据科学突破都发生在这个平台上。它的一个优点是易于分享，并且可以成为许多人使用的模板。

在这篇文章中，我们讨论了五个可以提升数据科学活动的免费 Jupyter Notebook 模板。这些模板包括：

1.  用于 Python 数据科学项目的 Cookiecutter 模板

1.  Coen Meintjes 提供的数据科学笔记本模板

1.  Yusuf Cinarci 的数据科学项目

1.  Sukman Singh 的数据科学项目

1.  Jupyter Naas 提供的精彩笔记本

**[](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)**** 是一名数据科学助理经理和数据撰写者。他在全职工作于 Allianz Indonesia 的同时，喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。Cornellius 涉及各种人工智能和机器学习主题的撰写工作。

### 相关话题

+   [10 个数据科学家必备的 Jupyter Notebook 小贴士和技巧](https://www.kdnuggets.com/2023/06/10-jupyter-notebook-tips-tricks-data-scientists.html)

+   [金融中的 Python：Jupyter Notebook 中的实时数据流](https://www.kdnuggets.com/python-in-finance-real-time-data-streaming-within-jupyter-notebook)

+   [如何在 Jupyter Notebook 上设置 Julia](https://www.kdnuggets.com/2022/11/setup-julia-jupyter-notebook.html)

+   [Jupyter Notebook 魔法方法备忘单](https://www.kdnuggets.com/jupyter-notebook-magic-methods-cheat-sheet)

+   [Mercury 概述：创建数据科学组合和…](https://www.kdnuggets.com/2022/05/overview-mercury-creating-data-science-portfolio-notebook-based-webapps.html)

+   [通过整合 Jupyter 和 KNIME 缩短实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)
