# 机器学习模型监控检查清单：需要跟踪的 7 件事

> 原文：[https://www.kdnuggets.com/2021/03/machine-learning-model-monitoring-checklist.html](https://www.kdnuggets.com/2021/03/machine-learning-model-monitoring-checklist.html)

[评论](#comments)

**由 [Emeli Dral](https://twitter.com/EmeliDral)，Evidently AI 的首席技术官兼联合创始人 & [Elena Samuylova](https://twitter.com/elenasamuylova/)，Evidently AI 的首席执行官兼联合创始人**

建立一个机器学习模型并不容易。将服务部署到生产环境中则更为困难。但即使您成功将所有管道拼接在一起，事情也不会止步于此。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

一旦模型投入使用，我们立即必须考虑如何平稳运行。毕竟，它现在在提供业务价值！任何对模型性能的干扰都会直接转化为实际的业务损失。

我们需要确保模型的表现不仅仅是一个返回 API 响应的软件，而是一个我们可以信任以做出决策的机器学习系统。

这意味着我们需要监控我们的模型。而且还有更多的事情需要注意！

![图片](../Images/b29caf6d23d8985bee785a88032cd741.png)

如果生产中的 ML 让您感到措手不及，这里有一个检查清单，供您关注。

**1\. 服务健康**

机器学习服务仍然是一种服务。您的公司可能已经有一些建立好的软件监控流程可以重用。如果模型是实时运行的，它需要适当的警报和负责任的人员值班。

即使您只处理批处理模型，也不要例外！我们仍然需要跟踪标准的健康指标，如内存利用率、CPU 负载等等。

*我们的目标是确保服务正常运作，并遵守必要的约束，如响应速度。*

一个开源工具供检查：[*Grafana*](https://github.com/grafana/grafana)*.*

**2\. 数据质量与完整性**

机器学习模型出现问题了吗？在绝大多数情况下，[数据是罪魁祸首](https://evidentlyai.com/blog/machine-learning-monitoring-what-can-go-wrong-with-your-data)。

上游管道和模型可能会出现故障。用户可能会进行未通知的模式更改。数据可能在源头消失，物理传感器可能会失败。问题不胜枚举。

因此，验证输入数据是否符合我们的预期至关重要。检查可能包括范围合规性、数据分布、特征统计、相关性或我们认为“正常”的任何行为。

*我们的目标是确认我们输入的数据是模型能够处理的。在模型返回不可靠的响应之前。*

开源工具检查：[*Great Expectations*](https://github.com/great-expectations/great_expectations)。

**3\. 数据与目标漂移**

事物总在变化。即使我们处理的是非常稳定的过程。几乎每个机器学习模型都有一个不便的特性：它会随着时间的推移而退化。

我们可能会遇到[数据漂移](https://evidentlyai.com/blog/machine-learning-monitoring-data-and-concept-drift)，当模型接收到它在训练中未见过的数据时。想象一下用户来自不同的年龄组、营销渠道或地理区域。

如果现实世界的模式发生变化，概念漂移就会发生。想想像全球大流行这种影响所有客户行为的常见情况。或者市场上出现了一款提供慷慨免费层的新竞争产品。它改变了用户对你营销活动的响应。

这两种漂移的**最终衡量标准是模型质量的退化**。但有时，实际值尚未知道，我们不能直接计算。在这种情况下，有一些领先指标可以跟踪。我们可以监测输入数据或目标函数的属性是否发生变化。

![Image](../Images/6efeba1798617c476e8b559d61ecbcb6.png)

例如，你可以跟踪关键模型特征和模型预测的分布。如果它们与过去的时间段显著不同，则触发警报。

*我们的目标是获得数据或世界发生变化的早期信号，及时更新我们的模型。*

开源工具检查：[*Evidently*](https://github.com/evidentlyai/evidently)。

**4\. 模型性能**

了解模型是否表现良好的最直接方法是将预测结果与实际值进行对比。你可以使用与模型训练阶段相同的指标，无论是分类的精准度/召回率，回归的RMSE，等等。如果数据质量或现实世界的模式发生变化，我们将看到这些指标逐渐下降。

这里有一些警告。

**首先，真实标签或实际标签往往会有延迟。** 例如，如果你为较长时间范围做预测，或者数据传递有延迟。有时你需要额外的努力来标记新数据以检查预测是否正确。在这种情况下，首先追踪数据和目标漂移作为早期警告是有意义的。

**其次，需要追踪的不仅仅是模型质量，还要关注相关的业务KPI。** ROC AUC的下降并不能直接说明它对营销转化率的影响有多大。将模型质量与业务指标连接起来或找到一些可解释的代理指标是至关重要的。

**第三，你的质量度量标准应适合使用场景。** 例如，如果你有不平衡的类别，准确度指标远非理想。在回归问题中，你可能会关注误差的符号。因此，你不仅应追踪绝对值，还应追踪误差分布。区分偶发异常值和实际退化也是至关重要的。

所以选择你的度量标准要明智！

![Image](../Images/8439a2a6935b22fb1dd1563563b1cb95.png)

*我们的目标是追踪模型的实际效果以及在出现问题时如何进行调试。*

一个开源工具进行检查：[*Evidently*](https://github.com/evidentlyai/evidently)。

**5. 按段性能**

对于许多模型，上述监控设置将足够。但如果你处理的是更关键的使用场景，还有更多的检查项。

例如，模型在哪些方面错误更多，在哪些方面表现最好？

你可能已经知道一些特定的段需要跟踪：比如针对你的高端客户与整体基础的模型准确性。这需要计算仅针对你定义的段内对象的自定义质量指标。

在其他情况下，主动搜索低性能段可能更有意义。比如说，你的房地产定价模型在某个特定地理区域始终建议高于实际的报价。这是你想要注意的！

根据使用场景，我们可以通过在模型输出上添加后处理或业务逻辑来解决问题。或者通过重建模型以考虑低性能段。

![Image](../Images/66ca85daea3a3aed3b32bb11d84053f1.png)

*我们的目标是超越整体性能，了解模型在特定数据切片上的质量。*

**6. 偏差/公平性**

在金融、医疗保健、教育以及其他模型决策可能具有严重影响的领域，我们需要更加仔细地审视我们的模型。

例如，模型性能可能因不同人口统计群体在训练数据中的代表性而有所不同。模型创建者需要意识到这种影响，并与监管者和利益相关者一起使用工具来减轻不公平现象。

为此，我们需要跟踪适当的度量标准，如准确率的公平性。这适用于模型验证和持续生产监控。因此，仪表盘上再添加几个指标！

*我们的目标是确保所有子群体的公平对待并追踪合规性。*

一个开源工具进行检查：[*Fairlearn*](https://github.com/fairlearn/fairlearn)。

**7. 异常值**

我们知道模型会出错。在一些使用场景中，比如广告定向，我们可能不在乎个别输入是否显得奇怪或不寻常。只要它们不构成模型失败的有意义的段落即可！

在其他应用中，我们可能希望了解每一个这样的案例。为了最小化错误，我们可以设计一套规则来处理异常值。例如，将它们送去人工审核，而不是做出自动决策。在这种情况下，我们需要一种方法来检测和标记这些异常。

*我们的目标是标记异常数据输入，其中模型预测可能不可靠。*

一个开源工具供检查：[*Seldon Alibi-Detect*](https://github.com/SeldonIO/alibi-detect)

![Image](../Images/0bcc5f81d4c8b8260764e402db3ec31d.png)

监控可能听起来很无聊。但它对机器学习在现实世界中的应用至关重要。不要等到模型失败才去创建你的第一个仪表板！

[**Emeli Dral**](https://twitter.com/EmeliDral) 是 Evidently AI 的联合创始人兼首席技术官，她在这里创建了用于分析和监控机器学习模型的工具。此前，她共同创立了一家工业 AI 初创公司，并担任 Yandex Data Factory 的首席数据科学家。她是 Coursera 上机器学习和数据分析课程的共同作者，拥有超过 100,000 名学生。

[**Elena Samuylova**](https://twitter.com/elenasamuylova/) 是 Evidently AI 的联合创始人兼首席执行官。此前，她共同创立了一家工业 AI 初创公司，并在 Yandex Data Factory 领导业务发展。自 2014 年以来，她与从制造业到零售业的公司合作，提供基于机器学习的解决方案。在 2018 年，Elena 被 Product Management Festival 评选为欧洲 50 位女性产品经理之一。

**相关：**

+   [机器学习系统设计：斯坦福大学免费课程](/2021/02/machine-learning-systems-design-free-stanford-course.html)

+   [MLOps：模型监控 101](/2021/01/mlops-model-monitoring-101.html)

+   [如何使用 MLOps 制定有效的 AI 策略](/2021/01/mlops-effective-ai-strategy.html)

### 更多相关话题

+   [成为一名优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应该掌握的 6 种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目的，并寻找目的以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
