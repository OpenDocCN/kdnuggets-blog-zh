# Kedro-Airflow：用 Airflow 协调 Kedro 管道

> 原文：[https://www.kdnuggets.com/2021/03/kedro-airflow-orchestrating-pipelines.html](https://www.kdnuggets.com/2021/03/kedro-airflow-orchestrating-pipelines.html)

[评论](#comments)

**由 [Jo Stichbury](https://www.linkedin.com/in/jostichbury/)、技术作家及 [Yetunde Dada](https://www.linkedin.com/in/yetudada/)、QuantumBlack 产品经理撰写**

[Kedro](https://github.com/quantumblacklabs/kedro) 是一个 [开源 Python 框架](https://kedro.readthedocs.io/en/stable/)，用于创建可重复、可维护和模块化的数据科学代码。其重点在于编写代码，而非协调、调度和监控管道运行。我们强调基础设施独立性，这对像 QuantumBlack 这样的咨询公司至关重要，Kedro 就是在这样的环境中诞生的。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

![图片](../Images/e0ed876d1b2801def9b0e141ee177d22.png)

> Kedro 不是一个协调工具。它旨在保持非常精简，并对管道运行的地点和方式不做任何意见。

只要你能运行 Python，你就可以几乎在任何地方以最小的努力部署 Kedro 项目。我们的用户可以自由选择他们的部署目标。未来部署 Kedro 管道的关键在于设计一个优秀的开发体验的部署过程。

作为开源社区的一员，我们可以探索与其他志同道合的框架和技术的合作伙伴关系，这是一个好处。我们特别兴奋与 Astronomer 团队合作，他们帮助组织采用 Apache Airflow——领先的开源数据工作流协调平台。

Airflow 中的工作流被建模并组织为[DAGs](https://en.wikipedia.org/wiki/Directed_acyclic_graph)，使其成为协调和执行使用 Kedro 编写的管道的合适引擎。为了保持工作流的无缝，我们很高兴推出最新版本的[Kedro-Airflow 插件](https://github.com/quantumblacklabs/kedro-airflow)，它简化了在 Airflow 上部署 Kedro 项目的过程。

我们与 Astronomer 的合作为用户提供了一种简单的方式来部署他们的管道。我们希望继续我们的工作，使过程更加顺畅，最终实现 Kedro 管道在 Airflow 上的“一键部署”工作流。

**我们已对对话进行了编辑，以便于简洁和清晰。**

### Pete DeJoy，你是Astronomer的产品经理。请简要介绍一下你自己！

![帖子图片](../Images/4dcafef1338f0d1e2ad883dc5878cf95.png)

我是Astronomer的创始团队成员之一，我们围绕开源编排框架Apache Airflow建立了公司。在这里我做了很多事情，但大部分精力都投入到我们的产品中，这个产品从白板上的一个想法发展成了一个支持数千用户的高规模系统。

### 是什么促使了Airflow 2.0的创建？这个版本的Airflow成功的标准是什么？

自2014年起，Airflow经历了相当大的发展；它现在在Github上拥有超过20,000个星标，每月下载量达到60万次，并且在全球拥有数万名用户。Airflow 1.x为开发者解决了很多一阶问题，但随着Airflow的广泛采用，企业需求的增加也随之而来，同时对提高开发者体验的压力也增大。Airflow 2.0满足了用户的需求，带来了许多备受期待的功能。这些功能包括：

+   高可用性、水平可扩展的调度器

+   升级版的稳定REST API

+   解耦的工作流集成（在Airflow中称为“提供者”）作为独立版本和维护的python包以及[更多内容](https://www.astronomer.io/blog/introducing-airflow-2-0)

我们将2.0视为项目的一个重要里程碑；它不仅显著提高了Airflow的可扩展性，还为我们持续构建新功能奠定了基础。

### 你是怎么了解到Kedro的？你什么时候意识到它与Airflow兼容的？

我曾与一些使用Kedro编写数据管道的科学家交谈，他们正在寻找一种好的方式将这些管道部署到Airflow中。Kedro在帮助数据科学家将良好的软件工程原则应用到他们的代码中并使其模块化方面做得非常出色，但Kedro管道需要一个单独的调度和执行环境来大规模运行。鉴于这一需求，Kedro管道与Airflow之间自然形成了联系：我们希望尽一切可能在这两种工具的交集处构建出色的开发者体验。

### 你认为Kedro-Airflow的未来发展会怎样？

Airflow 2.0扩展和升级了Airflow REST API，使其在未来几年中更加稳健。随着API的发展，将会有新的机会为特定的抽象层提供支持，以帮助DAG编写和部署，进而形成更丰富的插件生态系统。还将有更多机会将`kedro-airflow`包与Airflow API集成，以实现卓越的开发者体验。

### Airflow的未来是什么样的？

展望Airflow 3.0及未来，建立在开发者的喜爱和信任之上是不可避免的。但这还不会止步于此。随着数据编排对越来越多的业务单元变得至关重要，我们希望Airflow成为使数据工程更易接近的媒介。我们寻求民主化访问，以便产品负责人和数据科学家都可以利用Airflow的分布式执行和调度能力，而无需成为Python或Kubernetes的专家。在这个过程中，使用户能够从他们选择的框架中编写和部署数据管道将变得越来越重要。

### 工作流编排技术的未来是什么？

Airflow的诞生启动了一个“数据管道即代码”的运动，这改变了企业对工作流编排的思考方式。多年里，作业调度由传统的拖放框架和复杂的cron作业网络组合处理。随着我们过渡到“大数据”时代，公司开始建立专门的团队来操作其孤立的数据，额外的灵活性、控制和治理的需求变得显而易见。

当**Maxime Beauchemin**和Airbnb团队[构建并开源了Airflow](https://medium.com/airbnb-engineering/airflow-a-workflow-management-platform-46318b977fd8)，将灵活且编码化的数据管道作为一项首要功能时，他们将代码驱动的编排推向了聚光灯下。Airflow解决了许多数据工程师面临的首要问题，这解释了它的爆炸式采用。但随着早期的采用，也出现了一些陷阱；由于Airflow设计上高度可配置，用户开始将其应用于不一定为之设计的用例。这对项目施加了进化压力，推动社区增加额外的配置选项，以“塑造”Airflow以适应各种用例。

尽管增加的配置选项帮助Airflow扩展以适应这些额外的用例，但它们也引入了一类新的用户需求。数据平台拥有者和管理员现在需要一种方法来向他们的管道作者提供标准模式，以减少业务风险。同样，管道作者需要额外的保护措施，以确保他们不会“错误地使用Airflow”。最后，具有Python背景的工程师现在需要学习如何为大规模稳定可靠的编排操作大数据基础设施。

我们看到未来的工作流编排技术将适应这些用户需求的类别变化。如果到目前为止的旅程是“[数据工程师的崛起](https://www.freecodecamp.org/news/the-rise-of-the-data-engineer-91be18f1e603/)”，那么我们看到的未来是“数据工程的民主化”。所有用户——从数据科学家到数据平台所有者——都将能够访问强大、分布式、灵活的数据管道编排。他们将受益于与他们熟悉并喜爱的创作工具的集成，同时拥有特定使用模式的保护措施，以防止人们偏离理想路径。

*您可以在*[*Kedro 文档*](https://kedro.readthedocs.io/en/stable/10_deployment/11_airflow_astronomer.html)*中了解有关 Kedro-Airflow 插件的更多信息，并查看*[*GitHub 仓库*](https://github.com/quantumblacklabs/kedro-airflow)*。本文由*[*Jo Stichbury*](https://www.linkedin.com/in/jostichbury/)*— 技术写作人 和*[*Yetunde Dada*](https://www.linkedin.com/in/yetudada/)*— 产品经理编辑，*[*Ivan Danov*](https://www.linkedin.com/in/idanov/)*（Kedro 技术负责人）和*[*Lim Hoang*](https://www.linkedin.com/in/limhn/)*（Kedro 高级软件工程师）提供意见。*

[原文](https://medium.com/quantumblack/kedro-airflow-0-4-0-orchestrating-kedro-pipelines-with-airflow-23fb1283f24d)。经授权转载。

**相关内容：**

+   [在 Scikit-Learn 中使用管道简化混合特征类型预处理](/2020/06/simplifying-mixed-feature-type-preprocessing-scikit-learn-pipelines.html)

+   [5 步指南：使用 d6tflow 构建可扩展的深度学习管道](/2019/09/5-step-guide-scalable-deep-learning-pipelines-d6tflow.html)

+   [端到端机器学习平台概览](/2020/07/tour-end-to-end-machine-learning-platforms.html)

### 更多相关话题

+   [5 种数据编排的 Airflow 替代方案](https://www.kdnuggets.com/5-airflow-alternatives-for-data-orchestration)

+   [使用 HuggingFace 管道和 Streamlit 回答问题](https://www.kdnuggets.com/2021/10/simple-question-answering-web-app-hugging-face-pipelines.html)

+   [超越管道：将图形作为 Scikit-Learn 元估计器](https://www.kdnuggets.com/2022/09/graphs-scikitlearn-metaestimators.html)

+   [使用 HuggingFace Transformers 进行简单 NLP 管道](https://www.kdnuggets.com/2023/02/simple-nlp-pipelines-huggingface-transformers.html)

+   [通过特征/训练/推断管道统一批处理和 ML 系统](https://www.kdnuggets.com/2023/09/hopsworks-unify-batch-ml-systems-feature-training-inference-pipelines)

+   [使用 Pandas 构建数据科学管道](https://www.kdnuggets.com/building-data-science-pipelines-using-pandas)
