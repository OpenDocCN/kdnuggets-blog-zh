# 一个 Python 数据处理脚本模板

> 原文：[https://www.kdnuggets.com/2021/08/python-data-processing-script-template.html](https://www.kdnuggets.com/2021/08/python-data-processing-script-template.html)

[评论](#comments)

![图片](../Images/2af4a97469a9a229181550d8825a4383.png)

像你们中的许多人一样，我倾向于编写许多用于执行类似任务的 Python 脚本，或至少遵循类似的功能模式。为了避免重复自己（或者从不同角度看，为了以完全相同的方式重复自己），我喜欢为这些类型的脚本设置模板代码，以保持编程生活尽可能轻松。

我最近写了关于 [作为数据科学家的可重用 Python 代码管理](/2021/06/managing-reusable-python-code-data-scientist.html) 的文章，基于相同的思路，我整理了一个通用的 Python 数据处理脚本模板，这几乎是我现在开始大多数项目时使用的。它随着时间的推移发生了变化并进行了调整，但当前版本是我大多数非专业（即非机器学习）脚本的首选模板。

首先，这里简要介绍一下我现在通常希望通过脚本广泛实现的目标：

+   解析命令行参数

+   设置所需的路径和文件名

+   访问 Google Sheets 进行数据检索和存储

+   访问 SQL 数据库进行数据检索和存储

+   文件系统管理

+   HTML 和其他文本处理

+   使用一些我自己定制的 Python 模块

+   从互联网上检索一些资源，例如 HTML 或 CSV 文件

让我们一步步地查看代码。

### 库

我对导入的内容和方式很讲究。第一行导入（主要是）标准库模块；接下来的几行导入第三方库，顺序为“import”，“import ... as ...”，以及“from ... import ...”；导入一个名为 const 的自定义模块，它作为一个独立的 Python 文件包含不变的项目特定变量赋值（常量）；最后，导入任何我自己定制的 Python 模块，在这种情况下是来自我自己的预处理库的“dates”模块。

```py
import sys, re, datetime, os, glob, argparse, json, yaml, pprint, urllib.request, wget, logging
import gspread
import sqlite3
import pandas as pd
import numpy as np
from bs4 import BeautifulSoup
from oauth2client.service_account import ServiceAccountCredentials

import const
from my_preprocessing import dates
```

### 退出函数

除了错误日志记录，我还添加了一些退出函数，以便在后续代码中调用，原因各异。以下是一些示例，其中第一个是我总是包含的。

```py
def quit():
	"""Program was not initialized correctly"""
	print("example: python template.py -o something_here -b")
	sys.exit(1)

def custom_error(message):
	"""Some other custom error"""
	print("There is a problem: %s" % message)
	sys.exit(2)
```

我喜欢 argparse，见下文，但我喜欢调用一个自定义退出函数，并给出如何初始化命令行脚本的具体示例，而不仅仅是 argparse 的默认消息。第二个函数显然会根据需要被重新用于特定的任务。

### 解析命令行参数

我使用 argparse 来解析命令行参数，为什么不呢？这些参数被包含在我的模板中，方便快速编辑，因此我不需要一直查阅文档。

```py
# Parse and process command line args
parser = argparse.ArgumentParser(description='Python data processing command line template.')
parser.add_argument('required_arg', metavar='req', type=int,
	help='a required arg of type int')
parser.add_argument('-o', '--optional', type=str, default='default_value',
	help='optional arg of type str')
parser.add_argument('-b', '--boolean', action='store_true',
	help='optional boolean arg, False if not used')

# Call custom function for additional console output when parse fails
try:
	args = parser.parse_args()
except SystemExit:
	quit()

# Assign command line args to variables
var_1 = args.req
var_2 = args.optional
var_3 = args.boolean
```

注意在 try/except 块中调用的自定义退出函数，以及将解析的参数分配给更友好的变量名。

### 数据文件名和路径

现在是设置项目特定数据文件名和路径的时候，因为我几乎总是在处理某种数据。将这些存储在 YAML 文件中的好处是它们被集中在一个地方，并且可以轻松更改而无需在代码中搜索。

```py
# Process data filenames
files_yaml = '/path/to/files/config/files.yaml'
with open(files_yaml) as f:
	files_dict = yaml.safe_load(f)
input_file = files_dict['input']
temp_file = files_dict['temp']
output_file = files_dict['output']
database_file = files_dict['example.db']
```

在这里，我从项目 YAML 文件中获取了输入、输出、临时和数据库文件，该文件位于同一目录中，如下所示：

```py
input: '/path/to/project/specific/input/file'

output: '/path/to/project/specific/output/file'

temp: '/path/to/project/specific/temp/file'

database: '/path/to/project/specific/database/file.db'
```

文件名或位置有变化？只需在这里更改一次即可。

### Google Sheets 设置和配置

我已经开始使用 Google Sheets 进行大部分（小规模）数据存储，因此我需要能够访问和操作这些数据。我使用 [gspread](https://github.com/burnash/gspread) 库来做到这一点，配置内容超出了本文的范围。

```py
# Google Sheets API setup
creds_yaml = '/path/to/credentials/config/google_api.yaml'
with open(creds_yaml) as f:
	creds_dict = yaml.safe_load(f)
scope = creds_dict['scope']
creds_file = creds_dict['creds_file']
creds = ServiceAccountCredentials.from_json_keyfile_name(creds_file, scope)
client = gspread.authorize(creds)

# Google Sheets workbook and sheet setup
data_workbook = 'sample-workbook'
data_sheet = client.open(data_workbook).sheet1
```

这段代码从一对 YAML 和 JSON 文件中获取 API 凭据数据，进行身份验证，并连接到特定的工作簿和特定的工作表，以便在后续代码中进行一些操作。

### SQLite 设置和配置

我还定期使用 SQLite3 访问和操作 SQL 数据库。这里是设置和配置数据库连接的代码。

```py
# Create database connection
con = sqlite3.connect(database_file)

# Perform some SQL tasks
cur = con.cursor()
# ...

# Save (commit) the changes
con.commit()

# Close connection
con.close()
```

### BeautifulSoup、字符串操作和使用“常量”

抓取 HTML 是我常做的任务，我通过结合使用 BeautifulSoup 库、简单的字符串操作和正则表达式来完成。下面是设置 BeautifulSoup、删除一些 HTML 标签、使用存储在 const 模块中的一些变量进行简单字符串替换以及使用正则表达式的一些示例代码。这些都是示例代码段，可以在不查阅文档的情况下进行更改。

```py
Using BeautifulSoup for HTML scraping """
# Make the soup
html = input_file.read()
soup = BeautifulSoup(html, 'lxml')

""" Using variables from imported const file """
# Drop invalid tags
for tag in const.INVALID_TAGS: 
	for match in soup.find_all(tag):
		match.replaceWithChildren()

# BeautifulSoup bytes to string
soup_str = str(soup)

# String replacements and footer append
for pair in const.PAIRS:
	soup_str = soup_str.replace(str(pair[0]), str(pair[1]))
soup_str += const.FOOTER_INFO

""" Using regular expressions substitution """
# Remove excess newlines
soup_str = re.sub(r'\n\s*\n\n', '\n', soup_str)

# Output resulting HTML to file, clean up
output_file.write(soup_str)
output_file.close()
input_file.close()
```

在这种情况下，最终结果被保存到文件中，所有打开的文件都被关闭了。

### 自定义库

这是一些自定义库功能的示例。有关此代码创建一些简单日期特征的功能，请查看 [这里](/2021/08/engineer-date-features-python.html)。由于我经常使用我的预处理代码片段集合，它们已被打包为 my_preprocessing 库，我会自动导入。

```py
# Date feature engineering
my_date = dates.process_date('2021-12-31')
pprint.pprint(my_date)
```

### 整个模板

现在我们已经讨论了代码，这里是我每次创建新的数据处理脚本时复制并设置的整个 `template.py` 文件。

我希望有人觉得这有用。

**相关**：

+   [作为数据科学家的可重用 Python 代码管理](/2021/06/managing-reusable-python-code-data-scientist.html)

+   [Python 数据结构比较](/2021/07/python-data-structures-compared.html)

+   [使用 Python 自动化 Microsoft Excel 和 Word](/2021/08/automate-microsoft-excel-word-python.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 方面的工作

* * *

### 了解更多相关话题

+   [免费比率分析模板](https://www.kdnuggets.com/2023/04/boxplot-outlier-free-ratio-analysis-template.html)

+   [25 本免费书籍，掌握 SQL、Python、数据科学、机器学习…](https://www.kdnuggets.com/25-free-books-to-master-sql-python-data-science-machine-learning-and-natural-language-processing)

+   [Python 字符串处理备忘单](https://www.kdnuggets.com/2020/01/python-string-processing-primer.html)

+   [Python 中的并行处理大文件](https://www.kdnuggets.com/2022/07/parallel-processing-large-file-python.html)

+   [KDnuggets 新闻，7 月 20 日：机器学习算法详解…](https://www.kdnuggets.com/2022/n29.html)

+   [自然语言处理任务的数据表示](https://www.kdnuggets.com/2018/11/data-representation-natural-language-processing.html)
