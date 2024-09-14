# Kubernetes 中的高可用性 SQL Server Docker 容器

> 原文：[https://www.kdnuggets.com/2022/04/high-availability-sql-server-docker-containers-kubernetes.html](https://www.kdnuggets.com/2022/04/high-availability-sql-server-docker-containers-kubernetes.html)

![Kubernetes 中的 SQL Server Docker 容器](../Images/2e964f2d5f832ec43a26d0200b65f5a7.png)

图像由编辑提供

容器的诸多好处早已不是秘密——它们便携、安全、状态保留、可定制且无需安装。然而，IT 面临的一个主要难题是如何实现高可用性（HA），特别是对于像 Microsoft SQL Server Docker 容器这样的容器化状态工作负载。

* * *

## 我们的三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 需求

* * *

IT 通常在想要自动化管理、部署和扩展计算应用程序时转向 Kubernetes（K8s）。但作为独立解决方案，K8s 的原生 HA 对于支持 SQL Server Docker 容器的工作负载来说速度过慢。K8s 也不支持 SQL Server 可用性组（AG）。

为了克服这些障碍，你需要一种将 SQL Server 集群和软件定义边界（SDP）合并的方法，幸运的是，这种新型智能高可用性软件已经存在。这款软件提供了 SQL Server AG 所需的故障转移集群 HA，并且可以额外：

+   创建跨网络和平台的安全、无缝混合 SQL Server AG 集群，无需 VPN。

+   通过接近零恢复时间目标（RTO）的自动故障转移，保护节点、SQL Server Docker 容器和应用程序免受故障影响。

+   避免免费 Linux 集群中常见的 SQL Server HA 配置和网络问题。

由于 Kubernetes 不支持使用 SQL Server AG 自动故障转移，因此使用这种新型智能高可用性软件来解决这个问题是明智的。通过新的软件，企业可以在 Kubernetes 环境下通过 SQL Server Docker 容器实现预期的 SQL Server AG 性能，从而确保业务连续性。最佳实践是利用 K8s 在 SQL Server Docker 容器的 Pod 级别提供 HA，同时在数据库级别利用智能高可用性软件提供 SQL Server AG 支持。

# **智能高可用性软件的好处**

为简化在 Kubernetes 中建立 SQL Server 集群并配置你的 SQL Server AG，该软件可以作为 SQL Server 集群服务。它安装在每个 SQL Server Docker 容器上，并负责 SQL Server AG 在 Kubernetes 中的核心管理、网络管理、故障检测和故障转移自动化。智能高可用软件还提供其他好处，它灵活支持 Windows 和 Linux 的 SQL Server 集群。该软件还可以配置为“混合搭配”，在 SQL Server Docker 容器内外运行。

需要从一个平台迁移到另一个平台吗？智能高可用软件还可以协助迁移。你可以利用内置的软件定义边界功能，包括零信任网络访问（ZTNA）隧道，来创建多云环境。对于 SQL Server Docker 容器，该软件管理所需的 ZTNA 隧道，以实现集群通信和不同云中不同类型节点之间的 SQL Server AG 复制。

你可以在 K8s 内部或外部运行这种新型智能高可用软件来支持 SQL Server Docker 容器，使得在不同位置设置集群变得非常简单，同时通过 ZTNA 隧道和 Kubernetes 集群之间的自动故障转移保持一切安全。该软件提供所需的 HA 增强，因为 K8s 本身不支持 SQL Server AG 的自动故障转移。

智能高可用软件允许在 SQL Server Docker 容器之间形成 SQL Server AG——更不用说更容易部署，因为它允许 IT 生成一个 Docker 容器镜像，不仅包含 SQL Server 还包含智能高可用软件。然后，可以利用该镜像部署 SQL Server Docker 容器。

结论是什么？智能高可用软件使组织能够克服在 Kubernetes 中实施 SQL Server AG 的传统问题。企业可以利用该软件实现自动故障转移，并在网络中创建混合 Kubernetes SQL Server AG 集群。结果是保护节点、容器和应用程序免受常见故障的影响，并且不再为 HA/DR 配置而烦恼。

**[Don Boxley](https://www.linkedin.com/in/donboxleyjr/)** 是 [DH2i](http://www.dh2i.com) 的共同创始人兼 CEO。Don 从康奈尔大学约翰逊管理学院获得了 MBA 学位。

### 更多相关主题

+   [如何调试运行中的 Docker 容器](https://www.kdnuggets.com/how-to-debug-running-docker-containers)

+   [Kubernetes 实战：第二版](https://www.kdnuggets.com/2022/03/manning-kubernetes-action-second-edition.html)

+   [数据科学家的高薪副业](https://www.kdnuggets.com/2022/01/high-paying-side-hustles-data-scientists.html)

+   [KDnuggets™ 新闻 22:n04, 1月26日: 高薪副业...](https://www.kdnuggets.com/2022/n04.html)

+   [AI的人才管理：打造高效能AI团队](https://www.kdnuggets.com/2022/03/people-management-ai-building-highvelocity-ai-teams.html)

+   [数据科学家的7个高薪副业](https://www.kdnuggets.com/7-high-paying-side-hustles-for-data-scientists)
