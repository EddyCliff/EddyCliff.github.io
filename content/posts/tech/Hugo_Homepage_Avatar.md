---
title: "【Hugo网站搭建】在config.yml中设置主页头像"
date: 2023-03-11T00:17:58+08:00
lastmod: 2023-03-11T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- Homepage_Avatar
categories: 
- 
tags: 
- Hugo网站搭建
- Hugo网站主页头像
- Hugo网站 config.yml
description: "Hugo设置主页头像"
weight:
slug: ""
draft: false # 是否为草稿
comments: false
reward: false # 打赏
mermaid: true #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
    image: "https://eddyblog.cn/img/hugo.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---


## 前言

`Hugo`设置主页头像

---

## 操作步骤

### 1.制作圆形头像：在线网站 （链接来源于网络资源）

网站如下（示例）：

[https://crop-circle.imageonline.co/cn/#google_vignette](https://crop-circle.imageonline.co/cn/#google_vignette)

[https://bigimage.11zon.com/zh-cn/crop-circle-image/](https://bigimage.11zon.com/zh-cn/crop-circle-image/)

### 2.config.yml更改

```YAML
languages:
  en:
    params:
      languageName: "English"
      weight: 1
      profileMode:
        enabled: true
        title: Eddy - blog
        subtitle:
        # subtitle: 
        imageUrl: "img/logo3.png" #图片放在static/img/logo3.png
        imageTitle:
        imageWidth: 150  # 设置图像尺寸
        imageHeight: 150 # 设置图像尺寸
```

主页头像 image 即为下图中圆形图像

![1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_Homepage_Avatar/1.png)

---

## 总结

本文简单介绍了`Hugo`设置主页头像的操作步骤和方法。

