# 使用 R 在 Cloud Run 上进行无服务器机器学习

> 原文：[https://www.kdnuggets.com/2020/02/serverless-machine-learning-r-cloud-run.html](https://www.kdnuggets.com/2020/02/serverless-machine-learning-r-cloud-run.html)

[评论](#comments)

**作者 [Timothy Lin](https://www.linkedin.com/in/timothy-lin-0600ba141/)，数据科学家，计量经济学家**

每位数据科学家面临的主要挑战之一是模型部署。除非你是那些幸运的少数人之一，拥有大量数据工程师来帮助你部署模型，否则在企业项目中这确实是一个问题。我甚至没有暗示模型需要准备好生产使用，但即使是将模型和见解提供给业务用户这样一个看似简单的问题，也比实际需要的要麻烦得多。过去解决这个问题主要有两种方法：

1.  每次请求的临时手动运行

1.  将代码托管在服务器上并编写 API 接口以使结果可用

这两种方法代表了一个光谱的两端。临时运行太过繁琐，客户通常会要求某种自助服务界面，但要获得一个永久服务器来托管你的代码则要运气好。事实证明还有一种第三种方法——无服务器方法！AWS Lambdas 和 Google Cloud Platform Cloud Functions 确实开辟了一种新的结果服务方式，而无需管理任何基础设施。如果你可以访问云，这是一个非常有吸引力的选项。

如果你来自 JavaScript 或 Python 领域，你可能已经使用过这些工具。云函数的问题在于它限制于特定环境。然而，随着 GCP 的 [Cloud Run](https://cloud.google.com/run/docs/reference/container-contract) 的引入，我们不再受此问题的限制。Cloud Run 允许在云上提供自定义 Docker 镜像，从而为无服务器领域开辟了许多有趣的可能性。这意味着 R 用户也终于有了开发和部署无服务器机器学习模型的方式！????

这篇文章记录了我在使用 Cloud Run 时的一些经验，希望能作为一个好的参考模板，供任何可能想尝试的人使用。我从两个其他来源中获得了灵感，即 [Mark 的博客](https://code.markedmondson.me/googleCloudRunner-intro/) 和 [Eric 的文章](https://ericjinks.com/blog/2019/serverless-R-cloud-run/)。Mark 实际上在 CRAN 上有一个自动化一些部署工作的包，Eric 还介绍了持续集成管道的一些内容，所以一定要去看看。

这篇文章更像是一个实用指南，包含一个分析师可能开发并希望部署的真实世界机器学习应用，这使得应用变得更加具体和有用。这也是一个非常有趣的小副项目。

如果你是一个编程人员，你可以跳过剩下的文章，直接查看 [github 代码](https://github.com/timlrx/serverless-ml)，否则继续阅读 :)

### 我的项目会在 Cloud Run 上运行吗？

Cloud run 并不是万能的。查看 [需求规格说明](https://cloud.google.com/run/docs/reference/container-contract) 和 [限制](https://cloud.google.com/run/quotas) 以获取更多详细信息，但这里有一些你应该注意的限制。

+   状态不会被持久化（如果你想持久化它们，你需要一个外部数据库）

+   最大内存限制为 2GB

+   容器必须在收到请求后的 4 分钟内启动服务器，并在 15 分钟后超时

这使其非常适合短时间计算，但不适合需要大量内存或非常 CPU 密集的任务。15 分钟实际上还算不错！你可能可以运行一些回归分析、决策树，甚至解决线性规划问题，但可能无法训练神经网络。

### Twitter 项目 ????

![](../Images/42efd06b01197b8373875beb2914f7ee.png)

这是我有趣的 serverless-ml 周末项目：一个分析 Twitter 领域的应用程序。我想生成两个图表：一个比较推文频率随时间变化的图表，以及另一个对推文进行情感分析的图表。作为额外奖励，我决定使其具有交互性——这意味着既提供静态图表，也提供交互式 plotly 结果。

我使用的主要包有：rtweet, dplyr, ggplot2, tidytext, tidyr 和 stringr。如果你对 tidytext 不熟悉，可以查看我之前的一些帖子，例如2017年的这篇，分析了[食谱书](https://www.timlrx.com/2017/06/24/thesis-thursday-4-analysing-recipes/)。

rtweet 提供了一个方便的 API 来收集用户时间线信息。你需要一个 Twitter API 账户才能开始使用。这是一个简单的过程，你可以在这里注册： [https://developer.twitter.com/en/apply-for-access](https://developer.twitter.com/en/apply-for-access)。

注意这四个密钥/令牌。它们应该作为环境变量保存在你的系统中。²这四个密钥对应于 tweet.R 文件中的 API_KEY、API_SECRET_KEY、ACCESS_TOKEN 和 ACCESS_SECRET，并将在需要时以编程方式检索。

![](../Images/66daf56784262c7a5fe71ed865665954.png)

### [Tweet.R](https://www.timlrx.com/2020/01/22/serverless-machine-learning-with-r-on-cloud-run/(https://github.com/timlrx/serverless-ml/blob/master/twitter-r/tweet.R))

不会深入探讨数据科学代码，但你可以在 [这里](https://github.com/timlrx/serverless-ml/blob/master/twitter-r/tweet.R)查看。重要的是，我们将每一部分封装成函数，然后在主 API 路由文件 (app.R) 中调用这些函数。

对于情感分析，我们通过字典匹配来统计正面和负面词汇的数量。³ 词汇字典来自于[Bing Liu et al.](https://www.cs.uic.edu/~liub/FBS/sentiment-analysis.html)。这是否需要外部数据库？实际上不需要 - 外部依赖项或数据文件是可以的，只要它们是无状态的。我们可以将其与我们的docker文件一起打包，或者在这种情况下，它随tidytext包一起安装！

### [App.R](https://github.com/timlrx/serverless-ml/blob/master/twitter-r/app.R)

这个文件包含了服务逻辑。我们使用[plumber package](https://www.rplumber.io/)，它允许我们通过一些标记在R中创建REST API。你可以通过使用`#* @param`标记来指定查询参数，并指定返回的输出类型，如`#* @png`用于静态图像，或`#* @html`用于html。

作为额外的好处，还有一个开箱即用的选项来使htmlwidgets工作（`#* @serializer htmlwidget`）。这使得我们的plotly结果展示变得非常简单。我决定为每个图创建两个路径，一个静态的和一个plotly交互式的。所以总共有四个路径：

+   /frequency (ggplot)

+   /html/frequency (plotly)

+   /sentiment (ggplot)

+   /html/sentiment (plotly)

这些函数都接受两个参数：`n` - 推文数量，以及`users`，这可以是一个用逗号分隔的用户ID列表，我们将通过rtweet查询Twitter API获取相关信息。

### [Server.R](https://github.com/timlrx/serverless-ml/blob/master/twitter-r/server.R)

这段代码启动了我们的plumber服务器。我们通过环境变量（PORT）推断端口。

这就是R代码的全部内容。现在你应该有一个可以在本地计算机上运行的应用程序了。接下来，我们将深入探讨ML-ops的复杂细节 ????‍♀。这涉及到使用Docker打包我们的依赖项，并将其部署到云端。

### Docker

Docker是一个将不同的软件、配置和环境打包成容器的平台，这些容器整洁地封装了你的应用程序。最终用户只需要列出安装步骤以构建镜像，然后可以在docker平台上运行????。

为了启动配置，我们在官方的[r-base image](https://hub.docker.com/_/r-base)上进行构建。这是一个Linux镜像，我们需要安装一些额外的依赖项以使应用程序正常工作，即rtweet的libssl-dev和处理htmlwidgets的pandoc。这是我们的[Dockerfile](https://github.com/timlrx/serverless-ml/blob/master/twitter-r/Dockerfile)的开始：

```py
FROM r-base
RUN apt-get update -qq && apt-get install -y \
  git-core \
  libssl-dev \
  libcurl4-gnutls-dev \
  pandoc
```

接下来，我们将目录中的脚本复制到容器中的应用程序目录，并使用Rscript函数安装必要的R库：

```py
WORKDIR /usr/src/app

# Copy local code to the container image.
COPY . .

# Install any R packages
RUN Rscript -e "install.packages('plumber')"
RUN Rscript -e "install.packages(c('rtweet', 'dplyr', 'ggplot2', 'plotly', 'tidytext', 'tidyr', 'stringr'))"
```

我们暴露端口8000（这主要用于文档），并在容器启动时运行服务器：

```py
EXPOSE 8000

# Run the web service on container startup.
CMD [ "Rscript", "server.R"]
```

让我们构建docker镜像并运行它：

```py
docker build -t ${IMAGE} .
docker run -p 8000:8000 -e PORT=8000 -e API_KEY -e API_SECRET_KEY -e ACCESS_TOKEN -e ACCESS_SECRET ${IMAGE}
```

`${IMAGE}`这里代表你可以分配给镜像的名称。还记得我们需要的环境变量吗？Plumber 的端口和访问推特 API 的 API 密钥？我们在运行容器时将其传递给容器。注意：构建过程相当长，镜像大小为 1.22GB。⁴

如果图像成功运行，你应该能够在浏览器上访问这些路由。现在，我们可以将它从本地机器带到网络上！

### Cloud Run

你可以使用我的[deploy.sh](https://github.com/timlrx/serverless-ml/blob/master/twitter-r/deploy.sh)脚本作为构建和部署镜像的指南。在这样做之前，我建议你将 Cloud Run Admin 角色分配给运行脚本的账户或用户，以确保其正确部署。你可以通过 GCP 内的[IAM 面板](https://console.cloud.google.com/iam-admin)完成。

在脚本中，我程序化地检索了项目 ID，但可以用你的 GCP 项目替换它。脚本的作用是将本地 docker 镜像上传到 Google 的容器仓库，并运行 cloud run 从仓库中部署镜像：

```py
gcloud alpha run deploy \
    --image="gcr.io/${PROJECT_ID}/${IMAGE}:1.0.0" \
    --region="us-central1" \
    --platform managed \
    --memory=512Mi \
    --port=8000 \
    --set-env-vars API_KEY=${API_KEY},API_SECRET_KEY=${API_SECRET_KEY},ACCESS_TOKEN=${ACCESS_TOKEN},ACCESS_SECRET=${ACCESS_SECRET} \
    --allow-unauthenticated
```

我们添加了`allow-unauthenticated`来允许公共流量，并增加了内存，因为默认内存太低。你可以先从默认的256MB开始，但如果遇到任何错误，请检查 cloud run 日志，这对调试任何错误非常有用。如果一切顺利，你应该会看到这样的图像：

![](../Images/b85f4c428b7b6a3c3679eb5f06c07c18.png)

我们的推特项目现在成功托管在 Cloud Run 上！

### 测试一下

有趣的部分来了 - 尝试一下，实时可视化和分析推特数据吧！

你可以尝试我的托管 cloud run 服务，使用上面列出的4个端点之一：[https://twitter-r-cvdvxo3vga-uc.a.run.app/](https://twitter-r-cvdvxo3vga-uc.a.run.app/)

一些有趣的例子！

### 奥巴马推文的频率

在推特上拥有1.12亿粉丝的最受关注人物实际上并不常常发推 ????：

[https://twitter-r-cvdvxo3vga-uc.a.run.app/frequency?n=500&users=BarackObama](https://twitter-r-cvdvxo3vga-uc.a.run.app/frequency?n=500&users=BarackObama)

![](../Images/7f9e48e5df9219eff0f80ab9524e028d.png)

### BBCworld和realDonaldTrump的情感分析对比

伤心！⁵

[https://twitter-r-cvdvxo3vga-uc.a.run.app/sentiment?n=1000&users=BBCWorld,realDonaldTrump](https://twitter-r-cvdvxo3vga-uc.a.run.app/sentiment?n=1000&users=BBCWorld,realDonaldTrump)

![](../Images/42efd06b01197b8373875beb2914f7ee.png)

### 政治家和娱乐圈人士有什么共同之处？

它们 overwhelmingly positive（在这里我们使用我们的 plotly html 端点）

[https://twitter-r-cvdvxo3vga-uc.a.run.app/html/sentiment?n=500&users=narendramodi,TheEllenShow](https://twitter-r-cvdvxo3vga-uc.a.run.app/html/sentiment?n=500&users=narendramodi,TheEllenShow)

![](../Images/4a5195bfb62dc19b985472e8f761d1d8.png)

### 结论

这就是本次无服务器 + R 教程的全部内容。希望你能学到一些有用的东西，或者至少觉得 Twitter 分析很有趣 😃。拥有这个应用程序的好处是，你可以用你选择的任何账号（甚至你自己的）分析实时 Twitter 频率或情感图，因此随意尝试吧。⁶

1.  使用无服务器架构还有一些非常好的好处，比如按秒计费、自动扩展的容器，这使得它非常适合部署爱好项目。

1.  如果你在 R 环境中，可以调用 `Sys.setenv()` 或者直接使用 CLI 导出。

1.  [https://www.tidytextmining.com/sentiment.html](https://www.tidytextmining.com/sentiment.html)[↩](https://www.timlrx.com/2020/01/22/serverless-machine-learning-with-r-on-cloud-run/#fnref3)

1.  我花了大约 15 分钟来构建这个镜像。这是 R 的一个缺点 - 它对于数据分析非常方便，但对生产环境不太友好。

1.  从 realDonaldTrump 抓取数据存在一些问题 ([https://github.com/ropensci/rtweet/issues/382](https://github.com/ropensci/rtweet/issues/382))

1.  只要使用量没有突然激增并超过免费套餐限制，我会尽可能保持它运行。

**简介：[Timothy Lin](https://www.linkedin.com/in/timothy-lin-0600ba141/)** 是一名数据科学家和计量经济学家。Timothy 有兴趣将数据科学技术应用于解决商业问题。他为多个公司提供咨询，涉及大数据分析、消费者研究和图论等项目。在工作之外，他常常思考如何在生产环境中更好地部署模型，并在 [https://www.timlrx.com](https://www.timlrx.com) 上博客分享他的最新发现。

[原文](https://www.timlrx.com/2020/01/22/serverless-machine-learning-with-r-on-cloud-run/)。经许可转载。

**相关：**

+   [R 用户的客户细分](/2019/09/customer-segmentation-r-users.html)

+   [R 中 K-最近邻的初学者指南：从零到英雄](/2020/01/beginners-guide-nearest-neighbors-r.html)

+   [2019 年 Stackoverflow 调查中的 R 用户薪资](/2019/08/r-users-salaries-2019-stackoverflow-survey.html)

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在的组织的 IT 工作

* * *

### 了解更多内容

+   [如何让 Python 代码运行得极快](https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html)

+   [使用 Jupysql 和 GitHub Actions 计划和运行 ETL](https://www.kdnuggets.com/2023/05/schedule-run-etls-jupysql-github-actions.html)

+   [学习如何在您的设备上仅用几步运行 Alpaca-LoRA](https://www.kdnuggets.com/2023/05/learn-run-alpacalora-device-steps.html)

+   [在本地使用 LM Studio 运行 LLM](https://www.kdnuggets.com/run-an-llm-locally-with-lm-studio)

+   [PyTest 入门：轻松编写和运行 Python 测试](https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python)

+   [使用 llamafile 分发和运行 LLM 的 5 个简单步骤](https://www.kdnuggets.com/distribute-and-run-llms-with-llamafile-in-5-simple-steps)
