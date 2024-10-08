# 使用 ChatGPT 生成被动收入的 4 种方法

> 原文：[`www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html`](https://www.kdnuggets.com/2023/03/4-ways-generate-passive-income-chatgpt.html)

在当今快节奏的世界中，每个人都希望在不付出太多努力的情况下获得额外的收入。被动收入是一种很好的方式，它允许你在无需积极工作情况下赚取钱。

随着人工智能和聊天机器人技术的发展，现在可以利用这些技术产生被动收入。在本文中，我们将探讨使用 ChatGPT 这一由 OpenAI 创建的语言模型来产生被动收入的四种方法。让我们深入了解如何利用 ChatGPT 产生被动收入。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速通道进入网络安全职业。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升您的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

![使用 ChatGPT 生成被动收入的 4 种方法](img/31acee2d338430644cab26658d37a98c.png)

ChatGPT | 图片由 [Rolf van Root](https://unsplash.com/@freshvanroot?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1\. 构建一个付费的谷歌 Chrome 扩展

第一个方法是构建一个谷歌 Chrome 扩展。这是一种非常可靠的被动收入方法。例如，著名的 Ad block 谷歌 Chrome 扩展创造了超过 4400 万美元的收入。我们可以使用 ChatGPT 来帮助我们构建一个 Chrome 扩展，然后发布它。

让我们构建一个名为 veganit 的谷歌 Chrome 扩展，将非素食选项转换为素食选项。为此，我们首先需要打开 [ChatGPT 页面](https://openai.com/blog/chatgpt/) 并注册或登录。接下来，我们将给它以下提示：

> **你能告诉我创建一个名为 veganieit 的 Chrome 扩展脚本的步骤吗？这个扩展会读取网页内容并将网页上的“牛奶”一词更改为“素食牛奶替代品”**

以下是 ChatGPT 的输出：

当然，我可以给你一个关于如何创建一个 Chrome 扩展脚本的大致步骤，这个脚本会根据要求修改网页内容。

这是您可以遵循的基本步骤：

1.  在计算机上创建一个新文件夹以包含扩展文件。

1.  在新文件夹内，创建一个名为 manifest.json 的文件。此文件将包含有关您扩展的信息，包括其名称、版本和权限。

1.  打开 manifest.json 文件并添加以下代码：

```py
{
  "manifest_version": 2,
  "name": "VeganizeIt",
  "version": "1.0",
  "description": "Changes 'milk' to 'vegan milk alternative'.",
  "permissions": [
    "activeTab"
  ],
  "content_scripts": [
    {
      "matches": [
        ""
      ],
      "js": [
        "content.js"
      ]
    }
  ]
}
```

1.  保存 manifest.json 文件。

1.  在同一文件夹中创建一个名为 content.js 的新文件。这个文件将包含修改网页内容的脚本。将以下代码添加到 content.js：

```py
const replaceText = (node) => {
  const wordsToReplace = {
    "milk": "vegan milk alternative"
  };

  Object.keys(wordsToReplace).forEach((word) => {
    const regex = new RegExp(`\\b${word}\\b`, "gi");
    node.textContent = node.textContent.replace(regex, wordsToReplace[word]);
  });
};

const walk = (node) => {
  const ignoreNodes = {
    "STYLE": true,
    "SCRIPT": true,
    "NOSCRIPT": true,
    "IFRAME": true,
    "OBJECT": true,
    "EMBED": true,
    "VIDEO": true,
    "AUDIO": true
  };

  if (ignoreNodes[node.nodeName]) {
    return;
  }

  if (node.nodeType === 3) {
    replaceText(node);
  } else {
    for (let i = 0; i < node.childNodes.length; i++) {
      walk(node.childNodes[i]);
    }
  }
};

walk(document.body);
```

1.  保存 content.js 文件。

1.  打开 Google Chrome 并导航到 chrome://extensions。

1.  点击“加载解压缩的”按钮并选择包含扩展文件的文件夹。

1.  打开任何网页，通过查找“milk”一词是否被替换为“vegan milk alternative”来测试你的扩展。

之后你可以通过多种方式进行货币化：

+   在 Google Chrome 扩展商店中进行货币化

+   提供免费版和付费版，后者具有更多功能

+   用电子邮件进行交换并创建电子邮件列表，推广其他产品。

由于 ChatGPT 将使构建此类扩展变得简单，一切都取决于你的创造力以及提出独特且有需求的创意的能力，这样你将有更少的竞争和广阔的市场。你可以使用 ChatGPT 指导你构建扩展，然后可以提升你的技能以使其更好。

# 2\. 创建 HTML 网络工具

有很多简单的 HTML 网络工具可以创建，并且能够吸引大量受众。例如这个 BMI 计算器。你可以利用该网站的流量，通过 Google 广告创造收入。我们可以使用 similarweb 工具来衡量和分析网站流量。我们可以看到该网站每月访问量超过 4000 万：

![使用 ChatGPT 生成被动收入的 4 种方法](img/45ebb4e35c8c6d1a51ad1b750751d73c.png)

BMI 计算器流量和参与度

利用这些巨大的流量，你可以通过 Google 广告创造大量收入。我们来使用 ChatGPT 帮助我们构建一个类似的工具，我们首先将这个提示作为输入：

> **你能创建一个 HTML BMI 计算器，让用户输入他的身高、体重、性别和年龄，并得到他的 BMI 吗？**

这是 ChatGPT 输出的代码：

```py
<!DOCTYPE html>
    <html>
    <head>
     <title>BMI Calculator</title>
     <style>
      body {
       font-family: Arial, sans-serif;
       background-color: #f2f2f2;
      }
      form {
       background-color: #fff;
       padding: 20px;
       border-radius: 5px;
       box-shadow: 0px 0px 10px 0px rgba(0,0,0,0.2);
       width: 400px;
       margin: auto;
      }
      input[type="submit"], input[type="reset"] {
       background-color: #4CAF50;
       color: #fff;
       padding: 10px 15px;
       border: none;
       border-radius: 3px;
       cursor: pointer;
       margin-top: 10px;
      }
      input[type="submit"]:hover, input[type="reset"]:hover {
       background-color: #3e8e41;
      }
      label {
       display: block;
       margin-bottom: 5px;
       font-weight: bold;
      }
      input[type="number"], select {
       padding: 10px;
       border-radius: 3px;
       border: 1px solid #ccc;
       width: 100%;
       box-sizing: border-box;
       margin-bottom: 10px;
       font-size: 16px;
      }
     </style>
    </head>
    <body>
     <form>
      <label for="height">Height (cm)</label>
      <input type="number" id="height" name="height" placeholder="Enter your height" required>
      <label for="weight">Weight (kg)</label>
      <input type="number" id="weight" name="weight" placeholder="Enter your weight" required>
      <label for="gender">Gender</label>
      <select id="gender" name="gender" required>
       <option value="">Select your gender</option>
       <option value="male">Male</option>
       <option value="female">Female</option>
      </select>
      <label for="age">Age</label>
      <input type="number" id="age" name="age" placeholder="Enter your age" required>
      <input type="submit" value="Calculate BMI">
      <input type="reset" value="Clear">
      <p>Your BMI is: <span id="bmi"></span></p>
      <p id="message"></p>
     </form>
     <script>
      function calculateBMI() {
       var height = document.getElementById("height").value;
       var weight = document.getElementById("weight").value;
       var gender = document.getElementById("gender").value;
       var age = document.getElementById("age").value;
       // convert height to meters
       var heightInMeters = height / 100;
       // calculate BMI
       var bmi = weight / (heightInMeters * heightInMeters);
       // display BMI
       document.getElementById("bmi").textContent = bmi.toFixed(2);
       // display message based on BMI
       var message = "";
       if (bmi < 18.5) {
        message = "You are underweight.";
       } else if (bmi >= 18.5 && bmi <= 24.9) {
        message = "You are healthy.";
       } else if (bmi >= 25 && bmi <= 29
```

然后你可以使用网站构建工具来运行此代码并建立网站。你可以创造出更具独特性、竞争较少的创意，以获得最多的流量。例如，你可以制作一个网站，根据每月的平均观看时长和观看次数来计算估计的 **Youtube** 收入。正如提到的，这一切都归结于你的创造力以及发现新创意的能力，这些创意应具有广泛的受众。

# 3\. 交易机器人指标

交易机器人的主要功能之一是指标，它们用于分析市场数据并生成自动交易系统的信号。这些指标可以是技术指标、基本面指标或两者的组合。技术指标使用基于市场数据（如价格和交易量）的数学计算来生成交易信号。另一方面，基本面指标分析宏观经济和公司特定数据以生成信号。

通常，指标的订阅费用每年或每 6 个月可能高达数百美元，因此这里是我们将用作 ChatGPT 输入的提示：

> **你能创建一个预设指标，当 25 分钟时间框架内的交易量增加 5%时触发买入信号吗**

你能创建一个预设指标，当 25 分钟时间框架内的交易量增加 5%时触发买入信号吗

这里是 ChatGPT 的输出：

```py
//@version=5
indicator("Volume Increase Alert", shorttitle="VIA")

// Inputs
var float volumeThresholdPercent = input(5, title="Volume Threshold (%)", minval=0, maxval=100)

// Calculate volume change percentage
var float volumePercentChange = change(volume) / volume * 100

// Check if volume percent change is greater than threshold on 25 minute time frame
var bool volumeIncrease25m = timeframe.isseconds and timeframe.multiplier == 25 and volumePercentChange >= volumeThresholdPercent

// Generate buy signal if volume increase on 25 minute time frame
if volumeIncrease25m
    strategy.entry("Buy", strategy.long)
    alert("Volume increase on 25 minute time frame!")
```

你可以将其集成到你的机器人中使用，或者创建你自己的交易机器人，并内置多个指标，或者将其出售给交易机器人。你越具创造性和实用性，就越能产生收入。记住，现在使用 AI 可以创建类似的工具，关键在于创造力，以便脱颖而出。

# 4\. 编写并出版儿童书籍

我们将重点关注儿童书籍，因为它们的字数较少，易于编写、出版和在亚马逊上销售。另一个建议是关注教育性主题，以提供一个故事，同时帮助孩子们变得更好，并具有教育意义，这样父母就会愿意购买。

所以让我们使用 ChatGPT 来写一个关于一个叫詹姆斯的男孩被老师欺负以及他如何克服这一困境的故事。以下是 ChatGPT 的输入提示：

> **写一个关于一个叫詹姆斯的男孩被老师欺负以及他如何克服这一困境的儿童故事**

这里是 ChatGPT 的输出：

从前，有一个名叫詹姆斯的小男孩，他喜欢上学。他喜欢学习新事物，交新朋友，探索周围的世界。然而，有一件事让詹姆斯上学变得很有挑战，那就是他的老师。

詹姆斯的老师，格赖姆斯女士，是一个严厉和令人畏惧的人，她以对学生苛刻而闻名。她经常在同学面前批评詹姆斯，称呼他难听的名字，甚至身体上推搡他。詹姆斯尽力忽视她的行为，但随着时间的推移，这变得越来越困难。

一天，在格赖姆斯女士的行为特别让人不安之后，詹姆斯决定他再也无法忍受了。他不想再感到害怕或自卑。因此，他决定与父母谈谈这件事。

他的父母仔细倾听了他，并告诉他格赖姆斯女士的行为是不对的。他们鼓励他为自己发声，不让任何人对他不好。詹姆斯感到宽慰，知道自己的父母支持他，并且他并不孤单。

第二天，詹姆斯带着新获得的自信去上学。当格赖姆斯女士试图在同学面前贬低他时，詹姆斯站出来为自己辩护，并告诉她他不再容忍被欺负了。格赖姆斯女士被詹姆斯的大胆举动吓到了，不知道如何回应。

从那天起，詹姆斯拒绝让格赖姆斯女士的行为影响自己。他继续努力学习，结交了支持他的朋友。格赖姆斯女士最终意识到她对詹姆斯的对待是错误的，并向他道歉。

最终，詹姆斯克服了他的恐惧，并对抗了他的欺负者。他学到了为自己发声的重要性，并且在亲人的支持下，一个人可以克服任何挑战。因此，詹姆斯在学校和生活中继续茁壮成长，知道自己有足够的力量去克服任何困难。

现在我们也可以使用 AI 来绘制图片。首先，我们将让 ChatGPT 为每个故事角色提供描述，我们可以立即使用这些描述生成图像。例如，我们首先会让 ChatGPT 使用以下提示来给我们描述詹姆斯：

> **你能详细描述一下詹姆斯的样子，以便我可以让 AI 图像生成工具为他绘制一幅图吗？**

以下是 ChatGPT 的输出：

詹姆斯是一个 10 岁的男孩，短而略微凌乱的深棕色头发。他有一双明亮的蓝色眼睛，当他开心时会闪闪发光。他的皮肤较白，鼻子和脸颊上有几颗雀斑。他右眉毛上方有一个小伤疤，是去年夏天骑自行车摔伤的。

詹姆斯的身高在同龄人中算是中等，身材苗条。他喜欢穿舒适的衣服，比如 T 恤和牛仔裤，他最喜欢的颜色是蓝色。他经常穿一双红色运动鞋，因为经常跑来跑去和在外面玩耍，所以鞋子有点磨损。

总体来说，詹姆斯看起来友好且容易接近，眼中透着一丝调皮。

现在我们可以使用任何图像生成工具来开始生成图片。在这个例子中，我将使用 [稳定扩散 Hugging Face](https://huggingface.co/spaces/stabilityai/stable-diffusion) 空间。以下是使用稳定扩散生成的图像，基于我们从 ChatGPT 得到的提示：

![使用稳定扩散生成的图像](img/b09036c35006c13b5a688453631841aa.png)

使用稳定扩散生成的图像

你还可以让 ChatGPT 为故事中的其他角色和场景提供描述，并可以运用自己的想象力，使用 AI 图像生成工具创建更多场景。

接下来，你可以使用像 [Book Bolt Studio](https://bookbolt.io/) 这样的书籍工作室来制作书籍，并将其转换为合适的格式。最后，你可以在亚马逊 KDP 上自助出版你的书籍，这是一个亚马逊自助出版平台。

你可以用多种语言创建多本儿童书籍，并开始在亚马逊上销售，正如之前所说的，想法和创意越多，你能赚到的钱也会越多。

总结来说，ChatGPT 将为建立产品和生成收入提供很多机会，一切都取决于你的创造力和市场理解。在本文中，我们集中介绍了四种赚钱的方法：构建 Chrome 扩展程序、创建网络工具、构建交易机器人指标和编写儿童故事。

**[尤瑟夫·拉法特](https://www.linkedin.com/in/youssef-hosni-b2960b135)** 是一名计算机视觉研究员和数据科学家。他的研究专注于开发用于医疗保健应用的实时计算机视觉算法。他还在营销、金融和医疗保健领域担任数据科学家超过 3 年。

### 更多相关主题

+   [使用稳定扩散生成超逼真的面孔的 3 种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [利用数据科学将收入翻倍的 5 种方法](https://www.kdnuggets.com/2022/05/5-ways-double-income-data-science.html)

+   [使用 Google MusicLM 从文本生成音乐](https://www.kdnuggets.com/2023/06/generate-music-text-google-musiclm.html)

+   [如何生成合成的表格数据集](https://www.kdnuggets.com/2022/03/generate-tabular-synthetic-dataset.html)

+   [使用开源工具生成合成时间序列数据](https://www.kdnuggets.com/2022/06/generate-synthetic-timeseries-data-opensource-tools.html)

+   [结合数据管理和数据讲述来创造价值](https://www.kdnuggets.com/combining-data-management-and-data-storytelling-to-generate-value)
