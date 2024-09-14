# Python基础：语法、数据类型和控制结构

> 原文：[https://www.kdnuggets.com/python-basics-syntax-data-types-and-control-structures](https://www.kdnuggets.com/python-basics-syntax-data-types-and-control-structures)

![Python基础：语法、数据类型和控制结构](../Images/0df4813999f547470284c7df34c49b9d.png)

作者提供的图片

你是一个初学者，想学习Python编程吗？如果是的话，这个适合初学者的教程将帮助你熟悉语言的基础知识。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

本教程将介绍Python的——相当于英语友好的——语法。你还将学习如何处理不同的数据类型、条件语句和循环。

如果你已经在开发环境中安装了Python，请启动一个Python REPL并开始编写代码。或者如果你想跳过安装——立即开始编码——我建议你前往[Google Colab](https://colab.google/)并开始编写代码。

# 你好，Python！

在我们用Python编写经典的“Hello, world!”程序之前，先了解一下这门语言。Python是*解释型语言*。这是什么意思？

在任何编程语言中，你编写的所有源代码都应该被翻译成机器语言。虽然像C和C++这样的编译语言需要在程序运行之前获得整个机器代码，但*解释器*会解析源代码并即时解释。

创建一个Python脚本，输入以下代码并运行：

```py
print("Hello, World!")
```

为了打印出*Hello, World!*，我们使用了`print()`函数，它是Python中的众多内置函数之一。

在这个超简单的例子中，请注意“Hello, World!”是一个序列——一串字符。Python字符串由一对单引号或双引号界定。因此，要打印任何消息字符串，你可以使用`print("<message_string>")`。

## 读取用户输入

现在让我们更进一步，使用`input()`函数从用户那里读取一些输入。你应该始终提示用户，让他们知道*应该输入什么*。

这是一个简单的程序，它接收用户的名字作为输入，并向他们问好。

注释通过提供额外的上下文来帮助提高代码的可读性。Python中的单行注释以#开头。

请注意，代码片段中的字符串前面有一个`f`。这样的字符串称为*格式化字符串或f-strings*。要在f-string中替换变量的值，请在一对大括号中指定变量的名称，如下所示：

```py
# Get user input
user_name = input("Please enter your name: ")

# Greet the user
print(f"Hello, {user_name}! Nice to meet you!")
```

当你运行程序时，系统会首先提示你输入，然后打印出问候信息：

```py
Please enter your name: Bala
Hello, Bala! Nice to meet you!
```

让我们继续学习Python中的变量和数据类型。

# Python中的变量和数据类型

变量在任何编程语言中都像*容器*，用于存储信息。在我们编写的代码中，我们已经创建了一个变量`user_name`。当用户输入他们的名字（一个字符串）时，它会存储在`user_name`变量中。

## Python中的基本数据类型

让我们通过基本的数据类型在Python中逐一了解：`int`、`float`、`str`和`bool`，使用简单的示例逐步进行：

**整数**（`int`）：整数是没有小数点的整数。你可以这样创建整数并将其分配给变量：

```py
age = 25
discount= 10
```

这些是将值分配给变量的赋值语句。在像C这样的语言中，你需要在声明变量时指定数据类型，但Python是**动态类型语言**。它从值中推断数据类型。因此，你可以重新分配变量以保存完全不同数据类型的值：

```py
number = 1
number = 'one'
```

你可以使用`type`函数检查Python中任何变量的数据类型：

```py
number = 1
print(type(number))
```

`number`是一个整数：

```py
Output >>> <class 'int'>
```

我们现在将一个字符串值分配给`number`：

```py
number = 'one'
print(type(number))
```

```py
Output >>> <class 'str'>
```

**浮点数**（`float`）：浮点数表示带有小数点的实数。你可以这样创建`float`数据类型的变量：

```py
height = 5.8
pi = 3.14159
```

你可以对数值数据类型进行各种操作——加法、减法、整数除法、指数运算等。以下是一些示例：

```py
# Define numeric variables
x = 10
y = 5

# Addition
add_result = x + y
print("Addition:", add_result)  # Output: 15

# Subtraction
sub_result = x - y
print("Subtraction:", sub_result)  # Output: 5

# Multiplication
mul_result = x * y
print("Multiplication:", mul_result)  # Output: 50

# Division (floating-point result)
div_result = x / y
print("Division:", div_result)  # Output: 2.0

# Integer Division (floor division)
int_div_result = x // y
print("Integer Division:", int_div_result)  # Output: 2

# Modulo (remainder of division)
mod_result = x % y
print("Modulo:", mod_result)  # Output: 0

# Exponentiation
exp_result = x ** y
print("Exponentiation:", exp_result)  # Output: 100000
```

**字符串**（`str`）：字符串是字符的序列，封装在单引号或双引号中。

```py
name = "Alice"
quote = 'Hello, world!'
```

**布尔值**（`bool`）：布尔值表示`True`或`False`，指示条件的真实性。

```py
is_student = True
has_license = False
```

Python在处理不同数据类型方面的灵活性使你能够有效地存储、执行各种操作和操控数据。

这是一个将我们迄今为止学到的所有数据类型结合在一起的示例：

```py
# Using different data types together
age = 30
score = 89.5
name = "Bob"
is_student = True

# Checking if score is above passing threshold
passing_threshold = 60.0
is_passing = score >= passing_threshold

print(f"{name=}")
print(f"{age=}")
print(f"{is_student=}")
print(f"{score=}")
print(f"{is_passing=}")
```

这是输出结果：

```py
Output >>>

name='Bob'
age=30
is_student=True
score=89.5
is_passing=True
```

# 超越基本数据类型

假设你正在管理一个教室中的学生信息。创建一个集合来存储所有学生的信息会比为每个学生重复定义变量更有帮助。

## 列表

列表是有序的项集合——用一对方括号括起来。列表中的项可以是相同或不同的数据类型。列表是*可变的*，这意味着你可以在创建后更改其内容。

这里，`student_names`包含学生的名字：

```py
# List
student_names = ["Alice", "Bob", "Charlie", "David"]
```

## 元组

元组是类似于列表的有序集合，但它们是*不可变的*，这意味着你不能在创建后更改其内容。

假设你希望`student_scores`成为一个不可变的集合，包含学生的考试成绩。

```py
# Tuple
student_scores = (85, 92, 78, 88)
```

## 字典

字典是键值对的集合。字典的键应该是唯一的，它们映射到对应的值。字典是可变的，允许你将信息与特定键关联起来。

在这里，`student_info`包含关于每个学生的姓名和分数作为键值对：

```py
student_info = {'Alice': 85, 'Bob': 92, 'Charlie': 78, 'David': 88}
```

但请稍等，还有一种更优雅的方法来创建Python中的字典。

我们即将学习一个新概念：**字典推导式**。如果一开始不清楚没关系。你可以随时学习更多并在以后继续研究。

但推导式相当直观。如果你想让`student_info`字典的键是学生姓名，对应的值是他们的考试分数，你可以这样创建字典：

```py
# Using a dictionary comprehension to create the student_info dictionary
student_info = {name: score for name, score in zip(student_names, student_scores)}

print(student_info)
```

请注意我们如何使用[`zip()`函数](https://www.freecodecamp.org/news/the-zip-function-in-python-explained-with-examples/)同时遍历`student_names`列表和`student_scores`元组。

```py
Output >>>

{'Alice': 85, 'Bob': 92, 'Charlie': 78, 'David': 88}
```

在这个例子中，字典推导式直接将`student_names`列表中的每个学生姓名与`student_scores`元组中的对应考试分数配对，创建了`student_info`字典，其中姓名作为键，分数作为值。

现在你已经熟悉了基本数据类型以及一些序列/可迭代对象，让我们进入讨论的下一部分：**控制结构**。

# Python中的控制结构

当你运行一个Python脚本时，代码按脚本中的顺序依次执行。

有时，你需要实现逻辑来根据特定条件控制执行流程，或遍历可迭代对象以处理其中的项目。

我们将学习如何通过if-else语句实现分支和条件执行。我们还将学习如何使用循环遍历序列，以及循环控制语句break和continue。

## If语句

当你需要仅在特定条件为真时执行一块代码时，你可以使用`if`语句。如果条件为假，这块代码将不会被执行。

![Python基础：语法、数据类型和控制结构](../Images/49d1d18138bc08a5de338721c2ee7a51.png)

图片由作者提供

考虑这个例子：

```py
score = 75

if score >= 60:
    print("Congratulations! You passed the exam.")
```

在这个例子中，只有当`score`大于或等于60时，`if`块中的代码才会被执行。由于`score`是75，因此将打印出“恭喜！你通过了考试。”

```py
Output >>> Congratulations! You passed the exam.
```

## If-else 条件语句

`if-else`语句允许你在条件为真时执行一块代码，而在条件为假时执行另一块代码。

![Python基础：语法、数据类型和控制结构](../Images/6bc9a8173d76b970abae3f5f9781951b.png)

图片由作者提供

让我们基于测试分数的例子进行扩展：

```py
score = 45

if score >= 60:
    print("Congratulations! You passed the exam.")
else:
    print("Sorry, you did not pass the exam.")
```

在这里，如果`score`小于60，`else`块中的代码将被执行：

```py
Output >>> Sorry, you did not pass the exam.
```

## If-elif-else 结构

`if-elif-else`语句用于在有多个条件需要检查时。它允许你测试多个条件，并为第一个满足条件的情况执行相应的代码块。

如果`if`和所有`elif`语句中的条件都为假，则执行`else`块。

![Python基础：语法、数据类型和控制结构](../Images/6ce428852aa600f21cd1d49810f58463.png)

图片来源：作者

```py
score = 82

if score >= 90:
    print("Excellent! You got an A.")
elif score >= 80:
    print("Good job! You got a B.")
elif score >= 70:
    print("Not bad! You got a C.")
else:
    print("You need to improve. You got an F.")
```

在这个例子中，程序检查`score`是否符合多个条件。第一个满足条件的代码块将被执行。由于`score`是82，我们得到：

```py
Output >>> Good job! You got a B.
```

## 嵌套`if`语句

嵌套的`if`语句用于在另一个条件内检查多个条件。

```py
name = "Alice"
score = 78

if name == "Alice":
    if score >= 80:
        print("Great job, Alice! You got an A.")
    else:
        print("Good effort, Alice! Keep it up.")
else:
    print("You're doing well, but this message is for Alice.")
```

在这个例子中，有一个嵌套的`if`语句。首先，程序检查`name`是否是"Alice"。如果为真，它会检查`score`。由于`score`是78，因此执行内部的`else`块，打印"Good effort, Alice! Keep it up."。

```py
Output >>> Good effort, Alice! Keep it up.
```

Python提供了几种循环结构来迭代集合或执行重复任务。

## `for`循环

在Python中，`for`循环提供了一种简洁的语法，让我们能够迭代现有的可迭代对象。我们可以这样迭代`student_names`列表：

```py
student_names = ["Alice", "Bob", "Charlie", "David"]

for name in student_names:
    print("Student:", name)
```

上述代码的输出为：

```py
Output >>>

Student: Alice
Student: Bob
Student: Charlie
Student: David
```

## `while`循环

如果你想在某个条件为真时执行一段代码，你可以使用`while`循环。

让我们使用相同的`student_names`列表：

```py
# Using a while loop with an existing iterable

student_names = ["Alice", "Bob", "Charlie", "David"]
index = 0

while index < len(student_names):
    print("Student:", student_names[index])
    index += 1
```

在这个例子中，我们有一个包含学生姓名的`student_names`列表。我们使用`while`循环通过跟踪`index`变量来迭代列表。

循环会继续进行，只要`index`小于列表的长度。在循环内部，我们打印每个学生的姓名，并增加`index`以移动到下一个学生。注意使用`len()`函数来获取列表的长度。

这与使用`for`循环迭代列表得到的结果相同：

```py
Output >>>

Student: Alice
Student: Bob
Student: Charlie
Student: David
```

我们使用一个`while`循环，从列表中弹出元素直到列表为空：

```py
student_names = ["Alice", "Bob", "Charlie", "David"]

while student_names:
    current_student = student_names.pop()
    print("Current Student:", current_student)

print("All students have been processed.")
```

列表方法`pop`移除并返回列表中的最后一个元素。

在这个例子中，`while`循环只要`student_names`列表中还有元素就会继续进行。在循环内部，使用`pop()`方法来移除并返回列表中的最后一个元素，并打印当前学生的姓名。

循环会继续，直到所有学生都被处理完毕，并在循环外打印最终消息。

```py
Output >>>

Current Student: David
Current Student: Charlie
Current Student: Bob
Current Student: Alice
All students have been processed.
```

`for`循环通常在迭代现有可迭代对象如列表时更为简洁且易读。但`while`循环在循环条件更复杂时可以提供更多控制。

## 循环控制语句

`break`会提前退出循环，而`continue`会跳过当前迭代的其余部分并进入下一次迭代。

这是一个示例：

```py
student_names = ["Alice", "Bob", "Charlie", "David"]

for name in student_names:
    if name == "Charlie":
        break
    print(name)
```

当`name`为Charlie时，控制从循环中跳出，输出结果为：

```py
Output >>>
Alice
Bob
```

## 模拟Do-While循环行为

在 Python 中，没有像某些其他编程语言中的 `do-while` 循环。然而，你可以使用带有 `break` 语句的 `while` 循环来实现相同的行为。下面是如何在 Python 中模拟 `do-while` 循环：

```py
while True:
    user_input = input("Enter 'exit' to stop: ")
    if user_input == 'exit':
        break
```

在这个例子中，循环将无限运行，直到用户输入“exit”。循环至少运行一次，因为条件最初设置为 `True`，然后在循环内部检查用户的输入。如果用户输入“exit”，则会执行 `break` 语句，从而退出循环。

这是一个示例输出：

```py
Output >>>
Enter 'exit' to stop: hi
Enter 'exit' to stop: hello
Enter 'exit' to stop: bye
Enter 'exit' to stop: try harder!
Enter 'exit' to stop: exit
```

请注意，这种方法类似于其他语言中的`do-while`循环，其中循环体在检查条件之前至少会执行一次。

# 总结与下一步

希望你能顺利跟随这个教程进行编码。现在你已经掌握了 Python 的基础知识，是时候开始编码一些超级简单的项目，以应用你所学到的所有概念了。

**[Bala Priya C](https://www.linkedin.com/in/bala-priya/)** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和咖啡！目前，她正在通过撰写教程、使用指南、观点文章等，与开发者社区分享她的知识。

### 更多相关内容

+   [Python 控制流备忘单](https://www.kdnuggets.com/2022/11/python-control-flow-cheatsheet.html)

+   [没有复杂 RegEx 语法的 Python 字符串匹配](https://www.kdnuggets.com/2023/02/python-string-matching-without-complex-regex-syntax.html)

+   [优化数据存储：探索 SQL 中的数据类型和规范化](https://www.kdnuggets.com/optimizing-data-storage-exploring-data-types-and-normalization-in-sql)

+   [可视化框架类型](https://www.kdnuggets.com/types-of-visualization-frameworks)

+   [5 步骤入门 Python 数据结构](https://www.kdnuggets.com/5-steps-getting-started-python-data-structures)

+   [基础知识回顾 第1周：Python 编程与数据科学基础](https://www.kdnuggets.com/back-to-basics-week-1-python-programming-data-science-foundations)
