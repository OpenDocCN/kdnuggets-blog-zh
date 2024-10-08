# 将机器学习算法完整端到端地部署到实时生产环境中

> 原文：[`www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html`](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

![](img/128d7ad5f8e3664b5782ae9a622a497e.png)

图片来源：[Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 于 [Unsplash](https://unsplash.com/s/photos/python?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 引言

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

在 2021 年 10 月，我撰写了一篇关于“将机器学习和数据科学项目部署为公开网络应用”的文章（见 [`towardsdatascience.com/deploying-machine-learning-and-data-science-projects-as-public-web-applications-3abc91088c11`](https://towardsdatascience.com/deploying-machine-learning-and-data-science-projects-as-public-web-applications-3abc91088c11)）。

在这篇文章中，我探讨了如何使用 Voila、GitHub 和 mybinder 将 Jupyter Notebooks 部署为公开的网络应用。

文章发布后，我收到读者反馈，他们对如何进一步推动生产部署感兴趣，希望探索如何将机器学习算法完全部署到实时生产环境中，以便能够以平台无关的方式“消费”它，这也促成了这篇文章的产生……

## 第一步：开发一个机器学习算法

第一步是开发我们想要部署的机器学习算法。在实际应用中，这可能涉及数周或数月的开发时间以及在数据科学管道各个步骤中进行大量的迭代，但在这个例子中，我将开发一个基本的机器学习算法，因为这篇文章的主要目的是找到一种可以供“消费者”使用的算法部署方式。

我从 kaggle 选择了一个数据集 ([`www.kaggle.com/prathamtripathi/drug-classification`](https://www.kaggle.com/prathamtripathi/drug-classification))，该数据集由作者创建，并具有“CC0: 公共领域”许可，这意味着它没有版权，可以在其他作品中自由使用（详情请见 [`creativecommons.org/publicdomain/zero/1.0/`](https://creativecommons.org/publicdomain/zero/1.0/)）。

开发一个预测性机器学习算法以根据一系列患者标准分类药物处方的 Python 代码如下 -

```py
0.99 0.012247448713915901
```

此时我们可以看到，我们有一个训练好的机器学习算法用于预测药物处方，并且交叉验证（即数据折叠）已被用来评估模型准确率达到 99%。

目前一切顺利……

我们将把这个模型部署到生产环境中，虽然这是一个简单的示例，但我们不希望每次用户想要预测药物处方时都需要在实时环境中重新训练模型，因此我们的下一步是使用`pickle`保存我们训练过的模型的状态……

现在，每当我们想使用训练好的模型时，我们只需从`model.pkl`文件中重新加载其状态，而不是重新执行训练步骤。

## 步骤 2：从训练模型中进行单独预测

我将在步骤 2 中做几个假设 -

1.  机器学习算法的消费者有一个需求，就是对单个患者进行预测，而不是对一批患者进行预测。

1.  那些消费者希望使用类似文本的值（例如血压 = “正常”或“高”）来与算法进行通信，而不是它们的标签编码等效值（如 0 和 1）。

因此，我们将从审查所有标签编码的类别特征的值开始，这些特征作为输入传递给算法……

```py
Sex ['F', 'M'] [0, 1] 

BP ['HIGH', 'LOW', 'NORMAL'] [0, 1, 2] 

Cholesterol ['HIGH', 'NORMAL'] [0, 1] 

Drug ['DrugY', 'drugC', 'drugX', 'drugA', 'drugB'] [0, 3, 4, 1, 2]
```

这样，我们就得到了一个包含每个类别特征及其在数据中出现的唯一值和通过`LabelEncoder()`转换后的相应数值的列表。

有了这些信息，我们可以提供一组字典，将类似文本的值（例如“高”、“低”等）映射到它们的编码等效值，然后开发一个简单的函数来进行单独的预测，如下所示……

然后可以通过调用函数进行一些基于原始数据值的预测来验证这个实现，以便我们知道输出应该是什么……

```py
'drugC'
```

```py
'DrugY'
```

请注意，我们的`predict_drug`函数不需要训练模型，而是将先前通过`pickle`保存状态的模型“重新加载”到`model.pkl`文件中，我们可以从输出中看到，药物推荐的预测是正确的。

## 步骤 3：开发一个 Web 服务包装器

目前一切看起来都很好，但这里有一个主要问题：我们的机器学习算法的客户端或消费者必须用 Python 编程语言编写，不仅如此，我们还必须能够更改和修改应用程序。

如果第三方应用程序希望使用和消耗我们的算法，并且这个第三方应用程序不是用 Python 编写的怎么办？也许它是用 Java、C#、JavaScript 或其他非 Python 语言编写的。

这就是 Web 服务发挥作用的地方。Web 服务是一个“封装器”，它接收来自客户端和消费者的 HTTP GET 和 HTTP PUT 请求，调用 Python 代码并返回 HTML 响应。

这意味着客户端和调用者只需能够构造 HTTP 请求，几乎所有编程语言和环境都有实现这一点的方法。

在 Python 世界中，有几种不同的方法可供选择，但我选择的方式是使用 `flask` 构建我们的 Web 服务封装器。

代码本身并不复杂，但配置 VS Code 以使开发者能够调试 Flask 应用可能会有挑战。如果你需要这个步骤的教程，请查看我的文章《如何在 VS Code 中调试 Flask 应用》，可以在这里找到——[`towardsdatascience.com/how-to-debug-flask-applications-in-vs-code-c65c9bdbef21`](https://towardsdatascience.com/how-to-debug-flask-applications-in-vs-code-c65c9bdbef21)。

这是 Web 服务的封装器代码 …

从 Anaconda Navigator 页面启动 VS Code IDE（或通过启动 Anaconda 命令提示符并输入 `code`）。这将启动带有 Conda 基础环境的 VS Code，这是运行和调试 Flask 应用所必需的。

可以通过在 VS Code 中点击“运行和调试”然后选择“Flask 启动和调试 Flask Web 应用”来启动 Web 服务 -

![](img/996b565b247c8c94255815795a53f8be.png)

图片由作者提供

如果一切顺利，TERMINAL 窗口中的最后一条消息应为 `Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)`，这表示你的 Flask Web 应用已成功运行。

现在你应该使用以下方法之一来测试你的 Web 服务 -

1.  打开一个网页浏览器并输入：[`127.0.0.1:5000/drug?Age=60&Sex=F&BP=LOW&Cholesterol=HIGH&Na_to_K=20`](http://127.0.0.1:5000/drug?Age=60&Sex=F&BP=LOW&Cholesterol=HIGH&Na_to_K=20)

1.  打开 Anaconda 命令提示符并输入：`curl -X GET "http://127.0.0.1:5000/drug?Age=60&Sex=F&BP=LOW&Cholesterol=HIGH&Na_to_K=20"`

![](img/cd1f7c0d6b463172e182083c84035586.png)

图片由作者提供

如果你想了解更多关于开发 `flask` 应用和 Web 服务的信息，这些文章是一个很好的起点 -

+   [`programminghistorian.org/en/lessons/creating-apis-with-python-and-flask`](https://programminghistorian.org/en/lessons/creating-apis-with-python-and-flask)

+   [`code.visualstudio.com/docs/python/tutorial-flask`](https://code.visualstudio.com/docs/python/tutorial-flask)

## 第 4 步：将 Web 服务部署到 Microsoft Azure

我们现在有了一个预测性机器学习算法，可以以 99% 的准确率预测药物处方，我们有一个可以进行单独预测的辅助函数，并且我们有一个允许这些组件通过浏览器或命令行调用的 Web 服务封装器。

然而，这一切仍然只能在开发环境中调用。下一阶段是将所有内容部署到云中，以便客户可以通过公共互联网“访问” Web 服务。

有许多不同的公共服务可用于 Web 应用程序的部署，包括 -

+   谷歌 — [`cloud.google.com/appengine/docs/standard/python3/building-app/writing-web-service`](https://cloud.google.com/appengine/docs/standard/python3/building-app/writing-web-service)

+   亚马逊 Web 服务 — [`medium.com/@rodkey/deploying-a-flask-application-on-aws-a72daba6bb80`](https://medium.com/@rodkey/deploying-a-flask-application-on-aws-a72daba6bb80)

+   Microsoft Azure — [`medium.com/@nikovrdoljak/deploy-your-flask-app-on-azure-in-3-easy-steps-b2fe388a589e`](https://medium.com/@nikovrdoljak/deploy-your-flask-app-on-azure-in-3-easy-steps-b2fe388a589e)

我选择了 Azure，因为它是免费的（对于入门级账户），易于使用，快速，并且与我最喜欢的开发环境 VS Code 完全集成。

### 步骤 4.1：将 Azure App Service 扩展添加到 VS Code

切换到 VS Code，转到“扩展”（Ctrl+Shft_X），然后添加“Azure App Service”扩展。添加扩展后，您将在活动栏中看到一个新的 Azure 图标 -

![](img/80c2bd9c142d71a0e2634af868fc9abd.png)

作者提供的图片

### 步骤 4.1：创建一个 Azure 账户

您必须拥有一个账户才能开始在 Azure 云中进行部署，并且在注册过程中必须提供信用卡信息。然而，除非您特别选择离开免费许可证，否则不会收费。

您可以按照此页面上的说明 — [`azure.microsoft.com/en-gb/free/`](https://azure.microsoft.com/en-gb/free/) 通过浏览器创建您的免费 Azure 账户，但最简单的方法是点击活动栏中的新 Azure 图标，然后选择“创建免费 Azure 账户”（或者如果您已经有账户，则选择“登录 Azure”） -

![](img/71950cb1d3f6718a7cf4561c2010929e.png)

作者提供的图片

### 步骤 4.3：创建一个 Azure Web 应用程序

下一步是通过点击“APP SERVICE”窗口中的“+”符号来创建一个 Azure Web 应用程序以托管您的应用程序。系统会提示您输入应用程序的名称。这个名称将用于最终的 URL，并且必须是唯一的，但除此之外，名称并不是特别重要。

当系统提示您选择许可证类型时，选择“免费试用” — 您的 Web 应用程序现在将被创建，您可以开始部署。

![](img/c8c58d7388e1b88c86365cbe7f260215.png)

作者提供的图片

### 步骤 4.4 创建一个“requirements.txt”部署文件

在您可以将应用程序部署到 Azure 之前，您必须在与 Flask Web 应用程序相同的文件夹中创建一个“requirements.txt”文件，该文件包含 Azure 必须安装的所有依赖项和库的列表，以便运行您的应用程序。此步骤至关重要，因为如果库不在部署环境中，应用程序将崩溃。

我们应用程序的 `requirements.txt` 文件内容如下 -

![](img/ee9a13c2ca197fb9eb3b68c16a259110.png)

作者提供的图片

一些需要注意的点 -

1.  库名称必须与使用 pip 安装时输入的名称完全一致，例如 `pip install Flask`。

1.  请注意 `Flask` 的 “F” 是大写的。这是因为 Flask 采用了这种不寻常的大小写方式，通常库名都是小写的。

1.  `sklearn` 是执行重新加载的 `model.pkl` 所必需的。虽然代码中没有明确引用 sklearn 和 `DecisionTreeClassifier`，但它们是 `model.fit` 所需要的，因此如果省略 `sklearn`，应用程序将崩溃。

1.  `pickle` 的引用不是必需的，因为这个库是 Python 核心安装的一部分。如果你包含 `pickle`，部署将会崩溃，因为你不能执行 `pip install pickle`。

如果遵循这些规则，你的部署将会成功，任何错误信息通常足够详细，可以通过一些互联网搜索解决问题。

### 步骤 4.5 将你的应用程序部署到 Azure

如果你到目前为止一直在按照步骤操作，你现在应该在 VS Code 中有一个 Flask 应用程序。你的应用程序代码文件将叫做 `app.py`，应用程序名称为 `app`。Flask 应用程序已经在本地开发网络服务器上进行了测试。

你已安装 VS Code Azure App 扩展，并使用它创建了一个 Microsoft Azure 免费账户和一个 Azure 网络应用程序。

你应该在 VS Code 中打开你的 Flask 应用程序，并且你已准备好将应用程序部署到云端。

这通过简单地点击蓝色圆圈图标旁边的网页应用名称，然后点击 “+” 旁边的云图标来实现。

当被提示时，选择以下选项 -

+   选择默认文件夹进行部署

+   选择 “Free Trial” 订阅

+   选择你创建的网页应用名称

+   如果提示覆盖，选择 “Deploy”

+   当被问及 “Always deploy …” 时，选择 “Skip for now”

+   部署开始时点击 “output window”

现在坐下来喝杯咖啡，同时应用程序正在部署中 -

![](img/86b70d20bccef4ca382a22b42c5a429c.png)

作者提供的图片

部署完成后，点击 “Browse Website” 将带你到正确的 URL，该 URL 将运行 `app.route("/")` 函数。

只需添加我们用于测试本地部署的相同 URL 参数，你将看到完全部署的网页应用的输出！ -

[`graham-harrison68-web03.azurewebsites.net/drug?Age=60&Sex=F&BP=LOW&Cholesterol=HIGH&Na_to_K=20`](https://graham-harrison68-web03.azurewebsites.net/drug?Age=60&Sex=F&BP=LOW&Cholesterol=HIGH&Na_to_K=20)

![](img/055a1d456700ef8e3a689fca1b51a690.png)

作者提供的图片

一点需要注意的是：过一段时间，azure 应用会进入睡眠状态，第一次调用后需要很长时间。

如果你选择升级到付费的 Azure 订阅，有一个选项可以保持应用程序刷新和“唤醒”，但在免费订阅中，睡眠相关的延迟无法避免，因为此订阅仅用于测试目的，因此有一些限制。

## 步骤 5：构建一个客户端应用程序以使用 Azure 部署的网络服务

目前，任何能够发起网络请求的编程语言或环境都可以用几行代码调用部署的网络服务。

我们一开始确实说过可以使用非 Python 环境如 C#、JavaScript 等，但我将通过写一些代码来演示如何使用`ipywidgets`从 Python 客户端调用部署的应用程序——

![](img/adc1b12ee500aa695ebed31699d08f88.png)

图片来源：作者

如果你点击“Prescribe”并保持默认值，推荐的应该是“drugC”。

将年龄更改为 60，Na 更改为 K 为 20，应该开具“DrugY”。将年龄改回 47，Na 改回 K 为 14，并将 BP 更改为“HIGH”，应该开具 drugA。

这些简单的测试证明，使用决策树预测机器学习算法的 Azure 托管网络服务已经完全部署到公共云中，任何能够执行`http GET`命令的开发环境都可以调用，并且从端到端完全工作。

## 结论

这涉及到相当多的步骤，但通过使用现成的库和免费工具，包括 scikit-learn、pickle、flask、Microsoft Azure 和 ipywidgets，我们构建了一个功能完全的、公开可用的机器学习算法云部署，以及一个功能完备的客户端来调用和使用该网络服务并显示结果。

### 感谢阅读！

如果你喜欢阅读这篇文章，不妨查看我其他的文章：[`grahamharrison-86487.medium.com/`](https://grahamharrison-86487.medium.com/)

此外，我很乐意听取你对这篇文章、我其他的文章或任何与数据科学和数据分析相关的内容的看法。

如果你希望讨论这些主题，请在 LinkedIn 上查找我 —— [`www.linkedin.com/in/grahamharrison1`](https://www.linkedin.com/in/grahamharrison1) 或随时通过电子邮件联系我：GHarrison@lincolncollege.ac.uk。

如果你希望支持作者和全球数千名为文章撰写做出贡献的人，请使用这个链接订阅 —— [`grahamharrison-86487.medium.com/membership`](https://grahamharrison-86487.medium.com/membership)。注意：如果你使用此链接注册，作者将从费用中获得一部分。

**[格雷厄姆·哈里森](https://www.linkedin.com/in/grahamharrison1/?originalSubdomain=uk)** 是林肯学院集团的信息技术、信息管理及项目组的总监，负责英国、沙特阿拉伯和中国的数据和技术能力。格雷厄姆还是数据科学咨询公司 The Knowledge Ladder 的董事总经理和创始人，林肯郡网络安全论坛的委员会成员，以及大林肯郡和拉特兰分支的董事学会数字大使。格雷厄姆最近完成了关于数据科学民主化的学术研究，并热衷于为所有组织提供可访问、负担得起且灵活的数据科学服务，无论其规模、行业或以前的经验如何。详情请访问 [`www.theknowledgeladder.co.uk/`](https://www.theknowledgeladder.co.uk/) 和 [`www.youtube.com/watch?v=cFt03rny07Y`](https://www.youtube.com/watch?v=cFt03rny07Y)。

[原文](https://towardsdatascience.com/a-full-end-to-end-deployment-of-a-machine-learning-algorithm-into-a-live-production-environment-3d9971ade188)。转载已获许可。

### 更多相关话题

+   [初学者的端到端机器学习指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)

+   [理解 AI 中的代理环境](https://www.kdnuggets.com/2022/05/understanding-agent-environment-ai.html)

+   [5 个最佳端到端开源 MLOps 工具](https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools)

+   [使用 HuggingFace 实施的简单端到端项目](https://www.kdnuggets.com/a-simple-to-implement-end-to-end-project-with-huggingface)

+   [2024 年必须尝试的 7 个端到端 MLOps 平台](https://www.kdnuggets.com/7-end-to-end-mlops-platforms-you-must-try-in-2024)

+   [从数据收集到模型部署：数据科学项目的 6 个阶段…](https://www.kdnuggets.com/2023/01/data-collection-model-deployment-6-stages-data-science-project.html)
