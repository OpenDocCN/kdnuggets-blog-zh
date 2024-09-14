# ChatGPT 是否有潜力成为新的国际象棋超级大师？

> 原文：[https://www.kdnuggets.com/does-chatgpt-have-the-potential-to-become-a-new-chess-super-grandmaster](https://www.kdnuggets.com/does-chatgpt-have-the-potential-to-become-a-new-chess-super-grandmaster)

![ChatGPT 是否有潜力成为新的国际象棋超级大师？](../Images/1f8444fcb90abcac71fca7a2190c876d.png)

编辑提供的图片

作为一名资深的前国际象棋选手（青少年冠军，ELO 2000+）和自然语言处理数据科学家，我一直计划写这篇文章。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 管理

* * *

我第一次听说 ChatGPT 下棋的能力，是从我的一位同事那里。他是博士，也是个非常聪明的人。他给我发了一个链接，可以和 ChatGPT 对弈，正如他所认为的那样。不幸的是，这并不完全是 ChatGPT，它背后是其他的国际象棋引擎。他被欺骗了。你仍然可以在这里尝试：https://parrotchess.com/

为了撰写本文，我与 ChatGPT 对弈了 2 局。以下是我们的开局：

![ChatGPT 是否有潜力成为新的国际象棋超级大师？](../Images/37647c54692ea266a2b103bbc11e5f19.png)

让我们看看发生了什么。

快速国际象棋记谱课程 / 提醒（可以跳过）：

K = 王，Q = 后，R = 车，B = 象，N = 马，0–0 = 王侧易位。0–0–0 = 后侧易位，x = 吃子。对于兵，我们只写它落到的方格，除非兵吃子。那时，我们写出兵之前所在方格的字母，以及吃掉另一子后所去方格的字母和数字。例如，exd4。

Nikola Greb 对阵 ChatGPT 4，2024年1月7日

```py
1\. e4 e5 2\. Nf3 Nc6 3\. d4 exd4 4\. Nxd4 Nf6 5\. Nc3 Bb4 6\. Nxc6 bxc6 7\. Bd3 O-O 8.
O-O d5 9\. e5 Ne4 10\. Nxe4 Bc5 11\. Nxc5 Qe7 12\. Qh5 g6 13\. Qh6 f6 14\. exf6 Qxf6
15\. Bg5 Qf7 16\. Rae1 Bf5 17\. Re7 Qxe7 18\. Bxe7 Rae8 19\. Bxf8 Rxf8 20\. Bxf5 Rf7
21\. Re1 1-0
```

直到 e5 这步棋，ChatGPT 4 的表现像是一位非常优秀的国际象棋选手。我们可以说像国际象棋大师。但当我下了一步不精确但具有攻击性的棋（exd5 是最佳走法）时，它失去了常规，犯了一个大错，走了 Ne4。

![ChatGPT 是否有潜力成为新的国际象棋超级大师？](../Images/14b67b35d6cbd9fe1d48d1b77fc83086.png)

我用马吃掉了对方的马（10\. Nxe4），第一次幻觉发生了：

![ChatGPT 是否有潜力成为新的国际象棋超级大师？](../Images/1bae8ae5e01288f9edde8639fd9e554c.png)

![ChatGPT 是否有潜力成为新的国际象棋超级大师？](../Images/895ab0f40f6b4cd9bfb0860778b78d2d.png)

Bc5 再次是一个错误，明显的失误。由于游戏的其余部分没有国际象棋的价值，我将总结一下。ChatGPT 4 指责我走了不可能的棋步，结果陷入了幻觉（提议不可能的棋步），而不是投降。

让我们看看在游戏 2 中发生了什么，我在其中下黑棋：

Nikola Greb 对阵 ChatGPT 4（走法 1–9）和 ChatGPT 3.5（走法 10–12），2024年1月7日

```py
1\. e4 c5 2\. Nf3 Nc6 3\. d4 cxd4 4\. Nxd4 e5 5\. Nb5 d6 6\. c4 f5 7\. N1c3 Nf6 8\. Bg5 Be7 9\. Bd3 Nxe4 10\. Bxe4 fxe4 11\. Nxe4 Bxg5 12\. Nec3 0–1
```

在下面的位置之前，ChatGPT 4 玩得非常好，从中建立了一个显著更好的位置，而我在面对真正的大师（甚至是候选大师）或国际象棋引擎时会很快输掉。如果白方走 Bf6，黑方会丢掉一个兵。然而，ChatGPT 走了 Bd3：

![ChatGPT 是否有成为新国际象棋超级大师的潜力？](../Images/6d76c32e06fa03fdd0f1bb899329151b.png)

我走了 Ne4，而 ChatGPT 切换到 3.5 版本并走了 Bxe4。

![ChatGPT 是否有成为新国际象棋超级大师的潜力？](../Images/ee6f2e926854d1f4955ce8d8d0cb7e0d.png)

几步棋后，我有了决定性的优势（由于 ChatGPT 玩得很差，不是我做了什么出色的事），所以我决定用一个不规则的走法来测试对手。我在这个位置上提议黑方走 Ne6：

![ChatGPT 是否有成为新国际象棋超级大师的潜力？](../Images/84d6488ffbe6d56acca77b689ffd9743.png)

ChatGPT 3.5 完全没有关注我的棋步。在我的幻觉上，它回应了新的幻觉：

![ChatGPT 是否有成为新国际象棋超级大师的潜力？](../Images/e6277cb4dd406f135867edaaf32bb8d5.png)

# 结论

1\. ChatGPT 4 是一个非常弱的国际象棋玩家，表现非常奇怪——开局阶段非常好，而后期则很糟糕。这是由于国际象棋游戏进展中选项数量的增加。我会评估它的整体 ELO 低于 1500。3.5 也是如此。

2\. 没有发生隐性规则学习——ChatGPT 4 在国际象棋中仍然会产生幻觉，并且在收到关于幻觉的警告后仍然继续产生幻觉。这是人类无法出现的情况。

3\. 更多数据几乎无法解决问题，由于边缘情况如超长的终局重复，或玩不寻常的开局。大型语言模型（LLMs）根本不适合下国际象棋，也无法评估位置。我们已经有了 AlphaZero 和 Stockfish。

4\. 跟踪 LLM 在下国际象棋时产生的幻觉数量的减少，可能是理解 LLM 逻辑推理潜力的一个好路径。但悖论依然存在——LLM “知道”国际象棋规则，但仍然产生严重的幻觉。未来的机器学习可能会是 LLM 作为第一级代理，与用户沟通，然后调用专门的代理，其机器学习架构调整以适应特定的用例。

5. LLM 有潜力在科学研究中发挥作用，并显示出与其他机器学习算法相结合的有趣的创造力。最近的一个例子是 DeepMind 开发的 FunSearch 算法，它结合了 LLM 和评估器，用于在数学领域进行发现。与棋类中位置评估是最困难的任务不同，许多数学科学问题是“易于评估，尽管通常难以解决”。

我对基于变压器架构构建一个高性能的棋类程序持怀疑态度，但专门的语言模型（LLM）结合外部评估/棋类程序，可能很快成为棋类教练的良好替代品。DeepMind 创建了另一个很酷的模型，作为结合 LLM 和专用 AI 模型的良好示例——AlphaGeometry。它在几何问题上的表现非常接近奥林匹克金牌标准，推动了数学中的 AI 推理。

6. LLM 仍然是新兴的领域，尚且很年轻，且有太多的炒作，往往伴随着误导性和错误的结论。正如“《使用大型语言模型的程序搜索中的数学发现》”的作者所言：

> “……据我们所知，这表明了第一个科学发现——使用 LLM 对一个臭名昭著的科学问题的一个可验证的知识新片段。”（加速预览于 2023 年 12 月 14 日发布）。

7. 由乔·罗根（Joe Rogan）和两位嘉宾主持的视频，标题为“‘’我对 AI 并不害怕，直到我了解了这个’’”在 YouTube 上观看人数达到了 280 万。 一位嘉宾表示 ChatGPT 会下棋，这显然是不真实的。我可以想象这种内容如何影响人们，尤其是教育水平低或情绪不稳定的个体。我对此是非常肯定的。

总结来说，数据科学和软件开发建立在知识、精确性和追求真相的基础上。作为数据科学家和开发者，我们应该成为真理和智慧的传播者，平息大众媒体对 AI 的疯狂，而不是加剧这种疯狂。变压器，包括 ChatGPT，在语言任务中具有巨大的潜力，但离 AGI 仍然非常遥远。我们应该保持乐观但要准确。

作为指南，在做出重大声明之前，我们应该问自己：如果其他人根据我的陈述采取行动，会发生什么？你希望生活在什么样的世界里？

## 参考文献与进一步探索

1.  通过自我对弈和通用强化学习算法掌握国际象棋和将棋: https://arxiv.org/pdf/1712.01815.pdf

1.  FunSearch: 使用大型语言模型在数学科学中进行新发现: https://deepmind.google/discover/blog/funsearch-making-new-discoveries-in-mathematical-sciences-using-large-language-models/

1.  使用大型语言模型的程序搜索中的数学发现: https://www.nature.com/articles/s41586-023-06924-6

1.  AlphaGeometry：一个用于几何学的奥林匹克级 AI 系统：https://deepmind.google/discover/blog/alphageometry-an-olympiad-level-ai-system-for-geometry/

1.  我对 AI 不再感到害怕，直到我了解了这个：https://www.youtube.com/watch?v=2yd18z6iSyk&ab_channel=JREDailyClips

1.  如何与 ChatGPT 对弈国际象棋（以及为什么你可能不应该）：https://www.androidauthority.com/how-to-play-chess-with-chatgpt-3330016/

1.  Chat GPT 能玩国际象棋吗？：https://towardsdatascience.com/can-chat-gpt-play-chess-4c44210d43e4

1.  ChatGPT 玩国际象棋的水平如何？（剧透：你会感到惊讶）：https://medium.com/@ivanreznikov/how-good-is-chatgpt-at-playing-chess-spoiler-youll-be-impressed-35b2d3ac024a

1.  与 ChatGPT 的完整对话：https://chat.openai.com/share/a1ff82b5-6210-4f7b-807c-220052de232c

1.  使用通用强化学习算法通过自我对弈掌握国际象棋和将棋：https://arxiv.org/pdf/1712.01815.pdf

**[](https://www.linkedin.com/in/ngreb/)**[尼古拉·格雷布](https://www.linkedin.com/in/ngreb/)**** 编码经验超过四年，在过去两年里，他专注于 NLP。在转向数据科学之前，他在销售、人力资源、写作和国际象棋领域取得了成功。

### 更多相关话题

+   [ETL 与机器学习有什么关系？](https://www.kdnuggets.com/2022/08/etl-machine-learning.html)

+   [挖掘 2023 年数据产品的潜力](https://www.kdnuggets.com/2023/01/tapping-potential-data-products-2023.html)

+   [水印技术如何帮助减轻 LLM 的潜在风险？](https://www.kdnuggets.com/2023/03/watermarking-help-mitigate-potential-risks-llms.html)

+   [通过这门免费的 DevOps 快速入门课程释放你的潜力](https://www.kdnuggets.com/2023/03/corise-unlock-potential-with-this-free-devops-crash-course.html)

+   [揭示 CTGAN 的潜力：利用生成性 AI 进行…](https://www.kdnuggets.com/2023/04/unveiling-potential-ctgan-harnessing-generative-ai-synthetic-data.html)

+   [超越 Numpy 和 Pandas：解锁鲜为人知的…](https://www.kdnuggets.com/2023/08/beyond-numpy-pandas-unlocking-potential-lesserknown-python-libraries.html)
