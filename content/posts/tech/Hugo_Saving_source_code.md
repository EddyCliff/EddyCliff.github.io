---
title: "【Hugo网站搭建】在Github仓库分别储存博客源码和静态页面"
date: 2023-08-11T00:17:58+08:00
lastmod: 2023-08-11T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- public
- Github
categories: 
- 
tags: 
- Hugo网站搭建
- Github Pages网站托管服务
- Github
description: "在Github仓库分别储存博客源码和静态页面"
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

建议将博客源码和构建的静态页面分别放在两个仓库里。

---

## 操作步骤

### 1.博客源码（Github仓库）

私有仓库`Yourname-Blog`（注意`Github`仓库命名规范）存放博客源码。（也可以是一个公开的仓库，相当于为他人提供了一个博客模板）

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
│  │      └─cover
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

### 2.静态页面（Github仓库）

公有仓库`yourname.github.io`存放构建的静态页面。

将构建的静态页面，即`public/`目录下的内容上传到仓库`yourname.github.io` ，`yourname.github.io`会自动绑定`Github pages` , 就可以通过网址`yourname.github.io`访问部署好的博客。

```HTML
├─assets
│  ├─css
│  └─js
├─en
│  ├─about
│  ├─archives
│  ├─categories
│  ├─posts
│  │  ├─art
│  │  │  ├─art
│  │  │  └─page
│  │  │      └─1
│  │  ├─page
│  │  │  └─1
│  │  ├─projects
│  │  │  ├─page
│  │  │  │  └─1
│  │  │  └─projects
│  │  └─tech
│  │      ├─page
│  │      │  └─1
│  │      ├─tech
│  │      └─tech1
│  ├─search
│  ├─series
│  └─tags
│      ├─art
│      │  └─page
│      │      └─1
│      ├─projects
│      │  └─page
│      │      └─1
│      └─tech
│          └─page
│              └─1
├─fonts
└─img
```

---

## 总结

本文简单介绍了`Hugo`框架下在`Github`仓库分别储存博客源码和静态页面的操作步骤和方法。



