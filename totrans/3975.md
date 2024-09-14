# 使用 Python 的 Pathlib 组织、搜索和备份文件

> 原文：[https://www.kdnuggets.com/organize-search-and-back-up-files-with-pythons-pathlib](https://www.kdnuggets.com/organize-search-and-back-up-files-with-pythons-pathlib)

![pathlib-examples](../Images/cf0155e2b6b24b2c3482db40b1b6e159.png)

图片由作者提供

Python 内置的 [pathlib 模块](https://docs.python.org/3/library/pathlib.html) 使得处理文件系统路径变得非常简单。在 [如何使用 Python 的 Pathlib 导航文件系统](https://www.kdnuggets.com/how-to-navigate-the-filesystem-with-pythons-pathlib) 中，我们了解了处理路径对象和导航文件系统的基础知识。现在是时候深入了解了。

* * *

## 我们的前 3 个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你在 IT 领域的组织

* * *

在本教程中，我们将使用 pathlib 模块的功能来处理三项具体的文件管理任务：

+   按扩展名组织文件

+   搜索特定文件

+   备份重要文件

在本教程结束时，你将学会如何使用 pathlib 进行文件管理任务。让我们开始吧！

## 1\. 按扩展名组织文件

当你在为一个项目进行研究和工作时，你通常会创建临时文件并下载相关文档到工作目录中，直到它变得杂乱，你需要整理它。

让我们来看一个简单的例子，其中项目目录包含 requirements.txt、配置文件和 Python 脚本。我们希望将文件按扩展名分类到子目录中——每个扩展名对应一个子目录。为了方便起见，我们选择将扩展名作为子目录的名称。

![organize-files](../Images/8375a8a21063cc3860e26869efe8fb33.png)

按扩展名组织文件 | 图片由作者提供

这是一个 Python 脚本，它扫描一个目录，按文件扩展名识别文件，并将它们移动到相应的子目录中：

```py
# organize.py

from pathlib import Path

def organize_files_by_extension(path_to_dir):
    path = Path(path_to_dir).expanduser().resolve()
    print(f"Resolved path: {path}")

    if path.exists() and path.is_dir():
        print(f"The directory {path} exists. Proceeding with file organization...")

    for item in path.iterdir():
        print(f"Found item: {item}")
        if item.is_file():
            extension = item.suffix.lower()
            target_dir = path / extension[1:]  # Remove the leading dot

            # Ensure the target directory exists
            target_dir.mkdir(exist_ok=True)
            new_path = target_dir / item.name

            # Move the file
            item.rename(new_path)

            # Check if the file has been moved
            if new_path.exists():
                print(f"Successfully moved {item} to {new_path}")
            else:
                print(f"Failed to move {item} to {new_path}")

	  else:
       print(f"Error: {path} does not exist or is not a directory.")

organize_files_by_extension('new_project')
```

`organize_files_by_extension()` 函数接受一个目录路径作为输入，将其解析为绝对路径，并按文件扩展名在该目录内组织文件。它首先确保指定的路径存在且为目录。

然后，它遍历目录中的所有项目。对于每个文件，它获取文件扩展名，创建一个以扩展名命名的新目录（如果该目录尚不存在），并将文件移动到这个新目录中。

在移动每个文件后，它会通过检查文件在新位置的存在来确认操作是否成功。如果指定路径不存在或不是目录，它会打印错误信息。

下面是示例函数调用的输出（在 new_project 目录中整理文件）：

![organize](../Images/fb56ab7258f4809fd98ad6ee186a31d6.png)

现在在你的工作环境中的项目目录上尝试这个。我使用了 if-else 来处理错误。但你也可以使用 try-except 块来改进这个版本。

## 2\. 查找特定文件

有时你可能不希望像前面的示例那样按扩展名将文件组织到不同的子目录中。但你可能只想找到所有具有特定扩展名的文件（如所有图像文件），为此你可以使用 globbing。

假设我们想找到 requirements.txt 文件来查看项目的依赖项。让我们使用相同的示例，但在按扩展名将文件分组到子目录之后。

如果你使用如上所示的 `glob()` 方法在路径对象上查找所有文本文件（由模式 '*.txt' 定义），你会发现它没有找到文本文件：

```py
# search.py
from pathlib import Path

def search_and_process_text_files(directory):
    path = Path(directory)
    path = path.resolve()
    for text_file in path.glob('*.txt'):
    # process text files as needed
        print(f'Processing {text_file}...')
        print(text_file.read_text())

search_and_process_text_files('new_project')
```

这是因为 `glob()` 只搜索当前目录，而该目录不包含 requirements.txt 文件。requirements.txt 文件位于 txt 子目录中。因此，你必须使用 **递归 globbing** 和 `rglob()` 方法。

所以这里是找到文本文件并打印其内容的代码：

```py
from pathlib import Path

def search_and_process_text_files(directory):
    path = Path(directory)
    path = path.resolve()
    for text_file in path.rglob('*.txt'):
    # process text files as needed
        print(f'Processing {text_file}...')
        print(text_file.read_text())

search_and_process_text_files('new_project')
```

`search_and_process_text_files` 函数接收一个目录路径作为输入，将其解析为绝对路径，并使用 `rglob()` 方法在该目录 *及* 其子目录中查找所有 `.txt` 文件。

对于找到的每个文本文件，它打印文件的路径，然后读取并打印出文件的内容。此函数对于递归地定位和处理指定目录中的所有文本文件非常有用。

由于 requirements.txt 是我们示例中唯一的文本文件，我们得到了以下输出：

```py
Output >>>
Processing /home/balapriya/new_project/txt/requirements.txt...
psycopg2==2.9.0
scikit-learn==1.5.0
```

现在你知道如何使用 globbing 和递归 globbing，尝试重新做第一个任务——按扩展名整理文件——使用 globbing 查找并分组文件，然后将它们移动到目标子目录。

## 3\. 备份重要文件

迄今为止我们看到的示例是按扩展名整理文件和查找特定文件。但如何备份某些重要文件呢？为什么不呢？

在这里，我们希望将文件从项目目录复制到备份目录，而不是将文件移动到另一个位置。除了 pathlib，我们还将使用 [shutil](https://docs.python.org/3/library/shutil.html) 模块的 copy 函数。

让我们创建一个函数，将所有具有特定扩展名（所有 .py 文件）的文件复制到备份目录：

```py
#back_up.py
import shutil
from pathlib import Path

def back_up_files(directory, backup_directory):
    path = Path(directory)
    backup_path = Path(backup_directory)
    backup_path.mkdir(parents=True, exist_ok=True)

    for important_file in path.rglob('*.py'):
        shutil.copy(important_file, backup_path / important_file.name)
        print(f'Backed up {important_file} to {backup_path}')

back_up_files('new_project', 'backup')
```

`back_up_files()` 函数接收一个现有的目录路径和一个备份目录路径函数，并将指定目录及其子目录中的所有 Python 文件备份到指定的备份目录。

它为源目录和备份目录创建路径对象，并通过创建备份目录及其任何必要的父目录（如果尚不存在）来确保备份目录的存在。

然后，函数使用 `rglob()` 方法迭代源目录中的所有 `.py` 文件。对于每个找到的 Python 文件，它将文件复制到备份目录，同时保留原始文件名。本质上，这个函数帮助在项目目录内创建所有 Python 文件的备份。

运行脚本并验证输出后，你可以随时检查备份目录的内容：

![备份](../Images/19072361bcdcaf944a25088a4f8132a4.png)

对于你的示例目录，你可以使用 `back_up_files('/path/to/directory', '/path/to/backup/directory')` 来备份感兴趣的文件。

## 总结

在本教程中，我们探讨了使用 Python 的 pathlib 模块来按扩展名组织文件、搜索特定文件和备份重要文件的实际示例。你可以在 [GitHub](https://github.com/balapriyac/python-basics/tree/main/pathlib-examples) 上找到本教程中使用的所有代码。

正如你所见，pathlib 模块使得处理文件路径和文件管理任务变得更加轻松和高效。现在，继续在你的项目中应用这些概念，以更好地处理文件管理任务。祝编程愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她正在通过编写教程、操作指南、意见文章等方式学习并与开发者社区分享她的知识。Bala 还创建了有趣的资源概述和编码教程。

### 相关主题

+   [使用网格搜索和随机搜索进行超参数调整（Python）](https://www.kdnuggets.com/2022/10/hyperparameter-tuning-grid-search-random-search-python.html)

+   [通过 Uplimit 的机器学习搜索课程提升你的搜索引擎技能！](https://www.kdnuggets.com/2023/10/uplimit-elevate-your-search-engine-skills-search-with-ml-course)

+   [构建视觉搜索引擎 - 第2部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [如何使用 Python 的 Pathlib 导航文件系统](https://www.kdnuggets.com/how-to-navigate-the-filesystem-with-pythons-pathlib)

+   [IMPACT：数据可观察性峰会将于11月8日回归…](https://www.kdnuggets.com/2023/10/monte-carlo-impact-the-data-observability-summit-is-back)

+   [回到基础第1周：Python 编程与数据科学基础](https://www.kdnuggets.com/back-to-basics-week-1-python-programming-data-science-foundations)
