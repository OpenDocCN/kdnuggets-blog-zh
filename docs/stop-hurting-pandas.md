# 停止伤害你的 Pandas！

> 原文：[`www.kdnuggets.com/2020/04/stop-hurting-pandas.html`](https://www.kdnuggets.com/2020/04/stop-hurting-pandas.html)

评论

**由 [Pawel Rzeszucinski](https://www.linkedin.com/in/pawelrzeszucinski/) 提供，[Codewise](http://www.codewise.com/)**

![图示](img/47801c4858e2d8ce22b79bb7b046ad4b.png)

来源：维基媒体共享资源

Pandas 是几乎所有与 Python 相关的数据处理任务的王者。它已经存在了 12 年，尽管我们直到 2020 年 1 月才看到 1.0 版本的发布。使用 Pandas 进行数据操作、切片和更新非常直观，这可能是这个包从一开始就取得成功的原因。然而，尽管语法简单而一致，但在某些情况下需要特别小心，以确保准确实现意图。本文将探讨在不当使用 Pandas 切片时可能出现的问题。如果你看到警告“*A value is trying to be set on a copy of a slice from a DataFrame*”，那么这篇文章适合你。

Pandas 提供了清晰的规则来正确切片 DataFrames，详细概述可以在 这里 找到。然而，我们并不总是遵循最佳实践，因为这需要获得必要的知识并保持一定的自我严格。除了指导方针中概述的选项外，Pandas 还允许我们以多种不同的方式访问 DataFrames 元素。这可能会引发尝试以不当方式进行数据分配的诱惑，导致一些意想不到的效果。

让我们从定义一个简单的测试数据框开始：

```py` ``` df = pd.DataFrame({'x':[1,5,4,3,4,5],  				   'y':[.1,.5,.4,.3,.4,.5],  				   'w':[11,15,14,13,14,15]})       x    y   w  0  1  0.1  11  1  5  0.5  15  2  4  0.4  14  3  3  0.3  13  4  4  0.4  14  5  5  0.5  15 ```py ````

假设我们想找出所有 'x' 列大于 3 的 DataFrame 元素，并基于此将所有对应的 'y' 值改为 50。

如何根据 Pandas 的最佳实践正确执行这一操作？在这种情况下，使用 `.loc` 方法：

```py` ``` df.loc[df['x']>3,'y']=50 ```py ````

我们定位满足初始标准的行元素（第一个参数），以及我们想要更新的列（第二个参数），所有这些都在一次 DataFrame 调用中进行评估。

结果如预期。

```py` ```    x     y   w  0  1   0.1  11  1  5  50.0  15  2  4  50.0  14  3  3   0.3  13  4  4  50.0  14  5  5  50.0  15 ```py ````

正如之前提到的，Pandas 提供了多种不同的访问（但不一定是修改）数据的方法。

不遵循脚本（指导方针）可能会导致问题。例如，有些人可能更自然地将相同的操作写成以下形式：

```py` ``` df[df['x']>3]['y']=50 ```py ````

这很清楚，不是吗？取出 `df` 中 `'x'>3` 的子集，然后将列 'y' 中的值更改为 50。

我们来做一下：

```py` ```    x    y   w  0  1  0.1  11  1  5  0.5  15  2  4  0.4  14  3  3  0.3  13  4  4  0.4  14  5  5  0.5  15 ```py ````

我可能打错了什么，让我再运行一次。

```py` ``` df[df['x']>3]['y']=50       x    y   w  0  1  0.1  11  1  5  0.5  15  2  4  0.4  14  3  3  0.3  13  4  4  0.4  14  5  5  0.5  15 ```py ````

完全没有变化！为什么呢？

我们遇到了所谓的“链式索引”效应，即一个接一个地使用两个索引器，例如 `df[][]`

让我们分解一下我们的命令：

+   `df[df['x']>3]` 会导致 Pandas 创建一个原始 DataFrame 的独立副本。

+   `df[df['x']>3]['y'] = 50` 将新值分配给 'y' 列，但是在这个临时创建的副本上，而不是我们的原始 DataFrame 上。

观察这个情况的一种方法是使用 [基础的 'id' 函数](https://docs.python.org/3/library/functions.html#id)，它返回给定对象在机器内存中的地址。

```py` ``` id(df)  2838845867680    id(df[df['x']>3])  2838845989832    id(id) == id(df[df['x']>3])  False ```py ````

有趣的是，当我们反转命令中的切片顺序，即先调用列然后再使用我们想要满足的条件时，我们得到了预期的结果：

```py` ``` df['y'][df['x']>3]=50       x     y   w  0  1   0.1  11  1  5  50.0  15  2  4  50.0  14  3  3   0.3  13  4  4  50.0  14  5  5  50.0  15 ```py ````

这是因为，当我们只从 DataFrame 中选择一列时，Pandas 创建的是一个视图，而不是副本。

什么是视图？本质上，它是同一对象的代理，即在这个过程中不会创建新的对象。

```py` ``` z = df['y'] #z 是 df['y'] 的视图  id(df['y']) == id(z)  True ```py ````

尽管我们达到了目标，但可能会触发一些副作用：

让我们看看这组命令。

```py` ``` df # 原始 DataFrame     x    y   w  0  1  0.1  11  1  5  0.5  15  2  4  0.4  14  3  3  0.3  13  4  4  0.4  14  5  5  0.5  15    z = df['y'] # 'y' 列的视图  z[z>=0.5] = 30    z  0     0.1  1    30.0  2     0.4  3     0.3  4     0.4  5    30.0    df     x     y   w  0  1   0.1  11  1  5  30.0  15  2  4   0.4  14  3  3   0.3  13  4  4   0.4  14  5  5  30.0  15 ```py ````

哇！我们以为创建了一个与 `df` 独立的对象 'z'，在我们操作 'z' 时 `df` 的值是安全的。结果并非如此。

我们只创建了一个视图。好消息是 Pandas 会显示那个老旧的警告。

Pandas 这样做是因为它不知道我们是否想要仅仅改变 'y' 系列（通过代理 'z'），还是改变原始 `df` 的值。

好吧，那么如果我们想将 'z' 提取为一个独立的对象呢？Pandas 方法 `.copy()` 正是为了这个目的。

当我们将命令更新为下面所示的内容时，我们将创建一个具有自己内存地址的全新对象，任何对 'z' 的更新将不会影响 `df`。

```py` ``` z = df['y'].copy()  id(df['y']) == id(z)  False ```py ````

实际上有两个关键点可以防止在处理切片和数据操作时出现不希望的效果：

1.  避免链式索引。始终使用 `.loc`/`.iloc`（或 `.at`/`.iat`）选项：

1.  `copy()` 你的变量以创建独立的对象，并保护原始源不被意外修改。

**简介： [Pawel Rzeszucinski 博士](https://www.linkedin.com/in/pawelrzeszucinski/)** 是广泛定义的数据分析领域的 30 多篇出版物和专利的作者。他获得了克兰菲尔德大学的计算机科学硕士学位，随后转到曼彻斯特大学，在那里他获得了与 QinetiQ 相关的直升机齿轮箱诊断分析解决方案项目的博士学位。返回波兰后，他在 ABB 公司研究中心担任高级科学家，并在 HSBC 的战略分析部门担任高级风险建模师。他目前在 AdTech 公司 Codewise 担任首席数据科学家。Pawel Rzeszucinski 博士是 Forbes 技术委员会的成员。

**相关：**

+   [如何使用 [ ]、.loc、iloc、.at 和 .iat 在 Pandas 中选择行和列](/2019/06/select-rows-columns-pandas.html)

+   Python 数据分析… 真的那么简单吗？！?

+   使用 pdpipe 构建 Pandas 管道

* * *

## 我们的 3 个顶级课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

### 更多相关话题

+   [停止学习数据科学以寻找目标，并寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个 $9B 的 AI 失败，分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [学习数据科学统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每位数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
