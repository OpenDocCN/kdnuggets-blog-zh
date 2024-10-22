# 使用 Heroku 部署机器学习 Web 应用程序

> 原文：[`www.kdnuggets.com/2022/04/deploy-machine-learning-web-app-heroku.html`](https://www.kdnuggets.com/2022/04/deploy-machine-learning-web-app-heroku.html)

在之前的 博客文章 中，我展示了如何使用 Streamlit 库在 Python 中构建机器学习 Web 应用程序。最终产品看起来是这样的：

![使用 Heroku 部署机器学习 Web 应用程序](img/9980a38ef36f656c74bf0006897804b9.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

这是一个允许用户输入有关健康和生活方式信息的应用程序，并返回一个输出，预测一个人在 10 年内患心脏病的可能性。

如果你想了解更多关于模型以及应用程序如何构建的信息，可以通过这个 教程 来学习。

否则，你可以简单地访问这个 [Github 仓库](https://github.com/Natassha/fhs_model) 并克隆它。它包含了构建和部署 Web 应用程序所需的所有代码文件。

# 步骤 0：前置条件

要运行 Streamlit web 应用程序并将其部署到 Heroku，你需要安装一个非 GUI 的 Python 代码编辑器（Jupyter Notebook 不够用）。我目前使用的是 [Visual Studio Code](https://code.visualstudio.com/download)，但 [PyCharm](https://www.jetbrains.com/help/pycharm/installation-guide.html) 和 [Atom](https://atom.io/) 也是不错的替代选择。

完成后，确保使用‘pip’命令安装这三个库—— [Streamlit](https://docs.streamlit.io/library/get-started/installation)、[Joblib](https://joblib.readthedocs.io/en/latest/installing.html) 和 [Pandas](https://pypi.org/project/pandas/)。

# 第一步：在本地运行应用程序

现在环境已准备好，尝试在本地运行应用程序以检查一切是否正常。

打开你的终端并输入以下代码行：

```py
streamlit run streamlit_fhs.py
```

然后，打开浏览器并访问 [`localhost:8501`](http://localhost:8501/)。你应该会看到一个类似这样的 Web 应用程序：

![使用 Heroku 部署机器学习 Web 应用程序](img/9980a38ef36f656c74bf0006897804b9.png)

# 第两步：创建必要的文件

我们的文件夹目前有三个文件：

![使用 Heroku 部署机器学习 Web 应用程序](img/cf1d05881deb0e5ee4fa682535af5c1f.png)

为了成功将应用部署到 Heroku，我们需要创建另外三个文件：

**1. requirements.txt**

首先，创建一个文本文件并命名为 *requirements.txt*。然后，将以下内容粘贴到文件中：

```py
pandas==1.3.2
    gunicorn==19.9.0
    streamlit==1.5.1
    joblib==1.1.0
    sklearn==0.22
```

**2. Procfile**

接下来，你需要创建一个 Procfile。它告诉 Heroku 你的应用程序位置以及如何启动它。

要创建 Procfile，只需打开终端并导航到你刚刚克隆的文件夹。输入以下命令：

```py
echo web: gunicorn app:app >Procfile
```

**3. setup.sh**

最后，创建一个名为 *setup.sh* 的文件，内容如下：

```py
mkdir -p ~/.streamlit/ 
    echo "\ [server]\n\
    headless = true\n\
    port = $PORT\n\
    enableCORS = false\n\
    \n\" > ~/.streamlit/config.toml
```

# 第三步：设置 Heroku

现在，你需要 [创建一个免费的 Heroku 账户](https://signup.heroku.com/)。

完成后，[下载 Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) 并运行可执行文件。

为了确保你已成功安装 Heroku，在终端中输入以下命令：

```py
heroku --version
```

如果你收到“命令未找到”的错误，这意味着某些东西出错了，Heroku 在你的设备上未正确设置。否则，你的安装成功了，你可以进入下一步。

# 第四步：部署 Web 应用

再次打开终端并导航到你的应用程序所在的目录。

输入以下命令：

```py
heroku create my_app
```

*注意：你可以将“my_app”替换为你选择的任何名称。*

然后，你需要初始化并将代码推送到 Git。为此，在终端中输入以下命令：

```py
git init
    git add .
    git commit -m "first commit"
```

最后，运行这两个命令将你的代码部署到 Heroku：

```py
heroku git:remote -a my_app
    git push heroku master
```

*注意：再次记得将 my_app 改为你的应用名称。*

完成后，你会在终端上看到类似这样的输出：

![使用 Heroku 部署机器学习 Web 应用](img/a0c2ab86e69b58f304fb2a8f6579f256.png)

上面显示的链接是你的应用所在的位置，你现在可以在线访问它。以下是我的 Web 应用的 URL：[`fhs-pred-app.herokuapp.com/`](https://fhs-pred-app.herokuapp.com/)。

就这样！如果你正确执行了所有步骤，你现在已经部署了一个功能齐全的 Web 应用，你可以通过一个链接与其他人分享。

**[Natassha Selvaraj](https://www.natasshaselvaraj.com/)** 是一位自学成才的数据科学家，热衷于写作。你可以在 [LinkedIn](https://www.linkedin.com/in/natassha-selvaraj-33430717a/) 上与她联系。

### 了解更多相关内容

+   [在 Heroku 云上部署深度学习 Web 应用的提示与技巧](https://www.kdnuggets.com/2021/12/tips-tricks-deploying-dl-webapps-heroku.html)

+   [使用 DAGsHub 将 Streamlit Web 应用部署到 Heroku](https://www.kdnuggets.com/2022/02/deploying-streamlit-webapp-heroku-dagshub.html)

+   [在 5 分钟内构建一个机器学习 Web 应用](https://www.kdnuggets.com/2022/03/build-machine-learning-web-app-5-minutes.html)

+   [KDnuggets 新闻 2022 年 3 月 9 日：在 5 分钟内构建一个机器学习 Web 应用](https://www.kdnuggets.com/2022/n10.html)

+   [调查：机器学习项目仍然经常无法部署](https://www.kdnuggets.com/survey-machine-learning-projects-still-routinely-fail-to-deploy)

+   [如何成功部署数据科学项目](https://www.kdnuggets.com/2022/01/successfully-deploy-data-science-projects.html)
