# 如何从机器学习中的巨大数据集中正确选择样本

> 原文：[https://www.kdnuggets.com/2019/05/sample-huge-dataset-machine-learning.html](https://www.kdnuggets.com/2019/05/sample-huge-dataset-machine-learning.html)

![如何从机器学习中的巨大数据集中正确选择样本](../Images/09979ee53c1210a7e83d719a441a6d55.png)

照片由 [Lukas](https://www.pexels.com/@goumbik?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) 提供，来自 [Pexels](https://www.pexels.com/photo/analytics-blur-close-up-commerce-590020/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

在机器学习中，我们常常需要用一个**非常大的**数据集来训练模型。数据集的规模越大，它的**统计意义**和包含的信息就越多，但我们很少问自己：这么大的数据集**真的有用**吗？或者我们能否用一个更小、更易管理的数据集达到令人满意的结果？选择一个适度小的数据集，并包含足够的信息，真的可以让我们**节省时间**和金钱。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT领域

* * *

让我们做一个简单的心理实验。假设我们在一个**图书馆**里，想要逐字学习但丁·阿利吉耶里的 *神曲*。

我们有两个选择：

1.  获取我们找到的第一个版本并开始学习

1.  尽可能多地获取不同版本并从中学习

正确的答案很明确。为什么我们要从不同的书籍中学习，当**仅一本**书就足够呢？

机器学习也是如此。我们有一个**学习东西**的模型，和我们一样，它也需要一些时间。我们想要的是学习现象所需的**最小信息量**，以免浪费时间。信息冗余对我们没有任何商业价值。

但是我们如何确保我们的版本没有损坏或不完整呢？我们必须与其他版本的集合进行某种形式的**高水平比较**。例如，我们可以检查 *canti* 和 *cantiche* 的数量。如果我们的书有三本 *cantiche*，每本都有33个 *canti*，也许它是完整的，我们可以安全地从中学习。

我们正在做的是从一个样本（单一的 *神曲* 版本）中学习，并检查其**统计意义**（与其他书籍的宏观比较）。

相同的概念也可以应用于机器学习。我们可以通过对大量记录的总体进行**子采样**，保持所有**统计信息不变**，而不是直接从庞大的总体中学习。

# 统计框架

为了得到一个小而易于处理的数据集，我们必须确保**不会丧失**相对于总体的统计显著性。数据集过小无法提供足够的信息进行学习，而数据集过大则可能需要耗费大量时间进行分析。那么，我们如何在大小和信息之间找到合适的**折中**呢？

从统计角度来看，我们希望样本保持总体的**概率分布**在合理的**显著性水平**下。换句话说，如果我们查看样本的**直方图**，它必须与总体的直方图相同。

实现这一目标有许多方法。最简单的方法是随机抽取一个具有**均匀分布**的子样本，并检查其是否显著。如果它具有合理的显著性，我们就保留它。如果不显著，我们将再取一个样本并重复这一过程，直到获得良好的显著性水平。

# 多变量与多个单变量

如果我们有一个由*N*个变量组成的数据集，它可以聚合成一个*N*变量的直方图，任何我们从中提取的子样本也可以是这样。

尽管这一操作在学术上是正确的，但在实际操作中可能非常困难，特别是当我们的数据集混合了数值变量和分类变量时。

所以我更喜欢一种更简单的方法，这通常会引入一个可接受的近似值。我们将考虑每个变量**独立于**其他变量。如果样本列的每一个单变量直方图与总体列的相应直方图**可比**，我们可以**假设**样本是**不偏倚的**。

样本和总体之间的比较是这样进行的：

1.  从样本中选取一个变量

1.  比较它的概率分布与总体中同一变量的概率分布

1.  对所有变量重复此过程

你们中的一些人可能认为我们忽略了变量之间的**相关性**。在我看来，这不完全正确，如果我们**均匀**地选择样本。众所周知，均匀选择子样本在数量足够大的情况下，会产生与原始总体**相同的概率分布**。像**自助法**这样强大的重采样方法就是基于这一概念的（有关更多信息，请参见我的[上一篇文章](https://medium.com/data-science-journal/the-bootstrap-the-swiss-army-knife-of-any-data-scientist-acd6e592be13)）。

# 比较样本和总体

正如我之前所说的，对于每个变量，我们必须将其在样本中的概率分布与在总体中的概率分布进行比较。

分类变量的直方图可以使用**Pearson’s chi-square test** 进行比较，而数值变量的累计分布函数可以使用**Kolmogorov-Smirnov test** 进行比较。

两个统计测试都基于原假设，即样本具有**与总体相同的分布**。由于一个样本由许多列组成，我们希望所有列都**具有显著性**，因此如果**至少一个**测试的 p 值低于通常的**5% 置信水平**，我们可以拒绝原假设。换句话说，我们希望**每一列**都通过显著性测试，以接受样本为有效。

# R 示例

让我们从理论进入实践。像往常一样，我将使用 R 语言中的一个例子。我将展示统计测试如何**给我们警告**，当采样不正确时。

## 数据模拟

让我们模拟一些（巨大的）数据。我们将创建一个包含 100 万条记录和 2 列的数据框。第一列有 500,000 条记录，取自正态分布，而另一列的 500,000 条记录取自均匀分布。这个变量是**明显有偏倚的**，它将帮助我解释统计显著性的概念。

另一个字段是通过使用前 10 个字母创建的**因子变量**，这些字母均匀分布。

以下是创建这样一个数据集的代码。

```py
set.seed(100)
N = 1e6
dataset = data.frame(
  # x1 variable has a bias. The first 500k values are taken
  # from a normal distribution, while the remaining 500k
  # are taken from a uniform distribution
  x1 = c(
    rnorm(N/2,0,1) ,
    runif(N/2,0,1)
    ),

  # Categorical variable made by the first 10 letters
  x2 = sample(LETTERS[1:10],N,replace=TRUE)
)
```

## 创建一个样本并检查其显著性

现在我们可以尝试从原始数据集中创建一个包含 10,000 条记录的样本，并检查其显著性。

记住：数值变量必须通过**Kolmogorov-Smirnov** 检验，而分类变量（即 R 中的因素）需要**Pearson’s chi-square** 检验。

对于每个测试，我们会将其 p 值存储在一个命名列表中以便最终检查。如果**所有的 p 值**都大于 5%，我们可以说样本没有偏倚。

```py
sample_size = 10000
set.seed(1)
idxs = sample(1:nrow(dataset),sample_size,replace=F)
subsample = dataset[idxs,]
pvalues = list()
for (col in names(dataset)) {
  if (class(dataset[,col]) %in% c("numeric","integer")) {
    # Numeric variable. Using Kolmogorov-Smirnov test

    pvalues[[col]] = ks.test(subsample[[col]],dataset[[col]])$p.value

  } else {
    # Categorical variable. Using Pearson's Chi-square test

    probs = table(dataset[[col]])/nrow(dataset)
    pvalues[[col]] = chisq.test(table(subsample[[col]]),p=probs)$p.value

  }
}

pvalues
```

p 值如下：

![如何从机器学习中的巨大数据集中正确选择样本](../Images/436dc733ebd7412f7383722e384d8945.png)

它们中的每一个都**大于 5%**，所以我们可以说样本在统计上是显著的。

如果我们取前 10,000 条记录而不是随机选择，会发生什么？我们知道数据集中 X1 变量的前半部分与总体的分布不同，因此我们期望这样的样本不能代表整个总体。

如果我们重复测试，这些是 p 值：

![如何从机器学习中的巨大数据集中正确选择样本](../Images/3de7151fc14e6a869ec2ec3b2d63cd60.png)

如预期的那样，由于总体的偏倚，X1 的 p 值过低。在这种情况下，我们必须**不断生成**随机样本，直到所有 p 值都大于允许的最小置信水平。

# 结论

在这篇文章中，我展示了一个合适的样本在统计上可以显著地代表整个群体。这可能对机器学习有所帮助，因为一个小数据集可以让我们**更快地**训练模型，而信息量与较大的数据集相同。

然而，一切都与我们选择的**显著性水平**密切相关。对于某些问题，提高置信水平或丢弃那些不显示合适p值的变量可能会很有用。像往常一样，训练前的适当数据发现可以帮助我们决定如何正确地进行样本。

[原文](https://medium.com/data-science-journal/how-to-correctly-select-a-sample-from-a-huge-dataset-in-machine-learning-24327650372c)。经授权转载。

## 资源

+   [在线和基于网页的：分析、数据挖掘、数据科学、机器学习教育](https://www.kdnuggets.com/education/online.html)

+   [分析、数据科学、数据挖掘和机器学习的软件](https://www.kdnuggets.com/software/index.html)

**[詹卢卡·马拉托](http://www.gianlucamalato.it/)**是一位作家和博主，但最重要的是一位充满热情的读者。詹卢卡写作奇幻、恐怖和科幻小说和短篇故事。

### 更多相关主题

+   [如何在Pandas中选择行和列使用[ ], .loc, iloc, .at…](https://www.kdnuggets.com/2019/06/select-rows-columns-pandas.html)

+   [如何为机器学习创建数据集](https://www.kdnuggets.com/2022/02/create-dataset-machine-learning.html)

+   [无监督解缠表示学习中的类别…](https://www.kdnuggets.com/2023/01/unsupervised-disentangled-representation-learning-class-imbalanced-dataset-elastic-infogan.html)

+   [为您的数据集选择正确的聚类算法](https://www.kdnuggets.com/2019/10/right-clustering-algorithm.html)

+   [如何生成合成表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [ChatGPT驱动的数据探索：揭示数据集中的隐藏洞察](https://www.kdnuggets.com/2023/07/chatgptpowered-data-exploration-unlock-hidden-insights-dataset.html)
