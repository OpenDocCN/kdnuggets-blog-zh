# 如何使用 Docker 卷进行持久数据存储

> 原文：[`www.kdnuggets.com/how-to-use-docker-volumes-for-persistent-data-storage`](https://www.kdnuggets.com/how-to-use-docker-volumes-for-persistent-data-storage)

![如何使用 Docker 卷进行持久数据存储](img/77e40aefad5bc5fdb7a05f51fb881cf8.png)

使用 Docker 时，你可以使用卷来持久保存数据，即使你停止或重启容器。我们将为 PostgreSQL 创建和使用 Docker 卷。

## 前提条件

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

要跟随本教程：

+   你应该在你的机器上安装了[Docker](https://docs.docker.com/get-docker/)

+   你应该对 Docker 命令和 PostgreSQL 感到熟悉

## 步骤 1：拉取 PostgreSQL 镜像

首先，我们从 DockerHub 拉取 PostgreSQL 镜像：

```py
$ docker pull postgres
```

## 步骤 2：创建 Docker 卷

接下来，让我们创建一个 Docker 卷来存储数据。即使容器被移除，这个卷也会持久保存数据。

```py
$ docker volume create pg_data
```

## 步骤 3：运行 PostgreSQL 容器

现在我们已经拉取了镜像并创建了卷，我们可以运行 PostgreSQL 容器，将创建的卷附加到它上面。

```py
$ docker run -d \
	--name my_postgres \
	-e POSTGRES_PASSWORD=mysecretpassword \
	-v pg_data:/var/lib/postgresql/data \
	-p 5432:5432 \
	postgres
```

这个命令在分离模式下运行`my_postgres`。使用 -**v pg_data:/var/lib/postgresql/data** 将`pg_data`卷挂载到容器中的**/var/lib/postgresql/data**。并且使用**-p 5432:5432**将`my_postgres`的 5432 端口映射到主机上的 5432 端口。

## 步骤 4：验证卷使用情况

现在我们已经创建了卷，可以验证它是否正在使用。你可以检查卷并查看其内容。

```py
$ docker volume inspect pgdata
```

运行此命令将显示有关卷的详细信息，包括其在主机系统上的挂载点。你可以导航到该目录并查看 PostgreSQL 数据文件。

```py
[
	{
    	"CreatedAt": "2024-08-07T15:53:23+05:30",
    	"Driver": "local",
    	"Labels": null,
    	"Mountpoint": "/var/lib/docker/volumes/pg_data/_data",
    	"Name": "pg_data",
    	"Options": null,
    	"Scope": "local"
	}
]
```

## 步骤 5：创建数据库和表

连接到 Postgres 实例，创建数据库和表。

启动一个 psql 会话：

```py
$ docker exec -it my_postgres psql -U postgres
```

创建一个新的数据库：

```py
CREATE DATABASE mydb;
```

连接到新的数据库：

```py
\c mydb
```

创建一个表并插入一些数据：

```py
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);

INSERT INTO users (name, email) VALUES ('Abby', 'abby@example.com'), ('Bob', 'bob@example.com');
```

运行一个示例查询：

```py
SELECT * FROM users;
```

输出：

```py
 id | name |  	email  	 
----+------+------------------
  1 | Abby | abby@example.com
  2 | Bob  | bob@example.com
```

## 步骤 6：停止并删除容器

停止正在运行的容器并将其删除。我们这样做是为了测试即使容器被停止，数据是否仍然持久。

```py
$ docker stop my_postgres
$ docker rm my_postgres
```

## 步骤 7：使用相同的卷重新运行 Postgres 容器

启动一个新的 PostgreSQL 容器，使用相同的卷以确保数据持久性。

```py
$ docker run -d \
	--name my_postgres \
	-e POSTGRES_PASSWORD=mysecretpassword \
	-v pgdata:/var/lib/postgresql/data \
	-p 5432:5432 \
	postgres
```

连接到 Postgres 实例并验证数据是否持久。

打开一个 psql 会话：

```py
$ docker exec -it my_postgres psql -U postgres
```

连接到`mydb`数据库：

```py
\c mydb
```

验证`users`表中的数据：

```py
SELECT * FROM users;
```

你仍然应该看到输出：

```py
 id | name |  	email  	 
----+------+------------------
  1 | Abby | abby@example.com
  2 | Bob  | bob@example.com
```

我希望这个教程能帮助你理解如何使用卷来持久化 Docker 中的数据。

## 额外资源

要了解更多信息，请阅读以下资源：

+   [卷 | Docker 文档](https://docs.docker.com/storage/volumes/)

+   [在 Docker 中管理数据](https://docs.docker.com/storage/)

祝你探索愉快！

**[](https://twitter.com/balawc27)**[Bala Priya C](https://www.kdnuggets.com/wp-content/uploads/bala-priya-author-image-update-230821.jpg)**** 是来自印度的开发者和技术作家。她喜欢在数学、编程、数据科学和内容创作的交汇点上工作。她的兴趣和专业领域包括 DevOps、数据科学和自然语言处理。她喜欢阅读、写作、编码和喝咖啡！目前，她正在通过撰写教程、操作指南、观点文章等，学习并与开发者社区分享她的知识。Bala 还制作了引人入胜的资源概述和编码教程。

### 更多相关话题

+   [如何有效使用 Docker 标签来管理镜像版本](https://www.kdnuggets.com/how-to-use-docker-tags-to-manage-image-versions-effectively)

+   [优化数据存储：探索 SQL 中的数据类型和规范化](https://www.kdnuggets.com/optimizing-data-storage-exploring-data-types-and-normalization-in-sql)

+   [从 Oracle 到 AI 数据库：数据存储的演变](https://www.kdnuggets.com/2022/02/oracle-databases-ai-evolution-data-storage.html)

+   [云存储采纳是企业当前的需求](https://www.kdnuggets.com/2022/02/cloud-storage-adoption-need-hour-business.html)

+   [每个数据科学家都应该知道的 12 条 Docker 命令](https://www.kdnuggets.com/2023/01/12-docker-commands-every-data-scientist-know.html)

+   [数据科学的 Docker 备忘单](https://www.kdnuggets.com/2023/02/docker-data-science-cheat-sheet.html)
