# Python 数据结构比较

> 原文：[https://www.kdnuggets.com/2021/07/python-data-structures-compared.html](https://www.kdnuggets.com/2021/07/python-data-structures-compared.html)

[评论](#comments)![图示](../Images/beb472629858f9167f198fa19ad0f9ce.png)

图片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

选择存储数据的结构是解决编程任务和实施解决方案的重要部分，但往往没有得到应有的关注。不幸的是，在Python中，我经常看到列表被用作万能的数据结构。列表当然有其优点，但也有其缺点。还有很多其他的数据结构选项。

* * *

## 我们的前3个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在IT方面

* * *

让我们来看看5种不同的Python数据结构，看看它们如何用于存储我们在日常任务中可能处理的数据，以及它们在存储时所用的相对内存和创建及访问时所需的时间。

### 数据结构的类型

首先，让我们列出我们将在此考虑的5种数据结构，并提供一些初步见解。

**类**

在这种情况下，我们讨论的是普通类（与下面的数据类相对），Python [文档](https://docs.python.org/3/tutorial/classes.html)对其进行了高层次的描述：

> 类提供了一种将数据和功能捆绑在一起的方法。创建一个新类会创建一种新的*类型*的对象，允许创建该类型的新*实例*。每个类实例可以附加属性以维护其状态。类实例也可以具有方法（由其类定义）以修改其状态。

使用类的优点在于它们是传统的，并且被广泛使用和理解。它们是否在相对需要的内存或时间上过于冗余，是需要考虑的问题。

**数据类**

在Python 3.7中新增的 [数据类](https://www.python.org/dev/peps/pep-0557/) 是一种特殊的类，主要用于持有数据，提供了一些开箱即用的免费方法，用于典型功能，如实例化和打印实例内容。创建数据类是通过使用 `@dataclass` 装饰器来实现的。

> 尽管使用了非常不同的机制，数据类可以被认为是“带有默认值的可变命名元组”。因为数据类使用正常的类定义语法，你可以自由使用继承、 metaclasses、文档字符串、自定义方法、类工厂和其他Python类特性。这样的类被称为数据类，但实际上这个类没有什么特别之处：装饰器为类添加生成的方法，并返回与给定类相同的类。

正如你所见，[自动生成的方法](https://www.python.org/dev/peps/pep-0557/#id32)以及相关的时间节省是考虑数据类的主要原因。

**命名元组**

命名元组是[优雅的实现](https://docs.python.org/3/library/collections.html#collections.namedtuple)一种有用的数据结构，本质上是带有命名字段的元组子类。

> 命名元组为元组中的每个位置赋予含义，使代码更具可读性和自文档性。它们可以在常规元组使用的地方使用，并且可以通过名称而不是位置索引访问字段。

初看，命名元组似乎是Python中最接近简单C风格结构体的东西，使得它们对许多人具有天然的吸引力。

**字典**

Python [字典](https://python-reference.readthedocs.io/en/latest/docs/dict/)是键值对的集合。

> [Python字典](https://www.kdnuggets.com/2019/12/python-dictionary-methods.html)是可变的无序集合（它们不记录元素位置或插入顺序）的键值对。字典中的键必须是唯一且可哈希的。这包括数字、字符串和元组等类型。列表和字典不能用作键，因为它们是可变的。

字典的优势在于它们简单，数据易于访问，而且使用和理解都很广泛。

**列表**

这就是所谓的“万用”Python数据超结构，或者说很多代码会让你这么认为。实际上，[列表](https://python-reference.readthedocs.io/en/latest/docs/list/)是这样的：

> 列表是可变的有序且按索引排列的对象集合。列表的项是任意的Python对象。列表由放置在方括号中的逗号分隔的表达式列表组成。

为什么列表被广泛使用？它很简单易懂且易于实现，通常是学习Python时第一个接触的结构。是否存在与速度和内存使用相关的缺点？让我们来看看。

### 实现

首先，让我们看看这些结构的创建过程以及它们之间的比较。

我们可能使用这些数据结构存储数据的原因差异很大，但对于那些缺乏想象力的人，我们可以假设我们正在从SQL数据库中提取数据，需要将每条记录存储在这种结构中，以便在将数据进一步推进到我们的管道之前进行一些处理。

鉴于此，以下是创建每个五种结构的实例化代码。

```py
""" class """
class CustomerClass:
    def __init__(self, cust_num:str, f_name:str, l_name:str, address:str,
                       city:str, state:str, phone:str, age:int):
        self.cust_num = cust_num
        self.f_name = f_name
        self.l_name = l_name
        self.address = address
        self.city = city
        self.state = state
        self.phone = phone
        self.age = age

    def to_string(self):
        return(f'{self.cust_num}, {self.f_name}, {self.l_name}, {self.age}, 
                 {self.address}, {self.city}, {self.state}, {self.phone}'stgrcutures)

""" data class """
from dataclasses import dataclass
@dataclass
class CustomerDataClass:
    cust_num: int
    f_name: str
    l_name: str
    address: str
    city: str
    state: str
    phone: str
    age: int

""" named tuple """
from collections import namedtuple
CustomerNamedTuple = namedtuple('CustomerNamedTuple', 
                                'cust_num f_name l_name address city state phone age')

""" dict """
def make_customer_dict(cust_num: int, f_name: str, l_name: str, address: str, 
                       city: str, state: str, phone: str, age: int):
    return {'cust_num': cust_num,
            'f_name': f_name,
            'l_name': l_name,
            'address': address,
            'city': city,
            'state': state,
            'phone': phone,
            'age': age}

""" list """
def make_customer_list(cust_num: int, f_name: str, l_name: str, address: str, 
                       city: str, state: str, phone: str, age: int):
    return [cust_num, f_name, l_name, address,
            city, state, phone, age]
```

注意以下几点：

+   内置类型字典和列表的实例化已放入函数中。

+   上述讨论中类和数据类实现之间的区别。

+   命名元组的（显然主观的）优雅和简单性。

让我们看看这些结构的实例化情况，并比较实现这些操作所需的资源。

### 测试和结果。

我们将为每种结构创建一个单独的实例，每个实例中包含一个数据记录。我们将重复这个过程，将相同的数据字段用于每种结构1,000,000次，以获得更好的平均时间，并在我的一台简朴的Dell笔记本电脑上执行此过程，使用的是基于Ubuntu的操作系统。

![Image](../Images/b4fba0e02b2ae02ceba2ec1d33b38e96.png)

比较下面五种结构实例化的代码。

```py
""" instantiating structures """

from sys import getsizeof
import time

# class
customer_1 = CustomerClass('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
print(f'Data: {customer_1.to_string()}')
print(f'Type: {type(customer_1)}')
print(f'Size: {getsizeof(customer_1)} bytes')

t0 = time.time()
for i in range(1000000):
    customer = CustomerClass('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
t1 = time.time()
print('Time: {:.3f}s\n'.format(t1-t0))

# data class
customer_2 = CustomerDataClass('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
print(f'Data: {customer_2}')
print(f'Type: {type(customer_2)}')
print(f'Size: {getsizeof(customer_2)} bytes')

t0 = time.time()
for i in range(1000000):
    customer = CustomerDataClass('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
t1 = time.time()
print('Time: {:.3f}s\n'.format(t1-t0))

# named tuple
customer_3 = CustomerNamedTuple('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
print(f'Data: {customer_3}')
print(f'Type: {type(customer_3)}')
print(f'Size: {getsizeof(customer_3)} bytes')

t0 = time.time()
for i in range(1000000):
    customer = CustomerNamedTuple('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
t1 = time.time()
print('Time: {:.3f}s\n'.format(t1-t0))

# dict
customer_4 = make_customer_dict('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
print(f'Data: {customer_4}')
print(f'Type: {type(customer_4)}')
print(f'Size: {getsizeof(customer_4)} bytes')

t0 = time.time()
for i in range(1000000):
    customer = make_customer_dict('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
t1 = time.time()
print('Time: {:.3f}s\n'.format(t1-t0))

# list
customer_5 = make_customer_list('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
print(f'Data: {customer_5}')
print(f'Type: {type(customer_5)}')
print(f'Size: {getsizeof(customer_5)} bytes')

t0 = time.time()
for i in range(1000000):
    customer = make_customer_list('EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56)
t1 = time.time()
print('Time: {:.3f}s\n'.format(t1-t0))
```

这是上述内容的输出：

```py
Data: EP90210, Edward, Perez, 56, 123 Fake Street, Cityville, TX, 888-456-1234
Type: <class '__main__.CustomerClass'>
Size: 56 bytes
Time: 0.657s

Data: CustomerDataClass(cust_num='EP90210', f_name='Edward', l_name='Perez', address='123 Fake Street', city='Cityville', state='TX', phone='888-456-1234', age=56)
Type: <class '__main__.CustomerDataClass'>
Size: 56 bytes
Time: 0.630s

Data: CustomerNamedTuple(cust_num='EP90210', f_name='Edward', l_name='Perez', address='123 Fake Street', city='Cityville', state='TX', phone='888-456-1234', age=56)
Type: <class '__main__.CustomerNamedTuple'>
Size: 112 bytes
Time: 0.447s

Data: {'cust_num': 'EP90210', 'f_name': 'Edward', 'l_name': 'Perez', 'address': '123 Fake Street', 'city': 'Cityville', 'state': 'TX', 'phone': '888-456-1234', 'age': 56}
Type: <class 'dict'>
Size: 368 bytes
Time: 0.318s

Data: ['EP90210', 'Edward', 'Perez', '123 Fake Street', 'Cityville', 'TX', '888-456-1234', 56]
Type: <class 'list'>
Size: 128 bytes
Time: 0.184s
```

最后，另一个有用的数据是了解我们结构中存储的值的相对访问时间（在下面的例子中为地址）。相同的检索操作将重复1,000,000次，下面报告了平均时间。

```py
""" accessing an element """

# class
t0 = time.time()
for i in range(1000000):
    address = customer_1.address
t1 = time.time()
print(f'Type: {type(customer_1)}')
print('Time: {:.3f}s\n'.format(t1-t0))

# data class
t0 = time.time()
for i in range(1000000):
    address = customer_2.address
t1 = time.time()
print(f'Type: {type(customer_2)}')
print('Time: {:.3f}s\n'.format(t1-t0))

# named tuple
t0 = time.time()
for i in range(1000000):
    address = customer_3.address
t1 = time.time()
print(f'Type: {type(customer_3)}')
print('Time: {:.3f}s\n'.format(t1-t0))

# dictionary
t0 = time.time()
for i in range(1000000):
    address = customer_4['address']
t1 = time.time()
print(f'Type: {type(customer_4)}')
print('Time: {:.3f}s\n'.format(t1-t0))

# list
t0 = time.time()
for i in range(1000000):
    address = customer_5[3]
t1 = time.time()
print(f'Type: {type(customer_5)}')
print('Time: {:.3f}s\n'.format(t1-t0))
```

以及输出：

```py
Type: <class '__main__.CustomerClass'>
Time: 0.098s

Type: <class '__main__.CustomerDataClass'>
Time: 0.092s

Type: <class '__main__.CustomerNamedTuple'>
Time: 0.134s

Type: <class 'dict'>
Time: 0.095s

Type: <class 'list'>
Time: 0.117s

```

本文的意图不是推荐使用哪种数据结构，也不是建议存在一个适用于所有情况的最佳结构。相反，我们想要了解一些不同的选项及其相对的优缺点。像所有事情一样，必须做出权衡，并且在做出这些决策时，诸如可理解性、易用性等定量考虑因素也需要纳入考虑。

也就是说，从以上分析中有一些事情显而易见：

1.  在所有结构中，字典使用的存储量最大，在我们的例子中几乎是下一个最大结构的3倍——不过在我们查看扩展效应和内部字段数据类型之前，应该谨慎做出概括。

1.  不出所料，列表的实例化速度最快，但从中检索元素的速度却不是最快（几乎是最慢的）。

1.  在我们的例子中，命名元组是检索元素时速度最慢的结构，但在存储空间方面居中。

1.  两个类的实例化相对较长（这是预期的），但在元素检索和空间使用方面，两者与其他结构相比都非常具有竞争力。

所以我们不仅不打算在每种情况下推荐单一结构，也没有明确的赢家可以在每种情况下推荐。即使基于我们的小实验谨慎地进行一般化，显然在选择用于特定工作的结构时需要考虑优先级。至少，这些有限的实验提供了一些对Python中数据结构性能的小窗口洞察。

**相关**：

+   [作为数据科学家管理你的可重用Python代码](/2021/06/managing-reusable-python-code-data-scientist.html)

+   [5个Python数据处理技巧及代码片段](/2021/07/python-tips-snippets-data-processing.html)

+   [Python中的日期处理和特征工程](/2021/07/date-pre-processing-feature-engineering-python.html)

### 更多相关内容

+   [5步开始学习Python数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [Python基础：语法、数据类型和控制结构](https://www.kdnuggets.com/python-basics-syntax-data-types-and-control-structures)

+   [超级学习指南：免费算法和数据结构电子书](https://www.kdnuggets.com/2022/06/super-study-guide-free-algorithms-data-structures-ebook.html)

+   [AI和机器学习的数据结构入门指南](https://www.kdnuggets.com/guide-data-structures-ai-and-machine-learning)

+   [通过《快速Python数据科学》提升你的Python技能！](https://www.kdnuggets.com/2022/06/manning-step-python-game-fast-python-data-science.html)

+   [优化Python代码性能：深入探讨Python分析器](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)
