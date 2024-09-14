# 数据验证和数据核查 – 从词典到机器学习

> 原文：[https://www.kdnuggets.com/2021/03/data-validation-data-verification-dictionary-machine-learning.html](https://www.kdnuggets.com/2021/03/data-validation-data-verification-dictionary-machine-learning.html)

[评论](#comments)

**由 [Aditya Aggarwal](https://www.linkedin.com/in/aditya-agarwal-2395076/)，高级分析实践负责人，和 [Arnab Bose](https://www.linkedin.com/in/arnab-bose-phd-6369531/)，首席科学官，Abzooba**

通常，当我们谈论数据质量时，我们会将数据验证和数据验证互换使用。然而，这两个术语是不同的。本文将从4个不同的背景理解它们的区别：

1.  验证和验证的词典含义

1.  一般来说，数据验证和数据验证之间的区别

1.  从软件开发的角度来看，验证与验证的区别

1.  从机器学习的角度来看，数据验证和数据核查之间的区别

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT需求

* * *

### **1\. 验证和验证的词典含义**

表1解释了验证和验证这两个词的词典含义以及一些例子。

*表1：验证和验证的词典含义*

![Table](../Images/2162d1e6b2808a0481ce18d45bce1da3.png)

*总结来说，验证关注的是真实性和准确性，而验证关注的是支持观点的强度或主张的正确性。验证检查方法的正确性，而验证检查结果的准确性。*

### **2\. 数据验证和数据核查的一般区别**

现在我们理解了这两个词的字面意思，让我们深入探讨“数据验证”和“数据核查”之间的区别。

+   **数据验证：**确保数据的准确性。

+   **数据验证：**确保数据的正确性。

让我们通过表2中的例子来详细说明。

*表2：“数据验证”和“数据验证”的例子*

![Image](../Images/cbf5378988f1b4ce4587818cf85c2b35.png)

### **3\. 从软件开发的角度来看验证与验证的区别**

*从软件开发的角度来看，*

+   *验证是为了确保软件质量高、工程设计良好、健壮且无错误，而不涉及其可用性。*

+   验证是为了确保软件的可用性和满足客户需求的能力。

![图](../Images/74edceaf163bf8c34b86072e1f9f67d8.png)

图 1：软件开发中验证与验证的区别 ([来源](https://www.easterbrook.ca/steve/2010/11/the-difference-between-verification-and-validation/))

如图 1 所示，正确性证明、鲁棒性分析、单元测试、集成测试等都是**验证**步骤，这些任务旨在验证具体的内容。软件输出会与期望的输出进行核对。另一方面，模型检查、黑箱测试、可用性测试等都是**验证**步骤，这些任务旨在了解软件是否满足需求和期望。

### **4\. 从机器学习角度看数据验证与数据验证的区别**

**数据验证**在机器学习流程中的角色是作为守门员。它**确保数据的准确性和及时更新**。数据验证主要在新数据获取阶段进行，即在机器学习流程的第 8 步，如图 2 所示。该步骤的示例包括识别重复记录并进行去重，以及清理客户信息中的不匹配，如地址或电话号码。

另一方面，**数据验证**（在机器学习流程的第 3 步）确保从第 8 步添加到学习数据中的增量数据质量良好，并且（从统计属性角度看）与现有的训练数据类似。例如，这包括**发现数据异常**或检测**现有训练数据与新数据之间的差异**。否则，增量数据中的任何数据质量问题/统计差异可能被忽视，从而导致**训练错误可能随着时间的推移积累**，并**降低模型准确性**。因此，数据验证在早期阶段检测**增量训练数据中的显著变化（如有）**，有助于根本原因分析。

![图](../Images/47a1d3f16b6b1a259ca15944fd16445c.png)

图 2：机器学习流程的组成部分

**[Aditya Aggarwal](https://www.linkedin.com/in/aditya-agarwal-2395076/)** 在 Abzooba Inc. 担任数据科学实践主管。Aditya 拥有超过 12 年的经验，通过数据驱动的解决方案推动业务目标，专注于预测分析、机器学习、商业智能及业务策略，涉及多个行业。

**[阿尔纳布·博斯博士](https://www.linkedin.com/in/arnab-bose-phd-6369531/)** 是数据分析公司 Abzooba 的首席科学官，并且是芝加哥大学的兼职教师，教授机器学习与预测分析、机器学习运维、时间序列分析与预测、以及健康分析等课程。他是一位拥有 20 年预测分析行业经验的专家，喜欢利用非结构化和结构化数据来预测并影响医疗保健、零售、金融和运输领域的行为结果。他目前的关注领域包括利用机器学习进行健康风险分层和慢性病管理，以及机器学习模型的生产部署和监控。

**相关：**

+   [MLOps – “为什么需要？”和“它是什么”？](/2020/12/mlops-why-required-what-is.html)

+   [我的机器学习模型没有学习。我该怎么办？](/2021/02/machine-learning-model-not-learn.html)

+   [数据可观测性，第二部分：如何使用 SQL 构建自己的数据质量监控工具](/2021/02/data-observability-part-2-build-data-quality-monitors-sql.html)

### 更多相关主题

+   [数据科学、统计学与机器学习词典](https://www.kdnuggets.com/2022/05/data-science-statistics-machine-learning-dictionary.html)

+   [如何更新 Python 字典](https://www.kdnuggets.com/2023/02/update-python-dictionary.html)

+   [通过链式验证解锁可靠生成：A…](https://www.kdnuggets.com/unlocking-reliable-generations-through-chain-of-verification)

+   [MarshMallow：最甜美的 Python 数据序列化与验证库](https://www.kdnuggets.com/marshmallow-the-sweetest-python-library-for-data-serialization-and-validation)

+   [用于 PySpark 应用的数据验证](https://www.kdnuggets.com/2023/08/data-validation-pyspark-applications-pandera.html)

+   [Pydantic 教程：让 Python 数据验证变得简单](https://www.kdnuggets.com/pydantic-tutorial-data-validation-in-python-made-simple)
