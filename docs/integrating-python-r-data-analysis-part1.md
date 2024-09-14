# 将 Python 和 R 集成到数据分析管道中，第一部分

> 原文：[`www.kdnuggets.com/2015/10/integrating-python-r-data-analysis-part1.html/2`](https://www.kdnuggets.com/2015/10/integrating-python-r-data-analysis-part1.html/2)

### 命令行脚本

从命令行通过类似 Windows/Linux 的终端环境运行脚本在 R 和 Python 中是类似的。要运行的命令被分解为以下部分，

`<command_to_run> <path_to_script> <any_additional_arguments>`

其中：

+   `<command>` 是要运行的可执行文件（对于 R 代码是 `Rscript`，对于 Python 代码是 `Python`），

+   `<path_to_script>` 是执行脚本的完整或相对文件路径。请注意，如果路径名称中有空格，则整个文件路径必须用双引号括起来。

+   `<any_additional_arguments>` 这是传递给脚本本身的空格分隔参数的列表。请注意，这些参数将作为字符串传递。

例如，通过打开终端环境并运行以下命令来执行 R 脚本：

`Rscript path/to/myscript.R arg1 arg2 arg3`

**一些注意事项**

+   要找到 `Rscript` 和 `Python` 命令，这些可执行文件必须已经在你的路径中。否则，必须提供其在文件系统中的完整路径。

+   路径名称包含空格会造成问题，特别是在 Windows 上，因此必须用双引号括起来，以便被识别为单个文件路径。

#### 访问 R 中的命令行参数

在上面的例子中，`arg1`、`arg2` 和 `arg3` 是传递给正在执行的 R 脚本的参数，可以使用 `commandArgs` 函数来访问。

`## myscript.R`

`# 获取命令行参数 myArgs <- commandArgs(trailingOnly = TRUE)`

`# myArgs 是包含所有参数的字符向量 print(myArgs) print(class(myArgs))`

通过设置 `trailingOnly = TRUE`，向量 `myArgs` 只包含你在命令行上添加的参数。如果保持 `FALSE`（默认值），向量中将包含其他参数，例如刚刚执行的脚本的路径。

#### 访问 Python 中的命令行参数

对于通过在命令行运行以下命令执行的 Python 脚本

`python path/to/myscript.py arg1 arg2 arg3`

参数`arg1`、`arg2` 和 `arg3` 可以通过先导入 `sys` 模块来在 Python 脚本中访问。这个模块包含了系统特定的参数和函数，但我们这里只关注 `argv` 属性。这个 `argv` 属性是一个包含当前执行脚本的所有参数的列表。列表中的第一个元素始终是正在执行脚本的完整文件路径。

`# myscript.py import sys`

`# 获取命令行参数 my_args = sys.argv`

`# my_args 是一个列表，其中第一个元素是被执行的文件。 print(type(my_args)) print(my_args)`

如果你只希望保留解析到脚本中的参数，可以使用列表切片来选择除第一个元素外的所有元素。

`# 使用切片，选择除第一个元素之外的所有元素 my_args = sys.argv[1:]`

与上述 R 的例子类似，记住所有参数都作为字符串解析，因此需要根据需要转换为预期的类型。

### 将输出写入文件

在通过中间文件在 R 和 Python 之间共享数据时，你有几种选择。通常，对于平面文件，CSV 是一种适合表格数据的好格式，而 JSON 或 YAML 则更适合处理结构化较少的数据（或元数据），这些数据可能包含可变数量的字段或更多嵌套的数据结构。

所有这些都是非常常见的 [数据序列化格式](https://en.wikipedia.org/wiki/Serialization)，并且两种语言中都已有解析器。在 R 中，推荐使用以下包来处理每种格式：

+   [readr](https://cran.r-project.org/web/packages/readr/index.html) 用于 CSV 文件

+   [jsonlite](https://cran.r-project.org/web/packages/jsonlite/index.html) 用于 JSON 文件

+   [yaml](http://cran.fhcrc.org/web/packages/yaml/index.html) 用于 YAML 文件

而在 Python 中：

+   [csv](https://docs.Python.org/3/library/csv.html) 用于 CSV 文件

+   [json](https://docs.Python.org/3/library/json.html) 用于 JSON 文件

+   [PyYAML](http://pyyaml.org/wiki/PyYAMLDocumentation) 用于 YAML 文件

csv 和 json 模块是 Python 标准库的一部分，随 Python 本身分发，而 PyYAML 需要单独安装。所有 R 包也需要以通常的方式安装。

### 总结

在 R 和 Python 之间（以及反向）传递数据可以通过以下单一管道完成：

+   使用命令行传递参数，以及

+   通过结构化一致的平面文件传输数据。

然而，在某些情况下，使用平面文件作为中间数据存储既麻烦又对性能有害。在下一篇文章中，我们将探讨如何让 R 和 Python 直接调用对方并在内存中返回输出。

[原文](http://www.mango-solutions.com/wp/2015/10/integrating-python-and-r-into-a-data-analysis-pipeline-part-1/)。

**相关：**

+   R 与 Python：面对面的数据分析

+   数据科学编程：Python 与 R 的对比

+   R 与 Python 的数据科学：赢家是……

+   R，Python 用户表现出令人惊讶的稳定性，但存在显著的区域差异

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

### 更多相关主题

+   [将 ChatGPT 集成到数据科学工作流：技巧与最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [通过集成 Jupyter 和 KNIME 减少实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)

+   [优化数据分析：在 Databricks 中集成 GitHub Copilot](https://www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks)

+   [将生成式 AI 集成到内容创作中](https://www.kdnuggets.com/integrating-generative-ai-in-content-creation)

+   [使用 Python 监控 MLOps 管道中的模型性能](https://www.kdnuggets.com/2023/05/monitor-model-performance-mlops-pipeline-python.html)

+   [使用 Kafka 和 Risingwave 构建 Formula 1 实时数据管道](https://www.kdnuggets.com/building-a-formula-1-streaming-data-pipeline-with-kafka-and-risingwave)
