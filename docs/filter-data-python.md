# 如何用 Python 过滤数据

> 原文：[https://www.kdnuggets.com/2022/02/filter-data-python.html](https://www.kdnuggets.com/2022/02/filter-data-python.html)

![如何用 Python 过滤数据](../Images/2f796a4aaf9eb4ff7a2018725b22b59a.png)

图片由 [Sid Balachandran](https://unsplash.com/@itookthose?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## **介绍**

尽管数据科学家可以利用 SQL，但坦率地说，用 Python 操作来处理 pandas 数据框可能会更简单（*或者，作为补充*）。我个人喜欢将这两种语言结合使用来构建我的数据。在某些情况下，当你已经有了从 SQL 查询得到的数据框后，使用操作会更高效。例如，你可能会查询所有必要的列，然后读取你的数据框，再应用相应的操作来整理数据，然后再将其导入到数据科学模型中。话虽如此，我们来深入了解一些简单的操作，可能会让你的日常工作变得更轻松。

## **小于/大于**

对于所有这些用例，我将有一个*假设的* pandas 数据框。

以下操作是“小于”，你可以写下你的数据框别名，在这个例子中就是 df。你可以插入我放置的‘column_1’处的列名。我分配了一个新的数据框，命名为 df_less_than_20，这样我只有列值小于 20 的记录/行。

```py
df_less_than_20 = df[df['column_1'] < 20]
```

同样的概念可以应用于“大于”：

```py
df_more_than_20 = df[df['column_1'] > 20]
```

尽管这些操作很简单，但它们仍然有用，当结合使用时，效果会更好——正如我们下面将看到的那样。

另一种看待这个特性的方式是像 SQL 中的 WHERE 子句一样。

## **与/或**

现在我们有了上述语句，我们可以对数据应用进一步的过滤。

我们可以使用两者之一，或 & 或 | 操作。

为了澄清：

+   AND = &

+   OR = |

我知道 AND 操作，但 OR 实际上是我最近发现的一个操作，它非常有用，特别是在你的模型运行后，用于准确性和错误分析的筛选数据。当然，你也可以在过程的前一步使用这个操作。

现在，我们可以以以下方式使用这两者中的任意一个或两个：

```py
df[(df['column_1'] >= -100) & (df['column_1'] <= 1000)]
```

上述内容表示，给我值在负100到正100之间的数据。

下一步是使用 OR 操作，找到所有负值的行：

```py
df[(df['column_1'] < 0) | (df['column_1'] >= -100) & (df['column_1'] <= 100)]
```

我们也可以去掉中间子句，创建以下代码片段：

```py
df[(df['column_1'] < 0) | (df['column_1'] <= 100)]
```

然而，我们可以用过滤另一列其他值的内容替换其中一个子句。

```py
df[(df['column_1'] < 0) | (df['column_2'] <= 50)]
```

### **等于/不等于**

最后，我们还有另一种方法来通过选择具有特定值或没有特定值的行来过滤数据。

这两种操作如下：

+   DOES EQUAL: ==

+   DOES NOT EQUAL: !=

这里有一些例子：

```py
df[df['column_1'] == 100]
df[df['column_2'] == 50]
df[df['column_3'] == 'blue']
df[df['column_3'] != 'blue']
df[(df['column_3'] != 'red' ) | (df['column_200'] <= 8.60)]
```

### **总结**

总结一下，我们看到我们可以将上述讨论的几个操作结合起来，以创建一个过滤的数据集或 pandas 数据框。最终，这种编码方式可能对一些数据科学家来说更容易，他们更喜欢在 Python 中工作而不是 SQL。

这些操作本身的总结如下：

+   较小/较大： < >

+   和/或： & |

+   等于/不等于： == !=

感谢阅读！希望你喜欢这篇文章并觉得有用。

## **参考资料**

[1] 图片由 [Sid Balachandran](https://unsplash.com/@itookthose?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/pandas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)，(2019)

**[Matthew Przybyla](https://www.linkedin.com/in/matthew-przybyla-0a095b31/)** ([Medium](https://datascience2.medium.com/)) 是位于德克萨斯州 Favor Delivery 的高级数据科学家。他拥有南方卫理公会大学的数据科学硕士学位。他喜欢撰写有关数据科学领域的趋势话题和教程，从新算法到关于数据科学家日常工作经验的建议。Matt 喜欢突出数据科学的商业方面，而不仅仅是技术方面。欢迎通过 [LinkedIn](https://www.linkedin.com/in/matthew-przybyla-0a095b31) 联系 Matt。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关主题

+   [4 个你可能不知道的 Python Itertools 过滤函数](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)

+   [通过《数据科学快速 Python》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [优化 Python 代码性能：深入了解 Python 分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)

+   [Python Enum：如何在 Python 中构建枚举](https://www.kdnuggets.com/python-enum-how-to-build-enumerations-in-python)

+   [掌握 SQL、Python、数据清理、数据… 的指南合集](https://www.kdnuggets.com/collection-of-guides-on-mastering-sql-python-data-cleaning-data-wrangling-and-exploratory-data-analysis)

+   [2020年数据科学、数据可视化与机器学习的38个顶级Python库](https://www.kdnuggets.com/2020/11/top-python-libraries-data-science-data-visualization-machine-learning.html)
