## Installation guide in other language

[前往中文安装教程](README_CN.md)

## Original guide and documentation

[ORIGINAL ENGLISH README](README_ORIGINAL_EN.md) | [原版中文README在此](README_ORIGINAL_EN.md)


# "Talking Head(?) Anime from A Single Image 3: Now the Body Too" 's install process

## Hardware Requirements

Original author recommends RTX 2080 or RTX 3080 at least
personally tested on a RTX 4090 with the , so it should work with any nvidia card with this process on Windows 10 / 11 Machines

The `ifacialmocap_puppeteer` requires an iOS device that is capable of computing [blend shape parameters](https://developer.apple.com/documentation/arkit/arfaceanchor/2928251-blendshapes) from a video feed. This means that the device must be able to run iOS 11.0 or higher and must have a TrueDepth front-facing camera. (See [this page](https://developer.apple.com/documentation/arkit/content_anchors/tracking_and_visualizing_faces) for more info.) In other words, if you have the iPhone X or something better, you should be all set. Personally, I have used an iPhone 12 mini.

## Software Requirements

### GPU Related Software INSTALL CUDA with right version cuda 11.7 , pick windows 10 or 11 accordingly

Please update your GPU's device driver and install the [CUDA Toolkit](https://developer.nvidia.com/cuda-11-7-0-download-archive?target_os=Windows&target_arch=x86_64) that is compatible with your GPU and is newer than the version you will be installing in the next subsection.

### Install Anacoda! Then proceeds to run the following command in your annacoda environment prompt one by one.



One way to do so is to install [Anaconda](https://www.anaconda.com/) and run the following commands in your anaconda prompt:

![Use Anaconda Prompt](docs/AnacondaPrompt.png "Use Anaconda Prompt")


```
> conda create -n talking-head-anime-3-demo python=3.10
```

```
> conda activate talking-head-anime-3-demo
```

```
> conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
```

```
> conda install scipy
```

```
> conda install matplotlib
```

```
> conda install -c conda-forge notebook
```

```
> conda install -c conda-forge ipywidgets
```

```
> jupyter nbextension enable --py widgetsnbextension
```

## Download the Models

Before running the programs, you need to download the model files from this [Dropbox link](https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0) and unzip it to the ``data/models`` folder under the repository's root directory. In the end, the data folder should look like:

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

The model files are distributed with the 
[Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/legalcode), which
means that you can use them for commercial purposes. However, if you distribute them, you must, among other things, say 
that I am the creator.

## Running the `ifacialmocap_poser`

First, run iFacialMocap on your iOS device. It should show you the device's IP address. Jot it down. Keep the app open.

![IP address in iFacialMocap screen](docs/ifacialmocap_ip.jpg "IP address in iFacialMocap screen")

Open a shell. Activate the Python environment. Change your working directory to the repository's root directory. Then, run:

```
> python tha3/app/ifacialmocap_puppeteer.py
```

You will see a text box with label "Capture Device IP." Write the iOS device's IP address that you jotted down there.

![Write IP address of your iOS device in the 'Capture Device IP' text box.](docs/ifacialmocap_puppeteer_ip_address_box.png "Write IP address of your iOS device in the 'Capture Device IP' text box.")

Click the "START CAPTURE!" button to the right.

![Click the 'START CAPTURE!' button.](docs/ifacialmocap_puppeteer_click_start_capture.png "Click the 'START CAPTURE!' button.")

If the programs are connected properly, you should see the numbers in the bottom part of the window change when you move your head.

![The numbers in the bottom part of the window should change when you move your head.](docs/ifacialmocap_puppeteer_numbers.png "The numbers in the bottom part of the window should change when you move your head.")

Now, you can load an image of a character, and it should follow your facial movement.

## Contraints on Input Images

In order for the system to work well, the input image must obey the following constraints:

* It should be of resolution 512 x 512. (If the demo programs receives an input image of any other size, they will resize the image to this resolution and also output at this resolution.)
* It must have an alpha channel.
* It must contain only one humanoid character.
* The character should be standing upright and facing forward.
* The character's hands should be below and far from the head.
* The head of the character should roughly be contained in the 128 x 128 box in the middle of the top half of the image.
* The alpha channels of all pixels that do not belong to the character (i.e., background pixels) must be 0.

![An example of an image that conforms to the above criteria](docs/input_spec.png "An example of an image that conforms to the above criteria")

See the project's [write-up](http://pkhungurn.github.io/talking-head-anime-3/full.html#sec:problem-spec) for more details on the input image.