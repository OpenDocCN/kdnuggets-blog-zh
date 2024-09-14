# 4 个你可能不知道的 Python Itertools 过滤函数

> 原文：[`www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html`](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)

![4 个你可能不知道的 Python Itertools 过滤函数](img/9ce8e2d289bae33f8ee329b84d5e8275.png)

图片由作者提供

在 Python 中，迭代器帮助你编写更具 Python 风格的代码，并在处理长序列时更高效。内置的[itertools](https://docs.python.org/3/library/itertools.html)模块提供了几个有用的函数来创建迭代器。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

当你只想遍历迭代器、检索序列中的元素并处理它们时，这些功能特别有用——而无需将它们存储在内存中。今天我们将学习如何使用以下四个 itertools 过滤函数：

+   filterfalse

+   takewhile

+   dropwhile

+   islice

让我们开始吧！

## 开始之前：关于代码示例的说明

在本教程中：

+   我们将讨论的四个函数都提供*迭代器*。为了清晰起见，我们将使用简单的序列，并使用`list()`来获取包含迭代器返回的所有元素的列表。但在处理长序列时，除非必要，否则应避免这样做，因为这样会失去迭代器带来的内存节省。

+   对于简单的谓词函数，你也可以使用*lambdas*。但为了更好的可读性，我们将定义常规函数并将其用作谓词。

# 1\. filterfalse

如果你已经编程 Python 有一段时间，你可能已经使用过内置的`filter`函数，语法如下：

```py
filter(pred,seq)
# pred: predicate function
# seq: any valid Python iterable 
```

`filter`函数返回一个迭代器，该迭代器从序列中返回谓词返回`True`的元素。

让我们来看一个例子：

```py
nums = list(range(1,11)) #[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

def is_even(n):
    return n % 2 == 0
```

在这里，`nums`列表和`is_even`函数分别是序列和谓词。

要获取`nums`中所有*偶数*的列表，我们使用如下的`filter`：

```py
nums_even = filter(is_even, nums)
print(list(nums_even))
```

```py
Output >>> [2, 4, 6, 8, 10]
```

现在让我们了解一下`filterfalse`。我们将从**itertools**模块导入`filterfalse`函数（以及我们将讨论的所有其他函数）。

正如名称所示，`filterfalse`的功能与`filter`函数相反。它返回一个迭代器，该迭代器返回谓词返回`False`的元素。以下是使用`filterfalse`函数的语法：

```py
from itertools import filterfalse
filterfalse(pred,seq)
```

函数 `is_even` 对 `nums` 中的所有奇数返回 `False`。所以使用 `filterfalse` 得到的 `nums_odd` 列表是 `nums` 中所有奇数的列表：

```py
from itertools import filterfalse

nums_odd = filterfalse(is_even, nums)
print(list(nums_odd)) 
```

```py
Output >>> [1, 3, 5, 7, 9]
```

# 2\. takewhile

使用 `takewhile` 函数的语法是：

```py
from itertools import takewhile
takewhile(pred,seq)
```

`takewhile` 函数返回一个迭代器，该迭代器在谓词函数返回 `True` 时返回元素。当谓词首次返回 `False` 时，它停止返回元素。

对于长度为 n 的序列，如果 `seq[k]` 是谓词函数首次返回 `False` 的元素，则迭代器返回 `seq[0]`、`seq[1]`、...、`seq[k-1]`。

考虑以下 `nums` 列表和谓词函数 `is_less_than_5`。我们按如下方式使用 `takewhile` 函数：

```py
from itertools import takewhile

def is_less_than_5(n):
    return n < 5

nums = [1, 3, 5, 2, 4, 6]
filtered_nums_1 = takewhile(is_less_than_5, nums)
print(list(filtered_nums_1)) 
```

在这里，谓词 `is_less_than_5` 对数字 5 首次返回 `False`：

```py
Output >>> [1, 3]
```

# 3\. dropwhile

从功能上讲，`dropwhile` 函数做的是 `takewhile` 函数的反向操作。

你可以这样使用 `dropwhile` 函数：

```py
from itertools import dropwhile
dropwhile(pred,seq) 
```

`dropwhile` 函数返回一个迭代器，该迭代器会不断丢弃元素，只要谓词为 `True`。这意味着迭代器*不会返回任何东西*，直到谓词第一次返回 `False`。一旦谓词返回 `False`，迭代器将返回序列中的*所有*后续元素。

对于长度为 n 的序列，如果 `seq[k]` 是谓词函数首次返回 `False` 的元素，则迭代器返回 `seq[k]`、`seq[k+1]`、...、`seq[n-1]`。

让我们使用相同的序列和谓词：

```py
from itertools import dropwhile

def is_less_than_5(n):
    return n < 5

nums = [1, 3, 5, 2, 4, 6]
filtered_nums_2 = dropwhile(is_less_than_5, nums)
print(list(filtered_nums_2)) 
```

因为谓词函数 `is_less_than_5` 对元素 5 首次返回 `False`，我们得到从 5 开始的所有序列元素：

```py
Output >>> [5, 2, 4, 6]
```

# 4\. islice

你可能已经熟悉了切片 Python 可迭代对象，如列表、元组和字符串。切片的语法是：`iterable[start:stop:step]`。

然而，这种切片方法有以下缺点：

+   当处理大型序列时，每个切片或子序列都是一个占用内存的副本。这可能会导致低效。

+   因为步长也可以是负值，使用开始、停止和步长值会影响可读性。

`islice` 函数解决了上述限制：

+   它返回一个迭代器。

+   它不允许步长为负值。

你可以这样使用 `islice` 函数：

```py
from itertools import islice
islice(seq,start,stop,step) 
```

下面是几种你可以使用 `islice` 函数的不同方式：

+   使用 `islice(seq, stop)` 会返回一个遍历切片 `seq[0]`、`seq[1]`、...、`seq[stop - 1]` 的迭代器。

+   如果你指定开始和停止值：`islice(seq, start, stop)`，函数返回一个遍历切片 `seq[start]`、`seq[start + 1]`、...、`seq[start + stop - 1]` 的迭代器。

+   当你指定开始、停止和步长参数时，函数返回一个遍历切片 `seq[start]`、`seq[start + step]`、`seq[start + 2*step]`、...、`seq[start + k*step]` 的迭代器。使得 `start + k*step` < `stop` 且 `start + (k+1)*step` >= `stop`。

让我们以一个示例列表来更好地理解这一点：

```py
nums = list(range(10)) #[0,1, 2, 3, 4, 5, 6, 7, 8, 9]
```

现在让我们使用我们已经学到的语法来使用 `islice` 函数。

## 仅使用结束值

让我们只指定结束索引：

```py
from itertools import islice

# only stop
sliced_nums = islice(nums, 5)
print(list(sliced_nums)) 
```

这是输出结果：

```py
Output >>> [0, 1, 2, 3, 4]
```

## 使用开始值和结束值

在这里，我们使用了开始值和结束值：

```py
# start and stop
sliced_nums = islice(nums, 2, 7)
print(list(sliced_nums))
```

切片从索引 2 开始，扩展到但不包括索引 7：

```py
Output >>> [2, 3, 4, 5, 6]
```

## 使用开始值、结束值和步长值

当我们使用开始值、结束值和步长值时：

```py
# using start, stop, and step
sliced_nums = islice(nums, 2, 8, 2)
print(list(sliced_nums)) 
```

我们得到一个从索引 2 开始的切片，扩展到但不包括索引 8，步长为 2（返回每第二个元素）。

```py
Output >>> [2, 4, 6]
```

# 总结

我希望这个教程帮助你理解 itertools 过滤函数的基础知识。你已经看到了一些简单的示例，以便更好地理解这些函数的工作原理。接下来，你可以学习生成器 生成器函数和生成器表达式如何作为高效的 Python 迭代器工作。

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是一位来自印度的开发人员和技术写作者。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过编写教程、操作指南、评论文章等方式，与开发者社区分享她的知识。

### 了解更多相关话题

+   [你不知道的低代码工具的 7 种用法](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [你不知道的关于 SAS 数据科学学院的 3 件事](https://www.kdnuggets.com/2022/07/sas-3-things-didnt-know-sas-academy-data-science.html)

+   [探索 Python 的 itertools 中的无限迭代器](https://www.kdnuggets.com/exploring-infinite-iterators-in-python-itertools)

+   [如何使用 Python 过滤数据](https://www.kdnuggets.com/2022/02/filter-data-python.html)

+   [5 个你可能不知道的 Pandas 绘图函数](https://www.kdnuggets.com/2023/02/5-pandas-plotting-functions-might-know.html)

+   [数据科学面试中你应该知道的五种 SQL 窗口函数](https://www.kdnuggets.com/2022/01/top-five-sql-window-functions-know-data-science-interviews.html)
