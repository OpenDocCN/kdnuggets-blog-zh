# 构建一个回答常见问题的机器人：预测文本相似度

> 原文：[https://www.kdnuggets.com/2017/03/bot-answer-faqs-predicting-text-similarity.html](https://www.kdnuggets.com/2017/03/bot-answer-faqs-predicting-text-similarity.html)

**由 Amanda Sivaraj 撰写，[indico](https://indico.io/)。**

在我们[关于客户支持机器人的前一个教程](https://indico.io/blog/building-bot-better-customer-support/)中，我们使用自定义集合 API 训练了一个机器人，以将客户引导到最适合解决他们问题或查询的团队成员。这个机器人提高了我们团队的响应速度，因为我们不再需要依赖一个人类协调者（他在公司里还扮演许多其他角色 #startuplife）来完成这项工作。然而，我们通常只能在东部时间上午11点到下午7点的办公时间内响应，因此在这个时间段之外的询问仍然会有延迟。我们如何改进这一点？构建一个回答常见问题的机器人，以减少对更多客户的延迟，并确保我们的工程师不需要花费过多时间远离我们为你构建的产品 :).

![](../Images/1157128c1d56bcc71b5f88ab2458fa93.png)

### 任务

我们将进行 Python 中的最近邻搜索，将用户输入的问题与常见问题列表进行比较。为此，我们将使用 indico 的文本特征 API 来查找文本数据的所有[特征向量](https://en.wikipedia.org/wiki/Feature_vector)，并计算这些向量与用户输入问题的向量在 300 维空间中的距离。然后，我们将根据用户的问题与哪个常见问题最相似（如果它符合某个置信度阈值）返回相应的答案。

### 入门指南

首先，从我们的 SuperCell GitHub 仓库[获取骨架代码](https://github.com/IndicoDataSolutions/SuperCell/tree/master/faqs_bot)。

如果你没有所需的所有包，你需要安装它们——`texttable` 和当然还有`indicoio`。

如果你还没有设置 indico 账户，请按照我们的[快速入门指南](https://indico.io/docs#quickstart)操作。它将引导你完成获取 API 密钥和安装`indicoio` Python 库的过程。如果遇到任何问题，请查看文档中的[安装](https://indico.io/docs#install)部分。如果问题依然存在，你也可以通过那个小聊天气泡联系我们。假设你的账户已设置完毕，并且一切都已安装好，我们就开始吧！

首先，前往文件的顶部并导入`indicoio`。不要忘记设置你的 API 密钥。有[多种方法可以做到这一点](https://indico.io/docs#config)；我喜欢把我的 API 密钥放在配置文件中。

```py
 import indicoio
   indicoio.config.api_key = 'YOUR_API_KEY' 
```

### 使用 indico 的文本特征 API

你需要将常见问题及其答案存储在一个字典中。为了简便起见，我在脚本中创建了一个包含五个问题和答案的字典`faqs`。这将是我们的初始数据集。我们只需要提取问题的文本特征，而不是答案，因此我们提取`faqs.keys()`，然后将这些数据传递给我们的`make_feats()`函数。

接下来，让我们更新`run()`函数。将`feats`保存到Pickle文件中，这样每次你想将用户的问题与静态的FAQ列表进行比较时，就不需要每次都重新运行文本特征API。

### 比较常见问题与用户输入

现在我们已经获得了FAQ文本数据的特征表示，让我们进入下一阶段：收集和比较用户问题与我们的FAQ。为了使每个人都可以在本地运行这个脚本，无论你打算将它连接到什么客户支持聊天服务，我们将使用`raw_input()`。你需要根据你的消息应用程序的文档设置自己的webhook。

首先，让我们获取一个输入，将其添加到FAQ列表中，并为输入找到文本特征，然后将其添加到主要的`feats`列表中。这将简化我们在以后计算所有特征表示的距离时的操作。更新`input_question()`函数：

现在是时候再次更新`run()`函数了。这一次，你可以直接加载之前找到的FAQ特征的Pickle文件。

现在我们已经为所有FAQ和用户的问题获得了特征向量列表！这将如何帮助我们找出输入最相似的FAQ？文本之间的相似性是通过它们对应的特征向量之间的相似性来衡量的。我们在`calculate_distances`函数中预测它们的相似性，该函数计算这些向量在余弦空间中的距离。余弦通常是处理高维空间中点时的比较指标。`calculate_distances`生成一个`m`乘`n`的矩阵，该矩阵存储文档`m`和文档`n`之间的距离，位置为`distance_matrix[m][n]`。

再次更新`run()`函数：

最后，让我们看看我们的最近邻搜索表现如何！`similarity_text()`函数将遍历`distance_matrix`，根据相似度级别对每一段文本进行排序，然后以表格形式打印出来。我们不希望机器人在不确定找到FAQ匹配的情况下给出答案，因此我们需要设置一个信心阈值。将以下代码添加到`similarity_text()`中，紧接在`print t.draw()`之后：

如果机器人的信心水平达到阈值，它应该返回相应的FAQ答案。否则，它应该通知你的客户支持经理（你需要根据你的消息应用程序的文档来配置）：

最后一次更新`run()`函数，然后运行代码！

它的表现如何？这里有一个示例：

![常见问题机器人在终端中的结果](../Images/a0756344aaf110af06e421f5d7e6f105.png)

哇！它的表现相当不错，尽管输入问题的用词与FAQ匹配时有所不同且更加简洁，我们的丰富文本功能仍然能够捕捉其含义。那么，这里发生了什么——为什么会这样呢？

indico 的文本特征 API 为给定的文本输入创建了数十万个丰富的特征向量表示，这些特征是使用深度学习技术学习到的。这些特征向量——在多维空间中的数值表示——是计算机为一个词赋予意义的一种方式。

当我们将FAQ列表输入文本特征算法时，它本质上识别了每个问题中的某些词为“重要”，并确定了它们在多维空间中的位置。文本特征然后将这些词的表示组合起来，生成整个“文档”的表示（在这种情况下是FAQ——你也可以传入整篇文章来找到其特征表示）。当我们展示给它我们的新输入问题时，它做了同样的事情。然后我们使用这些文档表示来计算它们之间的距离，以确定两个“文档”在概念上的相似性。多维空间中的特征越接近，它们的意义就越相近。

### 下一步

从本质上讲，这是一个预测文本相似性的练习。你能想象这个任务的其他应用吗？如果你用我们的 API 创建了什么酷东西，一定要告诉我们，邮箱是 [contact@indico.io](mailto:contact@indico.io)！

**简介：[Amanda Sivaraj](https://www.linkedin.com/in/amandasivaraj/)** 是一位内容创作者、数字营销专家以及数据与技术爱好者。她为[indico](https://indico.io/)开发内容和教程，该公司提供用于自动化文本和图像分析的机器学习工具。

[原文](https://indico.io/blog/faqs-bot-text-features-api/)。经许可转载。

**相关：**

+   [增强版聊天机器人：推动聊天机器人的10项关键机器学习能力](/2017/01/chatbots-steroids-10-key-machine-learning-capabilities.html)

+   [人工智能与聊天机器人的语音识别：入门](/2017/01/artificial-intelligence-speech-recognition-chatbots-primer.html)

+   [近期聊天机器人技术进展的简要概述](/2017/01/grakn-year-review-chatbot-technologies.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关主题

+   [ChatGPT如何运作：机器人背后的模型](https://www.kdnuggets.com/2023/04/chatgpt-works-model-behind-bot.html)

+   [如何回答数据科学编码面试问题](https://www.kdnuggets.com/2022/01/answer-data-science-coding-interview-questions.html)

+   [你必须知道的前10个高级数据科学SQL面试问题…](https://www.kdnuggets.com/2023/01/top-10-advanced-data-science-sql-interview-questions-must-know-answer.html)

+   [Google通过在Docs和Gmail中加入生成性AI来回应ChatGPT](https://www.kdnuggets.com/2023/03/google-answer-chatgpt-adding-generative-ai-docs-gmail.html)

+   [评估计算文档相似性的方法](https://www.kdnuggets.com/evaluating-methods-for-calculating-document-similarity)

+   [使用回归模型预测加密货币价格](https://www.kdnuggets.com/2022/05/predicting-cryptocurrency-prices-regression-models.html)
