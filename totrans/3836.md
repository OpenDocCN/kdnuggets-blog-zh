# 理解 Python 中的可迭代对象与迭代器

> 原文：[https://www.kdnuggets.com/2022/01/understanding-iterables-iterators-python.html](https://www.kdnuggets.com/2022/01/understanding-iterables-iterators-python.html)

![理解 Python 中的可迭代对象与迭代器](../Images/f687a94152e04fa892eb31852b49e233.png)

图片由 [geralt on Pixabay](https://pixabay.com/users/geralt-9301/) 提供

可迭代对象和迭代器经常被混淆，然而它们是两个不同的概念。本文将解释它们之间的区别，以及如何使用它们。我们先简要了解一下什么是迭代。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

通俗来说，迭代意味着“重复步骤”。在编程术语中，迭代是指以特定次数重复执行语句/代码块，逐个产生输出。例如，可以使用 for 循环来执行迭代。

## Python 中的可迭代对象

可迭代对象是可以使用 for 循环进行遍历的对象。可迭代对象包含数据或有值，通过执行 for 循环可以进行迭代，逐个产生输出。

可迭代对象实现了 `__iter__()` 方法，并返回一个迭代器对象。然而，如果 `__iter__()` 方法未定义，Python 将使用 `__getitem__()` 方法。

可迭代对象的例子有列表、字典、字符串、元组等。只要你能遍历它，它就是一个可迭代对象。

要确定一个对象是否是可迭代的，你需要检查它是否支持 `__iter__`。为此，我们使用 `dir()` 函数，它返回指定对象的所有属性和方法，但不包括值。

**示例代码：**

```py
class Person:
 name = "Nisha"
 age = 25
 country = "England"

print(dir(Person))
```

**输出：**

```py
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', 
'__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', 
'__init__', '__init_subclass__', '__le__', '__lt__', '__module__', 
'__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', 
'__setattr__', '__sizeof__', '__str__', '__subclasshook__', 
'__weakref__', 'age', 'country', 'name']
```

## Python 中的迭代器

迭代器也是一个对象，它使用 `__iter__()` 和 `__next__()` 方法，这被称为迭代器协议。它是一个有状态的可迭代对象，这意味着它记住在迭代过程中处于什么阶段。

迭代器逐个返回值。当返回可迭代对象的下一个值时，迭代器的状态会被更新，并知道如何使用 `__next__()` 方法获取下一个值。迭代器只能向前移动，不能回退或重置。

迭代器在没有更多元素或对象已耗尽时也会引发 `StopIteration` 异常。

**示例代码：**

这里我创建了一个名为“numbers”的迭代器。我加入了一行代码来检查其类型。我们期望迭代能输出所有数字。然而，我要求它打印第六个输出，但迭代器中只有五个值。让我们看看会发生什么。

```py
numbers = iter([2, 4, 6, 8, 10])

print(type(numbers))

print(next(numbers))
print(next(numbers))
print(next(numbers))
print(next(numbers))
print(next(numbers))
# The next() function will raise StopIteration as it is exhausted
print(next(numbers))
```

**输出：**

我们可以看到类型是‘list_iterator’。输出在10处停止，并引发了`StopIteration`，因为数字列表中没有更多元素。

```py
<class 'list_iterator'>
2
4
6
8
10

---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-15-0cb721c5f355> in <module>()
     11 
     12 # The bext() function will raise StopIteration as it is exhausted
---> 13 print(next(numbers))

StopIteration:
```

**迭代器的限制：**

1.  迭代器只向前移动，不能向后或重置。

1.  迭代器不能被复制，因为它是一个只能向前移动的单向对象。

1.  由于其单向方向，无法检索到之前的元素。

## 可迭代对象和迭代器之间的相似性和差异

|  | **可迭代对象** | **迭代器** |
| --- | --- | --- |
| **通过以下方式迭代：** | for 循环 | for 循环 |
| **使用的方法：** | __iter__() | __iter__() 和 __next__() |

迭代器是一个可迭代对象，因为它也实现了`__iter__()`方法。

**记住：** 每个迭代器都是一个可迭代对象，但并非每个可迭代对象都是一个迭代器。

**可迭代对象的 `dir()` 示例：**

```py
numbers = [2, 4, 6, 8, 10]

print(dir(numbers))
```

**输出：**

在这个输出中，我突出了`__iter__`，显示它是一个可迭代对象的方法。

```py
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', 
'__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', 
'__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', 
'__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', 
'__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', 
'__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', 
'__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 
'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

**迭代器的 `dir()` 示例：**

在这个例子中，我们正在迭代数字列表。

```py
numbers = [2, 4, 6, 8, 10]
numbers2 = iter(numbers)

print(dir(numbers2))
```

**输出：**

在这个输出中，我突出了`__iter__`和`__next__`，显示它们是迭代器的方法。

```py
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', 
'__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', 
'__init_subclass__', '__iter__', '__le__', '__length_hint__', '__lt__', 
'__ne__', '__new__', '__next__', '__reduce__', '__reduce_ex__', 
'__repr__', '__setattr__', '__setstate__', '__sizeof__', '__str__', 
'__subclasshook__']
```

我希望这个简短的博客能让你更好地理解可迭代对象和迭代器之间的区别。

**[尼莎·阿里亚](https://www.linkedin.com/in/nisha-arya-ahmed/)** 是一名数据科学家和自由技术作家。她特别感兴趣于提供数据科学职业建议或教程，以及数据科学理论知识。她还希望探索人工智能如何/可以促进人类寿命的不同方式。作为一个积极学习者，她寻求拓宽自己的技术知识和写作技能，同时帮助指导他人。

### 更多相关内容

+   [探索 Python 的 itertools 中的无限迭代器](https://www.kdnuggets.com/exploring-infinite-iterators-in-python-itertools)

+   [理解 Python 的迭代和成员资格：`__contains__`和`__iter__`魔法方法指南](https://www.kdnuggets.com/understanding-pythons-iteration-and-membership-a-guide-to-__contains__-and-__iter__-magic-methods)

+   [理解和实现 Python 中的遗传算法](https://www.kdnuggets.com/understanding-and-implementing-genetic-algorithms-in-python)

+   [理解贝叶斯定理的三种方式将提升你的数据科学能力](https://www.kdnuggets.com/2022/06/3-ways-understanding-bayes-theorem-improve-data-science.html)

+   [通过实现来理解：决策树](https://www.kdnuggets.com/2023/02/understanding-implementing-decision-tree.html)

+   [理解人工智能中的代理环境](https://www.kdnuggets.com/2022/05/understanding-agent-environment-ai.html)
