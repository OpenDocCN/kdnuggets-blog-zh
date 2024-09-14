# 如何结构化数据科学项目：逐步指南

> 原文：[https://www.kdnuggets.com/2022/05/structure-data-science-project-stepbystep-guide.html](https://www.kdnuggets.com/2022/05/structure-data-science-project-stepbystep-guide.html)

![如何结构化数据科学项目：逐步指南](../Images/85d7d213143775fc7f9ba0a0329c2c44.png)

图片来源于[vectorjuice](https://www.freepik.com/vectorjuice)在[freepik](https://www.freepik.com/free-vector/software-engineer-statistician-visualizer-analyst-working-project-big-data-conference-big-data-presentation-data-science-concept_11667646.htm#query=Data%20Science%20Project&position=0&from_view=search)

在数据科学项目中取得成功需要对发现和探索的专注。但首先，你必须理解这个过程并优化它，以确保结果是可靠的，项目也容易跟进、维护和在必要时修改。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

结构化你的[data science project](/2020/12/14-data-science-projects-improve-skills.html)的最佳和最快的方法是使用一个主模板。你可以在网上找到一些优秀的模板，但要注意，它们可能不会涵盖良好的实践，比如配置、格式化和测试代码。

你需要一些可维护和可重复的东西，并且不需要花费太多时间。所以，查看一个被称为data-science-template的[库](https://github.com/khuyentran1401/data-science-template/blob/master/README.md)可能是个好主意。

# 结构化数据科学项目的优势

结构化与你的数据科学项目相关的数据和源代码有各种优势。这些优势包括：

+   **数据科学团队之间的更好合作/沟通。** 当所有组员都遵循相同的项目结构时，识别其他人所做的修改变得容易。

+   **效率。** 当你使用旧的Jupyter笔记本来重新处理你新的数据科学项目中的某些功能时，你可能最终会在平均10个笔记本之间反复迭代。在这种情况下，发现一段20行的代码可能会让人感到沮丧。当你结构化你的数据科学项目时，你会以一致的安排提交代码，这可以防止重复和自我重复，同时你也更容易找到你所寻找的内容。

+   **可复现性。** 拥有[可复现模型](https://neptune.ai/blog/how-to-solve-reproducibility-in-ml)是至关重要的，以跟踪版本并快速恢复到先前的版本。如果模型失败了，你可以在可复现的方式下组织和记录你的任务，成功确定新模型是否比旧模型表现更好。

+   **数据管理。** 将原始数据与处理过的数据和临时数据分开至关重要。这有助于确保所有参与数据科学项目的团队成员可以轻松复制现有模型。寻找在模型结构阶段使用的相关数据集所花费的时间大大减少。

此外，如果你没有替代用于模型构建的原始数据，一些工具可以帮助你制定一致的项目结构，便于你的数据科学项目的可复现性。

# 如何构建数据科学项目

这里是一些经过验证的[工具和资源](/2021/01/5-tools-effortless-data-science.html)，帮助你成功构建数据科学项目结构：

## Cookiecutter

Cookiecutter 是一个命令行工具，帮助你从提供的模板开发项目。该平台允许你创建自己的项目模板或利用现有模板。这个工具的强大之处在于你可以轻松导入模板，并仅使用适合你的部分。

它的安装非常简单 - 通过安装 Cookiecutter 下载模板即可开始。然后根据该模板创建一个特定的项目，并提供项目的详细信息以开始。

## 安装依赖

你可以使用许多在线平台之一轻松管理依赖关系。这些工具帮助你将[主要和子依赖](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)隔离到两个不同的文件中，而不是将依赖关系存储在 (requirements.txt) 中。

此外，它们帮助你创建清晰的依赖文件，避免下载与当前包冲突的新包，并仅用几行代码设置项目。

## 文件夹

你生成的项目模板结构使你能够安排你的数据、源代码、报告和数据科学工作流中的文件。通过这个结构，你可以监控对项目所做的更改。

这里是你项目应包含的一些文件夹：

+   **模型。** 模型是最终的产品，需要在一致的文件夹安排中存储，以确保你可以在未来复现精确的模型副本。

+   **数据。** 对数据进行分段是未来复制类似结果的关键。你用于构建机器学习模型的数据可能与未来拥有的数据不完全一致，即数据可能在最坏的情况下被覆盖或丢失。因此，为了实现可重复和可维护的机器学习管道，至关重要的是保持所有原始数据不可逆。对原始数据所做的任何进展都需要得到适当的文档记录，这就是文件夹的作用。你不再需要将文档命名为（final2_17_02_2020.csv）、（final_17_02_2020.csv）来跟踪更改。

+   **笔记本。** 各种数据科学项目在Jupyter笔记本中进行，使读者能够理解项目管道。实际上，笔记本中充满了多个代码块和函数，这使得创建者可能忽略代码块的功能。将代码块、结果和函数存储在隔离的文件夹中，让你可以更好地划分项目，并使在笔记本中跟踪项目逻辑变得更加容易。

+   **Src。** Src文件夹存储你在管道中使用的函数。你可以根据它们在功能上的关联来存放这些函数，例如软件产品。此外，你可以轻松调试和 [测试你的过程](https://www.atlantic.net/vps-hosting/most-popular-penetration-testing-tools-in-2021/)，使用它们就像将它们导入笔记本一样简单。

+   **报告。** 数据科学项目不仅生成模型，还产生图表和图形作为数据分析工作流的一部分。这些可以是条形图、平行线、散点图等。你应该存储生成的图形，以便在需要时轻松访问它们。

## Makefile

Makefile 允许数据科学家无缝地构建数据科学项目工作流。此外，该工具还帮助数据科学家记录他们的管道和重现构建的模型。使用Makefile，你可以确保可重复性，并简化数据科学团队内的协作。

## 利用Hydra进行配置文件管理

Hydra，一个Python库，让你可以在Python脚本中访问配置文件中的参数。

配置文件将所有值存储在一个集中位置，帮助你将这些值与代码分开，防止硬编码。所有配置文件都存放在这个模板的“config”目录下。

## 使用DVC管理模型和数据

数据存储在“data”下的子目录中。每个子目录保存来自不同阶段的数据。由于Git不适合版本控制二进制文件，你可以利用 [数据版本控制](https://dvc.org/doc/use-cases/versioning-data-and-model-files)（DVC）来对你的模型和数据进行版本控制。

使用数据版本控制的一个重要好处是，它允许你将平台监控的数据上传到远程存储。此外，你还可以将数据保留在 Google Drive、DagsHub、Amazon S3、Google Cloud Storage、Azure Blob Storage 等地。

## 在提交之前检查代码问题

在提交 Python 代码时，你需要确保你的代码：

+   看起来很有条理

+   包括文档字符串

+   遵循样式指南（PEP 8）

然而，确保所有这些标准在提交代码之前可能会令人感到艰巨。这时，预提交框架就发挥作用了，因为它可以让你在执行代码之前识别代码中的直接问题。

## 添加 API 文档

这要求你作为数据科学家有足够的时间与相关团队成员进行合作。因此，创建准确的项目相关文档至关重要。

# 总结

所以，你现在可以了解到所有必要的步骤来[成功构建](/2019/01/data-science-project-flow-startups.html)你的数据科学项目，利用数据科学模板。这些模板足够灵活，可以帮助你根据具体应用调整项目。

**[Nahla Davies](http://nahlawrites.com/)** 是一名软件开发者和技术作家。在全职投入技术写作之前，她曾在一个 Inc. 5000 的体验品牌组织担任首席程序员，该组织的客户包括三星、时代华纳、Netflix 和索尼。

### 更多相关主题

+   [KDnuggets 新闻，5月11日：专业人士的 SQL 笔记；如何……](https://www.kdnuggets.com/2022/n19.html)

+   [数据科学面试指南 - 第1部分：结构](https://www.kdnuggets.com/2022/04/data-science-interview-guide-part-1-structure.html)

+   [如何成为数据科学家指南（逐步方法）](https://www.kdnuggets.com/2021/05/guide-become-data-scientist.html)

+   [使用 Python 和 Beautiful Soup 的逐步网页抓取指南](https://www.kdnuggets.com/2023/04/stepbystep-guide-web-scraping-python-beautiful-soup.html)

+   [文本到视频生成：逐步指南](https://www.kdnuggets.com/2023/08/text2video-generation-stepbystep-guide.html)

+   [逐步阅读和理解 SQL 查询的指南](https://www.kdnuggets.com/a-step-by-step-guide-to-reading-and-understanding-sql-queries)
