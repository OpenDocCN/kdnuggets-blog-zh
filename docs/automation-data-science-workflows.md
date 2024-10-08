# 数据科学工作流程中的自动化

> 原文：[`www.kdnuggets.com/2023/03/automation-data-science-workflows.html`](https://www.kdnuggets.com/2023/03/automation-data-science-workflows.html)

机器学习解决方案已经自动化了世界上大部分的操作方式，并且现在也在处理自身的低效问题。所以，数据科学领域也不例外，正在经历核心机器学习工程过程的自动化，以实现更顺畅、更快速的开发。

![数据科学工作流程中的自动化](img/bf23c56a689fb58e0f9ca0f26af4754a.png)

图片由 [RODNAE Productions](https://www.pexels.com/photo/woman-using-her-smartphone-while-playing-league-of-legends-7915280/) 提供

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

想象一下以前那些步骤繁琐的过程——从数据集成到模型训练、选择和部署——都是手动完成的。每个步骤都非常严格，需要数据科学家付出大量的努力。无可否认，自动化在帮助数据科学家完成端到端建模和部署过程中变得极为重要。

自动化机器学习（AutoML）显著提高了开发者的生产力，使他们能够专注于需要时间和注意力的关键建模领域。

在评估 AutoML 的优缺点之前，让我们首先了解数据科学世界在机器学习过程自动化之前的运作方式，以更好地理解其价值主张。

# 自动化胜过人工努力——对组织和数据科学社区的双赢

AutoML 常被视为复制数据科学家的工作，但实际上它是一种加速构建更好模型的工具。数据科学家仍然有许多工作需要手动完成，这给机器学习实施带来了挑战。 [dotData](https://dotdata.com/) 的首席执行官藤卷良平如下解释。

> 对于组织来说，至关重要的是不要将自动化视为对数据科学家的“替代”，而应将其视为一种工具。我们发现许多企业现在将特征工程过程从数据科学组织中分离出来，交给专注于特征发现的专门团队。无论设置如何，提供使数据科学家工作更轻松的自动化工具和平台应成为重点。
> 
> – Ryohei Fujimaki，[dotData](https://dotdata.com/)的首席执行官

机器学习流程中最重要但非常关键且耗时的步骤之一是数据分析和验证数据的良好质量。在这一环节的任何失败或细节偏差都可能代价高昂，因此需要一位熟练的数据分析师来奠定基础。

除了数据分析，数据清洗和特征工程还大大加速了模型学习现象的速度。但问题在于这些技能需要时间来培养。因此，与其等待建立合适的团队和技能来筛选海量数据集中的模式并生成有价值的见解，不如通过自动化机器学习工作流来消除建立模型的障碍。

简而言之，它帮助企业快速扩展其机器学习计划，使非技术专家也能利用这些复杂算法的力量。不仅自动化有助于提高模型准确性，还带来了行业最佳实践，因此没有人需要在已经解决的重复问题上重新发明轮子。

让数据科学家免于花费在可以轻松自动化的无尽琐碎任务上的时间，从而赋予他们用脑力将创新变为现实的能力。

根据[微软](https://learn.microsoft.com/en-us/azure/machine-learning/concept-automated-ml)对 AutoML 的观点，它是通过自动化耗时的迭代任务来构建大规模、高效且富有生产力的机器学习模型的过程，同时保持模型质量。

这需要思维方式的转变，以通过自动化手动任务（如特征工程、特征发现、模型选择等）来改进流程和构建系统。

> 数据科学过程仍然是一个主要依赖人工的工作。若应用得当，自动化可以大大帮助数据科学家，而不必担心“失业”。当 AutoML 首次流行时，数据科学社区的讨论主要集中在自动化整个数据科学流程生命周期的利弊上。在 dotData，我们发现这种“全有或全无”的方法低估了数据科学流程的复杂性——尤其是在大型组织中。因此，我们认为公司应该更关注提供自动化，以简化数据科学家的工作并提高工作效率。一个这样的领域是特征工程。数据科学家花费大量时间与数据工程师和主题专家合作，发现、开发和优化他们模型的最佳特征。通过自动化大量特征发现过程，数据科学家可以专注于他们真正擅长的任务：构建最佳的机器学习模型。
> 
> – Ryohei Fujimaki，[dotData](https://dotdata.com/)的首席执行官

除了提升生产力和效率外，它还减轻了人为错误和偏见的风险，从而增加了模型的可靠性。但正如专家所说，过犹不及。因此，自动化在一定程度的人类监督下最佳，可以结合实时信息和领域专业知识。

# 自动化的重点领域

现在我们了解了自动化的好处，让我们重点关注最值得投入时间和精力的具体步骤和流程。以下列出的领域的自动化有可能显著提高效率和准确性：

+   **数据准备：**来自不同来源的数据使得数据科学家将其准备成适合输入模型训练阶段的格式变得具有挑战性。这涉及诸多步骤，如数据收集、清理和预处理等。

+   **特征选择和特征工程：**向模型选择和呈现正确的特征是学习正确现象的基础。自动化不仅有助于找到正确的特征，还用于工程化新特征，以加速学习过程。

+   **模型选择：**这是在候选模型集合中找到最佳表现模型的过程，它决定了模型开发流程的准确性和稳健性。AutoML 在迭代和识别适合特定任务的模型方面非常有用。

+   **超参数优化：**选择正确的模型是不够的，你还需要为给定的机器学习算法找到合适的超参数，如学习率、层数和迭代次数。这些模型设置需要机器学习工程师调整这些参数，以最优地解决机器学习问题。自动化的超参数优化是一个不可或缺的工具，通过评估各种组合来找到最适合你模型的架构。

+   **模型监控：**没有哪个机器学习模型能够持续提供准确的预测而无需定期重新训练。自动化工具监控并触发模型流程，以便在部署的模型偏离预期性能时采取纠正措施。

![数据科学工作流中的自动化](img/195e9a8fd05559b97f8c7e5aca96f9d0.png)

图片来自 [Canva](https://www.canva.com/photos/MADmjDZgSrI/)

# 结束语

一般来说，自动化被视为“技术抢走工作”，然而，它实质上有助于简化重复和单调的任务。数据科学中的自动化是数据科学家的一大助力，通过减少人工操作，从而实现改进和高效的建模过程。必须用公平的人类专业知识和监督来补充 AutoML，以充分发挥自动化处理数据科学工作流中挑战性部分的好处。

**[Vidhi Chugh](https://vidhi-chugh.medium.com/)** 是一位人工智能战略家和数字化转型领导者，致力于在产品、科学与工程的交汇处构建可扩展的机器学习系统。她是一位获奖的创新领袖、作者和国际演讲者。她的使命是让机器学习民主化，打破术语，使每个人都能参与到这场转型中。

### 更多相关主题

+   [免费 Python 自动化课程](https://www.kdnuggets.com/2022/07/free-automate-python-course.html)

+   [3 个有用的 Python 自动化脚本](https://www.kdnuggets.com/2022/11/3-useful-python-automation-scripts.html)

+   [将 ChatGPT 集成到数据科学工作流中：技巧与最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [掌握数据科学工作流与 ChatGPT](https://www.kdnuggets.com/mastering-data-science-workflows-with-chatgpt)

+   [云计算如何增强数据科学工作流](https://www.kdnuggets.com/2023/08/cloud-computing-enhances-data-science-workflows.html)

+   [6 家初创公司重新定义了 OpenUSD 和生成性人工智能的 3D 工作流](https://www.kdnuggets.com/6-startups-redefining-3d-workflows-with-openusd-and-generative-ai)
