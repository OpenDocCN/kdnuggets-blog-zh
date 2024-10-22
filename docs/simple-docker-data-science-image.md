# 创建一个简单的 Docker 数据科学镜像

> 原文：[`www.kdnuggets.com/2023/08/simple-docker-data-science-image.html`](https://www.kdnuggets.com/2023/08/simple-docker-data-science-image.html)

![创建一个简单的 Docker 数据科学设置](img/354326b0cc6fcfe512f843067a46c046.png)

作者使用 Midjourney 创建的图像

# 为什么选择 Docker 来进行数据科学？

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

作为数据科学家，拥有一个标准化且可移植的分析和建模环境至关重要。 [Docker](https://www.docker.com/) 提供了一种创建可重用和可共享数据科学环境的绝佳方式。在本文中，我们将详细介绍如何使用 Docker 设置基本的数据科学环境。

我们为什么要考虑使用 Docker 呢？Docker 允许数据科学家为他们的工作创建隔离且可重复的环境。使用 Docker 的一些关键优势包括：

+   **一致性** - 相同的环境可以在不同的机器上复制。再也没有“在我的机器上能用”的问题。

+   **可移植性** - Docker 环境可以轻松地在多个平台之间共享和部署。

+   **隔离性** - 容器隔离了不同项目所需的依赖项和库。再也没有冲突了！

+   **可扩展性** - 通过启动更多容器，轻松扩展在 Docker 内构建的应用程序。

+   **协作** - Docker 通过允许团队共享开发环境来促进协作。

# 第 1 步：创建 Dockerfile

任何 Docker 环境的起点是 Dockerfile。这个文本文件包含了构建 Docker 镜像的指令。

让我们创建一个 Python 数据科学环境的基本 Dockerfile，并将其保存为没有扩展名的 'Dockerfile'。

```py
# Use official Python image
FROM python:3.9-slim-buster

# Set environment variable
ENV PYTHONUNBUFFERED 1

# Install Python libraries 
RUN pip install numpy pandas matplotlib scikit-learn jupyter

# Run Jupyter by default
CMD ["jupyter", "lab", "--ip='0.0.0.0'", "--allow-root"]
```

这个 Dockerfile 使用了 [官方 Python 镜像](https://hub.docker.com/layers/library/python/3.9.9-slim-buster/images/sha256-24f5fd0379e7a0a729baa94e7637336b9f2dd2cf8251e46923ec181ee90ab54c)，并在其上安装了一些流行的数据科学库。最后一行定义了在启动容器时运行 Jupyter Lab 的默认命令。

# 第 2 步：构建 Docker 镜像

现在我们可以使用`docker build`命令构建镜像：

```py
docker build -t ds-python .
```

这将根据我们的 Dockerfile 创建一个标记为`ds-python`的镜像。

构建镜像可能需要几分钟，因为所有的依赖项都在安装中。完成后，我们可以使用`docker images`查看本地 Docker 镜像。

# 第 3 步：运行容器

镜像构建完成后，我们现在可以启动一个容器：

```py
docker run -p 8888:8888 ds-python
```

这将启动一个 Jupyter Lab 实例，并将主机的 8888 端口映射到容器的 8888 端口。

现在我们可以在浏览器中导航到 `localhost:8888` 来访问 Jupyter 并开始运行笔记本！

# 第 4 步：共享和部署镜像

Docker 的一个主要好处是能够在不同环境之间共享和部署镜像。

要将镜像保存为 tar 归档，运行：

```py
docker save -o ds-python.tar ds-python
```

这个 tar 包可以通过以下命令在任何其他安装了 Docker 的系统上加载：

```py
docker load -i ds-python.tar
```

我们还可以将镜像推送到 Docker 注册中心，如 Docker Hub，以便与他人公开或在组织内部私下共享。

要将镜像推送到 Docker Hub：

1.  如果您还没有 Docker Hub 账户，请创建一个

1.  使用 `docker login` 从命令行登录到 Docker Hub

1.  使用您的 Docker Hub 用户名标记镜像：`docker tag ds-python yourusername/ds-python`

1.  推送镜像：`docker push yourusername/ds-python`

`ds-python` 镜像现在托管在 Docker Hub 上。其他用户可以通过运行以下命令来拉取镜像：

```py
docker pull yourusername/ds-python
```

对于私有仓库，您可以创建一个组织并添加用户。这允许您在团队内部安全地共享 Docker 镜像。

# 第 5 步：加载和运行镜像

要在另一台系统上加载和运行 Docker 镜像：

1.  将 `ds-python.tar` 文件复制到新系统

1.  使用 `docker load -i ds-python.tar` 加载镜像

1.  使用 `docker run -p 8888:8888 ds-python` 启动一个容器

1.  在 `localhost:8888` 访问 Jupyter Lab

就是这样！`ds-python` 镜像现在已准备好在新系统上使用。

# 最终想法

这为您提供了使用 Docker 设置可重现的数据科学环境的快速入门指南。还需要考虑一些额外的最佳实践：

+   使用像 Python slim 这样的较小基础镜像来优化镜像大小

+   利用 Docker 卷进行数据持久化和共享

+   遵循安全原则，如避免以 root 用户身份运行容器

+   使用 Docker Compose 定义和运行多容器应用程序

我希望您觉得这个介绍有帮助。Docker 为简化和扩展数据科学工作流程提供了很多可能性。

**[Matthew Mayo](https://www.linkedin.com/in/mattmayo13/)** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 是数据科学家以及 KDnuggets 的总编辑，该网站是开创性的数据科学和机器学习在线资源。他的兴趣包括自然语言处理、算法设计和优化、无监督学习、神经网络以及自动化机器学习方法。Matthew 拥有计算机科学硕士学位和数据挖掘研究生文凭。他可以通过 editor1 at kdnuggets[dot]com 联系到。

### 更多相关内容

+   [如何有效管理镜像版本的 Docker 标签](https://www.kdnuggets.com/how-to-use-docker-tags-to-manage-image-versions-effectively)

+   [Mercury 概述：创建数据科学组合和…](https://www.kdnuggets.com/2022/05/overview-mercury-creating-data-science-portfolio-notebook-based-webapps.html)

+   [赢得演讲：创建和呈现有效的数据驱动演示](https://www.kdnuggets.com/2022/04/franks-winning-room-creating-delivering-effective-data-driven-presentation.html)

+   [使用 Python 创建一个从音频中提取主题的网络应用](https://www.kdnuggets.com/2023/01/creating-web-application-extract-topics-audio-python.html)

+   [为机器学习创建特征的挑战](https://www.kdnuggets.com/2022/02/challenges-creating-features-machine-learning.html)

+   [创建领域特定 AI 模型的最佳实践](https://www.kdnuggets.com/2022/07/best-practices-creating-domainspecific-ai-models.html)
