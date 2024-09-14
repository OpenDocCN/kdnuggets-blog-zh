# 3 种有趣的 Python 上下文管理器用法

> 原文：[https://www.kdnuggets.com/3-interesting-uses-of-python-context-managers](https://www.kdnuggets.com/3-interesting-uses-of-python-context-managers)

![3 种有趣的 Python 上下文管理器用法](../Images/4634679c3b9355581de61827cbecda51.png)

图片来源 [Freepik](https://www.freepik.com/free-vector/young-programmer-working-laptop-computer-cartoon-character_33906144.htm#query=python%20programmer&position=7&from_view=search&track=ais&uuid=660bb4cc-54bf-4668-8bd8-114802f3425e) 的 johnstocker

前一段时间，我写了一篇关于 [编写高效 Python 代码](/how-to-write-efficient-python-code-a-tutorial-for-beginners) 的教程。在其中，我讨论了如何使用上下文管理器和 with 语句高效地管理资源。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织进行 IT 工作

* * *

我使用了一个简单的文件处理示例，展示了当执行退出 with 块时，文件会被自动关闭——即使发生了异常。

虽然文件处理是一个很好的初步示例，但它很快会变得乏味。这就是为什么我想在本教程中介绍其他有趣的上下文管理器用法——超越文件处理。我们将重点讲解处理数据库连接、管理子进程以及高精度浮点运算。

# Python 中的上下文管理器是什么？

Python 中的上下文管理器使你在处理资源时能够编写更简洁的代码。它们通过以下方式提供了设置和拆除资源的简洁语法：

+   一个**进入**逻辑，当执行进入上下文时会被调用

+   一个**退出**逻辑，当执行退出上下文时会被调用

最简单的示例是文件处理。在这里，我们在 `with` 语句中使用 `open()` 函数来获取文件处理器：

```py
with open('filename.txt', 'w') as file:
    file.write('Something random')
```

这会获取资源——文件对象——在代码块中使用（我们写入文件）。当执行退出上下文时，文件会被关闭；因此不会有资源泄漏。

你可以像这样编写通用版本：

```py
with some_context() as ctx:
    # do something useful on the resource!

# resource cleanup is automatic 
```

现在让我们继续看具体的示例。

# 1\. 处理数据库连接

当你构建 Python 应用程序时，连接到数据库并查询它们包含的表是相当常见的。执行此操作的工作流程如下：

+   安装数据库连接器以便与数据库配合使用（例如用于 Postgres 的 psycopg2 和用于 MySQL 数据库的 mysql-connector-python）。

+   解析配置文件以检索连接参数。

+   使用 `connect()` 函数建立与数据库的连接。

![Python 上下文管理器的 3 个有趣用途](../Images/a78e3dbbd7547d349932a4affb886e73.png)

连接到数据库 | 图片来自作者

一旦你连接到数据库，你可以创建一个数据库来查询数据库。运行查询并使用 run 和 fetch 游标方法获取查询结果。

![Python 上下文管理器的 3 个有趣用途](../Images/00ab54842a3ba36c0eaeb04eecef89e3.png)

查询数据库 | 图片来自作者

这样，你创建了以下资源：一个数据库连接和一个数据库游标。现在让我们编写一个简单的通用示例，看看如何将连接和游标对象用作上下文管理器。

## 解析 Python 中的 TOML 文件

考虑一个示例 TOML 文件，例如 db_config.toml，包含连接数据库所需的信息：

```py
# db_config.toml

[database]
host = "localhost"
port = 5432
database_name = "your_database_name"
user = "your_username"
password = "your_password"
```

> **注意**：你需要 Python 3.11 或更高版本才能使用 [tomllib](https://docs.python.org/3/library/tomllib.html)。

Python 有一个内置的 [tomllib](https://docs.python.org/3/library/tomllib.html) 模块（在 Python 3.11 中引入），可以让你解析 TOML 文件。因此，你可以打开 db_config.toml 文件并像这样解析其内容：

```py
import tomllib

with open('db_config.toml','rb') as file:
	credentials = tomllib.load(file)['database']
```

注意，我们访问了 db_config.toml 文件中的 ‘database’ 部分。`load()` 函数返回一个 Python 字典。你可以通过打印 `credentials` 的内容来验证这一点：

```py
print(credentials)
```

```py
Output >>>
{'host': 'localhost', 'port': 5432, 'database_name': 'your_database_name', 'user': 'your_username', 'password': 'your_password'}
```

## 连接到数据库

假设你想连接到一个 Postgres 数据库。你可以使用 pip 安装 [psycopg2 连接器](https://www.psycopg.org/docs/)：

```py
pip install psycopg2
```

你可以像这样在 `with` 语句中使用连接和游标对象：

```py
import psycopg2

# Connect to the database
with psycopg2.connect(**credentials) as conn:
	# Inside this context, the connection is open and managed

	with conn.cursor() as cur:
    	# Inside this context, the cursor is open and managed

    	    cur.execute('SELECT * FROM my_table')
    	    result = cur.fetchall()
            print(result)
```

在这段代码中：

+   我们使用 `with` 语句来创建一个管理数据库连接的上下文。

+   在这个上下文中，我们创建了另一个上下文来管理数据库游标。游标在退出这个内部上下文时会自动关闭。

+   由于在退出外部上下文时连接也会被关闭，这种构造确保了连接和游标都得到正确管理——减少了资源泄漏的可能性。

在处理 SQLite 和 MySQL 数据库时，你也可以使用类似的构造。

# 2\. 管理 Python 子进程

Python 的 subprocess 模块提供了在 Python 脚本中运行外部命令的功能。`subprocess.Popen()` 构造函数创建一个新的子进程。你可以在 `with` 语句中像这样使用它：

```py
import subprocess

# Run an external command and capture its output
with subprocess.Popen(['ls', '-l'], stdout=subprocess.PIPE, text=True) as process:
	output, _ = process.communicate()
	print(output)
```

在这里，我们运行 Bash 命令 `ls -l` 来长列表显示当前目录中的文件：

```py
Output >>>

total 4
-rw-rw-r-- 1 balapriya balapriya   0 Jan  5 18:31 db_info.toml
-rw-rw-r-- 1 balapriya balapriya 267 Jan  5 18:32 main.py
```

一旦退出 `with` 语句的上下文，子进程相关的资源会被释放。

# 3\. 高精度浮点运算

Python 中的内置 float 数据类型不适合高精度浮点运算。但在处理财务数据、传感器读数等时，你确实需要高精度。对于这种应用，你可以使用 [decimal](https://docs.python.org/3/library/decimal.html) 模块。

`localcontext()` 函数 [返回一个上下文管理器](https://docs.python.org/3/library/decimal.html#decimal.localcontext)。因此，你可以在 `with` 语句中使用 `localcontext()` 函数，并按如下方式设置当前上下文的精度：

```py
from decimal import Decimal, localcontext

with localcontext() as cur_context:
    cur_context.prec = 40
    a = Decimal(2)
    b = Decimal(3)
    print(a/b)
```

输出如下：

```py
Output >>>
0.6666666666666666666666666666666666666667
```

在这里，精度设置为 40 位小数——但仅在当前 `with` 块内。当执行退出当前上下文时，精度将恢复为默认精度（28 位小数）。

# 总结

在本教程中，我们学习了如何使用上下文管理器处理数据库连接、管理子进程以及在高精度浮点运算中的上下文。

在下一节教程中，我们将探讨如何在 Python 中创建自定义上下文管理器。在此之前，祝编程愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正致力于通过撰写教程、使用指南、评论文章等方式学习并与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编程教程。**

### 更多相关内容

+   [如何在 Python 中创建自定义上下文管理器](https://www.kdnuggets.com/how-to-create-custom-context-managers-in-python)

+   [LinkedIn 如何使用机器学习来排序你的动态](https://www.kdnuggets.com/2022/11/linkedin-uses-machine-learning-rank-feed.html)

+   [顶级编程语言及其用途](https://www.kdnuggets.com/2021/05/top-programming-languages.html)

+   [KDnuggets™ 新闻 22:n04，1 月 26 日：高薪兼职……](https://www.kdnuggets.com/2022/n04.html)

+   [KDnuggets 新闻，11 月 16 日：LinkedIn 如何使用机器学习 •…](https://www.kdnuggets.com/2022/n45.html)

+   [天高任鸟飞：了解 JetBlue 如何使用 Monte Carlo 和 Snowflake……](https://www.kdnuggets.com/2022/12/monte-carlo-jetblue-snowflake-build-trust-improve-model-accuracy.html)
