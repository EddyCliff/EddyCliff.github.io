---
title: "【Github使用指南】bug日志-删除本地仓库/错误的远程仓库关联"
date: 2023-08-10T00:17:58+08:00
lastmod: 2023-08-10T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- error
- Repositories
categories: 
- 
tags: 
- Github使用指南
- Github
- bug日志
- Github仓库
description: "删除本地仓库/错误的远程仓库关联"
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
## 项目场景：

`Git bash`

---

## 问题描述1：

在错误的目录使用`git init`初始化了一个仓库，此时应该将该仓库删除

---

## 解决方案1：

1. 使用`ls -a` 命令显示隐藏文件夹和文件，检查该目录下是否有`.git`文件夹

2. 发现该目录下含有.git文件夹，使用`rm -rf .git`删除.git文件夹，同时本地仓库也会被销毁

---

## 问题描述2：

将本地仓库关联到错误的远程仓库，此时应该将该关联删除

---

## 解决方案2：

1. 查看本地仓库关联的远程仓库名称及URL

```bash
git remote -v
```

2. 确认关联的错误远程仓库之后，删除关联

```bash
git remote remove <远程仓库名称>
```



