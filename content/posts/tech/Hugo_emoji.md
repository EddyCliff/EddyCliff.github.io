---
title: "【Hugo网站搭建】主题美化-添加emoji"
date: 2023-05-13T00:17:58+08:00
lastmod: 2023-05-13T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- emoji
categories: 
- 
tags: 
- Hugo网站搭建
- Hugo网站美化
description: "Hugo主题美化-添加emoji"
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

`hugo`主题美化-添加emoji

---

## 一、emoji表情符号素材网站（链接来源于网络资源）

[https://www.lovelyemoji.com/emoji-biaoqing/](https://www.lovelyemoji.com/emoji-biaoqing/)

[https://www.emojipedia.org/zh/](https://www.emojipedia.org/zh/)

[https://www.emojiall.com/zh-hans](https://www.emojiall.com/zh-hans)

## 二、使用步骤

### 1.在emoji素材网站复制emoji

### 2.在`config.yml`进行粘贴emoji （也可以在自己想改的相应文件进行修改）

代码如下（示例）：

```YAML
menu:
      main:
        - identifier: search
          name: 🔍 搜索
          url: search
          weight: 1
        - identifier: home
          name: 🏠 主页
          url: /
          weight: 2
        - identifier: posts
          name: 📚 文章
          url: posts
          weight: 3
        - identifier: tags
          name: 🧩 标签
          url: tags
          weight: 15
        - identifier: archives
          name: ⏱️ 时间轴
          url: archives/
          weight: 20
        - identifier: about
          name: 🙋🏻‍♂️ 关于
          url: about
          weight: 50
```

### 3.效果图

![1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_emoji/1.png)

---

## 总结

本文仅仅简单介绍了`hugo`主题美化-添加emoji。









