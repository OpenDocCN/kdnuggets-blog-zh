# 人工智能、分析、机器学习、数据科学、深度学习研究 2019 年的主要发展及 2020 年的关键趋势

> 原文：[https://www.kdnuggets.com/2019/12/predictions-ai-machine-learning-data-science-research.html](https://www.kdnuggets.com/2019/12/predictions-ai-machine-learning-data-science-research.html)

[评论](#comments)

又到年末了，这意味着是时候进行 KDnuggets 年度年终专家分析和预测。今年我们提出了这个问题：

> **2019 年人工智能、数据科学、深度学习和机器学习的主要发展是什么？2020 年你期望哪些关键趋势？**

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加入网络安全领域的快车道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 需求

* * *

回顾我们专家[一年前的预测](/2018/12/predictions-machine-learning-ai-2019.html)，我们看到了一些可以被视为自然技术进步的混合，以及一些更为雄心勃勃的预测。出现了一些普遍的主题，以及几个值得注意的单独预测。

尤其是，对人工智能的持续担忧被多次提及，这一预测确实似乎已经实现。有关自动化机器学习的进展的讨论普遍存在，但关于它是否有用还是会失败的观点意见不一。我认为这在一定程度上仍未定论，但当对技术的期望得到缓解时，它更容易被视为一种有用的补充，而不是一个即将取代的存在。增加善用人工智能也被特别提到，原因充分，且有大量例子可以证明这一预测的准确性。提出实用机器学习将迎来考验的观点，标志着趣味和游戏的时代即将结束，现在是机器学习需要有所作为的时候了。这一点确实符合实际，越来越多的从业者正在寻找这些机会。最后，提到对反乌托邦人工智能发展（包括监控、恐惧和操控）的担忧，可以通过对过去一年新闻的简单检查，自信地将其归入成功预测的类别。

也有一些预测尚未实现。这在这样的活动中是不可避免的，我们将把这些留给感兴趣的读者自行探索。

今年我们的专家名单包括 Imtiaz Adam、Xavier Amatriain、Anima Anandkumar、Andriy Burkov、Georgina Cosma、Pedro Domingos、Ajit Jaokar、Charles Martin、Ines Montani、Dipanjan Sarkar、Elena Sharova、Rosaria Silipo 和 Daniel Tunkelang。我们感谢他们从繁忙的年终日程中抽出时间为我们提供见解。

这是未来一周中3篇类似文章的第一篇。虽然这些文章将分别涉及研究、部署和行业，但这些学科之间存在相当大的重叠，因此我们建议你在发布时查看所有3篇文章。

![头像头图](../Images/9f6e43b0d15ebf298133fb8ea558c675.png)

不再耽搁，以下是今年专家组对2019年关键趋势和2020年预测的分析。

**[Imtiaz Adam](https://www.linkedin.com/in/imtiaz-adam-7467528/) ([@DeepLearn007](https://twitter.com/deeplearn007)) 是一位人工智能与战略执行官。**

2019年，各组织对数据科学中的伦理和多样性问题有了更高的意识。

《彩票票假设》论文展示了用剪枝简化深度神经网络训练的潜力。《神经符号概念学习者》论文展示了将逻辑与深度学习结合起来，提升数据和记忆效率的潜力。

GANs 的研究获得了动力，深度强化学习尤其受到了大量研究关注，包括逻辑强化学习和用于参数优化的遗传算法等领域。

TensorFlow 2 带来了集成了 Keras 的 eager execution 默认模式。

2020年，数据科学团队和商业团队将更加整合。5G 将成为智能物联网增长的催化剂，边缘的 AI 推理意味着 AI 将越来越多地进入物理世界。深度学习与增强现实的结合将改变客户体验。

**[Xavier Amatriain](https://www.linkedin.com/in/xamatriain/) ([@xamat](https://twitter.com/xamat)) 是 Curai 的联合创始人兼首席技术官。**

我认为，很难争辩这一点：这一年确实是深度学习和自然语言处理的年份。更具体地说，是语言模型的年份。甚至更具体地说，是Transformers和GPT-2的年份。是的，可能很难相信，但自OpenAI首次~~发布~~谈论他们的[GPT-2语言模型](https://openai.com/blog/better-language-models/)已经不到一年。这篇博客文章引发了关于AI安全的大量讨论，因为OpenAI对发布该模型感到不安。从那时起，该模型被公开复制，并[最终发布](https://openai.com/blog/gpt-2-1-5b-release/)。然而，这并不是这一领域唯一的进展。我们看到Google发布了[AlBERT](https://arxiv.org/pdf/1909.11942.pdf)或[XLNET](https://arxiv.org/abs/1906.08237)，并谈论BERT是近年来对Google搜索的最大改进。从[Amazon](https://arxiv.org/abs/1904.09408)和[Microsoft](https://github.com/microsoft/dialogpt)到[Facebook](https://ai.facebook.com/blog/a-new-generative-qa-model-that-learns-to-answer-the-whole-question/)，每个人似乎都真正投身于语言模型革命中，我期待在2020年看到这一领域的令人印象深刻的进展，似乎我们越来越接近通过图灵测试。

**[Anima Anandkumar](https://www.linkedin.com/in/anima-anandkumar/) ([@AnimaAnandkumar](https://twitter.com/AnimaAnandkumar)) 是NVIDIA的机器学习研究主任和加州理工学院的布伦教授。**

研究人员旨在更好地理解深度学习及其泛化特性和失败案例。减少对标记数据的依赖是关键焦点，自我训练等方法获得了关注。模拟变得在AI训练中更加相关，并在如自动驾驶和机器人学习等视觉领域变得更为逼真，包括在NVIDIA平台如DriveSIM和Isaac上。语言模型变得越来越庞大，例如NVIDIA的80亿参数Megatron模型在512个GPU上训练，并开始生成连贯的段落。然而，研究人员发现这些模型中存在虚假的相关性和不良的社会偏见。AI监管成为主流，许多知名政治家表示支持政府机构禁止面部识别。AI会议开始执行行为规范，并增加了提高多样性和包容性的努力，从去年NeurIPS更名开始。在即将到来的一年里，我预测将会有新的算法发展，而不仅仅是深度学习的肤浅应用。这将特别影响“科学中的AI”，如物理学、化学、材料科学和生物学等许多领域。

**[Andriy Burkov](https://www.linkedin.com/in/andriyburkov/) ([@burkov](https://twitter.com/burkov)) 是Gartner的机器学习团队负责人，且是《百页机器学习书》的作者。**

主要的发展无疑是BERT，这种语言建模神经网络模型提高了几乎所有任务的自然语言处理质量。谷歌甚至将其作为相关性的主要信号之一——多年来最重要的更新。

在我看来，关键趋势将是PyTorch在行业中的更广泛应用，对更快的神经网络训练方法和在便捷硬件上快速训练神经网络的研究增加。

**[乔治娜·科斯马](https://www.linkedin.com/in/georginacosma/) ([@gcosma1](https://twitter.com/gcosma1)) 是拉夫堡大学的高级讲师。**

在2019年，我们欣赏到深度学习模型的令人印象深刻的能力，例如YOLOv3，特别是在实时物体检测等各种复杂计算机视觉任务中。我们还看到生成对抗网络继续在深度学习社区中受到关注，尤其是在图像合成方面，如BigGAN模型在ImageNet生成中的应用，以及StyleGAN在人体图像合成中的应用。今年我们还意识到欺骗深度学习模型的容易程度，一些研究还表明深度神经网络容易受到对抗性示例的攻击。在2019年，我们还见证了偏见的AI决策模型被用于面部识别、招聘和法律应用中。2020年，我预计会看到多任务AI模型的发展，这些模型旨在通用且多功能，同时也期待看到对伦理AI模型的兴趣增加，因为AI正在改变健康、金融服务、汽车及其他许多领域的决策。

**[佩德罗·多明戈斯](https://www.linkedin.com/in/pedro-domingos-77b183/) ([@pmddomingos](https://twitter.com/pmddomingos)) 是华盛顿大学计算机科学与工程系的教授。**

2019年的主要发展：

+   上下文嵌入的迅速传播。它们虽然不到两岁，但现在已主导了自然语言处理，谷歌已经在其搜索引擎中部署了这些嵌入，据报道改善了10%的搜索结果。从视觉到语言，先在大数据上预训练模型，然后针对特定任务进行调整，已成为标准做法。

+   双重下降的发现。我们对过度参数化模型如何在完全拟合训练数据的同时进行良好泛化的理论理解显著提高，特别是对于观察到的现象的候选解释——与经典学习理论预测相反——即随着模型容量的增加，泛化误差先下降，再上升，然后再次下降。

+   媒体和公众对AI进展的看法变得更加怀疑，对自动驾驶汽车和虚拟助手的期望降低，华而不实的演示也不再被轻信。

2020年的关键趋势：

+   深度学习圈试图从低级感知任务如视觉和语音识别“爬升”到高级认知任务如语言理解和常识推理的速度将会加快。

+   依靠不断增加数据和计算能力来获得更好结果的研究模式将达到其极限，因为它处于一个比摩尔定律更陡峭的指数成本曲线上，已经超出了即使是富有公司能够承担的范围。

+   如果运气好的话，我们将进入一个黄金时代，在这个时代里，既没有关于人工智能的过度炒作，也没有另一个人工智能寒冬。

**[Ajit Jaokar](https://www.linkedin.com/in/ajitjaokar/) ([@AjitJaokar](https://twitter.com/AjitJaokar)) 是牛津大学“人工智能：云和边缘实现”课程的课程主任。**

在2019年，我们在牛津大学重新品牌了我们的课程，改为[人工智能：云和边缘实现](https://www.conted.ox.ac.uk/courses/artificial-intelligence-cloud-and-edge-implementations)。这也反映了我个人的观点，即2019年是云计算成熟的一年。这一年，我们所谈论的各种技术（大数据、人工智能、物联网等）在云计算的框架内汇聚在一起。这一趋势将继续——特别是对于企业。公司将进行“数字化转型”——他们将利用云作为统一的范式来转型由人工智能驱动的过程（有点像重新工程化企业2.0）。

在2020年，我还看到自然语言处理的成熟（BERT、Megatron）。5G将继续部署。我们将在5G全面部署后看到物联网的更广泛应用（例如：自动驾驶汽车）。最后，在物联网方面，我关注一种叫做MCU（微控制器单元）的技术——特别是机器学习模型在MCU上的部署。

我相信人工智能是一个颠覆性的变化，每天我们都能看到令人着迷的人工智能应用实例。艾尔文·托夫勒在《未来冲击》中预测的许多情景今天已经发生——人工智能将如何进一步放大这些变化仍待观察！遗憾的是，人工智能的变化速度将让许多人被抛在后面。

**[Charles Martin](https://www.linkedin.com/in/charlesmartin14/) 是人工智能科学家、顾问及Calculation Consulting的创始人。**

BERT、ELMO、GPT2等等！2019年的人工智能在自然语言处理领域取得了巨大进展。OpenAI发布了他们的大型GPT2模型——即文本的深度伪造。谷歌宣布使用BERT进行搜索——自Panda以来最大的一次变化。即使是我在加州大学伯克利分校的合作者也发布了（量化的）QBERT以适应低足迹硬件。现在每个人都在制作自己的文档嵌入。

这对2020年意味着什么？根据搜索专家的说法，2020年将是相关性*(嗯，他们都在做什么？)*的年份。预计向量空间搜索将最终获得 traction，采用BERT风格的精细调优嵌入。

在 2019 年，PyTorch 超过 Tensorflow 成为 AI 研究的首选。随着 TensorFlow 2.x 的发布（以及对 pytorch 的 TPU 支持），2020 年的 AI 编程将完全围绕急切执行展开。

大公司在 AI 方面取得进展了吗？报告显示成功率为 1/10。不太理想。因此，AutoML 在 2020 年将会受到需求，尽管我个人认为，就像制作出色的搜索结果一样，成功的 AI 需要特定于业务的定制解决方案。

![Wordcloud](../Images/2e68f321253503a8f95d564a77c808f4.png)

**[伊内斯·蒙塔尼](https://ines.io/)（[@_inesmontani](https://twitter.com/_inesmontani)）是一位从事人工智能和自然语言处理技术的程序开发人员，并且是 Explosion 的联合创始人。**

每个人都选择“DIY AI”而不是云解决方案。推动这一趋势的一个因素是迁移学习的成功，这使得任何人都可以用较高的准确性训练自己的模型，且能够针对非常具体的使用案例进行微调。由于每个模型只有一个用户，因此服务提供商无法利用规模经济。迁移学习的另一个优点是数据集不再需要那么大，因此标注工作也在内部进行。内部化的趋势是一个积极的发展：商业 AI 远比许多人预期的要分散。几年前，人们担心每个人都只会从一个提供商那里获取“他们的 AI”。然而，人们并没有从任何提供商那里获取 AI——他们自己动手做。

**[迪潘詹·萨尔卡](http://www.linkedin.com/in/dipanzan) 是 Applied Materials 的数据科学负责人，Google 开发者专家 - 机器学习，作者、顾问和培训师。**

2019 年，人工智能领域的主要进展集中在 Auto-ML、可解释 AI 和深度学习方面。数据科学的民主化在过去几年中仍然是一个关键方面，各种与 Auto-ML 相关的工具和框架正试图简化这一过程。然而，仍需注意的是，我们在使用这些工具时必须小心，以确保不会得到有偏见或过拟合的模型。公平性、问责制和透明性仍然是客户、企业和公司接受 AI 决策的关键因素。因此，可解释 AI 不再只是研究论文中的话题。许多优秀的工具和技术已经开始使机器学习模型的决策更加可解释。最后但同样重要的是，我们看到深度学习和迁移学习，特别是自然语言处理领域的许多进展。我期待在 2020 年看到更多关于自然语言处理和计算机视觉的深度迁移学习研究和模型，希望能看到结合深度学习和神经科学精华的工作，从而引领我们迈向真正的 AGI。

**[埃琳娜·沙罗娃](https://codefying.com/) 是 ITV 的高级数据科学家。**

到目前为止，2019年最重要的机器学习进展是在玩游戏时使用深度强化学习，通过DeepMind的[DQN](https://deepmind.com/research/open-source/dqn)和[AlphaGo](https://deepmind.com/research/case-studies/alphago-the-story-so-far)实现的；这导致了[围棋冠军李世石的退休](https://www.thetimes.co.uk/article/go-world-champion-lee-sedol-retires-in-the-face-of-ai-grrf6t6wm)。另一个重要进展是在自然语言处理领域，Google开源了BERT（深度双向语言表示），而[微软领导了GLUE基准](https://blogs.msdn.microsoft.com/stevengu/2019/06/20/microsoft-achieves-human-performance-estimate-on-glue-benchmark/)并通过[开发和开源MT-DNN](https://github.com/namisan/mt-dnn)集成体用于发音分辨任务。

重要的是要强调欧洲委员会发布的[《可信AI伦理指南》](https://ec.europa.eu/digital-single-market/en/news/ethics-guidelines-trustworthy-ai)——这是首个设定合法、道德和稳健AI合理指南的官方出版物。

最后，我想和KDnuggets的读者分享的是，[PyData London 2019](https://pydata.org/london2019/)的所有主题演讲者都是女性——这是一个令人欢迎的发展！

我预期2020年的主要机器学习发展趋势将继续集中在自然语言处理（NLP）和计算机视觉（computer vision）领域。采用机器学习（ML）和数据科学（DS）的行业已经认识到，他们在定义共享的最佳实践标准方面已经滞后，包括招聘和留住数据科学家、管理涉及DS和ML的项目复杂性，以及确保社区保持开放和合作。因此，我们应该会看到未来会更多关注这些标准。

**[Rosaria Silipo](https://www.linkedin.com/in/rosaria/)（[@DMR_Rosaria](https://twitter.com/DMR_Rosaria)）是KNIME的首席数据科学家。**

2019年最令人期待的成就是主动学习、强化学习和其他半监督学习程序的采用。半监督学习可能为处理目前充斥我们数据库的未标记数据带来希望。

另一个伟大的进展是将autoML概念中的“auto”纠正为“guided”。对于更复杂的数据科学问题，专家干预似乎是不可或缺的。

在2020年，数据科学家将需要一个快速的解决方案来进行模型部署、持续模型监控和灵活的模型管理。真正的商业价值将来自数据科学生命周期的这三个最终部分。

我还相信，深度学习黑箱的更广泛应用将引发机器学习解释性（MLI）的问题。我们将在2020年底看到MLI算法是否能够应对解释深度学习模型内部运作的挑战。

**[Daniel Tunkelang](https://www.linkedin.com/in/dtunkelang/) ([@dtunkelang](https://twitter.com/dtunkelang)) 是一名独立顾问，专注于搜索、发现和机器学习/人工智能。**

人工智能的前沿依然专注于语言理解和生成。

OpenAI 宣布了 [GPT-2](https://openai.com/blog/better-language-models/https://openai.com/blog/better-language-models/) 以预测和生成文本。OpenAI 当时没有发布训练模型，担心恶意应用，但最终他们 [改变了主意](https://openai.com/blog/gpt-2-1-5b-release/)。

谷歌发布了一款 [80MB 的设备端语音识别器](https://ai.googleblog.com/2019/03/an-all-neural-on-device-speech.html)，使得在移动设备上进行语音识别成为可能，而无需将数据发送到云端。

与此同时，我们看到对人工智能和隐私的担忧越来越严重。今年，所有主要的数字助手公司都遭遇了员工或承包商监听用户对话的反对声。

2020 年人工智能会发生什么？我们将看到对话式人工智能的进一步发展，以及更好的图像和视频生成。这些进展将引发更多关于恶意应用的担忧，我们可能会看到一两起丑闻，尤其是在选举年。善与恶的人工智能之间的紧张关系不会消失，我们必须学习更好的应对方式。

**相关**：

+   [2018 年机器学习与人工智能的主要发展及 2019 年的关键趋势](/2018/12/predictions-machine-learning-ai-2019.html)

+   [2018 年人工智能、数据科学、分析的主要发展及 2019 年的关键趋势](/2018/12/predictions-data-science-analytics-2019.html)

+   [行业预测：2018 年人工智能、机器学习、分析与数据科学的主要发展及 2019 年的关键趋势](/2018/12/predictions-industry-2019.html)

### 更多相关话题

+   [人工智能、分析、机器学习、数据科学、深度学习……](https://www.kdnuggets.com/2021/12/developments-predictions-ai-machine-learning-data-science-research.html)

+   [2021 年主要发展及 2022 年 AI、数据科学的关键趋势](https://www.kdnuggets.com/2021/12/trends-ai-data-science-ml-technology.html)

+   [2021 年数据科学与分析行业的主要发展及关键…](https://www.kdnuggets.com/2021/12/developments-predictions-data-science-analytics-industry.html)

+   [成为一名优秀数据科学家所需的 5 项关键技能](https://www.kdnuggets.com/2021/12/5-key-skills-needed-become-great-data-scientist.html)

+   [每位初学者数据科学家应该掌握的 6 个预测模型](https://www.kdnuggets.com/2021/12/6-predictive-models-every-beginner-data-scientist-master.html)

+   [2021 年最佳 ETL 工具](https://www.kdnuggets.com/2021/12/mozart-best-etl-tools-2021.html)
