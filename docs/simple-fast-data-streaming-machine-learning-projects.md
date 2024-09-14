# 简单快速的数据流用于机器学习项目

> 原文：[`www.kdnuggets.com/2022/11/simple-fast-data-streaming-machine-learning-projects.html`](https://www.kdnuggets.com/2022/11/simple-fast-data-streaming-machine-learning-projects.html)

![简单快速的数据流用于机器学习项目](img/b9f199330321477dc272afb77c29bb8e.png)

图像作者

你是否曾经想过为什么你必须等待 DVC 拉取所有文件才能访问单个文件？也许你创建了自定义脚本来解决这个问题。但如果我告诉你还有更好的解决方案，你会如何想？

[直接数据访问](https://github.com/DagsHub/client/) 使你能够轻松加载来自 DagsHub DVC 服务器的单个或多个文件。你还可以使用上传 API 上传单个文件或多个文件。这将帮助你节省时间，因为你无需拉取整个数据集来推送单个文件。

直接数据访问在所有类型的 Python 库中表现更好，特别是那些使用高功能级别的库。在训练机器学习模型的情况下，你可以将数据直接加载到 DataLoader 中，开始训练或微调模型。

在本教程中，我们将深入研究 DagsHub 的直接数据访问，并使用 FastAI-2 和直接数据访问训练简单的瑜伽姿势分类模型。

# 开始使用直接数据访问

直接数据访问附带了 [DagsHub](https://github.com/DagsHub/client/) 客户端 API。你可以根据需要从 DagsHub 服务器访问文件，而不是使用 `dvc pull` 下载所有数据。

流式传输的文件与保存在本地磁盘上的文件无异。你可以提供一个本地文件路径，它会从 DagsHub 拉取文件。

使用 GitHub URL 和 pip 命令安装 DagsHub API。

```py
!pip install git+https://github.com/DagsHub/client.git
```

<class></class>

或者

```py
!pip install dagshub
```

> **注意：** 目前，直接数据访问功能处于测试版，有一些小问题。

## 手动方法

手动方法允许你使用 DagsHubFilesystem 流式传输文件。

**你只需替换：**

| **Python API** | **DagsHub 文件系统** |
| --- | --- |
| open() | fs.open() |
| os.stat() | fs.stat() |
| os.listdir() | fs.listdir() |
| os.scandir() | fs.scandir() |

如果你在 Git 仓库中，你不需要提供配置参数。

要覆盖自动检测的配置，请使用：

+   repo_url

+   username

+   Password

如果你在 Git 仓库外部，你也可以使用 `project_root` 参数指向项目目录。

在这个例子中，我们创建了一个 DagsHubFilesystem 并使用 listdir() 显示了文件和文件夹的列表。

```py
from dagshub.streaming import DagsHubFilesystem
fs = DagsHubFilesystem()
fs.listdir()
```

它显示了在本地目录中不可用但在 DVC 服务器上的“Mobilenet-Weights”和“Saved_Models”文件夹。简而言之，你可以访问在 DagsHub 仓库中看到的所有文件，而无需执行 `dvc pull` 命令。

```py
['Mobilenet-Weights',
 'Mobilenet-Weights.dvc',
 '.git',
 'dvc.lock',
 '.gitignore',
 'README.md',
 'src',
 'Saved_Models',
 'LICENSE',
 'Data',
 'dvc.yaml',
 '.dvc',
 'requirments.txt',
 '.dagshub-streaming',
 '.dvcignore']
```

## 自动方法

`install_hooks` 允许 Python 库（*某些例外情况适用*）像访问系统上的文件一样访问 DVC 跟踪的文件。它简单且快速。

在下面的示例中，我们调用了`install_hooks()`并使用 PIL.Image 显示了一张女人做下犬瑜伽姿势的图片。

```py
from PIL import Image
from dagshub.streaming import install_hooks
install_hooks()
Image.open("./Data/Yoga Pose/Downdog/00000011.jpg")
```

![简单快速的数据流处理，适用于机器学习项目](img/789836de3311bdae8f95c1da58639861.png)

我知道，这就像魔法一样。它也会让你惊讶于更快的加载时间。

> **注意：** 使用 DagsHubFilesystem 或 install_hooks 命令时，会要求你生成一个临时 OAuth 密钥以增加安全性。

# 使用 Direct Data Access 进行瑜伽姿势分类

在本教程中，我们将使用 Direct Data Access 对 Resnet34 模型进行微调，数据集为瑜伽姿势[数据集](https://dagshub.com/kingabzpro/Yoga-Pose-Classification)。该数据集包括 5 类瑜伽姿势图像，图像是通过 Bing API 提取的。

![简单快速的数据流处理，适用于机器学习项目](img/5ccfbe25f6d40cd47cc6a4c3dda2ea88.png)

作者提供的图片

## 设置

你可以 fork 并克隆我的[仓库](https://dagshub.com/kingabzpro/Yoga-Pose-Classification)，并在 Google Colab 或 Jupyter Notebook 上运行下面的命令。该命令需要一个分支名称、仓库名称、用户名和访问令牌。你可以简单地用你的配置替换占位符。

> **注意：** 请确保你在仓库内部以激活流媒体功能。

```py
!git clone -b {BRANCH} https://{DAGSHUB_USER_NAME}:{DAGSHUB_TOKEN}@dagshub.com/{DAGSHUB_USER_NAME}/{DAGSHUB_REPO_NAME}.git
%cd {DAGSHUB_REPO_NAME}
```

如果你在开始时遇到问题，可以查看[Colab 笔记本](https://colab.research.google.com/drive/1h-vikoiyOd5heR_-U76ScpJuov2vtzin?usp=sharing)。

接下来，我们将导入 Pytorch、FastAI、os 和 Matplotlib。该项目基于 FastAI 框架。阅读[文档](https://docs.fast.ai/)以了解更多信息。

```py
import torch
import matplotlib.pyplot as plt
from fastai.vision.all import *
from fastai.metrics import error_rate, accuracy
import os
```

## 数据加载器

在调用 install_hooks()之后，我们可以通过：`ImageDataloaders`自动访问文件。

加载和下载 18.1 MB 数据集花费了 6.54 秒。这相比使用`dvc pull` + ImageDataLoaders 的 73.84 秒要快。

```py
%%time
from dagshub.streaming import install_hooks
install_hooks()
path = Path('./Data/Yoga Pose')
data = ImageDataLoaders.from_folder(path, valid_pct=0.2, item_tfms=Resize(224))
```

```py
CPU times: user 1.68 s, sys: 914 ms, total: 2.59 s
Wall time: 6.54 s
```

## 可视化

我们的数据集非常简单，包括各种带标签的瑜伽姿势。我们将使用它来训练我们的模型。Resnet34 不需要大量数据，即使是几百张图像也能提供最先进的结果。

```py
data.show_batch()
```

![简单快速的数据流处理，适用于机器学习项目](img/46d4008e4fecddd9536514de05b294d1.png)

## 模型微调

在我们开始微调之前，我们需要设置 MLflow 以跟踪实验。DagsHub 提供了一个免费的 MLflow 服务器。你只需设置跟踪 URI，我们就可以开始了。你可以通过点击仓库页面上的绿色按钮（Remote）并通过 MLflow 选项卡访问 URI。

`get_experiment_id`函数生成并返回实验 ID。

```py
import mlflow
mlflow.set_tracking_uri(os.getenv("MLFLOW_TRACKING_URI"))

def get_experiment_id(name):
    exp = mlflow.get_experiment_by_name(name)
    if exp is None:
      exp_id = mlflow.create_experiment(name)
      return exp_id
    return exp.experiment_id

exp_id = get_experiment_id("yoga_colab")
```

之后，使用 mlflow.fastai.autolog()激活跟踪功能。它将记录实验并将其发送到 DagsHub 上的 MLflow 服务器。

最后，我们将用 resnet34 构建我们的学习器，并将指标设置为准确率和错误率。

```py
%%time
mlflow.fastai.autolog()
learn = vision_learner(data, resnet34, metrics=[accuracy, error_rate])
with mlflow.start_run(experiment_id=exp_id) as run:
         learn.fine_tune(3)
```

微调 3 个 epoch 后，我们获得了 95.9%的准确率和 0.04 的损失，取得了最先进的结果。

| **epoch** | **train_loss** | **valid_loss** | **accuracy** | **error_rate** | **time** |
| --- | --- | --- | --- | --- | --- |
| 0 | 0.368970 | 0.154318 | 0.954315 | 0.045685 | 00:05 |
| 1 | 0.220782 | 0.127155 | 0.964467 | 0.035533 | 00:05 |
| 2 | 0.145330 | 0.116135 | 0.959391 | 0.040609 | 00:05 |

```py
CPU times: user 12.4 s, sys: 4.55 s, total: 16.9 s
Wall time: 1min 14s
```

> **注意：** 如果你使用 Colab 训练模型，你需要设置标签以将提交与实验结果关联，使用：`mlflow.set_tag('mlflow.source.git.commit', "<commit-id>")`

保存模型并用于推断和可重复性。

```py
os.mkdir("/content/Yoga-Pose-Classification/Saved_Models/")
learn.save("/content/Yoga-Pose-Classification/Saved_Models/my_model", with_opt=False)
```

## 简单预测

让我们从训练集中获取一张随机图像来预测标签。正如你所见，我们以 99.99% 的确定性获得了 Downdog 标签。

```py
files = get_image_files("/content/Yoga-Pose-Classification/Data/Yoga Pose/Downdog")
learn.predict(files[0])
>>> ('Downdog',
 TensorBase(0),
 TensorBase([9.9999e-01, 8.2240e-07, 9.9132e-06, 5.6985e-07, 9.2867e-07]))
```

## 评估

模型评估显示模型准确地预测了所有图像。

```py
learn.show_results()
```

![简单快捷的数据流处理用于机器学习项目](img/84bedd1062e31a73f982431661497eba.png)

## MLflow

你可以使用 DagsHub 实验标签查看过去的实验和结果。我最初使用 Keras MobilenetV3 和其他各种模型进行训练。最后，我选择了 FastAI 和 Resnet34，以获得更简单和更好的结果。

![简单快捷的数据流处理用于机器学习项目](img/ffcba172d5d27efeb1b76a45ae94f463.png)

图片来自 [kingabzpro/Yoga-Pose-Classification](https://dagshub.com/kingabzpro/Yoga-Pose-Classification/experiments/#/)

# DagsHub 数据上传 API

流式传输是双向的。你也可以使用 DagsHub 上传 API 上传单个或多个文件。

为了在未见过的数据上测试我们的模型，让我们从 Google 下载瑜伽姿势图像。我们将上传这些图像，并通过流式传输访问它们进行预测。

![简单快捷的数据流处理用于机器学习项目](img/1115add7eec3555469eb1c571f93d32c.png)

图片来自 Google 搜索

我们将使用 `upload.Repo` 创建一个 repo 对象。这将帮助我们通过用户名、访问令牌、仓库名称和分支直接上传文件。

> **注意：** 上传时，你甚至不需要克隆任何 Git 仓库。

```py
from dagshub.upload import Repo

repo = Repo(
    owner=DAGSHUB_USER_NAME,
    name=DAGSHUB_REPO_NAME,
    branch=BRANCH,
    username=DAGSHUB_USER_NAME,
    password=DAGSHUB_TOKEN,
)
```

## 上传多个文件

要上传多个文件，我们需要提供一个文件夹目录。如果文件夹名称不存在，它会为你创建一个。

在下一步中，我们使用循环添加 2 个新的瑜伽姿势图像文件，并使用消息和版本控制提交。

通过使用版本控制，你可以上传由 Git 或 DVC 跟踪的文件。

```py
ds = repo.directory("Sample")

file_list = ["/content/yoga1.jpeg", "/content/yoga2.jpeg"]
for file1 in file_list:
    ds.add(file1)
ds.commit("multiple files upload", versioning="dvc")
```

文件已成功上传。太棒了!!!

这真是太棒了。

当我第一次上传文件而不克隆 Git 仓库并拉取 DVC 数据时，我非常高兴。

![简单快捷的数据流处理用于机器学习项目](img/e873359bb8cdb21bb587928894987b64.png)

## 上传单个文件

你也可以通过提供本地文件和仓库文件路径来使用单行上传文件。这样，你可以将文件上传到任何文件夹中。

在我们的例子中，我们使用消息和 DVC 版本控制将第三个文件上传到 Sample 文件夹。

```py
repo.upload(
    file="/content/yoga3.jpeg",
    path="Sample/yoga3.jpeg",
    commit_message="yoga single file upload",
    versioning="dvc",
)
```

第三张图像已成功上传到 DagsHub 仓库。

![简单快速的数据流处理用于机器学习项目](img/6271de110fa4911b3ed5f9a56a69d7ca.png)

## 模型预测

现在，我们将使用上传的图像来预测瑜伽姿势。

```py
%%time
install_hooks()
from PIL import Image
import IPython.display as display

img_loc = "./Sample/yoga3.jpeg"
img = Image.open(img_loc)
display.display(img)
learn.predict(img_loc)
```

该模型已以 99.9% 的准确度预测了 Warrior2 姿势。

![简单快速的数据流处理用于机器学习项目](img/5d4d3cf4ab050928f77e953971ab1795.png)

```py
CPU times: user 56.7 ms, sys: 2.14 ms, total: 58.8 ms
Wall time: 62.2 ms
('Warrior2',
 TensorBase(4),
 TensorBase([3.8390e-05, 5.2042e-04, 7.9451e-05, 5.5486e-06, 9.9936e-01]))
```

> **注意：** 要访问最近上传的文件，您必须再次运行 `install_hooks()`。

# 基准测试

尽管 DagsHub 的直接数据访问仍处于 Beta 阶段，但它显示出了良好的前景。通过流式处理，我们加载图像文件的速度比 `dvc pull` 快 **11 倍**。尽管 Beta 版本中仍有一些小的错误，但总体来说，我对 DagsHub 团队感到印象深刻，他们提出了一个令人惊叹的解决方案。

|  | **流式处理(s)** | **非流式处理(s)** |
| --- | --- | --- |
| **数据加载器** | 6.54 | 73.84 |
| **培训** | 74 | 72 |
| **总计** | **80.54** | **145.84** |

> 最后，我将请大家尝试一下 [直接数据访问](https://github.com/DagsHub/client/)，并在 [Discord](https://discord.com/invite/9gU36Y6) 支持频道中提问，如果遇到问题。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证数据科学专业人士，喜欢构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个人工智能产品，帮助那些患有心理疾病的学生。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

### 更多相关内容

+   [通过快速克里金法 (FKR) 加速机器学习](https://www.kdnuggets.com/2022/06/vmc-speed-machine-learning-fast-kriging.html)

+   [使用 Kafka 和 Risingwave 构建 Formula 1 流数据管道](https://www.kdnuggets.com/building-a-formula-1-streaming-data-pipeline-with-kafka-and-risingwave)

+   [快速有效的机器学习公平性审计方法](https://www.kdnuggets.com/2023/01/fast-effective-way-audit-ml-fairness.html)

+   [实用深度学习课程由 fast.ai 带回来了！](https://www.kdnuggets.com/2022/07/practical-deep-learning-fastai-2022.html)

+   [金融中的 Python：在 Jupyter Notebook 中实时数据流](https://www.kdnuggets.com/python-in-finance-real-time-data-streaming-within-jupyter-notebook)

+   [如何在 Snowflake 上构建流式半结构化分析平台](https://www.kdnuggets.com/2023/07/build-streaming-semistructured-analytics-platform-snowflake.html)
