---
title: "【Hugo网站搭建】bug日志-将网站部署到GitHub远程仓库时出现了错误"
date: 2023-04-17T00:17:58+08:00
lastmod: 2023-04-17T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- Github
categories: 
- 
tags: 
- Hugo
- Github
- error
description: "error:在本地使用Hugo构建网站并在本地服务器上正常运行，但在将网站部署到GitHub远程仓库时出现了错误"
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
## 项目场景：

`Hugo`

---

## 问题描述

在本地使用`Hugo` 构建网站并在本地服务器上正常运行，但在将网站部署到 GitHub 远程仓库时出现了错误

---

## 解决方案：

1. **检查 GitHub 仓库代码：** 首先，确保你将正确的代码推送到了 GitHub 远程仓库。在本地使用 `git status` 和 `git diff` 命令来检查是否有未提交的更改，并使用 `git push` 命令将代码推送到远程仓库。

- （假如发现Github远程仓库里的文件污染严重，修改难度很大，可以在Github页面将`username.github.io`该仓库删除，然后重新创建一次`username.github.io`仓库，不过要确保本地有文件备份再删除远程仓库，远程仓库删除后无法恢复）

2. **检查主题和模板：** 如果你使用了主题或模板，确保它们在 GitHub 上也是最新的版本，并与本地使用的版本相同。有时候，在不同环境中使用不同的主题或模板版本可能导致不一致。

3. **检查配置文件：** Hugo 使用配置文件来指定网站的设置和参数。确保在 GitHub 仓库中的配置文件与本地一致，并且没有不一致之处

