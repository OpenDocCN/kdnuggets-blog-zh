# 使用 USB 摄像头和无线连接的树莓派构建监控系统

> 原文：[`www.kdnuggets.com/2018/11/building-surveillance-system-usb-camera-wireless-connected-raspberry-pi.html/2`](https://www.kdnuggets.com/2018/11/building-surveillance-system-usb-camera-wireless-connected-raspberry-pi.html/2)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

### 3\. 使用 PyGame 捕获图像

“**fswebcam**”包对于快速测试摄像头是否正常工作非常有用。确保摄像头正常后，我们可以开始编写一个 Python 脚本，通过 PyGame 库连续捕获图像。以下代码使用 PyGame 捕获单张图像，打开一个窗口以显示该图像，并最终保存该图像。

```py
import pygame
import pygame.camera

# Captured image dimensions. It should be less than or equal to the maximum dimensions acceptable by the camera.
width = 320
height = 240

# Initializing PyGame and the camera.
pygame.init()
pygame.camera.init()

# Specifying the camera to be used for capturing images. If there is a single camera, then it have the index 0.
cam = pygame.camera.Camera("/dev/video0",(width,height))

# Preparing a resizable window of the specified size for displaying the captured images.
window = pygame.display.set_mode((width,height),pygame.RESIZABLE)

# Starting the camera for capturing images.
cam.start()

# Capturing an image.
image = cam.get_image()

# Stopping the camera.
cam.stop()

# Displaying the image on the window starting from the top-left corner.
window.blit(image,(0,0))

# Refreshing the window.
pygame.display.update()

# Saving the captured image.
pygame.image.save(window,'PyGame_image.jpg')

```

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的捷径

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你所在组织的 IT

* * *

假设上述代码保存在一个名为“**im_cap.py**”的 Python 文件中。要执行这些代码，我们可以从终端发出以下命令：

```py
pi@raspberry:~ $ python3 im_cam.py

```

这是执行该文件后显示的窗口。

![](img/70fb7b5dcdf168cdeb19ecf08f6c499f.png)

我们可以修改之前的代码以捕获多张图像。例如，我们可以使用**for**循环捕获预先指定数量的图像。我们也可以使用不限制图像数量的**while**循环。这里是修改后的代码，它使用**for**循环捕获 2,000 张图像。

```py
import pygame
import pygame.camera

# Captured image dimensions. It should be less than or equal to the maximum dimensions acceptable by the camera.
width = 320
height = 240

# Initializing PyGame and the camera.
pygame.init()
pygame.camera.init()

# Specifying the camera to be used for capturing images. If there is a single camera, then it has the index 0.
cam = pygame.camera.Camera("/dev/video0", (width, height))

# Preparing a resizable window of the specified size for displaying the captured images.
window = pygame.display.set_mode((width, height), pygame.RESIZABLE)

# Starting the camera for capturing images.
cam.start()

for im_num in range(0, 2000):
    print("Image : ", im_num)

    # Capturing an image.
    image = cam.get_image()

    # Displaying the image on the window starting from the top-left corner.
    window.blit(image, (0, 0))

    # Refreshing the window.
    pygame.display.update()

    # Saving the captured image.
    pygame.image.save(window, './pygame_images/image_' + str(im_num) + '.jpg')

# Stopping the camera.
cam.stop()

```

这是 8 张捕获的图像。请注意，摄像头的位置有些许变化。

![](img/e8f73bbe809027e9bf3f5ec9fc74063a.png)

### 4\. 构建背景模型

到目前为止，我们成功地构建了一个简单的监控系统，其中摄像头捕获的图像被保存在 RPi 的 SD 卡中。我们可以扩展该系统以自动检测场景变化。这是通过构建场景的背景模型来完成的。任何对该模型的更改都将指示一个变化。例如，如果有人经过场景，将导致背景的变化。

背景模型可以通过将多个捕获的图像平均化来简单创建，这些图像中不包含任何物体。因为我们关注的是颜色信息，这些图像将被转换为二进制。以下是用于构建背景模型的 Python 代码。

```py
import skimage.io
import os
import numpy

dir_files = os.listdir('./pygame_images/')

bg_image = skimage.io.imread(fname=dir_files[0], as_grey=True)

for k in range(1, len(dir_files)):
    fname = dir_files[k]
    im = skimage.io.imread(fname=fname, as_grey=True)
    bg_image = bg_image + im

bg_image = bg_image/(len(dir_files))
bg_image_bin = bg_image > 0.5

skimage.io.imsave(fname='bg_model.jpg', arr=bg_image)
skimage.io.imsave(fname='bg_model_bin.jpg', arr=bg_image_bin*255)

```

这是在平均 500 张图片后得到的灰度和二值背景模型。

![](img/bc07771e6f250b549b1f42728a842909.png)

### 5\. 检测背景模型的变化

在建立背景模型之后，我们可以测试新图片以检查背景是否发生变化。这可以通过将新图片转换为二值图像来简单实现。然后比较两张图像中的白色像素数量。如果数量超过给定的阈值，则表示背景发生了变化。阈值因场景而异。这里是用于测试新图片的`code`。

```py
bg_num_ones = numpy.sum(bg_image_bin)
test = skimage.io.imread(fname="./pygame_images/image_800.jpg", 
                         as_grey=True)
test_bin = test > 0.5
test_num_ones = numpy.sum(test_bin)
print("Num 1s in BG   :", bg_num_ones)
print("Num 1s in Test :", test_num_ones)

if(abs(test_num_ones-bg_num_ones) < 5000):
    print("Change.")

```

这里是一个测试图像，显示了颜色、灰度和二值图像中由于场景中出现物体（人）而与背景的变化。

![](img/d86cfb508869bb37d5643695b38cac51.png)

### 6\. 建立一个简单的电路，当发生变化时点亮 LED 灯

作为背景模型变化的指示，我们可以建立一个简单的电路，当检测到变化时 LED 灯会亮起。这个电路将连接到 RPi 的 GPIO（通用输入输出）引脚。电路需要以下组件：

+   一个面包板。

+   一个 LED 灯。

+   一个电阻（大于或等于 100 欧姆）。我使用的是 178.8 欧姆的电阻。

+   两根公/公跳线。

+   两根公/母跳线。

在将电路连接到 GPIO 引脚之前，建议先进行测试。这是因为如果电阻值选择不当，可能不仅会烧毁 LED，还会损坏 GPIO 引脚。测试时需要一个电池为面包板供电。这里是正确连接所有组件后的电路图。

![](img/af2906aa9e63f23914c18876149135cb.png)

然后，我们可以拆下电池，并将面包板连接到 RPi 的 GPIO 引脚。根据 GPIO 引脚的面包板编号，地连接到引脚号 20，高电压连接到输出引脚号 22。下图展示了面包板与 RPi 之间的连接。RPi 也连接到充电器和 USB 摄像头。

![](img/f917ae6b5def74ff37d2bb3f20c6398f.png)

输出 GPIO 引脚由以下 Python 脚本控制。其默认状态为 LOW，表示 LED 灯熄灭。当背景发生变化时，状态会变为 HIGH，表示 LED 灯点亮。LED 灯会亮 0.1 秒，然后状态恢复为熄灭。当另一个输入图像与背景不同时，LED 灯会再亮 0.1 秒。

```py
import time
import RPi.GPIO
import skimage.io
import numpy
import os
import pygame.camera
import pygame

#####GPIO#####
# Initializing the GPIO pins. The numbering using is board.
RPi.GPIO.setmode(RPi.GPIO.BOARD)

# Configuring the GPIO bin number 22 to be an output bin.
RPi.GPIO.setup(22, RPi.GPIO.OUT)

#####PyGame#####
# Initializing PyGame and the camera.
pygame.init()
pygame.camera.init()

# Captured image dimensions. It should be less than or equal to the maximum dimensions acceptable by the camera.
width = 320
height = 240

# Preparing a resizable window of the specified size for displaying the captured images.
window = pygame.display.set_mode((width, height), pygame.RESIZABLE)

# Specifying the camera source and the image dimensions.
cam = pygame.camera.Camera("/dev/video0",(width,height))
cam.start()

#####Background Model#####
# Reading the background model.
bg_image = skimage.io.imread(fname='bg_model_bin.jpg', as_grey=True)
bg_image_bin = bg_image > 0.5
bg_num_ones = numpy.sum(bg_image_bin)

im_dir = '/home/pi/pygame_images/'

for im_num in range(0, 2000):
    print("Image : ", im_num)

    im = cam.get_image()

    # Displaying the image on the window starting from the top-left corner.
    window.blit(im, (0, 0))

    # Refreshing the window.
    pygame.display.update()

    pygame.image.save(window, im_dir+'image'+str(im_num)+'.jpg')
    im = pygame.surfarray.array3d(window)

    test_bin = im > 0.5
    test_num_ones = numpy.sum(test_bin)

    # Checking if there is a change in the test image.
    if (abs(test_num_ones - bg_num_ones) < 5000):
        print("Change.")
        try:
            RPi.GPIO.output(22, RPi.GPIO.HIGH)
            time.sleep(0.1)
            RPi.GPIO.output(22, RPi.GPIO.LOW)
        except KeyboardInterrupt:  # CTRL+C
            print("Keyboard Interrupt.")
        except:
            print("Error occurred.")

# Stopping the camera.
cam.stop()

# cleanup all GPIO pins.
print("Clean Up GPIO.")
RPi.GPIO.cleanup()

```

下图显示了一张输入图像，由于存在一个人，它与背景图像不同。因此，LED 灯将亮 0.1 秒钟。

![](img/3024e1ed52ae5a4c77d2b20f56c94497.png)

以下视频（[`youtu.be/WOUG-vjg3A4`](https://youtu.be/WOUG-vjg3A4)）展示了使用相机捕获的多个帧的 LED 状态。当输入图像与背景模型根据使用的阈值不同，LED 会被点亮。

**更多详情**

Ahmed Fawzy Gad, “在树莓派上构建图像分类器”，2018 年 9 月，[`www.linkedin.com/pulse/building-image-classifier-running-raspberry-pi-ahmed-gad`](https://www.linkedin.com/pulse/building-image-classifier-running-raspberry-pi-ahmed-gad)

**联系作者**

+   电子邮件: [ahmed.f.gad@gmail.com](https://mailto:ahmed.f.gad@gmail.com/)

+   LinkedIn: [`linkedin.com/in/ahmedfgad/`](https://linkedin.com/in/ahmedfgad/)

+   KDnuggets: [`www.kdnuggets.com/author/ahmed-gad`](https://www.kdnuggets.com/author/ahmed-gad)

+   YouTube: [`youtube.com/AhmedGadFCIT`](https://youtube.com/AhmedGadFCIT)

+   TowardsDataScience: [`towardsdatascience.com/@ahmedfgad`](https://towardsdatascience.com/@ahmedfgad)

**简介: [Ahmed Gad](https://www.linkedin.com/in/ahmedfgad/)**于 2015 年 7 月在埃及门努非亚大学计算机与信息学院（FCI）获得信息技术学士学位，并以优异的成绩毕业。由于在学院中排名第一，他在 2015 年被推荐担任埃及某学院的教学助理，并在 2016 年继续担任教学助理和研究员。他目前的研究兴趣包括深度学习、机器学习、人工智能、数字信号处理和计算机视觉。

[原文](https://www.linkedin.com/pulse/building-surveillance-system-using-usb-camera-raspberry-ahmed-gad/)。经许可转载。

**相关:**

+   直观的集成学习指南：梯度提升

+   在树莓派上构建图像分类器

+   Python 中遗传算法的实现

### 相关主题

+   [用 Python 构建亚马逊产品推荐系统](https://www.kdnuggets.com/2023/02/building-recommender-system-amazon-products-python.html)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [等级系统如何帮助预测 AI 成本](https://www.kdnuggets.com/2022/03/level-system-help-forecast-ai-costs.html)

+   [系统设计学习：前 5 本必读书籍](https://www.kdnuggets.com/learning-system-design-top-5-essential-reads)

+   [使用 Python 的 Watchdog 监控文件系统](https://www.kdnuggets.com/monitor-your-file-system-with-pythons-watchdog)

+   [使用 Llama 和 ChatGPT 构建多聊天后端的微服务](https://www.kdnuggets.com/building-microservice-for-multichat-backends-using-llama-and-chatgpt)
