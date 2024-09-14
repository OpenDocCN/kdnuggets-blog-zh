# `__getitem__` 介绍：Python 中的魔法方法

> 原文：[`www.kdnuggets.com/2023/03/introduction-getitem-magic-method-python.html`](https://www.kdnuggets.com/2023/03/introduction-getitem-magic-method-python.html)

![Introduction to __getitem__: A Magic Method in Python](img/852caad5f1c3f9d13c5cf8ca82fa7203.png)

作者图片

# 介绍

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

Python 是一种神奇的语言，包含许多即使是高级用户也可能不熟悉的概念。双下划线方法或魔法方法就是其中之一。魔法方法是由双下划线包围的特殊方法。它们不像普通的 Python 方法那样被显式调用。一个这样的魔法方法是 `__getitem__` 方法，它使 Python 对象能够像序列或容器（例如列表、字典和元组）一样进行操作。它接受索引或切片，并从集合中检索其关联的值。当我们使用 `indexer [ ]` 运算符访问对象中的元素时，它会被自动调用。

将这个方法想象成一根魔法棒，它可以让你通过写几行代码就提取所需的信息。有趣吧？这个方法在数据分析和机器学习中也被广泛使用。因此，让我们深入探讨 `__getitem__` 方法，发现它的力量和灵活性。

# 使用 `__getitem__` 方法的好处

我希望你明白，作为一个 Python 程序员，你的职责不仅仅是编写功能代码。你的代码应该是高效的、可读的和可维护的。使用 `__getitem__` 将帮助你实现这些目标。以下是使用这个魔法方法的一些其他好处：

+   通过允许你提取仅必要的信息而不是将完整数据结构加载到内存中来减少内存使用

+   提供了更大的灵活性来处理和操作数据

+   允许你在不循环遍历数据的情况下迭代集合

+   通过允许你编写内置类型可能无法实现的高级索引来增强功能

+   简化代码，因为它使用了熟悉的表示法

# 实现 `__getitem__` 方法

`__getitem__` 方法的语法如下：

```py
def __getitem__(self, index):
	# Your Implementation
	pass
```

它定义了函数的行为，并将你尝试访问的索引作为参数。我们可以这样使用这个方法：

```py
my_obj[index] 
```

这在底层转换为语句 `my_obj.__getitem__(index)`。现在你可能会想，这与内置的 `indexer []` 运算符有什么不同？无论你在哪里使用这种表示法，Python 都会自动调用 `__getitem__` 方法，并且是访问元素的简写。但是如果你想改变自定义对象的索引行为，你需要显式调用 `__getitem__` 方法。

## 示例 #01

首先让我们从一个简单的例子开始。我们将创建一个 Student 类，该类将包含所有学生的列表，我们可以通过索引访问这些学生，并且索引表示他们的唯一学生 ID。

```py
class Student:
    def __init__(self, names):
        self.names=names

    def __getitem__(self,index):
        return self.names[index]

section_A= Student(["David", "Elsa", "Qasim"])
print(section_A[2])
```

**输出：**

```py
Qasim
```

现在我们将转到一个高级示例，我们将使用 `__getitem__` 方法来改变索引行为。假设我有一个字符串元素的列表，我希望在输入其索引位置时能检索到元素，并且如果我输入字符串本身，也能获取到索引位置。

```py
class MyList:
    def __init__(self, items):
        self.items = items

    def __getitem__(self, index):
        if isinstance(index, int):
            return self.items[index]
        elif isinstance(index, str):
            return self.items.index(index)
        else:
            raise TypeError("Invalid Argument Type")

my_list = MyList(['red', 'blue', 'green', 'black'])

# Indexing with integer keys
print(my_list[0]) 
print(my_list[2])  

# Indexing with string keys
print(my_list['red'])  
print(my_list['green']) 
```

**输出：**

```py
red
green
0    
2
```

# 结论

这个方法对于快速查找实例属性非常有用。考虑到这个方法的灵活性和多功能性，我会说这是 Python 中最少被利用的魔法方法之一。我希望你喜欢阅读这篇文章，如果你有兴趣了解 Python 中其他魔法方法，请在评论区告诉我。

**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1)** 是一名有志的软件开发者，对数据科学及 AI 在医学中的应用充满兴趣。Kanwal 被选为 2022 年 APAC 区域的 Google Generation Scholar。Kanwal 喜欢通过撰写关于热门话题的文章来分享技术知识，并且热衷于提高女性在科技行业的代表性。

### 更多相关话题

+   [使用 apply() 方法处理 Pandas 数据框](https://www.kdnuggets.com/2022/07/apply-method-pandas-dataframes.html)

+   [Python f-Strings 魔法：每个编码员需要知道的 5 个改变游戏规则的技巧](https://www.kdnuggets.com/python-fstrings-magic-5-gamechanging-tricks-every-coder-needs-to-know)

+   [每个程序员应该知道的 11 个 Python 魔法方法](https://www.kdnuggets.com/11-python-magic-methods-every-programmer-should-know)

+   [理解 Python 的迭代与成员：__contains__ 和 __iter__ 魔法方法指南](https://www.kdnuggets.com/understanding-pythons-iteration-and-membership-a-guide-to-__contains__-and-__iter__-magic-methods)

+   [揭开神经魔法：深入探讨激活函数](https://www.kdnuggets.com/unveiling-neural-magic-a-dive-into-activation-functions)

+   [跳进池塘：解开 CNN 池化层的魔法](https://www.kdnuggets.com/diving-into-the-pool-unraveling-the-magic-of-cnn-pooling-layers)
