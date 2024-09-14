# 用 Python 轻松构建命令行应用程序的 7 个步骤

> 原文：[https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps](https://www.kdnuggets.com/build-a-command-line-app-with-python-in-7-easy-steps)

![用 Python 轻松构建命令行应用程序的 7 个步骤](../Images/adfbc1490240615cbb4432af75a24c12.png)

作者提供的图片

构建简单项目是学习 Python 和任何编程语言的绝佳方式。你可以学习写 for 循环的语法、使用内置函数、读取文件等等。但是，只有当你开始构建某些东西时，你才会真正“学习”。

采用“通过构建学习”的方法，让我们编码一个可以在命令行中运行的简单待办事项应用程序。在此过程中，我们将探索诸如解析命令行参数、处理文件和文件路径等概念。我们还将重新审视定义自定义函数等基础知识。

所以让我们开始吧！

# 你将构建的内容

通过跟随本教程进行编码，你将能够构建一个可以在命令行中运行的待办事项应用程序。好吧，那么你希望这个应用程序具备什么功能呢？

像纸上的待办事项列表一样，你需要能够添加任务、查看所有任务，并在完成后删除任务（是的，在纸上划掉或标记完成）。所以我们将构建一个可以实现以下功能的应用程序。

将任务添加到列表中：

![用 Python 轻松构建命令行应用程序的 7 个步骤](../Images/30e5521fc97f097d0038bc3e4fe3c4f3.png)

作者提供的图片

获取列表中所有任务的清单：

![用 Python 轻松构建命令行应用程序的 7 个步骤](../Images/122c08c7195318e0e83e01aa44bd4882.png)

作者提供的图片

以及在完成后使用索引删除任务：

![用 Python 轻松构建命令行应用程序的 7 个步骤](../Images/85939a9a2447db626be2653cf89a2e53.png)

作者提供的图片

现在让我们开始编码吧！

# 第 1 步：开始

首先，为你的项目创建一个目录。在项目目录中，创建一个 Python 脚本文件。这将是我们的待办事项列表应用程序的主要文件。我们称之为 `todo.py`。

这个项目不需要任何第三方库。因此，只需确保你使用的是最新版本的 Python。本教程使用 Python 3.11。

# 第 2 步：导入必要的模块

在 `todo.py` 文件中，首先导入所需的模块。对于我们的简单待办事项列表应用程序，我们需要以下模块：

+   [argparse](https://docs.python.org/3/library/argparse.html) 用于命令行参数解析

+   [os](https://docs.python.org/3/library/os.html) 用于文件操作

所以让我们导入这两个模块：

```py
import argparse
import os
```

# 第 3 步：设置参数解析器

请记住，我们将使用命令行标志来添加、列出和删除任务。对于每个参数，我们可以使用短选项和长选项。对于我们的应用程序，使用如下选项：

+   `-a` 或 `--add` 用于添加任务

+   `-l` 或 `--list` 用于列出所有任务

+   `-r` 或 `--remove` 用于通过索引删除任务

这里我们将使用 argparse 模块来解析在命令行提供的参数。我们定义了`create_parser()`函数，完成以下操作：

+   初始化一个`ArgumentParser`对象（我们称之为`parser`）。

+   为添加、列出和删除任务添加参数，通过在解析器对象上调用`add_argument()`方法。

添加参数时，我们添加了短选项和长选项以及相应的帮助信息。所以这是`create_parser()`函数：

```py
def create_parser():
    parser = argparse.ArgumentParser(description="Command-line Todo List App")
    parser.add_argument("-a", "--add", metavar="", help="Add a new task")
    parser.add_argument("-l", "--list", action="store_true", help="List all tasks")
    parser.add_argument("-r", "--remove", metavar="", help="Remove a task by index")
    return parser
```

# 第 4 步：添加任务管理函数

我们现在需要定义函数来执行以下任务管理操作：

+   添加任务

+   列出所有任务

+   根据索引移除任务

以下函数`add_task`与一个简单的文本文件交互，以管理 TO-DO 列表中的项目。它以‘追加’模式打开文件，并将任务添加到列表末尾：

```py
def add_task(task):
    with open("tasks.txt", "a") as file:
    file.write(task + "\n")
```

注意我们如何使用`with`语句来管理文件。这样可以确保文件在操作后关闭——即使发生错误——从而最小化资源泄漏。

要了解更多信息，请阅读[本教程中的上下文管理器章节，以高效处理资源](/how-to-write-efficient-python-code-a-tutorial-for-beginners)。

`list_tasks`函数通过检查文件是否存在来列出所有任务。文件仅在你添加第一个任务时创建。我们首先检查文件是否存在，然后读取并打印任务。如果当前没有任务，我们会得到一条有用的信息：

```py
def list_tasks():
    if os.path.exists("tasks.txt"):
        with open("tasks.txt", "r") as file:
            tasks = file.readlines()
        	for index, task in enumerate(tasks, start=1):
                print(f"{index}. {task.strip()}")
    else:
        print("No tasks found.")
```

我们还实现了一个`remove_task`函数来根据索引删除任务。以‘写入’模式打开文件会覆盖现有文件。所以我们删除对应索引的任务，并将更新后的 TO-DO 列表写入文件：

```py
def remove_task(index):
    if os.path.exists("tasks.txt"):
        with open("tasks.txt", "r") as file:
            tasks = file.readlines()
        with open("tasks.txt", "w") as file:
            for i, task in enumerate(tasks, start=1):
                if i != index:
                    file.write(task)
        print("Task removed successfully.")
    else:
        print("No tasks found.")
```

# 第 5 步：解析命令行参数

我们已经设置了解析器来解析命令行参数。同时，我们也定义了执行添加、列出和删除任务的函数。那么接下来是什么？

你可能已经猜到了。我们只需根据接收到的命令行参数调用正确的函数。让我们定义一个`main()`函数，使用我们在第 3 步创建的`ArgumentParser`对象来解析命令行参数。

根据提供的参数，调用相应的任务管理函数。这可以通过一个简单的 if-elif-else 梯形结构来完成，如下所示：

```py
def main():
    parser = create_parser()
    args = parser.parse_args()

    if args.add:
        add_task(args.add)
    elif args.list:
        list_tasks()
    elif args.remove:
        remove_task(int(args.remove))
    else:
        parser.print_help()

if __name__ == "__main__":
    main()
```

# 第 6 步：运行应用程序

你现在可以从命令行运行 TO-DO 列表应用程序。使用短选项`h`或长选项`help`来获取使用信息：

```py
$ python3 todo.py --help
usage: todo.py [-h] [-a] [-l] [-r]

Command-line Todo List App

options:
  -h, --help  	show this help message and exit
  -a , --add  	Add a new task
  -l, --list  	List all tasks
  -r , --remove   Remove a task by index
```

起初，列表中没有任务，因此使用`--list`列出所有任务时会打印“未找到任务。”：

```py
$ python3 todo.py --list
No tasks found.
```

现在我们可以这样向 TO-DO 列表中添加一项：

```py
$ python3 todo.py -a "Walk 2 miles"
```

当你现在列出项目时，你应该能看到添加的任务：

```py
$ python3 todo.py --list
1\. Walk 2 miles
```

由于我们添加了第一个项目，tasks.txt 文件已被创建（请参阅第 4 步中`list_tasks`函数的定义）：

```py
$ ls
tasks.txt  todo.py
```

让我们在列表中再添加一个任务：

```py
$ python3 todo.py -a "Grab evening coffee!"
```

还有一个：

```py
$ python3 todo.py -a "Buy groceries"
```

现在让我们获取所有任务的列表：

```py
$ python3 todo.py -l
1\. Walk 2 miles
2\. Grab evening coffee!
3\. Buy groceries
```

现在让我们按索引删除一个任务。假设我们完成了晚间咖啡（希望今天就此结束），所以按如下方式将其删除：

```py
$ python3 todo.py -r 2
Task removed successfully.
```

修改后的待办事项列表如下：

```py
$ python3 todo.py --list
1\. Walk 2 miles
2\. Buy groceries
```

# 第 7 步：测试、改进和重复

好的，我们的应用程序的最简单版本已经准备好了。那么如何进一步发展呢？这里有几个你可以尝试的事项：

+   当你使用无效的命令行选项（比如 `-w` 或 `--wrong`）时会发生什么？默认行为（如果你记得 if-elif-else 结构）是打印帮助信息，但也会有异常。尝试使用 try-except 块实现错误处理。

+   通过定义包括边界情况的测试用例来测试你的应用程序。你可以首先使用内置的 unittest 模块。

+   通过添加一个选项来指定每个任务的优先级来改进现有版本。还可以尝试按优先级对任务进行排序和检索。

▶️ [本教程的代码在 GitHub 上](https://github.com/balapriyac/python-projects/tree/main/command-line-app)。

# 总结

在本教程中，我们构建了一个简单的命令行待办事项列表应用程序。在此过程中，我们学会了如何使用内置的 argparse 模块解析命令行参数。我们还使用命令行输入在后台对简单的文本文件执行相应操作。

那么，我们接下来该做什么呢？好吧，像 [Typer](https://typer.tiangolo.com/) 这样的 Python 库使构建命令行应用程序变得轻而易举。我们将在即将到来的 Python 教程中使用 Typer 构建一个。在此之前，继续编程吧！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发者和技术写作者。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过编写教程、操作指南、评论文章等与开发者社区分享她的知识。Bala 还创建引人入胜的资源概述和编码教程。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

### 更多相关话题

+   [用 Python 构建 AI 应用程序的 10 个简单步骤](https://www.kdnuggets.com/build-an-ai-application-with-python-in-10-easy-steps)

+   [在 5 分钟内构建机器学习网络应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [KDnuggets 新闻 2022年3月9日：在 5 分钟内构建机器学习网络应用](https://www.kdnuggets.com/2022/n10.html)

+   [用 Docker 容器化 Python 应用的 5 个简单步骤](https://www.kdnuggets.com/containerize-python-apps-with-docker-in-5-easy-steps)

+   [数据科学如何改变移动应用开发？](https://www.kdnuggets.com/2023/03/data-science-transform-mobile-app-development.html)

+   [使用 Heroku 部署机器学习网络应用](https://www.kdnuggets.com/2022/04/deploy-machine-learning-web-app-heroku.html)
