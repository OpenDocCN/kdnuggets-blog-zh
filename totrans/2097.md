# 介绍 Falcon2：TII 的下一代语言模型

> 原文：[https://www.kdnuggets.com/introducing-falcon2-next-gen-language-model-by-tii](https://www.kdnuggets.com/introducing-falcon2-next-gen-language-model-by-tii)

![Falcon2](../Images/d2e9caefaf85567b2d00eb13c5cadc25.png)

图片来源于作者

阿布扎比的技术创新研究所（TII）于 5 月 14 日发布了其下一系列 Falcon 语言模型。新模型符合 TII 作为技术推动者的使命，并作为开源模型在 HuggingFace 上提供。他们发布了两种 Falcon 2 模型变体：**Falcon-2-11B** 和 **Falcon-2-11B-VLM**。新的 VLM 模型承诺具有卓越的多模型兼容性，性能与其他开源和闭源模型相当。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持您的组织的 IT

* * *

## 模型特性与性能

最近的 Falcon-2 语言模型拥有 110 亿个参数，并在来自 [falcon-refinedweb](https://huggingface.co/datasets/tiiuae/falcon-refinedweb) 数据集的 5.5 万亿个标记上进行了训练。更新的、更高效的模型与 Meta 最近的 Llama3 模型（拥有 80 亿个参数）竞争良好。结果汇总在下面由 TII 分享的表格中：

![Falcon 2 结果](../Images/3be394b7d26dd694b77312d3b8ddb8a3.png)

图片来源于 [TII](https://falconllm.tii.ae/falcon-2.html)

此外，Falcon-2 模型在与谷歌的 Gemma（拥有 70 亿个参数）的比较中表现良好。Gemma-7B 仅比 Falcon-2 平均性能高出 0.01。该模型还是多语言的，训练了包括英语、法语、西班牙语和德语等常用语言。

然而，突破性的成就是 Falcon-2-11B 视觉语言模型的发布，该模型在同一语言模型中增加了图像理解和多模态功能。与 Llama3 和 Gemma 等近期模型具有相当能力的图像到文本对话能力是一项重要进展。

## 如何使用模型进行推断

让我们进入编码部分，这样我们就可以在本地系统上运行模型并生成响应。首先，像处理其他项目一样，让我们设置一个新的环境以避免依赖冲突。由于模型刚刚发布，我们将需要所有库的最新版本，以避免缺少支持和管道。

创建一个新的 Python 虚拟环境，并使用以下命令激活它：

```py
python -m venv venv
source venv/bin/activate
```

现在我们有一个干净的环境，我们可以使用 Python 包管理器安装所需的库和依赖项。对于这个项目，我们将使用互联网上可用的图像并在 Python 中加载它们。`requests` 和 `Pillow` 库适用于此目的。此外，为了加载模型，我们将使用 `transformers` 库，该库内置支持 HuggingFace 模型加载和推理。我们将使用 `bitsandbytes`、`PyTorch` 和 `accelerate` 作为模型加载实用工具和量化工具。

为了简化设置过程，我们可以创建一个简单的需求文本文件，如下所示：

```py
# requirements.txt
accelerate  # For distributed loading
bitsandbytes	# For Quantization
torch   # Used by HuggingFace
transformers	# To load pipelines and models
Pillow  # Basic Loading and Image Processing
requests	# Downloading image from URL 
```

现在我们可以使用以下命令在一行中安装所有依赖项：

```py
pip install -r requirements.txt
```

我们现在可以开始编写代码以使用模型进行推理。让我们从在本地系统中加载模型开始。模型可以在 [HuggingFace](https://huggingface.co/tiiuae/falcon-11B-vlm) 找到，总大小超过 20GB。我们无法在通常具有 8-16GB 内存的消费级 GPU 上加载模型。因此，我们需要对模型进行量化，即将模型加载为 4 位浮点数，而不是通常的 32 位精度，以减少内存需求。

`bitsandbytes` 库提供了一个简单的接口，用于量化 HuggingFace 中的大型语言模型。我们可以初始化一个量化配置并将其传递给模型。HuggingFace 内部处理所有必要的操作，并为我们设置正确的精度和调整。配置可以设置如下：

```py
from transformers import BitsAndBytesConfig
quantization_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_quant_type="nf4",
  	# Original model support BFloat16
    bnb_4bit_compute_dtype=torch.bfloat16,
) 
```

这使得模型可以适应 16GB GPU 内存，从而更容易加载模型而无需卸载和分发。我们现在可以加载 Falcon-2B-VLM。作为一个多模态模型，我们将同时处理图像和文本提示。`LLava` 模型和管道专为此目的而设计，它们允许基于 CLIP 的图像嵌入投影到语言模型输入中。`transformers` 库具有内置的 Llava 模型处理器和管道。我们可以按如下方式加载模型：

```py
from transformers import LlavaNextForConditionalGeneration, LlavaNextProcessor
processor = LlavaNextProcessor.from_pretrained(
	"tiiuae/falcon-11B-vlm",
	tokenizer_class='PreTrainedTokenizerFast'
)
model = LlavaNextForConditionalGeneration.from_pretrained(
	"tiiuae/falcon-11B-vlm",
	quantization_config=quantization_config,
	device_map="auto"
) 
```

我们将 HuggingFace 模型卡上的模型 URL 传递给处理器和生成器。我们还将 `bitsandbytes` 量化配置传递给生成模型，这样它将自动以 4 位精度加载。

我们现在可以开始使用模型生成响应！为了探索 Falcon-11B 的多模态特性，我们需要在 Python 中加载一张图像。作为测试样本，我们加载这个标准图像 [这里](http://images.cocodataset.org/val2017/000000039769.jpg)。要从网络 URL 加载图像，我们可以使用 `Pillow` 和 `requests` 库，如下所示：

```py
from Pillow import Image
import requests

url = "https://static.theprint.in/wp-content/uploads/2020/07/football.jpg"
img = Image.open(requests.get(url, stream=True).raw)
```

`requests` 库从 URL 下载图像，`Pillow` 库可以将字节读取为标准图像格式。现在我们有了测试图像，我们可以从模型生成示例响应。

让我们设置一个示例提示模板，模型对其很敏感。

```py
instruction = "Write a long paragraph about this picture."
prompt = f"""User:<image>\n{instruction} Falcon:"""
```

提示模板本身是自解释的，我们需要按照它来获得VLM的最佳响应。我们将提示和图像传递给Llava图像处理器。它内部使用CLIP创建图像和提示的组合嵌入。

```py
inputs = processor(
	prompt,
	images=img,
	return_tensors="pt",
	padding=True
).to('cuda:0') 
```

返回的张量嵌入作为生成模型的输入。我们传递这些嵌入，基于原始提供的图像和指令，使用基于变换器的Falcon-11B模型生成文本响应。

我们可以使用以下代码生成响应：

```py
output = model.generate(**inputs, max_new_tokens=256)
generated_captions = processor.decode(output[0], skip_special_tokens=True).strip()
```

就这样！`generated_captions`变量是一个字符串，包含了模型生成的响应。

## 结果

我们使用上述代码测试了各种图像，对其中一些图像的响应进行了总结，见下图。我们看到，Falcon-2模型对图像有很强的理解能力，并生成了清晰的答案，显示出其对图像场景的理解。它可以读取文本，还突出显示了整体信息。总之，模型在视觉任务上表现出色，可用于基于图像的对话。

![Falcon 2推理结果](../Images/69694f523f6c6f8effcbbe361ca41ddd.png)

图片来源：作者 | 图片来自互联网。来源：[猫咪图片](http://images.cocodataset.org/val2017/000000039769.jpg)、[卡片图片](https://usa.visa.com/dam/VCOM/global/common-assets/cards/visa-prepaid-card-800x450.png)、[足球图片](https://static.theprint.in/wp-content/uploads/2020/07/football.jpg)

## 许可与合规

除了开源外，这些模型还以Apache2.0许可证发布，使其可供公开访问。这允许对模型进行修改和分发，用于个人和商业用途。这意味着你现在可以使用Falcon-2模型来提升基于LLM的应用程序，并利用开源模型为用户提供多模态能力。

## 总结

总体来说，新版Falcon-2模型显示了令人鼓舞的结果。但这还不是全部！TII已经在着手下一次迭代，以进一步提升性能。他们计划将专家混合（MoE）和其他机器学习能力整合到模型中，以提高准确性和智能性。如果Falcon-2看起来像是一个改进，请准备好迎接他们的下一个公告。

**[](https://www.linkedin.com/in/kanwal-mehreen1/)**[Kanwal Mehreen](https://www.linkedin.com/in/kanwal-mehreen1/)** Kanwal 是一位机器学习工程师和技术作家，对数据科学及AI与医学的交集充满深厚的热情。她共同撰写了电子书《使用ChatGPT最大化生产力》。作为2022年亚太地区的Google Generation学者，她倡导多样性和学术卓越。她还被认定为Teradata多样性技术学者、Mitacs Globalink研究学者和哈佛WeCode学者。Kanwal 是变革的坚定倡导者，她创立了FEMCodes，旨在赋能女性在STEM领域中的发展。

### 更多相关主题

+   [介绍自然语言处理测试库](https://www.kdnuggets.com/2023/04/introducing-testing-library-natural-language-processing.html)

+   [介绍 John Snow Labs 的医疗专用大型语言模型](https://www.kdnuggets.com/2023/04/john-snow-introducing-healthcare-specific-large-language-models-john-snow-labs.html)

+   [介绍 TPU v4：谷歌前沿超算用于大型语言模型](https://www.kdnuggets.com/2023/04/introducing-tpu-v4-googles-cutting-edge-supercomputer-large-language-models.html)

+   [介绍 Objectiv：开源产品分析基础设施](https://www.kdnuggets.com/2022/06/objectiv-introducing-objectiv-opensource-product-analytics-infrastructure.html)

+   [介绍 OpenChat：构建自定义聊天机器人的免费简易平台](https://www.kdnuggets.com/2023/06/introducing-openchat-free-simple-platform-building-custom-chatbots-minutes.html)

+   [介绍 OpenLLM：开源大型语言模型库](https://www.kdnuggets.com/2023/07/introducing-openllm-open-source-library-llms.html)
