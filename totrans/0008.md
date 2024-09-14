# 扩散与去噪：解释文本到图像生成 AI

> 原文：[https://www.kdnuggets.com/diffusion-and-denoising-explaining-text-to-image-generative-ai](https://www.kdnuggets.com/diffusion-and-denoising-explaining-text-to-image-generative-ai)

![扩散与去噪：解释文本到图像生成 AI](../Images/ddf316fd3e2893f67a9d64b6401c6fef.png)

## 扩散的概念

去噪扩散模型经过训练以从噪声中提取模式，从而生成期望的图像。训练过程包括向模型展示具有不同噪声水平的图像（或其他数据），噪声水平是根据噪声调度算法确定的，目的是预测数据中的哪些部分是噪声。如果成功，噪声预测模型将能够从纯噪声中逐渐构建出逼真的图像，在每一步中从图像中减去噪声的增量。

* * *

## 我们的前三个课程推荐

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业。

![](../Images/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](../Images/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

![扩散和去噪过程](../Images/e53be5f6aad31b14b122d1a17b57103c.png)

与本节顶部的图像不同，现代扩散模型不会直接从添加噪声的图像中预测噪声。相反，它们在图像的潜在空间表示中预测噪声。潜在空间以压缩的数值特征集表示图像，这是变分自编码器或 [VAE](https://en.wikipedia.org/wiki/Variational_autoencoder) 编码模块的输出。这个技巧使得“潜在”被包含在 [潜在扩散](https://arxiv.org/pdf/2112.10752.pdf) 中，并大大减少了生成图像的时间和计算需求。正如论文作者所报告的，潜在扩散使得推理速度比直接扩散快至少约 2.7 倍，并且训练速度快约三倍。

使用潜在扩散的人们经常谈到使用“扩散模型”，但实际上，扩散过程涉及几个模块。如上图所示，文本到图像工作流程的扩散管道通常包括一个文本嵌入模型（及其分词器）、一个去噪预测/扩散模型和一个图像解码器。潜在扩散的另一个重要部分是调度器，它决定了噪声如何在一系列“时间步骤”（一系列逐步更新，逐渐从潜在空间中去除噪声）中被缩放和更新。

![潜在扩散模型架构图](../Images/e3eb94eb011d1bc8106e89dc07afa10a.png)

## 潜在扩散代码示例

我们将在大多数示例中使用 CompVis/latent-diffusion-v1-4。文本嵌入由一个[CLIPTextModel 和 CLIPTokenizer](https://en.wikipedia.org/wiki/DALL-E#Contrastive_Language-Image_Pre-training_(CLIP)) 处理。噪声预测使用一个‘[U-Net](https://en.wikipedia.org/wiki/U-Net)’，这是一种图像到图像的模型，最初在生物医学图像（特别是分割）中获得了广泛应用。为了从去噪的潜在数组生成图像，该管道使用变分自动编码器 ([VAE](https://en.wikipedia.org/wiki/Variational_autoencoder)) 进行图像解码，将这些数组转换成图像。

我们将从构建我们自己版本的 HuggingFace 组件管道开始。

```py
# local setup
virtualenv diff_env –python=python3.8
source diff_env/bin/activate
pip install diffusers transformers huggingface-hub
pip install torch --index-url https://download.pytorch.org/whl/cu118
```

如果你在本地工作，确保检查[ pytorch.org](https://pytorch.org/get-started/locally/)以确保适合你系统的版本。我们的导入相对简单，下面的代码片段足以用于所有后续演示。

```py
import os
import numpy as np
import torch
from diffusers import StableDiffusionPipeline, AutoPipelineForImage2Image
from diffusers.pipelines.pipeline_utils import numpy_to_pil
from transformers import CLIPTokenizer, CLIPTextModel
from diffusers import AutoencoderKL, UNet2DConditionModel, \
       PNDMScheduler, LMSDiscreteScheduler

from PIL import Image
import matplotlib.pyplot as plt
```

现在进入细节。首先定义图像和扩散参数以及提示。

```py
prompt = [" "]

# image settings
height, width = 512, 512

# diffusion settings
number_inference_steps = 64
guidance_scale = 9.0
batch_size = 1
```

用你选择的种子初始化伪随机数生成器，以便复现结果。

```py
def seed_all(seed):
    torch.manual_seed(seed)
    np.random.seed(seed)

seed_all(193)
```

现在我们可以初始化文本嵌入模型、自动编码器、U-Net 和时间步调度器。

```py
tokenizer = CLIPTokenizer.from_pretrained("openai/clip-vit-large-patch14")
text_encoder = CLIPTextModel.from_pretrained("openai/clip-vit-large-patch14")
vae = AutoencoderKL.from_pretrained("CompVis/stable-diffusion-v1-4", \
        subfolder="vae")
unet = UNet2DConditionModel.from_pretrained("CompVis/stable-diffusion-v1-4",\
        subfolder="unet")
scheduler = PNDMScheduler()
scheduler.set_timesteps(number_inference_steps)

my_device = torch.device("cuda") if torch.cuda.is_available() else torch.device("cpu")
vae = vae.to(my_device)
text_encoder = text_encoder.to(my_device)
unet = unet.to(my_device)
```

将文本提示编码为嵌入需要首先对字符串输入进行分词。分词将字符替换为对应于语义单元词汇的整数代码，例如通过字节对编码 ([BPE](https://en.wikipedia.org/wiki/Byte_pair_encoding))。我们的管道在图像的文本提示旁边嵌入了一个空提示（无文本）。这在提供的描述和一般自然出现的图像之间平衡了扩散过程。我们将在本文稍后看到如何改变这些组件的相对权重。

```py
prompt = prompt * batch_size
tokens = tokenizer(prompt, padding="max_length",\
max_length=tokenizer.model_max_length, truncation=True,\
        return_tensors="pt")

empty_tokens = tokenizer([""] * batch_size, padding="max_length",\
max_length=tokenizer.model_max_length, truncation=True,\
        return_tensors="pt")
with torch.no_grad():
    text_embeddings = text_encoder(tokens.input_ids.to(my_device))[0]
    max_length = tokens.input_ids.shape[-1]
    notext_embeddings = text_encoder(empty_tokens.input_ids.to(my_device))[0]
    text_embeddings = torch.cat([notext_embeddings, text_embeddings])
```

我们将潜在空间初始化为随机正态噪声，并根据我们的扩散时间步调度器对其进行缩放。

```py
latents = torch.randn(batch_size, unet.config.in_channels, \
        height//8, width//8)
latents = (latents * scheduler.init_noise_sigma).to(my_device)
```

一切准备就绪，我们可以深入扩散循环本身。我们可以通过周期性采样来跟踪图像，从而查看噪声如何逐渐减少。

```py
images = []
display_every = number_inference_steps // 8

# diffusion loop
for step_idx, timestep in enumerate(scheduler.timesteps):
    with torch.no_grad():
        # concatenate latents, to run null/text prompt in parallel.
        model_in = torch.cat([latents] * 2)
        model_in = scheduler.scale_model_input(model_in,\
                timestep).to(my_device)
        predicted_noise = unet(model_in, timestep, \
                encoder_hidden_states=text_embeddings).sample
        # pnu - empty prompt unconditioned noise prediction
        # pnc - text prompt conditioned noise prediction
        pnu, pnc = predicted_noise.chunk(2)
        # weight noise predictions according to guidance scale
        predicted_noise = pnu + guidance_scale * (pnc - pnu)
        # update the latents
        latents = scheduler.step(predicted_noise, \
                timestep, latents).prev_sample
        # Periodically log images and print progress during diffusion
        if step_idx % display_every == 0\
                or step_idx + 1 == len(scheduler.timesteps):
           image = vae.decode(latents / 0.18215).sample[0]
           image = ((image / 2.) + 0.5).cpu().permute(1,2,0).numpy()
           image = np.clip(image, 0, 1.0)
           images.extend(numpy_to_pil(image))
           print(f"step {step_idx}/{number_inference_steps}: {timestep:.4f}")
```

在扩散过程结束时，我们会得到一个不错的渲染结果。接下来，我们将讨论额外的技术以获得更大的控制力。由于我们已经创建了扩散管道，我们可以使用 HuggingFace 提供的精简扩散管道来进行接下来的示例。

## 控制扩散管道

我们将在这一部分使用一组辅助函数：

```py
def seed_all(seed):
    torch.manual_seed(seed)
    np.random.seed(seed)

def grid_show(images, rows=3):
    number_images = len(images)
    height, width = images[0].size
    columns = int(np.ceil(number_images / rows))
    grid = np.zeros((height*rows,width*columns,3))
    for ii, image in enumerate(images):
        grid[ii//columns*height:ii//columns*height+height, \
                ii%columns*width:ii%columns*width+width] = image
        fig, ax = plt.subplots(1,1, figsize=(3*columns, 3*rows))
        ax.imshow(grid / grid.max())
    return grid, fig, ax

def callback_stash_latents(ii, tt, latents):
    # adapted from fastai/diffusion-nbs/stable_diffusion.ipynb
    latents = 1.0 / 0.18215 * latents
    image = pipe.vae.decode(latents).sample[0]
    image = (image / 2\. + 0.5).cpu().permute(1,2,0).numpy()
    image = np.clip(image, 0, 1.0)
    images.extend(pipe.numpy_to_pil(image))

my_seed = 193
```

我们将从扩散模型最著名且最直接的应用开始：从文本提示生成图像，即文本到图像生成。我们将使用的模型是由发布了潜在扩散论文的学术[实验室](https://ommer-lab.com/)发布到 Hugging Face Hub 的。Hugging Face 通过方便的管道 API 协调像潜在扩散这样的工作流。我们需要根据是否拥有 GPU 来定义计算的设备和浮点数。

```py
if (1):
    #Run CompVis/stable-diffusion-v1-4 on GPU
    pipe_name = "CompVis/stable-diffusion-v1-4"
    my_dtype = torch.float16
    my_device = torch.device("cuda")
    my_variant = "fp16"
    pipe = StableDiffusionPipeline.from_pretrained(pipe_name,\
    safety_checker=None, variant=my_variant,\
        torch_dtype=my_dtype).to(my_device)
else:
    #Run CompVis/stable-diffusion-v1-4 on CPU
    pipe_name = "CompVis/stable-diffusion-v1-4"
    my_dtype = torch.float32
    my_device = torch.device("cpu")
    pipe = StableDiffusionPipeline.from_pretrained(pipe_name, \
            torch_dtype=my_dtype).to(my_device)
```

### 引导比例

如果你使用非常不寻常的文本提示（与数据集中的提示大相径庭），可能会进入潜在空间的较少涉足部分。空提示嵌入提供了平衡，按照guidance_scale将两者结合起来可以在提示的具体性与常见图像特征之间进行权衡。

```py
guidance_images = []
for guidance in [0.25, 0.5, 1.0, 2.0, 4.0, 6.0, 8.0, 10.0, 20.0]:
    seed_all(my_seed)
    my_output = pipe(my_prompt, num_inference_steps=50, \
    num_images_per_prompt=1, guidance_scale=guidance)
    guidance_images.append(my_output.images[0])
    for ii, img in enumerate(my_output.images):
        img.save(f"prompt_{my_seed}_g{int(guidance*2)}_{ii}.jpg")

temp = grid_show(guidance_images, rows=3)
plt.savefig("prompt_guidance.jpg")
plt.show()
```

由于我们使用 9 个引导系数生成了提示，你可以绘制提示并查看扩散是如何发展的。默认引导系数为 0.75，因此第 7 张图像将是默认图像输出。

### 负面提示

有时潜在扩散确实“想要”产生与您的意图不匹配的图像。在这些情况下，你可以使用负面提示将扩散过程推向不希望的输出。例如，我们可以使用负面提示来使我们的火星宇航员扩散输出看起来稍微少一些人性。

```py
my_prompt = " "
my_negative_prompt = " "

output_x = pipe(my_prompt, num_inference_steps=50, num_images_per_prompt=9, \
        negative_prompt=my_negative_prompt)

temp = grid_show(output_x)
plt.show()
```

你应该获得遵循提示的输出，同时避免输出负面提示中描述的内容。

### 图像变体

从头开始的文本到图像生成并不是扩散管道的唯一应用。实际上，扩散非常适合从初始图像开始进行图像修改。我们将使用一个稍有不同的管道和为图像到图像扩散调优的预训练模型。

```py
pipe_img2img = AutoPipelineForImage2Image.from_pretrained(\

        "runwayml/stable-diffusion-v1-5", safety_checker=None,\

torch_dtype=my_dtype, use_safetensors=True).to(my_device)
```

这种方法的一种应用是生成主题的变体。概念艺术家可能会使用此技术快速迭代不同的外星行星插图创意，基于最新的研究。

我们将首先下载一张公共领域艺术家的 TRAPPIST 系统中 1e 行星的概念图（[来源：NASA/JPL-Caltech](https://photojournal.jpl.nasa.gov/catalog/PIA22093)）。

然后，在降采样以去除细节后，我们将使用扩散流程制作几种不同版本的外星行星 TRAPPIST-1e。

```py
url = \
"https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/TRAPPIST-1e_artist_impression_2018.png/600px-TRAPPIST-1e_artist_impression_2018.png"
img_path = url.split("/")[-1]
if not (os.path.exists("600px-TRAPPIST-1e_artist_impression_2018.png")):
    os.system(f"wget \     '{url}'")
    init_image = Image.open(img_path)

seed_all(my_seed)

trappist_prompt = "Artist's impression of TRAPPIST-1e"\
                  "large Earth-like water-world exoplanet with oceans,"\
                  "NASA, artist concept, realistic, detailed, intricate"

my_negative_prompt = "cartoon, sketch, orbiting moon"

my_output_trappist1e = pipe_img2img(prompt=trappist_prompt, num_images_per_prompt=9, \
     image=init_image, negative_prompt=my_negative_prompt, guidance_scale=6.0)

grid_show(my_output_trappist1e.images)
plt.show()
```

![扩散图像变体测试](../Images/23eb7c1af72b1ce4acf0e8bc46b4e436.png)

通过向模型提供一个初始图像示例，我们可以生成类似的图像。你还可以使用文本引导的图像到图像流程，通过增加引导、添加负面提示以及更多如“非现实主义”或“水彩”或“纸上素描”等来改变图像的风格。你的结果可能会有所不同，调整提示将是找到你想创建的正确图像的最简单方法。

## 结论

尽管关于扩散系统和模仿人类生成艺术的讨论依然存在，但扩散模型还有其他更具影响力的用途。它已被 [应用于](https://github.com/microsoft/foldingdiff) [蛋白质折叠预测](https://www.pnas.org/doi/10.1073/pnas.0910390107)，用于蛋白质设计和药物开发。文本到视频也是一个 [活跃的领域](https://arxiv.org/abs/2311.15127) 的 [研究](https://arxiv.org/abs/2211.13221)，并由多个公司提供（例如 [Stability AI](https://stability.ai/stable-video)、[Google](https://imagen.research.google/video/)）。扩散也是一个 [新兴的方法](https://arxiv.org/abs/2305.04120) 用于 [文本到语音](https://arxiv.org/abs/2304.11750) 应用。

很明显，扩散过程在人工智能的演变以及技术与全球人类环境的互动中正发挥着核心作用。虽然版权、其他知识产权法律的复杂性以及对人类艺术和科学的影响既有积极也有消极的表现，但真正积极的是人工智能前所未有的语言理解和图像生成能力。曾经是 AlexNet 使计算机分析图像并输出文本，而现在计算机可以分析文本提示并输出连贯的图像。

[原文](https://www.exxactcorp.com/blog/deep-learning/diffusion-and-denoising-explaining-text-to-image-generative-ai)。经许可转载。

**[Kevin Vu](https://blog.exxactcorp.com/)** 负责管理 [Exxact Corp 博客](https://blog.exxactcorp.com/)，并与许多才华横溢的作者合作，这些作者写作内容涉及深度学习的不同方面。

### 相关话题

+   [生成型人工智能游乐场：文本到图像的稳定扩散…](https://www.kdnuggets.com/2024/02/intel-generative-ai-playground-text-to-image-stable-diffusion)

+   [稳定扩散：生成型人工智能的基本直觉](https://www.kdnuggets.com/2023/06/stable-diffusion-basic-intuition-behind-generative-ai.html)

+   [使用 Phraser 和稳定扩散成为 AI 艺术家](https://www.kdnuggets.com/2022/09/become-ai-artist-phraser-stable-diffusion.html)

+   [使用稳定扩散生成超现实面孔的三种方法](https://www.kdnuggets.com/3-ways-to-generate-hyper-realistic-faces-using-stable-diffusion)

+   [7 大基于扩散的应用及演示](https://www.kdnuggets.com/2022/10/top-7-diffusionbased-applications-demos.html)

+   [解释可解释性人工智能在对话中的应用](https://www.kdnuggets.com/2022/10/explaining-explainable-ai-conversations.html)
