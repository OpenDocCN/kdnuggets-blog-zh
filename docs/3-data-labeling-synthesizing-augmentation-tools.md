# 3 个数据获取、注释和增强工具

> 原文：[https://www.kdnuggets.com/2021/08/3-data-labeling-synthesizing-augmentation-tools.html](https://www.kdnuggets.com/2021/08/3-data-labeling-synthesizing-augmentation-tools.html)

[评论](#comments)

快速获取用于测试算法和构建模型的数据集已成为现代数据科学家和机器学习工程师的重要技能。将一个半完整的数据集转变为完全注释、充实了额外增强数据并准备好进行模型构建的完全数据集同样重要。

对于那些拥有不完整数据并需要帮助将其从“尚未完成”变为“可以开始”的人，这里有 3 个在 GitHub 上找到的项目，可以帮助您完成数据获取、注释和增强任务。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google Cybersecurity Certificate](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google Data Analytics Professional Certificate](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT Support Professional Certificate](https://www.kdnuggets.com/google-itsupport) - 支持您在 IT 领域的组织

* * *

### 1\. Google Images Download

需要为您的项目构建自定义图像数据集吗？[**Google Images Download**](https://github.com/hardikvasa/google-images-download) 可能能够提供帮助。

> Python 脚本可从 'Google Images' 下载数百张图像。这是一个可以直接运行的代码！

将您的需求输入命令行中或从 Python 脚本中调用，Google Images Download 将... 下载 Google 图像。阅读文档 [这里](https://google-images-download.readthedocs.io/en/latest/index.html) 进行优秀的抓取，但请记住在 Google 使用条款和版权法的范围内进行。

![Image](../Images/9f071a86f804212100be53e69e7116c4.png)

### 2\. LabelImg

一旦您获得了一些图像，注释可能会成为一个问题。这时，[**LabelImg**](https://github.com/tzutalin/labelImg) 就派上用场了。

> LabelImg 是一个图形图像注释工具，用于标记图像中的对象边界框。

使用 Python 和 Qt 构建的 LabelImg 是一个独立的应用程序，具有图形用户界面，旨在对图像进行注释。

![LabelImg](../Images/3e663d2eb5a66f52d8452a619ecb9e85.png)

> 注释以 XML 文件形式保存，采用 PASCAL VOC 格式，这种格式被 [ImageNet](http://www.image-net.org/) 使用。此外，它还支持 YOLO 和 CreateML 格式。

[GitHub 仓库](https://github.com/tzutalin/labelImg)提供了安装和使用信息，LabelImg 似乎是一个你可以快速启动并使用的应用程序，从而最大限度地利用你在数据集上的工作时间。祝标注愉快！

### 3\. TextAttack

将我们的注意力从计算机视觉数据集任务转向自然语言处理，让我们看看[**TextAttack**](https://github.com/QData/TextAttack)能做什么。

> TextAttack 是一个用于对抗攻击、数据扩充和 NLP 模型训练的 Python 框架。

TextAttack 提供了多种[内置配方](https://github.com/QData/TextAttack#augmenting-text-textattack-augment)来扩充文本数据，包括：用 WordNet 同义词替换单词；用嵌入空间中的邻近词替换单词；替换、删除、插入和交换相邻字符；缩写/扩展以及替换名称、地点、数字等。

尽管在这里我们突出了使用 TextAttack 扩充数据集的用例，但其功能的完整范围如下：

> +   **通过对 NLP 模型进行不同的对抗攻击并检查输出**，更好地理解 NLP 模型
> +   
> +   **使用 TextAttack 框架和组件库研究和开发不同的 NLP 对抗攻击**
> +   
> +   **扩充你的数据集**以提高模型的泛化能力和下游的鲁棒性
> +   
> +   **仅使用单个命令训练 NLP 模型**（所有下载已包含！）

![TextAttack](../Images/f507b72590781ab46bf5d0104f448f77.png)

要开始扩充你的 NLP 数据集，或利用 TextAttack 的其他功能，请查看完整文档，你可以在[这里](https://textattack.readthedocs.io/)找到。

**相关**：

+   [敏捷数据标注：它是什么以及为何需要它](/2021/08/agile-data-labeling.html)

+   [热门深度学习 GitHub 仓库](/2019/02/trending-top-deep-learning-github-repositories.html)

+   [GitHub Copilot 开源替代方案](/2021/07/github-copilot-open-source-alternatives-code-generation.html)

### 更多相关话题

+   [IT 员工扩充：AI 如何改变软件开发行业](https://www.kdnuggets.com/2023/05/staff-augmentation-ai-changing-software-development-industry.html)

+   [关于语义分割注释的误解](https://www.kdnuggets.com/2022/01/misconceptions-semantic-segmentation-annotation.html)

+   [快速指南：如何找到合适的注释人员](https://www.kdnuggets.com/2022/04/quick-guide-find-right-minds-annotation.html)

+   [边界框深度学习：视频注释的未来](https://www.kdnuggets.com/2022/07/bounding-box-deep-learning-future-video-annotation.html)

+   [封闭源代码与开源图像注释](https://www.kdnuggets.com/closed-source-vs-open-source-image-annotation)

+   [2022 年及以后顶级 AI 和数据科学工具与技术](https://www.kdnuggets.com/2022/03/nvidia-0317-top-ai-data-science-tools-techniques-2022-beyond.html)
