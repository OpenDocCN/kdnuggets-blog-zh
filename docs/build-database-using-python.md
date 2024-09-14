# 如何使用 Python 构建数据库

> 原文：[https://www.kdnuggets.com/2021/09/build-database-using-python.html](https://www.kdnuggets.com/2021/09/build-database-using-python.html)

[评论](#comments)

**作者 [Irfan Alghani Khalid](https://www.linkedin.com/in/alghaniirfan/)，计算机科学学生**

![](../Images/ce52fd46adf211dce1be3d55a04ee65c.png)

由 [Taylor Vick](https://unsplash.com/@tvick?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 摄影，刊登于 [Unsplash](https://unsplash.com/s/photos/server?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 介绍

SQLAlchemy 是一个 Python 库，用于在不使用 SQL 语言本身的情况下实现 SQL 数据库。换句话说，你只需使用 Python 语言来实现你的数据库。

Flask-SQLAlchemy 是一个用于在 Flask 项目中连接 SQLAlchemy 库的库，它使得你的数据库实现比以往任何时候都要容易。本文将向你展示如何使用 Flask-SQLAlchemy 库构建你的数据库。

不再赘述，我们开始吧！

## 数据库

在进入实现之前，让我先解释一下什么是数据库。数据库是一个数据集合，这些数据彼此集成，我们可以通过计算机来访问它。

在数据科学中，你可能会以类似电子表格的形式插入和分析数据。在软件开发领域，这略有不同。我们来看看这个电子表格。

![](../Images/6e533ae64100beee9b91915fe965f58d.png)

这张图片由作者拍摄。

在电子表格中，我们可以看到有两列。分别是书籍标题和作者姓名。如果你查看作者列，你会发现一些值重复了几次。这种情况我们称为冗余。

将整个数据集作为一个表格使用并不是最佳实践，特别是对于那些想要建立网站或应用程序的人来说。相反，我们需要将表格分开，这个过程叫做规范化。

总结来说，规范化过程将把数据集分成几个表格，每个表格都有其唯一的标识符，我们将这些标识符称为主键。如果我们分开上述数据集，它将会像这样：

![](../Images/50d8ae1eb5820bbc625bca7eae4307c1.png)

这张图片由作者拍摄。

从上面可以看出，数据集已经分成了两个表格。分别是书籍表和作者表。作者的姓名在作者表中。因此，我们不能像在电子表格中那样直接访问姓名。

要检索作者的姓名，我们必须通过从作者表中获取 id 来连接书籍表和作者表。我们将作者的 id 视为书籍表的外键。

这可能很复杂，但如果你开始为应用程序实现数据库，它将提高你应用程序的性能。

## 实现

### 安装库

在我们进入实现之前，首先需要做的是在计算机上安装库。为此，我们可以使用 pip。下面是安装库的语法：

要加载库，我们可以调用以下语法：

正如你从库的名称中知道的那样，我们还需要加载 Flask 库。

## 启动数据库引擎

加载库后，下一步是设置我们的 SQLAlchemy 对象和数据库路径。默认情况下，SQLAlchemy 附带 SQLite 软件。

SQLite 是一种数据库管理系统，我们可以在其中构建和分析我们建立的数据库。你可以使用其他 DBMS，如 MySQL、PostgreSQL 或任何你喜欢的 DBMS。要设置我们的数据库，请添加以下代码行：

这段代码将初始化 Flask 和 SQLAlchemy 对象，并设置一个包含数据库路径的参数。在这种情况下，数据库路径是 sqlite:///C:\\sqlite\\library.db。

## 实现数据库

在我们设置好对象和参数之后，我们可以开始实现数据库。让我们回顾一下我们之前的数据库表：

![](../Images/7b9c4c9111b45a12f0401e85f8eff70b.png)

这张图片由作者拍摄。

从上面可以看出，有两个表。每个表将有它的类，我们可以在其中初始化列名和表之间的关系。我们将继承一个名为 Model 的类来实现我们的表。根据上面的图示，代码如下：

## 插入值

创建类之后，我们可以构建我们的数据库。要构建数据库，你可以访问终端并运行以下命令：

现在让我们尝试在表中插入值。以这个示例为例，让我们输入之前电子表格中的数据。你可以按照以下代码插入值：

## 查询表格

在你向表中插入值之后，现在让我们运行查询以查看数据是否存在。以下是执行此操作的代码：

正如你从上面看到的，我们的提交成功了。虽然这仍然是一个介绍，但我希望你能掌握这个概念并在更大的项目中实现它。

## 附加内容：SQL 查询

除了 Flask-SQLAlchemy 库外，我还将演示如何使用原生 SQLite 访问数据库。我们这样做是为了确保我们的数据库已经创建。要访问 SQLite，你可以打开终端并编写以下脚本：

```py
**sqlite3**
```

之后，它将显示类似这样的 SQLite 界面：

![](../Images/1d9e8b3434bc88c1343b334ec12dc725.png)

这张图片由作者拍摄。

在下一步中，你可以使用 .open 命令打开数据库，并像这样添加数据库路径：

```py
**.open absolute//path//to//your//database.db**
```

为了确保我们已打开数据库，请在终端中输入 .tables，如下所示：

```py
**.tables**
```

它将生成如下结果：

![](../Images/4875eeee1a17496754dd8de667edd35b.png)

这张图片由作者拍摄。

很好，成功了。现在让我们尝试使用 SQL 语言查询我们的数据库：

```py
**SELECT * FROM books**
```

这是结果：

![](../Images/9b320fb36608d78b40b98eebc96c8044.png)

图片由作者拍摄。

现在让我们尝试将两个表合并为一个。你可以在终端中输入这行代码：

```py
**SELECT Book.title, Author.name FROM Book
INNER JOIN Author ON Book.author_id = Author.id;**
```

结果如下：

![](../Images/89026163943c619d7c977e4804f93a21.png)

图片由作者拍摄。

## 最后的评论

做得好！现在你已经使用 Flask-SQLAlchemy 库实现了你的数据库。我希望这篇文章对你在项目中实现数据库有所帮助，特别是当你想使用 Flask 构建网页应用时。

如果你对我的文章感兴趣，可以在 Medium 上关注我，也可以订阅我的通讯。如果你有任何问题或只是想打个招呼，也可以在 [**LinkedIn**](https://www.linkedin.com/in/alghaniirfan/) 上关注我。

感谢阅读我的文章！

**简介: [Irfan Alghani Khalid](https://www.linkedin.com/in/alghaniirfan/)** 是 IPB 大学的计算机科学学生，感兴趣于数据科学、机器学习和开源。

[原文](https://towardsdatascience.com/how-to-build-a-database-using-python-f4b62a19d190)。转载已获许可。

**相关内容:**

+   [如何用几行代码和 Flash 构建图像分类器](/2021/07/build-image-classifier-in-few-lines-of-code-with-flash.html)

+   [原生 Python 中的简单 SQL](/2021/09/easy-sql-native-python.html)

+   [如何查询你的 Pandas 数据框](/2021/08/query-pandas-dataframe.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 在 IT 方面支持你的组织

* * *

### 更多相关主题

+   [每位数据科学家都应该知道的三款 R 语言库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [是什么让 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [停止学习数据科学以寻找目标，并找到目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一个 90 亿美元的 AI 失败，深入分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [数据科学学习统计学的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
