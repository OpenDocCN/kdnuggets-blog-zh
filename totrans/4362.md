# 使用 TensorFlow Serving 部署训练好的模型到生产环境

> 原文：[https://www.kdnuggets.com/2020/11/serving-tensorflow-models.html](https://www.kdnuggets.com/2020/11/serving-tensorflow-models.html)

[评论](#comments)![图示](../Images/cc3390e60f5b0669e9916ac5a0a8c044.png)

照片由 [Kate Townsend](https://unsplash.com/@k8townsend?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/s/photos/serve?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

一旦你训练了一个 TensorFlow 模型，并且它准备好部署，你可能会想将其移动到生产环境中。幸运的是，TensorFlow 提供了一种方法来以最小的努力完成这项工作。在本文中，我们将使用一个预训练模型，保存它，并使用 TensorFlow Serving 提供服务。让我们开始吧！

### TensorFlow ModelServer

[TensorFlow Serving](https://www.tensorflow.org/tfx/guide/serving) 是一个专门用于将机器学习模型投入生产的系统。TensorFlow 的 ModelServer 提供对 RESTful API 的支持。不过，在使用之前，我们需要先安装它。首先，让我们将其添加为软件包源。

```py
echo "deb [arch=amd64] [http://storage.googleapis.com/tensorflow-serving-apt](https://storage.googleapis.com/tensorflow-serving-apt) stable tensorflow-model-server tensorflow-model-server-universal" | sudo tee /etc/apt/sources.list.d/tensorflow-serving.list && curl [https://storage.googleapis.com/tensorflow-serving-apt/tensorflow-serving.release.pub.gpg](https://storage.googleapis.com/tensorflow-serving-apt/tensorflow-serving.release.pub.gpg) | sudo apt-key add -
```

现在可以通过更新系统并使用 `apt-get` 安装 TensorFlow ModelServer。

```py
$ sudo apt-get update$ sudo apt-get install tensorflow-model-server
```

### 开发模型

接下来，让我们使用一个预训练模型来创建我们希望提供的模型。在这种情况下，我们将使用一个版本的 [VGG16](https://arxiv.org/abs/1409.1556)，其权重在 [ImageNet](http://www.image-net.org/) 上进行了预训练。为了使其正常工作，我们必须先完成几个导入：

+   `VGG16` 架构

+   `image` 用于处理图像文件

+   `preprocess_input` 用于预处理图像输入

+   `decode_predictions` 用于显示概率和类别名称

```py
from tensorflow.keras.applications.vgg16 import VGG16
from tensorflow.keras.preprocessing import image
from tensorflow.keras.applications.vgg16 import preprocess_input, decode_predictions
import numpy as np
```

接下来，我们使用 ImageNet 权重定义模型。

```py
model = VGG16(weights=’imagenet’)
```

模型准备好后，我们可以尝试一个示例预测。我们首先定义图像文件（一个狮子）的路径，并使用 `image` 加载它。

```py
img_path = ‘lion.jpg’
img = image.load_img(img_path, target_size=(224, 224))
x = image.img_to_array(img)
x = np.expand_dims(x, axis=0)
x = preprocess_input(x)
```

在预处理后，我们可以使用它进行预测。我们可以看到它能够以 99% 的准确率预测图像是狮子。

```py
preds = model.predict(x)
# decode the results into a list of tuples (class, description, probability)
# (one such list for each sample in the batch)
print(‘Predicted:’, decode_predictions(preds, top=3)[0])# Predicted: [('n02129165', 'lion', 0.9999999), ('n02130308',
# 'cheetah', 7.703386e-08), ('n02128385', 'leopard', 6.330456e-09)]
```

现在我们有了模型，我们可以将其保存，以便为 TensorFlow 提供服务做好准备。

### 保存模型

现在让我们保存这个模型。请注意，我们将其保存到一个`/1`文件夹中以指示模型版本。这是**关键的**，特别是当你希望自动提供新的模型版本时。稍后会详细介绍。

```py
model.save(‘vgg16/1’)
```

### 使用 TensorFlow ModelServer 运行服务器

让我们首先定义用于服务的配置：

+   `name` 是我们模型的名称——在这种情况下，我们将其命名为 `vgg16`。

+   `base_path` 是我们保存模型的位置的绝对路径。请确保将其更改为您自己的路径。

+   `model_platform` 显然是 TensorFlow。

+   `model_version_policy` 使我们能够指定模型版本信息。

现在我们可以运行命令来从命令行提供模型：

+   `rest_api_port=8000` 意味着我们的 REST API 将在 8000 端口提供服务。

+   `model_config_file` 定义了我们之前定义的配置文件。

+   `model_config_file_poll_wait_seconds` 表示检查配置文件更改之前的等待时间。例如，将配置文件中的版本更改为 2 会自动服务模型的版本 2。这是因为在这种情况下配置文件的更改每 300 秒检查一次。

```py
tensorflow_model_server — rest_api_port=8000 — model_config_file=models.config — model_config_file_poll_wait_seconds=300
```

### 使用 REST API 进行预测

此时，我们的模型的 [REST API](https://www.tensorflow.org/tfx/serving/api_rest) 可以在这里找到: [http://localhost:8000/v1/models/vgg16/versions/1:predict](http://localhost:8000/v1/models/vgg16/versions/1:predict)。

我们可以使用这个端点进行预测。为此，我们需要将 [JSON](https://www.json.org/json-en.html) 格式的数据传递给端点。为了达到这一目的 — 并非双关 — 我们将使用 Python 中的 `*json*` 模块。为了向端点发出请求，我们将使用 `requests` Python 包。

让我们开始导入这两个模块。

```py
import json
import requests
```

记住，*x* 变量包含了预处理的图像。我们将创建包含该图像的 JSON 数据。像其他 RESTFUL 请求一样，我们将内容类型设置为 `application/json`。然后，我们向端点发出请求，同时传递头信息和数据。获得预测结果后，我们将其解码，就像在本文开头一样。

![Image for post](../Images/20bf95dec0e473ec61b0efb359e74d48.png)

### 使用 Docker 服务模型

还有一种更快更简短的方式来服务 TensorFlow 模型——使用 [Docker](https://hub.docker.com/r/tensorflow/serving)。这实际上是推荐的方法，但了解之前的方法很重要，以防你在特定用例中需要它。使用 Docker 服务模型就像拉取 [TensorFlow Serving 镜像](https://www.tensorflow.org/tfx/serving/docker) 并挂载你的模型一样简单。

使用 [Docker 安装](https://docs.docker.com/engine/install/) 后，运行此代码来拉取 TensorFlow Serving 镜像。

```py
docker pull tensorflow/serving
```

现在让我们使用那个图像来服务模型。这可以通过 `docker run` 和传递一些参数来完成：

+   `-p 8501:8501` 表示容器的 8501 端口将在本地主机的 8501 端口上可访问。

+   `— name` 用于为我们的容器命名——选择你喜欢的名称。我在这个例子中选择了 `tf_vgg_server`。

+   `— mount type=bind,source=/media/derrick/5EAD61BA2C09C31B/Notebooks/Python/serving/saved_tf_model,target=/models/vgg16` 表示模型将被挂载到 Docker 容器中的 `/models/vgg16`。

+   `-e MODEL_NAME=vgg16` 表明 TensorFlow Serving 应该加载名为 `vgg16` 的模型。

+   `-t tensorflow/serving` 表示我们正在使用之前拉取的 `tensorflow/serving` 镜像。

+   `&` 表示在后台运行命令。

在你的终端中运行下面的代码。

```py
docker run -p 8501:8501 --name tf_vgg_server --mount type=bind,source=/media/derrick/5EAD61BA2C09C31B/Notebooks/Python/serving/saved_tf_model,target=/models/vgg16  -e MODEL_NAME=vgg16 -t tensorflow/serving &
```

现在我们可以使用 REST API 端点进行预测，就像之前一样。

![Image for post](../Images/2efc691497994698fa72ade40bf44f76.png)

很明显，我们得到了相同的结果。通过这一点，我们已经看到如何在有无Docker的情况下服务TensorFlow模型。

### 最后的想法

[这篇文章](https://www.tensorflow.org/tfx/serving/architecture)来自TensorFlow，将为你提供更多关于TensorFlow Serving架构的信息。如果你想深入了解，可以参考这个[资源](https://www.tensorflow.org/tfx/serving/serving_config)。

你还可以探索使用标准的[TensorFlow ModelServer](https://www.tensorflow.org/tfx/serving/serving_advanced)来进行构建。在这篇文章中，我们专注于使用CPU进行服务，但你也可以探索如何[在GPU上服务](https://www.tensorflow.org/tfx/serving/docker)。

这个[仓库包含链接](https://github.com/tensorflow/serving)到更多关于TensorFlow Serving的教程。希望这篇文章对你有所帮助！

[**mwitiderrick/TensorFlow-Serving**](https://github.com/mwitiderrick/TensorFlow-Serving)

提供TensorFlow模型服务。通过在GitHub上创建账户，参与mwitiderrick/TensorFlow-Serving的开发。

**个人简介：Derrick Mwiti** 是一名数据科学家，对分享知识充满热情。他通过Heartbeat、Towards Data Science、Datacamp、Neptune AI、KDnuggets等博客积极贡献于数据科学社区。他的内容在互联网上的浏览量已超过一百万次。Derrick还是一名作者和在线讲师。他还与各种机构合作实施数据科学解决方案，并提升其员工技能。Derrick在多媒体大学学习数学和计算机科学，同时也是Meltwater创业技术学院的校友。如果你对数据科学、机器学习和深度学习感兴趣，你可以查看他的[完整数据科学与机器学习Python训练营课程](https://www.udemy.com/course/data-science-bootcamp-in-python/?referralCode=9F6DFBC3F92C44E8C7F4)。

[原文](https://heartbeat.fritz.ai/serving-tensorflow-models-3989df5d7d53)。转载已获许可。

**相关内容：**

+   [处理机器学习中的数据不平衡](/2020/10/imbalanced-data-machine-learning.html)

+   [如何将PyTorch Lightning模型部署到生产环境](/2020/11/deploy-pytorch-lightning-models-production.html)

+   [AI不仅仅是一个模型：完成工作流程成功的四个步骤](/2020/11/mathworks-ai-four-steps-workflow.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织在 IT 方面

* * *

### 更多相关内容

+   [2023 年 Feature Store 峰会：部署 ML 模型的实用策略](https://www.kdnuggets.com/2023/09/hopsworks-feature-store-summit-2023-practical-strategies-deploying-ml-models-production-environments)

+   [将机器学习模型部署到云端生产环境](https://www.kdnuggets.com/deploying-your-ml-model-to-production-in-the-cloud)

+   [排名前 7 的模型部署和服务工具](https://www.kdnuggets.com/top-7-model-deployment-and-serving-tools)

+   [机器学习模型部署：逐步教程](https://www.kdnuggets.com/deploying-machine-learning-models-a-step-by-step-tutorial)

+   [优先考虑生产中的数据科学模型](https://www.kdnuggets.com/2022/04/prioritizing-data-science-models-production.html)

+   [在 Heroku 云上部署深度学习 Web 应用的提示与技巧](https://www.kdnuggets.com/2021/12/tips-tricks-deploying-dl-webapps-heroku.html)
