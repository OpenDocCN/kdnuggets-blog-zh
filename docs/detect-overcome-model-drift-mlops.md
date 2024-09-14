# 如何检测和克服 MLOps 中的模型漂移

> 原文：[https://www.kdnuggets.com/2021/08/detect-overcome-model-drift-mlops.html](https://www.kdnuggets.com/2021/08/detect-overcome-model-drift-mlops.html)

[评论](#comments)

**由 [Bhaskar Ammu](https://www.sigmoid.com/)，[Sigmoid](https://www.sigmoid.com/) 的高级数据科学家**

![](../Images/d3d4445359db348c3d3000d7f0fcf71d.png)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

机器学习（ML）被广泛认为是数字化转型的基石，但 ML 模型却最容易受到数字环境动态变化的影响。ML 模型由其创建时的变量和参数定义和优化。

让我们来看看一个**[ML 模型](https://www.sigmoid.com/ebooks-whitepapers/ml-models-poc-to-production/)**的例子，该模型旨在跟踪基于当时可能已广泛传播的垃圾邮件的一般化模板的垃圾邮件。凭借这个基线，ML 模型能够识别并阻止这些邮件，从而防止潜在的网络钓鱼攻击。然而，随着威胁环境的变化和网络犯罪分子的智慧提升，更加复杂和逼真的邮件已经取代了旧邮件。当面对这些新的攻击尝试时，基于先前年份变量的 ML 检测系统将无法妥善分类这些新威胁。这只是模型漂移的一个例子。

模型漂移（或模型衰退）是 ML 模型预测能力的退化。由于数字环境的变化以及概念和数据等变量的随之变化，模型漂移在 ML 模型中很突出，这正是机器语言模型的本质所在。

假设所有未来变量将与 ML 模型创建时的变量保持不变，这是**[MLOps 中的模型漂移](https://www.sigmoid.com/machine-learning-operationalization-mlops/)**的滋生温床。

例如，如果模型在一个静态环境中使用静态数据运行，那么其性能不应该退化，因为预测的数据来自于训练期间使用的相同分布。然而，如果模型存在于一个不断变化的动态环境中，涉及过多变量，则模型性能也会有所不同。

## 模型漂移的类型

模型漂移可以根据变量或预测因子的变化广泛地分为两种主要类型：概念漂移和数据漂移。

**概念漂移** – 当模型中目标变量的统计属性发生变化时，会出现概念漂移。简单来说，如果模型变量的本质发生变化，那么模型将无法按预期运行。

**数据漂移** – 最常见的模型漂移类型，当某些预测因子的统计属性发生变化时会发生。随着变量的变化，模型可能会失败。在一个时间段内有效的模型可能在应用到不同环境时效果不佳，仅仅是因为数据没有适应变化的变量。

> 在概念漂移与数据漂移的争论中，上游数据变化也在模型漂移中扮演着重要角色。随着所有必要数据通过**[数据管道](https://www.sigmoid.com/etl-and-data-warehousing/)**流动，未生成的特征和单位（如测量）变化也可能导致缺失值，从而影响机器学习模型的操作。

## 解决模型漂移

及早检测模型漂移对于维护模型准确性至关重要。这是因为随着时间的推移，模型的准确性会下降，预测值会继续偏离实际值。这个过程越进一步，对模型整体造成的不可替代的损害就越大。因此，及早发现问题是必要的。F1得分可以快速检测是否有异常，其中模型的精准度和召回能力被评估其准确性。

同样，根据模型的用途，各种其他指标在不同情况下也会显得重要。用于医疗用途的机器学习模型将需要与用于业务操作的机器学习模型不同的一组指标。然而，最终结果是一样的：每当指定的指标低于设定的阈值时，就有很高的可能性发生模型漂移。

然而，在某些情况下，无法测量模型的准确性——特别是在获取预测数据和实际数据困难时，这仍然是**[扩展机器学习模型的挑战](https://www.sigmoid.com/blogs/5-challenges-to-be-prepared-for-before-scaling-machine-learning-models/)**。在这种情况下，基于过去经验重新调整模型可以帮助创建预测时间表，以便在模型漂移可能发生时进行准备。考虑到这一点，可以定期重新开发模型以应对即将发生的模型漂移。

> 保持原始模型不变也可以作为基准，从而创建新的模型，这些新模型将改进和修正之前基准模型的预测结果。

然而，当数据随时间变化时，根据当前变化对数据进行加权可能很重要。通过确保模型对最近的数据变化赋予更多的权重，而对较旧的数据变化赋予较少的权重，机器学习模型将变得更加稳健，并建立一个整洁的小数据库来管理潜在的未来漂移相关变化。

## 创建可持续的机器学习模型

没有一种万能的方法可以确保及时检测和解决模型漂移。无论是通过计划的模型重训练还是通过实时的**[机器学习](https://www.sigmoid.com/blogs/5-best-practices-for-putting-ml-models-into-production/)**，创建一个可持续的机器学习模型本身就是一个挑战。

然而，MLOps的出现简化了更频繁且时间更短的模型重训练过程。它使数据团队能够自动化模型重训练，而触发这一过程的最可靠方法是通过调度。通过自动化重训练，公司可以在特定时间框架内用新的新鲜数据来强化现有的数据管道。好消息是，这不需要任何特定的代码更改或重建管道。然而，如果公司发现了在模型训练过程中未曾出现的新特征或算法，那么在部署重训练模型时将其纳入其中，可以显著提高模型的准确性。

在决定模型需要多频繁重训练时，有几个变量需要考虑。有时，等待问题出现成为唯一的实际选择（特别是如果没有过去的历史可供参考的话）。在其他情况下，模型应根据与变量季节性变化相关的模式进行重训练。然而，在这片变化的海洋中，保持监控的重要性是恒定的。无论是时间表还是业务领域；在定期间隔内进行持续监控始终是检测模型漂移的最佳方式。

尽管管理、检测和解决成千上万个机器学习模型的模型漂移的挑战可能看起来令人望而生畏，但来自服务提供商如Sigmoid的机器学习操作化解决方案可以为您提供直面这些问题所需的优势。**[Sigmoid的MLOps实践](https://www.sigmoid.com/machine-learning-operationalization-mlops/)** 提供了**[数据科学](https://www.sigmoid.com/data-science-services/)**、**[数据工程](https://www.sigmoid.com/data-engineering/)**和**[DataOps专业知识](https://www.sigmoid.com/data-devops/)**的正确组合，这些组合是实现机器学习操作化和扩展以交付业务价值所必需的，并构建一个**[有效的AI战略](https://www.sigmoid.com/blogs/mlops-for-effective-ai-strategy/)**。

*想了解更多关于我们如何帮助数据驱动的公司加快AI项目的商业价值实现并克服模型漂移的挑战，请点击链接**[这里](https://www.sigmoid.com/machine-learning-operationalization-mlops/)**。*

**简介： [Bhaskar Ammu](https://www.sigmoid.com/)** 是Sigmoid的高级数据科学家。他专注于为客户设计数据科学解决方案，构建数据库架构，以及管理项目和团队。

[原文](https://www.sigmoid.com/blogs/how-to-detect-and-overcome-model-drift-in-mlops/)。已获转载许可。

**相关：**

+   [MLOps是工程学科：初学者概述](/2021/07/mlops-engineering-discipline.html)

+   [使用PyCaret + MLflow轻松实现MLOps](/2021/05/easy-mlops-pycaret-mlflow.html)

+   [何时重新训练机器学习模型？运行这5个检查以决定时间表](/2021/07/retrain-machine-learning-model-5-checks-decide-schedule.html)

### 更多相关内容

+   [使用MLOps管理生产中的模型漂移](https://www.kdnuggets.com/2023/05/managing-model-drift-production-mlops.html)

+   [使用Eurybia检测数据漂移以确保生产机器学习模型的质量](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [如何使用合成数据克服机器学习模型训练中的数据短缺](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

+   [识别虚假数据科学家的20个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/01/20-questions-detect-fake-data-scientists-chatgpt-1.html)

+   [识别虚假数据科学家的20个问题（附答案）：ChatGPT…](https://www.kdnuggets.com/2023/02/20-questions-detect-fake-data-scientists-chatgpt-2.html)

+   [如何克服对数学的恐惧并学习数据科学中的数学](https://www.kdnuggets.com/2021/03/overcome-fear-learn-math-data-science.html)
