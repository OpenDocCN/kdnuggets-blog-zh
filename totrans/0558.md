# 不受欢迎的观点——数据科学家应该更多地从端到端

> 原文：[https://www.kdnuggets.com/2020/09/data-scientists-should-be-more-end-to-end.html](https://www.kdnuggets.com/2020/09/data-scientists-should-be-more-end-to-end.html)

[评论](#comments)

**由[尤金·闫](https://www.linkedin.com/in/eugeneyan/)，亚马逊应用科学专家，作家和演讲者**。

![](../Images/ae8a45ca81b835c9ae0c2d81d3a47aad.png)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在IT领域支持你的组织

* * *

最近，我看到了一条关于数据科学和机器学习中不同角色的[Reddit讨论线程](https://www.reddit.com/r/datascience/comments/i48b5q/for_those_that_work_for_a_team_that_has_both_data/): 数据科学家、决策科学家、产品数据科学家、数据工程师、机器学习工程师、机器学习工具工程师、AI架构师等。

我发现这让人感到*担忧*。当数据科学过程（问题框定、数据工程、机器学习、部署/维护）分散在不同的人手中时，很难做到高效。这会导致协调开销、责任分散以及缺乏全局视角。

在我看来，**我相信数据科学家通过从端到端的方式可以更有效**。在这里，我将讨论[好处](https://eugeneyan.com/writing/end-to-end-data-science/#from-start-identify-the-problem-to-finish-solve-it)和[反对观点](https://eugeneyan.com/writing/end-to-end-data-science/#but-we-need-specialist-experts-too)，[如何](https://eugeneyan.com/writing/end-to-end-data-science/#the-best-way-to-pick-it-up-is-via-learning-by-doing)成为端到端，以及[Stitch Fix和Netflix](https://eugeneyan.com/writing/end-to-end-data-science/#end-to-end-in-stitch-fix-and-netflix)的经验。

### 从开始（确定问题）到结束（解决问题）

你可能见过类似的*标签*和定义，例如：

+   [通才](https://towardsdatascience.com/why-you-shouldnt-be-a-data-science-generalist-f69ea37cdd2c)：专注于角色（[产品经理](https://en.wikipedia.org/wiki/Product_manager)，[业务分析师](https://en.wikipedia.org/wiki/Business_analyst)，[数据工程师](https://www.oreilly.com/content/data-engineering-a-quick-and-simple-definition/)，[数据科学家](https://en.wikipedia.org/wiki/Category:Data_scientists)，[机器学习工程师](https://www.quora.com/What-exactly-does-a-machine-learning-engineer-do)）；有些负面的含义

+   [全栈](https://skillcrush.com/blog/front-end-back-end-full-stack/)：专注于技术（Spark、Torch、Docker）；由全栈开发者普及

+   [独角兽](https://www.infoworld.com/article/3429185/stop-searching-for-that-data-science-unicorn.html)：专注于神话；被认为不存在

我发现这些定义比我更喜欢的要具备更强的规定性。相反，我有一个简单（且务实）的定义：端到端数据科学家能够**识别并解决数据问题以提供价值**。为了实现这一目标，他们将佩戴所需的（或较少的）多种帽子。他们还会学习和应用有效的技术、方法和过程。在整个过程中，他们会提出以下问题：

+   问题是什么？为什么重要？

+   我们能解决这个问题吗？我们应该如何解决？

+   估计值是多少？实际值是多少？

> ***数据科学流程***
> 
> 另一种定义端到端数据科学的方法是通过流程。这些流程通常很复杂，我在主要讨论中省略了它们。尽管如此，这里有一些以供你参考：
> 
> +   [CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)：跨行业数据挖掘标准过程（1997）。
> +   
> +   [KDD](https://en.wikipedia.org/wiki/Data_mining#Process)：数据库中的知识发现。
> +   
> +   [TDSP](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview)：团队数据科学过程，由微软在2018年提出。
> +   
> +   [DSLP](https://github.com/dslp/dslp)：数据科学生命周期过程。
> +   
> 如果这些流程看起来繁重且令人不知所措，不用担心。你不必完全采纳它们——可以逐步开始，保留有效的部分，并调整其余部分。

.

### 更多的背景信息，更快的迭代，更大的满意度

对于大多数数据科学角色来说，更多的端到端经验提升了你产生有意义影响的能力。（尽管如此，还有一些[角色](https://nvidia.wd5.myworkdayjobs.com/en-US/NVIDIAExternalCareerSite/job/US-CA-Santa-Clara/Senior-Deep-Learning-Data-Scientist--RAPIDS---AI_JR1929838)专注于机器学习。）

**端到端工作提供了更高的背景信息。** 虽然专业化角色可以提高效率，但它减少了背景信息（对于数据科学家而言），并导致次优的解决方案。

> 忘记整体图景的诀窍是近距离观察一切。——查克·帕拉纽克

在没有上游问题的完整背景信息的情况下，很难设计出整体解决方案。假设转换率下降，项目经理提出了改进搜索算法的请求。然而，最初导致下降的原因是什么？可能有多种原因：

+   产品：是否存在欺诈/质量差的产品减少了客户信任？

+   数据管道：数据质量是否受到影响，或者是否存在延迟/中断？

+   模型刷新：模型是否没有定期/正确刷新？

更常见的是，问题和解决方案往往在*机器学习之外*。改进*算法*的解决方案会忽略根本原因。

同样，在不了解下游工程和产品约束的情况下开发解决方案是有风险的。没有意义：

+   如果基础设施和工程师无法支持，构建近实时推荐器

+   如果无限滚动推荐器不适合我们的产品和应用

通过端到端的工作，数据科学家将拥有完整的背景，能够识别正确的问题并开发可用的解决方案。这也可能导致一些专业人员因其狭窄的背景而可能忽视的创新想法。总体而言，这提高了交付价值的能力。

**沟通和协调开销减少。** 拥有多个角色会带来额外的开销。让我们来看一个例子：数据工程师（DE）清理数据和创建特征，数据科学家（DS）分析数据和训练模型，机器学习工程师（MLE）进行部署和维护。

> 一位程序员在一个月内能完成的工作，两位程序员在两个月内也能完成。——弗雷德里克·P·布鲁克斯

数据工程师（DE）和数据科学家（DS）需要就哪些数据是（和不是）可用的、如何清理数据（例如，异常值、标准化）以及应该创建哪些特征进行*沟通*。类似地，数据科学家（DS）和机器学习工程师（MLE）必须讨论如何部署、监控和维护模型，以及模型的更新频率。当出现问题时，我们需要三个人在场（可能还有项目经理PM），以确定根本原因和解决步骤。

这也会导致额外的协调工作，因为在执行和按顺序传递工作时，时间表需要保持一致。如果数据科学家（DS）想要尝试额外的数据和功能，我们需要等待数据工程师（DE）处理数据并创建功能。如果一个新的模型准备好进行A/B测试，我们还需要等待机器学习工程师（MLE）将其（转换为生产代码）并部署。

尽管实际开发工作可能需要几天，但来回的沟通和协调可能需要几周，甚至更长时间。通过端到端的数据科学家，我们可以最大限度地减少这些额外开销，并防止技术细节在传递过程中丢失。

（但是，端到端的数据科学家真的能做到这一切吗？我认为可以。虽然数据科学家在某些任务上可能不如数据工程师（DE）或机器学习工程师（MLE）熟练，但他们能够有效地执行大多数任务。如果他们需要帮助来扩展或强化，他们总是可以寻求专业数据工程师（DE）和机器学习工程师（MLE）的帮助。）

> ***沟通和协调的成本***
> 
> 哈佛心理学家理查德·哈克曼（Richard Hackman）显示，一个团队中的关系数量是*N(N-1) / 2*，其中*N*是人数。这导致链接的指数增长，其中：
> 
> +   一支7人的初创团队有21个链接需要维护
> +   
> +   一组21人（即三支初创团队）拥有210个链接
> +   
> +   一组63人拥有近2000个链接。
> +   
> 在我们简单的例子中，只有三种角色（即六个链接）。但随着PM、BA和其他成员的加入，沟通和协调成本呈现出超过线性的增长。因此，虽然每增加一名成员会提高团队整体生产力，但增加的开销意味着生产力以递减的速度增长。（亚马逊的[two-pizza teams](https://buffer.com/resources/small-teams-why-startups-often-win-against-google-and-facebook-the-science-behind-why-smaller-teams-get-more-done/)是可能的解决方案。）

**迭代和学习的速度提高。** 在更大的背景和更少的开销下，我们现在可以更快地迭代、失败（即学习）并交付价值。

这对开发数据和算法产品尤为重要。与软件工程（一个更为成熟的领域）不同，我们不能在开始构建之前完成所有的学习和设计——我们的蓝图、架构和设计模式还不够完善。因此，快速迭代对于设计-构建-学习的循环至关重要。

**责任感和担当意识更强。** 数据科学过程分布在多人之间可能导致责任的扩散，更糟糕的是，可能会出现“社会懒散”现象。

一个常见的反模式是“[抛砖引玉](https://wiki.c2.com/?ThrownOverTheWall)”。例如，DE创建特征并将数据库表抛给DS，DS训练模型并将R代码抛给MLE，MLE将其转换为Java进行生产。

如果出现翻译失误或结果意外，谁负责？有了强烈的责任文化，每个人都会在各自的角色中挺身而出。但没有这种文化，工作可能退化为掩盖责任和指责他人，同时问题仍然存在，客户和业务也会受到影响。

让全程数据科学家对整个过程负责可以缓解这种问题。他们应该被赋予从头到尾的行动权限，从客户问题和输入（即原始数据）到输出（即部署的模型）和可衡量的结果。

> ***责任扩散与社会懒散***
> 
> [责任扩散](https://en.wikipedia.org/wiki/Diffusion_of_responsibility): 当有其他人在场时，我们不太可能承担责任和采取行动。如果我们知道还有其他人也在关注情况，个人就会感到责任感和紧迫感减少。
> 
> 这种现象的一种形式是[Bystander effect](https://en.wikipedia.org/wiki/Diffusion_of_responsibility#Bystander_effect)，其中[Kitty Genovese](https://en.wikipedia.org/wiki/Murder_of_Kitty_Genovese)在她居住的街对面公寓楼外被刺伤。虽然有38名目击者看见或听见了攻击，但没有人报警或提供帮助。
> 
> [社会懈怠](https://en.wikipedia.org/wiki/Social_loafing)：我们在团队中工作时的努力比单独工作时少。19世纪90年代，Ringelmann让人们在单独和团队中拉绳子。他测量了他们拉的用力程度，发现团队成员在拉绳子时的努力程度通常低于单独工作的个体。

**对于（一些）数据科学家来说，这可以带来更高的动力和工作满意度，** 这与[自主性、精通和目的](https://www.clearpointstrategy.com/how-employees-are-motivated-autonomy-mastery-purpose/)密切相关。

+   **自主性：** 通过能够独立解决问题。端到端的数据科学家能够识别和定义问题，构建自己的数据管道，并部署和验证解决方案，而不是等待和依赖他人。

+   **精通：** 从端到端的问题、解决方案和结果。他们也可以根据需要掌握领域和技术。

+   **目的**：通过深度参与整个过程，他们与工作和结果有更直接的联系，从而增强了*目的感*。

### 但我们也需要专家

然而，全面掌握所有领域并不适合每个人（和每个团队），原因包括：

**想要专注于** 机器学习，或许是机器学习中的某个特定领域，如神经文本生成（参见：[GPT-3 primer](https://mc.ai/the-subtle-art-of-priming-gpt-3/)）。虽然全面掌握是有价值的，但我们也需要这些世界级的专家在研究和工业中推动边界。我们在机器学习中拥有的许多东西来自学术界和纯研究努力。

> 没有人通过成为一个通才来获得伟大。你不会通过分散注意力来提高某项技能。达到下一个水平的唯一方法是专注。 – 约翰·C·麦克斯韦

**缺乏兴趣。** 并不是每个人都热衷于与客户和企业互动，以定义问题、收集需求和编写设计文档。同样，也不是每个人都对软件工程、生产代码、单元测试和CI/CD管道感兴趣。

**在大型、高杠杆的系统上工作，其中0.01%的改进会产生巨大的影响。** 例如，算法交易和广告。在这种情况下，需要超专业化来挖掘这些改进。

其他人也提出了为什么数据科学家应该专注于某一领域（而不是全面掌握）的理由。以下是一些文章，提供平衡和反驳的观点：

+   [为什么你不应该成为数据科学通才](https://towardsdatascience.com/why-you-shouldnt-be-a-data-science-generalist-f69ea37cdd2c)

+   [为什么每个数据科学家都需要专注](https://www.simplilearn.com/why-every-data-scientist-needs-to-specialize-article)

+   [想在数据科学领域找到工作？这里是你应该专注的原因](https://www.dataquest.io/blog/job-data-science-specialize/)

### 学习的最佳方式是通过实践

如果你仍然渴望更全面，我们现在将讨论如何做到这一点。在此之前，未涉及具体技术，这里是全面数据科学家常用的技能类别：

+   产品：理解客户问题，定义和优先排序需求

+   沟通：促进团队之间的合作，获得支持，编写文档，分享结果

+   数据工程：将数据从A点移动和转换到B点

+   数据分析：理解和可视化数据，A/B测试与推断

+   机器学习：通常包括实验、实施和度量

+   软件工程：生产代码实践，包括单元测试、文档、日志记录

+   运维：基本的容器化和云熟练度，构建和自动化工具

（这个列表既不是强制性的，也不是详尽无遗的。大多数项目并不需要全部这些。）

这里有四种方法可以让你更接近成为一名全面的数据科学家：

**学习正确的书籍和课程。**（好吧，这*不是*通过实践学习，但我们都需要从某处开始）。我会关注那些涵盖隐性知识的课程，而非具体工具。虽然我没有找到这样的材料，但我听说[全栈深度学习](https://course.fullstackdeeplearning.com/)有很好的评价。

**自己做完整的项目**以获得整个过程的第一手经验。冒着简化的风险，以下是我会采取的一些步骤及其相关技能。

> 听而忘之，看而记之，做而理解。——孔子

从识别要解决的问题和确定成功指标（*产品*）开始。接着，找到一些[原始数据](https://datasetsearch.research.google.com/)（即非Kaggle竞赛数据）；这可以让你清理和准备数据，并创建特征（*数据工程*）。然后，尝试各种机器学习模型，检查学习曲线、误差分布和评估指标（*数据科学*）。

在选择一个模型之前，评估每个模型的性能（例如，查询延迟、内存占用），并编写一个基本的[inference class](https://eugeneyan.com/writing/product-categorization-api-part-3-creating-an-api/#creating-a-titlecategorize-class)用于生产（*软件工程*）。 （你可能还想构建一个简单的用户界面）。然后，将其容器化并通过你首选的云提供商在线部署，让其他人使用（*运维*）。

一旦完成，额外付出努力分享你的工作。你可以为你的网站写一篇文章，或在聚会上讲述（*沟通*）。通过有意义的视觉效果和表格展示你在数据中发现的内容（*数据分析*）。在GitHub上分享你的工作。[公开学习](https://www.swyx.io/writing/learn-in-public/)和工作是获取反馈和寻找潜在合作者的好方法。

**通过像[DataKind](https://www.datakind.org/)这样的组织进行志愿服务。** DataKind与社会组织（如非政府组织）和数据专业人士合作，以解决人道主义问题。通过与这些非政府组织合作，你将有机会作为团队的一部分，处理真实（且非常复杂）的数据来解决实际问题。

虽然志愿者可能会被分配具体角色（例如，PM，DS），但你总是欢迎跟随和观察。你将看到（并学习）PM如何与非政府组织合作来构建问题框架、定义解决方案，并围绕它组织团队。你将从其他志愿者那里学到如何使用数据来开发有效的解决方案。参加类似黑客马拉松的[DataDives](https://www.datakind.org/datadives)和长期的[DataCorps](https://www.datakind.org/datacorps)是贡献于数据科学过程端到端的一个绝佳方式。

**加入一个类似初创公司的团队。** 注意：类似初创公司的团队并不等同于初创公司。大型组织中有些团队以类似初创公司的方式运作（例如，双比萨团队），而初创公司中也可能由专才组成。寻找一个精干的团队，在那里你受到鼓励并有机会进行端到端的工作。

### 在Stitch Fix和Netflix中的端到端

**Stitch Fix**的Eric Colson最初是由于对“过程效率的吸引”而被吸引到功能型分工中（即[data science pin factory](https://multithreaded.stitchfix.com/blog/2019/03/11/FullStackDS-Generalists/)）。但经过反复试验，他发现端到端的数据科学家更为高效。现在，Stitch Fix不再为专业化和生产力组织数据团队，而是为了**学习和开发新的数据与算法产品**来组织它们。

> 数据科学的目标不是执行。而是学习和开发新的业务能力。……没有蓝图；这些是具有固有不确定性的全新能力。……你所需要的所有元素必须通过实验、试错和迭代来学习。— Eric Colson

他建议数据科学角色应更加通用，拥有广泛的责任，不依赖于技术职能，并优化学习。因此，他的团队招聘和培养可以进行概念化、建模、实施和测量的通才。当然，这依赖于一个可靠的数据平台，该平台可以抽象掉基础设施设置、分布式处理、监控、自动故障转移等的复杂性。

端到端的数据科学家提升了Stitch Fix的学习和创新能力，使他们能够发现并构建更多的业务能力（相比于专业团队）。

**Netflix**的Edge Engineering最初有专门的角色。然而，这在产品生命周期中造成了低效。代码发布花费的时间更长（几周而非几天），部署问题的检测和解决也花费了更长时间，生产问题需要多次反复沟通。

![](../Images/b48a7850434da38689387e8219cf14fa.png)

*极端情况下，每个功能区域/产品由7个人负责（[source](https://netflixtechblog.com/full-cycle-developers-at-netflix-a08c31f83249)）。*

为了解决这个问题，他们尝试了[全周期开发者](https://netflixtechblog.com/full-cycle-developers-at-netflix-a08c31f83249)，这些开发者被授权跨越整个软件生命周期进行工作。这需要思维方式的转变——开发者不仅要考虑设计和开发，还要考虑部署和可靠性。

![](../Images/b2a35c662f7201962c6455ca760bc707.png)

*与多个角色和人员不同，我们现在有了完整周期开发（[source](https://netflixtechblog.com/full-cycle-developers-at-netflix-a08c31f83249)）。*

为了支持全周期开发者，集中团队构建了工具来自动化和简化常见的开发流程（例如，构建和部署管道、监控、管理回滚）。这些工具可以在多个团队之间重复使用，起到倍增效应，并帮助开发者在整个周期内有效工作。

通过全周期开发者的方法，边缘工程能够更快地迭代（而不是在团队之间协调），实现更快和更常规的部署。

### 这对我有效吗？以下是一些例子

在IBM，我在一个团队中创建了员工的职位推荐。运行整个管道需要很长时间。我认为通过将数据准备和特征工程管道移到数据库中可以将时间缩短一半。但数据库人员没有时间进行测试。我不耐烦，进行了一些基准测试，将整体运行时间减少了90%。这使我们可以更快地进行实验，并节省了生产中的计算成本。

在构建Lazada的排名系统时，我发现Spark对于数据管道是必要的（因为数据量大）。然而，我们的集群只支持Scala API，而我对此不熟悉。不想等待（数据工程支持），我选择了更快但痛苦的方式来弄清楚Scala Spark并自己编写管道。这可能将开发时间减少了一半，并让我更好地理解数据，从而构建了更好的模型。

在一次成功的A/B测试后，我们发现业务利益相关者不信任模型。因此，他们手动选择展示的顶级产品，从而降低了在线指标（例如，点击率，转化率）。为了了解更多情况，我去了我们的市场（例如，印尼，越南）。通过相互教育，我们能够解决他们的担忧，减少手动覆盖的数量，并获得收益。

在上面的例子中，**超出常规DS & ML职位范围有助于更快地交付更多价值**。在最后一个例子中，有必要解除我们数据科学工作的阻碍。

### 尝试一下

你现在可能不是端到端的。这没关系——很少有人是。不过，考虑一下它的好处，并朝着它靠近一点。

哪些方面会不成比例地提升你作为数据科学家的交付能力？与客户和利益相关者的更多互动以设计更全面、创新的解决方案？构建和协调自己的数据管道？对工程和产品限制有更大的认识，以便更快地整合和部署？

> 不受欢迎的观点：数据科学家应该更具端到端能力。
> 
> 尽管这种做法（过于泛泛而谈！）受到批评，我发现它可以带来更多的背景、更快的迭代、更大的创新——更多的价值，速度更快。
> 
> 更多细节和Stitch Fix & Netflix的经验 [https://t.co/aOBjuBSsSz](https://t.co/aOBjuBSsSz)
> 
> — Eugene Yan (@eugeneyan) [2020年8月12日](https://twitter.com/eugeneyan/status/1293360153916407808?ref_src=twsrc%5Etfw)

[原文](https://eugeneyan.com/writing/end-to-end-data-science/)。经许可转载。

**简介：** Eugene Yan（[@eugeneyan](https://twitter.com/eugeneyan)）在消费者数据和技术的交叉点工作，构建帮助客户的机器学习系统。Eugene还撰写有关如何在数据科学、学习和职业生涯中取得成效的文章。目前，Eugene是亚马逊的一名应用科学家，帮助用户阅读更多内容，从阅读中获得更多收益。

**相关内容：**

+   [成功的数据科学家需要具备哪些素质？](https://www.kdnuggets.com/2020/09/successful-data-scientist.html)

+   [我作为数据科学家的两年所学](https://www.kdnuggets.com/2020/09/learned-2-years-data-scientist.html)

+   [机器学习工程师与数据科学家（数据科学结束了吗？）](https://www.kdnuggets.com/2020/06/machine-learning-engineer-vs-data-scientist.html)

### 更多相关主题

+   [端到端机器学习初学者指南](https://www.kdnuggets.com/2021/12/beginner-guide-end-end-machine-learning.html)

+   [将机器学习算法完整地部署到…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [5个最佳端到端开源MLOps工具](https://www.kdnuggets.com/5-best-end-to-end-open-source-mlops-tools)

+   [一个简单的端到端项目实现，使用HuggingFace](https://www.kdnuggets.com/a-simple-to-implement-end-to-end-project-with-huggingface)

+   [2024年必须尝试的7个端到端MLOps平台](https://www.kdnuggets.com/7-end-to-end-mlops-platforms-you-must-try-in-2024)

+   [分类问题的更多性能评估指标](https://www.kdnuggets.com/2020/04/performance-evaluation-metrics-classification.html)
