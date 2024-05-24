---
title: "【Hugo网站搭建】GitHub+jsDelivr+PicGo搭建博客图床"
date: 2023-08-29T00:17:58+08:00
lastmod: 2023-08-29T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- jsDelivr
- PicGo
- CDN
- image host
categories: 
- 
tags: 
- Github Pages网站托管服务
- Github
- jsDelivr CDN服务
- 图床工具PicGo
- 内容分发网络CDN
- 图床
- Hugo网站搭建
description: "在这篇【博客写作指南】中，了解如何利用GitHub、jsDelivr和PicGo搭建博客图床，为你的博客添加稳定的图片存储和快速的CDN加速服务。跟随详细步骤，轻松创建自己的图床，为博客内容增添视觉魅力。"
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
    image: "https://eddyblog.cn/img/github.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 前言

在本篇教程中，我们将为你详细介绍如何使用 `GitHub`、`jsDelivr` 和 `PicGo` 这三大工具，快速搭建一个高效、稳定的博客图床。通过优化图片加载方式，你不仅可以提升博客内容的展示效果，还能够加速网页加载速度，为读者呈现更好的阅读体验。

### 图床的重要性

博客作为信息传播的重要途径，常常需要插入图片来更生动地展示内容。然而，直接将图片嵌入博客文章中可能导致博客服务器负担加重，进而影响网页的加载速度。这时，一个稳定的图床就显得尤为必要。

## GitHub：稳定的存储平台

首先，我们将利用GitHub作为图床的存储平台。GitHub的稳定性和可靠性已被广泛认可，尤其是在被微软收购后。它不仅提供免费的存储空间，还能够通过版本控制管理图片，确保你的图像资源安全可控。

## jsDelivr：快速CDN加速

为了加速图片加载，我们将使用jsDelivr作为内容分发网络（CDN）。jsDelivr可以在全球范围内提供快速的图像传输，减少用户访问博客时的加载时间，提升用户体验。通过将GitHub存储的图片链接转换为jsDelivr链接，你可以轻松地为博客内容加速。

## PicGo：便捷的上传工具

为了简化上传过程，我们将使用PicGo，一个强大的上传工具。PicGo具备许多有用的功能，其中最引人注目的是剪贴板图片上传。当你在QQ或微信截取了图片后，只需点击剪贴板图片上传按钮，PicGo会自动将剪贴板中的图片上传到配置的图床中，并生成图片外链。这个便利的功能大大提高了图片上传的效率。

## 搭建步骤

### 第一步：注册 GitHub 账号

首先，你需要注册一个 [GitHub 账号](https://github.com/)。如果你还没有账号，现在就前往 GitHub 官网进行注册。

### 第二步：创建 GitHub 仓库

登录你的 `GitHub` 账号，点击页面右上角的`+`按钮，选择`New repository`来创建一个新的仓库。这个仓库将用于存储你的博客图片资源。

![1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/GitHub_jsDelivr_PicGo/1.png)

### 第三步：生成 GitHub Token

为了让 `PicGo` 能够上传图片至 `GitHub`仓库，我们需要生成一个 `Token`。前往 [GitHub Settings](https://github.com/settings/profile) 页面，点击左侧菜单中的`Developer settings`，然后选择`Personal access tokens`。点击`Generate new token`，并为它授予`repo`权限，然后翻到页面最底部，点击`Generate token`的绿色按钮生成`token`。

![2.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/GitHub_jsDelivr_PicGo/2.png)

![3.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/GitHub_jsDelivr_PicGo/3.png)
### 第四步：安装并配置 PicGo

`PicGo`官方指南：[https://picgo.github.io/PicGo-Doc/zh/guide/#picgo-is-here](https://picgo.github.io/PicGo-Doc/zh/guide/#picgo-is-here)

`PicGo`安装包：[https://github.com/Molunerfinn/PicGo/releases](https://github.com/Molunerfinn/PicGo/releases)

64位电脑下载[PicGo-Setup-2.4.0-beta.4-x64.exe](https://picgo-release.molunerfinn.com/2.4.0-beta.4/PicGo-Setup-2.4.0-beta.4-x64.exe)，下载最新版本即可。

下载并安装 [PicGo](https://github.com/Molunerfinn/PicGo/releases) 工具。打开 `PicGo`，进入左侧的`图床设置`，选择`GitHub图床`。填入你的 `GitHub` 用户名和仓库名，将刚才生成的 `Token` 粘贴到对应位置。默认的分支名是`main`。

```JSON
{
  "repo": "", // 仓库名，格式是username/reponame
  "token": "", // github token
  "path": "", // 自定义存储路径，比如img/
  "customUrl": "", // 自定义域名，注意要加http://或者https://
  "branch": "" // 分支名，默认是main
}
```

![4.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/GitHub_jsDelivr_PicGo/4.png)
### 第五步：使用自定义域名

为了优化图片加载速度，我们将使用 `jsDelivr` 提供的 `CDN` 服务。在 `PicGo` 的 `GitHub` 图床设置中，设置自定义域名为：`https://cdn.jsdelivr.net/gh/你的GitHub用户名/仓库名`。

### 第六步：上传图片并获取外链

现在，你可以使用 `PicGo` 强大的功能了。通过快捷键或剪贴板图片上传，将图片上传到你的 `GitHub` 仓库。`PicGo` 将自动为你生成图片外链，方便你插入到博客文章中。

![5.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/GitHub_jsDelivr_PicGo/5.png)

## 总结

通过 `GitHub+jsDelivr+PicGo` 的组合，你可以快速搭建一个高效、稳定的博客图床，提升博客内容的展示效果。无论你是新手还是老手，在这个教程的指引下，你都能轻松实现图床的搭建，为你的博客增色不少。希望你能在博客创作中享受更顺畅的图片管理和展示体验！



## 感谢

`PicGo`官方指南：[https://picgo.github.io/PicGo-Doc/zh/guide/#picgo-is-here](https://picgo.github.io/PicGo-Doc/zh/guide/#picgo-is-here)

