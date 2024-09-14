# 如何让 Python 代码运行得极快

> 原文：[https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html](https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html)

![如何让 Python 代码运行得极快](../Images/cf09bbaf063f4212ca4ecc13d7713603.png)

[由 brgfx 制作的图片](https://www.freepik.com/free-vector/young-boy-laptop-python-concept_3576656.htm#query=Python&position=5&from_view=search&track=sph) 在 Freepik 上

Python 是开发者中最受欢迎的编程语言之一。无论是网页开发还是机器学习，它都无处不在。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

其流行有很多原因，例如社区支持、惊人的库、在机器学习和大数据中的广泛使用以及简易的语法。

尽管有这些优点，Python 仍然有一个缺点，就是其 *速度较慢*。作为一种解释型语言，Python 比其他编程语言要慢。不过，我们可以通过一些技巧来克服这个问题。

在本文中，我将分享一些 Python 技巧，利用这些技巧，我们可以让 Python 代码运行得比平时更快。让我们开始吧！

# 1\. 合适的算法和数据结构

每种数据结构对运行时间都有显著影响。Python 中有许多内置数据结构，如列表、元组、集合和字典。大多数人使用列表数据结构来处理所有情况。

在 Python 中，集合和字典具有 O(1) 的查找性能，因为它们使用哈希表。你可以在以下情况下使用集合和字典代替列表：

+   你在集合中没有重复项。

+   你需要在集合中重复搜索项目。

+   该集合包含大量项目。

你可以在这里查看不同数据结构的时间复杂度：

[**通过 Python Wiki 了解时间复杂度**](https://wiki.python.org/moin/TimeComplexity)

本页面记录了当前 CPython 中各种操作的时间复杂度（又称“Big O”或“Big Oh”）...

# 2\. 使用内置函数和库

Python 的内置函数是加速代码的最佳方法之一。你必须在需要时使用内置 Python 函数。这些内置函数经过良好测试和优化。

这些内置函数之所以快，是因为 Python 的内置函数，如 min、max、all、map 等，是用 C 语言实现的。

你应该使用这些内置函数，而不是编写手动函数，这将帮助你更快地执行代码。

示例：

```py
newlist = []

for word in wordlist:
    newlist.append(word.upper())
```

更好的代码写法是：

```py
newlist = map(str.upper, wordlist)
```

这里我们使用了内置的 **map** 函数，它是用 C 编写的。因此，它比使用循环要快得多。

# 3\. 使用多重赋值

如果你想赋值多个变量，那么不要逐行赋值。Python 有一种优雅且更好的方式来赋值多个变量。

示例：

```py
firstName = "John"
lastName = "Henry"
city = "Manchester"
```

更好的变量赋值方式是：

```py
firstName, lastName, city = "John", "Henry", "Manchester"
```

这种变量赋值方式比上述方法更简洁优雅。

# 4\. 优先使用列表推导而非循环

列表推导是一种优雅且更好的方式，可以基于现有列表的元素在一行代码中创建新列表。

列表推导被认为是一种比定义空列表并向其添加元素更具 Python 风格的创建新列表的方式。

列表推导的另一个优点是比使用追加方法向 Python 列表添加元素更快。

示例：

使用列表追加方法：

```py
newlist = []
for i in range(1, 100):
    if i % 2 == 0:
        newlist.append(i**2)
```

使用列表推导的更好方式：

```py
newlist = [i**2 for i in range(1, 100) if i%2==0]
```

使用列表推导时代码更简洁。

# 5\. 正确导入

应避免在不需要时导入不必要的模块和库。你可以指定模块名，而不是导入整个库。

导入不必要的库会导致代码性能下降。

示例：

假设你需要计算一个数的平方根。代替这样：

```py
import math
value = math.sqrt(50)
```

使用这个：

```py
from math import sqrt
value = sqrt(50)
```

# 6\. 字符串连接

在 Python 中，我们使用‘+’运算符连接字符串。但在 Python 中连接字符串的另一种方式是使用 **join** 方法。

Join 方法是一种更具 Python 风格的字符串连接方式，并且比使用‘+’运算符连接字符串更快。

join() 方法更快的原因是‘**+**’运算符每一步都会创建一个新字符串并复制旧字符串，而 join() 方法并不会这样工作。

示例：

```py
output = "Programming" + "is" + "fun
```

使用 join 方法：

```py
output = " ".join(["Programming" , "is", "fun"])
```

两种方法的输出结果是相同的。唯一的区别是 join() 方法比‘+’运算符更快。

# 结论

这篇文章到此为止。在这篇文章中，我们讨论了一些可以使代码运行更快的技巧。这些技巧特别适用于竞争编程，其中时间限制至关重要。

希望你喜欢这篇文章。感谢阅读！

**[Pralabh Saxena](https://www.linkedin.com/in/pralabh-saxena-7a82b5124/)** 是一名拥有 1 年经验的软件开发人员。Pralabh [撰写文章](https://pralabhsaxena.medium.com/) 涉及 Python、机器学习、数据科学和 SQL 等主题。

### 更多相关内容

+   [开始使用 PyTest：轻松编写和运行 Python 测试](https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python)

+   [通过《数据科学中的快速 Python》提升你的 Python 技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [如何简化代码文档编写](https://www.kdnuggets.com/2022/12/make-documenting-code-easier.html)

+   [使用 Jupysql 和 GitHub Actions 安排和运行 ETL](https://www.kdnuggets.com/2023/05/schedule-run-etls-jupysql-github-actions.html)

+   [了解如何在几步之内在你的设备上运行 Alpaca-LoRA](https://www.kdnuggets.com/2023/05/learn-run-alpacalora-device-steps.html)

+   [使用 LM Studio 本地运行 LLM](https://www.kdnuggets.com/run-an-llm-locally-with-lm-studio)
