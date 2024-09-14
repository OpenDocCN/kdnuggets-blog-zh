# 用 5 步骤开始学习 Python 数据结构

> 原文：[https://www.kdnuggets.com/5-steps-getting-started-python-data-structures](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

![用 5 步骤开始学习 Python 数据结构](../Images/7b50389edfcbefed4f60b8c9297d036c.png)

# Python 数据结构简介

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT

* * *

当谈到学习如何编程时，无论你使用什么特定的编程语言，你会发现你所接触的大部分内容都可以归类为你新选择的学科中的几个主要主题。其中的一些，按理解的顺序排列，大致有：语法（语言的词汇）；命令（将词汇组合成有用的方式）；流程控制（我们如何引导命令执行的顺序）；算法（我们解决特定问题所采取的步骤……*这个词为什么变得如此令人困惑？*）；最后是数据结构（我们在算法执行期间用于数据操作的虚拟存储库（再次……一系列步骤））。

实质上，如果你想通过将一系列命令拼凑成算法的步骤来实现问题的解决方案，那么数据最终会被处理，数据结构就会变得至关重要。这些数据结构提供了一种有效组织和存储数据的方法，对创建快速、模块化的代码至关重要，这些代码能够执行有用的功能并良好地扩展。Python，这种特定的编程语言，拥有一系列内置的数据结构。

本教程将重点介绍这四种基础的 Python 数据结构：

+   列表 - 有序、可变，允许重复元素。用于存储数据序列。

+   元组 - 有序、不可变，允许重复元素。将它们视为不可变的列表。

+   字典 - 无序、可变，通过键值对映射。用于以键值格式存储数据。

+   集合 - 无序、可变，包含唯一元素。用于成员测试和消除重复。

除了基本的数据结构，Python 还提供了更高级的结构，如堆、队列和链表，这些都可以进一步提升你的编码能力。这些高级结构在基础结构的基础上构建，支持更复杂的数据处理，并且常用于特定场景。但是，你并不受限于这些结构；你可以利用所有现有的结构作为基础，来实现自己的结构。然而，对列表、元组、字典和集合的理解仍然至关重要，因为这些是更高级数据结构的基础。

本指南旨在提供对这些核心结构的清晰和简明的理解。随着你开始 Python 之旅，以下章节将引导你掌握基本概念和实际应用。从创建和操作列表到利用集合的独特功能，本教程将为你提供在编码中脱颖而出的技能。

# 步骤 1：在 Python 中使用列表

## 什么是 Python 中的列表？

Python 中的列表是有序的、可变的数据类型，可以存储各种对象，允许重复元素。列表通过使用方括号 `[ ]` 定义，元素之间用逗号分隔。

例如：

```py
fibs = [0, 1, 1, 2, 3, 5, 8, 13, 21]
```

列表在组织和存储数据序列方面极其有用。

## 创建列表

列表可以包含不同的数据类型，如字符串、整数、布尔值等。例如：

```py
mixed_list = [42, "Hello World!", False, 3.14159]
```

## 操作列表

列表中的元素可以被访问、添加、修改和删除。例如：

```py
# Access 2nd element (indexing begins at '0')
print(mixed_list[1])

# Append element 
mixed_list.append("This is new")

# Change element
mixed_list[0] = 5

# Remove last element
mixed_list.pop(0)
```

## 有用的列表方法

一些用于列表的实用内置方法包括：

+   `sort()` - 就地排序列表

+   `append()` - 将元素添加到列表末尾

+   `insert()` - 在索引处插入元素

+   `pop()` - 移除索引处的元素

+   `remove()` - 移除值的第一个出现

+   `reverse()` - 就地反转列表

## 实战示例：列表

```py
# Create shopping cart as a list
cart = ["apples", "oranges", "grapes"]

# Sort the list 
cart.sort()

# Add new item 
cart.append("blueberries") 

# Remove first item
cart.pop(0)

print(cart)
```

输出：

```py
['grapes', 'oranges', 'blueberries']
```

# 步骤 2：理解 Python 中的元组

## 什么是元组？

元组是 Python 中另一种序列数据类型，与列表类似。然而，与列表不同的是，元组是不可变的，这意味着其元素一旦创建就无法更改。它们通过用圆括号 `( )` 括起来来定义。

```py
# Defining a tuple
my_tuple = (1, 2, 3, 4)
```

## 何时使用元组

元组通常用于不应修改的项集合。元组比列表更快，这使得它们非常适合只读操作。一些常见的使用场景包括：

+   存储常量或配置数据

+   函数返回值包含多个组件

+   字典键，因为它们是可哈希的

## 访问元组元素

访问元组中的元素与访问列表元素的方式类似。索引和切片的工作方式相同。

```py
# Accessing elements
first_element = my_tuple[0]
sliced_tuple = my_tuple[1:3]
```

## 对元组的操作

由于元组是不可变的，许多列表操作如`append()`或`remove()`不适用。然而，你仍然可以执行一些操作：

+   **连接：** 使用`+`运算符组合元组。

```py
concatenated_tuple = my_tuple + (5, 6)
```

+   **重复：** 使用`*`运算符重复元组。

```py
repeated_tuple = my_tuple * 2
```

+   **成员资格：** 使用`in`关键字检查元素是否存在于元组中。

```py
exists = 1 in my_tuple
```

## 元组方法

元组相较于列表由于其不可变特性，内置方法较少。一些有用的方法包括：

+   **`count()`:** 统计特定元素的出现次数。

```py
count_of_ones = my_tuple.count(1)
```

+   **`index()`:** 查找值第一次出现的索引。

```py
index_of_first_one = my_tuple.index(1)
```

## 元组打包和解包

元组打包和解包是Python中的便捷特性：

+   **打包：** 将多个值分配给一个元组。

```py
packed_tuple = 1, 2, 3
```

+   **解包：** 将元组元素分配给多个变量。

```py
a, b, c = packed_tuple
```

## 不可变但不是严格的

虽然元组本身是不可变的，但它们可以包含像列表这样的可变元素。

```py
# Tuple with mutable list
complex_tuple = (1, 2, [3, 4])
```

请注意，虽然你不能改变元组本身，但可以修改其中的可变元素。

# 步骤3：掌握Python中的字典

## Python中的字典是什么？

Python中的字典是一个无序的、可变的数据类型，用于存储唯一键到值的映射。字典用大括号`{ }`表示，并由用逗号分隔的键值对组成。

例如：

```py
student = {"name": "Michael", "age": 22, "city": "Chicago"}
```

字典对于以结构化方式存储数据并通过键访问值非常有用。

## 创建字典

字典的键必须是不可变对象，如字符串、数字或元组。字典的值可以是任何对象。

```py
student = {"name": "Susan", "age": 23}

prices = {"milk": 4.99, "bread": 2.89}
```

## 操作字典

元素可以通过键进行访问、添加、更改和移除。

```py
# Access value by key
print(student["name"])

# Add new key-value 
student["major"] = "computer science"  

# Change value
student["age"] = 25

# Remove key-value
del student["city"]
```

## 有用的字典方法

一些有用的内置方法包括：

+   `keys()` - 返回键的列表

+   `values()` - 返回值的列表

+   `items()` - 返回(key, value)元组

+   `get()` - 返回键的值，避免KeyError

+   `pop()` - 移除键并返回值

+   `update()` - 添加多个键值对

## 使用字典的实际示例

```py
scores = {"Francis": 95, "John": 88, "Daniel": 82}

# Add new score
scores["Zoey"] = 97

# Remove John's score
scores.pop("John")  

# Get Daniel's score
print(scores.get("Daniel"))

# Print all student names 
print(scores.keys())
```

# 步骤4：探索Python中的集合

## Python中的集合是什么？

Python中的集合是一个无序的、可变的唯一不可变对象的集合。集合用大括号`{ }`表示，但与字典不同，没有键值对。

例如：

```py
numbers = {1, 2, 3, 4}
```

集合对成员测试、消除重复项和数学操作非常有用。

## 创建集合

可以通过将列表传递给`set()`构造函数来创建集合：

```py
my_list = [1, 2, 3, 3, 4]
my_set = set(my_list) # {1, 2, 3, 4}
```

集合可以包含混合数据类型，如字符串、布尔值等。

## 操作集合

元素可以从集合中添加和移除。

```py
numbers.add(5) 

numbers.remove(1)
```

## 有用的集合操作

一些有用的集合操作包括：

+   `union()` - 返回两个集合的并集

+   `intersection()` - 返回集合的交集

+   `difference()` - 返回集合之间的差集

+   `symmetric_difference()` - 返回对称差集

## 使用集合的实际示例

```py
A = {1, 2, 3, 4}
B = {2, 3, 5, 6}

# Union - combines sets 
print(A | B) 

# Intersection 
print(A & B)

# Difference  
print(A - B)

# Symmetric difference
print(A ^ B)
```

# 步骤5：比较列表、字典和集合

## 特性比较

以下是对我们在本教程中提到的四种Python数据结构的简明比较。

| 结构 | 有序 | 可变 | 允许重复元素 | 用途 |
| --- | --- | --- | --- | --- |
| 列表 | 是 | 是 | 是 | 存储序列 |
| 元组 | 是 | 否 | 是 | 存储不可变序列 |
| 字典 | 否 | 是 | 键：否 值：是 | 存储键值对 |
| 集合 | 否 | 是 | 否 | 消除重复项，成员测试 |

## 何时使用每种数据结构

将此视为在特定情况下选择数据结构的软性指南。

+   使用列表来处理有序的、基于序列的数据。适合堆栈/队列。

+   使用元组来表示有序的、不可变的序列。当你需要一个不应更改的固定元素集合时，元组非常有用。

+   使用字典来存储键值数据。适合存储相关属性。

+   使用集合来存储唯一元素和进行数学操作。

## 使用所有四种数据结构的实际示例

让我们看看这些结构如何在一个比单行代码更复杂的示例中协同工作。

```py
# Make a list of person names
names = ["John", "Mary", "Bob", "Mary", "Sarah"]

# Make a tuple of additional information (e.g., email)
additional_info = ("john@example.com", "mary@example.com", "bob@example.com", "mary@example.com", "sarah@example.com")

# Make set to remove duplicates
unique_names = set(names)

# Make dictionary of name-age pairs
persons = {}
for name in unique_names:
  persons[name] = random.randint(20,40)

print(persons)
```

输出：

```py
{'John': 34, 'Bob': 29, 'Sarah': 25, 'Mary': 21}
```

这个例子利用列表来表示有序序列，利用元组来存储附加的不可变信息，利用集合来去除重复项，利用字典来存储键值对。

# 向前推进

在这个全面的教程中，我们深入探讨了Python中的基础数据结构，包括列表、元组、字典和集合。这些结构是Python编程的基石，提供了数据存储、处理和操作的框架。理解这些结构对于编写高效且可扩展的代码至关重要。从使用列表操作序列，到用字典组织数据，再到利用集合确保唯一性，这些基本工具在数据处理上提供了巨大的灵活性。

正如我们通过代码示例所见，这些数据结构可以以各种方式组合使用来解决复杂问题。通过利用这些数据结构，你可以开启数据分析、机器学习及其他领域的广泛可能性。不要犹豫，查阅官方的[Python数据结构文档](https://docs.python.org/3/tutorial/datastructures.html)以获得更多见解。

编程愉快！

[**马修·梅约**](https://www.linkedin.com/in/mattmayo13/) ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为KDnuggets的主编，马修旨在使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、机器学习算法和探索新兴的人工智能。他致力于使数据科学社区的知识普及化。马修从6岁起便开始编程。

### 关于这个话题的更多内容

+   [5步开始使用SQL](https://www.kdnuggets.com/5-steps-getting-started-with-sql)

+   [5步开始使用Scikit-learn](https://www.kdnuggets.com/5-steps-getting-started-scikit-learn)

+   [5步开始使用Google Cloud Platform](https://www.kdnuggets.com/5-steps-google-cloud-platform)

+   [5步开始使用PyTorch](https://www.kdnuggets.com/5-steps-getting-started-pytorch)

+   [Python 数据科学入门](https://www.kdnuggets.com/getting-started-with-python-for-data-science)

+   [Python 生成器入门](https://www.kdnuggets.com/2023/02/getting-started-python-generators.html)
