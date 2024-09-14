# 使用Bash构建您的第一个ETL管道

> 原文：[https://www.kdnuggets.com/building-your-first-etl-pipeline-with-bash](https://www.kdnuggets.com/building-your-first-etl-pipeline-with-bash)

![使用Bash构建第一个ETL管道](../Images/25385ed4de8b6638a1ca5f30f762b1f7.png)

图片由作者提供 | Midjourney & Canva

## 介绍

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的IT工作

* * *

ETL，即提取、转换、加载，是一个必要的数据工程过程，涉及从各种来源提取数据，将其转换为可用的格式，并将其移至某个目的地，如数据库。ETL管道自动化了这一过程，确保数据以一致和高效的方式处理，为数据分析、报告和机器学习等任务提供框架，并确保数据干净、可靠且可用。

Bash，缩写自Bourne-Again Shell — 又名Unix Shell — 是一个强大的工具，用于构建ETL管道，因其简洁性、灵活性和极广泛的适用性，因此是初学者和资深专家的绝佳选择。Bash脚本可以自动化任务、移动文件和与命令行上的其他工具进行交互，这使它成为ETL工作的良好选择。此外，bash在类Unix系统（如Linux、BSD、macOS等）中无处不在，因此在大多数这类系统上无需额外操作即可使用。

本文针对的是希望构建第一个ETL管道的初学者和实践数据科学家、数据工程师。假设读者具备基本的命令行理解，并旨在提供一个使用bash创建ETL管道的实用指南。文章的最终目标是引导读者完成构建基础ETL管道的过程，通过这篇文章，读者将能够了解如何实现一个从源提取数据、转换数据并将其加载到目标数据库的ETL管道。

## 设置您的环境

在开始之前，请确保您具备以下内容：

+   一个基于Unix的系统（Linux或macOS）

+   Bash shell（通常在Unix系统上预装）

+   基本的命令行操作理解

对于我们的ETL管道，我们将需要这些特定的命令行工具：

+   `curl`

+   `jq`

+   `awk`

+   `sed`

+   `sqlite3`

您可以使用系统的包管理器安装它们。在基于Debian的系统上，您可以使用`apt-get`：

```py
sudo apt-get install curl jq awk sed sqlite3
```

在macOS上，您可以使用`brew`：

```py
brew install curl jq awk sed sqlite3 
```

让我们为 ETL 项目设置一个专用目录。打开终端并运行：

```py
mkdir ~/etl_project
cd ~/etl_project 
```

这会创建一个名为 `etl_project` 的新目录并进入该目录。

## 提取数据

数据可以来自各种来源，如 API、CSV 文件或数据库。在本教程中，我们将演示如何从公共 API 和 CSV 文件中提取数据。

让我们使用 `curl` 从公共 API 中获取数据。例如，我们将从一个提供样本数据的模拟 API 中提取数据。

```py
# Fetching data from a public API
curl -o data.json "https://api.example.com/data"
```

这个命令将下载数据并保存为 `data.json`。

我们也可以使用 `curl` 从远程服务器下载一个 CSV 文件。

```py
# Downloading a CSV file
curl -o data.csv "https://example.com/data.csv"
```

这将把 CSV 文件保存为我们工作目录中的 `data.csv`。

## 数据转换

数据转换是将原始数据转换为适合分析或存储的格式的必要步骤。这可能涉及解析 JSON、过滤 CSV 文件或清理文本数据。

`jq` 是一个处理 JSON 数据的强大工具。我们将使用它从 JSON 文件中提取特定字段。

```py
# Parsing and extracting specific fields from JSON
jq '.data[] | {id, name, value}' data.json > transformed_data.json
```

这个命令从 JSON 数据中的每个条目中提取 `id`、`name` 和 `value` 字段，并将结果保存到 `transformed_data.json` 中。

`awk` 是一个用于处理 CSV 文件的多功能工具。我们将使用它从我们的 CSV 文件中提取特定的列。

```py
# Extracting specific columns from CSV
awk -F, '{print $1, $3}' data.csv > transformed_data.csv
```

这个命令从 `data.csv` 中提取第一列和第三列，并将它们保存到 `transformed_data.csv`。

`sed` 是一个用于过滤和转换文本的流编辑器。我们可以用它来执行文本替换和清理数据。

```py
# Replacing text in a file
sed 's/old_text/new_text/g' transformed_data.csv
```

这个命令将 `transformed_data.csv` 中的 `old_text` 替换为 `new_text`。

## 加载数据

常见的数据加载目的地包括数据库和文件。对于本教程，我们将使用 SQLite，一种常用的轻量级数据库。

首先，让我们创建一个新的 SQLite 数据库和一个表来存储我们的数据。

```py
# Creating a new SQLite database and table
sqlite3 etl_database.db "CREATE TABLE data (id INTEGER PRIMARY KEY, name TEXT, value REAL);"
```

这个命令创建一个名为 `etl_database.db` 的数据库文件，并创建一个名为 `data` 的表，该表有三列。

接下来，我们将把转换后的数据插入到 SQLite 数据库中。

```py
# Inserting data into SQLite database
sqlite3 etl_database.db <<EOF
.mode csv
.import transformed_data.csv data
EOF
```

这段命令将模式设置为 CSV 并将 `transformed_data.csv` 导入到 `data` 表中。

我们可以通过查询数据库来验证数据是否正确插入。

```py
# Querying the database
sqlite3 etl_database.db "SELECT * FROM data;"
```

这个命令检索 `data` 表中的所有行并显示它们。

## 最终思考

我们在使用 bash 构建 ETL 管道时涵盖了以下步骤，包括：

1.  环境设置和工具安装

1.  使用 `curl` 从公共 API 和 CSV 文件中提取数据

1.  使用 `jq`、`awk` 和 `sed` 进行数据转换

1.  使用 `sqlite3` 在 SQLite 数据库中加载数据

Bash 是 ETL 的一个不错选择，因为它简单、灵活、自动化能力强，并且与其他 CLI 工具兼容。

为了进一步调查，可以考虑加入错误处理、通过 cron 调度管道，或学习更高级的 bash 概念。你也可以探索其他转换应用和方法，以提升你的管道技能。

尝试自己的 ETL 项目，将所学应用于更复杂的场景。幸运的话，这里的一些基本概念将成为你进行更复杂数据工程任务的良好起点。

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 [KDnuggets](https://www.kdnuggets.com/) 和 [Statology](https://www.statology.org/) 的主编，以及 [Machine Learning Mastery](https://machinelearningmastery.com/) 的特邀编辑，Matthew 旨在使复杂的数据科学概念变得易于理解。他的专业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的人工智能技术。他致力于在数据科学社区普及知识。Matthew 从 6 岁起就开始编程。

### 更多相关话题

+   [ETL 与 ELT：哪个更适合你的数据管道？](https://www.kdnuggets.com/2023/03/etl-elt-one-right-data-pipeline.html)

+   [构建可扩展的 ETL 系统与 SQL + Python](https://www.kdnuggets.com/2022/04/building-scalable-etl-sql-python.html)

+   [为多变量时间序列构建可处理的特征工程管道](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [利用 Kafka 和 Risingwave 构建 Formula 1 实时数据管道](https://www.kdnuggets.com/building-a-formula-1-streaming-data-pipeline-with-kafka-and-risingwave)

+   [使用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [逐步教程：构建你的第一个机器学习模型](https://www.kdnuggets.com/step-by-step-tutorial-to-building-your-first-machine-learning-model)
