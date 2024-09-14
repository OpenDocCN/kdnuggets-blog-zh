# 去中心化和协作式 AI：微软研究如何利用区块链构建更透明的机器学习模型

> 原文：[`www.kdnuggets.com/2019/07/decentralized-collaborative-ai-microsoft-research-blockchains-transparent-machine-learning.html`](https://www.kdnuggets.com/2019/07/decentralized-collaborative-ai-microsoft-research-blockchains-transparent-machine-learning.html)

![评论](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

![头图](img/5b5fb10468254b6e7062f2104a47ac47.png)

未来十年人工智能（AI）面临的最大挑战是，数据和智能是否仍然是少数几个国家的大型技术公司特权，还是可以向世界其他地区普及。机器学习和 AI 应用的中心化特性促成了一种“富者愈富”的动态，只有那些拥有高质量数据集和数据科学人才的公司才能利用 AI 机会。去中心化 AI 是应对这一挑战的领先趋势之一。尽管对许多实际应用仍不切实际，但去中心化 AI 领域在 AI 社区中稳步获得关注。最近，微软的 AI 研究人员开源了[去中心化与协作 AI 区块链](https://github.com/microsoft/0xDeCA10B)项目，该项目支持基于区块链技术实现去中心化的机器学习模型。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

从训练到优化，机器学习模型生命周期中的每一个步骤都可以通过一定程度的去中心化进行改进。以一个简单的预测模型为例，该模型旨在预测某一产品的销售情况。在传统的集中式方法中，我们需要隐含地信任一组数据科学家来选择合适的神经网络架构、构建正确的数据集、高效训练模型、调整超参数以优化性能，以及其他十几个任务。完成这些之后，我们不能真正确定模型是否表现最佳。一旦我们开始引入新版本的模型，这个问题会变得更糟，因为几乎不可能将特定的变化与特定的性能相关联。去中心化的人工智能方法旨在通过实现透明的问责制和全阶段的有机协作来简化这个问题。

区块链技术的普及和成熟成为了去中心化人工智能架构的重要催化剂。区块链技术的不可变性和分布式共识模型本质上引入了信任水平，并在机器学习应用中实现了协作动态。微软研究团队利用区块链技术的一些原生功能，在机器学习模型中实现了不同程度的去中心化。

### 微软去中心化与协作人工智能

去中心化与协作人工智能在区块链上的应用（DCAI）是一个在区块链基础设施上托管和训练机器学习模型的框架。当前版本的 DCAI 限于以太坊区块链，并利用智能合约作为机器学习程序的主要封装机制。从概念上讲，智能合约是不可变的程序，包含在区块链运行时上执行的业务逻辑。在 DCAI 框架中，智能合约用于实现机器学习模型中的去中心化训练机制。

从功能角度来看，DCAI 根据三个主要组件来构建向机器学习模型添加数据/训练的过程。

1.  **激励机制：** 这个组件旨在鼓励高质量数据的贡献。激励机制负责验证交易，例如，在某些情况下需要“质押”或存款。

1.  **数据处理器：** 这个组件将数据和元数据存储在区块链上。这确保了它可以在所有未来的使用中访问，而不仅限于这个智能合约。

1.  **模型：** 这个组件封装了一个特定的机器学习模型，该模型根据预定义的训练算法进行更新。

![未提供此图像的替代文本](img/cbf6a095199e5b0782286ca8d35b48ec.png)

去中心化人工智能应用的一个基本挑战是依靠正确的激励机制，鼓励不同的参与方贡献新的数据集或训练机器学习模型。在当前版本中，DCAI 依赖于两个主要的激励模型：

+   **游戏化：** 利用这种激励机制，数据贡献者可以在其他贡献者验证他们的贡献时获得积分和徽章。该提案完全依赖于贡献者合作的意愿，以实现共同的目标——模型的改善。

+   **基于预测市场：** 在该模型中，如果贡献者的贡献在使用特定测试集进行评估时提高了模型的性能，他们将获得奖励。

以下动画展示了用于 IMDB 评价的情感分类模型中的激励机制。贡献高质量数据集的参与者能够根据模型的性能获利，而贡献效果不佳的参与者则失去他们的资金。

从编程模型的角度来看，DCAI 通过类似于以下内容的智能合约来抽象化机器学习模型的训练：

DCAI 仍处于实验阶段，但已经为人工智能模型引入了重要的好处：

+   **问责制：** DCAI 在以太坊区块链上保持数据集和模型性能的不可更改记录。

+   **数据可重用性：** DCAI 数据处理器将训练数据集记录到以太坊区块链，以备将来使用。

+   **合作：** DCAI 的激励机制创建了一个可行的机器学习模型训练合作模型。

### 其他有趣的去中心化人工智能倡议

DCAI 不是去中心化人工智能领域中唯一相关的倡议。虽然去中心化人工智能仍是一个新兴且仍在实验中的市场，但已经有一些倡议在研究阶段之外获得了一定的知名度：

+   [SingularityNet](https://singularitynet.io/)：SingularityNet 可以说是去中心化人工智能领域中最雄心勃勃的公司。它因驱动著名的 Sophia 机器人而闻名，SingularityNet 正致力于在人工智能生命周期的所有方面引入去中心化。从技术上讲，SingularityNet 是一个平台，能够以去中心化的模型实施和使用人工智能服务。基于以太坊区块链，SingularityNet 提供了一种模型，其中网络中的不同参与者因实施或使用人工智能服务而获得激励。

+   [Ocean Protocol](https://oceanprotocol.com/)：Ocean Protocol 提供了一个去中心化的数据提供者和消费者网络，支持 AI 应用的实现和使用。Ocean 通过一个代币化的服务层暴露了 AI 应用的许多常见基础设施元素，如存储、计算或算法，从而抽象出去中心化 AI 程序的核心构建模块。Ocean Protocol [最近通过独家代币发售平台 CoinList 宣布了一次代币销售](https://www.coindesk.com/angellist-spin-off-coinlist-announces-first-token-sale-of-2019)。

+   [Erasure](https://erasure.xxx/)：由创新型对冲基金 Numerai 创建，Erasure 提供了一个去中心化的协议，用于创建和运行预测模型。Erasure 的目标是提供一个去中心化的市场，数据科学家可以在其中上传基于可用数据的预测，使用加密代币对这些预测进行质押，并根据预测的表现赚取奖励。Erasure 为 Numerai 提供数据科学交互的事实体现在协议的简单性和实际应用性上。

+   [OpenMined](https://www.openmined.org/)：OpenMined 是去中心化人工智能市场中最活跃的项目之一。OpenMined 不仅仅是一个平台，它还是一个用于实施去中心化 AI 应用的工具和框架生态系统。OpenMined 已经建立了一个非常活跃的开发者社区，并提供与主流机器学习技术栈的无缝集成。

微软是机器学习和区块链技术领域的市场领导者之一。在微软的支持下，像 DCAI 这样的倡议有机会为大量用户带来更透明、负责任的机器学习模型。

[原文](https://www.linkedin.com/pulse/decentralized-collaborative-ai-how-microsoft-research-jesus-rodriguez/)。经许可转载。

**相关：**

+   This New Google Technique Help Us Understand How Neural Networks are Thinking

+   Neural Code Search: How Facebook Uses Neural Networks to Help Developers Search for Code Snippets

+   Introducing Gen: MIT’s New Language That Wants to be the TensorFlow of Programmable Inference

### 更多相关话题

+   [Federated Learning: Collaborative Machine Learning with a Tutorial…](https://www.kdnuggets.com/2021/12/federated-learning-collaborative-machine-learning-tutorial-get-started.html)

+   [Open Assistant: Explore the Possibilities of Open and Collaborative…](https://www.kdnuggets.com/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html)

+   [An Intuitive Explanation of Collaborative Filtering](https://www.kdnuggets.com/2022/09/intuitive-explanation-collaborative-filtering.html)

+   [有效的小型语言模型：微软的 13 亿参数 phi-1.5](https://www.kdnuggets.com/effective-small-language-models-microsoft-phi-15)

+   [使用 Python 自动化 Microsoft Excel 和 Word](https://www.kdnuggets.com/2021/08/automate-microsoft-excel-word-python.html)

+   [为什么越来越多的开发者在机器学习项目中使用 Python？](https://www.kdnuggets.com/2022/01/developers-python-machine-learning-projects.html)
