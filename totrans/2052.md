# 不再需要使用 Docker

> 原文：[https://www.kdnuggets.com/2020/10/use-docker-anymore.html](https://www.kdnuggets.com/2020/10/use-docker-anymore.html)

[评论](#comments)

**由 [Martin Heinz](https://www.linkedin.com/in/heinz-martin/), IBM 的 DevOps 工程师**

在容器的古老时代（实际上大约是 4 年前），*Docker* 是容器领域的唯一玩家。然而现在情况已不再如此，Docker 并不是 *唯一* 的容器引擎，而只是 *另一个* 容器引擎。Docker 允许我们构建、运行、拉取、推送或检查容器镜像，但对于每一项任务，还有其他替代工具，可能比 Docker 做得更好。所以，让我们深入了解一下这些工具，并且（*也许*）彻底卸载和忘记 Docker……

![图示](../Images/f504eeb72a30558da7db944a64f471d0.png)

[Nicole Chen](https://unsplash.com/@917sunny?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/@917sunny?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 那么为什么不使用 Docker 呢？

如果你已经使用 Docker 很长时间，我认为需要一些说服才能让你考虑切换到不同的工具。所以，下面就是：

首先，Docker 是一个整体的工具。它试图做所有事情，这通常不是最好的方法。大多数情况下，选择一个只做一件事但做得非常好的专业工具更为理想。

如果你担心切换到不同的工具集，因为你需要学习使用不同的 CLI、不同的 API 或一般的不同概念，那也不会成为问题。选择本文中展示的任何工具都可以完全无缝过渡，因为它们（包括 Docker）都遵循 OCI 的相同规范。OCI 即 [Open Container Initiative](https://opencontainers.org/)，该倡议包含了 [容器运行时](https://github.com/opencontainers/runtime-spec)、[容器分发](https://github.com/opencontainers/distribution-spec) 和 [容器镜像](https://github.com/opencontainers/image-spec) 的规范，这涵盖了处理容器所需的所有功能。

感谢 OCI，你可以选择最适合自己需求的一套工具，同时你仍然可以使用与 Docker 相同的 API 和 CLI 命令。

如果你愿意尝试新的工具，那我们可以比较 Docker 及其竞争对手的优缺点和特点，看看是否有必要考虑抛弃 Docker 转而使用一些新的闪亮工具。

### 容器引擎

在比较 Docker 与任何其他工具时，我们需要分解它的组件，首先要谈的是*容器引擎*。容器引擎是一个提供用户界面以操作镜像和容器的工具，这样你就不必处理像`SECCOMP`规则或 SELinux 策略之类的事务。它的工作也是从远程仓库拉取镜像并将其展开到你的磁盘上。它还*似乎*运行容器，但实际上它的工作是创建容器清单和包含镜像层的目录。然后，它将这些传递给*容器运行时*，如`runc`或`crun`（我们稍后会讨论）。

现在有许多容器引擎可用，但与 Docker 竞争最突出的对手是*Podman*，由*Red Hat*开发。与 Docker 不同，Podman 不需要守护进程运行，也不需要 root 权限，这长期以来一直是 Docker 的一个问题。根据名称，Podman 不仅可以运行容器，还可以运行*pods*。如果你不熟悉 pods 的概念，那么 pod 是 Kubernetes 的最小计算单元。它由一个或多个容器组成——主要容器和所谓的*sidecars*——执行支持任务。这使得 Podman 用户更容易将工作负载迁移到 Kubernetes。所以，作为一个简单的演示，这就是如何在一个 pod 中运行 2 个容器：

最后，Podman 提供了与 Docker 完全相同的 CLI 命令，因此你可以直接使用`alias docker=podman`，假装什么都没有改变。

除了 Docker 和 Podman，还有其他容器引擎，但我认为它们都属于死胡同技术，或不适合本地开发和使用。但为了全面了解，我们至少提一下现有的选择：

+   [LXD](https://linuxcontainers.org/lxd/introduction/) — LXD 是 LXC（Linux Containers）的容器管理器（守护进程）。这个工具提供运行*系统*容器的能力，这些容器提供的环境更类似于虚拟机。它处于一个非常狭窄的领域，没有很多用户，所以除非你有非常特定的用例，否则你可能还是使用 Docker 或 Podman 更好。

+   [CRI-O](https://cri-o.io/) — 当你搜索 cri-o 是什么时，你可能会发现它被描述为容器引擎。但它实际上是容器运行时。除了它实际上不是引擎外，它也不适合*“正常”*使用。我的意思是，它是专门为 Kubernetes 运行时 (CRI) 构建的，而不是供终端用户使用。

+   [rkt](https://coreos.com/rkt/) — rkt (*“rocket”*) 是由*CoreOS*开发的容器引擎。提到这个项目只是为了完整性，因为该项目已结束，开发被暂停——因此不应使用它。

### 构建镜像

在容器引擎方面，实际上只有一个替代 Docker 的选择。然而，在构建镜像方面，我们有更多的选择。

首先，让我介绍一下 [Buildah](https://buildah.io/)。Buildah 是 Red Hat 开发的另一种工具，它与 Podman 非常兼容。如果你已经安装了 Podman，你可能已经注意到 `podman build` 子命令，这实际上就是 Buildah 的伪装，因为它的二进制文件包含在 Podman 中。

就其功能而言，它遵循与 Podman 相同的路线——它是无守护进程和无根的，并且生成符合 OCI 标准的镜像，因此可以保证你的镜像会以与 Docker 构建的镜像相同的方式运行。它还能够从 `Dockerfile` 或更适合命名的 `Containerfile` 构建镜像，这两者本质上是相同的，只是名称不同。除此之外，Buildah 还提供了对镜像层的更精细控制，允许你将多个更改提交到单个层中。与 Docker 的一个意外但（在我看来）不错的区别是，Buildah 构建的镜像是特定于用户的，因此你只能列出你自己构建的镜像。

现在，考虑到 Buildah 已经包含在 Podman CLI 中，你可能会问为什么还要使用单独的 `buildah` CLI？实际上，`buildah` CLI 是 `podman build` 命令的超集，因此你可能不需要触及 `buildah` CLI，但通过使用它你可能会发现一些额外的有用功能（有关 `podman build` 和 `buildah` 之间差异的具体信息，请参见以下 [文章](https://podman.io/blogs/2018/10/31/podman-buildah-relationship.html)）。

话虽如此，让我们来看一个小演示：

从上述脚本中你可以看到，你可以简单地使用 `buildah bud` 来构建镜像，其中 `bud` 代表 *使用 Dockerfile 构建*，但你也可以使用更具脚本化的方法，使用 Buildah 的 `from`、`run` 和 `copy`，这些命令与 Dockerfile 中的命令（ `FROM image`、`RUN ...`、`COPY ...` ）是等效的。

接下来是 Google 的 [Kaniko](https://github.com/GoogleContainerTools/kaniko)。Kaniko 也从 Dockerfile 构建容器镜像，类似于 Buildah，它也不需要守护进程。与 Buildah 的主要区别是 Kaniko 更专注于在 Kubernetes 中构建镜像。

Kaniko 旨在作为一个镜像运行，使用 `gcr.io/kaniko-project/executor`，这对于 Kubernetes 是有意义的，但对本地构建不是很方便，实际上有点违背了目的，因为你需要使用 Docker 来运行 Kaniko 镜像以构建你的镜像。也就是说，如果你正在寻找在 Kubernetes 集群中构建镜像的工具（例如在 CI/CD 管道中），那么考虑到 Kaniko 是无守护进程的且（[也许](https://github.com/GoogleContainerTools/kaniko#security)）更安全，它可能是一个不错的选择。

从我的个人经验来看——我使用过 Kaniko 和 Buildah 在 Kubernetes/OpenShift 集群中构建镜像，我认为这两者都能完成任务，但使用 Kaniko 时我遇到过一些随机的构建崩溃和推送镜像到注册表时的失败。

第三个竞争者是 [buildkit](https://github.com/moby/buildkit)，也可以称为*下一代*`docker build`。它是 *Moby* 项目（和 Docker 一样）的一部分，可以通过 `DOCKER_BUILDKIT=1 docker build ...` 将其作为实验性功能在 Docker 中启用。那么，这到底会带来什么呢？它引入了一系列改进和很酷的功能，包括并行构建步骤、跳过未使用的阶段、更好的增量构建和无根构建。然而，它仍然需要运行守护进程（`buildkitd`）。所以，如果你不想摆脱 Docker，但希望获得一些新功能和改进，使用 buildkit 可能是一个不错的选择。

与上一节一样，这里我们也有一些*“值得一提”*的工具，它们填补了一些非常特定的使用场景，但不会是我的首选：

+   [Source-To-Image (S2I)](https://github.com/openshift/source-to-image) 是一个直接从源代码构建镜像的工具包，无需 Dockerfile。这个工具在简单、预期的场景和工作流程中表现良好，但如果需要稍微多一些定制，或者项目的布局不符合预期，它很快就会变得麻烦和笨拙。如果你对 Docker 还不太自信，或者在 OpenShift 集群上构建镜像，考虑使用 S2I，因为 S2I 是一个内置的功能。

+   [Jib](https://github.com/GoogleContainerTools/jib) 是 Google 另一个工具，专门用于构建 Java 镜像。它包括 [Maven](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#quickstart) 和 [Gradle](https://github.com/GoogleContainerTools/jib/tree/master/jib-gradle-plugin#quickstart) 插件，可以让你轻松构建镜像，而无需处理 Dockerfile。

+   最后但同样重要的是 [Bazel](https://github.com/bazelbuild/bazel)，这是 Google 另一个工具。它不仅仅用于构建容器镜像，而是一个完整的构建系统。如果你*仅仅*想构建一个镜像，那么深入研究 Bazel 可能有些过于复杂，但绝对是一个很好的学习经验，所以如果你愿意的话，[rules_docker](https://github.com/bazelbuild/rules_docker) 部分是一个很好的起点。

### 容器运行时

最后一块大拼图是*容器运行时*，负责运行容器。容器运行时是整个容器生命周期/堆栈的一部分，你很可能不会去碰它，除非你有一些非常特定的速度、安全性等要求。所以，如果你已经厌倦了我，那么你可能想跳过这一部分。如果你只是想了解有哪些选项，那么接下来是：

[runc](https://github.com/opencontainers/runc)是最流行的容器运行时，基于OCI容器运行时规范创建。它被Docker（通过*containerd*）、Podman和CRI-O使用，所以几乎涵盖了除LXD（使用LXC）以外的一切。我没什么其他可以补充的。它是（几乎）所有事物的默认运行时，所以即使你在读完这篇文章后放弃Docker，你很可能仍然会使用runc。

与`runc`类似且容易混淆的一个替代品是[crun](https://github.com/containers/crun)。这是由Red Hat开发的工具，完全用C语言编写（而runc是用Go语言编写的）。这使得它比runc更快且更节省内存。考虑到它也是OCI兼容的运行时，如果你想亲自检查一下，应该能轻松切换。尽管目前它还不太流行，但从RHEL 8.3版本起，它将作为OCI运行时的替代品进行技术预览，鉴于它是Red Hat的产品，我们最终可能会看到它成为Podman或CRI-O的默认运行时。

说到CRI-O，之前我提到CRI-O实际上不是一个容器引擎，而是一个容器运行时。这是因为CRI-O不包括推送镜像等功能，而这是你从容器引擎中期望的。CRI-O作为运行时内部使用runc来运行容器。这个运行时并不适合在你的机器上使用，因为它是为Kubernetes节点设计的运行时，你可以看到它被描述为*“Kubernetes所需的所有运行时功能而无其他”*。所以，除非你在设置Kubernetes集群（或OpenShift集群——CRI-O在这里已经是默认的），否则你可能不应该使用它。

这一部分的最后一个工具是[containerd](https://containerd.io/)，它是CNCF的一个毕业项目。它是一个作为各种容器运行时和操作系统的API外观的守护进程。在后台，它依赖于runc，并且是Docker引擎的默认运行时。它也被Google Kubernetes Engine（GKE）和IBM Kubernetes Service（IKS）使用。它是Kubernetes容器运行时接口（与CRI-O相同）的实现，因此它是你的Kubernetes集群的一个很好的运行时候选。

### 镜像检查和分发

容器栈的最后一部分是镜像检查和分发。这有效地替代了`docker inspect`，并且（可选地）增加了在远程注册表之间复制/镜像镜像的能力。

我将在这里提到的唯一可以完成这些任务的工具是[Skopeo](https://github.com/containers/skopeo)。它由Red Hat制作，是Buildah、Podman和CRI-O的配套工具。除了我们都熟悉的基本`skopeo inspect`，Skopeo还能够使用`skopeo copy`来复制镜像，这使得你可以在远程注册表之间镜像镜像，而不需要首先将它们拉取到本地注册表。如果你使用本地注册表，这个功能也可以作为拉取/推送使用。

作为一个小小的奖励，我还想提到 [。Dive](https://github.com/wagoodman/dive)，这是一个用于检查、探索和分析镜像的工具。它更具用户友好性，提供了更易读的输出，并可以深入挖掘（或*潜入*，我想）你的镜像，分析并测量其效率。它也适用于CI管道中，可以测量你的镜像是否*“足够高效”*，换句话说——是否浪费了过多空间。

### 结论

这篇文章并非旨在说服你完全放弃Docker，而是展示了构建、运行、管理和分发容器及其镜像的整个领域和所有选项。包括Docker在内的每种工具都有其优缺点，重要的是评估哪一组工具最适合你的工作流程和用例，希望这篇文章能帮助你做到这一点。

### 资源

+   [让我们尝试所有可用的CRI运行时用于Kubernetes。真的！](https://www.youtube.com/watch?v=FKoVztEQHss&ab_channel=CNCF%5BCloudNativeComputingFoundation%5D)

+   [容器运行时在Kubernetes中的重要性](https://events19.linuxfoundation.org/wp-content/uploads/2017/11/How-Container-Runtime-Matters-in-Kubernetes_-OSS-Kunal-Kushwaha.pdf)

+   [容器术语实用介绍](https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction)

+   [比较下一代容器镜像构建工具](https://events19.linuxfoundation.org/wp-content/uploads/2017/11/Comparing-Next-Generation-Container-Image-Building-Tools-OSS-Akihiro-Suda.pdf)

+   [全面的容器运行时比较](https://www.capitalone.com/tech/cloud/container-runtime/)

+   [没有Docker构建容器](https://blog.alexellis.io/building-containers-without-docker/)

**简介： [马丁·海因茨](https://www.linkedin.com/in/heinz-martin/)** 是IBM的一名DevOps工程师。作为一名软件开发者，马丁对计算机安全、隐私和密码学充满热情，专注于云计算和无服务器计算，并时刻准备接受新的挑战。

[原文](https://martinheinz.dev/blog/35?utm_source=tds&utm_medium=referral&utm_campaign=blog_post_35)。经许可转载。

**相关：**

+   [Docker镜像优化策略](/2020/10/strategies-docker-images-optimization.html)

+   [自动化你Python项目的各个方面](/2020/09/automating-every-aspect-python-project.html)

+   [让Python程序运行得更快](/2020/09/making-python-programs-blazingly-fast.html)

### 更多主题

+   [如果没有相关学位如何进入数据分析领域](https://www.kdnuggets.com/2021/12/how-to-get-into-data-analytics.html)

+   [如何有效管理镜像版本的Docker标签](https://www.kdnuggets.com/how-to-use-docker-tags-to-manage-image-versions-effectively)

+   [如何使用Docker卷进行持久数据存储](https://www.kdnuggets.com/how-to-use-docker-volumes-for-persistent-data-storage)

+   [我们不需要数据科学家，我们需要数据工程师](https://www.kdnuggets.com/2021/02/dont-need-data-scientists-need-data-engineers.html)

+   [深度神经网络不会引领我们走向通用人工智能](https://www.kdnuggets.com/2021/12/deep-neural-networks-not-toward-agi.html)

+   [不要成为一个商品化的数据科学家](https://www.kdnuggets.com/2022/10/commoditized-data-scientist.html)
