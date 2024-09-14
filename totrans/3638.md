# 操作化机器学习：成功MLOps的七个考虑因素

> 原文：[https://www.kdnuggets.com/2018/04/operational-machine-learning-successful-mlops.html](https://www.kdnuggets.com/2018/04/operational-machine-learning-successful-mlops.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**作者：Nisha Talagala，[ParallelM](http://www.parallelm.com/)。**

机器学习无处不在。从广告到物联网到医疗保健，以及其他领域，几乎所有行业都在采用或研究机器学习（ML）以提升业务。然而，为了利用ML获得正的投资回报率（ROI），它需要被*操作化*或投入生产。

将机器学习投入生产带来了[一系列独特的挑战](http://www.parallelm.com/more-machine-learning-models-than-ever-but-are-they-making-it-into-production/)。

*操作化的第一步是了解你的ML应用的样子、它的各个部分以及它们如何协同工作。*

**1: 了解你的ML应用**

在大多数情况下，ML用于优化（即添加洞察）业务应用。虽然这听起来很简单，但一个ML操作通常需要多个互补但独立的ML程序（训练、推理等）协同运行。什么是ML应用？ML应用是一起提供ML功能的程序和依赖项的集合。

图1(a)展示了将机器学习（ML）添加到业务应用中的最基本方式。业务应用请求预测，这些预测可以由ML推理程序提供（微服务方法在这里很受欢迎）。ML推理程序使用一个离线训练的模型，这通常由数据科学家完成。虽然这个流程很简单，但通常不够充分。在数据变化频繁的行业（如广告技术）中，需要对模型进行频繁的重新训练，以跟上变化的环境。此时需要添加一个重新训练管道以供推理使用，最终形成图1(b)中的模式。

重新训练引入了许多模型，可能需要人工干预以决定哪些模型部署到生产环境中（在财政、健康或其他结果与ML预测相关的情况下，这一点尤为重要）。添加人工审批会导致图1(c)中的模式。如果使用先进的算法（如集成模型）来提高准确性，模式将变为图1(d)中的模式。

最后，许多生产部署会并行使用多个预测管道（如冠军/挑战者、金丝雀等）来监控预测模式，检测意外的变化或异常。添加这样的测试基础设施生成了类似于图1(e)的模式。

![机器学习应用](../Images/ff41052418e4c68a37d7bec58e6cec04.png)

![机器学习应用2](../Images/c09837c953a3bbfbc7f43f58f164cb66.png)

**图1: ML应用**

一旦你的机器学习（ML）应用程序定义完毕，下一步就是确保它能够安全且成功地投入生产。

***投资回报率***：机器学习应用程序应该改善其伴随的业务应用，同时保持在行业和组织的要求范围内。

**2: 衡量成功**

机器学习应用程序存在的目的是服务于业务需求。业务应用的成功指标或关键绩效指标（KPI）应当被追踪，并与机器学习应用的引入及随后的优化相关联。这种关联为所有利益相关者提供了可见性，确保机器学习投资能够产生足够的回报，并帮助从数据科学家到运营人员的每个人衡量和证明新的运营投资。

**3: 管理生产机器学习风险**

风险管理并不会在模型开发阶段结束。一旦投入运行，模型需要被监控并不断评估，以确保它们在预期的范围内运行。生产机器学习的健康状况受到复杂性的影响，因为实时数据没有标签（因此无法使用常见的指标如准确率、精确度、召回率等）。替代方法（如数据偏差检测、漂移检测、金丝雀管道、生产A/B测试）应成为机器学习应用的一部分。

**4: 确保治理和合规**

一些行业，比如[金融服务](https://www.federalreserve.gov/supervisionreg/srletters/sr1107a1.pdf)，已经有了多年的机器学习合规要求。其他行业和地区也开始引入指导方针，比如欧盟的[GDPR](http://www.reubenbinns.com/blog/how-to-comply-with-gdpr-article-22-automated-credit-decisions/)或[纽约市算法问责法案](https://www.bizjournals.com/newyork/news/2017/12/13/n-y-c-council-passes-landmark-bill-to-ensure.html)。一个全面的生产治理机制对于确保机器学习应用程序（以及所有相关的模型、管道、代码、执行等）能够被追踪以实现可重复性、可审计性，并辅助[解释性](https://www.cc.gatech.edu/~alanwags/DLAI2016/(Gunning)%20IJCAI-16%20DLAI%20WS.pdf)是至关重要的。

***与DevOps的和谐***：MLOps应该与现有的DevOps集成，同时提供管理机器学习所需的额外独特能力。

**5: 自动化**

机器学习管道是代码，经典的 DevOps 工具链组件（如 Git 这样的源代码库、Jenkins 这样的自动化设施、AirFlow 这样的编排工具等）在 MLOps 中扮演着重要角色。然而，除了业务重点、风险降低和合规需求之外，生产中的机器学习还带来了额外的 CI/CD 和编排挑战，这些挑战是常见 DevOps 工具链无法解决的。例如，像图 1(e) 中的 ML 应用可能会有多个管道并行执行，并且有 ML 特定的相互依赖关系（如模型批准/传播或漂移检测）也需要进行管理。这些 ML CI/CD 和 ML 编排需要与组织中已经存在的 DevOps 实践无缝集成。

**6: 规模**

ML 应用可能需要不同于它们所服务的业务应用的硬件配置和扩展点。例如，训练神经网络模型可能需要强大的 GPU，而训练常规 ML 模型可能需要 CPU 集群。根据推理实现的不同，可能需要流处理器集群、REST 端点或批处理推理操作。许多强大的生产级分析引擎（如 Spark、Flink、PyTorch、scikit-learn 等）存在用于执行 ML 管道。ML 应用需要根据管道的需求，合理配置和映射到这些引擎中的一个或多个。

***与组织的和谐***: MLOps 应该使你所有的组织职能协同工作，以使 ML 为你的业务应用发挥作用。

**7: 人员**

生产中的机器学习需要组织内的多个职能（数据科学家、数据工程师、业务分析师和运营人员）进行协作。每个角色需要查看 ML 应用的不同方面。数据科学家可能关注训练准确性的指标、生产 A/B 测试的置信度或数据偏差检测，而运营人员可能想查看正常运行时间和资源利用率，业务分析师可能希望看到 KPI 的改进。MLOps 工具链需要为所有这些参与方提供可见性、管理访问控制和协作功能。

***总结: 结合一切***

![MLOps 元素](../Images/94c55c1cb9c630c35eac9c47f886a0b5.png)

总体而言，你的 MLOps 应该包含图 2 中的所有元素，所有元素应协同工作，以形成一个成功的 ML 操作的整体。

**简介: [Nisha Talagala](https://www.linkedin.com/in/nisha-talagala-6a6b20/)** 是ParallelM的CTO/VP工程。她的背景是分布式系统的软件开发，专注于存储、I/O、文件系统、持久内存和闪存非易失性存储。之前在PM之前，她是Fusion-io（被SanDisk收购）的首席架构师/研究员，开发持久内存、非易失性内存文件系统（NVMFS）和应用加速的新技术和软件栈。她拥有加州大学伯克利分校的博士学位，并持有56项专利。

**相关：**

+   [将机器学习应用于DevOps](https://www.kdnuggets.com/2018/02/applying-machine-learning-devops.html)

+   [分析DevOps范式中的数据版本控制](https://www.kdnuggets.com/2017/08/data-version-control-analytics-devops-paradigm.html)

+   [2018年机器学习/人工智能的重点领域应该是什么？](https://www.kdnuggets.com/2018/04/focus-areas-ml-ai-2018.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的IT工作

* * *

### 更多相关内容

+   [机器学习与您的大脑不同，第七部分：神经元…](https://www.kdnuggets.com/2022/08/machine-learning-like-brain-part-seven-neurons-good.html)

+   [数据质量在成功机器学习模型中的重要性](https://www.kdnuggets.com/2022/03/significance-data-quality-making-successful-machine-learning-model.html)

+   [成功数据科学家的五大特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [如何在2022年成为成功的数据科学自由职业者](https://www.kdnuggets.com/2022/02/become-successful-data-science-freelancer-2022.html)

+   [MLOps中的机器学习设计模式](https://www.kdnuggets.com/2022/02/design-patterns-machine-learning-mlops.html)

+   [优化和管理机器学习生命周期的前10大MLOps工具](https://www.kdnuggets.com/2022/10/top-10-mlops-tools-optimize-manage-machine-learning-lifecycle.html)
