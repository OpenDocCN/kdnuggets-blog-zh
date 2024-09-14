# Python 序列的 5 个高级技巧

> 原文：[https://www.kdnuggets.com/2021/11/5-advanced-tips-python-sequences.html](https://www.kdnuggets.com/2021/11/5-advanced-tips-python-sequences.html)

[评论](#comments)

**作者 [Michael Berk](https://www.linkedin.com/in/michael-berk-48783a146/)，Tubi 的数据科学家**

![](../Images/a192e1567eb76d4791fcee7218864b73.png)

图片来源：[NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

> “66%的数据科学家每天都在使用 Python。” — [src](https://www.dasca.org/world-of-big-data/article/top-6-programming-languages-for-data-science-in-2021)

如果你是那66%中的一员，这篇文章就是为你准备的。

我们将涵盖[Luciano Ramalho 的《流畅的 Python》](https://www.amazon.com/Fluent-Python-Concise-Effective-Programming/dp/1491946008)第二章中的主要内容，该章节涉及**序列**，例如列表、元组等。

## 1 — 列表与元组

**提示：列表应保存相同类型的信息，而元组可以保存不同类型的信息。**

从基础开始，让我们讨论一下列表和元组之间的主要区别。下面我们可以看到每种的示例——列表用方括号`[]`括起来，而元组用圆括号`() `括起来。

```py
my_tuple = (1,'a',False)
my_list =  [1,'a',False]
```

在后端，**列表是可变的，而元组则不是**。不可变变量通常需要更少的内存，因此尽可能使用元组。

然而，《流畅的 Python》中涵盖了更深层次的内容。

从语义上讲，最好将不同的*kinds*的数据存储在元组中，将相同的*kinds*的数据存储在列表中。请注意，元组和列表都支持在同一变量中使用多种 Python 数据类型，但我们讨论的是变量的概念类型。

例如，一个元组可以用来存储以下信息：`(latitude, longitude, city_name)`。这些不仅是不同的数据类型`(float, float, str)`，而且在概念上也不同。另一方面，列表仅应存储纬度、经度、城市名称或三者的元组。

```py
# list of [float, float, str]
bad_practice_list = [[39.9526, 75.1652, 'Philadelphia'], 
                     [6.2476, 75.5658m 'Medellín']]# list of tuples
good_practice_list = [(39.9526, 75.1652, 'Philadelphia'), 
                      (6.2476, 75.5658m 'Medellín')]
```

为了提高 Python 代码的组织性，你应该始终将相同类型的信息保存在列表中。**元组用于结构，列表用于序列。**

## 2 — 解包可迭代对象

**提示：使用**`*****`**和**`**_**`**来改进你的解包。**

解包是一种非常流畅且易读的方式来访问可迭代对象中的值。它们在循环、列表推导式和函数调用中非常常见。

解包是通过将类序列的数据类型分配给以逗号分隔的变量名来完成的，例如……

```py
x, y, z = (1,2,3)
```

然而，《流利的 Python》介绍了一些高级解包方法。例如，你可以使用`*`来解包可迭代对象中的“其余”项。**使用星号符号是当你有一些感兴趣的项和其他不那么重要的项时很常见的做法。**

```py
x, *y, z = [1,2,3,4,5,6,7]
x #1
y #[2,3,4,5,6]
z #7
```

如你所见，`*`操作符可以出现在一组变量的中间，Python 会将所有未处理的值分配给该变量。

不过，我们可以进一步使用星号解包操作符。你可以使用`_`来解包并**不保存一个值**。这种惯例在你希望解包某些内容时很有用，但与上面的例子不同，你不需要所有的变量。

```py
x, _ = (1,2)
x #1
```

下划线`_`解包操作符的一个使用场景是当你处理字典或内建方法返回多个值时。

最后，为了锦上添花，我们可以将这两种方法结合起来，**解包而不保存“其余”值**。

```py
x, *_ = (1,2,3,5,6)
x #1
```

## 3 — 函数是否返回 None？

**提示：如果一个函数返回`**None**`，它执行的是原地操作。**

许多 Python 数据类型有两个版本的相同函数，如下所示的`x.sort()`和`sorted(x)`。

```py
x = [3,1,5,2]
x.sort()
x # [1,2,3,5]x = [3,1,5,2]
y = sorted(x)
x # [3,1,5,2]
y # [1,2,3,5]
```

在使用`x.sort()`的第一个例子中，我们执行的是原地排序，这更高效且占用更少的内存。但在使用`sorted(x)`的第二个例子中，我们能够保留列表的原始顺序。

通常，Python 保持这种符号约定。[点操作符](https://www.askpython.com/python/built-in-methods/dot-notation)如`x.sort()`通常返回`None`并进行原地变更。像`sorted(x)`这样的函数将变量作为参数，并返回**变更后的变量的副本**，但保持原始变量不变。

## 4 — 生成器表达式 vs. 列表推导式

**提示：如果你只访问一次变量，请使用生成器表达式。如果不是，请使用列表推导式。**

[列表推导式](https://www.w3schools.com/python/python_lists_comprehension.asp)（listcomps）和[生成器表达式](https://www.python.org/dev/peps/pep-0289/)（genexps）是实例化序列数据类型的不同方法。

```py
list_comp = [x for x in range(5)]
gen_exp = (x for x in range(5))
```

如上所示，列表推导式和生成器表达式之间唯一的语法区别是括号的类型——生成器表达式使用圆括号`()`，而列表推导式使用方括号`[]`。

**列表推导式是实例化的，这意味着它们会被评估并保存在内存中。生成器表达式则不是。**每次程序需要生成器表达式时，它都会执行计算来评估该表达式。

所以这就是为什么生成器表达式在你只使用变量一次时更好的原因——它们实际上不会存储在内存中，因此效率更高。但是，如果你反复访问一个序列或需要列表特定的方法，最好将其存储在内存中。

有趣的附注——你还可以使用列表推导语法创建字典……

```py
my_dict = {k:v for k,v in zip(['a','b'], [1,2])}
```

## 5 — 切片

最后，让我们简单总结一下切片。与解包不同，有时我们希望通过索引访问可迭代对象中的值。切片允许我们通过以下格式来做到这一点：`my_list[start:stop:step]`

对于那些知道`my_list[::-1]`可以反转列表顺序但不知道为什么（例如我自己），这就是原因。通过将`-1`作为步长参数传递，我们可以反向遍历列表。

现在大多数 Python 包遵循`[start:stop:index]`语法。Numpy 和 pandas 是一些显著的例子。让我们逐一看看每个参数……

+   `start`：你切片中的起始索引

+   `end`：你切片中的不包括的结束索引

+   `step`：你在`start`和`stop`索引之间的步长（及方向）

因此，由于这些值都是可选的，我们可以进行各种有趣的切片操作……

```py
x = [1,2,3,4]x[1:3]                   # [2,3]
x[2:0:-1]                # [3,2]last = [-1::]            # 4
all_but_last = x[:-1:]   # [1,2,3]
reversed = x[::-1]       # [4,3,2,1]
```

就这样！来自《流畅的 Python》第二章的5个主要技巧。还有一个部分……

## 数据科学家的有用笔记

声明，我并不特别有资格对这篇文章提出个人意见。不过，这些笔记应该相当直观。如果你不同意，请告诉我。

1.  **列表推导几乎总是应该替代循环。** 如果循环体很复杂，你可以创建一个执行这些操作的函数。通过将用户定义的函数与列表推导语法结合起来，你可以编写出既可读又高效的代码。如果你需要迭代多个变量，可以使用`[enumerate()](https://realpython.com/python-enumerate/)`或`[zip()](https://www.w3schools.com/python/ref_func_zip.asp)`。

1.  **在 Python 中“优化”并不重要。** 如果你在编写生产级代码，情况可能会有所不同。但实际上，使用元组与列表相比，你不会看到显著的性能提升。确保你的数据处理步骤逻辑清晰且高效是工作中的99%。如果1%很重要，那么你可以开始担心元组与列表的问题。此外，如果你真的关注高效代码，你可能不会使用 Python。

1.  **最后，切片非常酷。** 我一直知道`x[::-1]`可以反转列表，但直到阅读《流畅的 Python》这一章节才明白原因。它同样适用于 numpy 和 pandas！

*感谢阅读！我将再写 35 篇文章，将学术研究带入数据科学行业。查看我的评论以获取此文章的主要来源和一些有用资源的链接。*

**个人简介：[Michael Berk](https://www.linkedin.com/in/michael-berk-48783a146/)** (**[https://michaeldberk.com/](https://michaeldberk.com/)**) 是 Tubi 的数据科学家。

[原文](https://towardsdatascience.com/5-advanced-tips-on-python-sequences-5b0e09a21a83)。已获授权转载。

**相关：**

+   [如何发现你的机器学习模型中的弱点](/2021/09/weaknesses-machine-learning-models.html)

+   [使用这个Python库进行简单的文本抓取、解析和处理](/2021/10/simple-text-scraping-parsing-processing-python-library.html)

+   [使用Faker在Python中生成简单的合成数据](/2021/11/easy-synthetic-data-python-faker.html)

### 更多相关主题

+   [5门免费的高级Python编程课程](https://www.kdnuggets.com/5-free-advanced-python-programming-courses)

+   [25个针对数据科学家的高级SQL面试问题](https://www.kdnuggets.com/2022/10/25-advanced-sql-interview-questions-data-scientists.html)

+   [你必须知道的10个高级数据科学SQL面试问题…](https://www.kdnuggets.com/2023/01/top-10-advanced-data-science-sql-interview-questions-must-know-answer.html)

+   [机器学习模型的高级特征选择技术](https://www.kdnuggets.com/2023/06/advanced-feature-selection-techniques-machine-learning-models.html)

+   [回到基础第4周：高级主题和部署](https://www.kdnuggets.com/back-to-basics-week-4-advanced-topics-and-deployment)

+   [16个顶级技术数据来源，用于高级数据科学项目](https://www.kdnuggets.com/top-16-technical-data-sources-for-advanced-data-science-projects)
