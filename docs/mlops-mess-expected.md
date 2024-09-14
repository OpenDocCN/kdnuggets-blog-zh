# MLOps Is a Mess But That’s to be Expected

> 原文：[https://www.kdnuggets.com/2022/03/mlops-mess-expected.html](https://www.kdnuggets.com/2022/03/mlops-mess-expected.html)

[![MLOps Is a Mess But That's to be Expected](../Images/08a488eb2f4010f220a8a0378086e076.png)](https://www.mihaileric.com/static/brown_puzzle-a8291f30f3a4deef88a28faa03f82a7c-8701a.jpg)

这听起来熟悉吗？你读到一篇文章说，做机器学习是2022年*最好的*工作，不仅需求狂热，而且薪水在[行业中名列前茅](https://artificialintelligence-news.com/2019/03/15/machine-learning-jobs-high-paying-demand/)。

听起来不错：工作安全和钱。这有什么不好？

你决定去学习，掌握成为机器学习工程师的技能，做一些项目来丰富你的简历，并争取到那份工作。你感觉很好。我是说，这可能有多难呢？

你记得在Twitter上看到有[伯克利的全栈深度学习课程](https://fullstackdeeplearning.com/)似乎非常好。你做了几节课，然后看到这个现代ML生态系统所需工具的图表：

[![MLOps Is a Mess But That's to be Expected](../Images/a4fb80956180299fd498cbd1bc5b065c.png)](https://www.mihaileric.com/static/ml_tools_new-fc680c1262695a4d85bdd89a47b85770-0f93e.png)

哇。这真是要学很多东西。你用过一些这些技术，但什么是Airflow？dBt？Weights & Biases？Streamlit？Ray？

你有点沮丧。经过一天的课程材料审查后，你决定从专家那里获得一些激励。

风险投资家总是善于考虑大局，描绘美好的前景，让人们感到兴奋。

你记得那位风险投资人[Matt Turck](https://mattturck.com/)总是会做一些关于当前AI热门的年度回顾。

新兴的炫酷技术。那总能让你比波士顿动力的演示视频更兴奋。

所以你查看了[他2021年的回顾](https://mattturck.com/data2021/)讨论机器学习和数据领域的情况。

这是你看到的第一张图片：

[![MLOps Is a Mess But That's to be Expected](../Images/fb0b143ffbb6742f2b0db501e432e9fd.png)](https://www.mihaileric.com/static/ml_and_data_landscape-d79aaf0a06b8b0c3767711103a8f19e4-75a12.jpeg)

这究竟是什么鬼？

你关闭浏览器，倒上一杯苏格兰威士忌，思索生活的无常。

今天，机器学习仍然是最受关注和推崇的技术浪潮之一，承诺将彻底改变社会的每一个角落。

尽管如此，这个生态系统依然处于狂热的状态。

每周都有新的基础科学进展。初创公司和企业将新的开发工具投入市场，试图捕捉被许多人猜测为2025年市场价值在[$40-120 billion](https://searchsoftwarequality.techtarget.com/feature/Analysts-mixed-on-future-growth-of-MLOps-AutoML-tools)之间的份额。

事情发展迅猛而激烈。

然而，如果你刚刚进入这个讨论，你如何理解这一切呢？

在这篇文章中，我想重点讨论今天机器学习操作（MLOps）的现状，我们现在的位置，以及未来的方向。

作为在像 Amazon Alexa 这样的 AI 先进组织工作过的从业者，同时还经营着一个 [机器学习咨询公司](https://www.pametandata.com/?utm_source=website&utm_medium=blog&utm_campaign=mlops-is-a-mess)，我亲身经历了将机器学习应用于现实世界中的种种考验与磨难。

我真的相信机器学习有很多值得乐观的地方，但这条路并非没有一些小障碍。

因为 Google Analytics 告诉我约 87% 的读者会在这个引言后离开，这里是给忙碌读者的 TLDR。

**TLDR**: 目前 MLOps 在工具、实践和标准方面处于非常混乱的状态。然而，考虑到我们仍处于更广泛企业机器学习采纳的早期阶段，这种状态是可以预期的。随着这一转型在未来几年继续发展，预计混乱将会平息，同时基于 ML 的价值将变得更加普遍。

我们开始吧。

# 名称背后的意义

让我们先从一些定义开始。

MLOps 指的是一套将机器学习系统部署并可靠维护于生产环境中的实践和工具。简而言之，MLOps 是机器学习进入和存在于现实世界的媒介。

这是一门存在于 devops、数据科学和软件工程交汇处的多学科领域。

[![MLOps 是一团糟，但这是可以预期的](../Images/61787ba99920c8bf8c67924bbc7ed98e.png)](https://www.mihaileric.com/static/mlops_venn_diagram-15ead300c3224748ba1fee10a5a6c1dd-17122.png)

尽管 AI 研究中仍有令人兴奋的新进展，但今天我们正处于机器学习的部署阶段。

在 Gartner 的炒作周期范式中，我们正逐渐进入启蒙坡度，我们已经超过了 AGI 的恐慌宣传和 [*Her*](https://en.wikipedia.org/wiki/Her_(film)) 的承诺，组织现在开始提出关于如何最大化机器学习投资的严肃操作问题。

[![MLOps 是一团糟，但这是可以预期的](../Images/d84133e298abab07403420ad2b48f321.png)](https://www.mihaileric.com/static/hype_cycle_ml-ada152ca101ecf90fdaf92b934ac08a7-be75c.png)

# 目前的状态

目前 MLOps 的状态非常混乱，工具的种类繁多，犹如亚马逊雨林中的珍稀物种。

举个例子，大多数从业者都会同意，在生产环境中监控你的机器学习模型是维护一个稳健、高效架构的关键部分。

然而，当你开始选择供应商时，我可以在不费吹灰之力的情况下列出 6 个不同的选项：Fiddler、Arize、Evidently、Whylabs、Gantry、Arthur 等。我们甚至还没提到那些纯数据监控工具。

不要误解我：有选项是好的。但这些监控工具是否真的*如此*有区别，以至于我们需要6个以上的工具？即使你选择了一个监控工具，你仍然需要知道跟踪哪些指标，这通常高度依赖于上下文。

这进一步引出了一个问题，监控市场是否真的*如此*庞大，以至于这些都是十亿美元的公司？

至少在监控方面，人们通常对这些公司试图掌控机器学习生命周期的确切部分有一个普遍的共识。堆栈的其他部分则没有那么明确地被理解和接受。

为了说明这一点，许多公司现在倾向于将他们为MLOps堆栈构建的每一个新工具称为某种*商店*。我们从[模型商店](https://neptune.ai/blog/mlops-model-stores)开始。接着，[特征商店](https://www.tecton.ai/blog/what-is-a-feature-store/)出现了。现在我们还有[度量商店](https://medium.com/airbnb-engineering/how-airbnb-achieved-metric-consistency-at-scale-f23cc53dea70)。哦，还有[评估商店](https://gantry.io/)。

我的一般看法是，机器学习社区在为*数据库*创造同义词方面特别有创造力。

更严肃的看法是，整个领域仍在标准化全面构建ML管道的最佳方式。围绕最佳实践达成共识将是一个5-10年以上的转型过程。

在[MLOps社区](https://mlops.community/)的从业者之间进行的一次特别引人入胜的讨论中，[Lina](https://www.linkedin.com/in/lina-weichbrodt-344a066a/?originalSubdomain=de)提出了一个观点，即*ML堆栈*大致上和*后端编程开发堆栈*一样普遍。

那种观察非常敏锐，认为经典的*ML堆栈*仍然没有明确定义。

从这个角度来看，当我们考虑MLOps管道的阶段时，与其像[famous Sculley paper](https://proceedings.neurips.cc/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf)中的清晰架构图，不如说我们现在有了我称之为MLOps Amoeba™的东西。

[![MLOps是一团糟，但这是可以预期的](../Images/3370b37aa7c0334621215fbfc7cb2b78.png)](https://www.mihaileric.com/static/expectvsreality-fa263ca0d1a603fee36365595c235f15-4acac.png)

我们对很多正确的部分有一个大致的认识，但真正的关注点分离仍在发展中。

因此，MLOps工具公司往往进入市场时解决某个特定的细分市场，然后不可避免地开始像变形虫一样扩展到周围的架构职责。

我相信，这种不断变化的工具景观和新的界限对于领域中的新手尤其困难。现在是进入MLOps领域的艰难时期。

我把今天的MLOps比作现代网页开发的状态，新工具不断进入市场，你可以使用大约300种不同的框架组合来构建一个简单的*Hello World*网页应用。

在这种情况下，我对新手的建议是：

1.  寻求更有经验的个人来帮助你考虑选择，思考不同的技术，并成为“愚蠢”问题的倾听者。

1.  仔细考虑你要解决的问题和[解决问题所需的基本方法论](https://madewithml.com/#mlops)，而不是过于分心于炫目的工具或平台。

1.  花大量时间构建真实系统，以便你可以亲身体验不同工具解决的问题。

并且要认识到没有人拥有所有答案。我们仍在摸索*正确*的做法。

# 你可以从合理规模的机器学习开始。

另一点值得注意的是，很容易给人一种企业中的机器学习成熟度非常先进的印象。

根据我的经验以及与我交谈的其他从业者的经验，企业机器学习成熟度的现实远比我们根据工具和资金环境所认为的要温和得多。

事实上，只有少数几个超级先进的AI首创企业拥有强大的机器学习基础设施来处理其PB级的数据。

尽管大多数公司没有那种规模的数据，因此没有那些类型的机器学习需求，但以AI为首的企业最终会定义工具和标准的叙事。

实际上，仍有大量杰出的公司在摸索自己的机器学习战略。

[![MLOps 混乱，但这在意料之中](../Images/b7038f935a17da474e9024da74046971.png)](https://www.mihaileric.com/static/ml_sophistication_plot-2ca09886b22fea264818760cdcdf59c8-cc70d.png)

这些“合理规模的机器学习”公司（使用[Jacopo的术语](http://www.jacopotagliabue.it/)），本身就是出色的企业（涉及自动化、时尚等不同领域），拥有良好的专有数据集（数百GB到TB），但在机器学习应用上仍处于早期阶段。

这些公司有望通过机器学习获得首次成功，通常有相对[容易获得的成果](https://eugeneyan.com/writing/real-time-recommendations/#how-to-design-and-implement-an-mvp)来实现这些成功。它们甚至不一定需要这些超级先进的亚毫秒延迟的超实时基础设施来开始提升其机器学习水平。

我相信，在未来10年中，MLOps面临的一个大挑战将是帮助这些类型的企业上手。

这需要构建适合机器学习稀缺的工程团队的友好工具，改善尚未准备好进行高级数据工作的内部基础设施，并获得关键业务利益相关者的文化认同。

# MLOps可以从DevOps的经验中学到什么

为了帮助我们理解MLOps的进展阶段以及未来的方向，考虑DevOps运动的类比是有帮助的。

企业对DevOps实践的采纳是一个历时数十年的转型。在DevOps引入之前，软件工程和IT（或运维）团队作为功能上独立的实体运作。这种孤立的组织造成了产品发布和更新的巨大低效。

谷歌是最早认识到这种低效的组织之一，在2003年引入了站点可靠性工程师的角色，以帮助弥合开发人员和运维人员之间的差距。

DevOps的原则在2009年由John Allspaw和Paul Hammond在一场[重要演讲](https://www.youtube.com/watch?v=LdOe18KhtT4)中进一步确立，演讲中主张企业应雇佣“像开发者一样思考的运维人员”和“像运维人员一样思考的开发者”。

随着DevOps的成熟，我们引入了诸如持续集成（和部署）等概念，以及一些已经成为全球开发团队常用的工具。

好的，历史课讲完了，我们怎么把这些联系起来呢？

DevOps是理解MLOps的一个有趣案例，原因有很多：

1.  这突显了企业采纳所需的漫长转型过程。

1.  这显示了该运动既包括工具的进步，也包括组织文化心态的变化。两者必须携手前进。

1.  这突显了对具备[cross-functional skills and expertise](https://netflixtechblog.com/full-cycle-developers-at-netflix-a08c31f83249)的从业者的迫切需求。孤立无用。

目前的MLOps处于一种混乱状态，但这是意料之中的。如何最好地定义关于基础设施、开发和数据问题的清晰抽象仍不清楚。

我们将采用新的实践和方法论。工具会不断变化。存在很多问题，作为一个社区，我们正在积极地假设答案。这个运动仍处于初期阶段。

# 强预测，弱持有

现在我们已经讨论了现状，我想花些时间描述一下未来值得期待的事物。

这些未来的趋势是许多与ML从业者讨论的轶事性组合，以及我最兴奋的内容的适度添加。

从技术和架构的角度来看，有几个方面我们将继续看到投资：

+   **闭环机器学习系统**。现在许多机器学习系统仍然主要遵循从数据源到预测的单向信息流。但这会导致过时和根本破碎的流程。我们需要关闭这些环路，以使流程变得更智能和灵活。实现这一目标的第一步是拥有一个好的监控系统。虽然我在本文的早期部分批评了监控，但我确实认为我们应该继续考虑对系统的监控，包括预测建模层以及更上游的数据层。这是[Shreya](https://www.shreya-shankar.com/rethinking-ml-monitoring-1/)在这方面做了大量优秀写作的内容。

+   **机器学习的声明式系统**。近年来，我们已经看到机器学习建模工具随着像[PyTorch](https://pytorch.org/)这样的软件以及像[HF Transformers](https://huggingface.co/docs/transformers/index)这样的高层库的出现而逐渐商品化。这对该领域来说是一个受欢迎的步骤，但我相信我们还能做得更多。使某事物民主化涉及开发隐藏不必要实现细节的抽象，以便下游用户使用。[声明式机器学习系统](https://arxiv.org/abs/2107.08148)是这一建模演变中最令人兴奋的下一个阶段之一。让全新一代领域专家将机器学习应用于他们的问题需要一些工具，这些工具类似于[Webflow和其他工具](https://webflow.com/)对网页开发所做的工作。

+   **实时机器学习**。机器学习系统本质上是数据驱动的，因此对于大多数应用来说，拥有最新的数据可以提供更准确的预测。实时机器学习可以有很多含义，但一般来说，拥有更新的模型更好。这是我们在接下来的几年中将继续看到投资的领域，因为构建实时系统是一个困难的基础设施挑战。我还要补充一点，我不认为许多公司（特别是那些在上面提到的“合理规模”的公司）需要实时机器学习才能获得初步胜利。但最终我们会达到这样的阶段，从第一天起集成这些类型的系统将不再是一个巨大的负担。[Chip](https://huyenchip.com/2020/12/27/real-time-machine-learning.html)在这方面做得非常好。

+   **更好的数据管理**。数据是每个机器学习系统的命脉，在一个机器学习驱动大多数企业功能的未来世界中，数据是每个组织的命脉。然而，我认为我们在开发工具来管理数据生命周期，从来源和策划到标记和分析，方面还有很长的路要走。不要误解我的意思：我和其他人一样喜欢Snowflake。但我相信我们可以做得比组织VPN中的少数几个孤立表更好。我们如何才能让数据从业者更容易回答关于他们拥有什么数据、他们可以用数据做什么以及他们还需要什么的问题？[数据发现工具](https://eugeneyan.com/writing/data-discovery-platforms/) 是解决这些问题的初步步骤。在我看来，当我们考虑到非结构化数据时，工具的不足尤为明显。

+   **将商业洞察工具与数据科学/机器学习工作流程融合**。围绕商业洞察的工具一直比数据科学的工具更为成熟，[更多成熟](https://www.hashpath.com/2020/11/build-a-modern-data-analytics-stack-from-scratch-in-under-an-hour/)的原因之一在于，数据分析师的产出往往更容易被商业领袖看到，因此商业智能工具的投资被认为对组织的底线更为关键。随着数据科学产出越来越直接地为组织创造价值，我相信我们将看到这两个领域之间的协同作用不断增加。归根结底，机器学习是一种能够实现更高效操作、改善指标和更好产品的工具。从这个角度来看，它的目标与商业智能的目标并没有太大区别。

现在，从技术层面退一步，以下是我们未来将看到的一些元趋势：

+   **人才短缺仍然严重**。对许多组织来说，如今很难吸引优质的机器学习人才。当大型科技公司为AI人才开出中位数年薪[$330K/year](https://aipaygrad.es/)时，你可以想象招聘领域的竞争有多激烈。供给要多年才能最终与需求平衡。在此期间，这种短缺将继续证明公司对MLOps工具的投资是合理的：如果你无法雇佣机器学习工程师，就尝试用工具替代他们！

+   **围绕端到端平台的整合越来越紧密**。三大云服务提供商（AWS、GCP、Azure）以及其他专门的公司（[DataRobot](https://www.datarobot.com/)、[Databricks](https://databricks.com/)、[Dataiku](https://www.dataiku.com/)）都在积极尝试掌控从头到尾的机器学习工作流。虽然我认为目前没有任何端到端解决方案已经准备好进入主流市场，但这些公司有几个优势：1）大量资本用于吞并（抱歉，收购）较小的公司，2）使用方便，但灵活性不足且成本较高，3）品牌惯性（即“买IBM不会被解雇”症候群）。

+   **机器学习思维的文化采纳**。随着我们获得越来越先进的工具，组织将围绕其产品和团队采纳机器学习的思维方式。在许多方面，这也呼应了[DevOps原则](https://www.atlassian.com/agile/devops)被采纳时发生的文化转变。我们可以期待看到一些变化，包括更全面的系统思维（打破孤岛）、增强产品中的反馈循环，以及培养持续实验和学习的心态。

哇，这篇文章很长。如果你还在阅读，谢谢你。

虽然今天的MLOps仍然混乱，但我对机器学习为社会带来的价值仍然充满乐观。在许多领域，数据驱动的技术能够带来效率提升、洞察和改善结果。

所有的要素都在逐步到位：不断发展的工具链来构建系统，使其不断改进，[教育](https://madewithml.com/) [资源](https://stanford-cs329s.github.io/) 帮助培训下一波从业者，以及越来越广泛地认识到对机器学习的有意识投资对组织的重要性。

未来光明。

*感谢 [Goku Mohandas](https://twitter.com/gokumohandas)、[Shreya Shankar](https://twitter.com/sh_reya)、[Eugene Yan](https://twitter.com/eugeneyan)、[Demetrios Brinkmann](https://twitter.com/Dpbrinkm) 和 [Sarah Catanzaro](https://twitter.com/sarahcat21) 对本帖早期版本提供的深刻和周到的反馈。好的内容归他们所有，任何糟糕的笑话都是我的。*

**[Mihail Eric](https://twitter.com/mihail_eric)** 是一名机器学习研究员以及Confetti的创始人。

[原文](https://www.mihaileric.com/posts/mlops-is-a-mess/)。经允许转载。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关话题

+   [Baize：一个开源聊天模型（但有所不同？）](https://www.kdnuggets.com/2023/04/baize-opensource-chat-model-different.html)

+   [如果你想掌握生成 AI，忽略所有（但两个）工具](https://www.kdnuggets.com/if-you-want-to-master-generative-ai-ignore-all-but-two-tools)

+   [5 个需求高但未得到足够认可的 IT 职业](https://www.kdnuggets.com/5-it-jobs-that-are-high-in-demand-but-dont-get-enough-recognition)

+   [在纽约的 Rev 3 连接数据科学社区，#1…](https://www.kdnuggets.com/2022/03/domino-connect-data-science-community-nyc-mlops-conference.html)

+   [MLOps：推动 AI 主流的关键](https://www.kdnuggets.com/2022/07/mlops-key-pushing-ai-mainstream.html)

+   [最后召集：Stefan Krawcyzk 的《掌握 MLOps》直播班](https://www.kdnuggets.com/2022/08/sphere-last-call-stefan-krawcyzk-mastering-mlops.html)
