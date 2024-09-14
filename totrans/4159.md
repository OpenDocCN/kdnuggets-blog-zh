# 如何在几秒钟内处理数百万行的 DataFrame

> 原文：[https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)

![如何在几秒钟内处理数百万行的 DataFrame？](../Images/2de067561faae5b8eb4d567583434cd1.png)

图片由 [Jason Blackeye](https://unsplash.com/@jeisblack?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据科学正在经历其复兴时刻。很难跟上所有可能改变数据科学工作方式的新工具。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 部门

* * *

我最近在与一位同事的交谈中了解到这个新的数据处理引擎，这位同事也是一名数据科学家。我们讨论了大数据处理，这是该领域创新的前沿，这个新工具就出现了。

尽管 [pandas](https://towardsdatascience.com/11-pandas-built-in-functions-you-should-know-1cf1783c2b9) 是 Python 中数据处理的事实标准工具，但它并不适合处理大数据。对于更大的数据集，迟早会遇到内存溢出异常。

研究人员早在很久以前就面临了这个问题，这促使了像 [Dask 和 Spark](https://towardsdatascience.com/are-you-still-using-pandas-for-big-data-12788018ba1a)这样的工具的开发，它们试图通过将处理分布到多台机器上来克服“单机”限制。

这个活跃的创新领域也为我们带来了像 [Vaex](https://towardsdatascience.com/are-you-still-using-pandas-to-process-big-data-in-2021-850ab26ad919)这样的工具，试图通过提高单机处理的内存效率来解决这个问题。

而且问题不仅仅于此。还有另一种大数据处理工具你应该了解……

## 认识 Terality

![如何在几秒钟内处理数百万行的 DataFrame？](../Images/b5eb8dba8279c6a93aa0d3ff1bffb3b3.png)

图片由 [Frank McKenna](https://unsplash.com/@frankiefoto?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[Terality](https://www.terality.com/) 是一个无服务器数据处理引擎，在云端处理数据。无需管理基础设施，因为 Terality 负责计算资源的扩展。其目标用户是工程师和数据科学家。

我与 Terality 团队交换了几封电子邮件，因为我对他们开发的工具感兴趣。他们迅速回复了。这些是我向团队提出的问题：

![如何在几秒钟内处理包含百万行的 DataFrame？](../Images/e2d665667a01d44cb4b0515081c0cb5c.png)

我给 Terality 团队发的第 n 封邮件（作者截图）

## 使用 Terality 进行数据处理的主要步骤是什么？

1.  Terality 附带一个 Python 客户端，你可以将其导入到 Jupyter Notebook 中。

1.  然后你以**“pandas 方式”**编写代码，Terality 安全地上传你的数据，并处理分布式计算（以及扩展）以计算你的分析。

1.  处理完成后，你可以将数据转换回常规的 pandas DataFrame，并在本地继续分析。

## 幕后发生了什么？

Terality 团队开发了一个专有的数据处理引擎——它不是 Spark 或 Dask 的分支。

目标是避免 Dask 的缺陷，Dask 与 pandas 的语法不同，异步操作，未包含所有 pandas 函数，并且不支持自动扩展。

Terality 的数据处理引擎解决了这些问题。

## Terality 是否可以免费使用？

[Terality](https://www.terality.com/pricing)提供一个免费计划，允许你每月处理最多 500 GB 的数据。它还为需求更大的公司和个人提供付费计划。

在本文中，我们将重点关注免费计划，因为它适用于许多数据科学家。

> Terality 如何计算数据使用量？ ([来自 Terality 的文档](https://www.terality.com/pricing))
> 
> 考虑一个总大小为 15GB 的数据集，如操作 *df.memory_usage(deep=True).sum()* 所返回的那样。
> 
> 对该数据集执行一个（1）操作，例如 *.sum* 或 *.sort_values*，将消耗 Terality 中 15GB 的处理数据。
> 
> 只有在任务运行成功状态下才会记录可计费使用量。

## 数据隐私如何？

当用户执行读取操作时，Terality 客户端将数据集复制到 Terality 在 Amazon S3 上的安全云存储中。

Terality 对数据隐私和保护有严格的政策。他们保证不会使用数据，并安全地处理数据。

Terality 不是存储解决方案。他们将在 Terality 客户端会话关闭后的最多 3 天内删除你的数据。

Terality 处理目前发生在 AWS 的法兰克福区域。

[查看安全部分以获取更多信息。](https://docs.terality.com/faq/security)

## 数据需要公开吗？

不！

用户需要访问本地计算机上的数据集，Terality 将在幕后处理上传过程。

上传操作也进行了并行处理，因此更快。

## Terality 能处理大数据吗？

目前，**在 2021 年 11 月，** Terality 仍处于测试阶段。它优化了最多 100-200 GB 的数据集。

我询问了团队是否计划增加此功能，他们计划很快开始优化到 Terabytes 级别。

## 让我们试驾一下

![如何在几秒钟内处理包含数百万行的 DataFrame？](../Images/5049cb89fc1e3da7e44cdff7875f87a1.png)

由 [尤金·赫斯季亚科夫](https://unsplash.com/@eugenechystiakov?utm_source=medium&utm_medium=referral) 拍摄的照片，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

我惊讶于你可以简单地用 Terality 的包替换 pandas 的导入语句，然后重新运行你的分析。

请注意，一旦你导入了 Terality 的 Python 客户端，数据处理将不再在本地机器上进行，而是在 Terality 的云端数据处理引擎中完成。

现在，让我们安装 Terality 并在实践中尝试一下……

## 设置

你可以通过运行以下命令来安装 Terality：

```py
pip install --upgrade terality
```

然后你在 [Terality](https://app.terality.com/) 上创建一个免费账户，并生成一个 API 密钥：

![如何在几秒钟内处理包含数百万行的 DataFrame？](../Images/0e12eac837f2e08e697cc1a50885c409.png)

在 [Terality](https://app.terality.com/) 上生成新的 API 密钥（截图由作者提供）

最后一步是输入你的 API 密钥（同时用你的电子邮件替换电子邮件）：

```py
terality account configure --email your@email.com
```

## 让我们从小处开始……

现在，我们已经安装了 Terality，可以运行一个小示例来熟悉它。

> 实践表明，使用 Terality 和 pandas 两者结合，你可以充分发挥它们各自的优势——一个用于聚合数据，另一个则在本地分析聚合后的数据

以下命令通过导入 pandas.DataFrame 创建一个 terality.DataFrame：

```py
**import** **pandas** **as** **pd**
**import** **terality** **as** **te**df_pd = pd.DataFrame({"col1": [1, 2, 2], "col2": [4, 5, 6]})
df_te = te.DataFrame.from_pandas(df_pd)
```

现在，数据已经在 Terality 的云端，我们可以继续进行分析：

```py
df_te.col1.value_counts()
```

运行过滤操作和其他熟悉的 pandas 操作：

```py
df_te[(df_te["col1"] >= 2)]
```

一旦我们完成分析，可以使用以下命令将其转换回 pandas DataFrame：

```py
df_pd_roundtrip = df_te.to_pandas()
```

我们可以验证 DataFrames 是否相等：

```py
pd.testing.assert_frame_equal(df_pd, df_pd_roundtrip)
```

## 让我们搞大点……

我建议你查看 [Terality 的快速入门 Jupyter Notebook，](https://docs.terality.com/getting-terality/quick-start/tutorial) 其中介绍了 40 GB Reddit 评论数据集的分析。他们还有一个包含 5 GB 数据集的小型教程。

我点击了 Terality 的 Jupyter Notebook 并处理了 40 GB 的数据集。它在 45 秒内读取了数据，并花费了 35 秒进行排序。与另一个表的合并花费了 1 分钟 17 秒。感觉就像在我的笔记本电脑上处理一个更小的数据集。

然后我尝试在我的笔记本电脑（配备 16 GB 主内存）上用 pandas 加载相同的 40GB 数据集——结果返回了内存不足的异常。

[官方 Terality 教程带你分析一个包含 Reddit 评论的 5GB 文件。](https://docs.terality.com/getting-terality/quick-start/tutorial)

## 结论

![如何在几秒钟内处理包含数百万行的 DataFrame？](../Images/008b2b1520e546c7af167ea70812c0aa.png)

照片由 [Sven Scheuermeier](https://unsplash.com/@sveninho?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我尝试了 Terality，并且我的体验没有遇到重大问题。这让我感到惊讶，因为它们官方仍在测试阶段。另一个好兆头是他们的支持团队非常响应迅速。

当你有一个大型数据集无法在本地机器上处理时，我认为 Terality 的应用场景非常出色——无论是因为内存限制还是处理速度。

使用 Dask（或 Spark）需要启动一个集群，这样的成本远高于使用 Terality 完成分析的成本。

此外，配置这样一个集群是一个繁琐的过程，而使用 Terality 你只需要更改导入语句。

我喜欢的另一点是我可以在本地 JupyterLab 中使用它，因为我有许多扩展、配置、深色模式等。

我期待着团队在接下来的几个月中取得的 Terality 进展。

**[Roman Orac](https://www.linkedin.com/in/romanorac/?originalSubdomain=si)** 是一位机器学习工程师，在改进文档分类和项目推荐系统方面取得了显著成功。Roman 具有管理团队、指导初学者和向非工程师解释复杂概念的经验。

[原文](https://towardsdatascience.com/how-to-process-a-dataframe-with-millions-of-rows-in-seconds-fe7065b8f986)。经许可转载。

### 了解更多相关主题

+   [利用 ChatGPT 在几秒钟内创建惊人的数据可视化](https://www.kdnuggets.com/create-stunning-data-viz-in-seconds-with-chatgpt)

+   [如何使用 [ ], .loc, iloc, .at… 在 Pandas 中选择行和列](https://www.kdnuggets.com/2019/06/select-rows-columns-pandas.html)

+   [3 种将行附加到 Pandas DataFrames 的方法](https://www.kdnuggets.com/2022/08/3-ways-append-rows-pandas-dataframes.html)

+   [如何将 JSON 数据转换为 Pandas DataFrame](https://www.kdnuggets.com/how-to-convert-json-data-into-a-dataframe-with-pandas)

+   [Pandas 与 Polars：Python 数据框库的比较分析](https://www.kdnuggets.com/pandas-vs-polars-a-comparative-analysis-of-python-dataframe-libraries)

+   [机器学习过程的框架方法](https://www.kdnuggets.com/2018/05/general-approaches-machine-learning-process.html)
