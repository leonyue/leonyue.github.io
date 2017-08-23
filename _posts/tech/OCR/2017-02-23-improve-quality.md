---
layout: post
title: 提高识别质量
category: OCR
tags: Tesseract,OCR,ImproveQuality
description:
---

>   有各种各样的原因你可能不会得到优质的识别结果。重新训练Tesseract不一定有用，除非你使用的是一种非常特殊的字体或者一种新的语言。

####    图片处理

-   调整尺寸
-   二值化
-   降噪
-   旋转/矫正
-   移除边框
-   工具/库
-   示例

---

####    页面分割法

####  字典，可选词链表，正则表达式

#### 还有其他问题？

##### 图片处理

Tesseract 内部在进行实际的OCR操作前进行了各式各样的处理操作（使用Leptonica），总的来说干的不错，但是不可避免的有些情况下不够好，从而影响了识别正确率。

你可以再运行Tesseract使用[configuration variable](https://github.com/tesseract-ocr/tesseract/wiki/ControlParams)`tessedit_write_images` to `true`，如果结果文件`tessinput.tif`看上去有问题试试这些图片处理操作再传给Tesseract。

##### 调整大小

Tesseract 最佳工作图片至少300DPI。

##### 二值化

![image](/assets/uploads/binarisation.png)

这是把一张图片转成黑白图片。Tesseract在内部做了此处理，但是效果可能不是最佳，特别当图片背景是黑的不均匀的情况。

##### 降噪

![image](/assets/uploads/noise.png)

噪点是图片中敏感度或者颜色的变种，它们会使图片上的文字更加难以辨识。特定类型的噪点不能在Tesseract二值化步骤中被移除，从而降低了正确率。

##### 旋转/矫正

![image](/assets/uploads/skew-linedetection.png)

扭曲的图片是当页面在不值的情况下扫描的。Tesseract的线条分割可以减少明显的图片扭曲，图片扭曲会严重影响OCR质量。旋转页面以保证文字行是水平的。

##### 移除边框

![image](/assets/uploads/borders.png)

扫描的页面通常有黑色的边框环绕，这会被错误的当做额外的字符，特别是当形状和变化多样化时。

##### 工具/库

- [Leptonica](http://leptonica.com/)
- [OpenCV](http://opencv.org/)
- [Scan Tailor](http://scantailor.sourceforge.net/)
- [ImageMagick](http://www.imagemagick.org/)
- [unpaper](https://www.flameeyes.eu/projects/unpaper)
- [ImageJ](http://rsb.info.nih.gov/ij/)
- [Gimp](http://www.gimp.org/)

##### 示例

如果你需要一个实例如何用代码改进图片质量，可以参考以下：

- [OpenCV - Rotation(Deskewing)](http://felix.abecassis.me/2011/10/opencv-rotation-deskewing/) - c++ example
- [Fred's ImageMagick TEXTCLEANER](http://www.fmwconcepts.com/imagemagick/textcleaner/index.php) - bash script for processing a scanned document of text to clean the text background.
- [rotation_spacing.py](https://gist.github.com/endolith/334196bac1cac45a4893#) - python script for automatic detection of rotation and line spacing of an image of text
- [crop_morphology.py](https://github.com/danvk/oldnyc/blob/master/ocr/tess/crop_morphology.py) - [Finding blocks of text in an image using Python, OpenCV and numpy](http://www.danvk.org/2015/01/07/finding-blocks-of-text-in-an-image-using-python-opencv-and-numpy.html)

#### 页面分割法

默认Tesseract将图片分段时把它当成一页文字。如果你仅仅寻求一块区域来OCR时可以尝试另外的分段模式：`-psm` 参数。需要知道的是在文字周围加上白色边框裁切也同样有效。

查看支持的页面分割模式完整列表使用`tesseract -h`。下面为3.04的列表：

```
 0   Orientation and script detection (OSD) only.
 1   Automatic page segmentation with OSD.
 2   Automatic page segmentation, but no OSD, or OCR.
 3   Fully automatic page segmentation, but no OSD. (Default)
 4   Assume a single column of text of variable sizes.
 5   Assume a single uniform block of vertically aligned text.
 6   Assume a single uniform block of text.
 7   Treat the image as a single text line.
 8   Treat the image as a single word.
 9   Treat the image as a single word in a circle.
10   Treat the image as a single character.
```

#### 字典，可选词链表，正则表达式

默认Tesseract被优化成识别单词长句。如果你想识别其他，如账单，价格列表，代码，你可以在再次确认确当的分割模式的同时做一些事情来改进识别结果的正确率。

当你的文字主要不是单词时禁用字典会提高识别率。禁用的方法：[configuration variables](https://github.com/tesseract-ocr/tesseract/wiki/ControlParams) `load_system_dawg` and `load_freq_dawg` to` false`.

也可以添加可选词到词列表来帮助识别，如果你知道输入的类型可以增加表达式，这会对提供识别率有很大帮助。这里有详细的解释[Tesseract manual](https://github.com/tesseract-ocr/tesseract/blob/master/doc/tesseract.1.asc#config-files-and-augmenting-with-user-data).

如果你知道你只会遇到语言中的一部分字符，例如仅数字，你可以设置`tessedit_char_whitelist` [configuration variable](https://github.com/tesseract-ocr/tesseract/wiki/ControlParams). See the [FAQ for an example](https://github.com/tesseract-ocr/tesseract/wiki/FAQ#how-do-i-recognize-only-digits).

#### 仍有问题？

如果你已经尝试了上有方法但仍然识别率很低，到[论坛](http://www.fmwconcepts.com/imagemagick/textcleaner/index.php)寻求帮助把，推荐贴一张示范图。

[原文地址](https://github.com/tesseract-ocr/tesseract/wiki/ImproveQuality#rescaling)
