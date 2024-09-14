# AWS 上的 IoT：来自传感器数据的机器学习模型和仪表板

> 原文：[https://www.kdnuggets.com/2018/06/zimbres-iot-aws-machine-learning-dashboard.html](https://www.kdnuggets.com/2018/06/zimbres-iot-aws-machine-learning-dashboard.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png) [评论](#comments)

**由 [Rubens Zimbres](https://www.linkedin.com/in/rubens-zimbres), 数据科学家**

Google Colab 具有帮助数据科学家在全球范围内进行工作的开源项目。受到这种思维方式的启发，我使用我的笔记本作为 IoT 设备，AWS IoT 作为基础设施，开发了我的第一个 IoT 项目。

所以，我有一个“简单”的想法：从运行 Ubuntu 的笔记本电脑中收集 CPU 温度，发送到 Amazon AWS IoT，保存数据，使其可用于机器学习模型和仪表板。

然而，这个想法的实施相当复杂：首先，开发一个 Python 笔记本，内部运行 Ubuntu 命令行（‘sensors’），收集 CPU 温度，并能够通过合适的安全协议使用 MQTT 连接到 AWS IoT。无需使用像 Mosquitto 这样的 MQTT 代理。

需要在 AWS IoT 创建一个 Thing，获取证书，创建并附加策略，并创建一个 SQL 规则，将数据（JSON）发送到 Cloud Watch 和 Dynamo DB。然后，从 Dynamo DB 到 S3 创建一个数据管道，使数据可用于机器学习模型以及 AWS Quick Sight 仪表板。

让我们从在 Ubuntu 16.04 中安装‘sensors’和在 Anaconda 3 中安装‘AWSIoTPythonSDK’库开始：

```py

$ sudo apt-get install lm-sensors
$ sudo service kmod start  

```

我们来看看‘sensors’命令的样子：

![图片](../Images/82c2d2ab18a91a704f8880a9f54eb20a.png)

现在，安装 AWSIoTPythonSDK 库：

```py

$ pip install AWSIoTPythonSDK

```

从 Python 笔记本开始：以下函数用于在延迟 5 秒后收集 CPU 温度：

```py

import subprocess
import shlex
import time

def measure_temp():
        temp = subprocess.Popen(shlex.split('sensors -u'),
                                stdout=subprocess.PIPE,
                                bufsize=10, universal_newlines=True)
        return temp.communicate()

while True:
    string=measure_temp()[0]
    print(string.split()[8])
    time.sleep(5)
```

然后，我们从 Linux 命令行运行笔记本：

![图片](../Images/ce5145e73ef340db7ae016a891cad001.png)

好的。现在这段代码被插入到 AWSIoTPythonSDK 库中的 basicPubSub.py 笔记本中，如下所示：

```py

while True:
    if args.mode == 'both' or args.mode == 'publish':
        args.message=measure_temp()[0].split()[8]
        mess={"reported": {"light": "blue",
                                       "Temperature": measure_temp()[0].split()[8],"timestamp": time.time()
                                     },"timestamp": 1526519248}
        args.message=mess
        print(measure_temp()[0].split()[8],(time.time()-start)/60,'min')
        print(mess,'\n')
        message = {}
        message['message'] = args.message
        message['sequence'] = loopCount
        messageJson = json.dumps(message)
        myAWSIoTMQTTClient.publish(topic, messageJson, 1)
        if args.mode == 'publish':
            print('Published topic %s: %s\n' % (topic, messageJson))
        loopCount += 1
    time.sleep(5)

```

很棒。我们有一个 Python 笔记本，它将通过 MQTT 协议连接到 AWS IoT Core。现在我们在 AWS IoT 上设置影子（JSON 文件），这类似于 Microsoft 的‘device twin’。注意，由于我只有一个设备，所以我没有在 JSON 文件中插入设备 ID。

```py

{
  "desired": {
    "light": "green",
    "Temperature": 55,
    "timestamp": 1526323886
  },
  "reported": {
    "light": "blue",
    "Temperature": 55,
    "timestamp": 1526323886
  },
  "delta": {
    "light": "green"
  }
}

```

现在我们获得了证书.pem、.key 文件和 rootCA.pem 以确保安全连接。我们在 Ubuntu 中按 CTRL+ALT+T 打开命令行，输入命令并发布到一个主题 '-t'：

```py

$ python basicPubSub_adapted.py -e 1212345.iot.us-east-1.amazonaws.com -r rootCA.pem -c 2212345-certificate.pem.crt -k 2212345-private.pem.key -id arn:aws:iot:us-east-1:11231112345:thing/CPUUbuntu -t 'Teste'
```

我们将在 Linux shell 中收到来自 AWS IoT 连接的反馈，并在 AWS IoT 监控工具中检查（1 分钟后）连接是否成功：

![图片](../Images/7177fd039e2ed87c2d4478dafa3ad5dc.png)

还可以查看消息是否正在发布（橙色区域），以及用于连接的协议（左侧）：

![图片](../Images/941499f684dd7b4e951657720d97ea6d.png)

同时，我们可以看到‘shadow’也在更新中（中间）：

![图片](../Images/e7e43d00421e4d3e1b8c499d87714f1f.png)

现在我们创建一个 SQL 规则，将数据发送到 Cloud Watch 和 Dynamo DB，并创建 IAM 角色、策略和权限：

![Image](../Images/9a101f2e1a378e2363376ae28f716fcf.png)

![Image](../Images/734310ed2148a0933de572fb7b9f54ca.png)

数据随后保存到 DynamoDB，作为 JSON 文件。你可以使用 MessageID 作为主键，而不是时间戳。

![Image](../Images/0cb9407ab4ec653e86db220e76b56ca7.png)

现在我们可以在 CloudWatch 中可视化云动态和数据传输：

![Image](../Images/664b5f745138cd0395f21246a0fd3b3d.png)

然后我们创建一个从 DynamoDB 到 S3 的数据管道，以便 QuickSight 使用：

![Image](../Images/1be476996e3aa4cbd73c77d76c4d4989.png)

还需要创建一个 JSON 文件并设置 IAM 权限，以便 QuickSight 可以从 S3 存储桶中读取：

```py

{
    "fileLocations": [
        {
            "URIs": [
                "https://s3.amazonaws.com/your-bucket/2018-05-19-19-41-16/12345-c2712345-12345"
            ]
        },
        {
            "URIPrefixes": [
                "https://s3.amazonaws.com/your-bucket/2018-05-19-19-41-16/12345-c2712345-12345"
            ]
        }
    ],
    "globalUploadSettings": {
        "format": "JSON",
        "delimiter": "\n","textqualifier":"'"
    }
}

```

现在我们在 QuickSight 中有了静态的 CPU 温度图。

![Image](../Images/fc12a46fa8ff728d3c2d49b569c3edd2.png)

此外，S3 数据（.JSON 文件）现在可用于机器学习模型，如异常检测、预测和分类，使得可以使用 Sage Maker 和深度学习库创建管道 = 有趣。

这是一种很好的方式来接触 Amazon AWS 服务，如 EC2、IoT、Cloud Watch、DynamoDB、S3、Quick Sight 和 Lambda。设置所有这些服务及其依赖关系确实不容易，但这个部分的项目成本不到 1 美元。而且非常有趣！

这是项目第一部分在 AWS 上的流程图：

![Image](../Images/a747d00790333e65eed9bad2f560cb0a.png)

### 项目第二部分 – 接近实时仪表盘

现在让我们开发第二个解决方案，使用从 AWS IoT 发送到 Kinesis / Firehose 的流数据，然后到 AWS ElasticSearch，最后到 Kibana，形成一个接近实时的仪表盘。你可以选择使用 Lambda 清理和提取数据（也可以不使用），将 AWS IoT 作为输入，AWS Batch 作为输出连接到 Kinesis。无论如何，Kibana 能够解析你的 JSON 文件。

![Image](../Images/dd47965d3e2952596fe435604fde331d.png)

首先，我们必须为 AWS IoT 设置另一条规则，以将遥测数据发送到 Kinesis Firehose 流：

![Image](../Images/06b92365ebdbe5924ad2c04c0296fd43.png)

然后创建一个 Elastic Search 域

![Image](../Images/7cd6885fb708b7f253dc0ef2da6aa256.png)

设置对特定 IP 的访问：

```py

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:*",
      "Resource": "arn:aws:es:us-east-1:12345:domain/domain/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": "178.042.222.33"
        }
      }
    }
  ]
}
```

然后我们使用 Kinesis Firehose 创建流和流传输。

![Image](../Images/c275d7f80044f2f0ab3c9ee10ed3e2d3.png)

![Image](../Images/07ec9b798b92160182af451163f1a582.png)

最后，我们将 AWS Elasticsearch 与 Kibana 连接，在 Kibana 的“Dev Tools”中进行调整：

```py

PUT /data
{
 "mappings": {
  "doc": {
   "properties": {
     "light":{"type":"text"},
    "Temperature": {"type": "integer"},
    "timestamp": {"type": "integer"}
   }
  }
 }
}
```

注意 Elasticsearch 将提供一个 Kibana 端点。最后，我们有了我们的接近实时的 CPU 温度仪表盘。值得注意的是，我们几乎处于实时环境。问题在于 Kibana 每 5 秒（或 15 秒，如果你愿意）更新一次图形，但 Elasticsearch 的最小延迟为 60 秒。

我们现在可以可视化我们的炫酷仪表盘：

![Image](../Images/0afd4b1e7d4fe0761d02d5c25ebe7440.png)

更多信息和文件请访问我的GitHub - 仓库2018（CPU温度 – IoT项目）： [https://github.com/RubensZimbres/Repo-2018](https://github.com/RubensZimbres/Repo-2018)

**简历： [鲁本斯·津布雷斯](https://www.linkedin.com/in/rubens-zimbres)** 是一名数据科学家，拥有人工智能和细胞自动机方向的商业管理博士学位。目前在电信领域工作，为金融行业和农业开发机器学习、深度学习模型及IoT解决方案。

**相关：**

+   [通过命令行在TensorFlow中使用GANs：创建你的第一个GitHub项目](/2018/05/zimbres-first-github-project-gans.html)

+   [将“科学”带回数据科学](/2017/09/science-data-science.html)

+   [应用于大数据的机器学习，详解](/2017/07/machine-learning-big-data-explained.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT需求

* * *

### 更多相关主题

+   [为有效的Tableau和Power BI仪表板准备数据](https://www.kdnuggets.com/2022/06/prepare-data-effective-tableau-power-bi-dashboards.html)

+   [迁移到AWS云的11个最佳实践](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)

+   [在AWS EC2上设置和使用JupyterHub (TLJH)](https://www.kdnuggets.com/2023/01/setup-jupyterhub-tljh-aws-ec2.html)

+   [使用Datawig，一个用于缺失值填补的AWS深度学习库](https://www.kdnuggets.com/2021/12/datawig-aws-deep-learning-library-missing-value-imputation.html)

+   [AIoT革命：AI和IoT如何改变我们的世界](https://www.kdnuggets.com/2022/07/aiot-revolution-ai-iot-transforming-world.html)

+   [KDnuggets新闻，7月27日：AIoT革命：AI和IoT如何…](https://www.kdnuggets.com/2022/n30.html)
