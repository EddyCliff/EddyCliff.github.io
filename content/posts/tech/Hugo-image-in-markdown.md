---
title: "【Hugo网站搭建】markdown中图片大小和对齐方式调整"
date: 2023-12-03T00:17:58+08:00
lastmod: 2023-12-03T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- Markdown
- HTML
categories: 
- 
tags: 
- Hugo网站搭建
- Markdown
- HTML
description: "使用HTML标签对markdown中图片大小和对齐方式调整"
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
    image: "https://fastly.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/pc8.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 前言

当在Hugo博客网站中以Markdown格式编写博客内容时，直接使用图片的原始尺寸可能会导致页面排版混乱，甚至影响用户体验。为了优化页面显示效果，我们需要对图片的尺寸大小和对齐方式进行调整。

Markdown本身并不提供直接调整图片大小和对齐的方法，但可以借助HTML标签实现这些调整。这些标签提供了更灵活的方式来控制图片的显示效果。

## 调整图片大小

设置图片大小是保持页面布局整洁和用户体验良好的关键。通过指定图片的宽度和高度，我们可以确保图片在不失真的情况下适应页面布局。



### 1）使用HTML标签设置图片大小

```HTML
<img src="图片链接" alt="图片描述" width="300" height="200">
```

要注意的是，设定尺寸的图片需要指定`width` 和 `height`。

这两个参数可以是具体数值，也可以是百分比形式。



#### `width`和`height`的参数设置

1）宽度确定，高度自适应。

```HTML
<img src="图片链接" alt="图片描述" width="50%" height="auto">
```

2）宽度自适应，高度确定。

```HTML
<img src="图片链接" alt="图片描述" width="auto" height="50%">
```



#### 示例演示
> 原图

<img src="https://fastly.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/internet1.png" width="auto" height="auto">

> 宽度确定，高度自适应。

<img src="https://fastly.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/internet1.png" width="70%" height="auto">



> 宽度自适应，高度确定。

<img src="https://fastly.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/internet1.png" width="auto" height="70%">



## 设置图片对齐方式

除了大小，对图片的对齐方式也是十分重要的。居中、左对齐或右对齐可以使页面排版更加美观，提升可读性和视觉吸引力。



### 2）使用HTML标签设置图片大小设置图片对齐方式

使用`align`属性对图片对齐方式进行设置。

```HTML
<div align="center">
<img src="图片链接" alt="图片描述" width="300" height="200">
</div>
```

```HTML
<p align = "center">    
<img src="图片链接" alt="图片描述" width="300" height="200">
</p>
```



#### `align`的参数设置

1）居中对齐

```HTML
<div align="center">    
...
</div>
```

2）靠左对齐

```HTML
<div align="left">    
...
</div>
```

2）靠右对齐

```HTML
<div align="right">    
...
</div>
```

4）多个图片居中对齐

```HTML
<div align="right">    
<img src="图片链接" alt="图片描述" width="300" height="200">
<img src="图片链接" alt="图片描述" width="300" height="200">
<img src="图片链接" alt="图片描述" width="300" height="200">
<img src="图片链接" alt="图片描述" width="300" height="200">
<img src="图片链接" alt="图片描述" width="300" height="200">
</div>
```



#### 示例演示

> 居中对齐

<div align="center">    
<img src="https://fastly.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/internet1.png" width="70%" height="auto">
</div>



> 靠左对齐

<div align="left">    
<img src="https://fastly.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/internet1.png" width="70%" height="auto">
</div>



> 靠右对齐

<div align="right">    
<img src="https://fastly.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/internet1.png" width="70%" height="auto">
</div>

