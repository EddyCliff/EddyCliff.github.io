---
title: "【Hugo网站搭建】博客写作指南-如何在Hugo博客中巧妙运用照片"
date: 2023-08-29T00:17:58+08:00
lastmod: 2023-08-29T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Blog writing
- picture
categories: 
- 
tags: 
- Blog writing
- picture
description: "了解如何在Hugo博客中巧妙使用照片，为您的内容增添视觉吸引力和表现力。"
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/blog.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 1.增添博客封面
可以将博客封面存放在`static/img`下,然后引用
```json
---
cover:
    image: "https://eddyblog.cn/img/hugo.png" #图片路径：static/img/hugo.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---
```
原本存放在`static/img`下的`hugo.png`经过`hugo -F`生成`public`目录后会存放在`public/img`下，所以这里的链接是``https://eddyblog.cn/img/hugo.png``

原理分析：   
- 域名解析： 域名 eddyblog.cn 需要通过 DNS 解析将其映射到 GitHub Pages 的服务器 IP 地址。这样浏览器才能正确找到要请求的服务器。
- GitHub Pages 服务器： 当浏览器发出请求时，DNS 解析后的请求会被路由到 GitHub Pages 的服务器。
- 资源请求： GitHub Pages 服务器接收到请求后，会寻找请求的资源。在这种情况下，服务器会尝试寻找 /img/hugo.png 路径下的图片资源。
- 资源响应： 如果服务器找到了请求的图片资源，它会将图片资源作为 HTTP 响应发送回浏览器。
- 渲染页面： 浏览器接收到图片资源的响应后，会将图片渲染到相应的位置，使其显示在您的博客页面中。
在这个过程中，您的博客域名需要正确解析到 GitHub Pages 的服务器，而您提供的图片链接则是相对于博客站点根目录的路径。这样，当浏览器加载页面时，它会根据提供的图片链接请求并显示相应的图片资源。

## 2.在博客正文中插入图片
### 利用自己的服务器加载图片
在 Hugo 的博客中，使用静态文件来存放图片并在 Markdown 中引用这些图片的方法如下：

1. 在 Hugo 博客源码的根目录下的 `static` 目录中创建一个名为 `img` 的子目录，用于存放需要引用的图片。这个目录结构应该类似于 `static/img`。

2. 将需要引用的图片放置在 `static/img` 目录下。

3. 在您的 Markdown 文件中，使用以下语法来引用本地图片：
   
   ```markdown
   ![](/img/1.png)
   ```

   其中，`/img/1.png` 是图片在 `static/img` 目录下的相对路径。Hugo 在生成静态站点时会将 `static` 目录下的文件一并复制到最终生成的 `public` 文件夹中。

这种方式确保您的图片会与生成的网页一同部署，并可以通过相对路径在 Markdown 文件中引用。

### 利用图床加速加载图片
>如何搭建图床请见该链接
                       
![【博客写作指南】GitHub+jsDelivr+PicGo搭建博客图床](https://eddyblog.cn/posts/tech/github_jsdelivr_picgo/) 

使用 GitHub、jsDelivr 和 PicGo 搭建博客图床是另一种常见的方式，它允许您将博客中使用的图片存储在 GitHub 仓库中，并通过 jsDelivr 加速服务来加载图片。

以下是这种方式的一般步骤：

1. **创建 GitHub 仓库：** 在 GitHub 上创建一个用于存储博客图片的仓库，您可以选择将仓库设置为公开或私有，根据您的需要。

2. **上传图片：** 将博客中需要使用的图片上传到 GitHub 仓库中，您可以创建一个文件夹来组织图片。

3. **获取图片链接：** 在 GitHub 仓库中，找到您上传的图片文件，点击文件查看，然后点击 "Download"（下载）按钮，复制浏览器地址栏中的链接。

4. **使用 jsDelivr 进行加速：** 使用 jsDelivr 的 CDN 加速服务，将 GitHub 仓库中的图片链接进行加速。将 `cdn.jsdelivr.net` 加上 GitHub 仓库的用户名、仓库名和文件路径，形成类似以下的链接：

   ```
   https://cdn.jsdelivr.net/gh/用户名/仓库名/文件路径
   ```

   这样就可以通过 jsDelivr 加速加载图片。

5. **使用 PicGo 进行上传：** PicGo 是一个图床工具，可以帮助您批量上传图片到 GitHub 仓库，并自动生成图片链接。您可以在本地编辑 Markdown 文章时，使用 PicGo 快速上传图片并获取链接。

6. **在 Markdown 中引用图片：** 使用获取到的加速链接，将图片引用插入到 Markdown 文件中。

这种方式的优势在于，您可以将图片集中存储在 GitHub 仓库中，通过 CDN 加速服务来加载图片，提高图片加载速度。同时，使用 PicGo 可以简化图片上传和链接获取的过程。

