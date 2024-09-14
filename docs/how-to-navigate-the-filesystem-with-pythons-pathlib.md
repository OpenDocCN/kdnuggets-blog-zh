# 如何使用 Python 的 Pathlib 浏览文件系统

> 原文：[`www.kdnuggets.com/how-to-navigate-the-filesystem-with-pythons-pathlib`](https://www.kdnuggets.com/how-to-navigate-the-filesystem-with-pythons-pathlib)

![pathlib](img/0cfef492124b3397208b71330312bdc2.png)

作者提供的图片

在 Python 中，使用常规字符串来表示文件系统路径可能很麻烦，尤其是当你需要对路径字符串执行操作时。切换到不同的操作系统也会导致代码出错。是的，你可以使用 [os.path](https://docs.python.org/3/library/os.path.html) 来简化操作，来自 [os 模块](https://docs.python.org/3/library/os.html)。但 [pathlib](https://docs.python.org/3/library/pathlib.html) 模块使所有这些操作变得更加直观。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 工作

* * *

Python 3.4 中引入的 pathlib 模块（是的，它已经存在一段时间了）允许采用面向对象的方式来创建和操作路径对象，并且内置了处理常见操作的功能，如连接和操作路径、解析路径等。

本教程将介绍如何使用 pathlib 模块操作文件系统。让我们开始吧。

## 使用路径对象

要开始使用 pathlib，你首先需要导入 `Path` 类：

```py
from pathlib import Path 
```

这使得你可以实例化路径对象来创建和操作文件系统路径。

### 创建路径对象

你可以通过传递表示路径的字符串来创建 `Path` 对象，方法如下：

```py
path = Path('your/path/here')
```

你也可以从现有路径创建新的路径对象。例如，你可以从你的主目录或当前工作目录创建路径对象：

```py
home_dir = Path.home()
print(home_dir)

cwd = Path.cwd()
print(cwd)
```

这应该会给你类似的输出：

```py
Output >>>
/home/balapriya
/home/balapriya/project1
```

假设你有一个基础目录，并且你想创建一个指向子目录中文件的路径。你可以这样做：

```py
from pathlib import Path

# import Path from pathlib
from pathlib import Path

# create a base path
base_path = Path("/home/balapriya/Documents")

# create new paths from the base path
subdirectory_path = base_path / "projects" / "project1"
file_path = subdirectory_path / "report.txt"

# Print out the paths
print("Base path:", base_path)
print("Subdirectory path:", subdirectory_path)
print("File path:", file_path)
```

首先为基础目录创建一个路径对象：`/home/balapriya/Documents`。**请记得将这个基础路径替换为你工作环境中的有效文件系统路径**。

然后通过将 `base_path` 与子目录 `projects` 和 `project1` 连接来创建 `subdirectory_path`。最后，通过将 `subdirectory_path` 与文件名 `report.txt` 连接来创建 `file_path`。

如所见，你可以使用`/`操作符将目录或文件名附加到当前路径，创建一个新的路径对象。注意`/`操作符的重载提供了一种可读且直观的路径连接方式。

当你运行上述代码时，它将输出以下路径：

```py
Output >>>
Base path: /home/balapriya/documents
Subdirectory path: /home/balapriya/documents/projects/project1
File path: /home/balapriya/documents/projects/project1/report.txt
```

### 检查状态和路径类型

一旦你有了有效的路径对象，你可以调用简单的方法来检查路径的状态和类型。

要检查路径是否存在，调用`exists()`方法：

```py
path = Path("/home/balapriya/Documents")
print(path.exists())
```

```py
Output >>> True
```

如果路径存在，它会输出`True`；否则，返回`False`。

你还可以检查路径是文件还是目录：

```py
 print(path.is_file())
print(path.is_dir())
```

```py
Output >>>
False
True
```

> **注意**：`Path`类的对象为你的操作系统创建了一个具体的路径。但当你需要处理路径而不访问文件系统时，例如在 Unix 机器上处理 Windows 路径，你也可以使用`PurePath`。

## 导航文件系统

使用 pathlib 导航文件系统非常简单。你可以遍历目录的内容，重命名和解析路径等。

你可以像这样在路径对象上调用`iterdir()`方法，以遍历目录中的所有内容：

```py
path = Path("/home/balapriya/project1")

# iterating over directory contents

for item in path.iterdir():
    print(item) 
```

这是示例输出：

```py
Output >>>
/home/balapriya/project1/test.py
/home/balapriya/project1/main.py
```

### 重命名文件

你可以通过在路径对象上调用`rename()`方法来重命名文件：

```py
 path = Path('old_path')
path.rename('new_path')
```

在这里，我们将`project1`目录中的`test.py`重命名为`tests.py`：

```py
path = Path('/home/balapriya/project1/test.py')
path.rename('/home/balapriya/project1/tests.py')
```

现在你可以`cd`进入`project1`目录，检查文件是否已经重命名。

### 删除文件和目录

你也可以使用`unlink()`和`rmdir()`方法分别删除文件和移除空目录。

```py
# For files
path.unlink()   

# For empty directories
path.rmdir() 
```

> **注意**：如果你对删除空目录感到好奇，是否也可以创建它们。是的，你也可以使用`mkdir()`创建目录，如下所示：`path.mkdir(parents=True, exist_ok=True)`。`mkdir()`方法创建一个新目录。设置`parents=True`允许根据需要创建父目录，`exist_ok=True`则在目录已存在时防止出现错误。

### 解析绝对路径

有时，使用相对路径更方便，并在需要时扩展到绝对路径。你可以使用`resolve()`方法来完成，语法非常简单：

```py
absolute_path = relative_path.resolve()
```

这里是一个示例：

```py
relative_path = Path('new_project/README.md')
absolute_path = relative_path.resolve()
print(absolute_path)
```

和输出：

```py
Output >>> /home/balapriya/new_project/README.md
```

## 文件模式匹配

模式匹配对于查找符合特定模式的文件非常有用。让我们看一个示例目录：

```py
projectA/
├── projectA1/
│   └── data.csv
└── projectA2/
	├── script1.py
	├── script2.py
	├── file1.txt
	└── file2.txt
```

这是路径：

```py
path = Path('/home/balapriya/projectA')
```

让我们尝试使用`glob()`查找所有文本文件：

```py
text_files = list(path.glob('*.txt'))
print(text_files)
```

令人惊讶的是，我们没有得到文本文件。列表是空的：

```py
Output >>> []
```

这是因为这些文本文件在子目录中，模式匹配不会搜索子目录。使用递归模式匹配`rglob()`。

```py
text_files = list(path.rglob('*.txt'))
print(text_files)
```

`rglob()`方法执行递归搜索，查找目录及其所有子目录中的所有文本文件。因此我们应该得到预期的输出：

```py
Output >>>
[PosixPath('/home/balapriya/projectA/projectA2/file2.txt'), 
PosixPath('/home/balapriya/projectA/projectA2/file1.txt')]
```

事情就这样结束了！

## 总结

在本教程中，我们探讨了 pathlib 模块及其如何使 Python 中的文件系统导航和操作变得更加便捷。我们已经覆盖了足够的内容，以帮助你在 Python 脚本中创建和使用文件系统路径。

你可以在这个教程中找到所使用的代码 [在 GitHub](https://github.com/balapriyac/python-basics/tree/main/pathlib-tutorial)。在下一个教程中，我们将探讨有趣的实际应用。在那之前，继续编程吧！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)** 是一位来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过撰写教程、操作指南、观点文章等方式，与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。**

### 更多相关主题

+   [如何使用 Bash 导航文件系统](https://www.kdnuggets.com/how-navigate-filesystem-bash)

+   [使用 Python 的 Pathlib 组织、搜索和备份文件](https://www.kdnuggets.com/organize-search-and-back-up-files-with-pythons-pathlib)

+   [帮助我应对困难就业市场的 4 个职业经验](https://www.kdnuggets.com/2023/05/4-lessons-made-difference-navigating-current-job-market.html)

+   [AI Con USA: 探索未来的 AI](https://www.kdnuggets.com/2024/02/techwell-ai-con-usa-navigate-the-future-of-ai)

+   [AI Con USA: 探索未来的 AI 2024](https://www.kdnuggets.com/2024/04/ai-con-usa-navigate-the-future-of-ai)

+   [优化 Python 代码性能：深入探讨 Python 性能分析工具](https://www.kdnuggets.com/2023/02/optimizing-python-code-performance-deep-dive-python-profilers.html)
