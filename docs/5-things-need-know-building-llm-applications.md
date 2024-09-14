# 构建 LLM 应用时需要了解的 5 个事项

> 原文：[`www.kdnuggets.com/2023/08/5-things-need-know-building-llm-applications.html`](https://www.kdnuggets.com/2023/08/5-things-need-know-building-llm-applications.html)

基于 LLM 的应用程序无疑可以为多个问题提供宝贵的解决方案。然而，了解和积极应对诸如幻觉、提示上下文、可靠性、提示工程和安全性等挑战，将在发挥 LLM 的真正潜力的同时，确保最佳性能和用户满意度。在本文中，我们将深入探讨这些构建 LLM 应用程序时开发人员和从业者需要了解的五个关键考虑因素。

![5 个构建 LLM 应用时需要了解的事项](img/c0d10329440b7d4b4ef2783eb38e49a7.png)

图片由[Nadine Shaabana](https://unsplash.com/@nadineshaabana?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 方面的工作

* * *

# 1\. 幻觉

![5 个构建 LLM 应用时需要了解的事项](img/86d71fed809729db435856db310c0a01.png)

图片由[Ehimetalor Akhere Unuabona](https://unsplash.com/@mettyunuabona?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

使用 LLM 时需要注意的主要方面之一是幻觉。在 LLM 的上下文中，幻觉指的是生成虚假的、不正确的、无意义的信息。LLM 非常有创造力，可以用于和调整到不同的领域，但仍然存在一个非常关键的未解决问题，那就是它们的幻觉。由于 LLM 不是搜索引擎或数据库，因此这些错误是不可避免的。

为了克服这个问题，可以通过提供足够的细节和限制条件来控制生成，以限制模型产生幻觉的自由。

# 2\. 选择合适的上下文

如前所述，解决幻觉问题的一种方法是为输入提示提供适当的上下文，以限制 LLM 的幻觉自由。然而，另一方面，LLM 对可以使用的单词数量有限。解决这个问题的一种可能方法是使用索引，将数据转换为向量并存储在数据库中，并在运行时搜索适当的内容。索引通常有效，但实现起来较为复杂。

# 3\. 可靠性和一致性

如果你基于大语言模型（LLM）构建应用程序，你将面临一个问题，那就是可靠性和一致性。LLM 并不可靠，也不一致，无法确保每次模型输出都是正确的或符合预期的。你可以构建一个应用程序的演示并多次运行它，但当你正式发布应用程序时，你会发现输出可能不一致，这会给用户和客户带来很多问题。

# 4\. 提示工程不是未来

与计算机沟通的最佳方式是通过编程语言或机器语言，而不是自然语言。我们需要明确的指令，以便计算机理解我们的要求。LLM 的问题在于，如果你用相同的提示要求 LLM 做一件特定的事情十次，你可能会得到十个不同的输出。

# 5\. 提示注入安全问题 A

当构建基于 LLM 的应用程序时，你将面临的另一个问题是提示注入。在这种情况下，用户会强迫 LLM 给出某个不符合预期的输出。例如，如果你创建了一个生成 YouTube 视频脚本的应用程序，用户可以指示 LLM 忘记一切并编写一个故事。

# 总结

构建 LLM 应用程序非常有趣，可以解决多个问题并自动化许多任务。然而，它也带来了一些在构建基于 LLM 的应用程序时需要处理的问题。从幻觉开始，到选择正确的提示上下文以克服幻觉和输出的可靠性和一致性，再到提示注入的安全问题。

# 参考文献

+   [大型语言模型中的幻觉简明介绍](https://machinelearningmastery.com/a-gentle-introduction-to-hallucinations-in-large-language-models/#:~:text=In%20the%20context%20of%20LLMs,from%20the%20prompt%20you%20provided.)

+   [使用大型语言模型时的 5 个问题](https://www.youtube.com/watch?v=nNwE0sQq39w)

**[尤瑟夫·拉法特](https://www.linkedin.com/in/youssef-hosni-b2960b135)** 是一位计算机视觉研究员和数据科学家。他的研究重点是开发用于医疗保健应用的实时计算机视觉算法。他还在营销、金融和医疗保健领域担任了超过 3 年的数据科学家。

### 相关主题

+   [关于数据管理和其重要性的 6 个要点](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [你不知道的 7 件低代码工具的用法](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [KDnuggets 新闻，4 月 13 日：数据科学家应该知道的 Python 库…](https://www.kdnuggets.com/2022/n15.html)

+   [你不知道的 SAS 数据科学学院的 3 件事](https://www.kdnuggets.com/2022/07/sas-3-things-didnt-know-sas-academy-data-science.html)

+   [扩展你的网络数据驱动产品时你应该知道的事项](https://www.kdnuggets.com/2023/08/things-know-scaling-web-datadriven-product.html)

+   [Cohere 的 LLM 大学你需要知道的一切](https://www.kdnuggets.com/2023/07/everything-need-llm-university-cohere.html)
