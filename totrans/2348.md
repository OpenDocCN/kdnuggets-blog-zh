# 机器学习过程的框架

> 原文：[https://www.kdnuggets.com/2018/05/general-approaches-machine-learning-process.html](https://www.kdnuggets.com/2018/05/general-approaches-machine-learning-process.html)

比较机器学习过程的方法是否值得？这些框架之间是否存在根本性的差异？

尽管 [经典方法](/2016/03/data-science-process-rediscovered.html) 存在且存在已久，但从新的和不同的角度咨询仍然值得，原因有很多：我是否遗漏了什么？是否有之前未考虑的新方法？我是否应该改变处理机器学习的视角？

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

我最近遇到的两个最相关的资源概述了处理机器学习过程的框架，分别是 Yufeng Guo 的 [机器学习的 7 个步骤](https://towardsdatascience.com/the-7-steps-of-machine-learning-2877d7e5548e) 和 Francois Chollet 的 [用 Python 深度学习](https://www.amazon.com/Deep-Learning-Python-Francois-Chollet/dp/1617294438) 第 4.5 节。这些是否与您目前处理类似任务的方式有所不同？

以下是这两种监督机器学习方法的概要、简要比较，以及尝试将它们融合成一个第三个框架，以突出（监督）机器学习过程中的最重要领域。

# 机器学习的 7 个步骤

实际上，我是通过先观看 [他的一个视频](https://youtu.be/nKW8Ndu7Mjw?list=PLIivdWyY5sqJxnwJhe3etaK7utrBiPBQ2) 在 YouTube 上发现郭的文章的。该帖子与视频的内容相同，因此如果有兴趣，任一资源都足够。

![图片](../Images/bb2013560dcb54a0a997f3e3533b3671.png)

[图片来源](https://towardsdatascience.com/the-7-steps-of-machine-learning-2877d7e5548e)

郭的步骤如下（我稍作了些即兴发挥）：

1.  数据收集

    → 数据的数量和质量决定了我们的模型准确性

    → 这一阶段的结果通常是数据的表示（郭将其简化为指定一个表），我们将用来进行训练

    → 使用预先收集的数据，例如来自 Kaggle、UCI 等的数据集，仍然符合这一步骤

1.  数据准备

    → 整理数据并为训练做好准备

    → 清理可能需要的内容（移除重复项、纠正错误、处理缺失值、标准化、数据类型转换等）

    → 随机化数据，这可以消除我们收集和/或准备数据时特定顺序的影响

    → 可视化数据以帮助检测变量之间的相关关系或类别不平衡（偏差警报！），或进行其他探索性分析

    → 划分为训练集和评估集

1.  选择模型

    → 不同的算法适用于不同的任务；选择合适的算法

1.  训练模型

    → 训练的目标是尽可能准确地回答问题或做出预测

    → 线性回归示例：算法需要学习*m*（或*W*）和*b*（*x*是输入，*y*是输出）

    → 每次过程迭代都是一个训练步骤

1.  评估模型

    → 使用某种度量或度量组合来“衡量”模型的客观性能

    → 在之前未见过的数据上测试模型

    → 这些未见数据旨在某种程度上代表模型在实际世界中的表现，但仍有助于调整模型（与测试数据不同，测试数据没有这样的作用）

    → 良好的训练/评估划分？80/20、70/30或类似的，取决于领域、数据可用性、数据集细节等

1.  参数调整

    → 这一步指的是*超参数*调整，这是一种“艺术形式”，而不是科学

    → 调整模型参数以提高性能

    → 简单的模型超参数可能包括：训练步骤数、学习率、初始化值和分布等

1.  进行预测

    → 使用进一步（测试集）数据，这些数据在此之前一直被保留在模型之外（且已知类别标签），用于测试模型；更好地近似模型在实际世界中的表现

# 机器学习的通用工作流程

在[他的书](https://www.amazon.com/Deep-Learning-Python-Francois-Chollet/dp/1617294438)的第4.5节中，Chollet概述了机器学习的通用工作流程，他将其描述为解决机器学习问题的蓝图：

> 这个蓝图将我们在本章中学到的概念联系起来：问题定义、评估、特征工程和抗过拟合。

这与上述郭的框架相比如何？让我们看看Chollet的7个步骤（记住，虽然没有明确说明专门针对它们，但他的蓝图是为*神经网络*的书籍而写的）：

1.  定义问题并组建数据集

1.  选择成功的衡量标准

1.  决定评估协议

1.  准备你的数据

1.  开发一个表现优于基线的模型

1.  扩展：开发一个过拟合的模型

1.  正则化你的模型并调整参数

![机器学习](../Images/628c051fa81b348e055c44294678c8d2.png)

来源：斯坦福大学Andrew Ng的机器学习课程

Chollet 的工作流程更高层次，更多关注于将模型从良好提升到卓越，而 Guo 的方法则更关注于从零开始到良好。虽然它并不会为了做到这一点而舍弃其他重要步骤，但其蓝图更强调超参数调整和[正则化](http://neuralnetworksanddeeplearning.com/chap3.html#overfitting_and_regularization)以追求卓越。这里的简化似乎是：

良好模型 → “过于优秀”的模型 → 缩减回“可泛化”的模型

# 制定简化框架

我们可以合理地得出结论，Guo 的框架勾勒出了一种“初学者”机器学习过程，更明确地定义了早期步骤，而 Chollet 的方法则更为高级，强调了模型评估的明确决策和机器学习模型的调整。这两种方法都同样有效，并没有本质上的不同；你可以将 Chollet 的方法叠加在 Guo 的方法之上，虽然两个模型的7个步骤不完全对齐，但它们最终会覆盖相同的任务。

将 Chollet 的方法与 Guo 的方法对照，这里是我看到步骤对接的地方（Guo 的步骤编号，Chollet 的步骤则列在相应的 Guo 步骤下方，Chollet 工作流程步骤的编号在括号中）：

1.  数据收集

    → 定义问题并组装数据集（1）

1.  数据准备

    → 准备你的数据（4）

1.  选择模型

1.  训练模型

    → 开发一个优于基线的模型（5）

1.  评估模型

    → 选择成功的衡量标准（2）

    → 决定评估协议（3）

1.  参数调整

    → 扩展：开发一个过拟合的模型（6）

    → 对你的模型进行正则化和参数调整（7）

1.  预测

这并不完美，但我坚持这样做。

在我看来，这提出了一个重要的问题：这两个框架达成了一致，并共同强调了框架中的特定要点。应该明确的是，模型评估和参数调整是机器学习的重要方面。其他被一致认可的重要领域还包括数据的组装/准备以及原始模型的选择/训练。

# 一个简化的机器学习过程

让我们利用上述内容来组成一个简化的机器学习框架，即**机器学习过程的5个主要领域**：

1.  数据收集与准备

    → 从选择数据来源到数据清理并准备进行特征选择/工程的整个过程

1.  特征选择和特征工程

    → 这包括从数据清理完成后到数据被输入机器学习模型之间的所有变化

1.  选择机器学习算法并训练我们的第一个模型

    → 获得一个“优于基线”的结果，基于此我们可以（希望）进行改进

1.  评估我们的模型

    → 这包括度量的选择以及实际评估；看似比其他步骤小，但对我们的最终结果很重要。

1.  模型调整、正则化和超参数调优

    → 这是我们从“足够好”的模型逐步过渡到最佳努力的过程。

那么，你应该使用哪个框架？是否真的存在重要的差异？郭和肖莱特所介绍的框架是否提供了之前所缺少的东西？这个简化的框架是否提供了实际的好处？只要涵盖了基础，并且处理了框架之间明确存在的任务，那么选择这两个模型中的任何一个，结果都会相同。你的视角或经验水平可能会偏好其中一个。

正如你可能已经猜到的，这实际上更多的是关于一个合理的机器学习过程**应该**是什么样子，而不是决定或比较特定框架。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家以及 KDnuggets 的主编，KDnuggets 是一个重要的数据科学和机器学习在线资源。他的兴趣包括自然语言处理、算法设计与优化、无监督学习、神经网络以及自动化机器学习方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系到。

### 了解更多相关话题

+   [PyTorch 还是 TensorFlow？比较流行的机器学习框架](https://www.kdnuggets.com/2022/02/packt-pytorch-tensorflow-comparing-popular-machine-learning-frameworks.html)

+   [开发安全、可靠和可信的人工智能框架的专家见解](https://www.kdnuggets.com/expert-insights-on-developing-safe-secure-and-trustworthy-ai-frameworks)

+   [可视化框架类型](https://www.kdnuggets.com/types-of-visualization-frameworks)

+   [Chip Huyen 分享了实施机器学习系统的框架和案例研究](https://www.kdnuggets.com/2023/02/sphere-chip-huyen-shares-frameworks-case-studies-implementing-ml-systems.html)

+   [2023年你应该考虑的顶级 AutoML 框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)

+   [如何在几秒钟内处理包含百万行的数据框](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)
