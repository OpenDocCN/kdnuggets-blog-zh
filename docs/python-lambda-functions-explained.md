# Python Lambda 函数详解

> 原文：[https://www.kdnuggets.com/2023/01/python-lambda-functions-explained.html](https://www.kdnuggets.com/2023/01/python-lambda-functions-explained.html)

![Python Lambda 函数详解](../Images/0ad70492b45ee5b7fe82629b3ae46a5a.png)

编辑者提供的图像

自计算机编程出现以来，函数通过提供如重用性、可读性、模块化、错误减少和易于修改等优势，扮演了重要角色。重用性被认为是函数最有用的特性之一，但如果我告诉你有一些函数虽然不可重用但仍然有用，你会怎么想？要了解更多，请继续阅读！

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

# Lambda 函数及其语法

lambda 函数没有名称，是一个立即调用的函数。它可以接受任意数量的参数，但只返回一个表达式，这与普通函数不同。

它有如下语法：

```py
lambda parameters: expression
```

上述 lambda 函数的语法包含三个元素：

+   关键字 “lambda” — 类似于用户定义函数中的‘def’

+   parameters — 类似于普通函数中的参数

+   expression — 这是用来计算结果的操作

与普通函数不同，lambda 函数中的参数没有括号。考虑到表达式是一行代码，它需要简洁，但同时必须对参数执行所需的操作。lambda 函数在定义时被消耗，因此不能在未明确重新定义的情况下重用。

通常，lambda 函数作为参数传递给高阶函数，如 Python 内置函数 – filter()、map() 或 reduce()。但什么是高阶函数呢？

高阶函数定义为接收其他函数作为参数的函数（将在后续章节中讨论）。

# 用途与示例

现在我们已经了解了语法，是时候用一个简单的例子来理解 lambda 函数了。假设你想计算一个数字的平方。你可以定义一个名为“square”的函数，或者编写一个如下所示的 lambda 函数：

```py
def square(x):
    return x**2
```

```py
lambda x: x**2
```

上述 lambda 函数接受一个参数 x 并返回它的平方。

## 调用 Lambda 函数

调用 lambda 函数非常简单，首先包装 lambda 函数的构造，然后在括号中放入参数。

```py
(lambda x: x**2)(3)
```

```py
Output >> 9
```

## 使用多个参数调用

对于具有多个参数的 lambda 函数，输入参数由逗号分隔。相应的参数在执行时遵循相同的顺序。

```py
(lambda x, y, z: x**2 + y**2 + z**2)(1, 2, 0)
```

```py
Output >> 100
```

## 单一条件语句

您还可以执行条件操作，例如下面示例中的 if-else 块：

```py
(lambda x: 100 if x > 100 else (50 if x > 50 else x))(75)
```

```py
Output >> 100
```

## 嵌套条件语句

由于这些是单行函数，条件嵌套是通过圆括号而不是缩进来完成的。

```py
(lambda x: 100 if x > 100 else (50 if x > 50 else x))(75)
```

```py
Output >> 50
```

与上述 lambda 函数对应的用户定义函数如下所示。

```py
def conditional_statement_demo(x):
    if x > 100:
        return100
    elif x > 50:
        return 50
    else:
        return x
conditional_statement_demo(75)
```

```py
Output >> 50
```

请注意，在嵌套条件场景中，用户定义的函数是更好的选择。

## 赋值给变量

lambda 函数也可以赋值给变量，并像用户定义的函数一样调用。

```py
square = lambda x: x**2
square(3)
```

```py
Output >> 9
```

尽管可以将函数赋值给变量，但这很少使用，因为它违背了 lambda 函数的唯一目的，即立即调用。变量赋值在将 lambda 函数用于另一个函数时比较有用。

## 字符串连接

以下示例演示了如何连接两个字符串——在其中，您打印包含作为参数传递的人的名字的欢迎消息。

```py
welcome_msg = lambda name : print('Hi', name + '! This is your computer.')
welcome_msg(“Vidhi”)
```

```py
Output >> Hi Vidhi! This is your computer.
```

## 嵌套函数

lambda 函数在另一个函数内部使用时最为强大。

让我们考虑一个用户定义的函数示例，该函数接受一个参数，用作任何数字的指数。

```py
def power(y):
  return lambda x : x**y
square = power(2)
print(square(5))
```

```py
Output >> 25
```

## 通过下划线调用

是时候来点魔法了！让我们通过另一种方式来调用 lambda 函数。

```py
lambda x, y : x**y
_(2,3)
```

```py
Output >> 8
```

这里发生了什么？一旦定义了 lambda 函数（本质上是匿名函数），它将使用“_”和参数进行调用。

## 与 map() 一起使用

lambda 函数经常作为 map() 函数的参数使用。它将序列映射到一个函数，并且不需要明确的定义（尤其是对于下面显示的这种简单操作）

```py
print(list(map(lambda x: x**2, range(1,11))))
```

```py
Output >> [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

map() 将从 1 到 10 的序列映射到 lambda 函数，并返回序列中所有元素的平方。

## 与 reduce() 一起使用

reduce 函数对序列中的所有元素执行 lambda 函数中定义的操作，以返回一个单一的输出值。示例中将 1 到 4 的所有元素相乘，并计算输出为 1*2*3*4 = 24。

```py
from functools import reduce
print(reduce((lambda x, y: x * y), range(1,5)))
```

```py
Output >> 24
```

让我们检查另一个示例，其中 reduce 与 lambda 函数一起使用，以返回两个元素中较大的一个。当将列表作为第二个参数传递时，它返回列表中的最大数字。

```py
lst = [8, 9, 50, 6, 12]
print(reduce(lambda a, b: a if a > b else b, lst))
```

```py
Output >> 50
```

## 与 filter() 一起使用

lambda 函数的另一个重要用途是与 filter() 一起使用。在以下示例中，如果数字是奇数，lambda 函数返回 True。当与 filter 一起使用时，它返回列表中的所有奇数。

```py
lst = [12, 2, 8, 46, 3, 34, 68, 92, 49]
result = list(filter(lambda x: (x % 2 != 0), lst))
print(result)
```

```py
Output >> [3, 49]
```

lambda 函数非常方便，可以节省大量编码工作。本文解释了 lambda 函数的语法以及它与用户定义的函数的不同之处。希望您发现它对开始学习 lambda 函数的基础知识有所帮助。

**[Vidhi Chugh](https://vidhi-chugh.medium.com/)** 是一位人工智能策略师和数字化转型领导者，致力于在产品、科学和工程交汇处构建可扩展的机器学习系统。她是一位获奖的创新领袖、作者和国际演讲者。她的使命是使机器学习大众化，并打破术语，让每个人都能参与这一转型。

### 更多相关主题

+   [Python 中的统计函数](https://www.kdnuggets.com/2022/10/statistical-functions-python.html)

+   [4 个你可能不知道的 Python Itertools 过滤函数](https://www.kdnuggets.com/2023/08/4-python-itertools-filter-functions-probably-didnt-know.html)

+   [提升 Python 函数编写质量的 5 个技巧](https://www.kdnuggets.com/5-tips-for-writing-better-python-functions)

+   [10 个 Python 统计函数](https://www.kdnuggets.com/10-python-statistical-functions)

+   [KDnuggets 新闻，7月20日：机器学习算法详解…](https://www.kdnuggets.com/2022/n29.html)

+   [什么是矩量生成函数？](https://www.kdnuggets.com/2022/12/momentgenerating-functions.html)
