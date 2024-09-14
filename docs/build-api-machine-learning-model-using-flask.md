# 如何在5分钟内使用Flask为机器学习模型构建API

> 原文：[https://www.kdnuggets.com/2019/01/build-api-machine-learning-model-using-flask.html](https://www.kdnuggets.com/2019/01/build-api-machine-learning-model-using-flask.html)

[评论](#comments)

**作者 [Tim Elfrink](https://medium.com/@tim.elfrink.94)，Vantage AI的数据科学家**

作为一名数据科学家顾问，我希望通过我的机器学习模型产生影响。然而，这说起来容易做起来难。在开始一个新项目时，首先需要在Jupyter Notebook中对数据进行探索。一旦你对所处理的数据有了全面的理解，并与客户达成了下一步的计划，那么一个可能的结果是创建一个预测模型。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的IT需求

* * *

你会感到兴奋，然后回到你的笔记本中尽可能创建最好的模型。模型和结果呈现出来，大家都很满意。客户希望在他们的基础设施中运行模型，以测试是否能真正产生预期的影响。此外，当人们能够使用模型时，你会得到必要的反馈，以便逐步改进它。但鉴于客户有一些你可能不熟悉的复杂基础设施，我们该如何迅速做到这一点呢？

为此，你需要一个能够适应他们复杂基础设施的工具，最好是你熟悉的语言。这就是你可以使用 [Flask](http://flask.pocoo.org/) 的地方。Flask是一个用Python编写的微型Web框架。它可以创建一个REST API，允许你发送数据，并接收预测作为响应。

![](../Images/5ef05360393f1309dd9a4fc4a6af5797.png)

### **创建你的模型**

让我向你展示 [这是如何工作的](https://github.com/timelfrink/flask-api)。为了演示的目的，我将使用一个示例数据集来训练一个简单的DecisionTreeClassifier模型，该数据集可以从 [scikit-learn包](https://scikit-learn.org/stable/modules/generated/sklearn.datasets.load_wine.html#sklearn.datasets.load_wine) 中加载。

我如何在Jupyter Notebook中创建我的模型

一旦客户对你创建的模型满意，你可以将其保存为 [pickle](https://docs.python.org/3.7/library/pickle.html) 文件。然后你可以在之后打开这个pickle文件，并调用函数 `predict` 来对新的输入数据进行预测。这正是我们将在Flask中做的事情。

### **运行 Flask**

Flask 运行在服务器上。根据客户端的要求，这可以是客户端环境或不同的服务器。当运行`python app.py`时，它首先加载创建的 pickle 文件。一旦加载完成，你就可以开始进行预测。

所有你需要的简单 API 代码都在 Flask 中！

### **请求预测**

通过将 POST JSON 请求传递给创建的 Flask 网络服务器（默认在 5000 端口）来进行预测。在`app.py`中，这个请求被接收，并根据我们模型中已加载的预测函数做出预测。它以 JSON 格式返回预测结果。

测试我们的 API

现在，你只需使用正确的数据点语法调用网络服务器。这与原始数据集的格式相对应，以获取预测的 JSON 响应。例如：

`python request.py -> <Response[200]> “[1.]"`

对于我们发送的数据，我们得到了模型的类 1 预测结果。实际上，你所做的只是将数据以数组的形式发送到一个端点，该端点将其转换为 JSON 格式。端点读取 JSON POST 并将其转换回原始数组。

通过这些简单的步骤，你可以轻松让其他人使用你的机器学习模型，并迅速产生重大影响。

### **注意**

在这篇文章中，我没有考虑数据中的任何错误或其他异常。本文展示了如何简单地启动并从模型输出中学习，但在准备投入生产之前还需要进行大量改进。当创建一个包含 API 的 Docker 文件并将其托管到 Kubernetes 上时，这个解决方案可以变得可扩展，从而可以在不同的机器之间平衡负载。但这些都是从概念验证到生产环境的过程中需要采取的步骤。

感谢 [Ruurtjan Pul](https://medium.com/@ruurtjan?source=post_page) 和 [Jasper Makkinje](https://medium.com/@jasper.makkinje?source=post_page)。

**简介：[Tim Elfrink](https://medium.com/@tim.elfrink.94)** 是 **Vantage AI** 的数据科学家，这是一家位于荷兰的数据科学咨询公司。如果你需要帮助创建适用于你的数据的机器学习模型，请随时通过 [**info@vantage-ai.com**](mailto:info@vantage-ai.com) 联系我们。

[原文](https://medium.com/vantageai/how-to-build-an-api-for-a-machine-learning-model-in-5-minutes-using-flask-eb72d8cb4504)。已获得许可转发。

**相关：**

+   [为深度学习应用程序连接点](/2017/08/connecting-dots-deep-learning-app.html)

+   [使用 TensorFlow 和 Flask RESTful Python API 构建 ConvNet HTTP 基础应用程序的完整指南](/2018/05/complete-guide-convnet-tensorflow-flask-restful-python-api.html)

+   [更多 Google Colab 环境管理技巧](/2019/01/more-google-colab-environment-management-tips.html)

### 更多相关主题

+   [停止学习数据科学以寻找目标，并寻找目标来……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个90亿美元的人工智能失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的五个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让Python成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该知道的三个R库（即使你使用Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
