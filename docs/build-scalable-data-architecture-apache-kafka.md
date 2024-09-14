# 如何使用 Apache Kafka 构建可扩展的数据架构

> 原文：[`www.kdnuggets.com/2023/04/build-scalable-data-architecture-apache-kafka.html`](https://www.kdnuggets.com/2023/04/build-scalable-data-architecture-apache-kafka.html)

![如何使用 Apache Kafka 构建可扩展的数据架构](img/9c5d0e1e11b48a64ac229f965a43009f.png)

图片由作者提供

Apache Kafka 是一个分布式消息传递系统，采用发布-订阅模型。它由 Apache 软件基金会开发，使用 Java 和 Scala 编写。Kafka 的创建旨在克服传统消息传递系统在分发和扩展性方面面临的问题。它可以以最小的延迟和高吞吐量处理和存储大量数据。由于这些优点，它适用于实时数据处理应用程序和流媒体服务。它目前是开源的，被许多组织如 Netflix、沃尔玛和 LinkedIn 使用。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

消息传递系统使多个应用程序可以相互发送或接收数据，而无需担心数据传输和共享问题。**点对点**和**发布-订阅**是两种广泛使用的消息传递系统。在点对点系统中，发送方将数据推送到队列中，接收方从队列中弹出数据，就像一个标准的队列系统，遵循 FIFO（先进先出）原则。此外，数据在被读取后会被删除，并且每次只允许一个接收方。接收方读取消息没有时间依赖。

![如何使用 Apache Kafka 构建可扩展的数据架构](img/efa5d4c06f76f04465e52ebcdb70dc19.png)

**图 1** 点对点消息系统 | 图片由作者提供

在发布-订阅模型中，发送方称为发布者，接收方称为订阅者。在这种模型下，多个发送方和接收方可以同时读取或写入数据。但是，它存在时间依赖性。消费者必须在一定时间内消费消息，因为超过该时间消息将被删除，即使未被读取。根据用户的配置，这个时间限制可以是一天、一周或一个月。

![如何使用 Apache Kafka 构建可扩展的数据架构](img/a47141ce8f4609d6c2550993775a5f18.png)

**图 2** 发布-订阅消息系统 | 图片由作者提供

# Kafka 架构

Kafka 架构由几个关键组件组成：

1.  主题

1.  分区

1.  代理

1.  生产者

1.  消费者

1.  Kafka 集群

1.  Zookeeper

![如何构建一个可扩展的数据架构与 Apache Kafka](img/883018809ecf5ef892050f17df50b2d0.png)

**图 3** Kafka 架构 | 图片来源：[ibm-cloud-architecture](https://ibm-cloud-architecture.github.io/refarch-eda/technology/kafka-overview/)

让我们简要了解每个组件。

Kafka 将消息存储在不同的**主题**中。主题是包含特定类别消息的组。它类似于数据库中的表。一个主题可以通过其名称唯一标识。我们不能创建两个名称相同的主题。

主题进一步被分类为**分区**。这些分区的每条记录都与一个称为**偏移量**的唯一标识符相关联，这个标识符表示记录在该分区中的位置。

除此之外，系统中还有生产者和消费者。生产者使用生产 API 在主题中写入或发布数据。这些生产者可以在主题或分区级别写入数据。

消费者使用消费者 API 从主题中读取或消费数据。他们也可以在主题或分区级别读取数据。执行类似任务的消费者将组成一个称为**消费者组**的群体。

还有其他系统，如**代理**和**Zookeeper**，它们在 Kafka 服务器的后台运行。代理是维护和记录已发布消息的软件。它还负责使用偏移量将正确的消息传递给正确的消费者，并按照正确的顺序传递。集体相互通信的代理组可以称为**Kafka 集群**。代理可以动态添加或从 Kafka 集群中移除，而不会导致系统停机。Kafka 集群中的一个代理被称为**控制器**。它管理集群内的状态和副本，并执行管理任务。

另一方面，Zookeeper 负责维护 Kafka 集群的健康状态，并与集群中的每个代理协调。它以键值对的形式维护每个集群的元数据。

本教程主要集中在 Apache Kafka 的实际实现上。如果你想了解更多关于其架构的内容，可以阅读[这篇](https://www.upsolver.com/blog/apache-kafka-architecture-what-you-need-to-know#:~:text=Its%20core%20architectural%20concept%20is,maintain%20a%20consistent%20system%20state.)由 Upsolver 撰写的文章。

# 出租车预订应用程序：一个实际的用例

考虑一个像 Uber 这样的出租车预订服务的用例。这个应用程序使用 Apache Kafka 通过各种服务（如事务、电子邮件、分析等）发送和接收消息。

![如何构建一个可扩展的数据架构与 Apache Kafka](img/664bb35c487cf63d41198f8b0d60b017.png)

**图 4** 出租车应用程序的架构 | 图片来源：作者

架构包括几个服务。`Rides`服务接收来自客户的乘车请求，并将乘车详细信息写入 Kafka 消息系统。

然后这些订单详细信息被`Transaction`服务读取，该服务确认订单和支付状态。确认该乘车后，`Transaction`服务将确认的乘车再次写入消息系统，并附加一些额外的详细信息。最后，确认的乘车详细信息会被其他服务（如电子邮件或数据分析）读取，以向客户发送确认邮件并进行一些分析。

我们可以以非常高的吞吐量和最小的延迟实时执行所有这些过程。此外，由于 Apache Kafka 的水平扩展能力，我们可以扩展此应用程序以处理数百万用户。

# 上述用例的实际实现

本节包含了在我们的应用程序中实现 Kafka 消息系统的快速教程。它包括下载 Kafka、配置它以及创建生产者-消费者函数的步骤。

**注意：** 本教程基于 Python 编程语言，并使用 Windows 机器。

## Apache Kafka 下载步骤

从[该链接](https://kafka.apache.org/downloads)下载最新版本的 Apache Kafka。Kafka 基于 JVM 语言，因此必须在系统中安装 Java 7 或更高版本。

1.  从计算机（C:）驱动器中提取下载的 zip 文件，并将文件夹重命名为`/apache-kafka`。

1.  上级目录包含两个子目录，`/bin`和`/config`，其中包含 Zookeeper 和 Kafka 服务器的可执行文件和配置文件。

## 配置步骤

首先，我们需要为 Kafka 和 Zookeeper 服务器创建日志目录。这些目录将存储这些集群的所有元数据以及主题和分区的消息。

**注意：** 默认情况下，这些日志目录会创建在`/tmp`目录中，这是一个易失性目录，当系统关闭或重新启动时，所有数据都会消失。我们需要为日志目录设置永久路径以解决此问题。让我们看看如何操作。

导航到`apache-kafka` >> `config`并打开`server.properties`文件。在这里，你可以配置 Kafka 的许多属性，例如日志目录路径、日志保留时间、分区数量等。

在`server.properties`文件中，我们需要将日志目录文件的路径从临时的`/tmp`目录更改为永久目录。日志目录包含 Kafka 服务器中生成或写入的数据。要更改路径，请将`log.dirs`变量从`/tmp/kafka-logs`更新为`c:/apache-kafka/kafka-logs`。这将使日志永久存储。

```py
log.dirs=c:/apache-kafka/kafka-logs
```

Zookeeper 服务器还包含一些日志文件，用于存储 Kafka 服务器的元数据。要更改路径，请重复上述步骤，即打开`zookeeper.properties`文件并按如下方式替换路径。

```py
dataDir=c:/apache-kafka/zookeeper-logs
```

这个 Zookeeper 服务器将作为我们 Kafka 服务器的资源管理器。

## 运行 Kafka 和 Zookeeper 服务器

要运行 Zookeeper 服务器，请在父目录下打开一个新的 cmd 提示符，并运行以下命令。

```py
$ .\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```

![如何使用 Apache Kafka 构建可扩展的数据架构](img/b6dc180652bec959922eb6b09f7361dd.png)

作者提供的图片

保持 Zookeeper 实例运行。

要运行 Kafka 服务器，请打开一个独立的 cmd 提示符，并执行以下代码。

```py
$ .\bin\windows\kafka-server-start.bat .\config\server.properties
```

保持 Kafka 和 Zookeeper 服务器运行，在下一节中，我们将创建生产者和消费者函数，它们将读取和写入 Kafka 服务器中的数据。

## 创建生产者和消费者函数

为了创建生产者和消费者函数，我们将以之前讨论的电子商务应用为例。`Orders` 服务将作为生产者，将订单详情写入 Kafka 服务器，而 Email 和 Analytics 服务将作为消费者，从服务器读取数据。Transaction 服务将既作为消费者又作为生产者工作。它读取订单详情，并在确认交易后将其重新写回。

但首先，我们需要安装 Kafka 的 Python 库，它包含用于生产者和消费者的内置函数。

```py
$ pip install kafka-python
```

现在，创建一个名为 `kafka-tutorial` 的新目录。我们将在该目录中创建包含所需函数的 Python 文件。

```py
$ mkdir kafka-tutorial
$ cd .\kafka-tutorial\
```

**生产者函数：**

现在，创建一个名为 `rides.py` 的 Python 文件，并将以下代码粘贴到其中。

`rides.py`

```py
import kafka
import json
import time
import random

topicName = "ride_details"
producer = kafka.KafkaProducer(bootstrap_servers="localhost:9092")

for i in range(1, 10):
    ride = {
        "id": i,
        "customer_id": f"user_{i}",
        "location": f"Lat: {random.randint(-90, 90)}, Long: {random.randint(-90, 90)}",
    }
    producer.send(topicName, json.dumps(ride).encode("utf-8"))
    print(f"Ride Details Send Succesfully!")
    time.sleep(5)
```

**解释：**

首先，我们导入了所有必要的库，包括 kafka。然后，定义了主题名称和各种项目的列表。请记住，主题是一个包含类似类型消息的组。在这个例子中，这个主题将包含所有的订单。

然后，我们创建一个 KafkaProducer 实例，并将其连接到运行在 localhost:9092 的 Kafka 服务器。如果您的 Kafka 服务器运行在其他地址和端口上，则必须在此处指定服务器的 IP 和端口号。

然后，我们将生成一些 JSON 格式的订单，并将它们写入定义的主题名称的 Kafka 服务器。使用 Sleep 函数在后续订单之间生成间隔。

**消费者函数：**

`transaction.py`

```py
import json
import kafka
import random

RIDE_DETAILS_KAFKA_TOPIC = "ride_details"
RIDES_CONFIRMED_KAFKA_TOPIC = "ride_confirmed"

consumer = kafka.KafkaConsumer(
    RIDE_DETAILS_KAFKA_TOPIC, bootstrap_servers="localhost:9092"
)
producer = kafka.KafkaProducer(bootstrap_servers="localhost:9092")

print("Listening Ride Details")
while True:
    for data in consumer:
        print("Loading Transaction..")
        message = json.loads(data.value.decode())
        customer_id = message["customer_id"]
        location = message["location"]
        confirmed_ride = {
            "customer_id": customer_id,
            "customer_email": f"{customer_id}@xyz.com",
            "location": location,
            "alloted_driver": f"driver_{customer_id}",
            "pickup_time": f"{random.randint(1, 20)}mins",
        }
        print(f"Transaction Completed..({customer_id})")
        producer.send(
            RIDES_CONFIRMED_KAFKA_TOPIC, json.dumps(confirmed_ride).encode("utf-8")
        )
```

**解释：**

`transaction.py` 文件用于确认用户所做的过渡，并分配给他们一个司机和预计的接送时间。它从 Kafka 服务器读取行程详情，并在确认行程后将其重新写入 Kafka 服务器。

现在，创建两个名为 `email.py` 和 `analytics.py` 的 Python 文件，这些文件分别用于向客户发送行程确认邮件和进行一些分析。这些文件的创建只是为了演示即使是多个消费者也可以同时从 Kafka 服务器读取数据。

`email.py`

```py
import kafka
import json

RIDES_CONFIRMED_KAFKA_TOPIC = "ride_confirmed"
consumer = kafka.KafkaConsumer(
    RIDES_CONFIRMED_KAFKA_TOPIC, bootstrap_servers="localhost:9092"
)

print("Listening Confirmed Rides!")
while True:
    for data in consumer:
        message = json.loads(data.value.decode())
        email = message["customer_email"]
        print(f"Email sent to {email}!")
```

`analysis.py`

```py
import kafka
import json

RIDES_CONFIRMED_KAFKA_TOPIC = "ride_confirmed"
consumer = kafka.KafkaConsumer(
    RIDES_CONFIRMED_KAFKA_TOPIC, bootstrap_servers="localhost:9092"
)

print("Listening Confirmed Rides!")
while True:
    for data in consumer:
        message = json.loads(data.value.decode())
        id = message["customer_id"]
        driver_details = message["alloted_driver"]
        pickup_time = message["pickup_time"]
        print(f"Data sent to ML Model for analysis ({id})!")
```

现在，我们已经完成了应用程序，在下一部分，我们将同时运行所有服务并检查性能。

## 测试应用程序

在四个不同的命令提示符中逐一运行每个文件。

```py
$ python transaction.py
```

```py
$ python email.py
```

```py
$ python analysis.py
```

```py
$ python ride.py
```

![如何使用 Apache Kafka 构建可扩展的数据架构](img/aa6353d2ed294dbf68800c4b306dd089.png)

作者提供的图片

当行程详情被推送到服务器时，您可以同时从所有文件中接收输出。您还可以通过删除`rides.py`文件中的延迟函数来提高处理速度。`rides.py`文件将数据推送到 kafka 服务器，其他三个文件则同时从 kafka 服务器读取数据并相应地执行功能。

希望您对 Apache Kafka 及其实现有一个基本的了解。

# 结论

在这篇文章中，我们了解了 Apache Kafka、它的工作原理以及如何使用出租车预订应用的案例来实际应用它。设计一个可扩展的 Kafka 管道需要精心规划和实施。您可以增加代理和分区的数量，以使这些应用程序更具可扩展性。每个分区是独立处理的，以便将负载分配到各个分区。同时，您可以通过设置缓存大小、缓冲区大小或线程数量来优化 kafka 配置。

[GitHub](https://github.com/aryan0141/apache-kafka-tutorial/tree/master) 链接，获取文章中使用的完整代码。

感谢阅读本文。如果您有任何评论或建议，请随时通过[Linkedin](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)与我联系。

**[Aryan Garg](https://www.linkedin.com/in/aryan-garg-1bbb791a3/)** 是一名电气工程学士学位学生，目前在本科的最后一年。他的兴趣领域是网页开发和机器学习。他已追求这一兴趣，并渴望在这些方向上进一步工作。

### 更多相关主题

+   [使用 Kafka 和 Risingwave 构建 F1 流数据管道](https://www.kdnuggets.com/building-a-formula-1-streaming-data-pipeline-with-kafka-and-risingwave)

+   [使用 SQL + Python 构建可扩展的 ETL](https://www.kdnuggets.com/2022/04/building-scalable-etl-sql-python.html)

+   [广义和可扩展的最优稀疏决策树（GOSDT）](https://www.kdnuggets.com/2023/02/generalized-scalable-optimal-sparse-decision-treesgosdt.html)

+   [KDnuggets™新闻 22:n07, 2 月 16 日：如何为机器学习学习数学…](https://www.kdnuggets.com/2022/n07.html)

+   [数据网格及其分布式数据架构](https://www.kdnuggets.com/2022/02/data-mesh-distributed-data-architecture.html)

+   [数据网格架构：重新构想数据管理](https://www.kdnuggets.com/2022/05/data-mesh-architecture-reimagining-data-management.html)
