[返回新的　README](README.md)

以下是中文翻译版本：

# "Talking Head(?) Anime from A Single Image 3: Now the Body Too" 的示例代码

本仓库包含了 [Talking Head(?) Anime from a Single Image 3: Now the Body Too](https://pkhungurn.github.io/talking-head-anime-3/index.html) 项目的示例程序。项目名称暗示，该项目允许您动画化动漫角色，您只需要该角色的单张图像即可。有两个示例程序：

* ``manual_poser`` 允许您通过图形用户界面操纵角色的面部表情、头部旋转、身体旋转和胸部扩张。
* ``ifacialmocap_puppeteer`` 允许您将您的面部动作传递给动漫角色。

## 在 Google Colab 上尝试手动 poser

如果您没有所需的硬件（在下面讨论）或不想下载代码并设置环境来运行它，请单击 [![this link](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/pkhungurn/talking-head-anime-3-demo/blob/master/colab.ipynb) 在 [Google Colab](https://research.google.com/colaboratory/faq.html) 上尝试运行手动 poser。

## 硬件要求

这两个程序都需要一个最新的和强大的 Nvidia GPU 来运行。我个人可以用 Nvidia Titan RTX 运行它们。然而，我认为最近的高端游戏 GPU，如 RTX 2080、RTX 3080 或更好的 GPU 也能做到。

`ifacialmocap_puppeteer` 需要一个能够从视频流计算 [blend shape parameters](https://developer.apple.com/documentation/arkit/arfaceanchor/2928251-blendshapes) 的 iOS 设备。这意味着该设备必须能够运行 iOS 11.0 或更高版本，并必须具有 TrueDepth 前置摄像头。（有关更多信息，请参阅 [此页面](https://developer.apple.com/documentation/arkit/content_anchors/tracking_and_visualizing_faces)。）换句话说，如果您有 iPhone X 或更好的手机，您就应该一切就绪。我个人使用的是 iPhone 12 mini。

## 软件要求

### GPU 相关软件

请更新您 GPU 的设备驱动程序并安装与您的 GPU 兼容且比下一个子节中要安装的版本更新的 [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit)。

### Python 环境

``manual_poser`` 和 ``ifacialmocap_puppeteer`` 都可以作为桌面应用程序使用。要运行它们，您需要为运行用 [Python](http://www.python.org) 语言编写的程序设置环境。该环境需要具有以下软件包

### Python 环境

``manual_poser`` 和 ``ifacialmocap_puppeteer`` 都可以作为桌面应用程序使用。要运行它们，您需要为运行用 [Python](http://www.python.org) 语言编写的程序设置环境。该环境需要具有以下软件包:

* Python >= 3.8
* PyTorch >= 1.11.0 with CUDA support
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

一种做法是安装 [Anaconda](https://www.anaconda.com/) 并在您的 shell 中运行以下命令:

```
> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib
```
#### Caveat 1: Do not use Python 3.10 on Windows

同样，由于在 Windows 上无法使用 [wxPython](https://www.wxpython.org/)，因此不要使用 Python 3.10 直到 [此错误](https://github.com/wxWidgets/Phoenix/issues/2024) 被修复。这意味着您不应该在上面列表中的第一个 ``conda`` 命令中设置 ``python=3.10``。

#### Caveat 2: Adjust versions of Python and CUDA Toolkit as needed

上面的命令创建的环境给您 Python 3.8 版本和使用 CUDA Toolkit 11.3 编译的 PyTorch 版本。这种特定的设置可能在将来无法使用，因为您可能会发现该特定 PyTorch 包不适用于新电脑。解决方案是:

1. 在第一个命令中更改 Python 版本为适用于您的 OS 的最新版本。(即，如果您在 Windows 上使用，请不要使用 3.10。)
2. 在第三个命令中更改 CUDA 工具套件版本为 PyTorch 网站上可用的版本。特别地，滚动到“安装 PyTorch”部分，使用那里的选择器选择适合您电脑的命令。替换上面第三个命令来安装 PyTorch。

![The command to install PyTorch](docs/pytorch-install-command.png "The command to install PyTorch")

### Jupyter Environment

``manual_poser`` 也可以作为 [Jupyter Nootbook](http://jupyter.org) 使用。要在本地机器上运行它，您还需要安装:

```
> jupyter nbextension enable --py widgetsnbextension
```

在安装上面的这两个包之后，使用 Anaconda，我使用以下命令完成了上述操作:

```
> conda install -c conda-forge notebook
> conda install -c conda-forge ipywidgets
> jupyter nbextension enable --py widgetsnbextension
```
### Anaconda 一次性下载并安装所有 Python 包

您还可以使用 Anaconda 一次性下载并安装所有 Python 包。打开你的 shell，将目录更改到克隆存储库的位置，并运行:

```
> conda env create -f environment.yml
```

这将根据配置文件中的要求创建一个新的环境，并安装所需的所有 Python 包。

如果您想要更新环境中的包，可以使用:

```
> conda env update -f environment.yml
```

如果您想要删除该环境，可以使用:

```
> conda remove --name talking-head-anime-3-demo --all
```

这将创建一个名为 ``talking-head-anime-3-demo`` 的环境，其中包含所有必需的 Python 包。

### iFacialMocap

如果您想使用 ``ifacialmocap_puppeteer``，您还需要一个名为 [iFacialMocap](https://www.ifacialmocap.com/) 的 iOS 软件 (在 App Store 中 980 日元购买)。这次您不需要下载配对应用。您的 iOS 和电脑必须使用相同的网络。例如，您可以将它们连接到同一无线路由器。

## 下载模型

在运行程序之前，您需要从此 [Dropbox 链接](https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0) 下载模型文件，并将其解压到存储库根目录下的 ``data/models`` 文件夹中。最终，数据文件夹应该看起来像:

```
+ data
  + images
    - crypko_00.png
    - crypko_01.png
        :
    - crypko_07.png
    - lambda_00.png
    - lambda_01.png
  + models
    + separable_float
      - editor.pt
      - eyebrow_decomposer.pt
      - eyebrow_morphing_combiner.pt
      - face_morpher.pt
      - two_algo_face_body_rotator.pt
    + separable_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_float
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
```

模型文件使用 [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/legalcode) 发布，这意味着您可以将其用于商业目的。但是，如果您分发它们，您必须在其他方面说明我是创建者。

## 运行 `manual_poser` 桌面应用程序

打开一个 shell。将工作目录更改为存储库的根目录。然后运行:

```
> python tha3/app/manual_poser.py
```

请注意，在运行上面的命令之前，您可能需要激活包含所需软件包的 Python 环境。如果您使用 Anaconda 创建了环境，如上面所述，则需要运行

```
> conda activate talking-head-anime-3-demo
```
如果您尚未激活该环境。

## 选择要使用的系统变量

正如 [项目报告](http://pkhungurn.github.io/talking-head-anime-3/index.html) 中所述，我创建了 4 种神经网络系统的变体。它们被称为 ``standard_float``、``separable_float``、``standard_half`` 和 ``separable_half``。它们都具有相同的功能，但它们的大小、RAM 使用、速度和准确性有所不同。您可以通过 ``--model`` 命令行选项指定 ``manual_poser`` 程序使用哪个变体。

```
> python tha3/app/manual_poser --model <variant_name>
```

其中 ``<variant_name>`` 必须是上述 4 个名称之一。如果未指定变体，则将使用 ``standard_float`` 变体 (这是最大、最慢和最准确的)。

## 运行 `manual_poser` Jupyter Notebook

打开一个 shell。激活环境。将工作目录更改为存储库的根目录。然后运行:

```
> jupyter notebook
```

浏览器窗口应该会打开。在其中，打开 `manual_poser.ipynb`。完成后，您应该会看到它有两个单元格。按顺序运行这两个单元格。然后滚动到文档的末尾，您将在那里看到 GUI。

您可以通过更改第一个单元格中的 ``MODEL_NAME`` 变量来选择要使用的系统变体。如果您这样做，则需要重新运行两个单元格，以便加载变体并正确更新 GUI 以使用它。

## 运行 `ifacialmocap_poser`

首先，在 iOS 设备上运行 iFacialMocap。它应该会显示设备的 IP 地址。记下它。保持应用程序打开

```
> python tha3/app/ifacialmocap_puppeteer.py
```

在 "Capture Device IP" 文本框中输入您之前记下的 iOS 设备的 IP 地址。

![在 'Capture Device IP' 文本框中输入 iOS 设备的 IP 地址。](docs/ifacialmocap_puppeteer_ip_address_box.png "在 'Capture Device IP' 文本框中输入 iOS 设备的 IP 地址。")

点击右侧的 "START CAPTURE!" 按钮。

![点击 'START CAPTURE!' 按钮。](docs/ifacialmocap_puppeteer_click_start_capture.png "点击 'START CAPTURE!' 按钮。")

如果程序连接正常，当您移动头部时，应该能看到窗口底部的数字发生变化。

![窗口底部的数字应该在您移动头部时发生变化。](docs/ifacialmocap_puppeteer_numbers.png "窗口底部的数字应该在您移动头部时发生变化。")

现在，您可以加载一个角色的图像，它应该遵循您的面部动作。

## 输入图像的限制

为了使系统正常工作，输入图像必须遵循以下限制：

* 它应该是 512 x 512 的分辨率。（如果演示程序接收到其他大小的输入图像，它将将图像调整为此分辨率，并在此分辨率输出。）
* 它必须具有alpha通道。
* 它必须只包含一个人形角色。
* 角色应该立着直立并面对前方。
* 角色的手应该在头部下方并远离头部。
* 角色的头部大致应该包含在图像上半部的中间128 x 128框中。
* 不属于角色的所有像素的alpha通道（即背景像素）的alpha通道必须为0。

示例图像:
![符合上述标准的图像示例](docs/input_spec.png "符合上述标准的图像示例")

更多关于输入图像的详细信息，请参阅项目的 [write-up](http://pkhungurn.github.io/talking-head-anime-3/full.html#sec:problem-spec)

## 引用

如果您的学术工作受此存储库中的代码的益处，请如下引用项目网页:

> Pramook Khungurn. **Talking Head(?) Anime from a Single Image 3: Now the Body Too.** http://pkhungurn.github.io/talking-head-anime-3/, 2022. Accessed: YYYY-MM-DD.

您还可以使用以下 BibTex 条目:
```
@misc{Khungurn:2022,
    author = {Pramook Khungurn},
    title = {Talking Head(?) Anime from a Single Image 3: Now the Body Too},
    howpublished = {\url{http://pkhungurn.github.io/talking-head-anime-3/}},
    year = 2022,
    note = {Accessed: YYYY-MM-DD},
}
```

## Disclaimer

While the author is an employee of [Google Japan](https://careers.google.com/locations/tokyo/), this software is not Google's product and is not supported by Google.

The copyright of this software belongs to me as I have requested it using the [IARC process](https://opensource.google/documentation/reference/releasing#iarc). However, Google might claim the rights to the intellectual
property of this invention.

The code is released under the [MIT license](https://github.com/pkhungurn/talking-head-anime-2-demo/blob/master/LICENSE).
The model is released under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/legalcode). Please see the README.md file in the ``data/images`` directory for the licenses for the images there.
