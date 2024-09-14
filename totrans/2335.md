# 从零开始的机器学习：决策树

> 原文：[https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html](https://www.kdnuggets.com/2022/11/machine-learning-scratch-decision-trees.html)

![v](../Images/36ac427f5a02fcb2b5f248c0eecb8ace.png)

图片来源于 [Pexel](https://www.pexels.com/photo/leafless-tree-on-grass-field-1102909/)

决策树是机器学习世界中最简单的非线性监督算法之一。顾名思义，它们用于在ML术语中进行决策，我们称之为分类（尽管它们也可以用于回归）。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT需求

* * *

决策树具有单向树结构，即在每个节点上，算法根据某些停止标准做出决策以分裂成子节点。决策树通常使用熵、信息增益、基尼指数等。

决策树中有一些已知的算法，如ID3、C4.5、CART、C5.0、CHAID、QUEST、CRUISE。在这篇文章中，我们将讨论最简单且最古老的一个：ID3。

**ID3**，即Iterative Dichotomiser，是Ross Quinlan开发的三种决策树实现中的第一个。

算法以自上而下的方式构建树，从一组行/对象和特征规范开始。在树的每个节点上，根据最小化熵或最大化信息增益来测试一个特征以分裂对象集。这个过程会持续进行，直到某个节点中的集合是同质的（即节点包含相同类别的对象）。该算法使用贪婪搜索。它使用信息增益标准选择测试，然后从不探索替代选择的可能性。

缺点：

+   模型可能存在过拟合的情况。

+   仅适用于分类特征

+   不处理缺失值

+   低速

+   不支持剪枝

+   不支持提升

既然一个算法有这么多缺点，我们为什么还要讨论它呢？

答：这很简单，非常适合培养对树算法的直觉。

# 让我们通过一个例子来理解

今天的示例是最受欢迎的童谣之一，其中雨决定Johny/Arthur是否会在外面玩。唯一的变化是不仅仅是雨，任何恶劣天气都会影响孩子的玩耍，我们将使用决策树来预测他是否会在外面。

![从零开始的机器学习：决策树](../Images/372d0781819ea8688ab43a50782ecf20.png)

来源：维基百科

数据如下所示：

![从零开始的机器学习：决策树](../Images/2c58f39b5313bb05eaab4d9000b15892.png)

‘Temperature’，‘WindSpeed’，‘Outlook’和‘Humidity’是特征，而‘Play’是目标变量。只有分类数据且没有缺失值意味着我们可以使用ID3。

在深入算法之前，我们先了解一下划分标准。为了简化起见，我们将仅讨论二分类情况的每个标准。

**熵：** 这用于计算样本的异质性，其值在0和1之间。如果样本是同质的，则熵为0，如果样本中所有类别的比例相同，则熵为1。

```py
S = -(p * log₂p + q * log₂q)
```

`p`和`q`是样本中两个类别的各自比例。

这也可以写作：`S = -(p * log₂p + (1-p)* log₂(1-p))`

![从零开始的机器学习：决策树](../Images/a7189f79ab54740aa38045d1954de6fd.png)

**信息增益：** 它是节点的熵与子节点所有值的平均熵之间的差异。选择信息增益最大的特征进行拆分。

# 让我们通过示例来理解这一点

根节点的熵：（9 — Yes 和 5 — No）

```py
S = -(9/14)*log(9/14) — (5/14) * log(5/14) = 0.94
```

根节点有4种可能的拆分方式。（‘Temperature’，‘WindSpeed’，‘Outlook’，和‘Humidity’）。因此，如果选择上述任意一个，我们将计算子节点的加权平均熵。

```py
I(Temperature) = Hot*E(Temperature=Hot) + Mild*E(Temperature=Mild) + Cool*E(Temperature=Cool)
```

其中，Hot，Mild和Cool代表数据中三个值的比例。

```py
I(Temperature) = (4/14)*1 + (6/14)*0.918 + (4/14)*0.811 = 0.911
```

这里，每个值的熵通过使用特征的值过滤样本，然后使用熵的公式来计算。例如，E(Temperature=Hot) 是通过过滤原始样本中温度为Hot的样本来计算的（在这种情况下，我们有相等数量的Yes和No，这意味着熵等于1）。

![从零开始的机器学习：决策树](../Images/2faaecf786f9881367c444e38303a93b.png)

我们通过从根节点的熵中减去温度的平均熵来计算温度划分的信息增益。

```py
G(Temperature) = S — I(Temperature) = 0.94–0.911 = 0.029
```

同样，我们计算所有四个特征的增益，并选择增益最大的一项。

```py
G(WindSpeed) = S — I(WindSpeed) = 0.94–0.892 =0.048
```

```py
G(Outlook) = S — I(Outlook) = 0.94–0.693 =0.247
```

```py
G(Humidity) = S — I(Humidity) = 0.94–0.788 =0.152
```

Outlook具有最大的 信息增益，因此我们将在Outlook上拆分根节点，每个子节点代表按Outlook的某个值（即Sunny，Overcast和Rainy）过滤的样本。

现在，我们将重复相同的过程，将新形成的节点视为根节点，使用过滤样本计算每个节点的熵，并通过计算每个进一步拆分的平均熵并从当前节点熵中减去来获得 信息增益。请注意，ID3不允许使用已使用的特征来拆分子节点。因此，每个特征在树中只能用于一次拆分。

这是通过所有拆分形成的最终树：

![从零开始的机器学习：决策树](../Images/40ba6d18ea1dde3426503c175f884f32.png)

使用 Python 代码的简单实现可以在[这里](https://www.kaggle.com/ankitmalik/decision-trees-from-scratch-id3)找到

## 结论

我尽力解释了 ID3 的工作原理，但我知道你可能还有问题。请在评论中告诉我，我会很高兴回答。

谢谢阅读！

**[安基特·马利克](https://www.linkedin.com/in/ankitmalikds/)** 正在多个领域（如市场营销、供应链、零售、广告和流程自动化）构建可扩展的 AI/ML 解决方案。他曾在财富 500 强公司领导数据科学项目，也曾是多个初创公司数据科学孵化器的创始成员。他开创了各种创新产品和服务，并且相信服务型领导。

[原文](https://ai.plainenglish.io/ml-from-scratch-decision-trees-da68cdaa13bc)。经授权转载。

### 更多相关内容

+   [决策树与随机森林的解释](https://www.kdnuggets.com/2022/08/decision-trees-random-forests-explained.html)

+   [通用且可扩展的最优稀疏决策树（GOSDT）](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

+   [揭开决策树在现实世界中的神秘面纱](https://www.kdnuggets.com/demystifying-decision-trees-for-the-real-world)

+   [用 NumPy 从头实现线性回归](https://www.kdnuggets.com/linear-regression-from-scratch-with-numpy)

+   [如何从头构建和训练一个 Transformer 模型……](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [决策树算法的解释](https://www.kdnuggets.com/2020/01/decision-tree-algorithm-explained.html)
