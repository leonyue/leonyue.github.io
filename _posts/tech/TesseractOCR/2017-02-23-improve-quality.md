---
layout: post
title: 提高识别质量
category: TesseractOCR
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

##### 未完待续

[原文地址](https://github.com/tesseract-ocr/tesseract/wiki/ImproveQuality#rescaling)