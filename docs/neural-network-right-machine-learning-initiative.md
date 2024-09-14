# 如何知道神经网络是否适合您的机器学习计划

> 原文：[`www.kdnuggets.com/2020/11/neural-network-right-machine-learning-initiative.html`](https://www.kdnuggets.com/2020/11/neural-network-right-machine-learning-initiative.html)

评论

**作者：Frank Fineis，Avatria 的首席数据科学家 [Avatria](http://www.avatria.com/)**

![Image](img/144a940379294e3e3f816ae0f54df5a4.png)

* * *

## 我们的前三名课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您组织的 IT

* * *

深度学习模型（即神经网络）现在驱动着从自动驾驶汽车到 YouTube 推荐视频的所有事物，在过去几年中变得非常流行。尽管它们很受欢迎，但该技术也有一些缺点，例如深度学习的“[可重复性危机](https://www.wired.com/story/artificial-intelligence-confronts-reproducibility-crisis/)”，因为一个研究人员很常无法重现另一研究人员发布的一组结果，即使在相同的数据集上。此外，深度学习的高昂成本会让任何公司感到迟疑，因为[FAANG](https://en.wikipedia.org/wiki/Big_Tech#FAANG)公司已花费[超过 $30,000](https://medium.com/syncedreview/the-staggering-cost-of-training-sota-ai-models-e329e80fa82)来训练一个（非常）深的网络。即使是全球最大的科技公司也在神经网络的规模、深度和复杂性上遇到困难，而对较小的数据科学组织来说，这些问题更加明显，因为神经网络可能既费时又昂贵。此外，神经网络并不一定能超越基准模型，如逻辑回归或梯度提升模型，因为神经网络难以调优，通常需要额外的数据和工程复杂性。

鉴于这些关注点，重要的是要记住，即使考虑神经网络也必须有一个**商业原因**，而不是因为高管们感到害怕错过（FOMO）。

在考虑神经网络的需求时，将数据视为三种类型是有用的：

1.  连续变量，由数值和小数点值组成。

1.  类别变量，提供有限数量的可能值，通常具有语义。

1.  文本序列，提供非结构化的语义信息。

基于决策树的算法不是处理文本序列或具有多种不同值的分类变量的最有效方法，这可能要求公司采用创造性的方式将这些值编码为模型的数值特征。这也可能意味着*大量*的手动特征工程工作，因为这种方法在潜在值数量超过少量时会增加模型管道的复杂性。

神经网络通常更容易学习所谓的“稀疏特征”，这也是任何考虑涉足深度学习的组织在此之前可能想要考虑的建议：

1.  尽量坚持使用预构建的即插即用解决方案（至少在开始时），例如已经与[TensorFlow](https://www.tensorflow.org/)预组装好的模型。尽管追求令人兴奋的新论文并尝试追求绝对的 SOTA 很有诱惑力，但如果模型不是来自具有一定常规用户的包（例如 GitHub/GitLab 上的用户），它可能无法直接适应你的数据集。

1.  避免实现任何没有附带代码的算法。[Paperswithcode.com](https://paperswithcode.com/) 跟踪深度学习/AI 领域的 SOTA，并方便地链接描述神经网络架构及其在 GitHub 上的实现的论文。如果你的团队考虑基于一篇没有相应开源实现的论文（例如谷歌的[seq2slate](https://paperswithcode.com/paper/seq2slate-re-ranking-and-slate-optimization)神经网络架构论文）进行建模，那就不要尝试。这将非常困难且耗时，并且可能意味着很少有人实际使用论文所提议的内容。

1.  如果你没有看到立竿见影的结果，千万不要放弃！我们无法过分强调保持配置和结果精确记录的价值。可能需要调整数十个超参数，其中一些对结果的影响很小，但一些会极大地改变结果。仔细关注你选择的优化器和学习率——这组合能决定训练是否会取得零进展、少量进展或大量进展。一个好的经验法则是从非常小的学习率开始，检查训练期间的损失是否下降。

1.  使用仅有的 CPU 训练神经网络可能需要很长时间，但使用 GPU（图形处理单元）可以将训练速度提高 10 倍。通过 [Google Colab](https://colab.research.google.com/notebooks/gpu.ipynb) 获得免费的 GPU 访问权限，它方便地预装了最新版本的 TensorFlow。在评估新模型时，你的数据科学团队通常应在自己的 VPC 上进行特征提取和特征工程，然后将训练集上传到 Google Drive，以便在 Colab 笔记本中训练模型。这可以降低成本，并避免额外的基础设施维护。最后，使用 TensorFlow 的 tensorboard 工具，数据科学家可以直接从云端研究模型训练结果，并比较所有不同的实验。

1.  编写可重用的代码。很容易调整神经网络的多个超参数，或者以微妙的方式操控训练集，这可能会极大地影响模型结果，并且在笔记本开发环境中可能会丢失导致这些结果的设置。但维护中心数据集创建和模型训练脚本可以让你记录最佳设置。

**简介： [Frank Fineis](https://www.linkedin.com/in/frank-fineis-41770277/)** 是 [Avatria](http://www.avatria.com/) 的首席数据科学家，该公司是一家数字商务公司，专注于开发电子商务解决方案。该公司构建的数据驱动产品利用机器学习为其 B2C 和 B2B 客户的电子商务需求提供可操作的见解。

**相关：**

+   神经网络能展现想象力吗？DeepMind 认为可以

+   黑箱内部观察：如何欺骗神经网络

+   深度学习、自然语言处理和计算机视觉的顶级 Python 库

### 相关话题

+   [RedPajama 项目：一个开源倡议以民主化 LLMs](https://www.kdnuggets.com/2023/06/redpajama-project-opensource-initiative-democratizing-llms.html)

+   [使用 TensorFlow 和 Keras 构建和训练你的第一个神经网络](https://www.kdnuggets.com/2023/05/building-training-first-neural-network-tensorflow-keras.html)

+   [通过构建 15 个神经网络项目来学习深度学习（2022 年）](https://www.kdnuggets.com/2022/01/15-neural-network-projects-build-2022.html)

+   [使用 AIMET 进行神经网络优化](https://www.kdnuggets.com/2022/04/qualcomm-neural-network-optimization-aimet.html)

+   [使用 PyTorch 构建卷积神经网络](https://www.kdnuggets.com/building-a-convolutional-neural-network-with-pytorch)

+   [神经网络预测中置换的重要性](https://www.kdnuggets.com/2022/12/importance-permutation-neural-network-predictions.html)
