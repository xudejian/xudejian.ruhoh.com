---
title: 迭代小节
date: '2014-06-30'
author: Dejian Xu
description: 迭代小节
categories: '迭代小节'
tags:
  - 迭代小节
---

## 迭代 06.23 -- 06.27

### 做了什么

1. 相册的页面部分
2. 相册预览功能的调研
3. 新框架下上传图片和图片尺寸切割的调研
4. 一些bug的修改
5. 分享插件的交接

### 为什么这么做

1. 关于预览功能的调研
    需求中有一个重要的功能，就是预览功能，IE8是我们需要重点支持的浏览器，
我们的调研主要针对IE8下功能的实现。

    最终调研结果：我们找到了能兼容IE8预览的方案。

    缺点：我们系统中会有两套上传图片的功能模块并行。

    是否要将两套模块合并，这个以后视情况而定。

2. 关于上传图片和图片尺寸切割的调研
    我们在需求的迭代中，多次涉及到图片的尺寸，
一是旧尺寸无法满足要求，二是切分尺寸种类太多。

    所以我们需要在系统中，更加方便的，增加自定义切割尺寸规格。

    我们会做一套新的上传功能模块，并替换掉旧的上传功能。

### 进度不理想的原因

这个迭代的工作基本在计划内的时间完成。有个别卡中的某验收点暂时无法完成，
顺延到之后的迭代中。
