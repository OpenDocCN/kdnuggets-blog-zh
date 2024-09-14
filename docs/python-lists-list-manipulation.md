# Python 列表与列表操作

> 原文：[https://www.kdnuggets.com/2019/11/python-lists-list-manipulation.html](https://www.kdnuggets.com/2019/11/python-lists-list-manipulation.html)

[评论](#comments)

**由 [Michael Galarnyk](https://www.linkedin.com/in/michaelgalarnyk/)，数据科学家**

Python 列表与列表操作视频

在开始之前，我应该提到，本文博客和上述 [视频](https://www.youtube.com/watch?v=w9I8R3WSVqc) 中的代码可以在我的 [github](https://github.com/mGalarnyk/Python_Tutorials/blob/master/Python_Basics/Intro/Python3Basics_Part1.ipynb) 上找到。

### 定义列表

列表用方括号 [] 包裹

![图示](../Images/0f0d6b6cfca3d1e902a9ec55201c31a6.png)

定义列表。此表中的第二行索引是访问列表项的方法。

```py
# Define a list
z = [3, 7, 4, 2]
```

列表存储一个有序的项集合，这些项可以是不同类型的。上面定义的列表中的所有项都是同一类型（int），但列表中的所有项不需要是相同类型，如下所示。

```py
# Define a list
heterogenousElements = [3, True, 'Michael', 2.0]
```

列表包含一个 int，一个 bool，一个字符串和一个 float。

### 访问列表中的值

列表中的每一项都有一个分配的索引值。需要注意的是，Python 是一个基于零索引的语言。这意味着列表中的第一个项的索引是 0。

![图示](../Images/b89d7bba815577c207d36e6508c9d096.png)

访问索引为 0 的项（用蓝色标出）

```py
# Define a list
z = [3, 7, 4, 2]*# Access the first item of a list at index 0*
print(z[0])
```

![图示](../Images/e506b11c3c3095273a47b4da1d3b757d.png)

访问索引为 0 的项的输出。

Python 还支持负索引。负索引从末尾开始。有时使用负索引来获取列表中的最后一项会更方便，因为你不需要知道列表的长度即可访问最后一项。

![图示](../Images/08102028b645d582d9f293b068ef229e.png)

访问最后一个索引处的项。

```py
*# print last item in the list*
print(z[-1])
```

![图示](../Images/77ccf2e6eb5c18a41b1a3406cc403ea6.png)

访问列表中最后一项的输出

作为提醒，你也可以使用正索引访问相同的项（如下所示）。

![图示](../Images/8f32d3df0a624d4f9cf1260295ac31cf.png)

访问列表 z 中最后一项的替代方法

### 列表切片

切片适用于获取列表中的子集。以下示例代码将返回一个列表，其中包含从索引 0 开始到不包括索引 2 的项。

![图示](../Images/b31d8c1f94517d1b95a686b9e11b7bdf.png)

第一个索引是包含的（在 : 之前），而最后一个索引（在 : 之后）是不包含的

```py
# Define a list
z = [3, 7, 4, 2]print(z[0:2])
```

![图示](../Images/1f34d7abc2e96e0e6f925be856a85647.png)

列表切片语法。

![](../Images/0eca160ed1f66c0b9bfe57903ccf7698.png)

```py
*# everything up to but not including index 3*
print(z[:3])
```

![](../Images/26d4397ca86dad3c42a634871805927a.png)

![](../Images/3983df8d301c889ad3e0194259c2c21d.png)

下面的代码返回一个包含从索引 1 到列表末尾的项的列表

```py
*# index 1 to end of list*
print(z[1:])
```

![](../Images/8161147ede53d54272f7bbbf16cc856c.png)

### **更新列表中的项**

![](../Images/bb2fd0ebf6c2a84049c7f77d9c408c4f.png)

Python中的列表是可变的。这意味着在定义列表后，可以更新列表中的单个项。

```py
# Defining a list
z = [3, 7, 4, 2]# Update the item at index 1 with the string "fish"
z[1] = "fish"
print(z)
```

![图](../Images/e470c3724abb94e178a6c6bed138e6b7.png)

修改列表中项的代码

### 列表方法

Python列表有不同的方法帮助你修改列表。本教程部分仅介绍各种Python列表方法。

### **索引方法**

![](../Images/6c48708ebc305bd3b39598f13ee40b72.png)

```py
# Define a list
z = [4, 1, 5, 4, 10, 4]
```

![](../Images/21335a4cb2ed6052d824b958874e8657.png)

索引方法返回值出现的第一个索引。在下面的代码中，它将返回0。

```py
print(z.index(4))
```

![](../Images/9529bfaebe65a4f0919cf7372331a1f2.png)

![](../Images/0e83d8ef6b09f05d84473d71b703e579.png)

你还可以指定从哪里开始搜索。

```py
print(z.index(4, 3))
```

![](../Images/e192a4904a4bfbc025b3da01b31a4aaa.png)

### **计数方法**

count方法的作用如其名称所示。它计算一个值在列表中出现的次数

```py
random_list = [4, 1, 5, 4, 10, 4]
random_list.count(5)
```

![](../Images/00997ff65acfb0e694537f95d8344882.png)

### **排序方法**

![图](../Images/96e935cabcf093fd3c8ba28455d7676b.png)

对Python列表进行排序——实际代码为：z.sort()

sort方法会原地对列表进行排序和修改。

```py
z = [3, 7, 4, 2]
z.sort()
print(z)
```

![](../Images/b9b943fa973c0e851aa94bb1a82de875.png)

上面的代码将一个列表按从低到高排序。下面的代码显示了你也可以按从高到低的顺序对列表进行排序。

![图](../Images/48d83f04e24a67d55cb802e1dc45a095.png)

将Python列表按从高到低排序

```py
*# Sorting and Altering original list*
*# high to low*
z.sort(reverse = True)
print(z)
```

![](../Images/03fa965a366da06c49b468e87d308626.png)

顺便提一下，你也可以将字符串列表按从a到z和从z到a进行排序，具体可以参见[这里](https://youtu.be/w9I8R3WSVqc?t=4m16s)。

### **追加方法**

![图](../Images/27cae59fc7b4824d149ffaac8842d2b1.png)

将值3添加到列表的末尾。

append方法将元素添加到列表的末尾。这会在原地发生。

```py
z = [7, 4, 3, 2]
z.append(3)
print(z)
```

![](../Images/84d5498da7272bb3f11e6e08c705b0d3.png)

### 删除方法

![](../Images/1f1093808a3d7ef0ab846f8bcdd209a0.png)

remove方法删除列表中第一次出现的值。

```py
z = [7, 4, 3, 2, 3]
z.remove(2)
print(z)
```

![图](../Images/a9787d46bd38cb470a918d872c58485a.png)

代码从列表z中删除值2的第一次出现

### 弹出方法

![图](../Images/658e10358cc9c89ff711788ef09fdf8f.png)

z.pop(1) 移除索引为1的值，并返回值4。

pop方法移除你提供的索引处的项。此方法还会返回你从列表中移除的项。如果你不提供索引，它将默认移除最后一个索引处的项。

```py
z = [7, 4, 3, 3]
print(z.pop(1))
print(z)
```

![](../Images/a7518ec4ba34da9ee44360e4a3d832e5.png)

### 扩展方法

![](../Images/58195ca811341000b060c99ef9f2e170.png)

该方法通过附加项来扩展列表。其好处在于你可以将列表合并在一起。

```py
z = [7, 3, 3]
z.extend([4,5])
print(z)
```

![图](../Images/32b16d7484f288bf66c0cad7f2c1e076.png)

将列表[4, 5]添加到列表z的末尾。

另外，也可以使用+运算符来完成相同的操作。

```py
print([1,2] + [3,4])
```

![](../Images/b17f5b2bbaff25d02671c1b5c2f88427.png)

### 插入方法

![图](../Images/07ae4f25a5d697021d7a490a0be28d84.png)

在索引 4 处插入列表 [1,2]

插入方法在你提供的索引之前插入一个项目

```py
z = [7, 3, 3, 4, 5]
z.insert(4, [1, 2])
print(z)
```

![](../Images/0a1c5eb208cf57bf76abfa8ac3b4073d.png)

### 结语

如果你有任何问题，请通过这里或 [youtube 视频](https://www.youtube.com/watch?v=w9I8R3WSVqc) 的评论区或 [Twitter](https://twitter.com/GalarnykMichael) 告诉我！下篇文章将回顾 [python 字典](https://medium.com/@GalarnykMichael/python-basics-10-dictionaries-and-dictionary-methods-4e9efa70f5b9)。如果你想学习如何利用 Pandas、Matplotlib 或 Seaborn 库，请考虑参加我的 [Python 数据可视化 LinkedIn 学习课程](https://www.linkedin.com/learning/python-for-data-visualization/value-of-data-visualization)。这是一个 [免费预览视频](https://youtu.be/BE8CVGJuftI)。

**个人简介：[Michael Galarnyk](https://www.linkedin.com/in/michaelgalarnyk/)** 是一位数据科学家和企业培训师。他目前在斯克里普斯转化研究所工作。你可以在 Twitter (https://twitter.com/GalarnykMichael)、Medium (https://medium.com/@GalarnykMichael) 和 GitHub (https://github.com/mGalarnyk) 上找到他。

[原文](https://towardsdatascience.com/python-basics-6-lists-and-list-manipulation-a56be62b1f95)。已获许可转载。

**相关内容：**

+   [理解箱线图](/2019/11/understanding-boxplots.html)

+   [应用于 Pandas DataFrames 的集合操作](/2019/11/set-operations-applied-pandas-dataframes.html)

+   [理解 Python 中的分类决策树](2019/08/understanding-decision-trees-classification-python.html)

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关内容

+   [5 种过滤 Python 列表的方法](https://www.kdnuggets.com/2022/11/5-ways-filtering-python-lists.html)

+   [8 个最佳 Python 图像处理工具](https://www.kdnuggets.com/2022/11/8-best-python-image-manipulation-tools.html)

+   [数据处理的必要 Python 库](https://www.kdnuggets.com/essential-python-libraries-for-data-manipulation)

+   [10 个 Pandas 一行代码进行数据访问、处理和管理](https://www.kdnuggets.com/2023/01/pandas-one-liners-data-access-manipulation-management.html)

+   [为什么不应过度使用 Python 列表推导式](https://www.kdnuggets.com/why-you-should-not-overuse-list-comprehensions-in-python)

+   [如何开始使用 SQL - 免费学习资源列表](https://www.kdnuggets.com/2022/10/get-running-sql-list-free-learning-resources.html)
