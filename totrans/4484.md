# 自然语言处理配方：最佳实践和示例

> 原文：[https://www.kdnuggets.com/2020/05/natural-language-processing-recipes-best-practices-examples.html](https://www.kdnuggets.com/2020/05/natural-language-processing-recipes-best-practices-examples.html)

[评论](#comments)

我们KDnuggets最近尽力突显一些优质的自然语言处理（NLP）资源，最显著的包括[The Big Bad NLP Database](/2020/02/big-bad-nlp-database.html)和[The Super Duper NLP Repo](/2020/04/super-duper-nlp-repo.html)，这是Quantum Stat管理的两个项目。其中第一个是围绕任务精心组织的NLP数据集的策划仓库，而第二个是演示这些任务实现的Google Colab notebooks集合。

[![图示](../Images/7acb6c2517b9040d8f27923fb8032809.png)](https://github.com/microsoft/nlp-recipes)

在这种背景下，我们发现微软的[Natural Language Processing Best Practices & Examples](https://github.com/microsoft/nlp-recipes)仓库是这一集合中另一个值得添加的资源。该仓库描述了其用处如下：

> 该仓库包含构建NLP系统的示例和最佳实践，以Jupyter notebooks和实用功能的形式提供。该仓库的重点是最先进的方法和在处理文本和语言问题的研究人员和从业者中流行的常见场景。

这些笔记本和它描述的实用功能更像是对NLP任务的指导，而非端到端解决方案，以确保你在实现系统时考虑到最佳实践。

> 这个仓库的目标是构建一整套全面的工具和示例，利用最近在NLP算法、神经网络架构和分布式机器学习系统方面的进展。[...] 我们希望这些工具能够显著缩短“上市时间”，通过从定义业务问题到解决方案开发的过程大幅简化体验。此外，示例笔记本将作为指导，并展示工具在各种语言中的最佳实践和使用方法。

强调[Emily Bender](https://twitter.com/emilymbender)的多语言原则，该仓库还阐明NLP“并不等同于英语”，并确保项目的目标是“提供尽可能多语言的端到端示例”，鼓励社区贡献以促进实现。

该仓库包含一系列的[Jupyter notebooks](https://github.com/microsoft/nlp-recipes/tree/master/examples)，这些笔记本实现了表格中额外描述的以下NLP场景。

[![图示](../Images/5f647153b9276f5d19fb59a37741f80d.png)](https://github.com/microsoft/nlp-recipes/tree/master/examples)

而且这些指南并不是单一维度的；例如，[文本分类笔记本](https://github.com/microsoft/nlp-recipes/tree/master/examples/text_classification)中有几种不同的笔记本，使用了不同的数据集、自然语言、环境（本地或Azure云端）、语言模型和任务焦点的组合。

[![图示](../Images/1c11178ecb7bca00f527d0c1d5f4bea0.png)](https://github.com/microsoft/nlp-recipes/tree/master/examples/text_classification)

这些笔记本还依赖于[utils_nlp](https://github.com/microsoft/nlp-recipes/tree/master/utils_nlp)模块中的脚本，以帮助减轻一些“从数据加载、数据集理解、模型开发、模型评估到将训练好的NLP模型投入生产”的繁琐任务。务必查看微软研究开发的这些工具，旨在节省时间并加速一些与自然语言处理相关的繁重任务。

我对几乎所有NLP的内容情有独钟，从学习资源到示例笔记本，再到框架和库、语言模型、数据集集合等等。如果你也是如此，我建议你查看微软的这个最佳实践导向的仓库。

**相关内容**：

+   [超级NLP仓库：100个即用的Colab笔记本](/2020/04/super-duper-nlp-repo.html)

+   [大型NLP数据库：访问近300个数据集](/2020/02/big-bad-nlp-database.html)

+   [使用TensorFlow和Keras进行分词和文本数据准备](/2020/03/tensorflow-keras-tokenization-text-data-prep.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT工作

* * *

### 更多相关话题

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [成为伟大的数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学数据科学家都应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [自然语言处理中的N-gram语言建模](https://www.kdnuggets.com/2022/06/ngram-language-modeling-natural-language-processing.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标来…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)
