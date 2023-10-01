---
title: "【Github使用指南】将仓库中的master分支合并到main分支"
date: 2023-06-20T00:17:58+08:00
lastmod: 2023-06-20T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- master
- main
categories: 
- 
tags: 
- Github
description: "将仓库中的master分支合并到main分支"
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
## 前言：

`Git bash`本地常用指令是默认上传到远程仓库的`master`分支，而实际上我们应该将文件传入到`main`主分支

---

## 使用步骤：

> 假如是将远程库`master`分支合并到`main`分支，那么要先将远程仓库克隆到本地

```bash
git clone xxx.git
```



将本地的`master`分支合并到远程的`main`分支，则上个步骤不用做

1. 在本地`master`分支的文件夹目录，创建并切换至`main`分支

```bash
git checkout -b main
```

2. 推送至`main`主分支

```bash
git push origin main
```

3. 删除本地`master`分支（假如本地仓库还要使用的话，最好先不要删除）

```bash
git branch -d master
```

4. 删除远程`master`分支

```bash
git push origin :master
```



