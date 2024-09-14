# Pydantic教程：简化Python中的数据验证

> 原文：[https://www.kdnuggets.com/pydantic-tutorial-data-validation-in-python-made-simple](https://www.kdnuggets.com/pydantic-tutorial-data-validation-in-python-made-simple)

![Pydantic教程：简化Python中的数据验证](../Images/472c573b553fe701a75ff772541202af.png)

作者提供的图片

# 为什么使用Pydantic？

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

Python是一种动态类型语言。因此，你可以创建变量而无需明确指定数据类型。你也可以随时将完全不同的值赋给同一个变量。**虽然这使得初学者更容易上手，但也同样容易在你的Python应用程序中创建无效对象。**

你可以创建数据类，允许定义带有类型提示的字段。但它们不直接支持数据验证。引入 [Pydantic](https://docs.pydantic.dev/latest/)，一个流行的数据验证和序列化库。Pydantic提供开箱即用的数据验证和序列化支持。这意味着你可以：

+   利用Python的类型提示来验证字段，

+   使用Pydantic提供的自定义字段和内置验证器，并

+   根据需要定义自定义验证器。

在本教程中，我们将建模一个简单的‘员工’类，并使用Pydantic的数据验证功能来验证不同字段的值。让我们开始吧！

# 安装Pydantic

如果你有Python 3.8或更高版本，你可以使用pip安装Pydantic：

```py
$ pip install pydantic
```

如果你在应用程序中需要电子邮件验证，可以在安装Pydantic时像这样安装可选的 [email-validator](https://github.com/JoshData/python-email-validator) 依赖：

```py
$ pip install pydantic[email]
```

或者，你可以运行以下命令安装email-validator：

```py
$ pip install email-validator
```

**注意**：在我们的示例中，我们将使用电子邮件验证。如果你希望跟随代码进行，请安装相应的依赖。

# 创建一个基本的Pydantic模型

现在让我们创建一个简单的`Employee`类。首先，我们创建一个继承自`BaseModel`类的类。各种字段和预期类型如下所示：

```py
# main.py

from pydantic import BaseModel, EmailStr

class Employee(BaseModel):
    name: str
    age: int
    email: EmailStr
    department: str
    employee_id: str
```

注意，我们已将电子邮件指定为Pydantic支持的`EmailStr`类型，而不是普通的Python字符串。这是因为*所有有效的字符串可能不是有效的电子邮件*。

# 在员工模型中验证字段

因为`Employee`类很简单，让我们为以下字段添加验证：

+   `email`：应为有效的电子邮件。指定`EmailStr`可以满足这个要求，我们在创建具有无效电子邮件的对象时会遇到错误。

+   `employee_id`：应为有效的员工 ID。我们将为该字段实现自定义验证。

## 实现自定义验证

在这个例子中，假设`employee_id`应该是一个长度为 6 的仅包含字母数字字符的字符串。

我们可以使用`@validator`装饰器与`employee_id`字段作为参数，并定义`validate_employee_id`方法，如下所示：

```py
# main.py 

from pydantic import BaseModel, EmailStr, validator

...

@validator("employee_id")
    def validate_employee_id(cls, v):
        if not v.isalnum() or len(v) != 6:
            raise ValueError("Employee ID must be exactly 6 alphanumeric characters")
        return v
```

现在，这种方法检查我们尝试创建的 Employee 对象的`employee_id`是否有效。

此时，你的脚本应该如下所示：

```py
# main.py

from pydantic import BaseModel, EmailStr, validator

class Employee(BaseModel):
    name: str
    age: int
    email: EmailStr
    department: str
    employee_id: str

    @validator("employee_id")
     def validate_employee_id(cls, v):
         if not v.isalnum() or len(v) != 6:
             raise ValueError("Employee ID must be exactly 6 alphanumeric characters")
         return v
```

# 使用 Pydantic 模型加载和解析 JSON 数据

在实际操作中，将 API 的 JSON 响应解析为像 Python 字典这样的数据结构是非常常见的。假设我们有一个名为‘employees.json’的文件（在当前目录中），其记录如下：

```py
# employees.json

[
	{
    	"name": "John Doe",
    	"age": 30,
    	"email": "john.doe@example.com",
    	"department": "Engineering",
    	"employee_id": "EMP001"
	},
	{
    	"name": "Jane Smith",
    	"age": 25,
    	"email": "jane.smith@example.com",
    	"department": "Marketing",
    	"employee_id": "EMP002"
	},
	{
    	"name": "Alice Brown",
    	"age": 35,
    	"email": "invalid-email",
    	"department": "Finance",
    	"employee_id": "EMP0034"
	},
	{
    	"name": "Dave West",
    	"age": 40,
    	"email": "dave.west@example.com",
    	"department": "HR",
    	"employee_id": "EMP005"
	}
]
```

我们可以看到，在对应于‘Alice Brown’的第三条记录中，我们有两个无效的字段：`email`和`employee_id`：

![Pydantic 教程：简化 Python 数据验证](../Images/d740eb545c95d03f1e265ba994f3f067.png)

因为我们已经指定电子邮件应为`EmailStr`，所以电子邮件字符串将自动进行验证。我们还添加了`validate_employee_id`类方法来检查对象是否具有有效的员工 ID。

现在让我们添加代码来解析 JSON 文件并创建员工对象（我们将使用内置的[json 模块](https://docs.python.org/3/library/json.html)）。我们还从 Pydantic 中导入`ValidationError`类。实际上，我们尝试创建对象，当数据验证失败时处理 ValidationError 异常，并打印出错误：

```py
# main.py

import json
from pydantic import BaseModel, EmailStr, ValidationError, validator
...

# Load and parse the JSON data
with open("employees.json", "r") as f:
    data = json.load(f)

# Validate each employee record
for record in data:
    try:
        employee = Employee(**record)
        print(f"Valid employee record: {employee.name}")
    except ValidationError as e:
        print(f"Invalid employee record: {record['name']}")
        print(f"Errors: {e.errors()}")
```

当你运行脚本时，你应该会看到类似的输出：

```py
Output >>>

Valid employee record: John Doe
Valid employee record: Jane Smith
Invalid employee record: Alice Brown
Errors: [{'type': 'value_error', 'loc': ('email',), 'msg': 'value is not a valid email address: The email address is not valid. It must have exactly one @-sign.', 'input': 'invalid-email', 'ctx': {'reason': 'The email address is not valid. It must have exactly one @-sign.'}}, {'type': 'value_error', 'loc': ('employee_id',), 'msg': 'Value error, Employee ID must be exactly 6 alphanumeric characters', 'input': 'EMP0034', 'ctx': {'error': ValueError('Employee ID must be exactly 6 alphanumeric characters')}, 'url': 'https://errors.pydantic.dev/2.6/v/value_error'}]
Valid employee record: Dave West
```

正如预期的那样，只有对应于‘Alice Brown’的记录*不是*有效的员工对象。放大输出的相关部分，你可以看到关于为什么`email`和`employee_id`字段无效的详细信息。

这是完整的代码：

```py
# main.py

import json
from pydantic import BaseModel, EmailStr, ValidationError, validator

class Employee(BaseModel):
    name: str
    age: int
    email: EmailStr
    department: str
    employee_id: str

    @validator("employee_id")
     def validate_employee_id(cls, v):
         if not v.isalnum() or len(v) != 6:
             raise ValueError("Employee ID must be exactly 6 alphanumeric characters")
         return v

# Load and parse the JSON data
with open("employees.json", "r") as f:
    data = json.load(f)

# Validate each employee record
for record in data:
    try:
        employee = Employee(**record)
        print(f"Valid employee record: {employee.name}")
    except ValidationError as e:
        print(f"Invalid employee record: {record['name']}")
        print(f"Errors: {e.errors()}")
```

# 总结

本教程到此为止！这是一个 Pydantic 的入门教程。希望你了解了建模数据的基础知识，并使用了 Pydantic 提供的内置和自定义验证。本教程中使用的所有代码都在[GitHub](https://github.com/balapriyac/python-basics/tree/main/pydantic-basics)上。

接下来，你可以尝试在你的 Python 项目中使用 Pydantic，并探索序列化功能。编程愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)** 是一位来自印度的开发者和技术作者。她喜欢在数学、编程、数据科学和内容创作的交汇点工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过撰写教程、操作指南、评论文章等与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编程教程。**

### 更多相关主题

+   [Ollama 教程：本地运行 LLMs 的简单方法](https://www.kdnuggets.com/ollama-tutorial-running-llms-locally-made-super-simple)

+   [让数据分析师使用 BigQuery ML 简化机器学习](https://www.kdnuggets.com/machine-learning-made-simple-for-data-analysts-with-bigquery-ml)

+   [简化 Pandas DataFrames 的合并](https://www.kdnuggets.com/2022/09/combining-pandas-dataframes-made-simple.html)

+   [个性化 AI 变得简单：你的无代码 GPT 适配指南](https://www.kdnuggets.com/personalized-ai-made-simple-your-no-code-guide-to-adapting-gpts)

+   [MarshMallow：用于数据序列化和…的最甜 Python 库](https://www.kdnuggets.com/marshmallow-the-sweetest-python-library-for-data-serialization-and-validation)

+   [使用 Pandera 对 PySpark 应用进行数据验证](https://www.kdnuggets.com/2023/08/data-validation-pyspark-applications-pandera.html)
