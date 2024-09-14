# SlangSD：俚语词汇的情感词典

> 原文：[https://www.kdnuggets.com/2016/09/slangsd-sentiment-dictionary-slang-words.html](https://www.kdnuggets.com/2016/09/slangsd-sentiment-dictionary-slang-words.html)

**作者：梁武、弗雷德·莫斯塔特、刘欢，亚利桑那州立大学。**

人们使用“OMG”和“LOL”等网络俚语词汇来表达他们的感受。识别俚语情感词汇可以在准确发现推文和客户评论中隐藏的情感方面带来极大的优势。为了促进用户生成内容中的情感分析，ASU DMML 发布了一个专门针对网络俚语的情感词典——SlangSD。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速开启网络安全职业生涯

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织中的 IT

* * *

SlangSD 是第一个针对俚语词汇的情感词典，提供超过 90,000 个俚语词汇/短语及其情感得分，范围从“强烈负面”、“负面”到“中性”、“正面”和“强烈正面”。我们从 Urban Dictionary ([www.urbandictionary.com](http://www.urbandictionary.com/)) 收集俚语词汇，该词典是最大的众包在线俚语词典。情感得分是根据现有情感词汇、词汇间相似性以及社交媒体中的词汇使用自动估算的。

**下载、集成并使用**

作为一个附加词典，SlangSD 可以下载 ([slangsd.com/data/SlangSD.zip](http://slangsd.com/data/SlangSD.zip)) 并轻松与现有情感分析项目集成。

![Slang SD](../Images/21883d81980ad46ed9559944d90b9ee2.png)

**扩展并构建你自己的词典**

SlangSD 建立在 Urban Dictionary、社交媒体内容和现有情感词典之上，整个构建过程完全自动化。因此，其他研究人员很容易扩展词典或构建自己的“SlangSD”。生成过程包括以下两个步骤：

+   从 Urban Dictionary 收集俚语词汇和短语。

+   通过以下方式计算俚语词汇的情感得分：

    +   参考现有的情感词典；

    +   交叉引用与推文中共现的情感词汇；

    +   使用用户提供的同义词进行估算。

关于我们如何构建 SlangSD 以及 SlangSD 如何改进现有情感分析工具的结果，请参阅报告 [arxiv.org/abs/1608.05129](https://arxiv.org/abs/1608.05129)。

**相关：**

+   [2016 年里约奥运会在 Twitter 上：积极情绪（75%），水上运动，西蒙·拜尔斯获胜](/2016/08/rio-olympics-twitter-sentiment.html)

+   [用 Python 矿工 Twitter 数据 第 6 部分：情感分析基础](/2016/07/mining-twitter-data-python-part-6.html)

+   [选举中的开放数据：利用可视化和图形发现分析进行选民教育和公民参与](/2016/04/open-data-elections-using-visualization-graphical-discovery-analysis.html)

### 更多相关内容

+   [Python 中的情感分析：超越词袋模型](https://www.kdnuggets.com/sentiment-analysis-in-python-going-beyond-bag-of-words)

+   [数据科学、统计学和机器学习词典](https://www.kdnuggets.com/2022/05/data-science-statistics-machine-learning-dictionary.html)

+   [如何更新 Python 字典](https://www.kdnuggets.com/2023/02/update-python-dictionary.html)

+   [如何收集客户情感分析的数据](https://www.kdnuggets.com/2022/12/collect-data-customer-sentiment-analysis.html)

+   [使用同态加密对加密数据进行情感分析](https://www.kdnuggets.com/2022/12/zama-sentiment-analysis-encrypted-data-homomorphic-encryption.html)

+   [如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)
