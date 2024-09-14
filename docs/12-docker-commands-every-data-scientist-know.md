# 每个数据科学家应知道的 12 个 Docker 命令

> 原文：[https://www.kdnuggets.com/2023/01/12-docker-commands-every-data-scientist-know.html](https://www.kdnuggets.com/2023/01/12-docker-commands-every-data-scientist-know.html)

![每个数据科学家应知道的 12 个 Docker 命令](../Images/e346bd1486a4f74e575375cb7ebddacf.png)

作者提供的图片

从事数据科学项目总是令人兴奋的。然而，这也并非没有挑战。每个项目都需要你安装一长串（可能）库及其特定版本。因此，理清项目的依赖关系可能相当具有挑战性。这时 **Docker** 可以提供帮助。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 加入网络安全领域的快车道

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

Docker 是一种流行的容器化技术。使用 Docker，你可以将你的数据科学应用程序——包括代码和所需的依赖项——打包成一个称为 **镜像** 的可移植工件。因此，Docker 促进了开发环境的复制，使本地开发变得轻而易举。

以下是一些关键的 Docker 命令，它们在你进行下一个项目时会非常有用。我们将使用来自 [Docker Hub](https://hub.docker.com/) 的镜像，这是一个非常流行的平台，用于查找、共享和管理容器镜像。

# 1\. docker pull

要从 Docker Hub 拉取镜像，你可以运行 `docker pull` 命令，如下所示：

```py
docker pull <name-of-the-image>
```

例如，要从 Docker Hub 拉取 Python 镜像，你可以运行以下命令：

```py
docker pull python
```

![每个数据科学家应知道的 12 个 Docker 命令](../Images/34e7c97e5edabc29da75c888a3b9befe.png)

默认情况下，此命令拉取可用的 *最新* 版本的镜像。你可以 *选择性地* 添加标签以拉取特定版本的镜像。

> **注意**：如果你想以非超级用户权限运行 Docker 命令，可以创建 `docker` 组并将用户添加到该组中。

# 2\. docker images

要查看所有已下载的镜像列表，你可以运行 `docker images` 命令。

```py
docker images
```

![每个数据科学家应知道的 12 个 Docker 命令](../Images/2c0234a7a6767d6dce9bc0389c96e2d4.png)

# 3\. docker run

你可以使用 docker run 命令从下载的镜像启动一个容器。下载镜像后，你可以启动一个 docker 容器，即镜像的运行实例，如下所示：

```py
docker run <name-of-the-image>
docker run [options] <name-of-the-image> 
```

例如，你可以使用 -i 选项在启动容器时启动一个交互式 Python REPL，而 -t 选项分配一个伪终端，如下所示：

![12 Docker Commands Every Data Scientist Should Know](../Images/7baf6b74b34c36c7f859df5a01f25cda.png)

镜像是一个可移植的工件，容器是该镜像的运行实例。这意味着你可以从单个 Docker 镜像运行多个容器。

![12 Docker Commands Every Data Scientist Should Know](../Images/a640a18ff7397b92ed31916cd71e9ffa.png)

图片作者

# 4\. docker ps

你可以运行 `docker ps` 命令来获取所有运行中的容器的列表。

```py
docker ps
```

![12 Docker Commands Every Data Scientist Should Know](../Images/9bba5e921e73f949cb50daad4db825d2.png)

请注意，每个 Docker 容器都有一个 `CONTAINER ID`。在接下来的几分钟内，我们将学习 Docker 命令来停止和重启容器、检查日志等。我们将在这些命令中使用特定容器的 `CONTAINER ID`。

假设你在之前的会话中运行了一个容器，并且该容器现在不再运行。在这种情况下，你可以运行带有 `-a` 选项的 `docker ps` 命令。这将列出所有容器：当前运行的容器以及之前停止的容器。

```py
docker ps -a
```

# 5\. docker stop

有时你可能需要停止一个正在运行的容器。要做到这一点，请运行 `docker stop` 命令。

```py
docker stop <CONTAINER ID>
```

# 6\. docker start

你可以使用 `docker start` 命令来重启之前停止的容器。你可以运行 `docker ps -a` 命令，获取容器 ID，然后在 `docker start` 命令中使用该 ID 来重启容器。

```py
docker start <CONTAINER ID>
```

# 7\. docker rmi

要移除特定的镜像，可以运行 `docker rmi` 命令。

```py
docker rmi <name-of-the-image>
```

运行此命令会从本地开发环境中移除镜像。下次你想从该镜像启动容器时，需要从 DockerHub 拉取镜像。

# 8\. docker rm

要从开发环境中永久移除一个容器，你可以运行 `docker rm` 命令。但是，建议确保容器已停止后再尝试移除它。

```py
docker rm <CONTAINER ID>
```

# 9\. docker logs

`docker logs` 命令在调试容器时特别有用。

```py
docker logs <CONTAINER ID>
```

![12 Docker Commands Every Data Scientist Should Know](../Images/6d8afbc8cd76bee3aed7626fe3bc81ca.png)

# 10\. docker exec

使用 `docker exec` 命令，你可以在运行中的容器内执行命令。

```py
docker exec <CONTAINER ID> <COMMAND> <ARGS>
```

> **亲自尝试**：作为一个快速练习，总结你所学的内容，从 Docker Hub 拉取 [官方 Bash 镜像](https://hub.docker.com/_/bash)。接下来，尝试在启动容器时启动一个交互式终端会话，并运行一个基本的 Bash 命令。

# 11\. docker version

要检查工作环境中安装的 docker 版本，运行 `docker version` 命令：

```py
docker version
```

![12 Docker Commands Every Data Scientist Should Know](../Images/ce3c19af81f81817ef1bdd81ae887d37.png)

# 12\. docker info

`docker info` 命令提供了有关系统范围内Docker安装的更详细信息。

```py
docker info
```

![每个数据科学家都应该知道的12个Docker命令](../Images/8963e50a7b615b55bbac24a6e3f7385f.png)

docker info的输出（截断）

# 结论

希望你觉得这个关于必备Docker命令的教程对你有帮助。一旦你熟悉了Docker，你可以尝试将你的Python和数据科学应用程序Docker化。然后，你可以将应用程序的镜像推送到DockerHub。其他开发者将能够拉取你的镜像并在他们的工作环境中启动容器——这一切只需一个命令。

**[Bala Priya C](https://twitter.com/balawc27)** 是一位技术作家，喜欢创建长篇内容。她的兴趣领域包括数学、编程和数据科学。她通过编写教程、操作指南等，向开发者社区分享她的学习经验。

### 更多相关话题

+   [KDnuggets新闻，6月29日：数据科学的20个基础Linux命令…](https://www.kdnuggets.com/2022/n26.html)

+   [数据科学家的14个必备Git命令](https://www.kdnuggets.com/2022/06/14-essential-git-commands-data-scientists.html)

+   [数据科学初学者的20个基础Linux命令](https://www.kdnuggets.com/2022/06/20-basic-linux-commands-data-science-beginners.html)

+   [数据科学的16个必备DVC命令](https://www.kdnuggets.com/2022/07/16-essential-dvc-commands-data-science.html)

+   [数据科学的10个必备SQL命令](https://www.kdnuggets.com/2022/10/10-essential-sql-commands-data-science.html)

+   [Streamlit的12个必备命令](https://www.kdnuggets.com/2023/01/12-essential-commands-streamlit.html)
