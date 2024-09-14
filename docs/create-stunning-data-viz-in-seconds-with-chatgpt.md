# 使用 ChatGPT 迅速创建惊艳的数据可视化

> 原文：[`www.kdnuggets.com/create-stunning-data-viz-in-seconds-with-chatgpt`](https://www.kdnuggets.com/create-stunning-data-viz-in-seconds-with-chatgpt)

![使用 ChatGPT 迅速创建惊艳的数据可视化](img/8690901295ed4040e30885372223138a.png)

图片来源：作者

# 介绍

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

数据可视化是任何从事数据工作的人必备的关键技能。但创建美观且信息丰富的数据可视化可能耗时且需要专业工具。这就是 ChatGPT 的用武之地。借助其最新更新，ChatGPT 使数据可视化比以往任何时候都更快、更容易。

最新更新显著提升了 ChatGPT 的体验。现在，你无需像原始 GPT-4、GPT4 高级分析版或 DALLE-3 之间切换，只需输入一个提示，ChatGPT 会自动解释你的请求并生成所需结果。

![使用 ChatGPT 迅速创建惊艳的数据可视化](img/ab15ef20e41590132b3ebb8f8fc27308.png)

图片来源：ChatGPT

在这篇博客文章中，我们将探索如何使用简单的英文提示即时生成各种数据可视化。得益于 ChatGPT 的高级数据分析，你无需处理数据或运行 Python 代码。我们将从简单的饼图和条形图开始，然后使用实际数据集处理更复杂的可视化。

# 简单可视化

在这一部分，我们将编写一个简单的提示来生成图表。该提示包括以 Python 字典形式呈现的数据。

## 饼图

在我们创建提示之前，请确保你正在使用 GPT-4 模型，因为只有它支持生成可视化。

我们将编写一个提示来生成基于各种营养数据的饼图可视化。此外，我们请求 ChatGPT 使用更柔和的颜色组合，因为默认颜色比较鲜艳。

```py
**Prompt: ** Generate a pie chart of values {"Vitamin A":5, "Vitamin B": 1, "Vitamin C": 4, "Water": 90} to keep the color combination light.
```

正如你所见，我们得到了很好的结果。

![使用 ChatGPT 迅速创建惊艳的数据可视化](img/2f8431df82799754180c30ab2e00d996.png)

如果你想查看可视化背后的 Python 代码，你需要点击结果末尾的终端图标。

![使用 ChatGPT 迅速创建惊艳的数据可视化](img/9ae3bf46b9e1be5e899e8450525e4681.png)

之后，会出现一个包含源代码的窗口，你可以修改和执行这些代码。然而，这一步不是强制性的，因为 ChatGPT 会直接运行代码并以图像形式显示可视化结果。你可以将这些图像保存用于演示或报告。

![使用 ChatGPT 秒级创建令人惊叹的数据可视化](img/4b78243c2d0fb1c0e0a6d3dff43c0f81.png)

## 条形图

在下一部分，我们提供了汽车的 CO2 排放数据，并让 ChatGPT 施展魔法。

```py
**Prompt: ** Generate a bar plot co2 emissions of values {"Car A":30, "Car B": 25, "Car C": 20}.
```

它已添加标题、x 轴和 y 轴标签，并确保了降序排列。完美!!!

![使用 ChatGPT 秒级创建令人惊叹的数据可视化](img/36448be08a3240635bb303ee43050dbf.png)

# 探索性数据分析

与其过度控制 ChatGPT 的输出，不如让它独立创建结果，类似于各种 Python AutoViz 库。只需提供数据集并请求进行完整的探索性数据分析，以生成你需要查看的图表。

在我们的案例中，我们提供了一个[顾客购物趋势](https://www.kaggle.com/datasets/iamsouravbanerjee/customer-shopping-trends-dataset/data)数据集，该数据集提供了有关消费者行为和购买模式的宝贵见解。

```py
**Prompt: ** Perform exploratory data analysis on customer shopping trends dataset and display only plots.
```

ChatGPT 提供了快速的结果，处理和分析消费者趋势在不到一分钟的时间内完成，而这个任务通常需要我至少 30 分钟的编码和运行时间。

![使用 ChatGPT 秒级创建令人惊叹的数据可视化](img/0e4d9d7b6e669c3ece57dceff81abe9a.png)

![使用 ChatGPT 秒级创建令人惊叹的数据可视化](img/02839c52f16e168c429e0d3c113fe475.png)

你可以通过提供后续提示来改善结果，说明你感兴趣的可视化类型。

```py
**Prompt: ** Improve the analysis by plotting  a correlation chart, bar chart, pie chart, boxplot, and relplot.
```

![使用 ChatGPT 秒级创建令人惊叹的数据可视化](img/59b5cb2a4b1883093b8e36680f0238e2.png)

如果你想看到多层次的复杂可视化，你需要特别要求 ChatGPT。

```py
**Prompt: ** Use the dataset to plot various complex visualizations.
```

![使用 ChatGPT 秒级创建令人惊叹的数据可视化](img/12017d722a8a4a9caa1d117632be5efd.png)

# 模型评估

数据可视化在模型评估中发挥了至关重要的作用。在这一部分，我们将使用 Kaggle 的[糖尿病数据集](https://www.kaggle.com/datasets/mathchi/diabetes-data-set)，并要求 ChatGPT 训练和评估多个模型。为了最大化 ChatGPT 的能力，我们将请求它显示混淆矩阵、精准率-召回率和比较不同模型的图表。

```py
**Prompt: ** Multiple machine learning models should be trained using the target column "Outcome", and the resulting model evaluation visualization should include a confusion matrix, precision-recall, and model comparison chart.
```

显然，ChatGPT 表现出色。尽管模型在数据集上的表现不佳，但我们对其快速而准确的数据可视化能力印象深刻。它可以用来快速分析数据集，或在面试或家庭作业中回答问题。

![使用 ChatGPT 秒级创建令人惊叹的数据可视化](img/97895da636385c49ea59f9813f105634.png)

# 结论

ChatGPT 彻底改变了我们轻松创建数据可视化的方式。凭借其先进的数据分析能力，你可以使用简单的英文提示在几秒钟内生成令人惊叹和有信息量的数据可视化。

在这篇文章中，我们了解了 ChatGPT 如何即时生成各种图表，如饼图、条形图、相关矩阵，甚至是复杂的可视化图形如 relplots。

ChatGPT 在处理糖尿病数据集的机器学习模型训练以及生成评估指标和比较图表时也超出了预期。整个模型构建和可视化过程几乎只用了不到一分钟。

无论你需要简单的条形图、先进的模型分析，还是仅仅是快速理解数据集，ChatGPT 都能以最小的努力提供卓越的结果。随着能力的不断提升，现在是利用这个 AI 助手提升你的数据可视化技能的激动人心的时刻。

[](https://www.polywork.com/kingabzpro)****[Abid Ali Awan](https://www.polywork.com/kingabzpro)**** ([@1abidaliawan](https://www.linkedin.com/in/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为遭受心理疾病困扰的学生构建一款 AI 产品。

### 更多相关话题

+   [如何在几秒钟内处理百万行的 DataFrame](https://www.kdnuggets.com/2022/01/process-dataframe-millions-rows-seconds.html)

+   [视觉 ChatGPT：微软结合 ChatGPT 和 VFMs](https://www.kdnuggets.com/2023/03/visual-chatgpt-microsoft-combine-chatgpt-vfms.html)

+   [ChatGPT CLI：将你的命令行界面转变为 ChatGPT](https://www.kdnuggets.com/2023/07/chatgpt-cli-transform-commandline-interface-chatgpt.html)

+   [运用你的数据科学技能创造 5 种收入来源](https://www.kdnuggets.com/2023/03/data-science-skills-create-5-streams-income.html)

+   [如何为你的数据项目创建抽样计划](https://www.kdnuggets.com/2022/11/create-sampling-plan-data-project.html)

+   [使用 Tableau 创建高效的综合数据源](https://www.kdnuggets.com/2022/05/create-efficient-combined-data-sources-tableau.html)
