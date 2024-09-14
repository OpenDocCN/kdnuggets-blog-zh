# 探索思维树提示：人工智能如何通过搜索学习推理

> 原文：[https://www.kdnuggets.com/2023/07/exploring-tree-of-thought-prompting-ai-learn-reason-through-search.html](https://www.kdnuggets.com/2023/07/exploring-tree-of-thought-prompting-ai-learn-reason-through-search.html)

![探索思维树提示：人工智能如何通过搜索学习推理](../Images/c0ca0a67a120b9aa78228cb36a35e334.png)

图片由作者使用 Midjourney 创建

# 重点

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

+   一篇新论文提出了一个“思维树”框架，以实现更深思熟虑的问题解决

+   将推理过程表示为对可能的“思维”树的搜索

+   使用 LLM 自身生成和评估这些思维

+   使用经典的搜索算法来指导探索

# 介绍

最近，像 GPT-3 这样的语言模型在数学推理和常识知识等领域展示了令人印象深刻的能力。然而，它们的基本文本生成方法——从左到右、逐个单词——可能限制了战略规划和探索。论文显示，这种方法显著提高了 LLM 在解决数学难题和创造性写作等挑战中的能力。

# 讨论

最近一篇论文，[**思维树：使用大型语言模型进行深思熟虑的问题解决**](https://arxiv.org/abs/2305.10601) — 由 Shunyu Yao、Dian Yu、Jeffrey Zhao、Izhak Shafran、Thomas L. Griffiths、Yuan Cao 和 Karthik Narasimhan 共同提出 — 提出了一个名为“思维树”（ToT）的新框架，用于提升大型语言模型（LLMs），如 GPT-3 和 GPT-4 的问题解决能力。目前，LLMs 在生成文本时局限于从左到右的单词级别决策，这在需要更多战略规划和探索的任务中可能显得不足。

ToT 将问题解决过程表示为对树的搜索，其中每个节点是一个“思维”——一个表示中间推理步骤的连贯文本块。这使得 LLM 可以探索多个推理路径并评估不同思维在解决问题中的进展。具体来说，这一框架涉及：

1.  根据任务结构将问题分解为连贯的思维步骤。

1.  使用 LLM 在每一步生成多个思维候选，无论是独立生成还是基于先前思维的序列条件生成。

1.  让大语言模型（LLM）通过价值评估提示来评估不同状态（部分解决方案）的前景，这些提示评估到目前为止的进展。

1.  使用经典的搜索算法，如广度优先搜索或深度优先搜索，通过树结构，利用LLM的价值估计来指导探索和剪枝。

这种有意的搜索方法允许LLM进行前瞻性思考、回溯，并在需要时做出更具全球性的选择。这个模块化框架与模型无关，可以根据问题结构灵活调整其组件，如思维规模、生成、评估和搜索。

作者在三个新任务上演示了ToT——24点游戏、创意写作和迷你填字游戏。在所有情况下，ToT显著提升了GPT-4在问题解决中的表现，相比于标准提示基线。例如，在24点游戏中，成功率从使用链式思维提示的4%提高到使用ToT的74%。

总体而言，ToT提供了一种将经典AI中的符号规划和搜索方法与现代LLM结合的方式。其基于语言的思维和审议的可解释性也为更好的人类对齐提供了机会。作者将其提出作为开发更通用问题解决能力的一个令人兴奋的新方向。

# 研究问答

*“思维树”方法与其他结合符号规划或搜索与神经模型的方法（如NeuroLogic解码或LLM+P框架）相比如何？*

ToT框架的不同之处在于，它使用LLM自身在搜索过程中提供启发式指导，而不是依赖于单独的经典规划器（LLM+P）或硬编码的启发式方法（NeuroLogic）。基于语言的思维表示也比符号规划语言更灵活。然而，ToT尚未达到LLM+P所展示的LLM与规划器组件之间的紧密集成和双向通信水平。

*“思维树”方法是否可以应用于自然语言任务，如对话或故事生成，而不仅仅是受限的推理任务？*

尽管目前的论文重点是推理任务，但将可能的继续表示为可以审议的思维的通用框架似乎也适用于不那么受限的生成问题。对于对话，思维可以是候选的下一步发言，而对于故事，它们可以是情节点或角色动作。关键挑战将是定义连贯的思维步骤并开发有效的评估提示。

*这项研究的创新之处在哪里？*

关键的创新在于将语言模型推断框架设定为思维树上的搜索，而不仅仅是从左到右的标记生成。这允许更有意识的规划、探索替代方案和全局前瞻/回溯。将思维表示为连贯的语义单位也比之前的搜索方法更具创新性。

*这项研究的更广泛影响是什么？*

这项研究可能显著提升LLM的解决问题和推理能力，使其能够应用于更复杂的现实世界任务，如编码、数据分析、机器人技术等。同时，它还使模型决策更加可解释。将经典搜索方法与神经模型的结合是一个令人兴奋的方向。

*这项研究在呈现过程中是否存在潜在的问题或遗漏？*

探索的任务仍然相对简单。是否能将这一方法扩展到更开放的问题尚未可知。搜索过程可能比标准采样需要更高的计算成本。修剪次优分支的启发式方法目前仍不完美。

*从这项研究中，接下来的逻辑研究步骤是什么？*

重要的下一步是探索ToT在更复杂的规划和决策任务中的应用，将其与外部知识检索集成，并研究变体是否可以通过元学习或强化学习以更高效的样本方式进行学习，而不是仅依赖于预训练的LLM。分析思维规模、搜索预算和性能之间的相互作用也是一个未解的问题。

# 重点总结

+   Tree of Thoughts范式展示了如何将经典搜索技术与现代神经网络模型相结合。

+   允许LLM探索不同的推理路径使得它们的决策过程更具可解释性。

+   这一研究方向可能增强LLM在复杂现实世界规划和分析任务中的适用性。

+   关键的下一步是将这一方法扩展到更少约束的问题，提高搜索效率，并研究如何学习这些技能。

+   总体来说，Tree of Thoughts的深思熟虑和语义推理为人工智能代理提供了一种令人兴奋的新能力。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家，也是KDnuggets的总编辑，KDnuggets是一个开创性的在线数据科学和机器学习资源。他的兴趣领域包括自然语言处理、算法设计和优化、无监督学习、神经网络以及机器学习的自动化方法。Matthew拥有计算机科学硕士学位和数据挖掘研究生文凭。可以通过editor1 at kdnuggets[dot]com与他联系。

### 更多相关内容

+   [自动化思维链：AI如何自我提示进行推理](https://www.kdnuggets.com/2023/07/automating-chain-of-thought-ai-prompt-itself-reason.html)

+   [揭示大语言模型中的思维链提示的力量](https://www.kdnuggets.com/2023/07/power-chain-thought-prompting-large-language-models.html)

+   [构建视觉搜索引擎 - 第2部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [使用Python进行网格搜索和随机搜索的超参数调优](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [通过Uplimit的机器学习搜索课程提升你的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [用Python和Scikit-learn简化决策树的可解释性](https://www.kdnuggets.com/2017/05/simplifying-decision-tree-interpretation-decision-rules-python.html)
