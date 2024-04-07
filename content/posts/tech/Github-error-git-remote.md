---
title: "【Github使用指南】bug日志-git remote"
date: 2023-07-20T00:17:58+08:00
lastmod: 2023-07-20T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- error
categories: 
- 
tags: 
- Github
- error
description: "解决{error: remote origin already exists.}"
weight: 5
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
## 项目场景：

`Git bash` 

---

## 问题描述：

进行`git remote add origin https://github.com/你的用户名/你的用户名.github.io.git`命令时

出现错误：`error: remote origin already exists`

---

## 原因分析：

与本地仓库关联的远程仓库已经存在，无法进行新的关联

---

## 解决方案：

1. 删除已关联的远程库

```bash
git remote rm origin 
```

2. 关联正确的远程仓库

```bash
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
```

3. 推送到正确的仓库

```bash
git push -f origin main
```









