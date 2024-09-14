# skops：提升Scikit-learn生产环境的全新库

> 原文：[https://www.kdnuggets.com/2023/02/skops-new-library-improve-scikitlearn-production.html](https://www.kdnuggets.com/2023/02/skops-new-library-improve-scikitlearn-production.html)

在生产环境中处理机器学习模型时面临各种挑战。这些挑战包括版本控制中的可重现性和安全序列化。在这篇博客文章中，我将带你了解一个名为`skops`的库，以应对这些挑战。

我们将看到一个端到端的示例：首先训练一个模型，然后序列化它，记录我们的模型，并托管它。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

```py
# let's import the libraries first
import sklearn
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from datasets import load_dataset

# Load the data and split
data = load_dataset("scikit-learn/breast-cancer-wisconsin")
df = data["train"].to_pandas()
y = df["diagnosis"]
X = df.drop("diagnosis", axis=1)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42
)
pipe = Pipeline(
    steps=[
				("imputer", SimpleImputer()),
        ("scaler", StandardScaler()),
        ("model", LogisticRegression())
    ]
)
pipe.fit(X_train, y_train)
```

# 安全序列化

现在我们将保存模型。我们可以使用任何格式保存模型，包括`joblib`、`pickle`或`skops`。

`skops`引入了一种新的序列化格式。其动机是避免使用pickle或`joblib`来序列化`sklearn`模型。使用`pickle`或`joblib`进行序列化可能导致恶意行为者在你的本地机器上执行代码，如果pickle文件来自你不信任的来源，应该避免反序列化。它是一种将代码指令序列化为二进制格式的序列化协议，因此不可读。它可以实际做任何事情：删除你机器上的所有内容或安装恶意软件。你应该仅从你信任的来源反序列化pickle。

`skops`引入的序列化格式不依赖于`pickle`，并允许用户在加载文件之前查看文件的内容。你可以在[这里](https://skops.readthedocs.io/en/stable/modules/classes.html#module-skops.io)阅读更多相关信息。我们来看一下API。

你可以通过传递对象和保存路径来保存一个`sklearn`模型或管道。

```py
import skops.io as sio
sio.dump(pipe, "pipeline.skops")
```

棘手的部分是从文件中加载模型。我们将把文件路径传递给`load`。我们还有一个参数叫做`trusted`，它可以是`True`、受信任类型的列表或`False`。如果设置为`False`，它将仅加载受信任的类型。让我们来看看。

```py
# passing `True`
sio.load("pipeline.skops", trusted=True)
# result
Pipeline(steps=[('imputer', SimpleImputer()), ('scaler', StandardScaler()),
                ('model', LogisticRegression())])
```

我们可以使用`get_untrusted_types`来获取未受信任的类型列表。

```py
unknown_types = sio.get_untrusted_types(file="pipeline.skops")
print(unknown_types)
# output
['numpy.int64']
```

你可以直接将上述列表传递给`trusted`。

```py
loaded_model = sio.load("pipeline.skops", trusted=unknown_types)
```

如果你尝试在没有变换器的情况下加载，加载将失败并抛出`UntrustedTypesFoundException`。

```py
loaded_model = sio.load("pipeline.skops", trusted=unknown_types[1:])
# output
UntrustedTypesFoundException: Untrusted types found in the file: ['numpy.int64'].
```

请注意，你总是需要将某些内容传递给`trusted`，因为这会提示用户确定是否信任该文件。

```py
loaded_model = sio.load("pipeline.skops")
# output
UntrustedTypesFoundException: Untrusted types found in the file: ['numpy.int64'].
```

# 模型托管

如果你希望将模型公开托管，可以使用 `skops` 和 [Hugging Face Hub](https://huggingface.co/)。这使得无需下载模型即可进行推理，模型文档在模型库中，并且只需一行代码即可构建接口。

![skops: 一个用于提升 scikit-learn 生产环境的新库](../Images/2ae00b9390b39d888be239bc775340c6.png)

Hugging Face 模型库

![skops: 一个用于提升 scikit-learn 生产环境的新库](../Images/d2eb37c88c6eaabc93dbcb60dbc5eb51.png)

这只需一行代码即可构建

让我们看看如何以编程方式创建这些内容。

`hub_utils.init` 创建一个包含模型的本地文件夹，并且配置文件包含模型训练环境的要求、训练目标、数据集样本等。传递给 `init` 的样本数据和任务标识符将帮助 Hugging Face Hub 启用模型页面上的推理小部件以及发现功能来找到模型。

> **注意：** 目前推理小部件、推理 API 和 `gradio` 集成仅支持 pickle 格式。我们目前正在开发对 `skops` 格式的支持。因此，我们将暂时以 `pickle` 格式保存模型。

```py
from skops import hub_utils
import pickle
# let's save the model
model_path = "example.pkl"
local_repo = "my-awesome-model"
with open(model_path, mode="bw") as f:
    pickle.dump(pipe, file=f)
# we will now initialize a local repository
hub_utils.init(
    model=model_path,
    requirements=[f"scikit-learn={sklearn.__version__}"],
    dst=local_repo,
    task="tabular-classification",
    data=X_test,
)
```

该库现在包含启用推理的模型和配置文件，构建环境以加载模型等。配置文件是一个 JSON 文件，其中包含：

+   数据集的小样本，

+   数据集的列，

+   加载模型的环境要求，

+   模型文件在库中的相对路径，

+   正在解决的任务。

现在，我们将通过创建模型卡来记录我们的模型。skops 中的模型卡遵循 Hugging Face Hub 模型卡的格式：包括一个 markdown 部分和一个 yaml 元数据部分。你可以在 [这里](https://huggingface.co/docs/hub/models-cards#model-card-metadata) 查看元数据部分的键，以便更好地发现模型。模型卡遵循的模板包括：

+   顶部的 YAML 部分用于元数据（任务 ID、许可证、用于训练的库名称等）

+   以 markdown 格式呈现的自由文本部分和需要填写的部分（例如模型描述、预期用途、限制等），

模型卡的以下部分由 skops 自动生成：

+   模型的超参数，

+   模型的交互式图示，

+   下面是一个展示如何加载和使用模型的小示例，

+   元数据、库名称、任务标识符（例如表格分类）以及推理小部件所需的信息已填写。

skops 通过各种方法支持程序化编辑模型卡。有关卡片模块的文档以及 skops 提供的默认模板，请参见 [这里](https://github.com/skops-dev/skops/blob/main/skops/card/default_template.md)。

你可以从skops中实例化Card类来创建模型卡。这个类是一个中间数据结构，稍后会渲染为markdown。我们将把这个卡片保存到托管模型的仓库中。在初始化仓库时，任务名称（例如，tabular-regression）和库名称（例如，scikit-learn）会写入配置文件。任务和库名称也需要在卡片的元数据中，所以你可以使用metadata_from_config方法从配置文件中提取元数据，并在创建卡片时将其传递给卡片。你可以使用`add`方法来添加信息和编辑元数据。

```py
from skops import card
from pathlib import Path

# create the card
model_card = card.Card(pipe, metadata=card.metadata_from_config(Path(local_repo)))
limitations = "This model is not ready to be used in production."
model_description = (
    "This is a LogisticRegression model trained on breast cancer dataset."
)
# add information to the model card
model_card.add(**{"Model description/Intended uses & limitations": limitations})
# set the license in the metadata
model_card.metadata.license = "mit"
```

我们可以评估模型并将其写入模型卡作为指标。我们可以使用`add_metrics`方法将指标添加到我们的模型卡中，并以表格形式写入。

```py
from sklearn.metrics import (ConfusionMatrixDisplay, confusion_matrix,
                            accuracy_score, f1_score)
# let's make a prediction and evaluate the model
y_pred = pipe.predict(X_test)
# we can pass metrics using add_metrics and pass details with add
model_card.add_metrics(accuracy=accuracy_score(y_test, y_pred))
model_card.add_metrics(**{"f1 score": f1_score(y_test, y_pred, average="micro")})
```

可以使用`add_plots`添加可视化模型性能的图表。

```py
import matplotlib.pyplot as plt
# we will create a confusion matrix
cm = confusion_matrix(y_test, y_pred, labels=pipe.classes_)
disp = ConfusionMatrixDisplay(confusion_matrix=cm, display_labels=pipe.classes_)
disp.plot()

# save the plot
plt.savefig(Path(local_repo) / "confusion_matrix.png")

# the plot will be written to the model card under the name confusion_matrix
# we pass the path of the plot itself
model_card.add_plot(**{
    "Confusion Matrix": "path-to-confusion-matrix.png"})
```

让我们将模型卡保存在本地仓库中。这里的文件名应该是`README.md`，因为这是Hugging Face Hub所期望的。

```py
model_card.save(Path(local_repo) / "README.md")
```

仓库现在已准备好推送到Hugging Face Hub。我们可以使用`hub_utils`来完成这项操作。Hugging Face Hub 遵循带有令牌的认证流程，因此我们可以在推送时传递我们的令牌。

```py
# set create_remote to True if the repository doesn't exist remotely on the Hugging Face Hub
hub_utils.push(
    repo_id="scikit-learn/blog-example",
    source=local_repo,
    commit_message="pushing files to the repo from the example!",
    create_remote=True,
)
```

一旦模型在 Hugging Face Hub 上，它可以被任何人使用`download`下载，除非该模型是私有的。仓库包含模型、模型卡和模型配置，模型配置包含数据集的小样本用于重现性、要求等。

```py
# pass repository ID that our model is hosted on the Hub, destination directory can be any path
hub_utils.download(repo_id="scikit-learn/blog-example", dst="downloaded-model") 
```

可以使用推理小部件轻松测试模型。

![skops: 一个用于改进生产环境中的 scikit-learn 的新库](../Images/a0fe949f7d05a401bf767dcc47b7618d.png)

仓库中的推理小部件

现在我们可以使用`gradio`集成来处理`skops`。我们只用一行代码就创建了下面的接口！ ????

![skops: 一个用于改进生产环境中的 scikit-learn 的新库](../Images/ea88ae3bf3960a27251583080be7d1a1.png)

我们模型的Gradio UI

```py
import gradio as gr
gr.Interface.load("huggingface/scikit-learn/skops-blog-example").launch()
```

我们可以进一步自定义这个UI，如下所示：

我们可以将标题、描述等信息传递给加载的UI。有关可以自定义的更多信息，请查看gradio文档中的[Interface class](https://gradio.app/docs/#interface)。

```py
import gradio as gr

gr.Interface.load("huggingface/scikit-learn/blog-example", 
title="Logistic Regression on Breast Cancer").launch()
```

结果仓库在[这里](https://huggingface.co/scikit-learn/blog-example)。

进一步资源

+   [skops 文档](https://skops.readthedocs.org/)

+   [scikit-learn 组织在 ???? Hub](https://huggingface.co/scikit-learn)

**[Merve Noyan](https://www.linkedin.com/in/merve-noyan-28b1a113a/)** 是一位谷歌机器学习开发专家，并且是Hugging Face的开发者推广专家。

### 更多相关主题

+   [将机器学习算法完整端到端部署到…](https://www.kdnuggets.com/2021/12/deployment-machine-learning-algorithm-live-production-environment.html)

+   [检测数据漂移以确保生产机器学习模型质量使用Eurybia](https://www.kdnuggets.com/2022/07/detecting-data-drift-ensuring-production-ml-model-quality-eurybia.html)

+   [为生产优先排序数据科学模型](https://www.kdnuggets.com/2022/04/prioritizing-data-science-models-production.html)

+   [从概念验证到生产的机器学习操作化](https://www.kdnuggets.com/2022/05/operationalizing-machine-learning-poc-production.html)

+   [2023年特征存储峰会：部署机器学习的实用策略…](https://www.kdnuggets.com/2023/09/hopsworks-feature-store-summit-2023-practical-strategies-deploying-ml-models-production-environments)

+   [生产机器学习的元数据存储！](https://www.kdnuggets.com/2022/05/layer-metadata-store-production-ml.html)
