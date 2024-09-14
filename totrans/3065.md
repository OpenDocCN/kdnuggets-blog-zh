# 使用米切尔范式对学习算法的简明解释

> 原文：[https://www.kdnuggets.com/2018/10/mitchell-paradigm-concise-explanation-learning-algorithms.html](https://www.kdnuggets.com/2018/10/mitchell-paradigm-concise-explanation-learning-algorithms.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

> 如果一个计算机程序在一些任务类别*T*和性能测量*P*方面，随着经验*E*的增加，其在*T*中任务的性能（由*P*衡量）有所提升，则称该程序从经验*E*中学习。
> 
> - Tom Mitchell, "机器学习"¹

Tom Mitchell的引言在机器学习界广为人知并经过时间考验，首次出现在他1997年的书中。这个句子对我个人产生了很大的影响，我在多年来多次引用了它，并在我的硕士论文中提到过。这个引言在Goodfellow, Bengio & Courville的《深度学习》第五章中也占据了重要位置，为书中对学习算法的解释提供了起点。

尽管本质上抽象，但变量*E*、*T*和*P*可以映射到机器学习算法及其学习过程中，以帮助加深对学习算法的抽象理解，甚至更具体地理解。让我们看看如何从这些简洁的几个词中获得大量的信息。

![Image](../Images/dc620e17165be2135838c86857032359.png)

**图 1**. 视觉化的米切尔范式。

> 机器学习任务通常是根据机器学习系统应如何处理一个**示例**来描述的。
> 
> - Ian Goodfellow, Yoshua Bengio & Aaron Courville, "深度学习"²

首先，让我们记住我们这里关注的是机器学习算法和**学习过程**。这意味着我们必须区分我们的**任务**和学习过程，并理解它们之间的关系。

比如，如果我们想对图像进行分类，那么区分执行我们的任务（图像分类）和学习如何执行我们的任务非常重要，这可以被描述为“我们获得执行任务能力的方法”²。单张图像就是我们机器学习系统将处理的示例，回到上面的引用。

其他任务的示例包括回归、翻译、异常检测和密度估计。

> 为了评估机器学习算法的能力，我们必须设计一个定量的性能衡量标准。
> 
> - Ian Goodfellow, Yoshua Bengio & Aaron Courville, "深度学习"²

在学习过程中，我们必须确定我们的示例是否使我们更接近最终的任务目标；我们需要**衡量其性能**。换句话说，在我们的示例中，我们从之前处理的示例中学到的知识是否使我们能够更好地分类图像？

幸运的是，我们在这种情况下想要了解的可以量化和轻松测量：在分类的情况下是准确性或反向的错误率。有时，知道要测量什么很困难；知道要测量什么但无法测量也是一个潜在的问题。

为了最佳地测量性能，我们通常感兴趣的是评估那些之前未被我们的学习算法看到的数据，这就是训练数据集和测试数据集概念发挥作用的地方。

> 机器学习算法可以大致分为**无监督**或**监督**，这取决于它们在学习过程中允许拥有何种经验。
> 
> - Ian Goodfellow, Yoshua Bengio & Aaron Courville, "深度学习"²

**经验**的概念比其他概念稍微抽象一些，很难指向某个具体的东西（例如，某个实现模型中的特定代码行）来体现它。

广义上，经验与学习算法如何从中学习的数据有关。我们可以从数据是否被标记（监督学习）或未标记（无监督学习）的角度来描述这种经验。在这个意义上，经验就是关于学习过程如何进行的，比如：这是自我学习的形式，还是一个有指导的过程？

我们还可以将数据集作为经验的一部分更全面地描述。通常，数据被打包成矩阵，这些矩阵是示例的集合。示例则是特征的集合，这些特征描述了一个特定的示例。这个特征集可能会被标签（或目标）补充，具体取决于它是否用于监督学习或无监督学习。

![图片](../Images/b7637382d8bbc451a472ebbbffe9b730.png)

**图 2**. 使用 Mitchell 模式来解释图像分类任务。

关于这个主题的更多信息，你可以在下面找到的参考书籍中自由获取。

**参考文献：**

1.  [机器学习](http://www.cs.cmu.edu/afs/cs.cmu.edu/user/mitchell/ftp/mlbook.html)，Tom Mitchell，麦Graw-Hill，1997年。

1.  [深度学习](https://www.deeplearningbook.org/)，Ian Goodfellow, Yoshua Bengio & Aaron Courville，MIT出版社，2016年。

**相关：**

+   [Keras 4步工作流](/2018/06/keras-4-step-workflow.html)

+   [接近机器学习过程的框架](/2018/05/general-approaches-machine-learning-process.html)

+   [接近文本数据科学任务的框架](/2017/11/framework-approaching-textual-data-tasks.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 领域

* * *

### 更多相关话题

+   [线性回归与逻辑回归：简明解释](https://www.kdnuggets.com/2022/03/linear-logistic-regression-succinct-explanation.html)

+   [KDnuggets 新闻 22:n12，3月23日：最佳数据科学书籍](https://www.kdnuggets.com/2022/n12.html)

+   [协作过滤的直观解释](https://www.kdnuggets.com/2022/09/intuitive-explanation-collaborative-filtering.html)

+   [探索数据网格：数据架构的范式转变](https://www.kdnuggets.com/exploring-data-mesh-a-paradigm-shift-in-data-architecture)

+   [思想图谱：大规模语言模型中复杂问题解决的新范式](https://www.kdnuggets.com/graph-of-thoughts-a-new-paradigm-for-elaborate-problem-solving-in-large-language-models)

+   [机器学习中主要的监督学习算法](https://www.kdnuggets.com/2022/06/primary-supervised-learning-algorithms-used-machine-learning.html)
