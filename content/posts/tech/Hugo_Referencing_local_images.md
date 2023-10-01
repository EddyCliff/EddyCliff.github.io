---
title: "【Hugo网站搭建】实现post-markdown引用本地图片"
date: 2023-08-13T00:17:58+08:00
lastmod: 2023-08-13T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- Markdown
categories: 
- 
tags: 
- Hugo
- Markdown
description: "markdown引用本地图片"
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

`Hugo`博客源码根目录下的`static`目录是用来存放一些静态文件的（包括图片），执行`hugo -F`生成的`public`文件夹，会将`static`目录下的文件一并导入至`public`文件夹，并最终呈现至服务器网页上。

所以想要在`markdown`里引用本地图片，那么就在根目录的`static`目录下存放图片，并在`mardown`里引用就可以了。

---

## 操作步骤

### 1.在`static`目录下创建`img`子目录

将需要引用的图片都放在`static/img`下。

### 2.在`markdown`里引用本地图片

如想在`markdown`里引用`1.png`，那么就在`markdown`里输入

```Markdown
![](/img/1.png)
```

