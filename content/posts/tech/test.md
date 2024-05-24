---
title: "markdown测试"
date: 2024-04-10T00:17:58+08:00
lastmod: 2024-04-10T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Linux
- Operating System
- Operating system command
- 操作系统
- 操作系统命令
- 操作系统基本命令
- Linux命令
- Linux基本命令
- Linux新手必看
categories: 
- 
tags: 
- Hugo框架博客网站Papermode主题的博客渲染效果测试
description: "本文用于测试Hugo博客网站Papermod主题的博客渲染效果"
weight: 3
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/linux.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

> 测试：二级标题

## INIT

> 测试：使用HTML标签插入图片

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/INIT.jpg" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>


> 测试：<br>换行符(若该位置已换行，则证明`<br>`生效)


>测试：mac风格代码块显示(若代码块为mac风格，则证明`code.css`生效)

```shell
cd /  #切换路径到根目录
cd    #切换路径到当前用户的家目录
cd ~  #切换路径到当前用户的家目录
cd .. #切换到上一级目录
cd ../../Even/pic #返回上一级的上一级的Even里面的pic(相对路径)
cd /home/Even/pic #切换路径到/home/Even/pic(绝对路径) 
cd - #返回上一次所在的路径
```


>测试：非指定编程语言代码块显示(下面的代码块未指定编程语言)

```
-rw-r--r-- 1 ubuntu ubuntu 8980 Jul 6 2020 example.desktop
drwxr-xr-x 2 ubuntu ubuntu 4096 Jul 6 2020 Music
```

>测试：emoji显示

🤔`drwxr-xr-x`这是什么意思呢？

🔑我们可以把它拆解为4部分。

>测试：markown表格显示


| 符号  | 文件类型                 |
| --- | -------------------- |
| -   | 普通文件（mp3，mp4等）       |
| d   | 目录文件（文件夹）            |
| p   | 管道文件（一般用于进程间通信）      |
| l   | 链接文件（相当于快捷方式）        |
| s   | 套接字文件（一般用于网络通信）      |
| b   | 块设备文件（驱动的设备文件由驱动生成）  |
| c   | 字符设备文件（驱动的设备文件由驱动生成) |

