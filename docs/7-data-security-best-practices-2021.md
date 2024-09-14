# 2021 年七个数据安全最佳实践

> 原文：[`www.kdnuggets.com/2021/06/7-data-security-best-practices-2021.html`](https://www.kdnuggets.com/2021/06/7-data-security-best-practices-2021.html)

评论![图示](img/088cb9b6fdefe1da3ec95df04866f1a6.png)

Pixabay 提供的照片来自 Pexels

数据安全是任何公司面临的日益严重的问题，因为网络犯罪持续繁荣。由于数据科学家的整个工作都围绕着潜在的敏感数据，他们面临的压力比大多数人都大。如果你遭遇数据泄露，除了可能的财务和声誉损失外，你还可能无法履行工作职责。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 为你的组织提供 IT 支持

* * *

IBM 估计，[数据泄露的平均成本为 386 万美元](https://www.ibm.com/security/data-breach)。根据事件的严重程度，它还可能影响你的声誉，甚至你的工作。考虑到这一点，以下是今年需要采纳的七个数据安全最佳实践。

### **1\. 仅使用所需的内容**

保护数据的最佳方法之一是最小化你存储的内容。虽然收集尽可能多的数据很有诱惑力，特别是在训练机器学习模型时，但这会使你更容易受到攻击。你只能失去你拥有的东西，所以检查你的数据库，删除任何不必要的内容。

这一原则的一部分是保持手头数据的最新记录。如果你发现不再需要某些信息，请从数据库中清除它。保留遗留数据对你没有帮助，只会意味着你有更多的损失风险。

### **2\. 掩码敏感数据**

“仅使用所需的内容”原则同样适用于你存储的数据类型。许多数据科学操作不需要特定于用户的信息，所以如果你不需要，就不要存储。如果必须使用诸如个人身份标识符等敏感数据，你应该对其进行掩码处理。

掩码敏感数据的最常见方法之一是使用替换密码。令牌化，[用虚拟数据替换真实值](https://cloud.google.com/architecture/sensitive-data-and-ml-datasets#masking_sensitive_data)是另一个选项，通常更安全，因为它将加密值放在单独的数据库中。无论你使用哪种方法，确保首先清除所有不必要的敏感信息。

### **3\. 小心合作**

数据科学通常是一个协作过程，您应考虑如何与合作者沟通。虽然电子邮件可能方便，但[默认情况下并不加密](https://www.kaseware.com/cybersecurity-awareness-month/)，因此不适合用于共享数据或数据库访问凭证。有许多专门用于敏感文件共享的服务，因此它们是更好的选择。

无论与谁合作，都应尽量保持信任的最低限度。人们只能访问对其工作至关重要的信息。如果可能，您甚至可以考虑在共享信息前对其进行模糊处理，以减轻潜在泄露的影响。

### **4\. 尽可能多地加密**

当您分享数据时，应加密数据。数据存储在数据库中时也应加密。尽管加密不能解决所有安全问题，但它是增加保护层的低成本方法。

许多[最佳数据加密工具](https://rehack.com/featured/the-best-data-encryption-software-of-2020/)今天也不会显著拖慢您的处理速度。查看您的选项，找到能在所有场景中加密静态和动态数据的工具。虽然这不会完全阻止数据泄露，但可以减轻其成本。

### **5\. 不仅仅保护数据库**

记住，安全不仅仅适用于您存储数据的位置。数据库应该是您最关注的区域，但它们不应该是您唯一关心的地方。备份、连接的应用程序和分析服务器都可能成为数据的后门，因此它们也需要保护。

任何触及您数据的程序、驱动器或文件都应确保安全。在此过程中，当数据连接较少时，工作会更容易。减少对数据库的访问权能使工作更轻松，并提供更多保护。

### **6\. 小心第三方云供应商**

如果您使用像 AWS 这样的第三方云服务，务必不要对安全感到自满。不幸的是，许多用户会如此，最近的一项研究显示[82% 的公司](https://www.wiz.io/blog/82-of-companies-unknowingly-give-3rd-parties-access-to-all-their-cloud-data)给予这些供应商高度权限。第三方云服务本身并不危险，但您需要将安全放在首位。

检查您的权限，确保只给予供应商和其他应用程序最低限度的权限。使用强密码，包括多因素认证，并定期更换这些密码。如果您不知道该怎么办，许多这些供应商[提供安全最佳实践](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)供您参考。

### **7\. 建立明确的治理政策**

最后，你应该为你的整个团队建立一个清晰而具体的治理政策。拥有一份关于应该做什么和不应该做什么的书面文件，将有助于确保安全的用户行为。如果有人犯了危害安全的错误，你可以参考政策来查看哪里出了问题。

你的治理政策应该明确每个人在安全方面的角色。你可以设立轮班制度来监控和记录进出数据。你也可以给每个人分配一个固定角色。无论你做什么，都要具体明确，并确保每个人都理解。

### **数据科学安全必须改进**

数据科学在当今商业中扮演着越来越核心的角色。随着这一趋势的持续，你的工作也成为了网络犯罪分子的更有价值的目标。数据科学团队必须在这些日益增长的威胁面前拥抱安全。

从这些最佳实践开始，然后寻找其他更小的领域来提高安全性。当你的数据安全时，你可以自信地工作，并给潜在客户留下深刻印象。

**个人简介：德文·帕尔蒂达**是一位大数据和技术作家，同时也是[**ReHack.com**](https://rehack.com/)的主编。

**相关：**

+   自动化如何改善数据科学家的角色

+   数据科学家开发了一种更快的减少污染和减少温室气体排放的方法

+   数据专业人士如何为简历增加更多变化

### 了解更多相关话题

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)

+   [将 ChatGPT 集成到数据科学工作流中的技巧和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [数据仓库和 ETL 的最佳实践](https://www.kdnuggets.com/2023/02/data-warehousing-etl-best-practices.html)

+   [迁移到 AWS 云的 11 种最佳实践](https://www.kdnuggets.com/2023/04/11-best-practices-cloud-data-migration-aws-cloud.html)

+   [数据可视化最佳实践与有效沟通的资源](https://www.kdnuggets.com/2023/04/data-visualization-best-practices-resources-effective-communication.html)

+   [数据科学团队协作的 5 个最佳实践](https://www.kdnuggets.com/2023/06/5-best-practices-data-science-team-collaboration.html)
