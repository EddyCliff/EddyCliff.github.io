---
title: "Linux新手必看：一文掌握操作系统基本命令"
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
- Linux
- Operating System
- Operating system command
description: "掌握Linux，从这里开始！轻松入门操作系统基本命令。"
weight: 1
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

## Linux命令入门:17个基本命令

## INIT

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/INIT.jpg" alt = "INIT.jpg" width = "70%" height = "auto">
</div>

>**INIT：本节内容正式开始。action!**



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8.png" alt = "Linux命令入门.png" width = "70%" height = "auto">
</div>

## 📌命令：pwd

作用：打印当前的工作路径，用于确认您当前在文件系统中的位置。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-1.png" width = "70%" height = "auto">
</div>

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-2.png" width = "70%" height = "auto">
</div>

这里有几个概念需要说明一下。
```shell
~
.
..
/
```


***~ 就是当前用户的家目录缩写***

举例说明

```shell
cd ~ 
```
可以快速切换到你的家目录。

如果用户`john`的家目录是`/home/john`，那么可以使用`cd ~`命令，它等同于`cd home/john`

***. 表示当前目录***

举例说明

```shell
ls . 
```

这个命令会列出当前目录中的所有文件和文件夹。

***.. 表示上一级目录***

```shell
cd ..
```

从当前目录移动到上一级目录。

***/ 表示根目录***

在Unix-like系统中，没有像Windows那样的驱动器盘符（如C:、D:等），而是有一个单一的树状结构，以“/”作为整个文件系统的起点。

例如：

- `/home` 通常包含用户的主目录。
- `/usr` 用于存放用户安装的程序和文件。
- `/etc` 包含系统的配置文件。

## 📌命令：cd

作用：切换工作路径。

```shell
cd /  #切换路径到根目录
cd    #切换路径到当前用户的家目录
cd ~  #切换路径到当前用户的家目录
cd .. #切换到上一级目录
cd ../../Even/pic #返回上一级的上一级的Even里面的pic(相对路径)
cd /home/Even/pic #切换路径到/home/Even/pic(绝对路径) 
cd - #返回上一次所在的路径
```

## 📌命令：ls

作用：列举当前目录下的内容

```shell
ls -l #列举当前目录下的详细内容
ls -a #列举当前目录下的所有内容(包括隐藏文件.Filename)
```

举例说明  

在一个文件夹下运行了`ls` 命令得到了如下

```
-rw-r--r-- 1 ubuntu ubuntu 8980 Jul 6 2020 example.desktop
drwxr-xr-x 2 ubuntu ubuntu 4096 Jul 6 2020 Music
```

🤔`drwxr-xr-x`这是什么意思呢？

🔑我们可以把它拆解为4部分。

1）`d` 是一种文件类型，这里为目录文件。

文件系统类型：在Linux系统下一共有七种文件类型

| 符号  | 文件类型                 |
| --- | -------------------- |
| -   | 普通文件（mp3，mp4等）       |
| d   | 目录文件（文件夹）            |
| p   | 管道文件（一般用于进程间通信）      |
| l   | 链接文件（相当于快捷方式）        |
| s   | 套接字文件（一般用于网络通信）      |
| b   | 块设备文件（驱动的设备文件由驱动生成）  |
| c   | 字符设备文件（驱动的设备文件由驱动生成) |

2）`rwxr-xr-x` 有三个部分，三个字母为一个部分，从左到右依次代表文件的拥有者的权限`rwx`，同组用户的权限`r-x`，其他用户的权限`r-x`。

🤔`r` `w` `x`  `-`这是什么意思呢？

举个例子

| R 可读 | W 可写 | X 可执行 | 字母  | 值               |
| ---- | ---- | ----- | --- | --------------- |
| 1    | 1    | 0     | rw- | 4（二进制110换算成十进制） |
| 0    | 1    | 0     | -w- | 2（二进制010换算成十进制） |
这个权限值在我们后面更改权限时，或者设置初始权限时是有用处的。具体操作暂且不表。

🔑所以这里的`drwxr-xr-x`意思就是文件为目录文件，文件的拥有者的权限为可读可写可执行，同组用户的权限为可读不可写可执行，其他用户的权限为可读不可写可执行。

```
-rw-r--r-- 1 ubuntu ubuntu 8980 Jul 6 2020 example.desktop
drwxr-xr-x 2 ubuntu ubuntu 4096 Jul 6 2020 Music
```

🤔从`drwxr-xr-x`继续往后看，`drwxr-xr-x`后面的`2`是什么意思呢？

🔑这个`2`表示的就是文件的链接数。

目录文件：表示该目录中还有多少个目录。

普通文件：就是指该文件有多少个链接（访问方式）。

```
-rw-r--r-- 1 ubuntu ubuntu 8980 Jul 6 2020 example.desktop
drwxr-xr-x 2 ubuntu ubuntu 4096 Jul 6 2020 Music
```

🤔`drwxr-xr-x 2` 后面的`ubuntu ubuntu`是什么意思呢？

🔑第1个`ubuntu`表示文件的拥有者，第2个`ubuntu`表示所属小组。

🤔`ubuntu`后面的`8980`是什么意思呢？

🔑这个`8980`表示的是文件大小。

🤔`8980`后面的`Jul 6 2020`是什么意思呢？

🔑这个`Jul 6 2020`表示的是修改日期。

🤔`Jul 6 2020`后面的`example.desktop`是什么意思呢？

🔑这个`example.desktop`表示的是文件名。

🔚用一个图来总结。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-3.png" width = "70%" height = "auto">
</div>



## 📌命令：locate或find

作用：查找。

而`find`命令我认为更好用，所以我一般用`find`命令。

举例说明

```shell
find -name Even #在当前目录开始以名字查找 Even 文件
locate Even #从根目录开始查找所有包含 Even 字符串的文件名
```

## 📌命令：clear

作用：清除终端窗口。

快捷方式：Ctrl + L


## 📌命令：cat

作用：打印输出文件内容。

<br/>


</div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-4.png" width = "70%" height = "auto">
</div>




## 📌命令：head

作用：查看某个文件的头部。

```shell
head stdio.h #查看 stdio.h 文件的前10行
head -20 stdio.h #查看 stdio.h 文件的前20行(行数可以自行指定)
```

## 📌命令：tail

作用：查看某个文件的尾部。有助于查看日志文件的最后10行来阅读重要的系统信息。

```shell
tail stdio.h #查看 stdio.h 文件的末尾10行
tail -20 stdio.h #查看 stdio.h 文件的末尾20行(行数可以自行指定)
```



## 📌命令：grep

作用：在文件中查找指定的内容。

```shell
grep printf stdio.h -n #在stdio.h中查找内容 printf 并输出内容所出现的行号
```

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-5.png" width = "70%" height = "auto">
</div>


## 📌命令：chmod

作用：修改文件的权限。

```shell
chmod 367 Even #修改文件 Even 的权限为拥有者可写+可执行，同组用户可读+可写，其他用户可读+可写+可执行 
```

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-6.png" width = "70%" height = "auto">
</div>



## 📌命令：cp

作用：复制文件。

```shell
cp Even Jacy #把普通文件 Even 复制成 Jacy
cp abc/ ade -r #把目录文件 adc 复制成 ade  -r为递归选项(复制目录文件就需要加 -r )
```

💡小知识：  

`cp adc/ ade -r`这里的`adc/`这么写是为了提醒我们自己这是目录文件，建议输入命令时在目录文件后加`/`。

也可以写`cp adc ade -r`效果是一样的。


## 📌命令：mv

作用：移动文件/重命名。

```shell
mv Even abc/ #把文件 Even 移动到目录 abc 中
mv adc/ ade/ #把目录 adc 移动到目录 ade 中
mv abb acc #把文件 abb 重命名为 acc
mv aaa/ bbb/ccc #把目录 abb 移动到目录 bbb中，并且将目录 abb 重命名为 ccc 
```

📢注意：

mv具体是移动还是重命名的功能取决于第三个参数是否存在，如果第三个参数是一个不存在的文件名，则是重命名。如果第三个参数存在并且是一个目录文件，则是移动文件。



## 📌命令：mkdir

作用：创建目录文件。

```shell
mkdir abb #创建一个目录文件为 abb
```



## 📌命令：touch

作用：创建一个普通文件。

```shell
touch abc.c #创建一个普通文件 abc.c
```

## 📌命令：rm

作用：删除文件。

```shell
rm abc.c #删除普通文件 abc.c
rm acc/ -r #目录文件 abc/
```

## 📌命令：sudo

作用：临时使用超级用户的权限做某事。


<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-7.png" width = "70%" height = "auto">
</div>

<br/>

```shell
sudo touch abc.c #临时使用超级用户的权限，创建一个普通文件
```
<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-8.png" width = "70%" height = "auto">
</div>

```shell
sudo -s #临时切换为超级用户(不推荐这么使用，因为这样一直保持超级用户权限，容易误删东西。推荐sudo+命令去使用)
exit #输入 exit 可以退出超级用户
```

<br/>


<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%91%BD%E4%BB%A4%E5%85%A5%E9%97%A8-9.png" width = "70%" height = "auto">
</div>


## 📌命令：man

```shell
man man #查看一下 man 手册是什么
man -f mkfifo #查看一下 mkfifo 出现在哪一本手册中 
man 1 mkfifo #在第一本手册中查看 mkfifo 的解析(命令)
man 3 mkfido #在第三本手册中查看 mkfido 的解析(命令)
```

一共有9本手册。

1）可执行程序或shell命令。  
2）系统调用（内核提供的函数）  
3）库调用（程序库中的函数）
等等......

💡小知识：  
系统调用 VS 库调用

你给女朋友准备一个晚饭，比如做一个小炒黄牛肉，系统调用就是你自己去买牛肉，买葱，买调料等等，包括怎么做，做多久，都由你把握。  

而库调用，就是你去一个餐馆，点一个小炒黄牛肉，你只需要买单就可以了。

系统调用使用的系统提供的接口是比较简单而功能单一的，你需要去拼凑出来一个完整的功能。  

而库调用就是，它已经帮你拼凑了很多系统函数来组成一个完整的库函数。

这两者详细的细节暂且不表，在后续的部分会做详细解释。



## VMtools安装

想要实现Linux和Windows之间的复制粘贴，则需安装VMtools。


## END

>**END：本节内容到此结束。**

个人提升之余，别忘了和小伙伴积极交流，很多人觉得他们在思考，而实际上他们只是在重新整理自己的偏见。请珍惜和他人交流讨论的机会。

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END1.jpg" alt = "END1.jpg" width = "70%" height = "auto">
</div>

<br/>

希望你每一天都有所收获，进步up up up。今天的我们并不比昨天更聪明，但一定要比昨天更睿智。

<br/>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END2.jpg" alt = "END2.jpg" width = "70%" height = "auto">
</div>

