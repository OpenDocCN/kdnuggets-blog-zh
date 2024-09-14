# 初学者构建 LLM 应用的指南

> 原文：[`www.kdnuggets.com/beginners-guide-to-building-llm-apps-with-python`](https://www.kdnuggets.com/beginners-guide-to-building-llm-apps-with-python)

![初学者构建 LLM 应用的指南](img/c380ff86d656d015ad72ec4198aa6f28.png)

图片来源：编辑 | Midjourney & Canva

罗宾·夏尔马说过，**“每一个大师曾经都是初学者。每一个专家曾经都是业余爱好者。”** 你可能已经听说过大型语言模型（LLMs）、人工智能（AI）和变换器模型（GPT）在 AI 领域掀起的波澜，并且你对如何入门感到困惑。我可以向你保证，今天你看到的每一个正在构建复杂应用程序的人曾经也经历过这种阶段。

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png)1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png)2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png)3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

这就是为什么在本文中，你将获得开始使用 Python 编程语言构建 LLM 应用所需的知识。这篇文章完全适合初学者，你可以一边阅读一边进行编码。

你将在本文中构建什么？你将创建一个简单的 AI 个人助手，根据用户的提示生成回应，并将其部署以便全球访问。下面的图片展示了完成后的应用程序的样子。

![这张图片展示了本文将构建的 AI 个人助手的用户界面](img/239aaa46b34aa7ac7d9ae2214c9f7a63.png)

这张图片展示了本文将构建的 AI 个人助手的用户界面

## 先决条件

为了跟随本文，你需要具备几个条件。这包括：

1.  [Python](https://www.python.org/) (3.5+)，以及编写 Python 脚本的背景。

1.  OpenAI: [OpenAI](https://openai.com/) 是一个研究组织和技术公司，致力于确保人工通用智能（AGI）造福全人类。它的一个重要贡献是开发了先进的 LLM，例如 [GPT-3](https://openai.com/index/gpt-3-apps/) 和 [GPT-4](https://openai.com/index/gpt-4/)。这些模型能够理解和生成类人文本，使其成为各种应用的强大工具，如聊天机器人、内容创作等。

    [注册](https://www.openai.com/) OpenAI，并从你的账户 API 部分复制你的 API 密钥，以便你可以访问模型。使用下面的命令在你的计算机上安装 OpenAI：

```py
pip install openai
```

1.  LangChain: [LangChain](https://www.langchain.com/) 是一个框架，旨在简化利用 LLM 的应用程序开发。它提供了管理和简化处理 LLM 各个方面的工具和实用程序，使得构建复杂且强大的应用程序变得更容易。

使用下面的命令在你的计算机上安装 LangChain：

```py
pip install langchain
```

1.  Streamlit: [Streamlit](https://streamlit.io/) 是一个强大且易于使用的 Python 库，用于创建 Web 应用程序。Streamlit 允许你仅使用 Python 创建交互式 Web 应用程序。你不需要具备 Web 开发（HTML、CSS、JavaScript）方面的专业知识，就能构建功能齐全且视觉吸引人的 Web 应用。

    这对于构建机器学习和数据科学应用程序很有益，包括那些利用 LLM 的应用程序。使用下面的命令在你的计算机上安装 Streamlit：

```py
pip install streamlit
```

## 代码演示

安装所有必需的包和库后，是时候开始构建 LLM 应用程序了。在你的工作目录的根目录中创建一个 `requirement.txt` 文件，并保存依赖项。

```py
streamlit
openai
langchain
```

创建一个 `app.py` 文件，并添加下面的代码。

```py
# Importing the necessary modules from the Streamlit and LangChain packages
import streamlit as st
from langchain.llms import OpenAI
```

+   导入 Streamlit 库，该库用于创建交互式 Web 应用程序。

+   `from langchain.llms import OpenAI` 从 `langchain.llms` 模块中导入 OpenAI 类，该类用于与 OpenAI 的语言模型进行交互。

```py
# Setting the title of the Streamlit application
st.title('Simple LLM-App 🤖')
```

+   `st.title('Simple LLM-App 🤖')` 设置 Streamlit Web 的标题。

```py
# Creating a sidebar input widget for the OpenAI API key, input type is password for security
openai_api_key = st.sidebar.text_input('OpenAI API Key', type='password')
```

+   `openai_api_key = st.sidebar.text_input('OpenAI API Key', type='password')` 在侧边栏创建一个文本输入小部件，供用户输入其 OpenAI API 密钥。输入类型设置为 'password' 以隐藏输入的文本以提高安全性。

```py
# Defining a function to generate a response using the OpenAI language model
def generate_response(input_text):
    # Initializing the OpenAI language model with a specified temperature and API key
    llm = OpenAI(temperature=0.7, openai_api_key=openai_api_key)
    # Displaying the generated response as an informational message in the Streamlit app
    st.info(llm(input_text))
```

+   `def generate_response(input_text)` 定义一个名为 `generate_response` 的函数，该函数以 `input_text` 作为参数。

+   `llm = OpenAI(temperature=0.7, openai_api_key=openai_api_key)` 使用温度设置 0.7 和提供的 API 密钥初始化 OpenAI 类。

**Temperature** 是一个参数，用于控制语言模型生成文本的随机性或创造力。它决定了模型在预测中引入的变异程度。

+   **低温度 (0.0 - 0.5)**: 使模型更具确定性和专注性。

+   **中等温度 (0.5 - 1.0)**: 在随机性和确定性之间提供平衡。

+   **高温度 (1.0 及以上)**: 增加输出的随机性。较高的值使模型在响应中更具创造性和多样性，但也可能导致输出不连贯，产生更多无意义或离题的内容。

+   `st.info(llm(input_text))` 调用语言模型并用提供的 `input_text` 生成响应，然后将其作为信息消息显示在 Streamlit 应用中。

```py
# Creating a form in the Streamlit app for user input
with st.form('my_form'):
    # Adding a text area for user input
    text = st.text_area('Enter text:', '')
    # Adding a submit button for the form
    submitted = st.form_submit_button('Submit')
    # Displaying a warning if the entered API key does not start with 'sk-'
    if not openai_api_key.startswith('sk-'):
        st.warning('Please enter your OpenAI API key!', icon='⚠')
    # If the form is submitted and the API key is valid, generate a response
    if submitted and openai_api_key.startswith('sk-'):
        generate_response(text)
```

+   `with st.form('my_form')` 创建一个名为 `my_form` 的表单容器。

+   `text = st.text_area('Enter text:', '')` 在表单中添加一个文本区域输入小部件，供用户输入文本。

+   `submitted = st.form_submit_button('Submit')` 向表单添加一个提交按钮。

+   if not openai_api_key.startswith('sk-') 检查输入的 API 密钥是否不以 sk- 开头。

+   `st.warning('Please enter your OpenAI API key!', icon='⚠')` 如果 API 密钥无效，则显示警告消息。

+   if submitted and openai_api_key.startswith('sk-')检查表单是否已提交以及 API 密钥是否有效。

+   `generate_response(text)` 调用 `generate_response` 函数，使用输入的文本生成并显示响应。

综合起来，你现在拥有的是：

```py
# Importing the necessary modules from the Streamlit and LangChain packages
import streamlit as st
from langchain.llms import OpenAI

# Setting the title of the Streamlit application
st.title('Simple LLM-App 🤖')

# Creating a sidebar input widget for the OpenAI API key, input type is password for security
openai_api_key = st.sidebar.text_input('OpenAI API Key', type='password')

# Defining a function to generate a response using the OpenAI model
def generate_response(input_text):
    # Initializing the OpenAI model with a specified temperature and API key
    llm = OpenAI(temperature=0.7, openai_api_key=openai_api_key)
    # Displaying the generated response as an informational message in the Streamlit app
    st.info(llm(input_text))

# Creating a form in the Streamlit app for user input
with st.form('my_form'):
    # Adding a text area for user input with a default prompt
    text = st.text_area('Enter text:', '')
    # Adding a submit button for the form
    submitted = st.form_submit_button('Submit')
    # Displaying a warning if the entered API key does not start with 'sk-'
    if not openai_api_key.startswith('sk-'):
        st.warning('Please enter your OpenAI API key!', icon='⚠')
    # If the form is submitted and the API key is valid, generate a response
    if submitted and openai_api_key.startswith('sk-'):
        generate_response(text)
```

### 运行应用程序

应用程序已准备好；你需要使用适合你所用框架的命令来执行应用程序脚本。

```py
streamlit run app.py
```

通过运行此代码`streamlit run app.py`，你可以创建一个交互式 Web 应用程序，用户可以输入提示并接收 LLM 生成的文本响应。

当你执行`streamlit run app.py`时，以下情况会发生：

+   **Streamlit 服务器启动**：Streamlit 在你的机器上启动一个本地网络服务器，通常默认情况下可以通过`http://localhost:8501`访问。

+   **代码执行**：Streamlit 读取并执行`app.py`中的代码，根据脚本定义渲染应用程序。

+   **Web 界面**：你的 Web 浏览器会自动打开（或者你可以手动导航）到 Streamlit 提供的 URL（通常是**http://localhost:8501**），你可以在这里与 LLM 应用程序进行交互。

### 部署你的 LLM 应用程序

部署 LLM 应用程序意味着使其通过互联网可访问，以便其他人可以使用和测试，而无需访问你的本地计算机。这对协作、用户反馈和现实世界的测试很重要，确保应用程序在不同环境中表现良好。

要将应用程序部署到 Streamlit Cloud，请按照以下步骤操作：

+   为你的应用程序创建一个 GitHub 仓库。确保你的仓库包括两个文件：**app.py** 和 **requirements.txt**

+   访问 [Streamlit Community Cloud](http://share.streamlit.io/)，点击工作区中的“**新应用**”按钮，指定仓库、分支和主文件路径。

+   点击**部署**按钮，你的 LLM 应用程序将被部署到 Streamlit Community Cloud，并可以全球访问。

![使用 Python 构建 LLM 应用程序的初学者指南](img/6f640f2e2be2647ae054b649a22f3ced.png)

## 结论

恭喜！你已迈出构建和部署 Python LLM 应用程序的第一步。从了解前提条件、安装必要的库到编写核心应用程序代码，你现在已经创建了一个功能齐全的 AI 个人助手。通过使用 Streamlit，你使应用程序具有交互性和易用性，并通过将其部署到 Streamlit Community Cloud，你使其对全球用户可访问。

利用你在本指南中学到的技能，你可以进一步深入探讨 LLMs 和 AI，探索更高级的功能，并构建更复杂的应用。不断实验、学习，并与社区分享你的知识。LLMs 的可能性巨大，你的旅程才刚刚开始。编码愉快！

[Shittu Olumide](https://www.linkedin.com/in/olumide-shittu/)是一位软件工程师和技术作家，热衷于利用前沿技术编写引人入胜的叙述，具有敏锐的细节观察力和简化复杂概念的能力。你还可以在[Twitter](https://twitter.com/Shittu_Olumide_)上找到 Shittu。

### 相关话题

+   [Python 向量数据库和向量索引：构建 LLM 应用的架构](https://www.kdnuggets.com/2023/08/python-vector-databases-vector-indexes-architecting-llm-apps.html)

+   [帮助构建你的 LLM 应用的 5 个工具](https://www.kdnuggets.com/5-tools-to-help-build-your-llm-apps)

+   [构建数据管道以创建大型语言模型应用](https://www.kdnuggets.com/building-data-pipelines-to-create-apps-with-large-language-models)

+   [Web LLM：将 LLM 聊天机器人带到浏览器](https://www.kdnuggets.com/2023/05/webllm-bring-llm-chatbots-browser.html)

+   [用于构建 LLM 流程的拖放 UI：Flowise AI](https://www.kdnuggets.com/2023/07/draganddrop-ui-building-llm-flows-flowise-ai.html)

+   [构建 LLM 应用时需要知道的 5 件事](https://www.kdnuggets.com/2023/08/5-things-need-know-building-llm-applications.html)
