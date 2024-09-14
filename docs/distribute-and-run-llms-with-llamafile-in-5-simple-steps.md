# 使用 llamafile 分发和运行 LLMs 的 5 个简单步骤

> 原文：[https://www.kdnuggets.com/distribute-and-run-llms-with-llamafile-in-5-simple-steps](https://www.kdnuggets.com/distribute-and-run-llms-with-llamafile-in-5-simple-steps)

![使用 llamafile 分发和运行 LLMs 的 5 个简单步骤](../Images/f0ab7be9ca7fc383ce9e47655b07be97.png)

图片由作者提供

对于许多人来说，探索 LLMs 的可能性感觉遥不可及。无论是下载复杂的软件，弄懂编码，还是需要强大的机器——开始使用 LLMs 似乎都令人望而却步。但试想一下，如果我们能够像启动其他计算机程序一样轻松地与这些强大的语言模型互动。无需安装，无需编码，只需点击和对话。这种可达性对于开发者和最终用户都至关重要。llamaFile 作为一种新颖的解决方案，将 [llama.cpp](https://github.com/ggerganov/llama.cpp) 和 [Cosmopolitan Libc](https://github.com/jart/cosmopolitan) 合并成一个单一的框架。这个框架通过提供一个**称为“llama 文件”的单文件可执行文件**来简化 LLMs 的复杂性，可以在本地机器上运行而无需安装。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的IT

* * *

那么，它是如何工作的呢？llamaFile 提供了**两种便捷的方法**来运行 LLMs：

+   第一种方法涉及从 Hugging Face 下载 llamafile 的最新版本及相应的模型权重。一旦获得这些文件，你就可以开始使用了！

+   第二种方法更简单——你可以访问 [预先存在的示例 llamafiles](https://github.com/Mozilla-Ocho/llamafile?tab=readme-ov-file#other-example-llamafiles)，这些文件已内置权重。

在本教程中，你将使用第二种方法操作**LLaVa 模型**的 llamafile。它是一个 70 亿参数的模型，被量化为 4 位，你可以通过聊天、上传图片和提问与之互动。其他模型的示例 llamafiles 也可用，但我们将使用 LLaVa 模型，因为其 llamafile 大小为 3.97 GB，而 Windows 的最大可执行文件大小为 4 GB。这个过程非常简单，你可以按照下面提到的步骤运行 LLMs。

# 步骤 1：下载 llamafile

首先，你需要从提供的 [这里](https://huggingface.co/jartine/llava-v1.5-7B-GGUF/resolve/main/llava-v1.5-7b-q4.llamafile?download=true) 下载 llava-v1.5-7b-q4.llamafile (3.97 GB) 可执行文件。

# 第 2 步：授予执行权限（适用于 macOS、Linux 或 BSD 用户）

打开你计算机的终端并导航到文件所在的目录。然后运行以下命令以授予你的计算机执行此文件的权限。

```py
chmod +x llava-v1.5-7b-q4.llamafile
```

# 第 3 步：重命名文件（适用于 Windows 用户）

如果你使用 Windows，请在 llamafile 的名称末尾添加 “.exe”。你可以在终端中运行以下命令来完成这个操作。

```py
rename llava-v1.5-7b-q4.llamafile llava-v1.5-7b-q4.llamafile.exe
```

# 第 4 步：运行 llamafile

使用以下命令执行 llama 文件。

```py
./llava-v1.5-7b-q4.llamafile -ngl 9999
```

⚠️ 由于 MacOS 使用 zsh 作为默认的 shell，如果你遇到 `zsh: exec format error: ./llava-v1.5-7b-q4.llamafile` 错误，则需要执行以下操作：

```py
bash -c ./llava-v1.5-7b-q4.llamafile -ngl 9999
```

对于 Windows，你的命令可能如下所示：

```py
llava-v1.5-7b-q4.llamafile.exe -ngl 9999
```

# 第 5 步：与用户界面互动

运行 llamafile 后，它应该会自动打开你的默认浏览器并显示如下所示的用户界面。如果没有，打开浏览器并手动导航到 [http://localhost:8080](http://localhost:8080)。

![分发和运行 LLMs 的 llamafile 的 5 个简单步骤](../Images/42d390371b6887443db5df99300b6657.png)

图片由作者提供

让我们通过一个简单的问题来与界面互动，以提供一些与 LLaVa 模型相关的信息。以下是模型生成的回复：

![分发和运行 LLMs 的 llamafile 的 5 个简单步骤](../Images/86d758132c338800202b92705f4f5437.png)

图片由作者提供

该回复强调了 LLaVa 模型的开发方法及其应用。生成的回复相当迅速。让我们尝试实施另一个任务。我们将上传以下带有详细信息的银行卡样本图像，并从中提取所需的信息。

![分发和运行 LLMs 的 llamafile 的 5 个简单步骤](../Images/f2154617727a40577296a6d51bdb1cd4.png)

图片由 [Ruby Thompson](https://qph.cf2.quoracdn.net/main-qimg-664f0732755fc0ba072da4286668fef8-lq) 提供

这是回复：

![分发和运行 LLMs 的 llamafile 的 5 个简单步骤](../Images/481ad10a447dea169aa9fba0fe91b01e.png)

图片由作者提供

再次地，回复是相当合理的。LLaVa 的作者声称它在各种任务中表现出色。随意探索不同的任务，观察其成功和局限性，并亲自体验 LLaVa 的卓越表现。

一旦与你的 LLM 的互动完成，你可以通过返回终端并按 "Control - C" 来关闭 llama 文件。

# 总结

分发和运行 LLM 从未如此简单。在本教程中，我们解释了如何仅通过一个可执行的 llamafile 轻松运行和试验不同的模型。这不仅节省了时间和资源，还扩展了 LLM 的可访问性和现实世界的实用性。我们希望您发现本教程有帮助，并非常希望听到您的意见。此外，如果您有任何问题或反馈，请随时与我们联系。我们随时乐于提供帮助并重视您的意见。

感谢您的阅读！

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)**** Kanwal 是一位机器学习工程师和技术作家，对数据科学和 AI 与医学的交叉领域充满了深厚的热情。她合著了电子书《用 ChatGPT 最大化生产力》。作为 2022 年亚太地区的 Google Generation Scholar，她倡导多样性和学术卓越。她还被认可为 Teradata 多样性技术学者、Mitacs Globalink 研究学者和哈佛 WeCode 学者。Kanwal 是变革的积极倡导者，创立了 FEMCodes 以赋能 STEM 领域的女性。

### 更多相关内容

+   [如何使用 MLFlow 打包和分发机器学习模型](https://www.kdnuggets.com/2022/08/package-distribute-machine-learning-models-mlflow.html)

+   [只需几个步骤即可在您的设备上运行 Alpaca-LoRA](https://www.kdnuggets.com/2023/05/learn-run-alpacalora-device-steps.html)

+   [使用 Jupysql 和 GitHub Actions 安排和运行 ETL](https://www.kdnuggets.com/2023/05/schedule-run-etls-jupysql-github-actions.html)

+   [如何快速上手 PyTest：轻松编写和运行 Python 测试](https://www.kdnuggets.com/getting-started-with-pytest-effortlessly-write-and-run-tests-in-python)

+   [在本地使用 LM Studio 运行 LLM](https://www.kdnuggets.com/run-an-llm-locally-with-lm-studio)

+   [如何让 Python 代码运行得更快](https://www.kdnuggets.com/2021/06/make-python-code-run-incredibly-fast.html)
