# 用可视化调试器容器化 Jupyter

> 原文：[https://www.kdnuggets.com/2020/04/dockerize-jupyter-visual-debugger.html](https://www.kdnuggets.com/2020/04/dockerize-jupyter-visual-debugger.html)

[评论](#comments)

**作者：[Manish Tiwari](https://www.linkedin.com/in/manish-kumar-tiwari/)，数据爱好者**

![图示](../Images/b9e0fd2862820e4423939d8b443837c6.png)

图片来源：[Nilantha Ilangamuwa](https://unsplash.com/@ilangamuwa?utm_source=medium&utm_medium=referral) 来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Jupyter 最近宣布了期待已久的可视化调试器的首次公开发布。尽管这是首次发布，但它支持调试和检查变量等所需的所有基本调试需求。

数据科学社区因 Jupyter Notebook 能够以互动的方式轻松传达和共享成果而大量依赖它。

然而，唯一的问题是缺少可视化调试能力，因此人们通常需要切换到其他提供更好调试和代码重构功能的经典 IDE。这一功能受到数据科学社区的高度期待，现在终于发布了。

如需简要了解可视化调试器的实际效果，请参考下面的视频演示：

![图示](../Images/94d10f5444d449de1cebb00591f968a3.png)

视频演示：[Jeremy](https://github.com/jtpio) 提供，见 [Github](https://github.com/jupyterlab/debugger/blob/master/screencast.gif)

在本文中，我们将详细讲解在现有 JupyterLab 环境中设置可视化调试器的步骤，并将 JupyterLab 环境与默认启用的可视化调试器进行容器化。

### 前提条件：

JupyterLab 2.0+

对任何编程语言的调试基础知识

Docker 的基础知识。

### 安装：

假设您已经在使用 JupyterLab，只需安装 JupyterLab 调试器扩展以进行前端调试，以及在后端支持 Jupyter 调试协议的任何内核。

### 安装 JupyterLab 扩展以启用前端调试：

JupyterLab 使用 nodejs 安装扩展，因此我们需要先安装 nodejs 才能安装前端调试器扩展。

在未来的版本中，Jupyter 可能会默认包含此扩展。

```py
conda install -c conda-forge nodejs
jupyter labextension install @jupyterlab/debugger
```

### 安装内核 xeus-python：

目前，后端只有 xeus-python 支持 Jupyter 调试协议。未来可能会有更多内核支持该协议。

```py
conda install xeus-python -c conda-forge
```

现在如果运行 Jupyter Lab，您应该能够看到 xeus-python 内核在控制台和笔记本部分各有 2 个附加图标。

### 为什么要容器化？

容器使得跨多个环境的开发更加顺畅。这就是它们成为云原生应用交付方法技术基础的原因。

Docker 创始人 Solomon Hykes 说，当支持软件环境不一致时会出现问题。“你会在 Python 2.7 上测试，然后它会在生产环境中运行 Python 3，结果会发生一些奇怪的事情。或者你依赖某个 SSL 库的特定版本，而安装了另一个版本。你会在 Debian 上运行测试，而生产环境在 Red Hat 上，一切都可能变得很奇怪。”

容器通过将运行应用程序所需的环境、依赖项、二进制文件、所有必要的配置和应用程序本身打包到一个包中来解决此问题。这样，我们不再需要担心操作系统和其他环境特定的依赖项，因为一切都被打包在一个可以在任何地方运行的独立实体中。

### 启用 Visual Debugger 的 Jupyter Docker 化

我假设你对基本的 Docker 命令和术语已经很熟悉。解释 Docker 的工作原理超出了本文的范围。然而，如果你觉得需要复习，请参考 Docker [文档](https://docs.docker.com/)。

现在我们将创建创建所需环境 Docker 镜像所需的 Dockerfile。你可以将镜像视为包含所有必要指令的文件，用于在容器中运行我们的应用程序。

我们将使用 Miniconda，一个用于 Anaconda 的轻量级最小化安装程序。它是 Anaconda 的一个小型启动版本，只包含 conda、Python、它们所依赖的包和少量其他有用的包。

```py
FROM continuumio/miniconda3
```

定义 Docker 文件和工作目录的元数据：

```py
LABEL maintainer=”Manish Tiwari <m***[@gmail.com](mailto:mtiwari5@gmail.com)>”
LABEL version=”0.1"
LABEL description=”Debugging Jupyter Notebook”WORKDIR /jup
```

安装 JupyterLab

```py
RUN conda install -c conda-forge jupyterlab
```

安装用于前端调试的 nodejs 和 labextension。

```py
RUN conda install -c conda-forge nodejs
RUN jupyter labextension install [@jupyterlab/debugger](https://twitter.com/jupyterlab/debugger)
```

安装支持 Jupyter 调试协议的内核。

```py
RUN conda install xeus-python -c conda-forge
```

**注意：**这里我们使用了 conda 包管理器，你也可以使用 pip，但不推荐同时使用这两者，因为这可能会破坏环境。

最后，暴露端口并定义入口点。

```py
EXPOSE 8888
ENTRYPOINT [“jupyter”, “lab”,” — ip=0.0.0.0",” — allow-root”]
```

我们最终的 Dockerfile 应如下所示：

启用 Visual Debugger 的 JupyterLab Docker 化

### 从上述 Dockerfile 构建 Docker 镜像。

导航到包含上述 Dockerfile 的文件夹，然后运行以下命令。

```py
docker build -t visualdebugger .
```

另外，你也可以从任何地方运行命令，只需提供 Dockerfile 的绝对路径。

一旦镜像成功构建，使用以下命令列出 Docker 镜像以进行验证。

```py
docker image ls
```

输出应如下所示：

![](../Images/4d9348c00df9a889ea45e8f5a9d13fed.png)

现在在新容器中运行 Docker 镜像，如下所示：

```py
docker container run -p 8888:8888 visualdebugger-jupyter
```

在这里，我们将主机端口（冒号前的第一个端口）8888 映射到容器中暴露的端口 8888。这是主机与容器内的 Jupiter 监听端口进行通信所必需的。

运行上述命令后，你应该看到如下输出（前提是端口未被其他进程占用）：

![](../Images/52ee4e3c29d5a884d81fa56bd5e5c92f.png)

这意味着我们的 docker 容器正在运行。你现在可以打开上面指定的 URL，并使用 Jupyter 和可视化调试器，而不需要意识到它并未在主机上运行。

你还可以通过以下命令查看可用的容器列表：

```py
docker container ls
```

上面的命令应该会列出容器及其元数据，如下所示：

![](../Images/23094ef9780246a9158aed1a0794f675.png)

一旦你打开上面输出的 URL，你应该能看到 JupyterLab 在主机的 localhost 和 8888 端口上运行。

![图](../Images/109e93190201264d560fd95d2bad0e59.png)

在容器中运行的带有可视化调试器的 JupyterLab

现在，要使用可视化调试器，请在 Launcher 中打开**xpython**，而不是 Python。

我已经在 [docker hub](https://hub.docker.com/repository/docker/beingmanish/visualdebugger-jupyter) 上发布了我们刚刚构建的 docker 镜像，以便你获得一个带有可视化调试功能的 Jupyter 环境。

你可以通过以下命令拉取 docker 镜像并进行操作。

```py
docker pull beingmanish/visualdebugger-jupyter
```

如果你希望深入了解 Jupyter 的可视化调试架构，可以参考[这里](https://blog.jupyter.org/a-visual-debugger-for-jupyter-914e61716559)。

有建议或问题吗？请在评论中写下。

**参考文献:** [Jupyter 博客](https://blog.jupyter.org/)

[Jupyter@Github](https://github.com/jupyter)

**个人简介: [Manish Tiwari](https://www.linkedin.com/in/manish-kumar-tiwari/)** 是一位数据爱好者，热衷于分享 AI 领域的学习和经验。

[原文](https://towardsdatascience.com/dockerize-jupyter-with-official-visual-debugger-enabled-cbce1840b7f)。转载经许可。

**相关:**

+   [4 个最佳 Jupyter Notebook 深度学习环境](/2020/03/4-best-jupyter-notebook-environments-deep-learning.html)

+   [5 个 Google Colaboratory 小贴士](/2020/03/5-google-colaboratory-tips.html)

+   [GitHub Python 数据科学焦点：高级机器学习与 NLP，集成方法，命令行可视化与 Docker 简化](/2018/10/github-python-data-science-spotlight.html)

### 更多相关内容

+   [数据科学的基础数学：奇异值分解的可视化介绍](https://www.kdnuggets.com/2022/06/essential-math-data-science-visual-introduction-singular-value-decomposition.html)

+   [构建视觉搜索引擎 - 第 1 部分：数据探索](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-1.html)

+   [Visual ChatGPT: 微软结合 ChatGPT 和 VFMs](https://www.kdnuggets.com/2023/03/visual-chatgpt-microsoft-combine-chatgpt-vfms.html)

+   [人工智能：大规模语言模型与视觉模型](https://www.kdnuggets.com/2023/06/ai-large-language-visual-models.html)

+   [构建视觉搜索引擎 - 第 2 部分：搜索引擎](https://www.kdnuggets.com/2022/02/building-visual-search-engine-part-2.html)

+   [通过集成 Jupyter 和 KNIME 缩短实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)
