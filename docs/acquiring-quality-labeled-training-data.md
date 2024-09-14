# 以低成本获得高质量标记训练数据的 7 种方法

> 原文：[`www.kdnuggets.com/2017/06/acquiring-quality-labeled-training-data.html`](https://www.kdnuggets.com/2017/06/acquiring-quality-labeled-training-data.html)

数据科学家知道，未经训练的统计模型几乎毫无用处。没有高质量的标记训练数据，监督学习会崩溃，也无法确保模型能以任何准确度预测、分类或分析感兴趣的现象。

![训练数据](img/ab46a2921688bf5ea2d1daa6fda36c15.png)

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

当你进行监督学习时，最好在没有找到合适的训练数据的情况下不要开发模型。即使你找到了合适的训练数据集，如果其条目没有被标记、标签或注释来有效地训练你的机器学习算法，也没什么用处。

然而，标记工作是一项不受欢迎的工作，少有数据科学家会因为其他原因而做这项工作。标记训练数据在数据科学职位的声望等级中几乎处于底部。标记工作获得了（可能是不公平的）声誉，成为数据科学生态系统中低技能的“[蓝领](https://www.techrepublic.com/article/is-data-labeling-the-new-blue-collar-job-of-the-ai-era/?ftag=TRE684d531&bhid=20439713773007020840263695991524)”工作。或者，正如[这集搞笑剧集](https://www.youtube.com/watch?v=ACmydtFDTGs)所描绘的那样，数据训练的标记工作是一项让一些不道德的数据科学家试图欺骗无辜的年轻大学生为零报酬做的杂务。

所有这些都不公平地暗示，数据科学家无法获得可接受的训练数据，除非他们将标记功能外包给高科技版的血汗工厂。这是一种不幸的看法，因为正如我去年在 KDnuggets 专栏《认知时代的模式策展人》中提到的那样，标记工作可能依赖于高度熟练的主题专家（例如，肿瘤学家评估活检是否表明癌症组织），也同样依赖于我们任何人都能进行的平凡评估（例如，上面提到的虚构的“热狗/非热狗”示例）。

远程劳工并不是获取和标注训练数据的唯一方法，正如在[这篇最近的 Medium 文章](https://medium.com/merantix/applying-deep-learning-to-real-world-problems-ba2d86ac5837)中提到的那样。正如作者拉斯穆斯·罗瑟所指出的，还有其他方法可以以不会超出你的数据科学预算的成本产生标注的训练数据。以下是我对这些方法的总结：

1.  **重新利用现有的训练数据和标签**：如果我们假设新学习任务的领域与原始任务的领域足够相似，这可能是训练中最便宜、最简单、最快的方法。在采取这种方法时，“[迁移学习](http://www.infoworld.com/article/3155262/analytics/transfer-learning-jump-starts-new-ai-projects.html)”工具和技术可能帮助你确定哪些源训练数据集的元素可以重新用于新的建模领域。

1.  **从免费来源收集自己的训练数据和标签**：网络、社交媒体以及其他在线资源充满了可以利用的数据，只要你有合适的工具。在这个认知计算的时代，实际上你可以从我在[这篇 Dataversity 专栏](http://www.dataversity.net/will-harvest-cognition-2017/)中提到的各种来源中获取丰富的自然语言、社交情感和其他训练数据。如果你有数据爬虫的访问权限，这可能是从源内容和元数据中获取训练数据集及其关联标签的一个好选择。显然，你需要处理一系列与数据所有权、数据质量、语义、采样等相关的问题，以评估爬取的数据是否适合模型训练。

1.  **探索预标注的公共数据集**：在开源社区甚至各种商业提供商那里，有[大量免费的数据](https://www.forbes.com/sites/bernardmarr/2016/02/12/big-data-35-brilliant-and-free-data-sources-for-2016/#63f780cab54d)可供获取。数据科学家应当识别这些数据中是否有适合至少用于模型初始训练的数据。理想情况下，免费的数据集应当已按对你的学习任务有用的方式进行预标注。如果数据集没有预标注，你需要找出最具成本效益的标注方式。

1.  **在逐步提高质量的标注数据集上重新训练模型**：你自己的数据资源可能不足以训练你的模型。为了启动训练，你可以用与你的领域大致相关的免费公共数据进行预训练。如果免费数据集包含可接受的标签，那就更好。然后，你可以在直接与学习任务相关的小型高质量标注数据集上重新训练模型。随着你在更高质量的数据集上逐步重新训练模型，结果可能会使你能够对特征工程、类别和模型中的超参数进行微调。这种迭代过程可能会建议你获取其他更高质量的数据集和/或在未来的训练轮次中进行更高质量的标注，以进一步完善你的模型。不过，请注意，这些迭代改进可能需要逐步更昂贵的训练数据集和标注服务。

1.  **利用众包标注服务**：你可能没有足够的内部员工来标注你的训练数据。或者你的员工可能不可用，或标注成本太高，或者你的员工资源可能不足以迅速标注大量训练数据。在这种情况下，如果预算允许，你可以将标注任务外包给商业服务，如[亚马逊机械土耳其](https://www.mturk.com/mturk/welcome)或[众包花](https://www.crowdflower.com/)。将标注任务外包到以众包为导向的环境通常比内部处理更具扩展性，尽管你会失去对标签质量和一致性的某些控制。积极的一面是，这些服务往往使用高质量的标注工具，使得过程比你在内部处理时更加快速、精确和高效。

1.  **将标注任务嵌入在线应用程序**：如果你足够聪明，互联网中的人类认知是一个无限的资源，可以用来进行标注任务。例如，将训练数据嵌入[验证码](https://en.wikipedia.org/wiki/CAPTCHA)挑战中，这在双因素认证场景中很常见，是训练图像和文本识别模型的一种流行方法。类似地，你可以考虑在提供激励的游戏化应用中展示训练数据，以促使用户识别、分类或对图像、文本、对象及其他呈现的实体进行评论。

1.  **依赖于已经在标注数据上预训练的第三方模型**：许多学习任务已经被足够好的模型解决，这些模型已经用足够好的数据集进行训练，假设这些数据集在训练对应模型之前已得到充分标注。预训练模型可以从各种来源获得，包括学术研究者、商业供应商和开源数据科学社区。请记住，如果你的学习任务领域、特征集和学习任务随着时间的推移与源数据偏离，这些模型的实用性将会下降。

让模型适应用途取决于训练数据的可用性、频繁重训的需求、标注资源的可用性等等。显然，没有一种方法适用于所有获取和标注训练数据集的要求。

数据科学家在这方面必须做出的复杂决策会给有监督学习应用的生命周期带来风险和脆弱性。正如我在这篇[最近的 Wikibon 博客](https://wikibon.com/application-decay-and-the-burden-of-data-driven-algorithm-training/)中提到的，你选择如何训练算法会给任何消耗你分析模型输出的下游应用带来持续的维护负担。

### 更多相关话题

+   [用嘈杂标注数据微调 OpenAI 语言模型](https://www.kdnuggets.com/2023/04/finetuning-openai-language-models-noisily-labeled-data.html)

+   [宣布 PyCaret 3.0：Python 中的开源低代码机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [低代码：开发者还需要吗？](https://www.kdnuggets.com/2022/04/low-code-developers-still-needed.html)

+   [你可能不知道的 7 个低代码工具功能](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [进入数据科学的 3 种可能方式](https://www.kdnuggets.com/2022/03/3-possible-ways-get-data-science.html)

+   [你应该了解的关于梯度下降和成本函数的 5 个概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)
