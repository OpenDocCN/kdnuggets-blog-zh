# 部署您的第一个机器学习模型

> 原文：[https://www.kdnuggets.com/deploying-your-first-machine-learning-model](https://www.kdnuggets.com/deploying-your-first-machine-learning-model)

![部署您的第一个机器学习模型](../Images/30b7a469b92a895ba2fc5d6b23c675f2.png)

图片来源：[Lucas Fonseca](https://www.pexels.com/photo/man-in-front-of-monitor-2239655/)

# 介绍

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

在本教程中，我们将学习如何使用[玻璃分类](https://www.kaggle.com/datasets/uciml/glass)数据集构建一个简单的多分类模型。我们的目标是开发和部署一个能够预测各种类型玻璃的 Web 应用程序，例如：

1.  构建已处理浮动的窗户

1.  构建未处理浮动的窗户

1.  车辆窗户已处理浮动

1.  车辆窗户未处理浮动（数据集中缺失）

1.  容器

1.  餐具

1.  前照灯

此外，我们将了解：

+   **Skops**：分享基于 scikit-learn 的模型并投入生产。

+   **Gradio**：ML Web 应用程序框架。

+   **HuggingFace Spaces**：免费机器学习模型和应用程序托管平台。

到本教程结束时，您将获得构建、训练和部署基本机器学习模型作为 Web 应用程序的实际经验。

# 模型训练和保存

在本部分中，我们将导入数据集，将其拆分为训练和测试子集，构建机器学习管道，训练模型，评估模型性能并保存模型。

## 数据集

我们已加载数据集并对其进行了洗牌，以实现标签的均匀分布。

```py
import pandas as pd
glass_df = pd.read_csv("glass.csv")
glass_df = glass_df.sample(frac = 1)
glass_df.head(3)
```

我们的数据集

![部署您的第一个机器学习模型](../Images/48a488fe2a451ee7913f8bc95955424f.png)

之后，我们使用数据集选择了模型特征和目标变量，并将其拆分为训练集和测试集。

```py
from sklearn.model_selection import train_test_split

X = glass_df.drop("Type",axis=1)
y = glass_df.Type

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=125)
```

## 机器学习管道

我们的模型管道很简单。首先，我们通过一个填补器处理特征，然后使用标准缩放器对其进行归一化。最后，我们将处理后的数据输入随机森林分类器。

在将管道拟合到训练集后，我们使用 `.score()` 生成测试集上的准确度分数。

分数中等，我对性能感到满意。尽管我们可以通过集成或使用各种优化方法来改进模型，但我们的目标不同。

```py
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline

pipe = Pipeline(
    steps=[
        ("imputer", SimpleImputer()),
        ("scaler", StandardScaler()),
        ("model", RandomForestClassifier(n_estimators=100, random_state=125)),
    ]
)
pipe.fit(X_train, y_train)

pipe.score(X_test, y_test)
>>> 0.7538461538461538
```

分类报告也很好。

```py
from sklearn.metrics import classification_report

y_pred = pipe.predict(X_test)
print(classification_report(y_test,y_pred))
```

```py
precision    recall  f1-score   support

           1       0.65      0.73      0.69        15
           2       0.82      0.79      0.81        29
           3       0.40      0.50      0.44         4
           5       1.00      0.80      0.89         5
           6       1.00      0.67      0.80         3
           7       0.78      0.78      0.78         9

    accuracy                           0.75        65
   macro avg       0.77      0.71      0.73        65
weighted avg       0.77      0.75      0.76        65
```

## 保存模型

Skops 是一个很棒的库，用于将 scikit-learn 模型部署到产品中。我们将用它来保存模型，并在生产中加载。

```py
import skops.io as sio
sio.dump(pipe, "glass_pipeline.skops")
```

正如我们所见，通过一行代码，我们可以加载整个管道。

```py
sio.load("glass_pipeline.skops", trusted=True)
```

![部署你的第一个机器学习模型](../Images/9bd13a9b738b01b8dbfc59c5bd4e0e51.png)

# 构建 Web 应用

在这一部分，我们将学习如何使用 Gradio 构建一个简单的分类用户界面。

+   使用 skops 加载模型。

+   创建一个类名数组，将第一个留空或设为“None”，作为我们的数值类从 1 开始。

+   编写一个分类 Python 函数，该函数从用户那里获取输入，并使用管道预测类别。

+   使用滑块为每个特征创建输入。用户可以使用鼠标选择数值。

+   使用标签创建输出。它将以粗体文本显示在顶部。

+   添加应用的标题和描述。

+   最后，使用 `gradio.Interface` 组合所有内容

```py
import gradio as gr
import skops.io as sio

pipe = sio.load("glass_pipeline.skops", trusted=True)

classes = [
    "None",
    "Building Windows Float Processed",
    "Building Windows Non Float Processed",
    "Vehicle Windows Float Processed",
    "Vehicle Windows Non Float Processed",
    "Containers",
    "Tableware",
    "Headlamps",
]

def classifier(RI, Na, Mg, Al, Si, K, Ca, Ba, Fe):
    pred_glass = pipe.predict([[RI, Na, Mg, Al, Si, K, Ca, Ba, Fe]])[0]
    label = f"Predicted Glass label: **{classes[pred_glass]}**"
    return label

inputs = [
    gr.Slider(1.51, 1.54, step=0.01, label="Refractive Index"),
    gr.Slider(10, 17, step=1, label="Sodium"),
    gr.Slider(0, 4.5, step=0.5, label="Magnesium"),
    gr.Slider(0.3, 3.5, step=0.1, label="Aluminum"),
    gr.Slider(69.8, 75.4, step=0.1, label="Silicon"),
    gr.Slider(0, 6.2, step=0.1, label="Potassium"),
    gr.Slider(5.4, 16.19, step=0.1, label="Calcium"),
    gr.Slider(0, 3, step=0.1, label="Barium"),
    gr.Slider(0, 0.5, step=0.1, label="Iron"),
]
outputs = [gr.Label(num_top_classes=7)]

title = "Glass Classification"
description = "Enter the details to correctly identify glass type?"

gr.Interface(
    fn=classifier,
    inputs=inputs,
    outputs=outputs,
    title=title,
    description=description,
).launch()
```

# 部署机器学习模型

在最后一部分，我们将创建 Hugging Face 上的空间，并添加我们的模型和应用文件。

要创建空间，你需要登录 https://huggingface.co。然后，点击右上角的个人资料图片，选择“+ 新建空间”。

![部署你的第一个机器学习模型](../Images/ff769c581fbe825e4ffb1f12c0fd006b.png)

图片来自 HuggingFace

写下你的应用程序名称，选择 SDK，并点击创建空间按钮。

![部署你的第一个机器学习模型](../Images/8b2932f80b48a292ef1a576514707b78.png)

图片来自 Spaces

然后，创建一个 `requirements.txt` 文件。你可以通过前往“文件”选项卡并选择“+添加文件”按钮来添加或创建文件。

在 `requirements.txt` 文件中，你需要添加 skops 和 scikit-learn。

![部署你的第一个机器学习模型](../Images/3b05e80f445851d85eea57ff9e24ee90.png)

图片来自 Spaces

之后，通过将模型和文件从本地文件夹拖放到空间中来添加它们。然后，提交。

![部署你的第一个机器学习模型](../Images/5b32cd9226dbf2b8dfad02da6aefb072.png)

图片来自 Spaces

安装所需的包和构建容器需要几分钟时间。

![部署你的第一个机器学习模型](../Images/3fae1f801ce4efdcb7902dbda3b5da2c.png)

图片来自 Spaces

最后，你将获得一个无 bug 的应用程序，你可以与家人和同事分享。你还可以通过点击链接查看实时演示：[玻璃分类](https://huggingface.co/spaces/kingabzpro/glass-classification)。

![部署你的第一个机器学习模型](../Images/cc7b7d686ad60bdae764a2e874241c09.png)

图片来自 [玻璃分类](https://huggingface.co/spaces/kingabzpro/glass-classification)

# 结论

在本教程中，我们详细介绍了构建、训练和部署机器学习模型作为 Web 应用的完整过程。我们使用了玻璃分类数据集来训练一个简单的多类别分类模型。在使用 scikit-learn 训练模型后，我们利用 skops 和 Gradio 将模型打包并部署为 HuggingFace Spaces 上的 Web 应用。

在这个入门项目的基础上有很多可能性。你可以将更多功能整合到模型中，尝试不同的算法，或在其他平台上部署 Web 应用。重要的是，你现在已经获得了从头到尾的机器学习工作流程的实际经验。你已经接触了模型训练、将其打包以便于生产，并构建了用于与模型预测交互的 Web 界面。

感谢你的关注！如果在继续你的机器学习旅程中有任何其他问题，请告诉我。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan/)) 是一位认证的数据科学专家，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络为那些遭受心理疾病困扰的学生开发一个 AI 产品。

### 更多相关话题

+   [将你的机器学习模型部署到云端生产环境](https://www.kdnuggets.com/deploying-your-ml-model-to-production-in-the-cloud)

+   [构建你的第一个机器学习模型的逐步教程](https://www.kdnuggets.com/step-by-step-tutorial-to-building-your-first-machine-learning-model)

+   [机器学习模型部署：逐步教程](https://www.kdnuggets.com/deploying-machine-learning-models-a-step-by-step-tutorial)

+   [从零到英雄：使用 PyTorch 创建你的第一个 ML 模型](https://www.kdnuggets.com/from-zero-to-hero-create-your-first-ml-model-with-pytorch)

+   [在 Heroku 云上部署深度学习 Web 应用的技巧与窍门](https://www.kdnuggets.com/2021/12/tips-tricks-deploying-dl-webapps-heroku.html)

+   [使用 DAGsHub 将 Streamlit Web 应用部署到 Heroku](https://www.kdnuggets.com/2022/02/deploying-streamlit-webapp-heroku-dagshub.html)
