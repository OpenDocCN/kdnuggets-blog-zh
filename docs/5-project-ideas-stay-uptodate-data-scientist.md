# 5个保持数据科学家技能更新的项目创意

> 原文：[https://www.kdnuggets.com/2022/07/5-project-ideas-stay-uptodate-data-scientist.html](https://www.kdnuggets.com/2022/07/5-project-ideas-stay-uptodate-data-scientist.html)

![5个保持数据科学家技能更新的项目创意](../Images/7fb89a1bef3b7fff3be874a1a53b24e7.png)

我相信实践。实践是知识和思想的应用。我不相信的一种观点是，理论与实践之间的道路是一条单向高速公路。换句话说，它告诉我们实践仅仅是理论原则的应用。但实践远不止如此；它也是新理论的诞生地，推动了新理论的发展。

* * *

## 我们的前三大课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌IT支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的IT工作

* * *

阅读文章或研究论文、参加会议或聚会以跟上最新技术，并获得更好的理论基础是值得赞赏的。我强烈推荐这样做！但无论你获取了什么更新，作为数据科学家的工作仍然会归结为几个基本技能：数据收集、分析和可视化。

而且你需要使用它们！如果你缺乏机会，你必须自己创造机会。最全面的方法是构思一个[数据科学项目](https://www.stratascratch.com/blog/19-data-science-project-ideas-for-beginners/)，并从头到尾完成它。

在这样的项目中，你会使用API来获取实际数据。通过数据清洗和分析，你将获得洞见，并可以将其呈现在一些漂亮的可视化图表中。最后，你可以将其发布在reddit上，获得反馈，并可能考虑这些反馈以改进你的项目。

理想情况下，项目也应该有趣，而不仅仅是枯燥的技能打磨方式。

# 1\. 前10首歌曲的歌词词数变化

项目的构思是通过[Spotify API](https://developer.spotify.com/documentation/web-api/)获取Spotify上前10首最受欢迎歌曲的数据。然后，可以将Spotify的元数据与来自[Genius API](https://docs.genius.com/#/getting-started-h1)或其他歌词网站的歌词进行关联。

歌词中“词”的定义由你决定。例如，你可以计算总词数或仅计算唯一词数。你是否包括像‘na na na’或‘la la la’这样的合唱部分？

通过分析这些数据，你可以展示历史发展并预测未来的趋势。你可以包括其他一些参数，如歌曲长度或前奏长度。尤其是前奏，研究 [如俄亥俄州立大学的研究](https://www.rewire.org/streaming-changed-music-written/) 显示了歌曲前奏长度随着时间的推移大幅减少，这将特别有趣。

你可以从 [这个项目](https://www.reddit.com/r/dataisbeautiful/comments/vs2djz/oc_increasing_word_counts_in_pop_songs_19462021/) 中获取一些方法和可视化的灵感。

# 2\. 投资出租物业

![投资出租物业](../Images/755954edef96f7990160535bf9b3b0f1.png)

如果你考虑在全国范围内投资物业以便出租，分析哪些因素影响租金价格，从而影响业务的盈利性和潜力将是非常有用的。

数据可以通过一些 [租赁 API](https://rapidapi.com/collection/rents-rentals) 获取。决定投资何处何时的重要因素可能包括地点、物业大小、建造日期、设施、租金价格趋势等。

方法的灵感可以来自这个很棒的 [Airbnb 数据科学项目](https://mohamedirfansh.github.io/Airbnb-Data-Science-Project/)。

# 3\. 识别虚假新闻

在这里，你可以使用 [Facebook](https://developers.facebook.com/docs/)、[Twitter](https://developer.twitter.com/en/docs) 或 [reddit API](https://www.reddit.com/dev/api/) 来获取你将使用的数据。根据你拥有的数据，你可以分析社交媒体上的帖子，并将虚假新闻与非虚假新闻区分开来。你的方法可以更为一般化，但你也可以关注某个特定的主题。例如，选择一个主题，如 COVID-19 疫情、美国总统选举或乌克兰战争。你可以分析虚假新闻的地理分布、传播虚假新闻的人的人口统计特征，或最容易受虚假新闻影响的主题。

也许你可以尝试训练一个识别虚假新闻的模型。你可以从 [Kai Shu、Depak Deepak Mahudeswaran 和 Huan Liu 的工作](https://www.researchgate.net/publication/328271325_FakeNewsTracker_a_tool_for_fake_news_collection_detection_and_visualization) 中找到一些方法的灵感。其他有用的资源包括 [Data Flair 项目](https://data-flair.training/blogs/advanced-python-project-detecting-fake-news/) 或 [这个](https://github.com/chirras/Detection-of-Fake-News-Posts-on-Facebook)，你可以用 R 完成，也可以用 Python 重新创建。

# 4\. 邮票上的人物类型

如果你有兴趣在邮票上引入更多关于职业、种族或性别代表性的平等，这个项目可能适合你。这个想法是使用[维基百科 API](https://www.mediawiki.org/wiki/API:Main_page)获取邮票上的人物名单，例如[在美国](https://en.wikipedia.org/wiki/List_of_people_on_the_postage_stamps_of_the_United_States)。你也可以为其他国家甚至全世界做类似的分析。也许还可以比较各国的数据，看看它们之间的比较情况。一个想法是将这些数据与[不平等数据](https://wid.world/data/)连接起来。看看邮票上的代表性不平等如何与国家的经济不平等相关联会很有趣。

[有一个项目](https://www.netcredit.com/blog/country-celebrate-currency/)分析了各国货币上庆祝的人员类型。我认为你可以从中获取一些关于项目方法和可视化的好点子。

# 5\. 预测书籍销售

![预测书籍销售](../Images/9cef0528b65a3d5612a6ae52c46dfe94.png)

[亚马逊 API](https://aws.amazon.com/api-gateway/)是这个项目的一个很好的工具。通过它，你可以获得有关书籍销售的数据。例如，你可以分析不同类型、出版社、作者、页数、价格、评论数、评分等方面的销售情况。

一旦获取数据并分析，你可以预测你的书籍必须满足的参数，以成为畅销书。如果你需要关于如何处理和可视化这个项目的点子，可以查看[在 ResearchGate 上发布的这份 20 页的工作](https://www.researchgate.net/publication/336619238_Success_in_books_predicting_book_sales_before_publication)。

# 6\. 创建你自己的项目

当然，你不应该仅限于这五个项目。我强烈建议你提出自己的[数据分析项目创意](https://www.stratascratch.com/blog/data-analytics-project-ideas-that-will-get-you-the-job/)。当你想到一个项目时，考虑以下几点：

+   **目标：** 项目的目标应该是利用现代数据科学创建一些用户喜欢的东西。

+   **使其有趣且用户友好：** 这通常意味着构建一个互动模型并进行可视化。

+   **创意：** 一个好的项目灵感和可视化来源是[数据之美 subreddit](http://reddit.com/r/dataisbeautiful)。

+   **复制与创新：** 尝试复制你在其中找到的创意，或用它们来提出你自己对项目的改编。

将帮助你作为数据科学家保持最新的项目因素是：

+   使用 API 收集数据

+   使用一些可视化库来创建图形

+   将你的工作发布到 subreddit 上以获得反馈

# 总结

我相信这些项目中的任何一个都会挑战你运用现有的知识，并尝试一些全新的事物。在选择项目时，关键是它能够带来实际的成果，并且你在过程中也能享受乐趣。这就是为什么我尝试选择涉及我们生活各个方面的项目。

这些项目的共同点在于它们使你运用所有基本的数据科学技术和技能。使用这些项目是保持这些技能的最佳方式。

-   如果你能利用这些项目想出一些更好、更有趣的点子，那就更好了。想象力也是一个重要但常被忽视的数据科学技能。但请确保这些项目能挑战你其他的数据科学技能。

**[内特·罗西迪](https://www.stratascratch.com)** 是一名数据科学家，专注于产品战略。他还是一名兼职教授，教授分析课程，并且是 [StrataScratch](https://www.stratascratch.com/)、一个帮助数据科学家准备面试的平台的创始人。你可以通过 [Twitter: StrataScratch](https://twitter.com/StrataScratch) 或 [LinkedIn](https://www.linkedin.com/in/nathanrosidi/) 与他联系。

### 更多相关内容

+   [数据管理：如何保持在客户心中的地位？](https://www.kdnuggets.com/2022/04/data-management-stay-top-customer-mind.html)

+   [如何掌握人工智能领域的最新动态](https://www.kdnuggets.com/2022/03/stay-top-going-ai-world.html)

+   [如何保持 Python 的最新状态](https://www.kdnuggets.com/2022/06/stay-current-python.html)

+   [19 个数据科学初学者项目想法](https://www.kdnuggets.com/2021/11/19-data-science-project-ideas-beginners.html)

+   [掌握数据工程的项目想法](https://www.kdnuggets.com/project-ideas-to-master-data-engineering)

+   [2022 年人工智能项目想法](https://www.kdnuggets.com/2022/01/artificial-intelligence-project-ideas-2022.html)
