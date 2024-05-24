---
title: "【Hugo网站搭建】实现post-markdown引入树状目录"
date: 2023-08-25T00:17:58+08:00
lastmod: 2023-08-25T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- Markdown
- tree
categories: 
- 
tags: 
- Hugo网站搭建
- Markdown
- tree树状目录
description: "markdown引入树状目录"
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

利用`windows`系统的`tree`命令生成文件夹目录下的树形结构，并倒入至`markdown`。

---

## 一、tree是什么？

示例：pandas 是基于NumPy 的一种工具，该工具是为了解决数据分析任务而创建的。

## 二、使用步骤

### 1.在想要生成树形结构的文件夹目录下运行`powershell`，输入命令`tree`

![1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_Introduce_a_tree_directory/1.png)

输入命令`tree`生成树形目录（示例）：

```HTML
卷 Data 的文件夹 PATH 列表
卷序列号为 E84E-7569
D:.
├─.idea
├─archetypes
├─assets
│  ├─css
│  │  └─extended
│  └─js
├─content
│  ├─posts
│  │  ├─art
│  │  ├─projects
│  │  └─tech
│  │      └─tech1
│  └─tags
├─data
├─i18n
├─layouts
│  ├─partials
│  │  └─templates
│  ├─shortcodes
│  └─_default
│      └─_markup
├─public
│  ├─assets
│  │  ├─css
│  │  └─js
│  ├─en
│  │  ├─about
│  │  ├─archives
│  │  ├─categories
│  │  ├─posts
│  │  │  ├─art
│  │  │  │  ├─art
│  │  │  │  └─page
│  │  │  │      └─1
│  │  │  ├─page
│  │  │  │  └─1
│  │  │  ├─projects
│  │  │  │  ├─page
│  │  │  │  │  └─1
│  │  │  │  └─projects
│  │  │  └─tech
│  │  │      ├─page
│  │  │      │  └─1
│  │  │      ├─tech
│  │  │      └─tech1
│  │  ├─search
│  │  ├─series
│  │  └─tags
│  │      ├─art
│  │      │  └─page
│  │      │      └─1
│  │      ├─projects
│  │      │  └─page
│  │      │      └─1
│  │      └─tech
│  │          └─page
│  │              └─1
│  ├─fonts
│  └─img
├─resources
│  └─_gen
│      ├─assets
│      └─images
├─static
│  ├─fonts
│  └─img
└─themes
    └─hugo-PaperMod
        ├─.github
        │  ├─ISSUE_TEMPLATE
        │  └─workflows
        ├─assets
        │  ├─css
        │  │  ├─common
        │  │  ├─core
        │  │  ├─extended
        │  │  ├─hljs
        │  │  └─includes
        │  └─js
        ├─i18n
        ├─images
        └─layouts
            ├─partials
            │  └─templates
            ├─shortcodes
            └─_default
                └─_markup
```

### 2.输入命令`tree > tree.txt`保存生成的树形目录至当前文件夹目录下的`tree.txt`

![2.png](https://cdn.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_Introduce_a_tree_directory/2.png)

---



### 3.复制粘贴至markdown文件

在markdown使用```开启代码块，将复制的树形目录粘贴在代码块处

```HTML
├─.idea
├─archetypes
├─assets
│  ├─css
│  │  └─extended
│  └─js
├─content
│  ├─posts
│  │  ├─art
│  │  ├─projects
│  │  └─tech
│  │      └─tech1
│  └─tags
├─data
├─i18n
├─layouts
│  ├─partials
│  │  └─templates
│  ├─shortcodes
│  └─_default
│      └─_markup
├─public
│  ├─assets
│  │  ├─css
│  │  └─js
│  ├─en
│  │  ├─about
│  │  ├─archives
│  │  ├─categories
│  │  ├─posts
│  │  │  ├─art
│  │  │  │  ├─art
│  │  │  │  └─page
│  │  │  │      └─1
│  │  │  ├─page
│  │  │  │  └─1
│  │  │  ├─projects
│  │  │  │  ├─page
│  │  │  │  │  └─1
│  │  │  │  └─projects
│  │  │  └─tech
│  │  │      ├─page
│  │  │      │  └─1
│  │  │      ├─tech
│  │  │      └─tech1
│  │  ├─search
│  │  ├─series
│  │  └─tags
│  │      ├─art
│  │      │  └─page
│  │      │      └─1
│  │      ├─projects
│  │      │  └─page
│  │      │      └─1
│  │      └─tech
│  │          └─page
│  │              └─1
│  ├─fonts
│  └─img
├─resources
│  └─_gen
│      ├─assets
│      └─images
├─static
│  ├─fonts
│  └─img
└─themes
    └─hugo-PaperMod
        ├─.github
        │  ├─ISSUE_TEMPLATE
        │  └─workflows
        ├─assets
        │  ├─css
        │  │  ├─common
        │  │  ├─core
        │  │  ├─extended
        │  │  ├─hljs
        │  │  └─includes
        │  └─js
        ├─i18n
        ├─images
        └─layouts
            ├─partials
            │  └─templates
            ├─shortcodes
            └─_default
                └─_markup

```

## 总结

本文简单介绍了`windows`系统下`tree`命令的使用，`tree`命令可以生成文件夹目录下的树形结构，并倒入至`markdown`。


