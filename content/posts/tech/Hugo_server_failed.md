---
title: "【Hugo网站搭建】bug日志-hugo server失败"
date: 2023-08-20T00:17:58+08:00
lastmod: 2023-08-20T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- error
categories: 
- 
tags: 
- Hugo网站搭建
- bug日志
- hugo server失败
description: "error:hugo server失败"
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
## 项目场景

`Hugo`

---

## 问题描述

在本地博客源码目录下进行`hugo server` 失败，无法进入 `http://localhost:1313/` 

---

## 原因分析

假如修改过`"themes\hugo-PaperMod"`的话，那么可能是`"themes\hugo-PaperMod"`出现了问题

---

## 解决方案

将`"themes\hugo-PaperMod"`删除，在`hugo-PaperMod`的`Github`主页重新下载一次主题并放在`"themes\"`目录下，重新`hugo server`一次

建议对PaperMod主题做自定义修改的话，最好在博客源码目录下修改，而不是在`"themes\hugo-PaperMod"`修改

这是博客源码目录：

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



