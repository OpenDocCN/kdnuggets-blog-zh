# 如何优化 Dockerfile 指令以加快构建时间

> 原文：[https://www.kdnuggets.com/how-to-optimize-dockerfile-instructions-for-faster-build-times](https://www.kdnuggets.com/how-to-optimize-dockerfile-instructions-for-faster-build-times)

![如何优化 Dockerfile 指令以加快构建时间](../Images/6482b856bf98e0dd989cc253f576f91b.png)

编辑者提供的图像 | Midjourney & Canva

你可以通过利用构建缓存、减少构建上下文等方式来优化 Dockerfiles 以加快构建时间。本教程介绍了创建 Dockerfiles 时需要遵循的最佳实践。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

## 前提条件

你应该安装 Docker。如果尚未安装，请 [获取 Docker](https://docs.docker.com/get-docker/) 适用于你的操作系统。

## 1\. 使用更小的基础镜像

首先，你可以从更小的基础镜像开始，创建最小化镜像。这可以减少 Docker 镜像的总体大小，并加快构建过程。

例如，当容器化 Python 应用程序时，可以从 `python:3.x-slim` 镜像开始，这是一种较小的 `python:3.x` 版本，仅包含运行 Python 所需的基本组件，而不是默认的 `python:3.x`。

阅读 [如何为 Python 应用程序创建最小化 Docker 镜像](https://www.kdnuggets.com/how-to-create-minimal-docker-images-for-python-applications) 以了解更多。

## 2\. 利用 Docker 构建缓存

Dockerfile 中指令的顺序会影响构建时间，因为 Docker 会利用其构建缓存。

Docker 通过顺序执行 Dockerfile 中的指令来构建镜像——为每个指令创建一个新的镜像层。如果某层自上次构建以来没有更改，Docker 可以重用缓存的层，从而加快构建过程。

所以，**优化指令顺序以最大化缓存命中率** 是很重要的：

+   **将经常更改的指令放在最后**：将那些经常更改的指令，比如复制应用程序代码，放在 Dockerfile 的末尾。这可以减少使整个构建缓存失效的可能性。

+   **将不经常更改的指令放在前面**：像安装操作系统包、设置环境变量以及安装不常更改的依赖项等指令应放在前面，以最大化缓存命中率。

让我们看一个示例 Dockerfile：

```py
# Suboptimal ordering
FROM python:3.11-slim

# Set the working directory
WORKDIR /app

# Copy the entire application code
COPY . .

# Install the required Python packages
RUN pip install --no-cache-dir -r requirements.txt

# Expose the port on which the app runs
EXPOSE 5000

# Run the application
CMD ["python3", "app.py"] 
```

在这个初始的 Dockerfile 中，任何应用代码的更改都会使整个构建过程的缓存失效，包括依赖项的安装。

以下是优化版本：

```py
# Better ordering of instructions
FROM python:3.11-slim

# Set the working directory
WORKDIR /app

# Install dependencies
COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy the current directory contents into the container at /app
COPY . .

# Expose the port on which the app runs
EXPOSE 5000

# Run the application
CMD ["python3", "app.py"] 
```

在这个优化过的 Dockerfile 中，如果应用代码发生更改，Docker 仍然可以使用缓存层来安装依赖项。这样，应用代码的更改不会不必要地触发重新安装依赖项。

## 3\. 使用多阶段构建

多阶段构建允许你将构建环境与最终运行环境分开，这样可以通过仅包含必要的运行时依赖项来减少最终镜像的大小。

考虑以下使用多阶段构建的 Dokcerfile：

```py
# Build stage
FROM python:3.11 AS builder
RUN apt-get update && apt-get install -y build-essential
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Final stage
FROM python:3.11-slim
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin
COPY . /app
WORKDIR /app
CMD ["python3", "app.py"] 
```

在这个例子中，构建依赖项在 `builder` 阶段安装，只有必要的运行时依赖项被复制到最终镜像中。

## 4\. 使用 `.dockerignore` 文件来最小化构建上下文

确保你有一个 `.dockerignore` 文件，以排除不必要的文件被复制到 Docker 上下文中，从而减少构建时间。类似于 `.gitignore`，此文件告知 Docker 在构建过程中忽略哪些文件，从而减少上下文大小。

在 `.dockerignore` 文件中，你可以包含临时文件、虚拟环境、IDE 设置和其他你不希望包含在构建上下文中的不必要文件。

从减少基础镜像大小到优化构建上下文，这些优化应有助于提高 Docker 构建的效率。

## 额外资源

以下资源值得了解更多：

+   [Docker 构建缓存](https://docs.docker.com/build/cache/)

+   [Dockerfile 指令的最佳实践](https://docs.docker.com/develop/develop-images/instructions/)

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是一位来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交叉点上工作。她的兴趣和专长领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她正致力于通过撰写教程、使用指南、观点文章等与开发者社区分享她的知识。Bala 还创建了引人入胜的资源概述和编码教程。

### 更多相关主题

+   [如何优化 SQL 查询以更快地检索数据](https://www.kdnuggets.com/2023/06/optimize-sql-queries-faster-data-retrieval.html)

+   [量身定制 ChatGPT 以满足您的需求](https://www.kdnuggets.com/2023/08/tailor-chatgpt-fit-needs-custom-instructions.html)

+   [优化和管理机器学习生命周期的前 10 个 MLOps 工具](https://www.kdnuggets.com/2022/10/top-10-mlops-tools-optimize-manage-machine-learning-lifecycle.html)

+   [使用 AI 和分析引擎更快地准备时间序列数据](https://www.kdnuggets.com/2021/12/piexchange-faster-way-prepare-timeseries-data-ai-analytics-engine.html)

+   [通过参与竞赛将机器学习速度提高 4 倍](https://www.kdnuggets.com/2022/01/learn-machine-learning-4x-faster-participating-competitions.html)

+   [oBERT：复合稀疏化实现更快、更准确的 NLP 模型](https://www.kdnuggets.com/2022/05/obert-compound-sparsification-delivers-faster-accurate-models-nlp.html)
