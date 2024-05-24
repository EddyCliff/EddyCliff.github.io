---
title: "【Hugo网站搭建】Githubpages托管服务对应仓库username.github.io"
date: 2023-03-11T00:17:58+08:00
lastmod: 2023-03-11T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- Github Pages
categories: 
- 
tags: 
- Hugo网站搭建
- Github Pages网站托管服务
- Github Pages
- Github
description: "githubpages对应仓库username.github.io删除之后,重新创建一次仍然可以匹配到githubpages"
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

`Githubpages`对应仓库username.github.io删除之后,重新创建一次仍然可以匹配到`Githubpages`

---

## 操作步骤

如果你删除了之前的 GitHub Pages 仓库（例如 `username.github.io`），然后重新创建了同名的仓库，并将该仓库的内容推送到 `main` 分支（或其他指定的分支），GitHub Pages 会自动重新启用，并根据新的仓库内容生成你的个人网站。

以下是你需要做的步骤：

1. **删除旧的仓库：** 删除之前的 `username.github.io` 仓库。

2. **创建新的仓库：** 在 GitHub 上创建一个新的 `username.github.io` 仓库。

3. **将网站内容推送到新仓库：** 将你的网站内容（包括 HTML、CSS、JavaScript 文件等）推送到新仓库的 `main` 分支（或其他指定的分支）中。

4. **等待 GitHub Pages 构建：** 一旦你推送了内容，GitHub Pages 将会开始自动构建你的个人网站。

5. **访问个人网站：** 在一段时间后，你可以通过浏览器访问 `https://username.github.io` 来查看你的个人网站。GitHub Pages 将使用新的仓库内容来生成网站。

重要提示：可能需要一些时间来让 GitHub Pages 更新并生成你的新网站。这通常需要几分钟到几个小时的时间。此外，如果你之前使用了自定义域名（如 `username.com`）来指向 GitHub Pages，你可能需要重新设置域名解析以确保它与新的仓库匹配。

总之，删除并重新创建一个同名的 `GitHub Pages` 仓库后，只要你推送了新内容，`GitHub Pages` 将重新构建你的个人网站。

