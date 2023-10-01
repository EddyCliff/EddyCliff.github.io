---
title: "【Github使用指南】bug日志-OpenSSL SSL_read"
date: 2023-06-20T00:17:58+08:00
lastmod: 2023-06-20T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- error
- SSL
categories: 
- 
tags: 
- Github
- error
description: "解决{OpenSSL SSL_read: Connection was reset, errno 10054}"
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
## 项目场景：

`Git bash`

---

## 问题描述：

进行`git push -f origin master`命令

出现错误：`fatal: unable to access ' 我的库的http路径 ': OpenSSL SSL_read: Connection was reset, errno 10054`

---

## 原因分析：

无法关联`github`的远程库,`SSL`连接被重置

---

## 解决方案：

使用命令行解除`SSL`认证

```bash
git config --global http.sslVerify "false"
git config --global https.sslVerify "false"
```



