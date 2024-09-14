# 2020 年我作为数据科学家学习的 8 种新工具

> 原文：[`www.kdnuggets.com/2021/01/8-new-tools-learned-data-scientist-2020.html`](https://www.kdnuggets.com/2021/01/8-new-tools-learned-data-scientist-2020.html)

评论

**作者：[Ben Weber](https://www.linkedin.com/in/ben-weber-3b87482/)，Zynga 的杰出数据科学家**

![图](img/45ecc910201f8928de47528d7212dafc.png)

[来源](https://pixy.org/5939802/)

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

尽管 2020 年是一个充满挑战的年份，我还是利用远程工作的过渡期探索了新的工具，以扩展我的数据科学技能。这一年我从数据科学家转型为应用科学家，不仅负责原型设计数据产品，还要将这些系统投入生产并监控系统健康。我之前有使用 Docker 进行应用程序容器化的经验，但没有经验将容器部署为可扩展、负载均衡的应用程序。虽然我在 2020 年学到的许多技术更常与工程相关，而不是数据科学，但学习这些工具对于构建端到端的数据产品是有帮助的。这对于在初创公司工作的数据科学家尤为重要。以下是我在 2020 年学到的技术：

1.  MLflow

1.  Kubernetes

1.  NoSQL

1.  OpenRTB

1.  Java Web 框架

1.  HTTPS

1.  负载均衡

1.  日志记录

我将在下面详细介绍这些主题。使用这些不同工具的主要动机是为了建立一个程序化广告的研究平台。我负责构建和维护一个实时数据产品，需要探索新的工具来完成这个项目。

### MLflow

[MLflow](https://mlflow.org/) 是一个开源框架，用于模型生命周期管理。项目的目标是提供支持机器学习模型开发、服务和监控的模块。我在 2020 年开始使用其中的两个组件：MLflow 跟踪和模型注册表。跟踪模块使数据科学家能够记录不同模型管道的性能并可视化结果。例如，可以尝试不同的特征缩放方法、回归模型和超参数组合，并查看哪个管道配置产生了最佳结果。我在 Databricks 环境中使用了这个功能，它提供了有用的模型选择可视化工具。我还开始使用 MLflow 中的注册表模块来存储模型，其中一个训练笔记本训练并存储模型，而一个模型应用笔记本检索和应用模型。模型注册表中的一个有用功能是能够在部署之前对模型进行分阶段处理。注册表可以维护不同的模型版本，并提供在发现问题时回退到先前版本的能力。在 2021 年，我计划探索 MLFlow 中的更多模块，包括模型服务。

### Kubernetes

Kubernetes 是一个开源的容器编排平台。它使数据科学家能够将容器部署为可扩展的 Web 应用程序，并提供多种配置选项用于在 Web 上暴露服务。虽然从头开始设置 Kubernetes 部署可能相当复杂，但云平台提供了托管版本的 Kubernetes，使得上手这个平台变得容易。我对希望学习 Kubernetes 的数据科学家的建议是使用 Google Kubernetes Engine (GKE)，因为它提供了快速的集群启动时间，并具有出色的开发者体验。

为什么 Kubernetes 如此有用？因为它使团队能够将应用程序开发和应用程序部署问题分开。数据科学家可以构建一个模型服务容器，然后将其交给工程团队，后者将该服务暴露为可扩展的 Web 应用程序。在 GCP 中，它还与负载均衡和网络安全系统无缝集成。然而，使用托管服务如 GKE，可以降低使用 Kubernetes 的门槛，数据科学家应当亲自体验这个平台。这样可以使数据科学家能够构建端到端的数据产品。

### NoSQL

尽管我在数据科学职业生涯中使用了各种数据库，但直到 2020 年我才首次探索 NoSQL 数据库。NoSQL 包括实现低延迟操作的键值存储数据库。例如，Redis 是一个内存数据库，提供亚毫秒级的读取性能。这种性能在构建实时系统时非常有用，你需要在数据通过网络服务接收时更新用户档案。例如，你可能需要更新描述用户活动的特征向量的属性，该特征向量作为输入传递给流失模型，并在 HTTP POST 命令的上下文中应用。为了构建实时系统，数据科学家需要亲自使用 NoSQL 数据库。学习诸如 Redis 这样的技术时，使用[mock](https://github.com/fppt/jedis-mock)库来测试 API 在部署到云端之前是很有帮助的。

### OpenRTB

OpenRTB 是一种用于实时广告拍卖和广告投放的规范。该规范被用于如 Google 广告交换等交易所，以连接销售广告库存的出版商与希望投放广告的买家。我使用该协议实现了一个程序化用户获取的研究平台。虽然该规范对数据科学的广泛适用性不强，但对数据科学家学习如何构建能够实现标准化接口的系统非常有用。以 OpenRTB 为例，这涉及到构建一个接收带有 JSON 有效负载的 HTTP POST 请求并返回带有定价细节的 JSON 响应的网络服务。如果你有兴趣快速上手 OpenRTB 规范，Google 提供了一个[protobuf](https://github.com/google/openrtb)实现。

### Java Web Frameworks

我决定用 Java 编写 OpenRTB 研究平台，因为我对这种语言最为熟悉。然而，Rust 和 Go 都是构建 OpenRTB 系统的绝佳替代选择。由于我选择了 Java，我需要选择一个用于实现应用程序端点的 Web 框架。虽然我在十多年前使用 Jetty 库来构建简单的 Java Web 应用程序，但我决定根据基准测试探索新的工具。我从[Rapidoid](https://www.rapidoid.org/)库开始，它是一个用于用 Java 构建 Web 应用程序的轻量级和快速框架。然而，当我开始在响应 Web 请求时添加对 Redis 的调用时，我发现需要将 Rapidoid 从非托管模式转换为托管模式。我接着尝试了[Undertow](https://github.com/undertow-io/undertow)，它支持阻塞 IO，并发现它在我的基准测试中表现优于 Rapidoid。虽然数据科学家通常不使用 Java 编写代码，但学习如何尝试不同的 Web 框架，例如选择 gunicorn 和 uWSGI 来部署 Python Web 服务，是很有用的。

### **HTTPS**

实施 OpenRTB 协议现在需要通过安全的 HTTP 服务流量。为 Web 服务启用 HTTPS 涉及通过 DNS 将 Web 服务设置为命名端点，并使用签名证书来建立端点的身份。确保 GCP 上托管的 GKE 中的端点安全相对简单。一旦服务通过节点端口和服务入口暴露出来，你需要为服务的 IP 地址设置一个 DNS 条目，然后使用 GCP 托管的证书来启用 HTTPS。

对于数据科学家来说，学习设置 HTTPS 端点非常有用，因为这涉及到保护服务的细微差别。如果不需要端到端 HTTPS，比如在 OpenRTB 的情况下，其中 HTTP 可以在负载均衡器和 Kubernetes 集群中的 pods 之间内部使用，那么部署就会更容易。如果需要端到端 HTTPS，比如使用 OAuth 的 Web 服务，那么 Kubernetes 配置会稍微复杂一些，因为 pods 可能需要在与服务 Web 请求的端口不同的端口上响应健康检查。我最后提交了一个 [PR](https://github.com/lchapo/dash-google-auth/pull/15) 来解决与此相关的 Plotly Dash 应用程序中的一个问题。

### 负载均衡

为了扩展到 OpenRTB 量级的 Web 流量，我需要使用负载均衡来处理每秒超过 10 万个 Web 请求 (QPS)。Kubernetes 提供了扩展服务 Web 请求的 pods 数量的基础设施，但还需要以一种均匀分配请求的方式配置集群。Kubernetes 有一个 [open issue](https://learnk8s.io/kubernetes-long-lived-connections) 导致使用长连接时 pods 负载不均，这是 OpenRTB 系统推荐的配置。我使用了 [container native](https://cloud.google.com/kubernetes-engine/docs/how-to/container-native-load-balancing) 的负载均衡功能来缓解这个问题。对负载均衡进行实际操作在大型组织中对数据科学家来说并不常见，但对于拥有高请求量的端到端数据产品的初创公司或团队来说，这是一个有用的技能。

### 日志记录

部署 Web 应用程序还涉及到为系统设置监控，以确定是否出现任何问题。在构建 GCP 应用程序时，StackDriver 提供了一个用于日志消息、报告自定义指标和设置警报的托管系统。我能够利用这个系统来监控正常运行时间，并在发生事件时向 Slack 和 SMS 发送警报。对数据科学家来说，实际操作日志库非常有用，以确保部署到云中的系统按预期运行。

### 结论

在 2020 年，我学习了几种通常与工程角色相关的技术。作为一名数据科学家，我出于必要学习了这些工具，以建立和维护端到端系统。虽然许多这些技术在数据科学中并不广泛适用，但应用科学家的角色日益增长，正在对拥有更广泛技术栈经验的数据科学家产生需求。

**简介： [本·韦伯](https://www.linkedin.com/in/ben-weber-3b87482/)** 是 Zynga 的杰出数据科学家，也是《[数据科学生产](https://www.amazon.com/gp/product/B083H2YWP4)》一书的作者。

[原文](https://towardsdatascience.com/8-new-tools-i-learned-as-a-data-scientist-in-2020-6dea2f847c32)。转载已获许可。

**相关：**

+   MLOps 正在改变机器学习模型的开发方式

+   轻松数据科学的 5 种工具

+   每位懒散的全栈数据科学家应该使用的 5 种最有用的机器学习工具

### 更多相关话题

+   [我从使用 ChatGPT 进行数据科学中学到了什么](https://www.kdnuggets.com/what-i-learned-from-using-chatgpt-for-data-science)

+   [KDnuggets 新闻，5 月 25 日：每个数据科学家应该了解的 6 种 Python 机器学习工具…](https://www.kdnuggets.com/2022/n21.html)

+   [每位数据科学家都应该了解的 6 种 Python 机器学习工具](https://www.kdnuggets.com/2022/05/6-python-machine-learning-tools-every-data-scientist-know.html)

+   [每位数据科学家应该了解的工具：实用指南](https://www.kdnuggets.com/tools-every-data-scientist-should-know-a-practical-guide)

+   [KDnuggets™新闻 22:n01，1 月 5 日：跟踪和可视化的 3 种工具…](https://www.kdnuggets.com/2022/n01.html)

+   [2024 年每位数据科学家工具箱中必备的 5 种工具](https://www.kdnuggets.com/5-tools-every-data-scientist-needs-in-their-toolbox-in-2024)
