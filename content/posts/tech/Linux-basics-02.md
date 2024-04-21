---
title: "Linux入门：基础命令与实用技巧"
date: 2024-04-21T00:17:58+08:00
lastmod: 2024-04-21T00:17:58+08:00
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
description: "本博客为新手量身打造，系统介绍了Linux操作系统的基本概念、核心命令及其应用场景。通过实例演示，帮助读者快速掌握Linux操作技巧，为进一步学习打下坚实基础。"
weight: 2
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

## INIT

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/INIT.jpg" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

>**INIT：本节内容正式开始。action!**

## Linux是什么

🤔Linux是什么？

🔑让我们来聊聊Linux这个电脑界的“开源侠客”。

Linux，一个听起来像希腊神话中英雄的名字，实际上是一个自由、开源的操作系统内核，由林纳斯·托瓦兹（Linus Torvalds）在1991年“一剑封喉”，从此开启了开源世界的大门。想象一下，Linux就像是一位无私的大师，将内功心法（源代码）公开于世，让众多弟子（开发者）得以修炼、改进，使其成为众多服务器和超级计算机的“座右铭”（操作系统）。

Linux以其稳定性、安全性和灵活性著称，就像一位多才多艺的侠客，无论是在企业服务器的“江湖”中，还是在嵌入式系统的“小巷”里，都能见到它的身影。它不需要昂贵的“武功秘籍”（许可证），也不需要定期向“朝廷”（软件公司）进贡（付费更新），它就像一位行侠仗义的隐士，默默地守护着数据和隐私。

Linux的发行版（Distros）多如繁星，每个都有自己的特色和“门派”（社区），从新手友好的Ubuntu到高效能的Debian，从桌面系统到服务器，Linux都能提供合适的“武器”（软件）来应对各种挑战。

总之，Linux就像是一位无所不能的电脑界英雄，以其开源的精神，让技术的世界更加多元和自由。无论你是编程新手还是资深开发者，Linux都能成为你值得信赖的“伙伴”，一起在数字世界中“行侠仗义”。

📚定义：Linux是一种开源操作系统，以其稳定性、安全性和高度可定制性而受到广泛欢迎。它基于Unix，由林纳斯·托瓦兹在1991年首次发布。用户和开发者社区庞大，支持多种硬件平台，广泛应用于服务器、桌面和嵌入式系统等场景。

当我们讲到Linux，其实它有两层含义。

1）Linux内核它运行于系统的最底层，用户看不见也摸不到。
- 内存的管理
- 文件管理
- 任务管理
- 网络管理
- 设备管理

2）Linux发行版本

Ubuntu

国内乃至全球热门的Linux发行版。也是各种推荐入门Linux爱好者安装的一个Linux发行版。它的特点主要有以下：
- 安装简单
- Unity3D图形界面，比较华丽（因人而异）
- 对一些专有驱动支持比较好，例如显卡驱动
- 社区比较活跃，几乎遇到的问题都可以找到答案
- 版本更新较快，基本半年一个版本
 
Ubuntu的发行规律  
每个偶数年121416182022>的4月份就发行一个长期支持版（正式版本5年）。

>参考：[Linux系统调用 | 内核态与用户态的转换_用户态、内核态转换失败怎么办-CSDN博客](https://blog.csdn.net/weixin_40134955/article/details/103223856)

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.png" alt = "Linux入门与基础命令.png" width = "70%" height = "auto">
</div>
<br>

如上图所示，从宏观上来看，Linux操作系统的体系架构分为**用户态**和**内核态**（或者用户空间和内核）。内核从本质上看是一种**软件**——**控制计算机的硬件资源**，并提供上层应用程序运行的环境。用户态即**上层应用程序的活动空间**，应用程序的执行必须依托于内核提供的资源，包括CPU资源、存储资源、I/O资源等。为了使上层应用能够访问到这些资源，内核必须为上层应用提供访问的接口：**即系统调用。**

**系统调用**是操作系统的最小功能单位，这些系统调用根据不同的应用场景可以进行扩展和裁剪，现在各种版本的Unix实现都提供了不同数量的系统调用，如**Linux的不同版本提供了240-260个系统调用**，FreeBSD大约提供了320个（reference：UNIX环境高级编程）。我们可以把系统调用看成是一种不能再化简的操作，有人把它比作一个汉字的一个“笔画”，而一个“汉字”就代表一个上层应用，我觉得这个比喻非常贴切。因此，有时候如果要实现一个完整的汉字（给某个变量分配内存空间），就必须调用很多的系统调用。如果从实现者的角度来看，良好的程序设计方法是：**重视上层的业务逻辑操作，而尽可能避免底层复杂的实现细节**。库函数正是为了将开发者从复杂的细节中解脱出来而提出的一种有效方法。它实现对系统调用的封装，将简单的业务逻辑接口呈现给用户，方便用户调用，从这个角度上看，库函数就像是组成汉字的“偏旁”。这样的一种组成方式极大增强了程序设计的灵活性，对于简单的操作，我们可以直接调用系统调用来访问资源，如“人”，对于复杂操作，我们借助于库函数来实现。显然，这样的库函数依据不同的标准也可以有不同的实现版本，如ISO C 标准库，POSIX标准库等。

**Shell是一个特殊的应用程序，俗称命令行**，本质上是一个_命令解释器_，它下通系统调用，上通各种应用，通常充当着一种“胶水”的角色，来连接各个小功能程序，让不同程序能够以一个清晰的接口协同工作，从而增强各个程序的功能。同时，Shell是可编程的，它可以执行符合Shell语法的文本，这样的文本称为Shell脚本，通常短短的几行Shell脚本就可以实现一个非常大的功能，原因就是这些**Shell语句通常都对系统调用做了一层封装**。为了方便用户和系统交互，一般，一个Shell对应一个终端，终端是一个硬件设备，呈现给用户的是一个图形化窗口。我们可以通过这个窗口输入或者输出文本。这个文本直接传递给shell进行分析解释，然后执行。

总结一下，用户态的应用程序可以通过三种方式来访问内核态的资源：

- 系统调用
- 库函数
- Shell脚本

## vim编辑器

### Linux vi/vim

所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。

但是目前我们使用比较多的是 vim 编辑器。

vim 具有程序编辑的能力，可以主动的以字体颜色辨别语法的正确性，方便程序设计。

### 什么是 vim？

Vim 是从 vi 发展出来的一个文本编辑器。代码补全、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

简单的来说， vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。 vim 则可以说是程序开发者的一项很好用的工具。

### vi/vim 的使用

>参考：[Linux vi/vim | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-vim.html)

基本上 vi/vim 共分为三种模式，**命令模式（Command Mode）、输入模式（Insert Mode）和命令行模式（Command-Line Mode）**。

#### 命令模式

**用户刚刚启动 vi/vim，便进入了命令模式。**

此状态下敲击键盘动作会被 Vim 识别为命令，而非输入字符，比如我们此时按下 i，并不会输入一个字符，i 被当作了一个命令。

以下是普通模式常用的几个命令：

- i -- 切换到输入模式，在光标当前位置开始输入文本。
- x -- 删除当前光标所在处的字符。
- : -- 切换到底线命令模式，以在最底一行输入命令。
- a -- 进入插入模式，在光标下一个位置开始输入文本。
- o：在当前行的下方插入一个新行，并进入插入模式。
- O -- 在当前行的上方插入一个新行，并进入插入模式。
- dd -- 剪切当前行。
- yy -- 复制当前行。
- p（小写） -- 粘贴剪贴板内容到光标下方。
- P（大写）-- 粘贴剪贴板内容到光标上方。
- u -- 撤销上一次操作。
- Ctrl + r -- 重做上一次撤销的操作。
- :w -- 保存文件。
- :q -- 退出 Vim 编辑器。
- :q! -- 强制退出Vim 编辑器，不保存修改。

若想要编辑文本，只需要启动 Vim，进入了命令模式，按下 i 切换到输入模式即可。

命令模式只有一些最基本的命令，因此仍要依靠**底线命令行模式**输入更多命令。

#### 输入模式

在命令模式下按下 i 就进入了输入模式，使用 Esc 键可以返回到普通模式。

在输入模式中，可以使用以下按键：

- **字符按键以及Shift组合**，输入字符
- **ENTER**，回车键，换行
- **BACK SPACE**，退格键，删除光标前一个字符
- **DEL**，删除键，删除光标后一个字符
- **方向键**，在文本中移动光标
- **HOME**/**END**，移动光标到行首/行尾
- **Page Up**/**Page Down**，上/下翻页
- **Insert**，切换光标为输入/替换模式，光标将变成竖线/下划线
- **ESC**，退出输入模式，切换到命令模式

#### 底线命令模式

在命令模式下按下 :（英文冒号）就进入了底线命令模式。

底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。

在底线命令模式中，基本的命令有（已经省略了冒号）：

- `:w`：保存文件。
- `:q`：退出 Vim 编辑器。
- `:wq`：保存文件并退出 Vim 编辑器。
- `:q!`：强制退出Vim编辑器，不保存修改。

按 ESC 键可随时退出底线命令模式。

简单的说，我们可以将这三个模式想成底下的图标来表示：


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-1.png" alt = "Linux入门与基础命令-1.png" width = "70%" height = "auto">
</div>

## Linux基础命令

### 📌命令：alias

作用：给某个命令取别名。



<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-2.png" alt = "Linux入门与基础命令-2.png" width = "70%" height = "auto">
</div>
<br>

上面操作为临时的操作，当终端重启后则恢复原状。

如何让它永久？

其中一个方法是修改配置文件《每一次打开终端都会执行该配置文件》

```shell
vim ~/.bashrc #使用 vim 打开配置文件
```

在配置文件的末尾添加一行，把修改的命令写进去。

<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-3.png" alt = "Linux入门与基础命令-3.png" width = "70%" height = "auto">
</div>
<br>


📢修改配置文件时一定要小心，保险起见记得备份配置文件一份。

💡小技巧：

假如想要每次打开终端时都显示一句话，比如打印"It's another day full of energy. Come on."，（今天又是元气满满的一天，加油吧）

那么可以修改配置文件。

```shell
vim ~/.bashrc
```

在配置文件的末尾添加一行，把修改的命令写进去。

```shell
echo "It's another day full of energy. Come on."
```

这样每次打开终端的时候，就会显示"It's another day full of energy. Come on."了。



### 📌命令：date

作用：直接输出日期与时间（也可以设置时间）
```shell
date #输出时间
```

<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-4.png" alt = "Linux入门与基础命令-4.png" width = "70%" height = "auto">
</div>
<br>


```shell
sudo date -s "2077/12/1" #设置时间
```



<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-5.png" alt = "Linux入门与基础命令-5.png" width = "70%" height = "auto">
</div>
<br>



### 📌命令：dpkg

作用：软件包安装。

1）先下载dpkg的安装包，后缀.deb文件。



### 📌命令：echo

作用：回显，把你输入的字符输出到终端。


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-6.png" alt = "Linux入门与基础命令-6.png" width = "70%" height = "auto">
</div>
<br>

后面在shell编程中可以作为打印输出。



### 📌命令：sort

作用：对文件内容进行排序。

```shell
sort max.c > max.txt #重定向输出，把原本输出到标准输入文件的内容重定向到sort.txt文件中。
```



### 📌命令：which

作用：查看命令所在的位置。

```shell
which chmod
```

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-7.png" alt = "Linux入门与基础命令-7.png" width = "70%" height = "auto">
</div>

### 📌命令：uniq

作用：去掉文件中重复项然后输出。

```shell
uniq max.c
```



### 📌 | 管道

可以用来链接两个命令。

命令1 | 命令2 

把命令1的输出作为命令2的输入。

```shell
ls -la | sort #把 ls -la 的输出作为 sort的输入
```



<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-8.png" alt = "Linux入门与基础命令-8.png" width = "70%" height = "auto">
</div>
<br>


```shell
sort max.c | uniq #先对文件进行排序，然后再通过 uniq 进行去重复
```



### 📌 命令：diff

作用：检查文件是否相同。

这是一个非常牛的命令，用来对比两个文件或者目录的异同，并将之加工成符合某种格式的文档，这就是大名鼎鼎的补丁文件。毫不夸张地说：神器diff是各类版本管理软件（例如svn、git等）的基石。

（嵌入式用得不太多，一般补丁都是在大工程里面，嵌入式工程再大也就几十mb，这个部分自行拓展吧）。



### 📌 命令：file

作用：查看文件的格式信息。
 

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-9.png" alt = "Linux入门与基础命令-9.png" width = "70%" height = "auto">
</div>
<br>



```shell
file /bin/ls
```


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-10.png" alt = "Linux入门与基础命令-10.png" width = "70%" height = "auto">
</div>
<br>




可以看到这里显示这是x86-64位平台，我们的开发板可能是32位的，这个地方后面会改变，此处暂且不表。



### 📌 命令：wc

作用：计算字符数，单词数，行数。

```shell
wc ls.txt
```

<br>
<div align=center>
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-11.png" alt = "Linux入门与基础命令-11.png" width = "70%" height = "auto">
</div>
<br>


`7`表示行数，`56`表示单词数，`320`表示字符数，`ls.txt`就是文件名。



## 进程管理命令

🤔什么是进程？

进程：运行起来的程序就称为进程，它占用一定内存空间。



### 📌 命令：ps

作用：查看进程信息。

```shell
ps -ef
```

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-12.png" alt = "Linux入门与基础命令-12.png" width = "70%" height = "auto">
</div>
<br>


`init` 进程是系统所有进程的起点，你可以把它比拟成系统所有进程的老祖宗，没有这个进程，系统中任何进程都不会启动。

可以看到所有进程除了`init` 进程都有一个父进程号，也就是说除了`init` 进程，其他进程都有一个父进程。

图中`TTY`为？的进程为守护进程，意思是终端的关闭并不影响该进程。（举个例子：你手机在打游戏，关闭手机的话，游戏也就关闭了，但是守护进程就是挂机的游戏，你关闭手机之后，它依然在后台运行。）

进程名为`-bash`的进程就是你打开的终端，你打开两个终端的话，就会显示两个`-bash`的进程。


💡小知识：守护进程

许多程序需要开机启动。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。

init进程的一大任务，就是去运行这些开机启动的程序。

守护进程（Daemon）是在后台运行的服务进程，它不受用户直接控制，通常在系统启动时开始运行，并在系统关闭时结束。守护进程通常没有控制终端，这意味着它们不会与任何终端设备关联，可以在用户登录和注销的情况下持续运行。

守护进程的目的是提供对系统资源或网络服务的持续监控和管理。它们负责执行系统任务，如打印作业的排队（lpd）、网络连接的管理（sshd）、文件系统的同步（fsync）等。守护进程通常以超级用户（root）权限运行，以访问系统资源和执行需要特权操作的任务。

守护进程的特点包括：

1. 后台运行：守护进程在后台运行，不与任何终端会话直接交互。
2. 无控制终端：守护进程没有控制终端，因此不会受到用户登录或注销的影响。
3. 持续运行：守护进程通常在系统启动时启动，并在系统运行期间持续运行，直到系统关闭。
4. 父进程：守护进程的父进程通常是`init`进程（在较新的系统中是`systemd`），这样即使原始的父进程结束了，守护进程仍然可以继续运行。

在Linux系统中，守护进程通常通过`init`系统（如`systemd`、`upstart`或传统的`sysvinit`）来管理，这些系统负责启动、停止和管理守护进程的生命周期。



### 📌 命令：top

作用：查看进程信息（动态的）（类似于windows的进程管理器）。


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-13.png" alt = "Linux入门与基础命令-13.png" width = "70%" height = "auto">
</div>
<br>




```shell
top
```
<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-14.png" alt = "Linux入门与基础命令-14.png" width = "70%" height = "auto">
</div>
<br>




### 📌命令：kill

作用：向某一个进程发送信号。

```shell
kill -l #查看 Linux 的信号
kill -2 873 #向进程ID为873的进程发送了一个编号为1的信号，即 SIGINT 信号。
killall -3 a.out #向所有名为 a.out 的进程发送了一个编号为3的信号，即SIGQUIT 信号。默认情况下，SIGQUIT 信号会导致接收它的进程立即终止。
killall -SIGHUP a.out #向所有名为 a.out 的进程发送了一个 SIGHUP 信号，默认情况下，SIGHUP 会导致进程终止。

```


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-15.png" alt = "Linux入门与基础命令-15.png" width = "70%" height = "auto">
</div>
<br>


> kill发送的信号大部分作用都是杀掉进程。

📋案例：演示`kill -1 进程号`和`kill -2 进程号`关闭1个进程。

首先写一个程序，让这个程序一直打印{啦啦啦德玛西亚}，这样我们再`kill -2`中断该进程时就能明显看到效果。

```c
#include<stdio.h>

int main(int argc, char const *argv[])
{
	int i = 0;
	while(1)
	{
		printf("%d--啦啦啦德玛西亚\n",i++);
		sleep(1);
	}
	return 0;
}
```

然后我们编译运行该程序


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-16.png" alt = "" width = "70%" height = "auto">
</div>
<br>

此时我们再开一个终端，去查看该进程。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-17.png" alt = "Linux入门与基础命令-17.png" width = "70%" height = "auto">
</div>
<br>



可以看到`ps`命令没有查看到进程，所以我们输入命令`ps -ef`查看全部进程。


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-18.png" alt = "Linux入门与基础命令-18.png" width = "70%" height = "auto">
</div>
<br>



我们看到这个进程的进程号为842。

接下来我们使用`kill -1 进程号`来挂起该进程（实际上就是终止该进程）。

```shell
kill -1 842 #向进程ID为842的进程发送了一个编号为1的信号，即 SIGHUP 信号。
```


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-19.png" alt = "Linux入门与基础命令-19.png" width = "70%" height = "auto">
</div>
<br>

同样，我们再次运行该程序，`ps -ef`查到该进程号为873。

接下来我们使用`kill -2 进程号`来中断该进程（实际上就是终止该进程）。

```shell
kill -2 873 #向进程ID为873的进程发送了一个编号为1的信号，即 SIGINT 信号。
```

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-20.png" alt = "Linux入门与基础命令-20.png" width = "70%" height = "auto">
</div>
<br>



#### 命令：killall

在Unix和Linux系统中，`killall`命令用于向所有匹配特定名称的进程发送信号。

📋案例：演示用`killall -3 进程名`关闭一个进程。

```shell
killall -3 a.out #向所有名为 a.out 的进程发送了一个编号为3的信号，即SIGQUIT 信号。默认情况下，SIGQUIT 信号会导致接收它的进程立即终止。
```

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-21.png" alt = "Linux入门与基础命令-21.png" width = "70%" height = "auto">
</div>
<br>



📋案例：演示用`killall -SIGHUP 进程名`关闭一个进程。

```shell
killall -SIGHUP a.out #向所有名为 a.out 的进程发送了一个 SIGHUP 信号，默认情况下，SIGHUP 会导致进程终止。
```
<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-22.png" alt = "Linux入门与基础命令-22.png" width = "70%" height = "auto">
</div>
<br>



*kill发送的信号大部分作用都是杀掉进程。但是编号为18和19的信号不是杀掉进程的。*

📋案例：演示`killall -18 进程名`和`killall -19 进程名`的效果。

```shell
killall -19 a.out #这条命令是向所有名为 a.out 的进程发送了一个编号为19的信号，即 SIGSTOP 信号。SIGSTOP 信号用于停止一个进程的执行，而不会导致它终止。
```
<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-23.png" alt = "Linux入门与基础命令-23.png" width = "70%" height = "auto">
</div>
<br>





```shell
killall -18 a.out #向所有名为 a.out 的进程发送了一个编号为18的信号，即 SIGCONT 信号。 SIGCONT 信号用于继续一个之前被 SIGSTOP 或 SIGTSTP 信号停止的进程。
```

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-24.png" alt = "Linux入门与基础命令-24.png" width = "70%" height = "auto">
</div>
<br>



💡小知识：

`1 SIGHUP` 信号：

`SIGHUP` 信号传统上用于通知终端的挂起，默认情况下，`SIGHUP`会导致进程终止。

`2 SIGINT`信号：

`SIGINT`通常用于中断一个进程，它类似于用户在终端上按下`Ctrl+C`时发送的信号。

`3 SIGQUIT`信号：

默认情况下，`SIGQUIT`信号会导致接收它的进程立即终止，并且在大多数情况下，会在当前工作目录下创建一个核心转储文件，文件名通常是`core`或`core.<pid>`，其中`<pid>`是进程的ID。

`18 SIGCONT`信号：

`SIGCONT`信号通常用于恢复被暂停的进程，特别是在进程管理或调试过程中。与`SIGSTOP`信号不同，`SIGCONT`可以被进程捕获和处理，但通常情况下，进程会继续其正常的执行。

`19 SIGSTOP`信号：

`SIGSTOP`信号用于停止一个进程的执行，而不会导致它终止。当进程接收到`SIGSTOP`信号时，它会立即暂停执行，并且不会继续直到收到`SIGCONT`信号。


## 压缩与解压缩命令

### 📌 命令：tar

tar的基础选项。

```txt
-c: 创建归档文件。（创建一个压缩包）
-x: 释放归档文件。（解压一个压缩包）
-t: 查看归档文件（或者压缩文件）
-f: 指定要归档、压缩或者查看的文件的名称。
-v: 显示命令执行过程。
-z: 使用 gzip 压缩工具进行相应的压缩/解压
-j: 使用bzip2 压缩工具进行相应的压缩/解压
-J: 解压xz文件可以使用该选项
```


归档（把若干个文件整合成一个文件）

```shell
tar -cf demo.tar a.out demo.c ls.txt max.c min.c sort.txt #将文件`a.out`、`demo.c`、`ls.txt`、`max.c`、`min.c`和`sort.txt`打包到一个名为`demo.tar`的归档文件中。
tar -cf demo1.tar * #把当前路径下的所有文件进行归档生成为 demo1.tar 的文件
```


<br>
<div align= "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4.jpg" alt = "Linux入门与基础命令.jpg" width = "70%" height = "auto">
</div>
<br>

查看归档文件内容

```shell
tar -tf demo1.tar #查看归档文件的内容 
```

释放归档文件

```shell
tar -xvf demo1.tar -C abc # -C 是指定文件释放的路径， abc 就是当前目录下的 abc 目录
```

压缩与解压缩 gzip 文件

```shell
tar -czvf demo.tar.gz * # 把当前目录下所有文件归档并压缩为 `demo.tar.gz`
tar -xzvf demo.tar.gz -C ~ #把 `demo.tar.gz` 解压并释放到家目录
```

压缩与解压缩 bzip2 文件

```shell
tar -cjvf demo.tar.bz2 * # 把当前目录下所有文件归档并压缩为 `demo.tar.bz2`
tar -xjvf demo.tar.bz2 -C ~ #把 `demo.tar.bz2` 解压并释放到家目录
```

xz格式的压缩与解压

xz 命令不可以单独使用，因此必须先使用 tar 命令对文件进行归档， 归档后再使用xz进行压缩

```shell
xz demo.tar # 直接使用 xz 命令对归档文件进行压缩
```

解压 xz 格式文件

```shell
tar -Jxvf arm-linux-gnueabi-5.4.0.tar.xz
```

💡小知识：xz格式文件

比如说我们的内核压缩包就是xz格式文件，包括我们的交叉工具链。

📢注意：

文件的格式并不取决于文件名的后缀，后缀只是给我们看的。

举个例子

```shell
tar -czvf demo.tar.gz * # 把当前目录下所有文件归档并压缩为 `demo.tar.gz`
mv demo.tar.gz demo.tar.even #重命名文件 `demo.tar.gz` 为 `demo.tar.even`
tar -xzvf demo.tar.even -C ~ #将`demo.tar.even`解压并释放到家目录
```

可以看到我们将`demo.tar.gz` 重命名为 `demo.tar.even`后，虽然后缀名不是压缩包的后缀名，但是我们依然可以解压它，它本身仍然是压缩包。



### 📌 命令：zip

zip命令并不是Ubuntu 自带的，它需要我们手动来安装

```shell
sudo apt install zip #安装`zip`
```

使用zip 命令进行压缩：

```shell
zip test.zip * # 压缩当前目录下的所有文件，不包含子目录内容
zip test.zip * -r #添加`-r`选项之后`zip`命令在压缩的时候会把子目录进行压缩
```

📋案例：演示`zip test.zip *`的效果。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-25.png" alt = "Linux入门与基础命令-25.png" width = "70%" height = "auto">
</div>
<br>



可以看到包含子目录的目录文件压缩率为0%，说明该命令压缩当前目录下的所有文件，不包含子目录内容。

📋案例：演示`zip test.zip * -r`的效果。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-26.png" alt = "Linux入门与基础命令-26.png" width = "70%" height = "auto">
</div>
<br>



可以看到，有更深层次的压缩，这里压缩了当前目录下的`abc/`目录。说明添加`-r`选项之后`zip`命令在压缩的时候会把子目录进行压缩。

使用`unzip`命令进行解压

```shell
unzip test.zip
```



## apt命令

`apt`的定义：

`apt`是Linux操作系统中，特别是基于Debian的系统（如Ubuntu）中的一个命令行工具，用于处理软件包的安装、升级、配置和删除。它是高级包装工具（Advanced Package Tool）的简称，这个工具为用户提供了一个统一的接口来处理软件包。

1）设置软件源。

```shell
sudo vim /etc/apt/sources.list #编辑`/etc/apt/sources.list`文件来设置软件源
```

`ubuntu`也可以直接进入设置选项去设置软件源。也就是图中的`Software&Updates`

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-27.png" alt = "Linux入门与基础命令-27.png" width = "70%" height = "auto">
</div>
<br>


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-1.jpg" alt = "Linux入门与基础命令-1.jpg" width = "70%" height = "auto">
</div>
<br>




这里我们用的软件源是阿里云的。

在Linux上使用`apt`安装软件通常包括以下步骤：

1） **更新软件包列表**：

首先，你需要更新本地的软件包列表，以确保你看到的软件包信息是最新的。这可以通过以下命令完成：

   ```bash
   sudo apt update
   ```

2）**升级已安装的软件包**：
   
在安装新软件之前，你可能想要升级系统中已有的软件包。这可以通过以下命令完成：

   ```bash
   sudo apt upgrade
   ```

这个步骤是可选的，但建议执行，以确保系统的稳定性和安全性。

3）**搜索软件包**：

如果你不知道要安装的软件包的确切名称，你可以使用`apt`的搜索功能来查找它：

   ```bash
   apt search <package-name>
   ```

替换`<package-name>`为你想要安装的软件包的名称或相关关键词。

4）**安装软件包**：

一旦你知道了要安装的软件包的名称，你可以使用以下命令来安装它：

   ```bash
   sudo apt install <package-name>
   ```
   
   替换`<package-name>`为你想要安装的软件包的名称。

5）**配置软件包**：

有些软件包在安装过程中会询问你一些配置问题。按照提示操作即可。如果你不确定如何配置，通常可以选择默认设置。

6） **验证安装**：

安装完成后，你可以通过运行软件包或使用以下命令来验证它是否已正确安装：

   ```bash
   dpkg -l | grep <package-name>
   ```

如果软件包已安装，这个命令将显示软件包的详细信息。

7）**卸载软件包**（如果需要）：

如果你不想要某个软件包了，可以使用以下命令来卸载它：

   ```bash
   sudo apt remove <package-name>
   ```

如果你想要同时删除配置文件，可以使用`purge`代替`remove`：

   ```bash
   sudo apt purge <package-name>
   ```

请注意，这些步骤需要在具有管理员权限的账户下执行，因此需要使用`sudo`命令。此外，不同的Linux发行版可能会有细微的差别，但上述步骤适用于大多数基于Debian的系统。




## 网络命令

### 📌 命令：hostname

作用：查看主机名。

```shell
hostname
```

### 📌 命令：ifconfig

作用： 查看当前的网络配置状态。

```shell
ifconfig #查看的网卡 eth0 --- ens33 为网卡的名字
```

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-28.png" alt = "Linux入门与基础命令-28.png" width = "70%" height = "auto">
</div>
<br>

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-29.png" alt = "Linux入门与基础命令-29.png" width = "70%" height = "auto">
</div>
<br>




修改ip地址

📢修改IP地址前，记得先`ping`一下该地址，确保要使用的IP地址没有被占用。

```shell
ipconfig eth0 192.168.25.3 #修改 eth0 的网卡
ipconfig ens33 192.168.25.3 #修改 ens33 的网卡
```

开启/关闭网卡

```shell
 ipconfig eth0 up #启动 eth0 的网卡
 ipconfig eth0 down #关闭 eth0 的网卡
```


### 📌 命令：ping

作用：ping命令检查网络是否连通。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-30.png" alt = "Linux入门与基础命令-30.png" width = "70%" height = "auto">
</div>
<br>



如果像上面图中显示有延迟值，则说明网络没有问题。

💡小知识：生存时间（TTL）

TTL（Time To Live）是一个IP数据包中的一个值，它指定了数据包在被丢弃之前可以通过的最大路由器数。TTL的主要目的是防止数据包在网络中无限循环。每次数据包通过一个路由器时，TTL值就会减1。当TTL值减到0时，数据包将被路由器丢弃，并且通常会发送一个ICMP超时消息给源地址。

在Linux和Windows系统中，TTL的默认值是不同的。例如：

- 在Linux系统中，TTL通常设置为64。
- 在Windows系统中，TTL通常设置为128。

>🤔**这个值和我们在淘宝上抢购东西有关。何出此言呢？**

TTL值也可以用来大致判断数据包的源距离。例如，如果TTL值很高，那么数据包可能不需要经过很多路由器就能到达目的地，这意味着源和目的地可能在网络拓扑上比较近。相反，如果TTL值很低，那么数据包可能需要经过很多路由器，这意味着源和目的地可能在网络拓扑上比较远。

每次数据包通过一个路由器时，TTL值就会减1。假如你`ping`淘宝网的时候，发现`ttl`值很小，那么说明你离淘宝的服务器很远，可能这就是抢购不到东西的原因。🤣



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-31.png" alt = "Linux入门与基础命令-31.png" width = "70%" height = "auto">
</div>
<br>

#### ubuntu设置网络信息

📢如果你用的是wsl系统，那么就不用另外设置虚拟机的IP，因为wsl系统和你的主机是同一个IP。假如要改IP的话，也是去改自己主机的IP。

1）打开配置文件。

```shell
sudo vim /etc/network/interfaces
```

2）修改配置文件，设置为自动使用DHCP来自动分配IP。

```shell
auto ens33
iface ens33 inet dhcp
```
<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-32.png" alt = "Linux入门与基础命令-32.png" width = "70%" height = "auto">
</div>
<br>



或者设置IP为静态地址

```shell
auto ens33 #指示系统在启动时自动激活网络接口`ens33`。
iface ens33 inet static #指定`ens33`接口使用静态IP地址配置。
address 192.168.25.5 #设置接口的IPv4地址为`192.168.25.5`。
netmask 255.255.255.0 #设置子网掩码为`255.255.255.0`，这定义了网络的大小。
gateway 192.168.25.1 #设置默认网关为`192.168.25.1`，这是网络中用于访问其他网络或互联网的设备。
```
<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-33.png" alt = "Linux入门与基础命令-33.png" width = "70%" height = "auto">
</div>
<br>



📢

请注意，如果你使用的是NetworkManager来管理网络连接，那么您可能需要使用`nmcli`或`nmtui`工具来设置静态IP地址，而不是直接编辑`/etc/network/interfaces`文件。

此外，一些Linux发行版可能已经迁移到使用`systemd-networkd`来管理网络配置，在这种情况下，您可能需要在`/etc/systemd/network`目录中创建网络配置文件。

请注意，随着NetworkManager和其他网络配置工具的普及，直接编辑 `/etc/network/interfaces` 文件的方式在些现代Linux发行版中已经不那么常见了。在这些系统中，网络配置通常通过图形界面或命令行工具（如 `nmcli` 或 `nmtui`）来进行。

3）重启网络服务

```shell
sudo service networking force-reload
sudo service networking restar
```

#### windows设置网络


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Linux-Basics/Linux%E5%85%A5%E9%97%A8%E4%B8%8E%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4-34.png" alt = "Linux入门与基础命令-34.png" width = "100%" height = "auto">
</div>
<br>

## END

>**END：本节内容到此结束。**

个人提升之余，别忘了和小伙伴积极交流，很多人觉得他们在思考，而实际上他们只是在重新整理自己的偏见。请珍惜和他人交流讨论的机会。

<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END1.jpg" alt = "END1.jpg" width = "70%" height = "auto">
</div>
<br>

希望你每一天都有所收获，进步up up up。今天的我们并不比昨天更聪明，但一定要比昨天更睿智。

<br>
<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/END2.jpg" alt = "END2.jpg" width = "70%" height = "auto">
</div>



## 参考
（1） [Linux系统调用 | 内核态与用户态的转换_用户态、内核态转换失败怎么办-CSDN博客](https://blog.csdn.net/weixin_40134955/article/details/103223856)  

（2）[Linux vi/vim | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-vim.html)  

（3）[Linux 系统启动过程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-system-boot.html)

