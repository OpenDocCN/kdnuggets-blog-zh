# 告别`Print()`：使用日志模块进行有效调试

> 原文：[`www.kdnuggets.com/say-goodbye-to-print-use-logging-module-for-effective-debugging`](https://www.kdnuggets.com/say-goodbye-to-print-use-logging-module-for-effective-debugging)

![告别 Print()：使用日志模块](img/df8d7bf47fa27eaa8c487cb7ccb88789.png)

作者提供的图片 | DALLE-3 & Canva

许多人开始编程时会通过 YouTube 视频学习，为了简单起见，他们通常使用`print()`语句来跟踪错误。这是可以理解的，但随着初学者养成这个习惯，它可能会变得问题重重。虽然这些语句对于简单的脚本可能有效，但随着代码库的扩展，这种方法变得非常低效。因此，在这篇文章中，我将向你介绍 Python 内置的日志模块，它解决了这个问题。我们将看到什么是日志记录，它与`print()`语句的区别，以及一个实用的例子来全面了解其功能。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织进行 IT 工作

* * *

## 为什么使用日志模块而不是`Print()`？

当我们谈论调试时，Python 的日志模块提供了比简单的`print()`语句更详细的信息。这包括时间戳、模块名称、日志级别以及错误发生的行号等。这些额外的细节帮助我们更有效地理解代码的行为。我们想要记录的信息取决于应用程序的需求和开发者的偏好。因此，在进一步探讨之前，我们先讨论一下日志级别及其设置方法。

## 日志级别

你可以通过这些日志级别控制你想看到的信息量。每个日志级别都有一个数值，表示其严重性，数值越高表示事件越严重。例如，如果你将日志级别设置为`WARNING`，你是在告诉日志模块只显示`WARNING`级别或更高级别的消息。这意味着你将看不到`DEBUG`、`INFO`或其他较低严重性的消息。这样，你可以集中关注重要事件，忽略噪音。

这是一个展示每个日志级别所代表的详细信息的表格：

| 日志级别 | 数值 | 目的 |
| --- | --- | --- |
| DEBUG | 10 | 提供详细的信息以诊断与代码相关的问题，例如打印变量值和函数调用跟踪。 |
| INFO | 20 | 用于确认程序按预期工作，比如显示启动消息和进度指示器。 |
| WARNING | 30 | 表示一个潜在的问题，这可能不会中断程序的执行，但可能会在以后引发问题。 |
| ERROR | 40 | 表示代码的意外行为，这影响了其功能，如异常、语法错误或内存不足错误。 |
| CRITICAL | 50 | 表示严重错误，可能导致程序终止，如系统崩溃或致命错误。 |

## 设置日志模块

要使用日志模块，你需要按照一些配置步骤。这包括创建日志记录器、设置日志级别、创建格式化程序以及定义一个或多个处理器。处理器基本上决定将日志消息发送到哪里，比如控制台或文件。让我们从一个简单的例子开始。我们将设置日志模块做两件事：首先，它将在控制台上显示消息，为我们提供有用的信息（在 `INFO` 级别）。其次，它将把更详细的消息保存到文件中（在 `DEBUG` 级别）。希望你能跟上！

### 1\. 设置日志级别

日志记录器的默认级别设置为 `WARNING`。在我们的例子中，我们的两个处理器被设置为 `DEBUG` 和 `INFO` 级别。因此，为了确保所有消息都得到适当处理，我们必须将日志记录器的级别设置为所有处理器中的最低级别，在这种情况下是 `DEBUG`。

```py
import logging

# Create a logger
logger = logging.getLogger(__name__)

# Set logger level to DEBUG
logger.setLevel(logging.DEBUG)
```

### 2\. 创建一个格式化程序

你可以使用格式化程序个性化你的日志信息。这些格式化程序决定日志信息的显示方式。在这里，我们将设置格式化程序以包含时间戳、日志级别和消息内容，使用以下命令：

```py
formatter = logging.Formatter('%(asctime)s - %(levelname)s - %(message)s')
```

### 3\. 创建处理器

正如之前讨论的，处理器管理日志信息的发送位置。我们将创建两个处理器：一个控制台处理器，用于将消息记录到控制台，一个文件处理器，用于将日志消息写入名为 'app.log' 的文件。

```py
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.INFO)
console_handler.setFormatter(formatter)

file_handler = logging.FileHandler('app.log')
file_handler.setLevel(logging.DEBUG)
file_handler.setFormatter(formatter)
```

然后使用 `addHandler()` 方法将两个处理器添加到日志记录器中。

```py
logger.addHandler(console_handler)
logger.addHandler(file_handler)
```

### 4\. 测试日志设置

现在我们的设置完成了，在转到实际示例之前，让我们测试一下它是否正常工作。我们可以如下记录一些消息：

```py
logger.debug('This is a debug message')
logger.info('This is an info message')
logger.warning('This is a warning message')
logger.error('This is an error message')
logger.critical('This is a critical message')
```

当你运行这段代码时，你应该会看到日志信息被打印到控制台并写入名为 'app.log' 的文件中，格式如下：

**控制台**

```py
2024-05-18 11:51:44,187 - INFO - This is an info message
2024-05-18 11:51:44,187 - WARNING - This is a warning message
2024-05-18 11:51:44,187 - ERROR - This is an error message
2024-05-18 11:51:44,187 - CRITICAL - This is a critical message
```

**app.log**

```py
2024-05-18 11:51:44,187 - DEBUG - This is a debug message
2024-05-18 11:51:44,187 - INFO - This is an info message
2024-05-18 11:51:44,187 - WARNING - This is a warning message
2024-05-18 11:51:44,187 - ERROR - This is an error message
2024-05-18 11:51:44,187 - CRITICAL - This is a critical message
```

## 在 Web 应用程序中记录用户活动

在这个简单的示例中，我们将创建一个基本的网络应用程序，使用 Python 的 logging 模块记录用户活动。这个应用程序将有两个端点：一个用于记录成功的登录尝试，另一个用于记录失败的登录尝试（`INFO` 表示成功，`WARNING` 表示失败）。

### 1\. 设置你的环境

在开始之前，设置你的虚拟环境并安装 Flask：

```py
python -m venv myenv

# For Mac
source myenv/bin/activate

#Install flask
pip install flask
```

### 2\. 创建一个简单的 Flask 应用程序

当你向**/login**端点发送一个包含用户名和密码参数的 POST 请求时，服务器将检查凭据是否有效。如果有效，记录器会使用 logger.info() 记录事件，以表示登录尝试成功。然而，如果凭据无效，记录器会使用 logger.error() 记录事件，标记为登录尝试失败。

```py
#Making Imports
from flask import Flask, request
import logging
import os

# Initialize the Flask app
app = Flask(__name__)

# Configure logging
if not os.path.exists('logs'):
    os.makedirs('logs')
log_file = 'logs/app.log'
logging.basicConfig(filename=log_file, level=logging.DEBUG, format='%(asctime)s - %(levelname)s - %(message)s')
log = logging.getLogger(__name__)

# Define route and handler
@app.route('/login', methods=['POST'])
def login():
    log.info('Received login request')
    username = request.form['username']
    password = request.form['password']
    if username == 'admin' and password == 'password':
        log.info('Login successful')
        return 'Welcome, admin!'
    else:
        log.error('Invalid credentials')
        return 'Invalid username or password', 401

if __name__ == '__main__':
    app.run(debug=True)
```

### 3\. 测试应用程序

要测试应用程序，请运行 Python 脚本，并使用 Web 浏览器或类似 curl 的工具访问**/login**端点。例如：

**测试用例 01**

```py
 curl -X POST -d "username=admin&password=password" http://localhost:5000/login
```

**输出**

```py
Welcome, admin!
```

**测试用例 02**

```py
curl -X POST -d "username=admin&password=wrongpassword" http://localhost:5000/login
```

**输出**

```py
Invalid username or password
```

**app.log**

```py
2024-05-18 12:36:56,845 - INFO - Received login request
2024-05-18 12:36:56,846 - INFO - Login successful
2024-05-18 12:36:56,847 - INFO - 127.0.0.1 - - [18/May/2024 12:36:56] "POST /login HTTP/1.1" 200 -
2024-05-18 12:37:00,960 - INFO - Received login request
2024-05-18 12:37:00,960 - ERROR - Invalid credentials
2024-05-18 12:37:00,960 - INFO - 127.0.0.1 - - [18/May/2024 12:37:00] "POST /login HTTP/1.1" 200 -
```

## 总结

这篇文章到此为止。我强烈建议将日志记录作为你的编码常规的一部分。这是保持代码清洁和简化调试的好方法。如果你想深入了解，可以查看 [Python 日志记录文档](https://docs.python.org/3/library/logging.html) 以获取更多功能和高级技术。如果你希望进一步提升你的 Python 技能，欢迎查阅我其他的一些文章：

+   [掌握 Python：编写清晰、组织良好且高效代码的 7 个策略](https://www.kdnuggets.com/mastering-python-7-strategies-for-writing-clear-organized-and-efficient-code)

+   [8 个内置 Python 装饰器，用于编写优雅代码](https://www.kdnuggets.com/8-built-in-python-decorators-to-write-elegant-code)

[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一名机器学习工程师和技术作家，对数据科学及其与医学的交汇点充满热情。她共同撰写了电子书《利用 ChatGPT 最大化生产力》。作为 2022 年 APAC 的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认可为 Teradata 多样性技术学者、Mitacs Globalink 研究学者以及哈佛 WeCode 学者。Kanwal 是变革的坚定倡导者，创立了 FEMCodes 以赋能 STEM 领域的女性。

### 更多相关话题

+   [如何使用 Scikit-learn 的 Imputer 模块处理缺失数据](https://www.kdnuggets.com/how-to-handle-missing-data-with-scikit-learns-imputer-module)

+   [机器学习的有效测试](https://www.kdnuggets.com/2022/01/effective-testing-machine-learning.html)

+   [为有效的 Tableau 和 Power BI 仪表板准备数据](https://www.kdnuggets.com/2022/06/prepare-data-effective-tableau-power-bi-dashboards.html)

+   [快速有效地审计机器学习公平性的方法](https://www.kdnuggets.com/2023/01/fast-effective-way-audit-ml-fairness.html)

+   [数据可视化最佳实践和有效沟通资源](https://www.kdnuggets.com/2023/04/data-visualization-best-practices-resources-effective-communication.html)

+   [超越猜测：利用贝叶斯统计进行有效的…](https://www.kdnuggets.com/beyond-guesswork-leveraging-bayesian-statistics-for-effective-article-title-selection)
