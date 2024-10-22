# 9 个你机器学习项目失败的原因

> 原文：[`www.kdnuggets.com/2018/07/why-machine-learning-project-fail.html`](https://www.kdnuggets.com/2018/07/why-machine-learning-project-fail.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[Alberto Artasanchez](http://thedatascience.ninja/about/)，Knowledgent**。

![失败](img/7d6148125c88c0395eab70554cbedef1.png)

好吧，文章标题有些末日般的感觉，我不希望任何人遭遇不幸。我为你加油，希望你的项目超出预期地成功。本文的目的不是对你施加诅咒以确保你项目的失败。相反，它是对数据科学项目失败的最常见原因的探讨，希望能帮助你避免这些陷阱。

### 1\. 提问不当

如果你问错了问题，你会得到错误的答案。一个来自金融行业的例子是欺诈识别问题。最初的问题可能是“这笔交易是否是欺诈？”为了做出这种判断，你需要一个包含欺诈和非欺诈交易示例的数据集。这个数据集很可能是由人类帮助生成的。即，这些数据的标签可能是由一组训练有素的主题专家（SME）来确定的。然而，这个数据集可能是根据他们过去见过的欺诈行为进行标记的。因此，训练于这些数据的模型只会捕捉到符合旧有欺诈模式的欺诈。如果一个不法分子找到了一种新的欺诈方式，我们的系统将无法检测到它。一个可能更好的问题是“这笔交易是否异常？”因此，它不会仅仅寻找过去被证明是欺诈的交易，而是寻找不符合“正常”交易特征的交易。即使在最先进的欺诈检测系统中，也需要依靠人类进一步分析预测的欺诈交易以验证模型结果。这种方法的一个不良副作用是，它很可能会产生比之前的模型更多的假阳性。

我喜欢的另一个例子来自金融领域。投资传奇彼得·林奇。

*在他担任富达麦哲伦基金期间，林奇超越了市场，在大多数年份中击败了市场，实现了 29%的年化收益。但林奇自己指出了一个问题。他计算出，在同一时期内，基金的平均投资者只赚了大约 7%。当林奇遇到挫折时，例如，资金会通过赎回流出基金。当他重新回到正轨时，资金会流回，但错过了恢复期。*

**哪个问题会更值得回答？**

+   未来一年富达麦哲伦基金的表现会如何？

+   明年 Fidelity Magellan 基金的购买或赎回数量将是多少？

### 2\. 尝试用它解决错误的问题

常见的错误是没有关注业务用例。在定义需求时，应将注意力集中在这个问题上：“如果我们解决了这个问题，它是否会显著提升业务价值？”为了回答这个问题，在将问题分解成子任务时，初始任务应集中在回答这个问题上。例如，假设你想出一个很棒的人工智能产品点子，现在你想开始销售它。假设这是一个服务，你上传全身照片到网站，AI 程序会确定你的测量数据，然后创建一套适合你体型和尺寸的定制西装。让我们头脑风暴一下，为完成这个项目需要执行的一些任务。

+   开发 AI/ML 技术，从照片中确定身体测量数据。

+   设计并创建一个网站和一个手机应用程序与客户互动。

+   进行可行性研究，以确定是否存在市场需求。

作为技术人员，我们渴望设计和编码，因此可能会被诱惑去开始执行前两个任务。可以想象，如果我们在执行前两个任务后再进行可行性研究，而研究结果表明市场上没有我们的产品需求，那将是一个可怕的错误。

### 3\. 数据不足

我的某些项目涉及生命科学领域，其中一个问题是无法以任何价格获得某些数据。生命科学行业对存储和传输受保护的健康信息（PHI）非常敏感，因此大多数可用的数据集会删除这些信息。在某些情况下，这些信息可能相关，并会增强模型结果。例如，个人的地点可能对健康有统计显著的影响。例如，来自密西西比州的人比来自康涅狄格州的人更可能患糖尿病。但由于这些信息可能不可用，我们将无法使用。

另一个例子来自金融行业。一些最有趣和最相关的数据集可以在这个领域找到，但这些信息可能非常敏感且被严格保护，因此访问可能受到高度限制。但没有这些访问权限，就无法获得相关结果。

### 4\. 数据不正确

使用错误或脏数据可能会导致即使你拥有最好的模型也会做出不良预测。在监督学习中，我们使用之前已标记的数据。在许多情况下，这些标记通常由人工完成，这可能导致错误。一个极端的假设例子是，模型始终具有完美的准确性，但使用的是不准确的数据。考虑一下 MNIST 数据集。当我们对其进行建模时，我们假设图像的人工标记是 100%准确的。现在，假设三分之一的数字标记错误。你认为无论模型有多好，你的模型产生任何体面结果的难度会有多大？垃圾进，垃圾出这一古老的格言在数据科学领域依然适用。

![大数据炒作](img/985b4cb6f3a82f7971453ad211c4e154.png)

### 5\. 数据过多

理论上，你永远不会有过多的数据（只要数据是正确的）。在实践中，尽管在存储和计算成本及性能上已有了巨大的进步，我们仍然受到时间和空间的物理限制。因此，目前数据科学家最重要的工作之一是明智地挑选他们认为对实现准确模型预测有影响的数据源。例如，假设我们正在预测婴儿出生体重。直观上，母亲的年龄似乎是一个相关的特征，但母亲的名字可能不相关，而地址可能相关。另一个例子是 MNIST 数据集。MNIST 图像中的大多数信息都集中在图像的中心，我们可以考虑去掉图像周围的边框，而不会丢失太多信息。在这个例子中，仍然需要人工干预和直觉来确定去除一定数量的边框像素对预测的影响很小。关于降维的最后一个例子是使用主成分分析（PCA）和 t-分布随机邻居嵌入（t-SNE）。在运行模型之前，确定哪些特征是相关的仍然是计算机面临的难题，但这是一个充满自动化可能性的领域。与此同时，数据过多仍然是一个可能使你的数据科学项目脱轨的陷阱。

### 6\. 招聘错误的人员

你不会相信你的医生能修理你的车，也不应该相信你的机械师能做你的结肠镜检查。如果你有一个小型的数据科学实践，你可能别无选择，只能依赖一个或少数几个人来执行从数据收集和采购、数据清理和处理、特征工程和生成、模型选择到在生产环境中部署模型的所有任务。但随着团队的成长，你应该考虑为这些任务聘请专门的专家。成为 ETL 开发专家所需的技能与自然语言处理（NLP）专家的专长不一定重叠。此外，对于某些行业——如生物技术和金融——拥有深厚的领域知识可能是有价值的，甚至是关键的。然而，拥有一个主题专家（SME）和一个具备良好沟通技能的数据科学家可能是一个合适的替代方案。随着数据科学实践的增长，拥有合适的专业资源是一项微妙的平衡，但拥有合适的资源和人才池是实践成功的最重要关键之一。

### 7\. 使用错误的工具

这里有很多例子可以想到。一个常见的陷阱是所谓的“我有一把锤子，现在一切都像钉子”。一个更具行业特定的例子是：你最近让团队接受了 MySQL 的培训，现在他们回来了，你需要建立一个分析管道。由于培训记忆犹新，他们建议使用他们的新工具。然而，根据你的管道将处理的数据量和你需要对结果进行的分析量，这个选择可能不是最适合的。许多 SQL 产品对单个表能够存储的数据量有严格限制。在这种情况下，更好的选择可能是使用像 MongoDB 这样的 NoSQL 产品或像 AWS Redshift 这样的高度可扩展的列式数据库。

### 8\. 没有合适的模型

模型是现实的简化表示。这些简化旨在去除不必要的虚饰、噪音和细节。一个好的模型使其用户能够专注于特定领域中的重要现实方面。例如，在市场营销应用中，保持如客户电子邮件和地址等属性可能很重要。而在医疗环境中，患者的身高、体重和血型可能更为重要。这些简化根植于假设中；这些假设在某些情况下可能成立，但在其他情况下可能不成立。这表明，在某个特定情境下有效的模型可能在另一个情境下不起作用。

数学中有一个著名的定理。“无免费午餐”（NFL）定理表明，没有一个模型能适用于所有问题。一个领域的好模型的假设可能不适用于另一个领域，因此在数据科学中，使用多个模型进行迭代以找到最适合特定情况的模型并不罕见。这在监督学习中尤其如此。常用的验证或交叉验证来评估多个具有不同复杂度的模型的预测准确性，以找到最合适的模型。此外，表现良好的模型也可以通过多种算法进行训练，例如，线性回归可以通过正规方程或梯度下降进行训练。

根据使用情况，关键在于确定不同模型和算法在速度、准确性和复杂性之间的权衡，并使用最适合特定领域的模型。

### 9\. 没有合适的衡量标准

在机器学习中，能够评估训练模型的性能至关重要。必须衡量模型在训练数据和测试数据上的表现。这些信息将用于选择要使用的模型、超参数选择以及确定模型是否准备好用于生产。

要衡量模型性能，选择适合任务的最佳评估指标至关重要。关于指标选择有很多文献，我们不会深入探讨这个话题，但在选择指标时要记住的一些参数包括：

+   **机器学习问题的类型：**监督学习、无监督学习和强化学习。

+   **监督学习的类型：**二分类、分类或回归。

+   **数据集类型：**如果数据集不平衡，可能需要使用不同的指标。

### 结论

失败的方式有很多种，而解决问题的最佳方式却只有一个。成功的定义也可能会用不同的标准来衡量。所寻求的解决方案是一个快速而粗糙的补丁吗？你是否在寻找一个行业最佳的解决方案？重点是模型训练速度、推理端点的可用性，还是模型的准确性？也许发现某个解决方案不起作用可以被视为一种成功，因为这样可以将注意力转向其他备选方案。我很想听听你对这个话题的看法。你发现了哪些其他的失败方式？哪些解决方案对你来说效果最好？欢迎在下面讨论或通过 [Linked In](https://www.linkedin.com/in/Artasanchez) 联系我。编程愉快！

**简介**：[Alberto Artasanchez](http://thedatascience.ninja/about/) 是 Knowledgent 的首席数据科学家。他拥有计算机科学与人工智能硕士学位，是一位具有超过 25 年技术行业经验的人工智能/机器学习研究员、从业者、作者和教育者。自 2015 年至今，他作为 Knowledgent 的首席数据科学家，参与了金融和生物技术行业中各种数据科学、云计算和大数据的计划和项目，根据项目需求担任顾问、导师、设计师、故障排除者和编码员。

[原文](http://thedatascience.ninja/2018/07/12/why-your-machine-learning-project-will-fail/)。经许可转载。

**相关：**

+   [混乱对于保持机器学习的智慧是必要的](https://www.kdnuggets.com/2018/07/chaos-machine-learning.html)

+   [数据科学：大多数人无法交付的 4 个原因](https://www.kdnuggets.com/2018/05/data-science-4-reasons-failing-deliver.html)

+   [人工智能不是一设置了事](https://www.kdnuggets.com/2018/05/ai-not-set-forget.html)

* * *

## 我们的前 3 个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关内容

+   [为什么大多数人无法学会编程？](https://www.kdnuggets.com/2022/03/people-fail-learn-programming.html)

+   [6 个理由说明通用语义层对你的数据堆栈有益](https://www.kdnuggets.com/2024/01/cube-6-reasons-why-a-universal-semantic-layer-is-beneficial)

+   [你不应该使用机器学习的 4 个原因](https://www.kdnuggets.com/2021/12/4-reasons-shouldnt-machine-learning.html)

+   [调查：机器学习项目仍然经常失败部署](https://www.kdnuggets.com/survey-machine-learning-projects-still-routinely-fail-to-deploy)

+   [数据科学家应该使用 LightGBM 的 3 个理由](https://www.kdnuggets.com/2022/01/data-scientists-reasons-lightgbm.html)

+   [避免数据科学职业的 5 大理由](https://www.kdnuggets.com/2022/04/top-5-reasons-avoid-data-science-career.html)
