# 最佳 Jupyter 笔记本扩展前五名

> 原文：[`www.kdnuggets.com/2018/03/top-5-best-jupyter-notebook-extensions.html`](https://www.kdnuggets.com/2018/03/top-5-best-jupyter-notebook-extensions.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[Eliot Andres](http://ndres.me/)，自由职业机器学习工程师**

笔记本扩展是你可以轻松添加到 Jupyter 笔记本中的插件。最好的安装方法是使用[Jupyter NbExtensions Configurator](https://github.com/Jupyter-contrib/jupyter_nbextensions_configurator)。它会添加一个选项卡，让你启用/禁用扩展：

![NbExtensions Configurator 的截图](img/d0e1d98ada7f4335a10ed644af1cdebd.png)

NbExtensions Configurator 的截图

### 安装

使用 conda 安装：

```py
conda install -c conda-forge jupyter_contrib_nbextensions
conda install -c conda-forge jupyter_nbextensions_configurator

```

或者使用 pip：

```py
pip install jupyter_nbextensions_configurator jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
jupyter nbextensions_configurator enable --user

```

在[这里](https://github.com/Jupyter-contrib/jupyter_nbextensions_configurator#installation)找到更多关于安装的信息

### 1 - 可折叠标题

在处理大型笔记本时非常有用，可折叠标题允许你折叠笔记本的某些部分。

![使用可折叠标题](img/7ee97803b45f4f7da0fb34d48996b319.png)

使用可折叠标题

### 2 - 通知

对于长时间运行的任务，notify 扩展会在笔记本闲置时发送通知。

![使用通知](img/7b1059cadc9158658961d267db51f5ce.png)使用通知

*要使用它，请启用扩展，然后在按钮栏中启用。你选择的时间是笔记本必须运行的最短时间，以便你收到通知（请注意，你必须保持笔记本在浏览器中打开才能使通知有效）*

### 3 - 代码折叠

![使用代码折叠](img/62788bbedf6c672c403a202ecef1ed66.png)

使用代码折叠

### 4 - tqdm_notebook

这个并不真正是一个笔记本扩展。TQDM 是一个进度条库。但它有时在 Jupyter 笔记本中无法正常工作。感谢[Randy Olson](https://twitter.com/randal_olson)的提示：

> 今天学到：tqdm（[#Python](https://twitter.com/hashtag/Python?src=hash&ref_src=twsrc%5Etfw) 进度条库）有一个专门用于 Jupyter 笔记本的“tqdm_notebook”功能。
> 
> 不再在我的笔记本中看到混乱的进度条——万岁！[`t.co/r0jAQXQ6TM`](https://t.co/r0jAQXQ6TM) [pic.twitter.com/FyYBRm2qE1](https://t.co/FyYBRm2qE1)
> 
> — Randy Olson (@randal_olson) [2018 年 3 月 2 日](https://twitter.com/randal_olson/status/969657169342734336?ref_src=twsrc%5Etfw)

### 5 - %debug

这不是一个笔记本扩展，而是一个[IPython 魔法](http://ipython.readthedocs.io/en/stable/interactive/magics.html)命令。有关详细解释，我建议阅读[完整的推特线程](https://twitter.com/radekosmulski/status/945739571735748609)，来自[Radek Osmulski](https://twitter.com/radekosmulski)。

> 最近最喜欢的 jupyter 笔记本发现——%debug 魔法：
> 
> 1. 异常。
> 
> 2. 插入一个新单元，输入 %debug 并运行它。
> 
> 一个交互式调试器将打开，带你到异常发生的地方，并允许你四处查看！ [pic.twitter.com/9DSnSbpu15](https://t.co/9DSnSbpu15)
> 
> — Radek (@radekosmulski) [2017 年 12 月 26 日](https://twitter.com/radekosmulski/status/945739571735748609?ref_src=twsrc%5Etfw)

### 6 - 较小的扩展和其他提示

+   **%lsmagic**: 在单元格中运行此命令以列出所有可用的 IPython 魔法命令

+   **禅模式扩展**: 移除菜单以减少干扰

+   **执行时间扩展**: 显示单元格运行所花费的时间

+   **autoreload**: 自动重新加载外部文件，无需重新启动笔记本。启用方法：

```py
%load_ext autoreload
%autoreload 2

```

你知道必备的笔记本扩展吗？在推特上联系我或对这篇博客文章进行拉取请求！

### 编辑于 2018 年 3 月 7 日:

一些人 [在 Reddit](https://www.reddit.com/r/MachineLearning/comments/82fcw0/d_top_5_best_jupyter_notebook_extensions/) 提出了更多建议：

+   **[变量检查器](http://jupyter-contrib-nbextensions.readthedocs.io/en/latest/nbextensions/varInspector/README.html)**: 在浮动窗口中显示所有变量

+   **CodeMirror Keymap**: 让你在各种键绑定（如 vim）之间进行选择

+   **Scratchpad**: 在不修改笔记本文档的情况下执行代码

+   **Splitcells**: 垂直拆分单元格

**简介: [Eliot Andres](http://ndres.me/)** ([**@eliotandres**](https://twitter.com/eliotandres)) 是一名自由机器学习工程师，专注于将模型从原型转移到生产。他对 Tensorflow 和计算机视觉情有独钟

[原文](https://ndres.me/post/best-jupyter-notebook-extensions/)。经许可转载。

**相关:**

+   了解机器学习的 5 件事

+   从笔记本到 JupyterLab - 数据科学 IDE 的演变

+   Fast.ai 第 1 课在 Google Colab（免费 GPU）

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域的职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 需求

* * *

### 相关主题

+   [数据科学统计学习的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [停止学习数据科学以寻找目标，并通过寻找目标…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [一项 90 亿美元的 AI 失败，进行审视](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么使 Python 成为初创企业理想的编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每位数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
