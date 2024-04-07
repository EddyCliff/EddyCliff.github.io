---
title: "【Github使用指南】release上的安装包amd和arm的选择"
date: 2023-08-20T00:17:58+08:00
lastmod: 2022-08-20T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- release
- AMD
- ARM
categories: 
- 
tags: 
- Github
- release
description: "release上的安装包amd和arm的选择"
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/github.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---
## 前言

下载哪种架构的安装包取决于你的计算机处理器架构。通常，大多数桌面和笔记本电脑使用的是`x86_64`或简称为`amd64`架构，因此你应该下载针对该架构的安装包

---

## AMD

如果你的电脑使用的是`x86_64`或`amd64`架构，那么下载`x86_64`或`amd64`架构的安装包。

---

## ARM

如果你的计算机是`ARM`设备，例如某些Raspberry Pi设别或其他`ARM`架构的计算机，那么你应该下载适用于`ARM`架构的安装包

---

## 确定处理器架构

要确定你的计算机的处理器架构，可以执行一些操作，例如在命令行中输入`uname -m`（对于大多数`Linux`系统），或者在`Windows`上通过"系统信息"来查看处理器信息。

总之，下载与你计算机处理器架构匹配的安装包是确保软件能够正常运行的重要一步。



