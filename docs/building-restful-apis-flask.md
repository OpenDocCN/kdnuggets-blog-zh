# 使用 Flask 构建 RESTful API

> 原文：[https://www.kdnuggets.com/2021/05/building-restful-apis-flask.html](https://www.kdnuggets.com/2021/05/building-restful-apis-flask.html)

[评论](#comments)

**由 [Mahadev Easwar](https://www.linkedin.com/in/mahadeveaswar/), 数据工程师**

![](../Images/c9152a361ebd3ee36215cd3edc7bfdaf.png)

[Flask](https://github.com/pallets/flask/blob/master/artwork/logo-full.svg) [框架](https://www.pexels.com/)

**什么是 API？**

> 应用程序编程接口（API）是一个软件中介，允许两个应用程序进行通信。

**什么是 web 框架，它为什么有用？**

web 框架是一个软件框架，用于支持动态网站、web 服务和 web 应用程序的开发。web 框架是一个代码库，通过提供构建可靠、可扩展和可维护的 web 应用程序的基本模式，使 web 开发变得更快、更容易 ¹。

**Flask：简介**

*Flask* 是一个轻量级的 WSGI web 应用框架。它旨在快速和轻松地入门，并具有扩展到复杂应用的能力。它最初是 *Werkzeug* 和 *Jinja* 的一个简单封装，现已成为最受欢迎的 Python web 应用框架之一。

![](../Images/f33969b8eb5cd88b36c5e88784601d16.png)

上个月的包下载数量。[来源](https://pypistats.org/). ²

**为什么称之为微框架？**

> “微”并不意味着你的整个 web 应用程序必须适合到一个 Python 文件中（尽管它当然可以），也不意味着 Flask 缺乏功能。微框架中的“微”意味着 Flask 旨在保持核心简单但可扩展 ³。

Flask 提供建议，但不强制任何依赖项或项目布局。开发者可以选择他们想要使用的工具和库。社区提供了许多扩展，使添加新功能变得容易 ⁴。

**为什么使用 Flask？**

+   易于设置

+   极简主义

+   灵活地添加任何扩展以获得更多功能

+   活跃的社区

在本文中，我将演示如何设置、构建和运行一个简单的 Flask 应用。

**包安装**

通常建议在特定于项目的虚拟环境中安装包。

```py
# command prompt
**pip** install Flask
```

![](../Images/bc15a7c0d53eb3296ba251ca818d7da8.png)

安装 Flask 包

**JSON**

JSON 是一种通用数据格式，具有最少数量的值类型：字符串、数字、布尔值、列表、对象和空值。尽管表示法是 JavaScript 的一个子集，但这些类型在所有常见编程语言中都有表示，使 JSON 成为跨语言传输数据的良好候选者 ⁵。

```py
# JSON Example:
{
"id" : 1,
"name" : "john"
"age" : "25",
"activities" : ["cricket","football"],
"class" : [{"year":"2020","dept":"CSE"}]
}
```

**构建 Flask 应用**

我将使用 Flask 构建一个简单的应用程序，该应用程序将访问员工数据并返回输入中请求的信息。我们将使用 *JSON* 文件格式来发送和接收数据。由于我将提供基于数据的演示，我还加载了 *pandas* 包。

1\. 导入所需的包

```py
**from** flask **import** Flask, request, jsonify 
**import** pandas as pd
```

项目的目录结构如下

```py
Demo/
| — emp_info.csv
| — emp_status.csv
| — app.py
```

2\. 创建应用程序实例

```py
app = Flask(__name__)
```

3\. 使用`.route()`声明端点及其接受的方法，如`POST`、`GET`。默认情况下，它仅监听`GET`方法。让我们仅为此API启用`POST`方法。

```py
@app.route(**"/get_emp_info"**, methods = [**'POST'**])
```

4\. 定义应用程序将执行的功能。根据输入数据从CSV文件中检索员工数据。

```py
@app.route(**"/get_emp_info"**, methods = [**'POST'**])
**def** get_employee_record():
    input_data = json.loads(request.get_json())
    ids = input_data[**'**emp_ids**'**]
    status = input_data[**'**status**'**]
    emp_info = pd.read_csv(**'**emp_info.csv**'**)
    emp_status = pd.read_csv(**'**emp_status.csv**'**)
    emp_status = emp_status[(emp_status[**'**emp_no**'**].isin(ids)) &     (emp_status[**'**status**'**].isin(status))]
    emp_info = emp_info[emp_info[**'**emp_no**'**].isin(emp_status[**'**emp_no**'**])]
    emp_info = pd.merge(emp_info,emp_status,on=**'**emp_no**'**,how=**'**left**'**)
    out_data = emp_info.to_dict(orient=**'**records**'**)
    **return** jsonify(out_data)
```

**jsonify**()* 是Flask提供的一个辅助方法，用于正确返回*JSON*数据。它返回一个设置了application/json mimetype的Response对象。

5\. 将Python应用程序设置为在本地开发服务器上运行。默认情况下，调试模式为*False*。要在代码修改时重启服务，调试模式可以设置为*True*。

```py
**#** Setting port number and host i.e., localhost by default **if __name__** == **"__main__"**:
    app.run(**host**=**'**0.0.0.0**'**, **port**=6123)
```

Python Flask应用程序

6\. 在本地服务器上运行Python程序

```py
# command prompt
**python** app.py
```

![](../Images/bfb2786573aa6414d5b1f0406ad5dee3.png)

运行Python应用程序

**测试应用程序**

使用*requests*包向开发的应用程序发送`POST`请求。

```py
# command prompt - installing requests package
$ **pip** install requests# Python code to test **import** requests, json
# sample request
data = {"emp_ids":["1001","1002","1003"],"status":["active"]}
# Hitting the API with a POST request
ot = requests.post(url=**'http://localhost:6123/get_emp_info'**, json=json.dumps(data))
print(ot.json())# Response JSON 
[{
 'cmp': 'ABC',
 'emp_no': 1001,
 'name': 'Ben',
 'salary': 100000,
 'status': 'active'
}, {
 'cmp': 'MNC',
 'emp_no': 1002,
 'name': 'Jon',
 'salary': 200000,
 'status': 'active'
}]
```

**总结**

本文中涉及的概念：

+   API与Web框架

+   Flask简介

+   设置Flask环境

+   构建一个Flask API

+   使用*requests*包测试Flask API的请求

Flask就像是构建RESTful APIs的极简方法。

> 简单常常能产生奇妙的效果。— 阿梅利亚·巴尔

**结束语**

感谢所有走到这里的人。希望你们发现这篇文章有帮助。请在评论中分享你的反馈/疑问。现在，是时候从头开始构建自己的API了。祝你好运！

*如果你觉得这篇文章有趣，并且对数据科学、数据工程或软件工程充满热情，请点击*[*关注*](https://medium.com/@mahadeveaswar)*，并随时在*[*LinkedIn*](https://www.linkedin.com/in/mahadeveaswar/)*上加我。*

**简介: [Mahadev Easwar](https://www.linkedin.com/in/mahadeveaswar/)** 是一名数据工程师，具备Python、R和SQL方面的显著技能。

[原文](https://pub.towardsai.net/building-restful-apis-using-flask-8ba2716d361f)。已获得许可转载。

**相关：**

+   [如何将机器学习/深度学习模型部署到网络]( /2021/04/deploy-machine-learning-models-to-web.html)

+   [将Docker化的FastAPI应用程序部署到Google Cloud Platform](/2021/05/deploy-dockerized-fastapi-app-google-cloud-platform.html)

+   [如何在Kubernetes中部署Flask API并与其他微服务连接](/2021/02/deploy-flask-api-kubernetes-connect-micro-services.html)

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织 IT

* * *

### 更多相关内容

+   [OpenAI 的新 ChatGPT 和 Whisper API](https://www.kdnuggets.com/2023/03/new-chatgpt-whisper-apis-openai.html)

+   [FastAPI 教程：几分钟内用 Python 构建 API](https://www.kdnuggets.com/fastapi-tutorial-build-apis-with-python-in-minutes)

+   [使用 Llama 和 ChatGPT 构建多聊天后端的微服务](https://www.kdnuggets.com/building-microservice-for-multichat-backends-using-llama-and-chatgpt)

+   [构建 GPU 机器与使用 GPU 云服务](https://www.kdnuggets.com/building-a-gpu-machine-vs-using-the-gpu-cloud)

+   [加速您的 AI 旅程！加入 Uplimit 的免费 AI 课程…](https://www.kdnuggets.com/2024/01/uplimit-supercharge-your-ai-journey-openai-course)

+   [使用 Pandas 构建数据科学管道](https://www.kdnuggets.com/building-data-science-pipelines-using-pandas)
