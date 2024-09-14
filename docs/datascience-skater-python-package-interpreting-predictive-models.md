# DataScience.com 发布了用于解释预测模型决策过程的 Python 包。

> 原文：[`www.kdnuggets.com/2017/05/datascience-skater-python-package-interpreting-predictive-models.html`](https://www.kdnuggets.com/2017/05/datascience-skater-python-package-interpreting-predictive-models.html)

赞助文章。

DataScience.com 发布了其新 Python 库 **[Skater](https://www.datascience.com/resources/tools/skater)** 的测试版，该库用于解释预测模型。Skater 通过结合多种算法来解释数据与模型预测之间的关系，使用户能够评估模型的表现并识别关键特征。

随着机器学习在商业应用中变得越来越受欢迎——例如评估贷款申请者的信用 worthiness、向观众推荐相关内容以及预测在线购物者的消费金额——解释也正成为模型构建过程中的不可或缺的一部分。Skater 提供了一个通用框架，用于描述预测模型，无论构建它们所用的算法是什么，使数据科学从业者可以自由选择所用技术，而不必担心其复杂性。

> “在许多情况下，数据科学家会使用简单的建模技术，如线性回归或决策树，因为这些模型容易解释，”DataScience.com 首席战略官威廉·梅尔坎（William Merchan）说。“实际上，他或她是在牺牲性能以换取可解释性；例如，神经网络或集成模型更难以解释，但能产生高度准确的预测。Skater 旨在消除这种妥协。”

Skater 具有模型无关的部分依赖图，这是一种描述预测变量与目标变量之间关系的可视化图表，并且变量重要性衡量了特征对预测的影响程度。它还改进了现有的模型解释方法，如局部可解释模型无关解释（LIME）。Skater 允许将这些方法应用于任何机器学习模型——从集成模型到神经网络——无论是本地可用还是作为 API 部署。

[使用 Skater](https://www.datascience.com/resources/tools/skater)，数据科学从业者可以：

+   **评估模型在完整数据集或单个预测上的行为**：Skater 通过利用和改进现有技术（包括部分依赖图、相对变量重要性和 LIME），实现了对模型的全局和局部解释。

+   **识别潜在的特征交互并建立领域知识**：从业者可以使用 Skater 理解特征在特定用例中的相互关系——例如，信用风险模型如何使用银行客户的信用历史、支票账户状态或现有信用额度来批准或拒绝其信用卡申请——然后利用这些信息来指导未来的分析。

+   **衡量模型在生产环境中部署后的性能变化**：Skater 支持对已部署模型的解释，给从业者提供了衡量特征交互如何在不同模型版本间变化的机会。

“DataScience.com 企业数据科学平台的一个关键特点是能够通过 REST API 部署模型，使其能够立即与仪表板或实时应用程序集成，”Merchan 补充道。“Skater 帮助我们更进一步，使我们能够以数据科学从业者以及最终的非技术利益相关者都能理解的方式，解释部署在我们平台上的复杂模型——无论是在我们的平台上还是其他地方。”

Skater 包可通过 [GitHub](https://www.datascience.com/resources/tools/skater) 获取，并可以通过 pip 从 PyPI 轻松安装。

欲了解更多信息，请访问 [www.datascience.com](http://www.datascience.com)。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [如何使用 MLFlow 打包和分发机器学习模型](https://www.kdnuggets.com/2022/08/package-distribute-machine-learning-models-mlflow.html)

+   [Nota AI 发布 NetPresso 模型搜索的 beta 版本，他们…](https://www.kdnuggets.com/2022/04/nota-ai-releases-beta-version-netpresso-model-search-hardwareaware-automl-tool.html)

+   [KDnuggets™ 新闻 22:n01, 1 月 5 日：追踪和可视化的 3 个工具…](https://www.kdnuggets.com/2022/n01.html)

+   [构建预测模型：Python 中的逻辑回归](https://www.kdnuggets.com/building-predictive-models-logistic-regression-in-python)

+   [机器学习教材：随机过程与模拟](https://www.kdnuggets.com/2022/03/datashaping-machine-learning-textbook-stochastic-processes-simulations.html)

+   [Orca LLM：模拟 ChatGPT 的推理过程](https://www.kdnuggets.com/2023/06/orca-llm-reasoning-processes-chatgpt.html)
