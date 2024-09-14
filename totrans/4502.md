# 深度学习的四个最佳 Jupyter Notebook 环境

> 原文：[https://www.kdnuggets.com/2020/03/4-best-jupyter-notebook-environments-deep-learning.html](https://www.kdnuggets.com/2020/03/4-best-jupyter-notebook-environments-deep-learning.html)

[评论](#comments)

Notebooks 正在成为数据科学家进行原型设计和分析的事实标准。许多云服务提供商以 Jupyter notebooks 的形式提供机器学习和深度学习服务。其他公司现在也开始提供类似存储、计算和定价结构的云托管 Jupyter 环境。主要的区别之一可能是多语言支持和版本控制选项，这允许数据科学家在一个地方分享他们的工作。

### **Jupyter Notebook 环境的日益普及**

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT

* * *

Jupyter notebook 环境现在成为了将你的数据科学项目产品化的首选平台。Notebook 环境允许我们跟踪错误并保持代码整洁。尽管简单，但其中一个最佳特性是如果发现错误，Notebook 会停止编译你的代码。常规 IDE 即使检测到错误也不会停止编译，且根据代码量的不同，返回并手动检测错误位置可能会浪费时间。

![最佳 Jupyter Notebook 深度学习环境 | Macbook 计算机与 Jupyter 环境代码示例](../Images/5a3949da668c9b8ccfa096fda3b98c18.png)

许多云服务提供商和其他第三方服务看到了 Jupyter notebook 环境的价值，这就是为什么许多公司现在提供托管在云上的笔记本，且可供数百万用户访问。许多数据科学家没有进行大规模深度学习所需的硬件，但通过云托管环境，硬件和后台配置大多由系统处理，用户只需配置所需的参数，如 CPU/GPU/TPU、RAM、核心等。

### **[1\. MatrixDS](https://matrixds.com/)**

[![最佳 Jupyter Notebook 深度学习环境 | Matrixds](../Images/82b6c34c187113318abb87e6ea444857.png)](https://matrixds.com/)

+   MatrixDS 是一个云平台，提供了一种结合了社交网络和 GitHub 的体验，专为与同事分享数据科学项目而设计。它提供了一些最常用的技术，如 R、Python、Shiny、MongoDB、NGINX、Julia、MySQL 和 PostgreSQL。

+   他们同时提供免费和付费的服务。付费服务类似于主要云平台上提供的服务，您可以按使用量或时间付费。该平台根据需要提供 GPU 支持，以便在本地机器不足时完成内存密集型和计算密集型任务。

**开始使用 MatrixDS 中的 Jupyter Notebook 环境：**

+   注册服务以创建一个帐户。默认情况下，它应该是一个免费帐户。

+   然后，您将被提示进入一个项目页面。在这里，点击右上角的绿色按钮开始一个新项目。给它起个名字和描述，然后点击 CREATE。

+   然后您将被要求设置一些配置，如 RAM 和核心数量。由于这是一个免费帐户，您将被限制为 4GB RAM 和 1 个核心 CPU。

+   完成后，您将被带到一个页面，在这里您的工具（一个 Jupyter Notebook 实例）将会进行配置并准备就绪。

+   一旦您看到设置过程完成，点击 START，然后在运行时点击 OPEN，您将被带到一个新标签页，打开您的 Jupyter Notebook 实例。

### **[2\. Google Colaboratory](https://colab.research.google.com/notebooks/welcome.ipynb#recent=true)**

[![最佳深度学习 Jupyter Notebook 环境 | Google Colab](../Images/764a4191a5c1f36abd88995a7f87868c.png)](https://colab.research.google.com/notebooks/welcome.ipynb#recent=true)

+   Google Colab 是一个由 Google 提供的免费 Jupyter Notebook 环境，专为深度学习任务设计。它完全运行在云端，使您能够分享您的工作，直接保存到 Google Drive，并提供计算资源。

+   Colab 的主要优势之一是它提供了免费的 GPU 支持（当然有一定限制 – 请查看他们的 FAQ）。请参见 [Anne Bommer 关于 Google Colab 入门的精彩文章](https://towardsdatascience.com/getting-started-with-google-colab-f2fff97f594c)。

+   它不仅支持 GPU， [我们还可以在 Colab 上使用 TPU](https://colab.research.google.com/notebooks/tpu.ipynb)。

除了常规的 Jupyter Notebook 之外，使用 Google Colab 进行 Jupyter 环境的一个简单示例是使用来自 [opencv-python](https://github.com/skvark/opencv-python) 包的 The cv2.imshow() 和 cv.imshow() 函数。这两个函数与独立的 Jupyter Notebook 不兼容。Google Colab 提供了一个自定义修复此问题的方法：

```py` ``` 从 google.colab.patches 导入 cv2_imshow    !curl - o logo.png https://colab.research.google.com/img/colab_favicon_256px.png    导入 cv2  img = cv2.imread('logo.png', cv2.IMREAD_UNCHANGED)  cv2_imshow(img) ```py ````

在代码单元中运行上述代码，以验证其确实有效，并开始你的图像和视频处理任务。

### **[3\. Google Cloud的AI Platform Jupyter Notebooks](https://cloud.google.com/ai-platform-notebooks/)**

[![最佳深度学习Jupyter Notebook环境 | Google Cloud AI Platform](../Images/8f7a98500f9016cb2a3f3b3e89e87256.png)](https://cloud.google.com/ai-platform-notebooks/)

+   Google Cloud提供了集成的JupyterLab管理实例，预装了最新的机器学习和深度学习库，如TensorFlow、PyTorch、scikit-learn、pandas、NumPy、SciPy和Matplotlib。

+   该Notebook实例与BigQuery、Cloud Dataproc和Cloud Dataflow集成，以提供从数据摄取、预处理、探索、训练到部署的无缝体验。

+   集成服务使用户能够通过几次点击轻松扩展计算和存储容量。

**要在GCP上开始使用你的JupyterLab实例，请按照以下步骤操作：**

+   [在开始之前](https://cloud.google.com/ai-platform/notebooks/docs/before-you-begin)，然后，

+   [创建一个新的JupyterLab实例](https://cloud.google.com/ai-platform/notebooks/docs/create-new)

使用Keras运行以下代码，查看云环境和[GPU](https://blog.exxactcorp.com/whats-the-best-gpu-for-deep-learning-rtx-2080-ti-vs-titan-rtx-vs-rtx-8000-vs-rtx-6000/)支持如何加速你的分析：

数据集的链接是：[数据集CSV文件（pima-indians-diabetes.csv）](https://raw.githubusercontent.com/jbrownlee/Datasets/master/pima-indians-diabetes.data.csv)。为了简便，数据集应与您的python文件位于同一工作目录下。

将其保存为文件名：*pima-diabetes.csv*

```py` ``` 从 numpy 导入 loadtxt 从 keras.models 导入 Sequential 从 keras.layers 导入 Dense # 加载数据集 dataset = loadtxt('pima-diabetes.csv', delimiter=',') X = dataset[:,0:8] y = dataset[:,8] # 定义keras模型 model = Sequential() model.add(Dense(12, input_dim=8, activation='relu')) model.add(Dense(8, activation='relu')) model.add(Dense(1, activation='sigmoid')) # 由于这是一个二分类问题，选择Sigmoid model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy']) # 编译keras模型 model.fit(X, y, epochs=150, batch_size=10) # 训练keras模型 _, accuracy = model.evaluate(X, y) # 评估keras模型 print('Accuracy: %.2f' % (accuracy*100) ```py ````

### **[4\. Saturn Cloud](https://www.saturncloud.io/)**

[![最佳深度学习Jupyter Notebook环境 | Saturn Cloud](../Images/cae52462c59df045a1972bd112a0d874.png)](https://www.saturncloud.io/)

+   Saturn Cloud 是一个新的云服务，提供一键式托管在云端的 Jupyter 笔记本，可以根据你的计算和存储需求进行扩展，使用 AWS 作为后端。这里有一个教程来帮助你入门：[如何轻松创建、发布，甚至分享云托管的 Jupyter 笔记本，使用 Saturn Cloud](https://towardsdatascience.com/getting-started-with-saturn-cloud-jupyter-notebooks-b3f509a500ef)

+   Saturn Cloud 应该处理数据科学中的 DevOps 部分，通过提供版本控制和协作机会来使你的分析更加可重复。

+   Saturn Cloud 提供了[与 Dask（用 Python 编写）的并行计算基础设施](https://www.saturncloud.io/docs/why-dask)，而不是其他大数据工具，如 Spark。

**开始使用 Saturn Cloud：**

+   登录并创建一个账户：[Saturn Cloud 登录](https://www.saturncloud.io/auth/login?next=/dash/jupyter)。基础计划可以免费使用，以熟悉环境。

+   创建你的笔记本实例：

    +   为笔记本指定一个名称

    +   存储量

    +   使用的 GPU 或 CPU

    +   （可选）Python 环境（例如：Pip、Conda）

    +   （可选）自动关闭

    +   一个 requirements.txt 文件来安装你项目所需的库。

+   在指定以上参数后，你可以点击 **CREATE** 来启动服务器和你的笔记本实例。

+   *Saturn Cloud 还提供了托管你的笔记本，使其可以分享。这是 Saturn Cloud 处理数据科学项目的 DevOps 方面的一个示例，使用户不必担心。*

运行以下代码以验证你的实例是否按预期运行。

```py` ``` 导入 pandas 为 pd  导入 matplotlib.pyplot 为 plt  %matplotlib inline     url = "https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data"  names = ['sepal-length', 'sepal-width', 'petal-length', 'petal-width', 'class']  data = pd.read_csv(url, names=names)     pd.plotting.scatter_matrix(dataset)  plt.show() ```py ````

### **什么是最佳的 Jupyter Notebook 环境？**

![4 个最佳 Jupyter 环境用于深度学习 | Jupyter 环境的排名从最佳到最差](../Images/e7af69ea33839b73861206f200433c1e.png)

我们根据分析、可视化能力、数据存储和数据库功能等多个因素，将 Jupyter Notebook 环境从最佳到最差进行排名。每个平台都有其最佳和最差的用例及其独特的卖点。

上述所有服务都旨在满足你的深度学习需求，并提供一个可重现的环境，以便分享你的工作并尽可能少地进行后台工作。随着深度学习取得新进展，算法仍然需要大量的数据，而大多数数据科学家在本地机器上无法实现这一点。这时，上述替代方案允许我们进行无缝体验的分析。以下是我们对哪个平台最好、哪个最差的最佳尝试的客观观点：

[**#1 MatrixDS: **](https://matrixds.com/)

+   MatrixDS 的独特之处在于，它为不同的任务提供不同的工具。对于分析，它提供 Python、R、Julia、Tensorboard 等，对于可视化，它可以提供 Superset、Shiny、Flask、Bokeh 等，存储数据则提供 PostgreSQL。

[**#2 Saturn Cloud:**](https://www.saturncloud.io/)

+   Saturn Cloud 提供并行计算支持，并使注册过程和创建 Jupyter notebook 尽可能简单，与列表中的其他提供商相比。对于那些只想以最少的麻烦开始，并且只需要一个可以处理大数据的服务器的用户，这可能是最好的选择。

[**#3 Google 的 AI Platform Notebooks：**](https://cloud.google.com/ai-platform-notebooks/)

+   这个 notebook 环境支持 Python 和 R。数据科学用户可能会有首选的语言，而在主要云服务提供商上支持这两种语言是一个吸引人的选择。它还提供对 GCP 其他服务的访问，如 BigQuery，直接从 Notebook 本身进行，使查询数据更高效和强大。

[**#4 Google Colaboratory: **](https://colab.research.google.com/notebooks/welcome.ipynb#recent=true)

+   虽然相当强大且是唯一一个提供 TPU 支持的环境，但在数据科学工作流的全面性方面，它的功能不如其他环境丰富。它只支持 Python，功能上类似于标准的 Jupyter Notebook，但用户界面不同。它提供将你的 notebook 共享到 Google Drive 并可以访问你的 Google Drive 数据。

[原文](https://blog.exxactcorp.com/the-4-best-jupyter-notebook-environments-for-deep-learning/)。经许可转载。

**相关内容：**

+   [5 个 Google Colaboratory 提示](/2020/03/5-google-colaboratory-tips.html)

+   [替代的云托管数据科学环境](/2019/12/alternative-cloud-data-science-environments.html)

+   [如何优化你的 Jupyter Notebook](/2020/01/optimize-jupyter-notebook.html)

### 更多相关主题

+   [如何在 Jupyter Notebook 上设置 Julia](https://www.kdnuggets.com/2022/11/setup-julia-jupyter-notebook.html)

+   [10 个 Jupyter Notebook 使用技巧和窍门](https://www.kdnuggets.com/2023/06/10-jupyter-notebook-tips-tricks-data-scientists.html)

+   [Jupyter Notebook 魔法方法速查表](https://www.kdnuggets.com/jupyter-notebook-magic-methods-cheat-sheet)

+   [Python在金融中的应用：Jupyter Notebook中的实时数据流](https://www.kdnuggets.com/python-in-finance-real-time-data-streaming-within-jupyter-notebook)

+   [5 个免费的数据科学项目模板用于 Jupyter Notebook](https://www.kdnuggets.com/5-free-templates-for-data-science-projects-on-jupyter-notebook)

+   [Feature Store Summit 2023: 部署机器学习模型的实用策略](https://www.kdnuggets.com/2023/09/hopsworks-feature-store-summit-2023-practical-strategies-deploying-ml-models-production-environments)
