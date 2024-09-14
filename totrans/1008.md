# 开发开放标准的分析跟踪

> 原文：[https://www.kdnuggets.com/2022/07/developing-open-standard-analytics-tracking.html](https://www.kdnuggets.com/2022/07/developing-open-standard-analytics-tracking.html)

在2021年初，我们的长期数据爱好者团队开始开发[**开放分析分类法**](https://github.com/objectiv/objectiv-analytics)。目标是提出一种通用的方式来构建分析数据，以便在一个数据集上建立的模型可以在另一个数据集上部署和运行。

# 简要历史

* * *

## 我们的前3名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

我们曾经的工作是为企业客户构建模型和运行深入的用户行为分析，这些数据集主要是通过流行的分析工具如谷歌分析、Mixpanel和Adobe分析收集的。

在为50多个客户服务的过程中，有一件事特别突出：大多数客户的分析目标非常相似，但他们的数据集看起来却各不相同。

他们都希望防止用户流失、提高参与度或转化率、个性化用户体验和预测行为，但每个内部团队都制定了自己的事件类型、命名约定和数据结构方式。因此，无法复用。所有的管道和模型都需要从头开始构建，花费了大量时间将数据整理成干净的、适合模型的状态。

在这一阶段，我们对开发开放的分析分类法产生了兴趣。如果目标如此相似，是否存在一种通用的方法来构建分析数据，以覆盖所有这些常见的分析用例？对于更具体的需求，这意味着什么？如果可能的话，它会是什么样的？

快进到今天。经过无数小时在白板上草拟想法、收集来自数据科学家和工程师的反馈以及在实际用例中进行实验，我们已经达到了一个自信的阶段，准备开始涉及更广泛的受众。

允许我展示一下我们认为它可能的样子，并解释一些设计选择。

# 开放分析分类法的结构

[开放分析分类法](https://github.com/objectiv/objectiv-analytics)是对常见事件类型及其可能发生的上下文的通用分类。它是层次化的，每种事件和上下文类型都有其自己的属性、要求和关系。

![开放分析分类法的结构](../Images/e5417cbcdbf32ca3a8ca1b5c17927be9.png)

## 事件

让我们更详细地查看其中一个事件类，**PressEvent**：

![PressEvent](../Images/2241e1a35045acba94b51af0602037a8.png)

**PressEvent**用于描述用户点击或触摸（与设备无关）UI中的元素。这是**InteractiveEvent**类的一个子类，**InteractiveEvent**是所有交互事件的父类。

事件被分为两个子类：交互事件和非交互事件。第一种类型由用户触发，第二种类型通常由应用程序触发。**NonInteractiveEvent**的一个例子是**MediaStartEvent**，它在音频或视频开始播放时触发：

![MediaStartEvent](../Images/b9e8c4e2dc23467bf29c7e893674d0f7.png)

## 上下文

你可能已经注意到，**事件**需要**上下文**。这些上下文提供了额外的信息，帮助你准确定位事件发生的地点和情况。

与事件类似，上下文被分为两个子类：**全局上下文**和**位置上下文**。

**全局上下文**包含一般信息。例子包括用户会话信息、应用信息或与事件相关的营销活动细节。

![ data-lazy-src=](../Images/5a77fefe77fbb5990b30d0115f8180cc.png)

其次，**位置上下文**包含有关事件在UI中发生*位置*的信息。例如，前面提到的**MediaStartEvent**需要一个**MediaPlayerContext**来准确指出事件的来源。在这种情况下，特指媒体播放器，因为它的事件类。

![MediaStartEvent](../Images/4b7a519605a55276b6b2059d3923438c.png)

# 用例与设计选择

让我们看看如何使用它以及我们为何做出某些设计选择。

## 数据验证

严格定义事件和上下文类型的类意味着你可以在非常早的阶段验证数据。例如，如果**MediaPlayEvent**缺少**MediaPlayerContext**，你可以抛出错误并选择不存储它（或者更好的是：将其存储在其他地方）。这将允许你防止收集不完整或有缺陷的数据，从而节省大量分析阶段的准备时间。

## 仪器验证

当前端开发者正在设置追踪器以收集数据时，同样的原则适用。你可以根据开放分类法验证追踪仪器，以确保其配置能够按预期捕捉事件，并在条件未满足时抛出错误。

例如，我们自己的追踪SDK通过在浏览器控制台运行时抛出错误来实现这一点，并指向文档以展示如何修复：

![Instrumentation validation](../Images/e2063f29889106f2f0640d2d4d340ba0.png)

我们还利用它通过 TypeScript 定义在 IDE 中提供内联文档和验证问题的 linting：

![在你的 IDE 中通过 TypeScript 定义进行的验证问题](../Images/ca2ce5be9c0c49a2bb57ef328b732586.png)

## 特征选择

在分析阶段，数据中的层次结构和严格类型化对特征选择非常有用。子类和超类使你能够快速选择所需类型和级别的事件，而无需先手动映射它们。

想要所有交互式事件？你可以在笔记本中用一个命令来实现：

```py
interactive_events = df[df.stack_event_types.json.array_contains('InteractiveEvent')]
```

仅获取 PressEvents 更加简单：

```py
press_events = df[(df.event_type=='PressEvent')]
```

这在与**位置上下文**结合使用时尤其强大。让我演示一下我们如何利用它们来基于 UI 的层次结构实现快速特征选择。

我们要求仪器工程师通过标记 UI 的逻辑部分来丰富跟踪仪器，标记为**位置上下文**。我们的跟踪器随后为每个事件生成一个**位置栈**。它在**位置上下文**的层次结构中捕获事件触发的确切位置。

```py
{

       "_type": "RootLocationContext",

       "id": "modeling"

   },

   {

       "_type": "NavigationContext",

       "id": "docs-sidebar"

   },

   {

       "_type": "ExpandableContext",

       "id": "open-model-hub"

   },

   {

       "_type": "ExpandableContext",

       "id": "models"

   },

   {

       "_type": "ExpandableContext",

       "id": "helper-functions"

   },

   {

       "_type": "LinkContext",

       "id": "is_new_user",

       "href": "/docs/modeling/open-model-hub/models/helper-functions/is_new_user/"

   }
```

*事件位置栈的一个示例，捕获自我们文档的建模部分*![Objective.io docs](../Images/b31b7bf19675503991ffc75fe06f1459.png)

上述事件是在用户点击侧边栏中的 is_new_user 标签时捕获的

随着你的数据集现在包含了你产品的逻辑结构，你可以用它在非常细粒度的层面上切分事件。

例如，你可以轻松选择所有带有 NavigationContext 的事件：

![NavigationContext](../Images/6ae947f7527c424985ba99d6e086fbf4.png)

或者更具体地说，那些发生在文档侧边栏上的事件：

![docs sidebar](../Images/5ff461aa60caf3ea3ad484e3bb327c51.png)

…等等。

**重点：重用模型和工具**

对我们来说，这就是一切的起点。拥有一种严格的、通用的结构化分析数据的方法，将意味着所有数据集都变得一致和通用，从而使得基于一个数据集构建的模型可以部署和运行在另一个数据集上。

你为 Android 应用开发的用户细分模型？你可以与负责 iOS 应用或 Web 应用的团队分享，他们可以在不做任何更改的情况下进行部署和运行。

![它们可以在任何接受开放分析分类法的数据集上运行](../Images/659c137a4aeadea3bcfc7ac0f9f0443c.png)

我们的一个[示例笔记本](https://objectiv.io/docs/modeling/example-notebooks/)的摘录。它们可以在任何接受开放分析分类法的数据集上运行。

但我们认为这远不止于此。每天，其他公司中的某个人都在以略微不同的方式解决完全相同的问题。我们希望这能够结束，并通过协作的努力推动数据领域的发展：使你能够利用他人已经构建的东西，改进它，部署它，如果可能的话，将其交还给其他人以便他们也可以做到这一点。

目前还处于初期阶段，我们还有很长的路要走，但基础已经建立，我们看到采用速度迅速提升。

如果你想了解更多或参与其中，请查看 [开放分析分类文档](https://objectiv.io/docs/taxonomy/) 和 [Objectiv 的 Github 仓库](https://github.com/objectiv/objectiv-analytics)。如果你有问题或想法，也可以 [加入 Objectiv 的 Slack 频道](https://objectiv.io/join-slack)。

**[伊瓦尔·普鲁因](https://www.linkedin.com/in/ivarpruijn/)** 是 Objectiv 的联合创始人兼首席产品官，该公司提供开源产品分析基础设施。他拥有特温特大学的电信学硕士学位。他的大部分职业生涯都在科技初创公司和成长型企业中度过，他特别关注推动数据云的未来。

### 更多相关内容

+   [机器学习实验的版本控制与跟踪](https://www.kdnuggets.com/2021/12/versioning-machine-learning-experiments-tracking.html)

+   [7 个最佳机器学习实验跟踪工具](https://www.kdnuggets.com/2023/02/7-best-tools-machine-learning-experiment-tracking.html)

+   [在 Python 中使用标准差去除异常值](https://www.kdnuggets.com/2017/02/removing-outliers-standard-deviation-python.html)

+   [关于开发安全、可靠和可信的 AI 框架的专家见解](https://www.kdnuggets.com/expert-insights-on-developing-safe-secure-and-trustworthy-ai-frameworks)

+   [一个社区在为客户数据建模开发 Hugging Face](https://www.kdnuggets.com/2022/08/objectiv-community-developing-hugging-face-customer-data-modeling.html)

+   [开放助手：探索开放和协作的可能性](https://www.kdnuggets.com/2023/04/open-assistant-explore-possibilities-open-collaborative-chatbot-development.html)
