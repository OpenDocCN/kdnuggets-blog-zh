# 你可能不知道的7种低代码工具应用

> 原文：[https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

今天我想打破一些神话。我想揭示一个低代码工具只能解决简单问题的误解。特别是，我想带你参观使用开源低代码工具进行数据科学开发的应用，例如 [KNIME Analytics Platform](https://www.knime.com/knime-analytics-platform) 和它的商业对应产品 [KNIME Server](https://www.knime.com/knime-server)。例如，你知道使用 KNIME 软件可以连接到传感器板并将数据传输到仓库吗？或者你可以用深度学习模型创作音乐？或者你可以用 GANs 制作深度伪造图像？或者进行网页抓取？或者连接到云端的数据仓库？或者将应用程序部署为 REST 服务？或者构建仪表板？所有这些都可以通过低代码，甚至是非常低代码来实现。

让我们开始这次 tour。

# 1\. 从传感器板获取传感器数据

我想展示的第一个低代码解决方案是一个物联网应用，具体来说，是一个将数据从传感器板传输到数据仓库的 web 服务。通常，物联网问题涉及传感器连接、数据存储设置以及可选的时间序列分析来预测未来。我们也挑战自己解决了一个物联网问题，并建立了一个完全依赖 KNIME 软件的温度预测解决方案。具体来说，该项目的目标是了解第二天会有多热，即一个温度预测解决方案。

气象站由两个主要部分组成：数据收集部分和数据预测部分。

**数据收集**：

+   一个传感器板

+   一个用于数据存储的数据仓库

+   一个 REST 服务，将数据从传感器板传输到数据存储

**数据预测**：

+   一个经过训练的 sARIMA 模型，用于预测下一小时的温度

+   一个用于清理和处理获取的数据，并训练 sARIMA 模型的应用

+   一个仪表板应用，用于使用 sARIMA 模型预测下一小时的温度，以及预测第二天的最低和最高温度。

![你可能不知道的7种低代码工具应用](../Images/5f55ba71e0d85a1e9d37eb499e424bca.png)

图1\. 负责收集物联网传感器数据的 web 服务是用 KNIME 软件实现的。

每当有新的样本可用时，传感器板会触发数据收集的 REST 服务。REST 服务的唯一任务是验证请求数据的完整性，并将其存储到数据仓库中。

你可以在 KNIME Hub 文件夹 “[KNIME Weather Station](https://hub.knime.com/aliasghar_marvi/spaces/Public/latest/KNIME%20Weather%20Station~kA0mb0XmeTM2srRH/)” 中找到所有用于构建这个气象站的应用程序，更多细节可以参考 KNIME 博客中的这篇文章 “[Temperature Forecast from IoT sensor data](https://www.knime.com/blog/time-series-temperature-prediction-iot-sensors)”。

# 2\. 通过 iPhone 截图导入书籍元数据

类似于从传感器板传输数据的问题，还有从手机传输图像的问题。在这个第二个应用程序中，我们构建了一个书籍库存应用程序。在这里，一个 web 服务从 iPhone 接收一本书的照片，提取书籍元数据和封面，并通过 KNIME Server 将所有信息保存到最终的存储库中。

+   一个原生的定制移动应用通过 iPhone 相机捕捉书籍版权页的图像，将其转换为 base64 编码，并以请求对象的形式发送到 KNIME Server 上的 web 服务。这个 iPhone 应用的代码是按照 Apple 提供的规格实现的。

+   该 web 服务接受图像，通过 OCR 提取 ISBN 识别码，通过 Google Books 检索书籍元数据和封面图像，并最终将信息存储到数据存储库中（例如 SQLite 数据库）。

执行传输的 web 服务是使用 KNIME Analytics Platform 实现的，实际上非常简单，如图 1 所示。制作这个 web 服务只用了一行 Python 代码，具体在 “Decode base64 image” 元节点中。这行代码嵌入在 Python 脚本节点中，并引用了 Python 中的 base64.decodebytes() 函数。

你可以从 [KNIME Hub](https://kni.me/w/DpQmMrgB0Cx_o7-h) 免费下载工作流。

![7 个你不知道的低代码工具应用场景](../Images/645238a6577aa181c97656e93bf51290.png)

图 2\. 一个无代码的 RESTful API，从 iPhone 应用中导入截图，从 ISBN 识别码中提取书籍元数据和封面图像，并将其存储在数据库中。

# 3\. 将应用程序部署为 REST 服务

说到 REST 服务，你能想象使用 KNIME 创建 REST 服务有多简单吗？图 3 展示了一个示例。你只需要一个容器输入节点来接受请求中的数据，一个容器输出节点来提供响应的数据，以及中间的所有必要操作。实际上，每个移到 KNIME 服务器上的 KNIME 工作流都会自动作为 REST 服务生产化。因此，你所需要的只是两个用于数据输入和输出的容器节点。

![7 个你不知道的低代码工具应用场景](../Images/664ca98c5a19494f357a240fc64a964e.png)

图 3\. 包含容器节点以设置请求和响应数据结构的 KNIME 工作流，可部署为 REST 服务

通过 KNIME 工作流组装 REST 服务的过程在这篇文章 “[探索 REST 能力以实现 RESTful 工作流](https://www.knime.com/blog/explore-rest-capabilities-for-restful-workflows)” 中进行了说明。作为 REST 服务在 KNIME 服务器上执行 KNIME 工作流的过程在这篇文章 “[KNIME REST 服务器 API](https://www.knime.com/blog/the-knime-server-rest-api)” 中进行了说明。

# 4\. 构建仪表盘

不是所有的应用程序都作为 REST 服务投入生产。有些应用程序作为 web 应用程序部署，具有美观的仪表盘在网页浏览器上运行。使用 KNIME Analytics Platform 构建更复杂或简单的仪表盘也是可能的（而且很容易）。

Widget 和数据可视化节点提供视图项。将这些节点组装成一个组件，会生成一个复合视图，结合和关联组件内所有视图项的图形节点。图 4\. 显示了由一个非常简单但强大的组件生成的仪表盘示例。正如你所见，一些条形图和表格视图节点生成了一个复杂的仪表盘，包含连接和交互的表格及条形图。

![7 件你不知道的低代码工具功能](../Images/02f52b7ef207b7974b8e4a378e5064b8.png)

图 4\. 负责创建仪表盘（右侧）的组件内容（左侧）。

该应用的参考文章是 “[使用非常简单的组件创建仪表盘](https://medium.com/low-code-for-advanced-data-science/creating-a-dashboard-with-a-simple-component-b51ac91cb611)”，刊登在 Medium 杂志“高级数据科学的低代码”。相应的工作流也可在[KNIME Hub](https://kni.me/w/f7Q3YdyayggFxifM)上找到。

# 5\. 使用深度学习模型创建音乐

够了工程类型的任务！下一个问题是：像 KNIME Analytics Platform 这样的低代码工具能否实现先进的数据科学应用？让我们开始另一个探索。

KNIME Analytics Platform 提供了广泛的机器学习算法：从聚类到决策树，从随机森林到 XGBoost，从梯度提升树到回归，以及从神经网络到深度学习。使用基于 Keras 库的[KNIME 深度学习扩展](https://www.knime.com/deeplearning)，可以将实现不同神经层的不同节点组合在一起，创建不同类型的神经架构。

在图 5 中，棕色节点实现了神经层。神经架构相当复杂，包括三个输入层和三个输出层并行，因为音乐由三组向量序列表示：音符、时值和偏移量。该网络在舒伯特音乐数据上进行了训练，以生成音乐。尽管构建复杂，该网络的构建、训练和部署都无需编写一行代码。在“[AI 演奏钢琴](https://soundcloud.com/kathrin-melcher/aipiano?utm_source=clipboard&utm_campaign=wtshare&utm_medium=widget&utm_content=https%253A%252F%252Fsoundcloud.com%252Fkathrin-melcher%252Faipiano)”中聆听该网络生成的音乐样本。

这个项目的详细信息在文章“[我的 AI 为我弹钢琴](https://medium.com/low-code-for-advanced-data-science/my-ai-plays-piano-for-me-cd1b69e58b47)”中报告，刊登在“低代码高级数据科学”期刊上。

![7 个你不知道的低代码工具用途](../Images/87680b914ebc8716356d12014bf3b7d5.png)

图 5\. 这个深度学习网络包含一层 LSTM 单元，经过训练以生成舒伯特风格的音乐。

这个 AI 音乐生成项目实际上是对之前 AI 说唱歌曲（以及莎士比亚文本）生成项目的扩展。相关的文章“[使用深度学习像莎士比亚一样写作](https://medium.com/low-code-for-advanced-data-science/use-deep-learning-to-write-like-shakespeare-24ba8a22f4df)”也可以在同一 Medium 杂志中找到。

# 6\. 使用 GANs 创建假图像

另一个有趣的应用是生成对抗网络（GANs）在图像生成中的应用，在我们的案例中是生成面孔。

![7 个你不知道的低代码工具用途](../Images/08e740f6f7ab9eb2f15177d1bb37141c.png)

图 6\. 这个工作流程实现了一个 GAN，并在面孔数据集上进行了训练。

由于 KNIME 深度学习扩展目前尚未提供生成器和判别器层，因此在这里采用了 Python-KNIME 混合方法。所需的几行 Python 代码被打包在一个 KNIME 组件节点中，名为 DL Python Creator。不同参数的 DL Python Creator 组件实例分别实现了 GAN 的生成器和判别器部分（图 6）。一旦打包在组件中，所需的 Python 代码可以像处理其他 KNIME 节点一样处理，无需读取或更改底层代码。

使用了一个流行的 [Github 面孔数据集](blank) 来训练该网络。请注意，该数据集包含大量高分辨率彩色图像，因此训练该网络的计算量相当大。深度学习包的 GPU 加速使得在合理的时间内训练网络成为可能。

然后使用该网络生成假面孔，您可以在图 7 中查看结果。

关于此应用的更多细节可见于 Medium Journal “Low Code for Advanced data science” 上的文章 [如何用 KNIME Analytics Platform 创建 GAN](https://medium.com/low-code-for-advanced-data-science/how-to-create-gans-with-knime-analytics-platform-bd9ae7083ef)。

![你不知道你可以用低代码工具做的 7 件事](../Images/5dcf802505e5e05fac04cd7cc9c6be31.png)

图 7\. 由我们训练的 GAN 生成的一些虚假面孔。

# 7\. 构建一个推特机器人

我想在我的列表中插入的最后一个应用是推特机器人。也就是说，一个应用程序（机器人）访问推特，提取所有包含给定标签的推文，然后转发这些推文。实现这个推特机器人的工作流相当简单（图 8）。它访问一个推特账户，执行对选定标签的搜索，进行一些清理，如删除转发的推文，然后重新发布所有剩余的推文。

该应用程序在期刊“Low Code for Advanced data science”中的文章 “[确认你是机器人](https://medium.com/low-code-for-advanced-data-science/confirm-that-you-are-a-robot-39deaf9330d1?source=friends_link&sk=ccb8e1ee1e730fb2b5ee8509182411f8)” 中有所描述。

![你不知道你可以用低代码工具做的 7 件事](../Images/8ace89006421963453fed1d75bb91873.png)

图 8\. 实现推特机器人的工作流。

# 结论

这些简单的工作流只是低代码世界中某些任务变得如此轻松的一个例子，相较于编码世界。实际上，连接节点——所有连接节点——必须实现所有所需操作以访问选定的数据源。这使得连接操作变得像拖放一样简单，并将所需的权限、驱动程序和配置的所有复杂性隐藏在用户的视线之外。

![你不知道你可以用低代码工具做的 7 件事](../Images/6b2306d438ea75e57812aa5d5c608d21.png)

图 9\. KNIME 连接器备忘单。

KNIME Analytics Platform，例如，包含了大量这样的高级连接节点，以访问各种数据源，如 SQL 和 noSQL 数据库、REST 服务、云存储、大数据平台、Spark、Kafka、各种文件、MS Office 文档、Google 文档等等。通过 KNIME 连接器可以访问的大部分数据源见图 9。此备忘单可以从 KNIME 网站的页面 “[KNIME Connector Cheatsheet.pdf](https://www.knime.com/sites/default/files/2021-07/cheat-sheet-connectors.pdf)” 免费下载。

**[Rosaria Silipo](https://www.linkedin.com/in/rosaria/?originalSubdomain=ch)**不仅在数据挖掘、机器学习、报告和数据仓库方面是一位专家，还成为了KNIME数据挖掘引擎的公认专家，她已出版了三本相关书籍：*《KNIME初学者的好运》*、*《KNIME Cookbook》* 和 *《KNIME Booklet for SAS Users》*。Rosaria曾在整个欧洲为许多公司担任自由数据分析师。她还曾在Viseca（苏黎世）领导SAS开发团队，在Spoken Translation（加州伯克利）用C#实现语音转文字和文字转语音接口，并在Nuance Communications（加州门洛帕克）开发了多种语言的语音识别引擎。Rosaria于1996年在意大利佛罗伦萨大学获得生物医学工程博士学位。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

### 更多相关主题

+   [关于SAS数据科学学院的3件你不知道的事](https://www.kdnuggets.com/2022/07/sas-3-things-didnt-know-sas-academy-data-science.html)

+   [4个你可能不知道的Python Itertools过滤函数](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)

+   [宣布PyCaret 3.0：开源、低代码的Python机器学习](https://www.kdnuggets.com/2023/03/announcing-pycaret-30-opensource-lowcode-machine-learning-python.html)

+   [低代码：开发者还需要吗？](https://www.kdnuggets.com/2022/04/low-code-developers-still-needed.html)

+   [可能影响数据分析准确性的3个错误](https://www.kdnuggets.com/2023/03/3-mistakes-could-affecting-accuracy-data-analytics.html)

+   [关于数据管理你需要知道的6件事及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)
