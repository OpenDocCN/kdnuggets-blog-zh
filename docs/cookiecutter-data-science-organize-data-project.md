# Cookiecutter 数据科学：如何组织您的数据科学项目

> 原文：[https://www.kdnuggets.com/2018/07/cookiecutter-data-science-organize-data-project.html](https://www.kdnuggets.com/2018/07/cookiecutter-data-science-organize-data-project.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [DrivenData](https://www.drivendata.org/) 提供**

![Image](../Images/cd61bf44f3394ed05156d00b1e97ee5e.png)

* * *

## 我们的前 3 名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的IT需求

* * *

### 为什么使用这个项目结构？

> 我们不是在谈论细枝末节的缩进美学或拘泥于格式标准——最终，数据科学代码质量关乎正确性和可重复性。

当我们考虑数据分析时，我们通常只关注最终报告、洞察或可视化。虽然这些最终产品通常是主要事件，但很容易专注于使产品*看起来漂亮*，而忽视生成它们的*代码质量*。因为这些最终产品是程序性生成的，**代码质量依然重要**！我们不是在谈论细枝末节的缩进美学或拘泥于格式标准——最终，数据科学代码质量关乎正确性和可重复性。

众所周知，良好的分析通常是非常零散和偶然探索的结果。尝试性实验和快速测试可能不起作用的方法都是获得好结果的过程的一部分，并且没有魔法弹药可以将数据探索转化为简单的线性进展。

话虽如此，一旦开始，它不是一个可以仔细考虑代码或项目布局结构的过程，因此最好从一个干净、逻辑的结构开始，并在整个过程中坚持使用。我们认为使用这样的标准化设置是一个相当大的胜利。原因如下：

### 其他人会感谢你

> 没有人在创建新的Rails项目之前坐下来考虑他们想把视图放在哪里；他们只是运行`rails new`来获得像其他人一样的标准项目骨架。

一个明确定义的标准项目结构意味着新来者可以在不深入阅读大量文档的情况下开始理解分析。这也意味着他们不必在了解非常具体的内容之前阅读100%的代码。

组织良好的代码往往是自我文档化的，因为组织本身为你的代码提供了上下文而无需过多开销。人们会感谢你，因为他们可以：

+   更容易与你合作完成这项分析。

+   从你的分析中学习过程和领域知识。

+   对分析得出的结论感到自信。

一个很好的例子可以在任何主要的网络开发框架中找到，比如 Django 或 Ruby on Rails。在创建一个新的 Rails 项目之前，没有人会坐下来考虑他们想把视图放在哪里；他们只需运行`rails new`来获取一个标准的项目骨架，因为这个默认的项目结构是*合乎逻辑的*且在大多数项目中*合理标准化*，对于从未见过特定项目的人来说，找出各种活动部分会容易得多。

另一个很好的例子是[文件系统层级标准](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)用于类似 Unix 的系统。`/etc`目录有一个非常具体的目的，`/tmp`文件夹也是如此，每个人（或多或少）都同意遵守这个社会契约。这意味着 Red Hat 用户和 Ubuntu 用户都大致知道在何处寻找某些类型的文件，即使在使用对方的系统时——或者说任何其他符合标准的系统！

理想情况下，当同事打开你的数据科学项目时，应该是这样的。

### 你会感谢自己。

曾经尝试过重现几个月前甚至几年前做过的分析吗？你可能写过代码，但现在很难判断你应该使用`make_figures.py.old`、`make_figures_working.py` 还是 `new_make_figures01.py` 来完成任务。以下是一些我们在存在主义恐惧中学会提出的问题：

+   我们是否应该在开始之前先将列 X 加入数据，还是这已经从其中一个笔记本中得到了？

+   说到这一点，我们需要先运行哪个笔记本才能运行绘图代码：是“处理数据”还是“清理数据”？

+   地理图表的 shapefiles 是从哪里下载的？

+   *等等，无限重复。*

这些问题类型是痛苦的，并且是项目组织不善的症状。一个好的项目结构鼓励采用使得重新回到旧工作变得更加容易的实践，例如关注点分离、将分析抽象为一个[DAG](https://en.wikipedia.org/wiki/Directed_acyclic_graph)，以及版本控制等工程最佳实践。

### 这里没有任何约束。

> “愚蠢的一致性是小心眼者的鬼怪” — 拉尔夫·瓦尔多·爱默生（以及[PEP 8!](https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds)）

对几个默认文件夹名称不太满意？正在处理一个稍微非标准的项目，与当前结构不完全匹配？更愿意使用不同于（少数）默认值的包？

**加油吧！** 这是一个轻量级结构，旨在成为许多项目的良好*起点*。或者，正如 PEP 8 所说：

> 项目内的一致性更为重要。一个模块或函数内的一致性是最重要的。... 但要知道何时不一致——有时样式指南的建议不适用。遇到疑问时，运用你最好的判断。查看其他示例，决定什么看起来最好。并且不要犹豫去询问！

### 入门

考虑到这一点，我们创建了一个用于 Python 项目的数据科学 cookiecutter 模板。你的分析不必用 Python 进行，但模板确实提供了一些 Python 的样板代码，你可能需要删除（例如在`src`文件夹中，以及`docs`中的 Sphinx 文档骨架）。

### 需求

+   Python 2.7 或 3.5

+   [cookiecutter Python 包](http://cookiecutter.readthedocs.org/en/latest/installation.html) >= 1.4.0: `pip install cookiecutter`

### 启动一个新项目

启动一个新项目就像在命令行中运行这个命令一样简单。无需先创建目录，cookiecutter 会为你完成这项工作。

```py
cookiecutter https://github.com/drivendata/cookiecutter-data-science

```

### 示例

### 目录结构

```py
├── LICENSE
├── Makefile           <- Makefile with commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.py           <- Make this project pip installable with `pip install -e`
├── src                <- Source code for use in this project.
│   ├── __init__.py    <- Makes src a Python module
│   │
│   ├── data           <- Scripts to download or generate data
│   │   └── make_dataset.py
│   │
│   ├── features       <- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         <- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  <- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
│
└── tox.ini            <- tox file with settings for running tox; see tox.testrun.org

```

### 意见

项目结构中隐含了一些观点，这些观点源于我们在数据科学项目合作中的经验，有些是关于工作流程的，有些是关于简化工作生活的工具的。这些观点是该项目的基础——如果你有想法，请[贡献或分享](https://drivendata.github.io/cookiecutter-data-science/#contributing)。

### 数据是不可变的

切勿编辑你的原始数据，特别是不要手动编辑，尤其是在 Excel 中。不要覆盖你的原始数据。不要保存原始数据的多个版本。将数据（及其格式）视为不可变的。你编写的代码应该将原始数据通过管道传递到最终分析中。你不需要每次都运行所有步骤来生成新的图表（见[分析是一个DAG](https://drivendata.github.io/cookiecutter-data-science/#analysis-is-a-dag)），但任何人都应该能够仅使用`src`中的代码和`data/raw`中的数据重现最终产品。

此外，如果数据是不可变的，它不需要像代码那样进行源代码管理。因此，***默认情况下，数据文件夹包含在 `.gitignore` 文件中。*** 如果你有少量数据，且很少更改，你可能希望将数据包含在仓库中。Github 目前会警告文件超过 50MB，并拒绝超过 100MB 的文件。其他存储/同步大数据的选项包括 [AWS S3](https://aws.amazon.com/s3/) 及其同步工具（例如，[`s3cmd`](http://s3tools.org/s3cmd)）、[Git Large File Storage](https://git-lfs.github.com/)、[Git Annex](https://git-annex.branchable.com/) 和 [dat](http://dat-data.com/)。目前，我们默认要求使用 S3 存储桶，并使用 [AWS CLI](http://docs.aws.amazon.com/cli/latest/reference/s3/index.html) 将 `data` 文件夹中的数据与服务器同步。

### 笔记本用于探索和交流

像 [Jupyter notebook](http://jupyter.org/)、[Beaker notebook](http://beakernotebook.com/)、[Zeppelin](http://zeppelin-project.org/) 等笔记本包以及其他文学编程工具对于探索性数据分析非常有效。然而，这些工具在再现分析结果时可能效果较差。当我们在工作中使用笔记本时，我们通常会将 `notebooks` 文件夹进一步细分。例如，`notebooks/exploratory` 包含初步探索，而 `notebooks/reports` 是更为完善的工作，可以导出为 html 文件到 `reports` 目录。

由于笔记本是源代码管理的挑战性对象（例如，`json` 的差异通常不可读，合并几乎不可能），我们建议不要直接在 Jupyter 笔记本上与他人协作。我们建议有效使用笔记本的两个步骤：

1.  遵循一个命名规范，显示所有者和分析的顺序。我们使用 `<step>-<ghuser>-<description>.ipynb` 的格式（例如，`0.3-bull-visualize-distributions.ipynb`）。

1.  重构好的部分。不要在多个笔记本中编写执行相同任务的代码。如果这是数据预处理任务，将其放入 `src/data/make_dataset.py` 的管道中，并从 `data/interim` 加载数据。如果是有用的工具代码，将其重构到 `src` 中。

现在，默认情况下，我们将项目转换为一个 Python 包（参见 `setup.py` 文件）。你可以导入你的代码并在笔记本中使用类似以下的单元格：

```py
# OPTIONAL: Load the "autoreload" extension so that code can change
%load_ext autoreload

# OPTIONAL: always reload modules so that as you change code in src, it gets loaded
%autoreload 2

from src.data import make_dataset

```

### 分析是一个有向无环图（DAG）

在分析中，通常会有长时间运行的步骤来预处理数据或训练模型。如果这些步骤已经运行过（并且你将输出存储在类似于`data/interim`的目录中），你不希望每次都重新运行它们。我们更喜欢[`make`](https://www.gnu.org/software/make/)来管理相互依赖的步骤，特别是那些长时间运行的步骤。Make是Unix平台上常见的工具（并且[在Windows上也可用](https://drivendata.github.io/cookiecutter-data-science/)）。遵循[`make`文档](https://www.gnu.org/software/make/)、[Makefile约定](https://www.gnu.org/prep/standards/html_node/Makefile-Conventions.html#Makefile-Conventions)和[便携性指南](http://www.gnu.org/savannah-checkouts/gnu/autoconf/manual/autoconf-2.69/html_node/Portable-Make.html#Portable-Make)将帮助确保你的Makefile在不同系统上有效运行。这里有[一些](http://zmjones.com/make/) [示例](https://blog.kaggle.com/2012/10/15/make-for-data-scientists/)供你[入门](https://web.archive.org/web/20150206054212/http://www.bioinformaticszen.com/post/decomplected-workflows-makefiles/)。许多数据人员，包括[Mike Bostock](https://bost.ocks.org/mike/make/)，将`make`作为他们的首选工具。

还有其他一些管理DAG的工具是用Python编写的，而不是使用DSL（例如，[Paver](http://paver.github.io/paver/#)、[Luigi](http://luigi.readthedocs.org/en/stable/index.html)、[Airflow](https://pythonhosted.org/airflow/cli.html)、[Snakemake](https://bitbucket.org/snakemake/snakemake/wiki/Home)、[Ruffus](http://www.ruffus.org.uk/)或[Joblib](https://pythonhosted.org/joblib/memory.html)）。如果这些工具更适合你的分析，请随意使用它们。

### 从环境开始构建。

重现分析的第一步总是重现运行分析的计算环境。你需要相同的工具、相同的库以及相同的版本，以确保一切协调一致。

一种有效的方法是使用[virtualenv](https://virtualenv.pypa.io/en/latest/)（我们推荐使用[virtualenvwrapper](https://virtualenvwrapper.readthedocs.org/en/latest/)来管理虚拟环境）。通过在仓库中列出所有的需求（我们包含了一个`requirements.txt`文件），你可以轻松追踪重建分析所需的包。以下是一个好的工作流程：

1.  在创建新项目时运行`mkvirtualenv`。

1.  使用`pip install`安装分析所需的包。

1.  运行`pip freeze > requirements.txt`以固定用于重建分析的确切包版本。

1.  如果你发现需要安装另一个包，重新运行`pip freeze > requirements.txt`并将更改提交到版本控制系统中。

如果你对重建环境有更复杂的需求，可以考虑基于虚拟机的方法，如[Docker](https://www.docker.com/)或[Vagrant](https://www.vagrantup.com/)。这两种工具使用基于文本的格式（Dockerfile和Vagrantfile），你可以轻松地将其添加到版本控制中，以描述如何创建一个具有所需要求的虚拟机。

### 保持秘密和配置不在版本控制中

你*真的*不希望在Github上泄露你的AWS秘密密钥或Postgres用户名和密码。言尽于此——请参见[Twelve Factor App](http://12factor.net/config)原则。这里有一种方法：

**将你的秘密和配置变量存储在一个特殊的文件中**

在项目根文件夹中创建一个`.env`文件。由于`.gitignore`，此文件不应被提交到版本控制仓库中。以下是一个示例：

```py
# example .env file
DATABASE_URL=postgres://username:password@localhost:5432/dbname
AWS_ACCESS_KEY=myaccesskey
AWS_SECRET_ACCESS_KEY=mysecretkey
OTHER_VARIABLE=something

```

**使用一个包来自动加载这些变量。**

如果你查看`src/data/make_dataset.py`中的存根脚本，它使用了一个名为[python-dotenv](https://github.com/theskumar/python-dotenv)的包，将此文件中的所有条目加载为环境变量，因此可以通过`os.environ.get`访问。以下是从`python-dotenv`文档中改编的示例代码：

```py
# src/data/dotenv_example.py
import os
from dotenv import load_dotenv, find_dotenv

# find .env automagically by walking up directories until it's found
dotenv_path = find_dotenv()

# load up the entries as environment variables
load_dotenv(dotenv_path)

database_url = os.environ.get("DATABASE_URL")
other_variable = os.environ.get("OTHER_VARIABLE")

```

**AWS CLI 配置**

使用Amazon S3存储数据时，管理AWS访问的简单方法是将访问密钥设置为环境变量。然而，在一台机器上管理多个密钥集（例如在多个项目中工作时），最好使用[凭证文件](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html)，通常位于`~/.aws/credentials`。一个典型的文件可能如下所示：

```py
[default]
aws_access_key_id=myaccesskey
aws_secret_access_key=mysecretkey

[another_project]
aws_access_key_id=myprojectaccesskey
aws_secret_access_key=myprojectsecretkey

```

在初始化项目时，你可以添加配置文件名；假设没有设置适用的环境变量，配置文件的凭证将作为默认值使用。

### 在更改默认文件夹结构时要保守

为了使这种结构适用于多种不同类型的项目，我们认为最好的方法是对*你的*项目更改文件夹保持宽容，但对*所有*项目的默认结构保持保守。

我们创建了一个`folder-layout`标签，专门用于提议添加、删除、重命名或移动文件夹的问题。更广泛地说，我们还创建了一个`needs-discussion`标签，用于需要进行仔细讨论和广泛支持的问题。

### 贡献

Cookiecutter Data Science项目有其明确的观点，但不怕犯错。最佳实践在变化，工具在进化，经验在积累。**这个项目的目标是让开始、结构化和共享分析变得更容易。** 鼓励[拉取请求](https://github.com/drivendata/cookiecutter-data-science/pulls)和[提交问题](https://github.com/drivendata/cookiecutter-data-science/issues)。我们很想知道什么对你有效，什么无效。

如果你使用了Cookiecutter数据科学项目，请链接回此页面或 [给我们留言](https://twitter.com/drivendataorg) 并 [告诉我们](mailto:info@drivendata.org)!

### 相关项目和参考文献链接

项目结构和可重复性在R研究社区中有更多讨论。如果你在使用R，这里有一些项目和博客文章可能对你有所帮助。

+   [项目模板](http://projecttemplate.net/index.html) - 一个R数据分析模板

+   "[设计项目](http://nicercode.github.io/blog/2013-04-05-projects/)" 在Nice R Code上

+   "[我的研究工作流程](http://www.carlboettiger.info/2012/05/06/research-workflow.html)" 在Carlboettifer.info上

+   "[计算生物学项目的快速指南](http://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1000424)" 在PLOS计算生物学上

最后，感谢[Cookiecutter](https://cookiecutter.readthedocs.org/en/latest/)项目 ([github](https://github.com/audreyr/cookiecutter))，它帮助我们减少了在编写模板代码上的时间，使我们能够将更多时间用于完成实际工作。

**简介: [DrivenData](https://www.drivendata.org/)** 是一家以使命驱动的数据科学公司，将数据科学、机器学习和人工智能的强大能力带到应对全球最大挑战的组织。DrivenData Labs (drivendata.co) 帮助以使命为导向的组织利用数据更聪明地工作，提供更有影响力的服务，并充分发挥机器智能的潜力。DrivenData 还举办在线机器学习竞赛 (drivendata.org)，在这里，全球热情的数据科学家为社会影响构建算法。

[原版](https://drivendata.github.io/cookiecutter-data-science/)。经授权转载。

**相关:**

+   [与机器学习算法相关的数据结构](/2018/01/data-structures-related-machine-learning-algorithms.html)

+   [Python正则表达式备忘单](/2018/04/python-regular-expressions-cheat-sheet.html)

+   [Python中的函数式编程介绍](/2018/02/introduction-functional-programming-python.html)

### 更多相关话题

+   [成为优秀数据科学家所需的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每个初学者数据科学家应掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [停止学习数据科学以寻找目标，并找到目标以...](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [建立一个稳固的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)
