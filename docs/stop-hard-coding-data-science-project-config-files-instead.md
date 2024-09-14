# 停止在数据科学项目中硬编码 – 改用配置文件

> 原文：[https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html](https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html)

# 问题

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织在 IT 方面

* * *

在你的数据科学项目中，某些值往往会频繁变化，如文件名、选择的特征、训练-测试拆分比例以及模型的超参数。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/cefba5d985ec134fe74d605c3ce2058e.png)

在编写用于假设测试或演示目的的临时代码时，硬编码这些值是可以的。然而，随着代码库和团队的扩展，避免硬编码变得至关重要，因为这可能会导致各种问题：

+   **可维护性**：如果值散布在代码库的各个地方，一致地更新这些值就会变得更加困难。这可能会导致在值需要更新时出现错误或不一致。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/a819e669d53b3efcbcdd3bc1098bacaa.png)

+   **可重用性**：硬编码的值限制了代码在不同场景下的重用性。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/75cfa4101df5d9f497bdb8971e892093.png)

+   **安全问题**：将敏感信息如密码或 API 密钥直接硬编码到代码中可能存在安全风险。如果代码被分享或暴露，可能会导致未经授权的访问或数据泄露。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/d1028157ab86b81b450243c9e11bd062.png)

+   **测试和调试**：硬编码的值会使测试和调试变得更加困难。如果值被硬性嵌入到代码中，那么模拟不同的场景或有效地测试边界情况就会变得困难。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/c15f80670f1e33cb1f7cc2762e9dfe23.png)

# 解决方案 – 配置文件

配置文件通过提供以下好处来解决这些问题：

+   **将配置与代码分离**：配置文件允许你将参数与代码分开存储，这样可以提高代码的可维护性和可读性。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/3a2cf21b8c9a7c7b6db102109c10aef0.png)

+   **灵活性和可修改性**：使用配置文件，你可以轻松修改项目配置而不需要修改代码本身。这种灵活性允许快速实验、参数调整，并使项目适应不同的场景或环境。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/e3ef1c96c129aa9ed89b90a2df361bc4.png)

+   **版本控制**：将配置文件存储在版本控制系统中，可以跟踪配置的变化。这有助于维护项目配置的历史记录，并促进团队成员之间的协作。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/3b50a337a4b2b05a514700daa767d06b.png)

+   **部署和生产化**：在将数据科学项目部署到生产环境时，配置文件可以轻松定制特定于生产环境的设置，而无需修改代码。这种将配置与代码分离的方法简化了部署过程。

# Hydra 介绍

在众多用于创建配置文件的 Python 库中，[Hydra](https://hydra.cc/) 是我首选的配置管理工具，因为它具备令人印象深刻的一整套功能，包括：

+   方便的参数访问

+   命令行配置覆盖

+   从多个来源组合配置

+   执行多个具有不同配置的作业

让我们更深入地探讨这些功能。

随意查看和分叉本文的源代码：

[在 GitHub 上查看](https://github.com/khuyentran1401/hydra-demo)

## 方便的参数访问

假设所有配置文件存储在 `conf` 文件夹下，所有 Python 脚本存储在 `src` 文件夹下。

```py
.
├── conf/
│   └── main.yaml
└── src/
    ├── __init__.py
    ├── process.py
    └── train_model.py
```

`main.yaml` 文件看起来是这样的：

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/dd2082f71143219ff4241c390c733574.png)

在 Python 脚本中访问配置文件就像给你的 Python 函数应用一个装饰器一样简单。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/365163d28b5863e81258ebe08c4dec98.png)

要从配置文件中访问特定参数，我们可以使用点符号（例如，`config.process.cols_to_drop`），这种方式比使用括号（例如，`config['process']['cols_to_drop']`）更简洁直观。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/d0b832c5320a36999a8d2b811b449441.png)

这种简单的方法让你可以轻松地获取所需的参数。

## 命令行配置覆盖

假设你正在尝试不同的 `test_size`。反复打开配置文件并修改 `test_size` 值是耗时的。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/092d6c03e28cdd7e3241243fa909eb84.png)

幸运的是，Hydra 使得从命令行直接覆盖配置变得容易。这种灵活性允许快速调整和微调，而无需修改基础配置文件。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/c149dc88b1784ab4a7b39e90812ba3ba.png)

## 从多个来源组合配置

想象一下，你想尝试各种数据处理方法和模型超参数的组合。虽然你可以在每次运行新实验时手动编辑配置文件，但这种方法可能会很耗时。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/342d72099529870736080ee9e9259ef6.png)

Hydra 允许通过配置组从多个来源组合配置。要创建一个用于数据处理的配置组，创建一个名为`process`的目录，其中包含每种处理方法的文件：

```py
.
└── conf/
    ├── process/
    │   ├── process1.yaml
    │   └── process2.yaml
    └── main.yaml
```

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/a657e1e81facaa957ff2e82307a16410.png)

如果你想将`process1.yaml`文件设置为默认文件，请将其添加到 Hydra 的默认列表中。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/c6e0ebfe72a906ffc1587f5e3e5b37c5.png)

按照相同的步骤创建一个用于训练超参数的配置组：

```py
.
└── conf/
    ├── process/
    │   ├── process1.yaml
    │   └── process2.yaml
    ├── train/
    │   ├── train1.yaml
    │   └── train2.yaml
    └── main.yaml
```

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/3004a5a767edfdaeb9d9c2683b32f402.png)

将`train1`设置为默认配置文件：

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/20d582a2dff3abcb68e37f6f61e5d037.png)

现在，运行应用程序时默认将使用`process1.yaml`文件和`model1.yaml`文件中的参数：

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/1636d5af64af907827b84a8a37ffab6c.png)

这一能力在需要无缝结合不同配置文件时特别有用。

## 多次运行

假设你想进行多种处理方法的实验，一个一个地应用每种配置可能会非常耗时。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/6f006115d6aa0ce5f9d08a11fc317f43.png)

幸运的是，Hydra 允许你同时用不同的配置运行相同的应用程序。

![停止在数据科学项目中硬编码 - 改用配置文件](../Images/4caa4645b70b6c8c6381d3b1073444a1.png)

这种方法简化了用各种参数运行应用程序的过程，*最终节省了宝贵的时间和精力*。

# 结论

恭喜！你刚刚了解了使用配置文件的重要性以及如何使用 Hydra 创建配置文件。我希望这篇文章能为你提供创建自己配置文件所需的知识。

**[Khuyen Tran](https://www.linkedin.com/in/khuyen-tran-1401/)** 是一位多产的数据科学作家，撰写了[一系列令人印象深刻的有用数据科学主题，包括代码和文章](https://github.com/khuyentran1401/Data-science)。Khuyne 目前在 2022 年 5 月之后寻求机器学习工程师、数据科学家或开发者倡导者的职位，欢迎联系她，如果你在寻找拥有她技能的人才。

[原文](https://mathdatasimplified.com/2023/05/25/stop-hard-coding-in-a-data-science-project-use-configuration-files-instead/)。经许可转载。

### 更多相关主题

+   [数据科学中的 3 道困难 Python 编码面试题](https://www.kdnuggets.com/2023/03/3-hard-python-coding-interview-questions-data-science.html)

+   [为何应使用线性回归模型而非…的 3 个理由](https://www.kdnuggets.com/2021/08/3-reasons-linear-regression-instead-neural-networks.html)

+   [忘记 PIP、Conda 和 requirements.txt！改用 Poetry 吧，稍后会感激…](https://www.kdnuggets.com/2023/07/forget-pip-conda-requirementstxt-poetry-instead-thank-later.html)

+   [免费 Python 项目编码课程](https://www.kdnuggets.com/2022/08/free-python-project-coding-course.html)

+   [想成为数据科学家？第 1 部分：你需要的 10 个硬技能](https://www.kdnuggets.com/want-to-become-a-data-scientist-part-1-10-hard-skills-you-need)

+   [进入 FAANG 公司有多难](https://www.kdnuggets.com/2023/05/hard-get-faang-companies.html)
