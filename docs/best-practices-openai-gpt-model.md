# 使用 OpenAI GPT 模型的最佳实践

> 原文：[https://www.kdnuggets.com/2023/08/best-practices-openai-gpt-model.html](https://www.kdnuggets.com/2023/08/best-practices-openai-gpt-model.html)

![使用 OpenAI GPT 模型的最佳实践](../Images/271bbda36d9d64d7bac2fb5621791567.png)

图片来源：[rawpixel.com](https://www.freepik.com/free-photo/illustration-quality-product-warranty-assurance-laptop_18122652.htm#query=best%20practice&position=11&from_view=search&track=ais%22) 于 Freepik

自从 GPT 模型发布以来，每个人都在不断使用它。从提出简单问题到开发复杂的编码，GPT 模型可以迅速帮助用户。这就是为什么模型随着时间的推移会变得越来越强大的原因。

* * *

## 我们的前三名课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

为了帮助用户获得最佳输出，OpenAI 提供了他们的 [最佳实践](https://platform.openai.com/docs/guides/gpt-best-practices) 用于使用 GPT 模型。这来自于许多用户对该模型的不断实验和找到的最佳使用方式。

在本文中，我将总结你在使用 OpenAI GPT 模型时应知道的最佳实践。这些实践是什么？让我们深入了解。

# GPT 最佳实践

GPT 模型的输出质量仅取决于你的提示。通过明确的指示你想要什么，它将提供你期望的结果。一些提升 GPT 输出质量的建议包括：

1.  **在提示中包含详细信息以获得相关答案。** 例如，与其使用“给我代码来计算正态分布”的提示，不如写成“请提供标准分布计算的 Python 代码示例。在每个部分中放置注释，并解释每段代码为何如此执行。

1.  **提供角色或示例，并增加输出的长度。** 我们可以为模型提供角色或示例，以便更好地澄清。例如，我们可以传递系统角色参数，以教师讲解学生的方式来解释某些内容。通过提供角色，GPT 模型将以我们所需的方式呈现结果。如果你想更改角色，这里有一段示例代码。

```py
import openai

openai.api_key = ""

res = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    max_tokens=100,
    temperature=0.7,
    messages=[
        {
            "role": "system",
            "content": """

          When I ask to explain something, bring it in a way that teacher 
          would explain it to students in every paragraph.

          """,
        },
        {
            "role": "user",
            "content": """

         What is golden globe award and what is the criteria for this award? Summarize them in 2 paragraphs.

          """,
        },
    ],
)
```

提供示例结果也是一个好方法，以指导 GPT 模型如何回答你的问题。例如，在这段代码中，我传递了我如何解释情感的方式，GPT 模型应模仿我的风格。

```py
res = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    max_tokens=100,
    temperature=0.7,
    messages=[
        {
            "role": "system",
            "content": "Answer in a consistent style.",
        },
        {
            "role": "user",
            "content": "Teach me about Love",
        },
        {
            "role": "assistant",
            "content": "Love can be sweet, can be sour, can be grand, can be low, and can be anything you want to be",
        },
        {
            "role": "user",
            "content": "Teach me about Fear",
        },
    ],
)
```

1.  **指定完成任务的步骤。** 提供详细的步骤说明，以便获得最佳输出。详细分解 GPT 模型应如何操作的指令。例如，我们在这段代码中提供了带前缀和翻译的两步指令。

```py
res = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        max_tokens=100,
        temperature=0.7,
        messages= [
            {
          "role": "system",
          "content": """
          Use the following step-by-step instructions to respond to user inputs.

        step 1 - Explain the question input by the user in 2 paragraphs or less with the prefix "Explanation: ".

        Step 2 - Translate the Step 1 into Indonesian, with a prefix that says "Translation: ".

          """,
         },
         {
          "role": "user",
          "content":"What is heaven?",
         },
        ])
```

1.  **提供参考、链接或引用**。如果我们已经有了各种问题的参考资料，我们可以利用这些作为 GPT 模型提供输出的基础。提供任何你认为与问题相关的参考列表，并将其传入系统角色。

1.  **给 GPT 时间来“思考”**。提供一个查询，让 GPT 详细处理提示，然后再匆忙给出不正确的结果。这一点尤其重要，如果我们传递给助手角色一个错误的结果，我们希望 GPT 能够进行批判性思考。例如，下面的代码展示了我们如何要求 GPT 模型对用户输入更具批判性。

```py
res = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        max_tokens=100,
        temperature=0.7,
        messages= [
            {
          "role": "system",
          "content": """

        Work on your solution to the problem, then compare your solution to the user and evaluate
        if the solution is correct or not. Only decide if the solution is correct once you have done the problem yourself.

          """,
         }, 
         {
          "role": "user",
          "content":"1 + 1 = 3",
         },
        ])
```

1.  **让 GPT 使用代码执行以获得精确结果。** 对于更复杂的计算，GPT 可能无法正常工作，因为模型可能提供不准确的结果。为了解决这个问题，我们可以要求 GPT 模型编写和运行代码，而不是直接计算。这样，GPT 可以依赖代码而不是其计算结果。例如，我们可以提供如下输入。

```py
res = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        max_tokens=100,
        temperature=0.7,
        messages= [
            {
          "role": "system",
          "content": """

      Write and execute Python code by enclosing it in triple backticks,
       e.g. ```code goes here```py. Use this to perform calculations.

          """,
         }, 
         {
          "role": "user",
          "content":"""

          Find all real-valued roots of the following polynomial equation: 2*x**5 - 3*x**8- 2*x**3 - 9*x + 11.

          """,
         },
        ])
```

# 结论

GPT 模型是现有的最佳模型之一，以下是一些最佳实践来提高 GPT 模型的输出：

1.  **在提示中包含详细信息以获得相关回答**

1.  **提供角色或示例，并添加输出的长度**

1.  **指定完成任务的步骤**

1.  **提供参考、链接或引用**

1.  **给 GPT 时间来“思考”**

1.  **让 GPT 使用代码执行以获得精确结果**

**[Cornellius Yudha Wijaya](https://www.linkedin.com/in/cornellius-yudha-wijaya/)** 是数据科学助理经理和数据撰写员。在全职工作于 Allianz Indonesia 的同时，他喜欢通过社交媒体和写作媒体分享 Python 和数据技巧。

### 相关主题

+   [认识 Gorilla：UC Berkeley 和 Microsoft 的 API 扩展 LLM…](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)

+   [免费 ChatGPT 课程：使用 OpenAI API 编码 5 个项目](https://www.kdnuggets.com/2023/05/free-chatgpt-course-openai-api-code-5-projects.html)

+   [如何使用 GPT 生成创意内容（Hugging Face）…](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

+   [NExT-GPT 介绍：任何对任何的多模态大型语言模型](https://www.kdnuggets.com/introduction-to-nextgpt-anytoany-multimodal-large-language-model)

+   [与 OpenAI 一起构建 AI 产品：CoRise 的免费课程](https://www.kdnuggets.com/2023/07/corise-building-ai-products-openai-free-course-corise.html)

+   [介绍 OpenAI 的 Superalignment](https://www.kdnuggets.com/2023/08/introducing-superalignment-openai.html)
