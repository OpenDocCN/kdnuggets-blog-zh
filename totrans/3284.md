# 世界首个用于机器学习和人工智能的蛋白质数据库

> 原文：[https://www.kdnuggets.com/2017/06/dspp-protein-database-machine-learning-ai.html](https://www.kdnuggets.com/2017/06/dspp-protein-database-machine-learning-ai.html)

![c](../Images/3d9c022da2d331bb56691a9617b91b90.png)[评论](#comments)

**由 Peptone 创始人 Kamil Tamiola 提供**

![机器学习和人工智能的蛋白质数据库](../Images/284fe97f87be7300d6b84955e2d1c3b7.png)

我非常自豪和兴奋地介绍 [Peptone](https://peptone.io/) 的首个公开产品，[蛋白质结构倾向数据库](https://peptone.io/dspp)。

> [蛋白质结构倾向数据库 (**dSPP**)](https://peptone.io/dspp) 是世界上第一个与领先的机器学习框架 Keras 和 Tensorflow 无缝集成的蛋白质结构和动态特征的互动库。

dSPP 基于来自世界各地领先学术机构的同行评审研究，这些机构参与了用于蛋白质结构和无序特征表征的核磁共振光谱技术。dSPP 数据来自 7200 多种在生理相关条件下研究的无关蛋白质的溶液态和固态核磁共振光谱实验。

dSPP 是一个独特的信息来源，专注于固有无序蛋白质 (IDPs)，这是一类研究难度较大的蛋白质。IDPs 涉及到许多使人虚弱的病理状况，包括阿尔茨海默病、帕金森病、朊病毒病、癌症的分子基础、HIV、HSV、HVC、ZIKVR 等。

![固有无序蛋白质](../Images/622e2b7ea7ab7306d18d1e55afe9a8b8.png)

*MOAG-4 蛋白质的倾向分数的结构解释，提取自 dSPP 数据库 [https://peptone.io/dspp/entry/dSPP27058_0\](https://peptone.io/dspp/entry/dSPP27058_0)* [*MOAG-4 被认为能增强动物脑模型中的蛋白质聚集过程，从而加速帕金森病的早期发作。*](http://www.jbc.org/content/early/2017/03/23/jbc.M116.764886)

dSPP 数据可以被实验人员轻松使用，以深入了解二级结构基序的结构稳定性，以及旨在提供医学相关蛋白质现实模型的高通量计算技术。

与其他蛋白质数据集中为实验人员和机器学习社区提供的二元 (*logits*) 二级结构分配不同，dSPP 数据报告了蛋白质结构和局部动态，具有原子分辨率，基于**-1.0到1.0**的连续结构倾向分配。

![连续结构倾向](../Images/49732abe9f76228c12c2aa2a6fbb94e2.png)

dSPP 实验数据是在生理相关条件下收集的，使其在结构和无序预测方法中具有绝对的唯一性，这些方法旨在处理生物学和医学相关背景下的蛋白质折叠和稳定性。

dSPP 配备直观的用户界面，提供对相关决策数据、原始文献引用和统一呈现的感兴趣蛋白的机器学习数据的无缝访问。

通过[dspp-keras Python 包](https://github.com/PeptoneInc/dspp-keras)实现与**Keras**和**Tensorflow**机器学习框架的无缝集成，下载和设置时间不到 60 秒。因此，几乎任何对机器学习有基本了解的人都可以开始尝试蛋白质结构预测方法。

dSPP 是 Peptone 首个公开发布的产品，具有**自动化 14 天更新周期**，专为**持续学习的 AI 应用**而设计。

### 科学参考：

+   *蛋白质结构倾向数据库*。Kamil Tamiola, Matthew Michael Heberling, Jan Domanski。bioRxiv 144840；doi：*[*https://doi.org/10.1101/144840*](https://doi.org/10.1101/144840)

### 可用性和行动呼吁

+   互动搜索引擎和数据渲染：[https://peptone.io/dspp](https://peptone.io/dspp)

+   独立 JSON 和 Python cPickle 下载：[https://peptone.io/dspp/download](https://peptone.io/dspp/download)

+   Keras 和 Tensorflow 集成：[https://github.com/PeptoneInc/dspp-keras](https://github.com/PeptoneInc/dspp-keras)

+   在终端中：**pip install dspp-keras**

### 致谢

+   我们要感谢**郑文伟博士**（*NIDDK, US*）、**鲁德·谢克博士**（*格罗宁根大学, NL*）和**谢维尔·佩里奥勒博士**（*奥胡斯大学, DK*）对我们[dSPP 论文](http://biorxiv.org/content/early/2017/06/01/144840)的深刻评论和编辑建议。

+   [弗朗索瓦·肖莱](https://github.com/fchollet) 来自 [Keras / Google](https://github.com/fchollet/keras) 因对数据库接口的深刻反馈以及关于**Keras**集成的直接建议而受到高度赞赏。

+   我们衷心感谢**艾莉森·朗德斯**、**卡洛·鲁伊斯**和**亚当·格日瓦切夫斯基博士**（*NVIDIA Corporation*）促进了合作并提供了 DGX-1 超级计算机的使用。

+   **乔恩·韦德尔**（*BMRB*）因在从 BMRB 中获取 NMR 共振分配的技术支持而受到高度赞赏。

+   我们感谢**弗朗斯·A.A.·穆尔德博士**（*奥胡斯大学, DK*）和**佩德拉格·库基奇博士**（*剑桥大学, UK*）提供 MOAG-4 的结构集成模型。

+   最后，我们要特别感谢**马克·伯杰**（*NVIDIA Corporation*）在整个项目执行过程中给予的巨大支持。

### **新闻稿**

+   本新闻稿及媒体资产可从[https://drive.google.com/open?id=0B0VsF9FO3J_OMXljcm1MS3NCRHc](https://drive.google.com/open?id=0B0VsF9FO3J_OMXljcm1MS3NCRHc)下载。

### 关于 Peptone

成立于2016年（荷兰阿姆斯特丹），[Peptone](https://peptone.io/) 通过机器学习和人工智能提供先进的蛋白质生物技术解决方案。我们将来自公共和私人数据库的大数据转化为强大的预测模型和直观的工具，用于蛋白质生产、稳定性、无序性、工程和定向进化实验，为客户提供透明且互补的软件，节省时间并提供精准的研究答案。

[原始](https://www.linkedin.com/pulse/worlds-first-protein-database-machine-learning-ai-kamil-tamiola)文章。转载经许可。

**个人简介：** **[卡米尔·塔米奥拉](https://www.linkedin.com/in/ktamiola/)** 是一位企业家和研究员，拥有超算和蛋白质结构生物物理学的广泛科学背景。

**相关：**

+   [科学的大数据生态系统：基因组学](/2016/12/big-data-ecosystem-science-genomics.html)

+   [使用可视化进行高级数据分析的5个步骤](/2016/10/qlucore-5-steps-data-analysis-visualization.html)

+   [深度学习：通过XML和PMML编程TensorFlow](/2017/06/deep-learning-tensorflow-programming-xml-pmml.html)

* * *

## 我们的前3名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织IT

* * *

### 相关主题

+   [停止学习数据科学以寻找目标，并寻找目标以……](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [学习数据科学统计的最佳资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [9亿美元AI失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [构建一个可靠的数据团队](https://www.kdnuggets.com/2021/12/build-solid-data-team.html)

+   [使用管道编写干净的Python代码](https://www.kdnuggets.com/2021/12/write-clean-python-code-pipes.html)

+   [成功数据科学家的5个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)
