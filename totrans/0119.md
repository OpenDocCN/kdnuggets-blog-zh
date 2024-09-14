# ChatGPT 代码解释器：在几分钟内进行数据科学

> 原文：[https://www.kdnuggets.com/2023/07/chatgpt-code-interpreter-data-science-minutes.html](https://www.kdnuggets.com/2023/07/chatgpt-code-interpreter-data-science-minutes.html)

![ChatGPT 代码解释器：在几分钟内进行数据科学](../Images/2cefeb6ea7223a4a1b878193a4598000.png)

图片来自 Midjourney

作为一名数据科学家，我总是寻找最大化效率并通过数据驱动业务价值的方法。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

因此，当 ChatGPT 发布了其最强大的功能之一——Code Interpreter 插件时，我迫不及待地想尝试并将其纳入我的工作流程。

# 什么是 ChatGPT 代码解释器？

如果你还没有听说过 Code Interpreter，这是一项新功能，允许你在 ChatGPT 界面中上传代码、运行程序和分析数据。

在过去的一年中，每次我需要调试代码或分析文档时，我都必须将我的工作复制并粘贴到 ChatGPT 中以获得回应。

这证明是耗时的，而且 ChatGPT 界面有字符限制，这限制了我分析数据和执行机器学习工作流的能力。

Code Interpreter 通过允许你将自己的数据集上传到 ChatGPT 界面来解决所有这些问题。

尽管它被称为“代码解释器”，但这个功能并不限于程序员——该插件可以帮助你分析文本文件、总结 PDF 文档、构建数据可视化，甚至按你所需的比例裁剪图像。

# 如何访问代码解释器？

在我们深入其应用之前，让我们快速了解如何开始使用 Code Interpreter 插件。

要访问此插件，你需要订阅 [ChatGPT Plus](https://openai.com/blog/chatgpt-plus)，当前订阅费为每月 $20。

不幸的是，Code Interpreter 还没有对未订阅 ChatGPT Plus 的用户开放。

一旦你拥有了付费订阅，只需导航到 [ChatGPT](https://chat.openai.com/) 并点击界面左下角的三个点。

然后，选择设置：

![ChatGPT 代码解释器：在几分钟内进行数据科学](../Images/981c000c06414b3b5303fe3d8d091100.png)

图片由作者提供

点击“Beta 功能”，并启用标记为 Code Interpreter 的滑块：

![ChatGPT 代码解释器：在几分钟内进行数据科学](../Images/b230614abd01dd10f800d3dce8af7439.png)

图片由作者提供

最后，点击“新聊天”，选择“GPT-4”选项，并在出现的下拉菜单中选择“代码解释器”：

你会看到一个看起来像这样的屏幕，文本框附近有一个“+”符号：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/4f07dace7dbf055ed1bc6ee6842d42e4.png)

图片来源于作者

太棒了！你现在已经成功启用了 ChatGPT 代码解释器。

在这篇文章中，我将向你展示五种使用代码解释器来自动化数据科学工作流程的方法。

# 1\. 数据总结

作为数据科学家，我花了很多时间只是为了理解数据集中不同的变量。

代码解释器非常擅长为你拆解每一个数据点。

下面是如何让模型帮助你总结数据的方法：

让我们用 Kaggle 上的 [泰坦尼克号生存预测](https://www.kaggle.com/competitions/titanic) 数据集作为这个示例。我将使用“*train.csv*”文件。

下载数据集并导航到代码解释器：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/189479dd0dddc6c12ee4fba9474c4265.png)

图片来源于作者

点击“+”符号并上传你想要总结的文件。

然后，要求 ChatGPT 用简单的术语解释这个文件中的所有变量：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/bc1d05a25e5fca54acd9cbf9bd38f572.png)

图片来源于作者

完美！

代码解释器为我们提供了数据集中每个变量的简单解释。

# 2\. 探索性数据分析

现在我们对数据集中的不同变量有了理解，让我们要求代码解释器更进一步，进行一些 EDA。

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/513f77c01ff4807126eb33784a501fed.png)

图片来源于作者

模型生成了 5 个图表，让我们更好地理解数据集中的不同变量。

如果你点击“显示工作”下拉菜单，你会注意到代码解释器已经编写并运行了 Python 代码，以帮助我们实现最终结果：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/2409886d5747acd555067d83b6b6d102.png)

图片来源于作者

如果你想进行进一步分析，可以随时将这段代码复制粘贴到你自己的 Jupyter Notebook 中。

ChatGPT 还根据生成的可视化图提供了对数据集的一些见解：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/9ddce43c4af3617cc072eeeb6f08b06d.png)

图片来源于作者

它告诉我们女性、头等舱乘客和年轻乘客的生存率更高。

这些见解如果手动推导，会花费大量时间，尤其是当你对 Python 和像 Matplotlib 这样的数据可视化库不太熟悉时。

代码解释器在几秒钟内生成了这些结果，大大减少了执行 EDA 所需的时间。

# 3\. 数据预处理

我花了很多时间清理数据集并为建模过程做准备。

让我们请求代码解释器帮助我们预处理这个数据集：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/03ba50b01145025ad5700fa6fe345aad.png)

作者提供的图片

代码解释器已经概述了清理这个数据集过程中涉及的所有步骤。

它告诉我们需要处理三个缺失值的列，编码两个分类变量，执行一些特征工程，并删除与建模过程无关的列。

它创建了一个 Python 程序，在短短几秒钟内完成了所有预处理工作。

如果你想了解模型进行数据清理的步骤，可以点击“显示工作”：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/32e0ff275aa4ca8f99e12e211a47d3eb.png)

作者提供的图片

然后，我问 ChatGPT 如何保存输出文件，它给了我一个可下载的 CSV 文件：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/06c0695d4268828f741f606d524139e9.png)

作者提供的图片

请注意，在这个过程中我甚至没有运行过一行代码。

代码解释器能够接收我的文件，在界面中运行代码，并在创纪录的时间内给我提供输出。

# 4\. 构建机器学习模型

最后，我请代码解释器使用预处理后的文件来构建一个机器学习模型，以预测一个人是否会在泰坦尼克号沉船事件中幸存：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/bee40ab73781a461ca8c2e7cf884745e.png)

作者提供的图片

它在不到一分钟的时间内建立了模型，并达到了83.2%的准确率。

它还给我提供了一个混淆矩阵和分类报告，总结了模型的性能，并解释了所有指标的含义：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/8b661b02a6fbee79510cc8ceb4c4d496.png)

作者提供的图片

我要求 ChatGPT 提供一个输出文件，将模型预测与乘客数据进行映射。

我还想要一个它创建的机器学习模型的可下载文件，因为我们可以随时进行进一步的微调，并在其基础上进行训练：

![ChatGPT 代码解释器：几分钟内完成数据科学](../Images/f1922e557735f5e2564fa25a527c8326.png)

作者提供的图片

# 5\. 代码解释

我发现代码解释器的另一个有用应用是能够提供代码解释。

前几天，我在做情感分析模型时发现了一些与我的用例相关的 GitHub 代码。

我没有理解全部代码，因为作者导入了我不熟悉的库。

使用代码解释器，你只需上传一个代码文件，并请求它清楚地解释每一行。

你还可以要求它调试和优化代码，以获得更好的性能。

这里有一个例子：我上传了一个包含我几年前编写的代码的文件，用于构建一个 Python 仪表盘：

![ChatGPT Code Interpreter: 在几分钟内进行数据科学](../Images/41d0326b7143a1d8f259acc7ae99d200.png)

图片来源：作者

Code Interpreter 分解了我的代码，并清楚地概述了每个部分所做的工作。

![ChatGPT Code Interpreter: 在几分钟内进行数据科学](../Images/4497a7f445f1d28ca80922f3660c513d.png)

图片来源：作者

它还建议重构我的代码以提高可读性，并解释了我可以在哪里添加新部分。

我没有自己做这些，而是简单地要求 Code Interpreter 重构代码并提供改进版：

![ChatGPT Code Interpreter: 在几分钟内进行数据科学](../Images/1c01772c5113b9a886f78990d7ec4098.png)

图片来源：作者

Code Interpreter 重写了我的代码，将每个可视化封装到单独的函数中，使其更容易理解和更新。

# ChatGPT 代码解释器对数据科学家意味着什么？

目前围绕 Code Interpreter 的宣传很多，因为这是我们第一次看到一个能够读取代码、理解自然语言并执行端到端数据科学工作流的工具。

然而，重要的是要记住，这只是另一个工具，旨在帮助我们更高效地进行数据科学。

到目前为止，我一直在使用它来构建基线模型，使用的是虚拟数据，因为我不能将敏感的公司信息上传到 ChatGPT 界面。

此外，Code Interpreter 不具备特定领域的知识。我通常将它生成的预测作为基线预测？—？我经常需要调整其生成的输出，以匹配我组织的使用案例。

我无法展示由一个对公司内部运作没有可见性的算法生成的数字。

最后，我并不是每个项目都使用 Code Interpreter，因为我处理的一些数据包含数百万行，并且存储在 SQL 数据库中。

这意味着我仍然需要自己执行大部分查询、数据提取和转换操作。

如果你是初级数据科学家或有志成为数据科学家，我建议你学习如何利用像 Code Interpreter 这样的工具，更高效地完成工作中的重复部分。

本文到此为止，谢谢阅读！

**[Natassha Selvaraj](https://linktr.ee/natasshaselvaraj)** 是一位自学成才的数据科学家，热爱写作。你可以通过 [LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/) 与她联系。

### 更多相关信息

+   [5 种你可以使用 ChatGPT 的代码解释器进行数据科学的方法](https://www.kdnuggets.com/2023/08/5-ways-chatgpt-code-interpreter-data-science.html)

+   [介绍 MetaGPT 的数据解释器：SOTA 开源 LLM 基于数据解决方案](https://www.kdnuggets.com/metagpt-data-interpreter-open-source-llm-based-data-solutions)

+   [3 分钟理解偏差-方差权衡](https://www.kdnuggets.com/2020/09/understanding-bias-variance-trade-off-3-minutes.html)

+   [在 5 分钟内构建一个机器学习网页应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [在 5 分钟内用 Python 构建一个网页爬虫](https://www.kdnuggets.com/2022/02/build-web-scraper-python-5-minutes.html)

+   [介绍 OpenChat：一个免费且简单的平台，用于在几分钟内构建自定义聊天机器人](https://www.kdnuggets.com/2023/06/introducing-openchat-free-simple-platform-building-custom-chatbots-minutes.html)
