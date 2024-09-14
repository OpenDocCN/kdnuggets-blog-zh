# 需要管理的 Python 版本太多？Pyenv 来拯救

> 原文：[https://www.kdnuggets.com/too-many-python-versions-to-manage-pyenv-to-the-rescue](https://www.kdnuggets.com/too-many-python-versions-to-manage-pyenv-to-the-rescue)

![需要管理的 Python 版本太多？Pyenv 来拯救](../Images/2139531f43a05fc40e1648d1dbb6ffed.png)

作者提供的图片

想在早上尝试最新 Python 版本的新特性...午休时浏览旧版 Python 代码库——而不会破坏你的开发环境？

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

是的，这是可能的。[Pyenv](https://github.com/pyenv/pyenv) 可以帮助你。使用 Pyenv，你可以安装 Python 版本、切换版本和删除不再需要的版本。

本教程是 Pyenv 设置和使用的简要介绍。现在，让我们开始吧！

# 安装 Pyenv

第一步是安装 Pyenv。我使用的是 Linux：Ubuntu 23.01。如果你使用的是 Linux 机器，安装 Pyenv 的最简单方法是运行以下 `curl` 命令：

```py
$ curl https://pyenv.run | bash
```

这使用了 [pyenv-installer](https://github.com/pyenv/pyenv-installer) 来安装 Pyenv。

安装完成后，你将被提示完成设置你的 shell 环境以使用 Pyenv。为此，你可以将以下命令添加到 `~/.bashrc` 文件中：

```py
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc

echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

一切准备就绪，开始使用 Pyenv 吧！

> **注意**：如果你使用的是 Mac 或 Windows 机器，请查看 [如何安装 Pyenv](https://github.com/pyenv/pyenv#installation) 的详细说明。在 Windows 上，你需要在 Windows 子系统 for Linux (WSL) 中安装 Pyenv。

# 使用 Pyenv 安装 Python 版本

现在你已经安装了 Pyenv，你可以通过运行 `pyenv install` 命令来安装特定的 Python 版本，如下所示：

```py
$ pyenv install version
```

要检查已安装的 Python 版本列表，请运行以下命令：

```py
$ pyenv versions
* system (set by /home/balapriya/.pyenv/version)
```

我还没有安装任何新版本，所以唯一的 Python 版本是系统版本。在我的情况中是 Python 3.11：

```py
$ python3 –version

Python 3.11.4
```

让我们尝试安装 Python 3.8 和 3.12。尝试运行以下命令来安装 Python 3.8：

```py
$ pyenv install 3.8
```

第一次尝试用 Pyenv 安装特定版本的 Python 时，你可能会遇到错误，因为缺少一些构建依赖项。别担心，很容易修复！

## ⚙️ 一些故障排除提示

当我尝试在我的 Linux 发行版上使用 `pyenv install` 命令安装 Pyenv 时，由于缺少构建依赖项而遇到了错误。

[这个 StackOverflow 讨论串](https://stackoverflow.com/questions/60775172/pyenvs-python-is-missing-bzip2-module) 包含有关安装 Pyenv 所需的构建依赖项的有用信息。运行以下命令以安装缺少的依赖项：

```py
$ apt-get install build-essential zlib1g-dev libffi-dev libssl-dev libbz2-dev libreadline-dev libsqlite3-dev liblzma-dev
```

现在你应该可以在没有任何错误的情况下安装 Python 版本：

```py
$ pyenv install 3.8
```

> **注意**：当你安装 Python 3.x 时，默认会安装最新的版本。但你也可以进行更细粒度的控制，指定 3.x.y 来安装特定版本的 Python。你还可以运行 `pyenv install --list` 来获取所有可用 Python 版本的列表。然而，这个列表是非常 *长* 的。

类似地，运行 `pyenv install` 来安装 Python 3.12：

```py
$ pyenv install 3.12
```

现在如果你运行 `pyenv versions`，除了系统版本，你会看到 Python 3.8 和 3.12：

```py
$ pyenv versions
* system (set by /home/balapriya/.pyenv/version)
3.8.18
3.12.0
```

# 设置全局 Python 版本

使用 Pyenv，你可以设置一个 **全局** Python 版本。正如其名，全局版本是你在命令行中使用 Python 时使用的 Python 版本。

但要小心将其设置为相对较新的版本，以避免在运行使用更新 Python 版本的项目时出现错误。

比如，假设我们将全局版本设置为 Python 3.8.18 会发生什么。

```py
$ pyenv global 3.8.18
```

创建一个项目文件夹。在其中，创建一个 main.py 文件，并添加以下代码：

```py
# main.py

def handle_status_code(status_code):
    match status_code:
        case 200:
             print(f"Success! Status code: {status_code}")
        case 404:
            print(f"Not Found! Status code: {status_code}")
        case 500:
            print(f"Server Error! Status code: {status_code}")
        case _:
            print(f"Unhandled status code: {status_code}")

status_code = 404  # oversimplification, yes. 
handle_status_code(status_code)
```

如所示，这段代码使用了在 Python 3.10 中引入的 match-case 语句。因此，你需要 Python 3.10 或更高版本才能成功运行这段代码。如果你尝试运行脚本，你会遇到以下错误：

```py
File "main.py", line 2
	match status_code:
      	^
SyntaxError: invalid syntax
```

就我而言，系统 Python 版本是 3.11，非常新。因此，我可以将全局版本设置为系统 Python 版本，如下所示：

```py
$ pyenv global system
```

当你现在运行相同的脚本时，你应该会看到以下输出：

```py
Output >>>
Not Found! Status code: 404
```

如果你的系统 Python 是较旧的版本，例如 Python 3.6 或更早版本，安装一个更新的 Python 版本并将其设置为全局版本是很有帮助的。

# 为你的项目设置本地 Python 版本

当你想要处理使用较早版本 Python 的项目时，你需要安装该版本以避免任何错误（如不再支持的方法调用）。

比如，你想在处理项目 A 时使用 Python 3.8，在处理项目 B 时使用 Python 3.10 或更高版本。

![Python 版本太多，难以管理？Pyenv 来救援](../Images/784d50c72897ae279e6b24fb94dce3bd.png)

图片由作者提供

在这种情况下，你可以在项目 A 的目录中这样设置本地 Python 版本：

```py
$ pyenv local 3.8.18
```

你可以运行 `python --version` 来检查项目目录中的 Python 版本：

```py
$ python --version
Python 3.8.18
```

这在处理较旧的 Python 代码库时特别有用。

# 卸载 Python 版本

如果你不再需要某个 Python 版本，你可以通过运行 `pyenv uninstall` 命令将其卸载。例如，如果我们不再需要 Python 3.8.18，我们可以通过运行以下命令将其卸载：

```py
$ pyenv uninstall 3.8.18
```

你应该在终端中看到类似的输出：

```py
pyenv: remove /home/balapriya/.pyenv/versions/3.8.18? [y|N] y
pyenv: 3.8.18 uninstalled
```

# 总结

我希望你觉得这个关于 Pyenv 的入门教程有帮助。我们来回顾一些最常用的命令以便快速参考：

| **命令** | **功能** |
| --- | --- |
| `pyenv versions` | 列出所有当前安装的 Python 版本 |
| `pyenv install --list` | 列出所有可供安装的 Python 版本 |
| `pyenv install 3.x` | 安装 Python 3.x 的最新发布版 |
| `pyenv install 3.x.y` | 安装 Python 3.x 的 y 版本 |
| `pyenv global 3.x` | 将 Python 3.x 设置为全局 Python 版本 |
| `pyenv local 3.x` | 将本地 Python 版本设置为 3.x |
| `pyenv uninstall 3.x.y` | 卸载 Python 3.x 的 y 版本 |

如果你在想。是的，你可以 [使用 Docker](/2023/07/docker-tutorial-data-scientists.html)，这是一个使本地开发变得轻松的绝佳选择——无需担心依赖冲突。但你可能会觉得每次需要进行新项目时使用 Docker 或其他容器化解决方案有些过度。

所以我认为能够在命令行安装、管理和切换 Python 版本仍然很有用。你也可以探索 [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) 插件来创建和管理虚拟环境。编码愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发人员和技术作家。她喜欢在数学、编程、数据科学和内容创作的交集处工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编程和喝咖啡！目前，她正在通过编写教程、操作指南、观点文章等，学习并与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。

### 更多相关内容

+   [如何有效使用 Docker 标签管理镜像版本](https://www.kdnuggets.com/how-to-use-docker-tags-to-manage-image-versions-effectively)

+   [为什么那么多数据科学家辞职？](https://www.kdnuggets.com/2022/03/many-data-scientists-quitting-jobs.html)

+   [如何管理 Python 中的多重继承](https://www.kdnuggets.com/2022/03/manage-multiple-inheritance-python.html)

+   [准备好通过…获得网络安全硕士学位来应对威胁](https://www.kdnuggets.com/2022/07/baypath-prepared-manage-threat-ms-cybersecurity.html)

+   [优化和管理机器学习生命周期的前 10 个 MLOps 工具](https://www.kdnuggets.com/2022/10/top-10-mlops-tools-optimize-manage-machine-learning-lifecycle.html)

+   [准备好通过…获得网络安全硕士学位来应对威胁](https://www.kdnuggets.com/2022/12/baypath-prepared-manage-threat-ms-cybersecurity.html)
