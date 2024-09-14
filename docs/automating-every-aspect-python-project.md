# 自动化您的 Python 项目的每个方面

> 原文：[https://www.kdnuggets.com/2020/09/automating-every-aspect-python-project.html](https://www.kdnuggets.com/2020/09/automating-every-aspect-python-project.html)

[评论](#comments)

**作者为[马丁·海因兹](https://www.linkedin.com/in/heinz-martin/)，IBM 的 DevOps 工程师**

![图片](../Images/af58abcae89d0c2899a8c38dbc1b2607.png)

每个项目 - 无论您是在开发 Web 应用程序、某些数据科学或人工智能 - 都可以从良好配置的 CI/CD、在开发过程中既可调试又针对生产环境优化的 Docker 镜像或一些额外的代码质量工具（如*CodeClimate*或*SonarCloud*）中受益。本文中将介绍所有这些内容，并将看到如何将这些内容添加到您的*Python*项目中！

*本文是前一篇关于创建*[*“终极”Python 项目设置*](https://towardsdatascience.com/ultimate-setup-for-your-next-python-project-179bda8a7c2c)*的文章的续篇，所以在阅读本文之前您可能想查看那篇.*

*太长不看：这是我的包含完整源代码和文档的存储库：*[*https://github.com/MartinHeinz/python-project-blueprint*](https://github.com/MartinHeinz/python-project-blueprint)

### 为开发创建可调试的 Docker 容器

有些人不喜欢*Docker*，因为容器很难调试，或者因为它们的镜像构建时间很长。因此，让我们从这里开始，构建适合开发的镜像 - 构建快速且易于调试。

为了使镜像易于调试，我们需要一个包含我们在调试时可能需要的*所有*工具的基础镜像 - 诸如`bash`、`vim`、`netcat`、`wget`、`cat`、`find`、`grep`等工具。`python:3.8.1-buster`似乎是此任务的理想选择。它默认包含许多工具，我们可以很容易地安装所有缺少的内容。这个基础镜像相当*庞大*，但在这里这并不重要，因为它仅用于开发。另外，您可能已经注意到，我选择了非常具体的镜像 - 锁定了*Python*和*Debian*的版本 - 这是有意为之的，因为我们要尽量减少因较新且可能不兼容的*Python*或*Debian*版本引起的*“破坏”*的机会。

作为替代，您可以使用基于*Alpine*的镜像。不过，这可能会导致一些问题，因为它使用`musl libc`而不是`glibc`，而*Python*中依赖于`glibc`。因此，如果决定选择这条路，请记住这一点。

至于构建速度，我们将利用多阶段构建来允许我们尽可能缓存尽可能多的层。这样我们就可以避免下载依赖项和工具，比如`gcc`，以及我们的应用程序所需的所有库（来自`requirements.txt`）。

为了进一步加快速度，我们将从之前提到的`python:3.8.1-buster`创建自定义基础镜像，其中包含我们需要的所有工具，因为我们无法将下载和安装这些工具到最终运行镜像的步骤缓存。

说够了，让我们看看`Dockerfile`：

上面你可以看到，在创建最终的*runner*镜像之前，我们将经过三个中间镜像。第一个镜像被命名为`builder`。它下载构建最终应用程序所需的所有库，包括`gcc`和*Python*虚拟环境。安装完成后，它还会创建实际的虚拟环境，接下来镜像将使用这个虚拟环境。

接下来是`builder-venv`镜像，它将我们的依赖列表（`requirements.txt`）复制到镜像中，然后进行安装。这个中间镜像用于缓存，因为我们只希望在`requirements.txt`更改时安装库，否则我们只使用缓存。

在我们创建最终镜像之前，首先要对应用程序进行测试。这就是在`tester`镜像中发生的事情。我们将源代码复制到镜像中并运行测试。如果测试通过，我们会继续到`runner`镜像。

对于 runner 镜像，我们使用了一个包含一些额外工具的自定义镜像，例如`vim`或`netcat`，这些工具在普通的*Debian* 镜像中并不存在。你可以在*Docker Hub* [这里](https://hub.docker.com/repository/docker/martinheinz/python-3.8.1-buster-tools)找到这个镜像，还可以查看非常简单的`Dockerfile`，地址在`base.Dockerfile` [这里](https://github.com/MartinHeinz/python-project-blueprint/blob/master/base.Dockerfile)。在这个最终镜像中，我们首先从`tester`镜像中复制所有安装的依赖项的虚拟环境，然后复制经过测试的应用程序。现在我们在镜像中有了所有源文件，我们转到应用程序所在的目录，然后设置`ENTRYPOINT`，以便在镜像启动时运行应用程序。出于安全原因，我们还将`USER`设置为*1001*，因为最佳实践告诉我们不应以`root`用户身份运行容器。最后的两行设置了镜像的标签。这些标签将在使用`make`目标构建时被替换/填充，我们稍后将看到这一点。

### 生产环境优化的 Docker 容器

当涉及到生产级镜像时，我们希望确保它们小巧、安全和快速。我个人最喜欢的这个任务的选择是来自*Distroless*项目的*Python*镜像。那*Distroless*是什么呢？

我这样说吧——在一个理想的世界里，每个人都应该使用`FROM scratch`作为基础镜像（即空镜像）来构建他们的镜像。然而，这不是我们大多数人想做的，因为这要求你静态链接二进制文件等。这就是*Distroless*发挥作用的地方——它是为*每个人*提供的`FROM scratch`。

好的，现在来实际描述一下*Distroless*是什么。它是由*Google* 制作的一组镜像，包含了应用程序所需的最基本内容，意味着没有 shell、包管理器或任何其他会使镜像膨胀并给安全扫描器（如[CVE](https://cve.mitre.org/)）带来信号噪声的工具，从而使建立合规性变得更困难。

现在我们知道我们要处理什么了，让我们看看*production* `Dockerfile`... 实际上，我们不会在这里做太多更改，仅仅是2行：

我们只需更改用于构建和运行应用程序的基础镜像！但差异很大——我们的开发镜像是1.03GB，而这个镜像只有103MB，这个差别*相当*大！我知道，我已经听到你了——*“但是Alpine可以更小！”*——是的，没错，但大小并不是*那么重要*。你只会在下载/上传镜像时注意到镜像大小，这并不常见。当镜像运行时，大小根本不重要。比大小更重要的是安全性，在这方面*Distroless*无疑更优，因为*Alpine*（一个很好的替代品）有很多额外的包，增加了攻击面。

讨论*Distroless*时最后值得一提的是*debug*镜像。考虑到*Distroless*不包含*任何* shell（甚至没有`sh`），当你需要调试和探查时会变得非常棘手。为此，所有*Distroless*镜像都有`debug`版本。因此，当出现问题时，你可以使用`debug`标签构建生产镜像，并将其与正常镜像一起部署，进入它并执行，例如线程转储。你可以这样使用`python3`镜像的调试版本：

### 单一命令搞定一切

准备好所有的`Dockerfiles`后，让我们用`Makefile`来彻底自动化吧！我们要做的第一件事是用*Docker*构建我们的应用程序。因此，我们可以运行`make build-dev`来构建开发镜像，该命令运行以下目标：

这个目标通过首先将`dev.Dockerfile`底部的标签替换为由运行`git describe`生成的镜像名称和标签，然后运行`docker build`来构建镜像。

接下来——使用`make build-prod VERSION=1.0.0`构建生产环境：

这与之前的目标非常相似，但我们将使用作为参数传递的版本，而不是使用`git`标签作为版本，在上面的例子中是`1.0.0`。

当你在*Docker*中运行所有操作时，你还需要在*Docker*中进行调试，为此，有以下目标：

从上述内容我们可以看到，entrypoint 被`bash`覆盖，容器命令被参数覆盖。这样我们可以进入容器进行探查，或者像上面的例子一样运行一次性命令。

当我们完成编码并想要将镜像推送到*Docker*注册表时，我们可以使用`make push VERSION=0.0.2`。让我们看看这个目标做了什么：

它首先运行我们之前查看的`build-prod`目标，然后只是运行`docker push`。这假设你已经登录到*Docker*注册表，因此在运行之前需要先执行`docker login`。

最后的目标是清理*Docker*工件。它使用被替换到`Dockerfiles`中的`name`标签来筛选和查找需要删除的工件：

你可以在我的仓库中找到这个`Makefile`的完整代码列表：[https://github.com/MartinHeinz/python-project-blueprint/blob/master/Makefile](https://github.com/MartinHeinz/python-project-blueprint/blob/master/Makefile)

### 使用GitHub Actions进行CI/CD

现在，让我们利用所有这些方便的`make`目标来设置我们的CI/CD。我们将使用*GitHub Actions*和*GitHub Package Registry*来构建我们的管道（作业）和存储我们的镜像。那么，这些究竟是什么呢？

+   *GitHub Actions*是*jobs/pipelines*，它们帮助你自动化开发工作流程。你可以用它们来创建单独的任务，然后将这些任务组合成自定义工作流程，然后在每次推送到仓库或创建发布时执行这些工作流程。

+   *GitHub Package Registry*是一个与GitHub完全集成的包托管服务。它允许你存储各种类型的包，例如Ruby的*gems*或*npm*包。我们将用它来存储我们的*Docker*镜像。如果你对*GitHub Package Registry*不太熟悉并想要了解更多信息，你可以查看我的博客文章[这里](https://martinheinz.dev/blog/6)。

现在，为了使用*GitHub Actions*，我们需要创建*workflows*，这些*workflows*将根据我们选择的触发器（例如推送到仓库）执行。这些*workflows*是位于我们仓库中的`.github/workflows`目录中的*YAML*文件：

在这里，我们将创建2个文件`build-test.yml`和`push.yml`。第一个文件`build-test.yml`将包含2个作业，这些作业将在每次推送到仓库时触发，让我们来看看这些作业：

第一个作业叫做`build`，它通过运行我们的`make build-dev`目标来验证我们的应用程序是否可以构建。然而，在运行之前，它会首先通过执行名为`checkout`的操作来检出我们的仓库，这个操作在*GitHub*上发布。

第二个任务稍微复杂一些。它会对我们的应用程序以及3个代码质量检查工具（linters）运行测试。与之前的任务一样，我们使用`checkout@v1`操作来获取我们的源代码。之后，我们运行另一个名为`setup-python@v1`的已发布操作，它为我们设置Python环境（你可以在[这里](https://github.com/actions/setup-python)找到详细信息）。现在我们已经有了Python环境，我们还需要从`requirements.txt`中安装应用程序的依赖项，这些依赖项通过`pip`进行安装。此时，我们可以继续运行`make test`目标，这将触发我们的*Pytest*测试套件。如果我们的测试套件通过，我们会继续安装前面提到的代码检查工具——*pylint*、*flake8*和*bandit*。最后，我们运行`make lint`目标，这将触发每一个代码检查工具。

这就是构建/测试作业的全部内容，但推送作业呢？让我们也来看看：

前四行定义了我们希望何时触发此任务。我们指定这个任务仅在标签被推送到仓库时开始（`*`指定了标签名称的模式 - 在这种情况下是*任何*）。这样我们就不会在每次推送到仓库时都将 Docker 镜像推送到*GitHub Package Registry*，而是仅在推送指定应用程序新版本的标签时才这样做。

现在进入这项工作的主体 — 首先检出源代码，并将`RELEASE_VERSION`环境变量设置为我们推送的`git`标签。这个操作是使用*GitHub Actions*的内置`::setenv`功能完成的（更多信息见[这里](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/development-tools-for-github-actions#set-an-environment-variable-set-env)）。接下来，使用存储在仓库中的`REGISTRY_TOKEN`密钥和发起工作流的用户（`github.actor`）登录 Docker 注册表。最后一行执行`push`目标，这会构建生产镜像并将其推送到注册表中，标签为之前推送的`git`标签。

你可以在我的仓库中的文件里查看完整的代码列表[这里](https://github.com/MartinHeinz/python-project-blueprint/tree/master/.github/workflows)。

### 使用 CodeClimate 进行代码质量检查

最后但同样重要的是，我们还将使用*CodeClimate*和*SonarCloud*添加代码质量检查。这些检查将与上述*test*任务一起触发。所以，让我们在其中添加几行：

我们从*CodeClimate*开始，首先导出`GIT_BRANCH`变量，这个变量是通过`GITHUB_REF`环境变量检索到的。接下来，我们下载*CodeClimate*测试报告器并使其可执行。然后，我们用它来格式化测试套件生成的覆盖报告，最后一行将其通过测试报告器 ID 发送到*CodeClimate*，该 ID 存储在仓库的密钥中。

至于*SonarCloud*，我们需要在仓库中创建一个`sonar-project.properties`文件，其内容如下（该文件的值可以在*SonarCloud*仪表板的右下角找到）：

除此之外，我们只需使用现有的`sonarcloud-github-action`，它会为我们完成所有工作。我们所需要做的就是提供两个令牌 - *GitHub*的令牌（默认在仓库中）和*SonarCloud*的令牌（可以从*SonarCloud*网站获取）。

*注意：获取和设置之前提到的所有令牌和密钥的步骤在仓库的 README 文件中*[*这里*](https://github.com/MartinHeinz/python-project-blueprint/blob/master/README.md)*。*

### 结论

就这些！通过上述工具、配置和代码，你已准备好构建和自动化你下一个*Python*项目的所有方面！如果你需要更多关于本文中展示/讨论的主题的信息，可以查看我这里的文档和代码：[https://github.com/MartinHeinz/python-project-blueprint](https://github.com/MartinHeinz/python-project-blueprint)，如果你有任何建议或问题，请在仓库中提交问题，或者如果你喜欢这个小项目，请给它加星。 ????

**资源**

+   [适合Python应用的最佳Docker基础镜像](https://pythonspeed.com/articles/base-image-python-docker-images/)

+   [Google Distroless](https://github.com/GoogleContainerTools/distroless)

+   [扫描你的Docker镜像中的漏洞](https://medium.com/better-programming/scan-your-docker-images-for-vulnerabilities-81d37ae32cb3)

+   [5个开源容器安全工具](https://opensource.com/article/18/8/tools-container-security)

+   [SonarCloud GitHub Action](https://github.com/SonarSource/sonarcloud-github-action)

**个人简介: [马丁·海因茨](https://www.linkedin.com/in/heinz-martin/)** 是IBM的一名DevOps工程师。作为软件开发者，马丁对计算机安全、隐私和加密学充满热情，专注于云计算和无服务器计算，并且总是准备迎接新的挑战。

[原文](https://martinheinz.dev/blog/17)。已获许可转载。

**相关：**

+   [MIT免费课程: 计算机科学与Python编程入门](/2020/09/free-mit-intro-computer-science-programming-python.html)

+   [数据科学遇见Devops: 使用Jupyter、Git和Kubernetes的MLOps](/2020/08/data-science-meets-devops-mlops-jupyter-git-kubernetes.html)

+   [在AWS Fargate上部署机器学习管道](/2020/07/deploy-machine-learning-pipeline-aws-fargate.html)

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT需求

* * *

### 更多相关话题

+   [每位初学者数据科学家应该掌握的6种预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [成为优秀数据科学家需要的5项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [2021年最佳ETL工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [每个数据科学家都应该知道的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)

+   [使用管道编写干净的 Python 代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)
