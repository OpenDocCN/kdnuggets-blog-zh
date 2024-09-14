# 如何使用合成数据来克服数据短缺，以进行机器学习模型训练

> 原文：[`www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html`](https://www.kdnuggets.com/2022/03/synthetic-data-overcome-data-shortages-machine-learning-model-training.html)

![如何使用合成数据来克服数据短缺，以进行机器学习模型训练](img/9948a4e44a27c8a43b26295628cd8b5e.png)

图片由 [geralt 在 Pixabay](https://pixabay.com/users/geralt-9301/) 提供

人工智能的本质在于数据。如果我们没有足够的数据，就无法训练模型完成我们想要的任务，我们昂贵且强大的硬件将变得无用。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 部门

* * *

但是，获取相关、准确、合法和可靠的数据并非易事。这是因为 数据收集过程 往往和实际设置机器学习模型一样复杂且耗时。

此外，收集、记录和清理数据也需要时间和相当的资源，然后才能使用。但有一种方法可以解决这个挑战——使用合成数据。

# 什么是合成数据？

与实际数据不同，合成数据不是从真实世界事件中收集的，而是人工生成的。这些数据被制作得类似于真实的数据集。你可以 [使用这些合成数据](https://sloanreview.mit.edu/article/the-real-deal-about-synthetic-data/) 来检测内在模式、隐藏的交互和变量之间的关联。

你需要做的只是设计一个可以理解真实数据行为、外观和交互的机器学习模型。然后，这个模型可以被查询，从而生成数百万个额外的合成记录，这些记录的行为、外观和感觉都像真实数据一样。

现在，合成数据并没有什么神奇的地方，但如果你想创建高质量的合成数据，你需要遵循一些基本原则。例如，你的合成数据将基于用于生成它的实际数据。因此，如果你使用的是低质量的数据点，你不能指望它能提供高质量的合成数据集。

要期望高质量的数据输出，你需要高质量的数据作为输入。此外，这些数据还应该充足，以便你有足够的输入来用一流的合成数据点扩展现有数据集。

# 数据短缺的原因

要了解合成数据如何帮助你克服数据短缺，你需要首先理解数据短缺的原因。这是数据面临的两个最大问题之一。

首先，现有的数据可能不足以满足 AI/ML 模型的需求。由于数据隐私法律，企业需要获得明确的许可才能使用敏感的客户数据。而且，找到足够多愿意将数据提供用于研究目的的用户、客户或员工可能会很有挑战。

此外，[机器学习模型](https://docs.microsoft.com/en-us/windows/ai/windows-ml/what-is-a-machine-learning-model)不能与过时的数据集成。原因在于它们需要新的趋势，或者必须能够响应新的流程、技术或产品特性。因此，历史数据几乎没有用处，限制了数据的收集。

此外，由于数据本身的性质，样本量通常较小。例如，在一个测量股票价格敏感性的模型中，你可能每月仅获得一次客户价格指数的数据。即使你查找历史数据，你也仅会获得 50 年 CPI 历史中的 600 条记录——显然是一个非常小的数据集。

在某些情况下，标记数据的工作可能不具成本效益或不够及时。例如，为了预测客户满意度和衡量客户情感，机器学习模型需要手动检查大量的短信、电子邮件和服务电话，但这需要花费大量时间，而你可能没有这么多时间。

然而，借助合成数据，机器学习可以轻松地标记数据而没有任何问题。

# 合成数据为何有用？

假设你有一个数据集，其中某些变量的信息非常少，导致预测困难。你还受到限制，无法获取更多这种边缘类别的数据。在这种情况下，[合成数据](https://blogs.nvidia.com/blog/2021/06/08/what-is-synthetic-data/)可以提供帮助。它将允许你合成更多边缘类别的数据点，平衡你的模型，从而提高性能。

想象一个任务，涉及预测一块水果是橙子还是苹果，仅通过理解这两种水果的特征，比如它们的形状、颜色、季节性等。假设你有 500 个橙子的样本和 3000 个苹果的样本。因此，当机器学习模型尝试分析数据时，由于类别不平衡，算法预期会自动偏向苹果。因此，模型将不准确，表现也会令人不满意。

你唯一可以解决这种不平衡的方法是使用合成数据。生成 2500 个更多的橙子样本，以便为模型提供足够的数据，使其不会对这两种水果有偏见。现在预测将更准确。同样，你也可以有效地利用这个模型来[管理你的在线声誉](https://www.getweave.com/reputation-management/)，因为你可以从好评中筛选出差评。

同样，在选择合适的机器学习算法时要小心，以确保模型能够准确工作。

假设你有一个数据集你想分享。问题在于它包含了敏感数据，如个人身份信息（PII）、社会安全号码、全名、银行账户号码等。由于[隐私至关重要](https://privacycanada.net/online-privacy-guide/)，这些数据必须小心保护。现在，假设你希望分析这些数据或构建一个模型。但由于涉及 PII，你不能在没有法律团队介入、仅使用非个人身份信息、匿名化数据、实施安全的数据传输流程等措施的情况下将数据分享给第三方。

因此，可能会有几个月的延迟，因为你无法立即分享数据。在这种情况下，[合成样本可能会再次派上用场](https://www.natlawreview.com/article/synthetic-data-may-be-solution-to-ai-privacy-concerns)。你可以从真实数据集中获取这些样本。

这些合成数据可以毫无问题地与第三方分享。此外，你不会面临数据隐私被侵犯和个人信息泄露的风险。它对于提供[HIPAA 数据库托管](https://www.atlantic.net/hipaa-compliant-hosting/)和数据存储的服务尤其有用。

# 合成数据的新进展

合成数据可以填补数据中的空白，使其完整，从而帮助你生成大量安全的数据。生成的数据有助于[企业保持合规](https://www.forbes.com/sites/anniebrown/2020/12/17/synthetic-data-promises-fair-ai-and-privacy-compliance-but-how-exactly-does-it-work/)并维护数据平衡。此外，随着技术的最新创新，你可以生成更高准确度的数据。这使得合成数据在提供机器学习模型所需的缺失数据方面变得更加宝贵。

合成数据也被成功用于提升图像质量。此外，生成对抗网络（GAN）模型提升了合成表格数据的精确度。

创建合成数据的另一个近期进展是 Wasserstein GAN 或 WGAN。评论神经网络试图找到生成样本中观察到的分布与用于训练的数据集中的数据分布之间的最小距离。然后，WGAN 训练生成器模型以生成更逼真的数据。为了预测生成图像的真实性概率，它保留了图像真实性的评分，而不是利用判别器。

与 GAN 不同，WGAN 不通过寻找两个对立模型之间的平衡来追求稳定性。相反，[WGAN 寻找模型之间的交点](https://machinelearningmastery.com/how-to-code-a-wasserstein-generative-adversarial-network-wgan-from-scratch/)。因此，它生成的合成数据具有更接近现实生活的特征。

# 总结

随着技术进步和创新，合成数据变得越来越丰富、多样，并且与真实数据紧密对接。因此，随着时间的推移，使用合成数据来克服数据短缺将变得更加可能，因为你可以更高效地生成和使用合成数据。合成数据还将有助于维护用户隐私，保持企业合规，同时提升机器学习模型的速度和智能。

**[Nahla Davies](http://nahlawrites.com/)** 是一位软件开发人员和技术写作者。在将工作全职投入技术写作之前，她还担任过 Inc. 5,000 体验品牌组织的首席程序员，该组织的客户包括三星、时代华纳、Netflix 和索尼。

### 了解更多相关信息

+   [用于机器学习的合成数据](https://www.kdnuggets.com/synthetic-data-for-machine-learning)

+   [如何克服数学恐惧并学习数据科学中的数学](https://www.kdnuggets.com/2021/03/overcome-fear-learn-math-data-science.html)

+   [用伟大期望克服数据质量问题](https://www.kdnuggets.com/2023/01/overcome-data-quality-issues-great-expectations.html)

+   [高保真合成数据适用于数据工程师和数据科学家](https://www.kdnuggets.com/2022/tonic-high-fidelity-synthetic-data-engineers-scientists-alike.html)

+   [如何通过 AI 生成的合成数据使 AI/ML 和数据科学民主化](https://www.kdnuggets.com/2022/11/mostly-ai-democratize-aiml-data-science-aigenerated-synthetic-data.html)

+   [合成数据平台：释放生成 AI 在结构化数据中的力量](https://www.kdnuggets.com/2023/07/synthetic-data-platforms-unlocking-power-generative-ai-structured-data.html)
