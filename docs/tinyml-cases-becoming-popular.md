# 为什么 TinyML 案例越来越受欢迎？

> 原文：[`www.kdnuggets.com/2022/10/tinyml-cases-becoming-popular.html`](https://www.kdnuggets.com/2022/10/tinyml-cases-becoming-popular.html)

![为什么 TinyML 案例越来越受欢迎？](img/4ca3506fd8bf8de77342203bde20e0e8.png)

图片由[Nubia Navarro (nubikini)](https://www.pexels.com/photo/wall-e-die-cast-model-981588/)提供

各类组织都使用机器学习来处理和分析大型数据集。TinyML（Tiny Machine Learning）是一种机器学习 API，因其低功耗能力而日益受到欢迎。TinyML 正迅速成为从初学者层面掌握机器学习的最佳选择之一。

本文将概述 TinyML 的定义、应用场景以及它为何越来越受欢迎。

# 什么是 TinyML？

Tiny Machine Learning（TinyML）是一种优化的机器学习技术，允许机器学习模型（软件）在使用极低功耗微控制器的嵌入式系统上运行。

嵌入式系统由为特定功能设计的计算机硬件和软件组成。它们仅为一个目的而开发，这也是它们与笔记本电脑、PC、平板电脑和智能手机等典型计算机系统不同的原因。嵌入式系统的一个例子是电子计算器或 ATM。

TinyML 以非常优化的方式使机器学习在微控制器和[物联网](https://www.oracle.com/internet-of-things/what-is-iot/)（IoT）设备上得以实现，这意味着可以利用和分析大量数据，同时消耗极少的电力。

现在让我们更详细地了解微控制器。

## 微控制器概述

微控制器由计算机处理器、RAM、ROM 和嵌入式系统的输入/输出（I/O）端口组成，这是允许嵌入式系统处理软件的常见硬件配置。

使用微控制器的主要好处包括：

+   **低功耗** - 微控制器是低功耗硬件，工作时只需几毫瓦和微瓦。这意味着微控制器的功耗约为普通计算机系统的千分之一。

+   **低价格** - 微控制器非常便宜，仅在 2020 年就出货超过 280 亿个。

+   **多功能使用** - 微控制器可以用于所有设备、工具和家电。

# TinyML 的优势

使用 TinyML 有三个主要的[优势](https://news.mit.edu/2021/tiny-machine-learning-design-alleviates-bottleneck-memory-usage-iot-devices-1208)：

1.  **数据可以低延迟处理** - 由于 TinyML 允许在设备上进行分析而无需连接到服务器，嵌入式系统可以几乎没有延迟（低延迟）地处理数据并产生输出。

1.  **无需连接** - 该设备无需互联网连接即可使 TinyML 模型正常工作。

1.  **数据隐私** - 由于所有数据都保存在设备内部且没有连接，因此数据被泄露的风险极低。

# TinyML 的实施

有一些流行的机器学习框架支持 TinyML。

+   **Edge Impulse** 是一个免费的边缘设备（提供网络入口的硬件）机器学习开发平台。一个例子可能是路由器或路由交换机。

+   **TensorFlow Lite** 是[一套工具库](https://www.tensorflow.org/lite/guide)，允许开发人员在嵌入式系统、边缘设备和其他独立设备上启用设备端机器学习。

+   **PyTorch Mobile** 是一个开源的移动平台机器学习框架，兼容 TinyML。

# 为什么 TinyML 越来越受欢迎？

物联网（IoT）网络在各个行业中变得越来越普遍。因此，需要更多的边缘设备来连接电源和网络端点；TinyML 以具有成本效益的方式满足这些要求。

如前所述，TinyML 在不需要互联网连接的低功耗微控制器上工作。这使得设备能够在不消耗大量资源的情况下实时处理和响应数据。

TinyML 模型可以作为云环境的替代方案，降低成本，减少能耗，并提供更多的数据隐私。所有数据都在单个设备上处理，没有延迟，确保了令人印象深刻的连接和处理速度。

TinyML 在 2022 年变得越来越受欢迎，研究人员预计未来将进一步增长。技术研究和战略指导集团 ABI 预测，到 2030 年，[将有 25 亿台设备](https://www.abiresearch.com/press/tinyml-device-shipments-grow-25-billion-2030-15-million-2020/)使用 TinyML 系统。

TinyML 的优势从即时分析到零延迟，使其成为一个在依赖速度的世界中显而易见的选择。此外，本地数据处理意味着敏感信息比集中数据中心更好地保护免受网络犯罪分子的威胁。

# TinyML 面临的挑战

尽管 TinyML 的好处和潜力很明显，但它并非没有挑战，这些挑战可能对开发人员构成一些问题。

+   **有限的内存容量** - 使用 TinyML 的嵌入式系统的内存通常只有几兆字节，有时甚至只有几千字节。这对 TinyML 模型的复杂性施加了重大限制，因此只有少数框架可以用于 TinyML 开发。

+   **无法远程故障排除** - 由于 TinyML 模型仅在本地设备上运行，开发人员进行故障排除以确定和修复问题变得更加困难。这就是云环境提供优势的地方。

# TinyML 在实际情况中如何应用？

TinyML 可以有效应用于许多不同的行业，几乎所有使用 IoT 网络和数据的行业。以下是一些 TinyML 已用于推动操作的行业。

## 农业

TinyML 设备已被用于监控和收集实时的农作物和牲畜数据。一个市场领先的例子是由瑞典的边缘 AI 产品公司 Imagimob 开发的。Imagimob 已与欧盟内的 55 个组织合作，以了解 TinyML 如何提供成本效益高的牲畜和作物管理。

在一项研究中，两台拖拉机配备了 Dialog IoT Kit（博世传感器）设备和安卓手机以收集数据。这些数据要么由拖拉机操作员实时记录，要么通过分析智能手机的视频流来记录。

Imagimob 的 AI 软件已[安装在传感器](https://www.imagimob.com/blog/agritech-monitoring-cattle-with-iot-and-edge-ai)、电池和长程无线电设备上，以改善这种方法，并允许农民通过加速度计和陀螺仪传感器监控作物，通过网络发送几乎实时的数据。

## 零售

TinyML 已被零售行业采纳，用于监控库存，当库存低并需要重新订购时发送警报。

## 医疗

TinyML 还可以用于实时健康监测设备，为患者提供更好、更个性化的护理。例如，助听器是电池供电的硬件，使用低功耗微控制器，意味着它们需要最少的资源来有效运行。利用 TinyML，研究人员能够在不损失性能的情况下减少这些设备的延迟。

未来，实现 TinyML 设备可能会扩展到所有行业，帮助企业管理财务、处理发票、与客户更好地[合作](https://www.freshbooks.com/client-management-software)、收集和分析数据等。

## 制造

TinyML 可用于推动预测性维护工具，帮助减少任何停机时间和因设备停运而产生的额外成本。

# 结论

TinyML 在使用物联网 (IoT) 设备的各个行业中越来越受欢迎。这是因为 TinyML 模型可以在微控制器上运行，以提供特定功能，同时使用非常少的电力。

尽管是低功耗解决方案，TinyML 设备可以低延迟地处理数据，而无需互联网连接。这种缺乏网络连接的特点也有助于保护收集的数据免受黑客攻击。

TinyML 已被农业、制造业和医疗行业有效使用，其受欢迎程度预计将进一步增长。

**[Nahla Davies](http://nahlawrites.com/)** 是一名软件开发人员和技术作家。在全职从事技术写作之前，她曾在一家 Inc. 5000 级体验品牌机构担任首席程序员，该机构的客户包括三星、时代华纳、Netflix 和索尼。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 更多相关内容

+   [免费麻省理工课程：TinyML 和高效深度学习计算](https://www.kdnuggets.com/free-mit-course-tinyml-and-efficient-deep-learning-computing)

+   [为什么 DuckDB 越来越受欢迎？](https://www.kdnuggets.com/2023/07/duckdb-getting-popular.html)

+   [成为数据优先型企业的好处](https://www.kdnuggets.com/2022/07/benefits-becoming-datafirst-enterprise.html)

+   [成为科技行业专家的终极路线图](https://www.kdnuggets.com/the-ultimate-roadmap-to-becoming-specialised-in-the-tech-industry)

+   [企业中的机器学习：应用案例与挑战](https://www.kdnuggets.com/2022/08/dss-machine-learning-enterprise-cases-challenges.html)

+   [NoSQL 数据库及其应用案例](https://www.kdnuggets.com/2023/03/nosql-databases-cases.html)
