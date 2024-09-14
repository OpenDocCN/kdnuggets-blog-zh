# Python 函数参数：终极指南

> 原文：[https://www.kdnuggets.com/2023/02/python-function-arguments-definitive-guide.html](https://www.kdnuggets.com/2023/02/python-function-arguments-definitive-guide.html)

![Python 函数参数：终极指南](../Images/d9c4eae2eb65b627d7d7af25bef3cf00.png)

图片由作者提供

像所有编程语言一样，Python 允许你定义函数。定义函数后，你可以在脚本中随处调用它，还可以从一个模块中导入函数到另一个模块。函数使你的代码变得*模块化*和*可重用*。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

在 Python 中，你可以定义函数以接受不同类型的参数。本指南将教你所有关于 Python 函数参数的知识。让我们开始吧。

# Python 函数的快速回顾

要定义一个 Python 函数，你可以使用`def`关键字后跟函数名称和括号。如果你希望函数接受*参数*，则应该在括号内指定参数名称。定义函数后，你可以用参数值（称为参数）调用它。

例如，考虑`add()`函数的定义：`num1`和`num2`是参数，函数调用中用于这些参数的值是参数。

![Python 函数参数：终极指南](../Images/9439ddfa1c8901bec106c19a2adcc0e9.png)

参数与参数 | 图片由作者提供

# 位置参数

考虑以下函数`meet()`。它接受三个字符串并打印出该人的信息。

```py
def meet(name,job_title,city):
    return f"Meet {name}, a {job_title} from {city}."
```

你可以按如下方式用参数调用函数：

```py
meet1 = meet('James','Writer','Nashville')
print(meet1)
```

```py
Output >> Meet James, a Writer from Nashville.
```

使用这种方式调用函数时，参数作为位置参数传递。位置参数会按照它们在函数调用中出现的*相同*顺序映射到函数参数。

因此，函数调用中的第一个、第二个和第三个参数分别用作`name`、`job_title`和`city`的值。

这是一个例子。

```py
meet2 = meet('Artist','John','Austin')
print(meet2)
```

```py
Output >> Meet Artist, a John from Austin.
```

在这种情况下，输出“Meet Artist, a John from Austin.”是不合理的。这时，*关键字参数*可以发挥作用。

# 关键字参数

使用关键字参数调用函数时，你应该指定参数的*名称*和它接受的*值*。

你可以按照任何顺序指定关键字参数，只要参数的名称正确。我们可以调用相同的函数 `meet()`，但这次使用关键字参数。

```py
meet3 = meet(city='Madison',name='Ashley',job_title='Developer')
print(meet3)
```

```py
Output >> Meet Ashley, a Developer from Madison.
```

如果你想同时使用位置参数和关键字参数怎么办？这会使你的代码不够清晰，不推荐这样做。但如果你在函数调用中使用位置参数和关键字参数，位置参数应 *始终* 置于关键字参数之前。否则，你会遇到错误。

# 调用具有可变参数数量的函数

到目前为止，我们知道参数的数量，并相应地定义了函数。然而，这种情况并非总是如此。如果你希望你的函数每次被调用时都能接受可变数量的参数，该怎么办呢？

![Python 函数参数：终极指南](../Images/55eacdf8e0e782dbac43496538b6dbd1.png)

图片来自作者

在处理 Python 代码库时，你可能会遇到如下形式的函数定义：

```py
def some_func(*args, **kwargs):
	# do something on args and kwargs
```

使用 `*args` 和 `**kwargs` 在函数定义中，你可以让函数分别接受可变数量的位置参数和关键字参数。它们的工作方式如下：

+   `args` 将可变数量的位置参数收集为一个元组，然后可以使用 * 解包运算符进行解包。

+   `kwargs` 将函数调用中的所有关键字参数收集为一个字典。这些关键字参数可以通过 ** 运算符解包。

> **注意：** 使用 `*args` 和 `**kwargs` 并不是严格要求。你可以使用任何你选择的名称。

让我们通过示例更好地理解这一点。

# 可变数量的位置参数

函数 `reverse_strings` 接受可变数量的字符串并返回一个反转字符串的列表。

```py
def reverse_strings(*strings):
    reversed_strs = []
    for string in strings:
        reversed_strs.append(string[::-1])
    return reversed_strs
```

你现在可以根据需要调用函数，传递一个或多个参数。以下是一些示例：

```py
def reverse_strings(*strings):
    reversed_strs = []
    for string in strings:
        reversed_strs.append(string[::-1])
    return reversed_strs
```

```py
Output >> ['nohtyP']
```

```py
rev_strs2 = reverse_strings('Coding','Is','Fun')
print(rev_strs2)
```

```py
Output >> ['gnidoC', 'sI', 'nuF']
```

# 可变数量的关键字参数

以下函数 `running_sum()` 接受可变数量的关键字参数：

```py
def running_sum(**nums):
    sum = 0
    for key,val in nums.items():
        sum+=val
    return sum
```

由于 `nums` 是一个 Python 字典，你可以对字典对象调用 `items()` 方法以获取一个元组列表。每个元组都是一个键值对。

这里有几个例子：

```py
sum1 = running_sum(a=1,b=5,c=10)
print(sum1)
```

```py
Output >> 16
```

```py
sum2 = running_sum(num1=7,num2=20)
print(sum2)
```

```py
Output >> 27
```

> **注意：** 当使用 `*args` 和 `**kwargs` 时，位置参数和关键字参数不是必需的。因此，这是一种使你的函数参数 *可选* 的方法。

# 使用参数的默认值

在定义 Python 函数时，你可以为一个或多个参数提供默认值。如果你为某个特定参数提供了默认值，你就不必在函数调用中使用该参数。

+   如果在函数调用中提供了与参数对应的值，该值会被使用。

+   否则，将使用默认值。

在以下示例中，函数 `greet()` 具有一个参数 `name`，其默认值设置为 "there"。

```py
def greet(name='there'):
    print(f"Hello {name}!")
```

所以当你用特定字符串作为参数调用 `greet()` 时，它的值会被使用。

```py
greet('Jane')
```

```py
Output >> Hello Jane!
```

当你没有传递名称字符串时，将使用默认参数 "there"。

```py
greet()
```

```py
Output >> Hello there!
```

# 可变默认参数 - 奇特案例

使用默认参数时，你应该小心不要将默认值设置为可变对象。那为什么呢？让我们通过一个例子来理解。

![Python 函数参数：终极指南](../Images/7f0a4e840c1868954a962604a59fd92d.png)

图片来源 | 创建于 [imgflip](https://imgflip.com/)

以下函数`append_to_list()`接受一个元素和一个列表作为参数。它将元素追加到列表的末尾，并返回结果列表。

```py
# def_args.py
def append_to_list(elt,py_list=[]):
    py_list.append(elt)
    return py_list
```

你会期待以下行为：

+   当函数同时用元素和列表调用时，它会返回包含追加元素到原始列表末尾的列表。

+   当你*仅*用元素作为参数调用函数时，它会返回一个仅包含该元素的列表。

但让我们看看会发生什么。

打开你的终端，运行`python -i`以启动 Python REPL。我已在`def_args.py`文件中定义了`append_to_list()`函数。

```py
>>> from def_args import append_to_list
```

当你用数字 4 和列表 [1,2,3] 作为参数调用函数时，我们得到 [1,2,3,4]，这是预期的结果。

```py
>>> append_to_list(4,[1,2,3])
[1, 2, 3, 4]
```

现在，让我们进行一系列函数调用，仅将元素追加到列表中：

```py
>>> append_to_list(7)
[7]
>>> append_to_list(9)
[7, 9]
>>> append_to_list(10)
[7, 9, 10]
>>> append_to_list(12)
[7, 9, 10, 12]
```

等等，这不是我们预期的结果。第一次函数用 7 作为参数（没有列表）被调用时，我们得到 [7]。然而，在随后的调用中，我们看到元素被追加到*这个*列表中。

这是因为默认参数只在函数定义时绑定一次。列表在函数调用`append_to_list(7)`期间首次被修改。列表不再为空。所有后续对函数的调用—即使没有列表—也会修改原始列表。

因此，你应该避免使用可变的默认参数。一个可能的解决方法是将默认值设置为`None`，并在函数调用没有包含列表时初始化一个空列表。

# 结论

这是你在本教程中学到的总结。你学会了如何使用位置参数和关键字参数调用Python函数，以及如何使用`*args`和`**kwargs`分别传递可变数量的位置参数和关键字参数。接着，你学习了如何为某些参数设置默认值，以及可变默认参数的奇特情况。希望你觉得这个教程对你有帮助。继续编码！

**[Bala Priya C](https://twitter.com/balawc27)**是一位技术写作人员，喜欢创作长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过撰写教程、操作指南等与开发者社区分享她的学习经验。

### 更多相关话题

+   [将你的职业转向数据科学的终极指南](https://www.kdnuggets.com/2022/05/definitive-guide-switching-career-data-science.html)

+   [解决 MySQL 中幻读的终极指南](https://www.kdnuggets.com/2022/06/definitive-guide-solving-phantom-read-mysql.html)

+   [初学者指南：Pandas Melt 函数](https://www.kdnuggets.com/2023/03/beginner-guide-pandas-melt-function.html)

+   [你应该了解的关于梯度下降和成本函数的 5 个概念](https://www.kdnuggets.com/2020/05/5-concepts-gradient-descent-cost-function.html)

+   [什么是函数？](https://www.kdnuggets.com/2022/11/function.html)

+   [3 个 SQL 聚合函数面试问题，适用于数据科学](https://www.kdnuggets.com/2023/01/3-sql-aggregate-function-interview-questions-data-science.html)
