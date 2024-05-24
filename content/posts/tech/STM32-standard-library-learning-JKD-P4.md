---
title: "嵌入式开发-STM32标准库学习：新建Keil工程"
date: 2024-05-12T00:17:58+08:00
lastmod: 2024-05-12T00:17:58+08:00
author: ["Eddy"]
keywords: 
- STM32
- embedded
- Embedded development
- MCU
- ARM Cortex-M
- Development Board
- 开发板
- STM32简介
- STM32标准库
- 嵌入式开发
- Keil uVision
categories: 
- 
tags: 
- STM32
- MCU
- ARM Cortex-M
- 开发板
- STM32标准库开发
- 嵌入式开发
- Keil uVision
description: "本文档旨在指导STM32平台的新手开发者如何建立工程项目，并有效地运用标准库和HAL库来提高开发效率。首先，文档强调了基于寄存器、标准库和HAL库三种不同的STM32开发方式，并推荐初学者优先考虑使用标准库进行开发。其次，它详细介绍了在STM32环境中添加启动文件、设置头文件路径以及创建用户自定义函数的基本步骤，同时给出了基于寄存器进行开发的示例。文章还说明了如何通过库函数更方便地配置GPIO和控制LED，从而简化了对硬件的操作。此外，文档涵盖了如何新建工程、选择合适的启动文件以及调试器设置，旨在帮助开发者有效下载和运行程序。最后，通过一系列实例，文章阐述了基于库函数的STM32工程构建和理解，突出了中断服务函数的重要性和在工程中的应用。整体而言，本文档为STM32平台的开发者提供了一套完整且实用的开发指导，从基础设置到高级应用应有尽有。"
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/pc9.jpg" #图片路径例如：posts/tech/123/123.png
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

本小节我们来建立一个STM32的工程，这个STM32的工程结构还是比较复杂的，需要用到很多文件，之后我们的代码也都是需要建立在工程结构上的。

所以，在开始学习后续内容之前，我们先来学习一下如何新建STM32的工程。目前STM32的开发方式主要有基于寄存器的方式、基于标准库也就是库函数的方式和基于HAL库的方式。

基于寄存器的方式和我们51单片机的开发方式一样，是用程序直接配置寄存器，来达到我们想要的功能。这种方式最底层最直接，效率会更高一些。但是由于STM32的结构复杂，寄存器太多，所以基于寄存器的方式目前是不推荐的。

基于库函数的方式是使用ST官方提供用的封装好的函数，通过调用这些函数来间接的配置寄存器。由于ST对寄存器封装的比较好，所以这种方式既能满足对寄存器的配置，对开发人员也比较友好，有利于提高开发效率。我们本课程使用的就是库函数的开发方式。

最后一个基于HAL库的方式，可以用图形化界面快速配置STM32。这个比较适合快速上手STM32的情况。但是这种方式隐藏了底层逻辑。  
如果你对STM32不熟悉，基本只能停留在很浅的水平。所以目前暂时不推荐HAL库。但是推荐你学过标准库之后，去了解一下这个方式，毕竟这个HAL库还是非常方便的。

那使用库函数的方式，我们需要准备一个STM32库函数的压缩包。大家可以在我提供的资料链接里找到固件库的文件，然后可以看到STM32F10x标准外设库这个压缩包，我们先把它解压。打开解压后的文件夹，在这里就是酷函数的文件夹目录了。

好，那接下来我们就正式开始新建一个基于标准库的工程。首先我们需要先建立一个存放工程的文件夹，比如在D盘、E盘等位置。那我这里方便起见，就在桌面新建了，起个名字，可以叫STM32Project。以后我们的工程都存在这个文件夹下，这样比较方便管理。

下面开始新建工程。
## 一、选择对应芯片的器件支持包

选择器件支持包为ST32F103C8


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B.png" alt = "P4-新建工程.png" width = "70%" height = "auto">
</div>
<br>

## 二、添加工程所需的必要文件

### 1. 复制STM32的启动文件

现在这个工程还是不能直接用的，我们需要给他添加一些工程的必要文件。

```markdown
D:\江科大 STM32\固件库\STM32F10x_StdPeriph_Lib_V3.5.0\STM32F10x_StdPeriph_Lib_V3.5.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x\startup\arm
```

图：STM32的启动文件 



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-1.png" alt = "P4-新建工程-1.png" width = "70%" height = "auto">
</div>
<br>

那这些就是STM32的启动文件，STM32的程序就是从启动文件开始执行的。

我们把这些文件全部都复制下来，然后回到工程模板文件夹里，可以看到这些就是我们刚才新建工程自动生成的文件。

图：刚才新建工程自动生成的文件



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-2.png" alt = "P4-新建工程-2.png" width = "70%" height = "auto">
</div>
<br>

### 2. 在工程目录中新建Start文件夹用来存放工程所需文件

#### 2.1 复制复制STM32的启动文件到工程目录的Start文件夹

那如果直接把启动文件也放在这里，就有点太乱了，是吧？所以我们需要新建一个文件夹，可以叫做Start。然后把启动文件粘贴到这里面。

图：把启动文件粘贴到Start文件夹



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-3.png" alt = "P4-新建工程-3.png" width = "70%" height = "auto">
</div>
<br>

#### 2.2 复制外设寄存器描述文件stm32Ff10x.h和用于配置时钟的system文件

接着我们回到固件库的STM32F10x文件夹，可以看到stm32Ff10x.h和两个system开头的文件。

这个stm32Ff10x.h，就是STM32的外设寄存器描述文件。它的作用就跟51单片机的头文件REGX52.H一样。是用来描述STM32有哪些寄存器和它对应的地址的。

这两个system文件是用来配置时钟的。STM32主频72MHz，就是system文件里的函数配置的。

那我们把这三个文件复制下来，也粘贴到start文件夹下。

图：`D:\江科大 STM32\固件库\固件库\STM32F10x_StdPeriph_Lib_V3.5.0\STM32F10x_StdPeriph_Lib_V3.5.0\Libraries\CMSIS\CM3\DeviceSupport\ST\STM32F10x`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-4.png" alt = "P4-新建工程-4.png" width = "70%" height = "auto">
</div>
<br>



图：`D:\Keil5Project\STM32Project\2-1 STM32工程模板\Start`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-5.png" alt = "P4-新建工程-5.png" width = "70%" height = "auto">
</div>
<br>

#### 2.3 复制内核寄存器描述文件core_cm3.h和内核的配置函数core_cm3.c

接下来，因为这个STM32是内核和内核外围的设备组成的，而且这个内核的寄存器描述和外围设备的描述文件不是在一起的。所以我们还需要添加一个内核寄存器的描述文件。

我们可以打开CM3 CoreSupport，这2个CM3(Cortex-M3)文件就是内核的寄存器描述，当然它还带了一些内核的配置函数，所以多了个点儿.c文件。我们把它俩一并复制下来，也粘贴到Start文件夹下。

到此为止，我们工程的必要文件就复制完成了。

图：`D:\江科大 STM32\固件库\STM32F10x_StdPeriph_Lib_V3.5.0\STM32F10x_StdPeriph_Lib_V3.5.0\Libraries\CMSIS\CM3\CoreSupport`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-6.png" alt = "P4-新建工程-6.png" width = "70%" height = "auto">
</div>
<br>


图：`D:\Keil5Project\STM32Project\2-1 STM32工程模板\Start`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-7.png" alt = "P4-新建工程-7.png" width = "70%" height = "auto">
</div>
<br>

### 3. 在Keil工程中新建Start文件夹并将Start所需文件加入进来

#### 3.1 在Keil工程中新建Start文件夹

然后我们回到Keil软件，把我们刚才复制的那些文件添加到工程里来。我们可以点击选中这个Source Group，然后再单击一下，把这个组改一下名字，也叫Start。

图：将Source Group改名为Start



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-9.png" alt = "P4-新建工程-9.png" width = "70%" height = "auto">
</div>
<br>



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-8.png" alt = "P4-新建工程-8.png" width = "70%" height = "auto">
</div>
<br>

#### 3.2 将Start文件夹所需的文件添加进来

接着右键，选择添加已经存在的文件到组里，打开Start文件夹，把下面这个文件过滤器选择all files，这样我们就能看到文件夹里的所有文件了。

我们首先添加一下启动文件，这个启动文件有很多分类，我们只能添加其中一个。我们视频所用型号需要选择这个后缀为md.s的启动文件，至于启动文件怎么选择，我等会再讲。

我们选中它，点击add。然后剩下的.c和.h文件都要添加进来。我们可以按住Ctrl键，然后依次选择它们，点击Add，然后Close，这样我们Start文件夹里面的文件就添加好了。

图：添加完启动文件之后的Project目录



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-10.png" alt = "P4-新建工程-10.png" width = "70%" height = "auto">
</div>
<br>

这里的文件都是STM32里最基本的文件，是不需要我们修改的，我们添加进来即可。大家可以看到这个文件图标上带了个小钥匙，这个意思是他是个只读文件，我们可以打开试一下，这些信息都是不让我们修改的。

#### 3.3 在Keil工程中添加头文件路径

最后我们还需要在工程选项里添加上这个文件夹的头文件路径，要不然软件是找不到.h文件的。

我们点击这个魔术棒按钮，打开工程选项，在C/C++里找到这个Include Paths栏，然后点击右边三个点的按钮，在这里新建路径，然后再点三个点的按钮，把Start的路径添加进来，点击ok，这样就把这个文件夹的头文件路径添加进来了。

图：添加头文件路径



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-11.png" alt = "P4-新建工程-11.png" width = "70%" height = "auto">
</div>
<br>

图：新建 头文件路径



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-12.png" alt = "P4-新建工程-12.png" width = "70%" height = "auto">
</div>
<br>

图：添加Start文件夹头文件路径成功



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-13.png" alt = "P4-新建工程-13.png" width = "70%" height = "auto">
</div>
<br>

## 三、在Keil工程中新建User文件夹存放.c代码并编写代码

### 1. 在Keil工程中新建User文件夹 

接下来我们再新建一个main函数，看看这个工程是不是可行。我们打开工程文件夹，然后新建一个文件夹。叫做User，我们的main函数就放在这个文件夹里。

图：在工程文件夹目录下新建User文件夹

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-14.png" alt = "P4-新建工程-14.png" width = "70%" height = "auto">
</div>
<br>

然后Keil里，在Target这里右键，点击添加组，改个名字叫User。

图：在Keil里添加组 



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-15.png" alt = "P4-新建工程-15.png" width = "70%" height = "auto">
</div>
<br>

图：将新添加的组命名为User



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-16.png" alt = "P4-新建工程-16.png" width = "70%" height = "auto">
</div>
<br>

### 2. 在Keil工程中User文件夹新建main.c

然后在User上右键，点击添加新文件，选择.c文件，名字叫main。

图：在User文件夹 添加新文件



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-17.png" alt = "P4-新建工程-17.png" width = "70%" height = "auto">
</div>
<br>

下面的路径注意一下，要选择User文件夹，要不然默认是放在文件夹外面的。然后点击Add，这样我们就有了main.c文件了。

那在工程文件夹的User目录下也可以看到我们新建的main.c文件。

图：选择路径为User文件夹 添加main.c。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-19.png" alt = "P4-新建工程-19.png" width = "70%" height = "auto">
</div>
<br>

图：添加main.c成功



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-20.png" alt = "P4-新建工程-20.png" width = "70%" height = "auto">
</div>
<br>

### 3. 编写main.c，插入头文件

在这个main.c里，我们先右键，插入头文件，选择stm32f10x.h。

图：在main.c中插入头文件



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-21.png" alt = "P4-新建工程-21.png" width = "70%" height = "auto">
</div>
<br>

然后写一个main函数。
按Tab键可以进行缩进，里面写一个while(1)死循环。
这里注意main函数是一个int型返回值，void参数的函数。还有文件的最后一
行必须要是空行，要不然会报警告。

图：在main.c中编写代码



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-22.png" alt = "P4-新建工程-22.png" width = "70%" height = "auto">
</div>
<br>


## 四、编译工程

然后我们点击这个按钮，编译并建立工程。

图：编译



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-23.png" alt = "P4-新建工程-23.png" width = "70%" height = "auto">
</div>
<br>

图：编译结果 显示1警告 0错误



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-25.png" alt = "P4-新建工程-25.png" width = "70%" height = "auto">
</div>
<br>

为什么这里会显示1警告呢？

点击“魔法棒”，点击“output”，将“Create HEX File”勾选上。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-24.png" alt = "P4-新建工程-24.png" width = "70%" height = "auto">
</div>
<br>

这样就不会出现警告了。

可以看到下面提示的是零错误，零警告，那这就说明我们建立的工程是没问题的。

这个工程目前还没有添加STM32的库函数，所以它还是一个基于寄存器开发的工程。如果你想用寄存器开发STM32，那工程建到这里就可以了。

## 五、编程前期准备

接下来我给大家演示一下如何通过配置寄存器来完成点灯的操作。当然直接操作寄存器的方式不是我们本课程的重点，大家了解一下即可。

### 1. 调大字体

那我们先把这个界面的字体调大点，现在的字体太小，看着不舒服。

我们可以点击这个扳手工具。

图：点击这个扳手工具



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-26.png" alt = "P4-新建工程-26.png" width = "70%" height = "auto">
</div>
<br>

选择颜色和字体，选择C/C++编辑器的选项。然后点击这个按钮把字号调成14。

图：将C/C++编辑器的 Text 字体调整为Size：14



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-28.png" alt = "P4-新建工程-28.png" width = "70%" height = "auto">
</div>
<br>

这个Asm编辑器的字体，我们也把它调成14号了，然后OK，这样字体就变大了。

图：将Asm编辑器的 Default 字体调整为Size：14



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-29.png" alt = "P4-新建工程-29.png" width = "70%" height = "auto">
</div>
<br>

图：调大字体后的效果



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-30.png" alt = "P4-新建工程-30.png" width = "70%" height = "auto">
</div>
<br>

### 2. 设置编码格式

另外我们再点一下这个扳手工具，把这个编码格式选成UTF8这一项，这样可以防止一些中文乱码的问题。  

图：设置编码格式为UTF8



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-31.png" alt = "P4-新建工程-31.png" width = "70%" height = "auto">
</div>
<br>

当然你要打开我的工程，也要把编码格式选为UTF8，否则中文显示就会出现问题。

如果你打开别人的工程，看到中文是乱码的话，可能还需要再改一下这个编码格式。

### 3. 设置Tab键的size

下面这个Tab键的大小，选择4，这个缩进大小我比较习惯，然后点击ok，这样界面看起来就舒服一些。

图：设置Tab size为4



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-32.png" alt = "P4-新建工程-32.png" width = "70%" height = "auto">
</div>
<br>

图：设置Tab size为4后的效果



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-33.png" alt = "P4-新建工程-33.png" width = "70%" height = "auto">
</div>
<br>

### 4. 将STM32最小系统板通过STLINK连接电脑

接着我们需要拿出STM32的最小系统板，STLINK和4根母对母的杜邦线，按照插针边上的标识，把3.3V、SWDIO、SWCLK、GND对应连接好，插好之后就是这个样子的。

图：将STM32的最小系统板连线



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-34.png" alt = "P4-新建工程-34.png" width = "70%" height = "auto">
</div>
<br>

然后把STLINK插在电脑上。插上电之后，这个板子上的电源灯应该会常亮，另一个连接在PC13口上的灯默认应该是闪烁状态，这是芯片里的一个测试程序。

图：把STLINK插在电脑上



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-36.png" alt = "P4-新建工程-36.png" width = "70%" height = "auto">
</div>
<br>

### 5. 在Keil里面配置STLINK的调试器

然后我们再在Keil里面配置一下调试器，点击魔术棒按钮，选择Debug。这个调试器默认是ULINK，我们用的是STLINK，所以选择ST-Link Debugger。  

图：在Keil中选择调试器为ST-Link Debugger



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-38.png" alt = "P4-新建工程-38.png" width = "70%" height = "auto">
</div>
<br>

然后再点击右边的设置按钮，在Flash下载这一项，把这个Reset and Run这一项勾上，勾上这一项之后，我们下载程序后会立马复位并执行，这样方便一些。

否则每次下载之后，还需要按一下板子上的复位按键才能执行程序。那配置好调试器之后，点击确认，ok。

图：勾选Reset and Run这一项



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-39.png" alt = "P4-新建工程-39.png" width = "70%" height = "auto">
</div>
<br>

然后重新编译一下没有错误，再点击这个LOAD按钮，如果一切正常的话，这个程序就会下载到STM32里面了。

图：下载程序到开发板



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-40.png" alt = "P4-新建工程-40.png" width = "70%" height = "auto">
</div>
<br>

我们看一下板子，可以看到这个灯已经不闪了，因为我们目前的程序啥都没有。

图：下载程序到开发板后，灯不闪了



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-41.png" alt = "P4-新建工程-41.png" width = "70%" height = "auto">
</div>
<br>

## 六、寄存器编程实现点灯

那接下来，我们就配置一下寄存器，来点亮这个灯。我们只需要配置3个寄存器就可以点灯了，我们可以打开STM32的参考手册。

手册为：《STM32F10xxx参考手册（中文）》

### 1. 使能GPIOC的时钟

首先是RCC的一个寄存器，来使能GPIOC的时钟。GPIO都是APB2的外设，所以在这个APB2外设时钟使能寄存器RCC_APB2ENR里面配置。

可以看到，这里有个IOPCEN，这一位就是使能GPIOC的时钟的。下面的解释是，这一位写1，就是打开GPIOC的时钟，那这一位写1，其他的无关项我们先都给0。

图：APB2外设时钟使能寄存器 位4 IOPCEN



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-42.png" alt = "P4-新建工程-42.png" width = "70%" height = "auto">
</div>
<br>

图：APB2外设时钟使能寄存器 位4 IOPCEN 解释



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-43.png" alt = "P4-新建工程-43.png" width = "70%" height = "auto">
</div>
<br>

那整个寄存器的2进制数据换成16进制，就是4个一分组，也就是00000010。

图：寄存器的2进制数据换成16进制，就是4个一分组：0 0 0 0 0 0 1 0



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-44.png" alt = "P4-新建工程-44.png" width = "70%" height = "auto">
</div>
<br>

然后我们回到Keil，在while死循环之前，写上RCC的APB2ENR寄存器=0x00000010，这样就可以打开GPIOC的时钟了。

图：寄存器编程打开GPIOC的时钟



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-45.png" alt = "P4-新建工程-45.png" width = "70%" height = "auto">
</div>
<br>

### 2. 配置PC13口的模式

然后第二个寄存器，我们需要配置一下PC13口的模式。

我们可以找到端口配置高寄存器，寄存器GPIOLx_CRH，这个x可以是A到E的任意一个字母。

然后右边的CNF13和MODE13就是用来配置13号口的。

图：端口配置高寄存器GPIOLx_CRH，CNF13和MODE13用来配置13号口



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-46.png" alt = "P4-新建工程-46.png" width = "70%" height = "auto">
</div>
<br>

下面的说明我们看一下，这个CNF我们需要配置为通用推挽输出模式，也就是这两位为00。MODE要配置为输出模式，最大速度可以给50MHz，也就是这两位为11。

图：CNF为00 MODE为11



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-47.png" alt = "P4-新建工程-47.png" width = "70%" height = "auto">
</div>
<br>

最后我们对照上面的寄存器，这四位为0011，其他的我们也都给它配置为0。这样整个寄存器的值换算成16进制就是0 0 3 0 0 0 0 0。

图：整个寄存器的值换算成16进制就是0 0 3 0 0 0 0 0



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-48.png" alt = "P4-新建工程-48.png" width = "70%" height = "auto">
</div>
<br>

然后我们回到Keil，在这里写上GPIOC的CRH=0x00300000。

图：寄存器编程配置PC13口的模式



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-49.png" alt = "P4-新建工程-49.png" width = "70%" height = "auto">
</div>
<br>

### 3. 给PC13口输出数据

接下来我们就可以给PC13口输出数据了。我们可以看到这个端口输出数据寄存器GPIOx_ODR中间有一位ODR13，这一位写1，13号口就是高电平，写0就是低电平。

图：端口输出数据寄存器GPIOx_ODR  



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-50.png" alt = "P4-新建工程-50.png" width = "70%" height = "auto">
</div>
<br>

如果写1的话，ODR的值就是00002000。

在这里我们写上GPIOC的ODR=0x00002000。

因为这个灯是低电平点亮的，所以我们给ODR全为0，就是亮。给ODR 0x00002000就是灭。

那我们试一下，先给ODR全0，编译，下载。这时可以看到这个PC13的灯已经亮起来了。

图：给GPIOC->ODR 全0



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-51.png" alt = "P4-新建工程-51.png" width = "70%" height = "auto">
</div>
<br>

图：给GPIOC->ODR 全0 PC13灯亮



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-52.png" alt = "P4-新建工程-52.png" width = "70%" height = "auto">
</div>
<br>

如果我们给ODR 2000的话，编译，下载。此时灯就灭了。

图：给GPIOC->ODR 2000



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-53.png" alt = "P4-新建工程-53.png" width = "70%" height = "auto">
</div>
<br>

图：给GPIOC->ODR 2000 PC13灯灭



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-54.png" alt = "P4-新建工程-54.png" width = "70%" height = "auto">
</div>
<br>

这就是配置寄存器的方式进行点灯的操作。可以看出来，这种方式需要不断地查手册来了解每个寄存器的每一位都是干啥的。

而且这个操作方式也有个弊端，就是我们把除了PC13之外的位都配置成了0，这样会影响到其他端口的原有配置，如果要做到只配置PC13而不影响其他位，那还需要&=和|=的操作，这个在51单片机的视频里我们也经常遇到。

那这样配置就会更加麻烦。所以这种寄存器的操作方式，虽然代码简洁，但是还是不太方便。

## 七、库函数编程实现点灯

那接下来我们就要为这个工程添加库函数了，看看库函数数和寄存器的操作方式有哪些区别。

### 1. 在工程文件夹新建Library文件夹用来存放库函数

我们打开工程文件夹，在这里新建一个文件夹叫Library，用来存放库函数。

图：`D:\Keil5Project\STM32Project\2-1 STM32工程模板`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-55.png" alt = "P4-新建工程-55.png" width = "70%" height = "auto">
</div>
<br>

接着打开固件库的文件夹，打开Libraries，STM32标准外设驱动，src，这些就是库函数的源文件。

图：库函数的源文件 `D:\江科大 STM32\固件库\固件库\STM32F10x_StdPeriph_Lib_V3.5.0\STM32F10x_StdPeriph_Lib_V3.5.0\Libraries\STM32F10x_StdPeriph_Driver\src`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-56.png" alt = "P4-新建工程-56.png" width = "70%" height = "auto">
</div>
<br>

这个misc是内核的库函数，其他的就是内核外的外设库函数了。这个misc就是混杂的意思，看来这个ST公司还是不厚道，把内核的库函数都发配到杂项里面去。开个玩笑。

那我们按Ctrl+A全选，然后复制，在Library文件夹下粘贴。

然后再打开固件库的inc文件夹，这些是库函数的头文件。我们继续Ctrl+A全选，然后复制，在Library文件夹下粘贴。

图：加入库函数文件的Library文件夹 `D:\Keil5Project\STM32Project\2-1 STM32工程模板\Library`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-57.png" alt = "P4-新建工程-57.png" width = "70%" height = "auto">
</div>
<br>

### 2. Keil工程中新建组Library并导入库函数文件

接着回到Keil软件，同样在Target处右键，然后添加组，改个名字叫Library。再右键添加已经存在的文件，把Library文件夹下所有文件加进来。

图：Keil工程中新建组Library



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-58.png" alt = "P4-新建工程-58.png" width = "70%" height = "auto">
</div>
<br>

图：Keil工程中Library导入库函数文件



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-59.png" alt = "P4-新建工程-59.png" width = "70%" height = "auto">
</div>
<br>

这样就把所有的库函数文件都添加进来了。但是对于这个库函数来说，现在还不能直接使用，我们需要再添加一个文件。

我们打开固件库文件夹，打开Project，STM32Template，可以看到stm32f10x_conf.h和两个it结尾的文件。

图：stm32f10x_conf.h和两个it结尾的文件 `D:\江科大 STM32\固件库\STM32F10x_StdPeriph_Lib_V3.5.0\STM32F10x_StdPeriph_Lib_V3.5.0\Project\STM32F10x_StdPeriph_Template`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-60.png" alt = "P4-新建工程-60.png" width = "70%" height = "auto">
</div>
<br>

这个conf(configuration)文件是用来配置库函数头文件的包含关系的。另外这里面还有一个用来参数检查的函数定义，这是所有库函数都需要的.

两个it(interrupt)文件是用来存放中断函数的。我们把这三个文件复制下来，然后粘贴到工程的User目录下。

图：将三个文件粘贴到工程的User目录下  `D:\Keil5Project\STM32Project\2-1 STM32工程模板\User`



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-61.png" alt = "P4-新建工程-61.png" width = "70%" height = "auto">
</div>
<br>

接着回到Keil软件，在User组里把刚才那三个文件添加进来。

图：Keil工程中在User组里把刚才那三个文件添加进来



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-62.png" alt = "P4-新建工程-62.png" width = "70%" height = "auto">
</div>
<br>

### 3. Keil工程中设置宏定义以及头文件路径

最后还需要一个宏定义，我们可以在这个头文件右键，打开文件。

图：进入头文件



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-63.png" alt = "P4-新建工程-63.png" width = "70%" height = "auto">
</div>
<br>

然后滑到最下面。

图：stm32f10x.h中的 一个条件编译



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-64.png" alt = "P4-新建工程-64.png" width = "70%" height = "auto">
</div>
<br>

看到这个语句，这是一个条件编译，意思是如果你定义了使用标准外设驱动USE_STDPERIPH_DRIVER这个字符串，下面这个include conf.h语句才有效。

所以我们还需要复制一下这个字符串，然后打开工程选项，在C/C++的Define栏目粘贴这个字符串，这样才能包含标准外设库，也就是库函数。

当然还有下面的头文件路径，也不要忘了把这个User和Library目录的路径也都添加上。

图：设置工程宏定义，设置头文件路径



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-65.png" alt = "P4-新建工程-65.png" width = "70%" height = "auto">
</div>
<br>

然后OK，这样我们基于库函数的工程就建好了。

我们可以看一下这个Library里面的库函数也都带了钥匙，不需要我们更改。我们唯一需要更改的就是User组里面的这些文件。

### 4. 整理工程目录

我们点一下这个三个箱子的按钮，把这个Library往上挪下，把这些不用改的都放到最上面，这样看着舒服一下。

图：三个箱子 按钮



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-66.png" alt = "P4-新建工程-66.png" width = "70%" height = "auto">
</div>
<br>

图：把Library移上去



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-67.png" alt = "P4-新建工程-67.png" width = "70%" height = "auto">
</div>
<br>

图：把Library移上去的效果



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-68.png" alt = "P4-新建工程-68.png" width = "70%" height = "auto">
</div>
<br>

那我们编译看一下。这个第一次编译会比较慢，以后就快一些。

可以看到是0错误，0警告，这说明我们的工程建立是成功的。

然后我们再用库函数来实现点灯的操作。我们先把这三句删了，库函数其实也是间接的配置寄存器，所以它们的步骤也是一样的。

### 5. 使用RCC_APB2外设时钟控制函数 RCC_APB2PeriphClockCmd()来开启时钟

首先是使能时钟，那库函数就有这样一个函数来开启时钟，叫RCC_APB2外设时钟控制 RCC_APB2PeriphClockCmd()。然后这里提示有两个参数，第一个是选择外设，第二个是选择新的状态。

图：RCC_APB2外设时钟控制函数 RCC_APB2PeriphClockCmd()



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-69.png" alt = "P4-新建工程-69.png" width = "70%" height = "auto">
</div>
<br>

我们可以右键跳到函数定义 Go To Definition of ，这上面有函数的简介和参数说明。简介说这个函数是用来使能或者失能APB2的外设时钟。

第一个参数可以是下面这些值。那我们可以找到APB2外设GPIOC这一项，然后复制，直接作为第一个参数即可。

然后我们再回过去看第二个参数，NewState的值可以是ENABLE或者DISABLE。那我们复制ENABLE，放在第二个参数的位置。

最后别忘了括号和分号，这样GPIOC的外设时钟就配置好了。

图：在main.c中，使用库函数配置GPIOC的外设时钟



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-70.png" alt = "P4-新建工程-70.png" width = "70%" height = "auto">
</div>
<br>

我们可以看一下这个函数，它的内部其实还是配置RCC_APB2ENR这个寄存器。但是经过函数的包装，我们不需要再去查手册，来确认哪一位是干啥的了。

函数定义：RCC_APB2PeriphClockCmd()

```c
/**
  * @brief  Enables or disables the High Speed APB (APB2) peripheral clock.
  * @param  RCC_APB2Periph: specifies the APB2 peripheral to gates its clock.
  *   This parameter can be any combination of the following values:
  *     @arg RCC_APB2Periph_AFIO, RCC_APB2Periph_GPIOA, RCC_APB2Periph_GPIOB,
  *          RCC_APB2Periph_GPIOC, RCC_APB2Periph_GPIOD, RCC_APB2Periph_GPIOE,
  *          RCC_APB2Periph_GPIOF, RCC_APB2Periph_GPIOG, RCC_APB2Periph_ADC1,
  *          RCC_APB2Periph_ADC2, RCC_APB2Periph_TIM1, RCC_APB2Periph_SPI1,
  *          RCC_APB2Periph_TIM8, RCC_APB2Periph_USART1, RCC_APB2Periph_ADC3,
  *          RCC_APB2Periph_TIM15, RCC_APB2Periph_TIM16, RCC_APB2Periph_TIM17,
  *          RCC_APB2Periph_TIM9, RCC_APB2Periph_TIM10, RCC_APB2Periph_TIM11     
  * @param  NewState: new state of the specified peripheral clock.
  *   This parameter can be: ENABLE or DISABLE.
  * @retval None
  */
void RCC_APB2PeriphClockCmd(uint32_t RCC_APB2Periph, FunctionalState NewState)
{
  /* Check the parameters */
  assert_param(IS_RCC_APB2_PERIPH(RCC_APB2Periph));
  assert_param(IS_FUNCTIONAL_STATE(NewState));
  if (NewState != DISABLE)
  {
    RCC->APB2ENR |= RCC_APB2Periph;
  }
  else
  {
    RCC->APB2ENR &= ~RCC_APB2Periph;
  }
}
```

而且这里他已经帮我们用|=和&=来操作了。或者这个库函数的配置是不会影响到寄存器的其他位的，这就是库函数和寄存器的区别。

我们看到这个代码虽然比寄存器长，但是语义更加明确，也不需要我们再查表计算这个寄存器的值了。我们只需要调用库函数，按照它的提示把参数填好就行了。所以说从这点对比上来看，库函数是比寄存器有更大优势的。

好，那我们继续来配置。

### 6. 使用GPIO_Init()函数配置端口模式

第二步是配置端口模式，我们需要用到GPIO_Init这个函数。然后有两个参数，第一个是选择哪个GPIO，第二个是参数的结构体。

这个比上一个函数要麻烦一些，但也是一个套路，我们根据提示来配置参数即可。那我这里来操作一下，这里使用了结构体来配置参数，代码逻辑还是有些复杂的，这个我们下节还会继续讲，大家先跟着我操作就行了。

我们首先还是去到这个函数的定义，可以看到这个函数的介绍是根据GPIO_Init结构体的参数来配置GPIO。

```c
/**
  * @brief  Initializes the GPIOx peripheral according to the specified
  *         parameters in the GPIO_InitStruct.
  * @param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
  * @param  GPIO_InitStruct: pointer to a GPIO_InitTypeDef structure that
  *         contains the configuration information for the specified GPIO peripheral.
  * @retval None
  */
void GPIO_Init(GPIO_TypeDef* GPIOx, GPIO_InitTypeDef* GPIO_InitStruct)
{
  uint32_t currentmode = 0x00, currentpin = 0x00, pinpos = 0x00, pos = 0x00;
  uint32_t tmpreg = 0x00, pinmask = 0x00;
  /* Check the parameters */
  assert_param(IS_GPIO_ALL_PERIPH(GPIOx));
  assert_param(IS_GPIO_MODE(GPIO_InitStruct->GPIO_Mode));
  assert_param(IS_GPIO_PIN(GPIO_InitStruct->GPIO_Pin));  
  
/*---------------------------- GPIO Mode Configuration -----------------------*/
  currentmode = ((uint32_t)GPIO_InitStruct->GPIO_Mode) & ((uint32_t)0x0F);
  if ((((uint32_t)GPIO_InitStruct->GPIO_Mode) & ((uint32_t)0x10)) != 0x00)
  { 
    /* Check the parameters */
    assert_param(IS_GPIO_SPEED(GPIO_InitStruct->GPIO_Speed));
    /* Output mode */
    currentmode |= (uint32_t)GPIO_InitStruct->GPIO_Speed;
  }
/*---------------------------- GPIO CRL Configuration ------------------------*/
  /* Configure the eight low port pins */
  if (((uint32_t)GPIO_InitStruct->GPIO_Pin & ((uint32_t)0x00FF)) != 0x00)
  {
    tmpreg = GPIOx->CRL;
    for (pinpos = 0x00; pinpos < 0x08; pinpos++)
    {
      pos = ((uint32_t)0x01) << pinpos;
      /* Get the port pins position */
      currentpin = (GPIO_InitStruct->GPIO_Pin) & pos;
      if (currentpin == pos)
      {
        pos = pinpos << 2;
        /* Clear the corresponding low control register bits */
        pinmask = ((uint32_t)0x0F) << pos;
        tmpreg &= ~pinmask;
        /* Write the mode configuration in the corresponding bits */
        tmpreg |= (currentmode << pos);
        /* Reset the corresponding ODR bit */
        if (GPIO_InitStruct->GPIO_Mode == GPIO_Mode_IPD)
        {
          GPIOx->BRR = (((uint32_t)0x01) << pinpos);
        }
        else
        {
          /* Set the corresponding ODR bit */
          if (GPIO_InitStruct->GPIO_Mode == GPIO_Mode_IPU)
          {
            GPIOx->BSRR = (((uint32_t)0x01) << pinpos);
          }
        }
      }
    }
    GPIOx->CRL = tmpreg;
  }
/*---------------------------- GPIO CRH Configuration ------------------------*/
  /* Configure the eight high port pins */
  if (GPIO_InitStruct->GPIO_Pin > 0x00FF)
  {
    tmpreg = GPIOx->CRH;
    for (pinpos = 0x00; pinpos < 0x08; pinpos++)
    {
      pos = (((uint32_t)0x01) << (pinpos + 0x08));
      /* Get the port pins position */
      currentpin = ((GPIO_InitStruct->GPIO_Pin) & pos);
      if (currentpin == pos)
      {
        pos = pinpos << 2;
        /* Clear the corresponding high control register bits */
        pinmask = ((uint32_t)0x0F) << pos;
        tmpreg &= ~pinmask;
        /* Write the mode configuration in the corresponding bits */
        tmpreg |= (currentmode << pos);
        /* Reset the corresponding ODR bit */
        if (GPIO_InitStruct->GPIO_Mode == GPIO_Mode_IPD)
        {
          GPIOx->BRR = (((uint32_t)0x01) << (pinpos + 0x08));
        }
        /* Set the corresponding ODR bit */
        if (GPIO_InitStruct->GPIO_Mode == GPIO_Mode_IPU)
        {
          GPIOx->BSRR = (((uint32_t)0x01) << (pinpos + 0x08));
        }
      }
    }
    GPIOx->CRH = tmpreg;
  }
}
```

#### GPIO_Init()函数分析

这个函数是STM32微控制器的库函数，用于初始化GPIO（通用输入输出）端口。GPIO端口可以被配置为不同的模式，用于输入、输出或者作为其他功能的接口。

##### 函数的原型是：

```c
void GPIO_Init(GPIO_TypeDef* GPIOx, GPIO_InitTypeDef* GPIO_InitStruct);
```

##### 参数说明：

1. `GPIOx`: 一个指向GPIO端口的指针，其中`x`可以是A到G之间的字母，代表不同的GPIO端口。
2. `GPIO_InitStruct`: 一个指向`GPIO_InitTypeDef`结构体的指针，该结构体包含了要初始化的GPIO端口的配置信息。
- `GPIOA` 是一个指向GPIOA端口的指针，所以它本身就是一个地址，不需要加上`&`。
- `GPIO_InitStructure` 是一个`GPIO_InitTypeDef`类型的结构体变量，当你想传递这个结构体到函数中时，你需要传递它的地址，因此前面需要加上`&`。

##### 函数功能：

- 初始化指定的GPIO端口，根据`GPIO_InitStruct`中提供的配置信息。
- 配置GPIO端口的工作模式（如输入、输出、上拉/下拉等）。
- 配置输出速度（如果端口被配置为输出）。

##### 使用方法：

1. 定义一个`GPIO_InitTypeDef`类型的变量(结构体实例)，并设置其成员，以指定要初始化的GPIO端口的配置。
2. 调用`GPIO_Init`函数，传入GPIO端口的地址和配置结构体的地址。

`GPIO_InitTypeDef`：

`GPIO_InitTypeDef` 是一个结构体类型，用于定义GPIO端口的初始化参数。当您需要初始化一个GPIO端口时，您需要创建一个这种类型的变量（通常称为结构体实例），我们一般命名这个结构体实例为`GPIO_InitStructure`，并设置其成员变量，以指定您希望如何配置GPIO端口。

##### 示例：

```c
GPIO_InitTypeDef GPIO_InitStructure;
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;  // 设置要初始化的引脚，这里以GPIO的第0个引脚为例
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;  // 设置为推挽输出模式
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;  // 设置输出速度
GPIO_Init(GPIOA, &GPIO_InitStructure);  // 对GPIOA的第0个引脚进行初始化
```

##### 函数内部分析：

- 函数首先通过断言（`assert_param`）检查传入的参数是否有效。
- 然后，它根据`GPIO_InitStruct`中的模式和速度设置，计算出当前模式的值，并根据引脚是低8位还是高8位来配置相应的控制寄存器（CRL或CRH）。
- 如果模式是输入且带有上拉或下拉，则会设置或重置ODR（输出数据寄存器）相应的位。

##### 对小白的讲解：

想象一下，GPIO端口就像你家的电灯开关，你可以通过不同的方式控制它。这个函数就是用来告诉你的STM32微控制器，如何正确地设置这个开关。你需要告诉微控制器，你想要控制的是哪一个开关（通过`GPIOx`指定），以及你想要如何控制它（通过`GPIO_InitStruct`中的配置）。

例如，如果你想要一个开关可以控制LED灯的亮和灭，你就需要设置这个开关为输出模式，并且告诉微控制器，当输出高电平时LED亮，输出低电平时LED灭。

#### GPIO_Init()函数代码使用

##### 第一个参数GPIOx

第一个参数GPIOx，其中x可以是A到G，来选择你要配置哪个GPLIO，那我们是PC13口的LED，所以第一个参数就写GPIOC。

```txt
@param  GPIOx: where x can be (A..G) to select the GPIO peripheral.
```

##### 第二个参数是一个GPIO_InitTypeDef类型的结构体变量

第二个参数是一个GPIO_InitTypeDef类型的结构体变量。

```txt
@param  GPIO_InitStruct: pointer to a GPIO_InitTypeDef structure that
  *         contains the configuration information for the specified GPIO peripheral.
```

我们需要先定义一个结构体，在上面我们先把这个结构体的类型写上，然后给结构体起个名字。这个名字可以随便起，但是根据官方的推荐，我们最好起这样的名字，叫GPIO_InitStructure。

然后我们把结构体的每一个参数填上，复制粘贴结构体的名字，然后用点来引出结构体的参数。可以看到这个结构体有三个参数，分别是GPIO模式、GPIO端口、GPIO速度。我们先把这三个参数都罗列出来。

###### GPIO_InitTypeDef (结构体类型)的定义：

```c
typedef struct
{
  uint16_t GPIO_Pin;             /*!< Specifies the GPIO pins to be configured.
                                      This parameter can be any value of @ref GPIO_pins_define */

  GPIOSpeed_TypeDef GPIO_Speed;  /*!< Specifies the speed for the selected pins.
                                      This parameter can be a value of @ref GPIOSpeed_TypeDef */

  GPIOMode_TypeDef GPIO_Mode;    /*!< Specifies the operating mode for the selected pins.
                                      This parameter can be a value of @ref GPIOMode_TypeDef */
}GPIO_InitTypeDef;
```

💡知识回顾：C语言中typedef结构体的用法

在C语言中，`typedef` 结合结构体定义是一种非常实用的特性，用于为结构体创建一个更简洁、更易读的别名。这不仅让代码更加清晰，也使得程序员在声明和使用结构体变量时更为便捷。下面详细介绍其用法：

**一、基本语法**

基本的 `typedef` 结构体定义语法如下：

```c
typedef struct tag {
    // 结构体成员定义
    数据类型 成员名1;
    数据类型 成员名2;
    ...
} 自定义类型名;
```

- **tag**：这是结构体的标签，可选的，用于指明结构体的类型。在 `typedef` 语句中，这个标签有时会被省略，特别是在你不需要直接通过结构体标签来声明变量时。
- **自定义类型名**：这是通过 `typedef` 关键字创建的新类型名称，之后可以用这个名称直接声明结构体变量，无需再写 `struct` 关键字。

**二、简化定义与使用**

1、完整示例

```c
typedef struct Point {
    int x;
    int y;
} PointType;

PointType p1, p2; // 直接使用PointType声明变量p1和p2
```

在这个例子中，`struct Point` 定义了一个包含两个整型成员 `x` 和 `y` 的结构体，`typedef` 则为这个结构体定义了一个别名 `PointType`。之后就可以直接使用 `PointType` 来声明结构体变量，而不需要写成 `struct Point` 形式。

2、匿名结构体与typedef

有时，结构体定义中不提供标签，这时 `typedef` 更显得尤为重要，因为它允许你为匿名结构体提供一个类型名：

```c
typedef struct {
    int a;
    char b;
} MyStruct;

MyStruct example; // 使用MyStruct声明变量
```

**三、优点**

- **提高代码可读性**：通过给复杂的结构体类型起一个有意义的名字，可以让其他阅读代码的人更容易理解其用途。
- **简化声明**：直接使用类型别名声明变量，代码更简洁，减少出错机会。
- **便于维护和扩展**：修改结构体定义时，只需更改一处即可全局生效，尤其是当该类型被广泛使用时。

**四、注意事项**

- 当定义结构体成员时，注意对齐和大小端问题，可能影响内存使用效率。
- 如果结构体较大，考虑使用指针而非直接实例化，以节省栈空间。
- 虽然 `typedef` 可以让代码更易读，过度使用也可能导致代码难以理解，特别是当类型名没有清晰表达其含义时。

通过合理利用 `typedef` 结构体定义，可以使C语言的编程更加高效、清晰。

所以在这里，`GPIO_InitTypeDef`等价为`struct`，代表结构体类型。

`GPIO_InitTypeDef GPIO_InitStructure;` 也就意味着创建了一个结构体实例`GPIO_InitStructure`。

###### GPIO_InitStructure.GPIO_Mode

代码：GPIO_Mode的定义

```c
typedef struct
{
  uint16_t GPIO_Pin;             /*!< Specifies the GPIO pins to be configured.
                                      This parameter can be any value of @ref GPIO_pins_define */

  GPIOSpeed_TypeDef GPIO_Speed;  /*!< Specifies the speed for the selected pins.
                                      This parameter can be a value of @ref GPIOSpeed_TypeDef */

  GPIOMode_TypeDef GPIO_Mode;    /*!< Specifies the operating mode for the selected pins.
                                      This parameter can be a value of @ref GPIOMode_TypeDef */
}GPIO_InitTypeDef;
```

这右边的介绍说，这个参数可以是GPIOMode_TypeDef里面的一个值。因为这是注释里面的东西，所以没办法用右键跳转了。

那这里我们需要选中GPIOMode_TypeDef这个字符，按一下Ctrl+F，搜索一下这个定义的位置。点击find next，可以看到这是个枚举。GPIOMode就是这里的其中一个值。

图：搜索GPIOMode_TypeDef 



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-75.png" alt = "P4-新建工程-75.png" width = "70%" height = "auto">
</div>
<br>

代码：GPIOMode_TypeDef 的定义

```c
typedef enum
{ GPIO_Mode_AIN = 0x0,
  GPIO_Mode_IN_FLOATING = 0x04,
  GPIO_Mode_IPD = 0x28,
  GPIO_Mode_IPU = 0x48,
  GPIO_Mode_Out_OD = 0x14,
  GPIO_Mode_Out_PP = 0x10,
  GPIO_Mode_AF_OD = 0x1C,
  GPIO_Mode_AF_PP = 0x18
}GPIOMode_TypeDef;
```

然后我们选择Out_PP这一项，复制，这个就是通用推挽输出。

然后在这里写上GPIO_Mode_Out_PP，这样这个参数就配置好了。

编写代码：main.c中配置好了GPIO_InitStructure.GPIO_Mode

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC,ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = ;
	GPIO_InitStructure.GPIO_Speed = ;
	GPIO_Init(GPIOC,);
	
	while(1)
	{
	
	}
}
```

###### GPIO_InitStructure.GPIO_Pin

然后我们继续看一下下一个参数，转到它的定义这里下面出现了一个框，这个是说它的定义有很多个。

图：GPIO_Pin的定义有很多个



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-76.png" alt = "P4-新建工程-76.png" width = "70%" height = "auto">
</div>
<br>

我们来看一下，我们选择这个member这一项，双击，然后跳转的其实还是刚才那个位置。

图：双击 GPIO_Pin的member这一项



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-77.png" alt = "P4-新建工程-77.png" width = "70%" height = "auto">
</div>
<br>

代码：GPIO_Pin的定义

```c
typedef struct
{
  uint16_t GPIO_Pin;             /*!< Specifies the GPIO pins to be configured.
                                      This parameter can be any value of @ref GPIO_pins_define */

  GPIOSpeed_TypeDef GPIO_Speed;  /*!< Specifies the speed for the selected pins.
                                      This parameter can be a value of @ref GPIOSpeed_TypeDef */

  GPIOMode_TypeDef GPIO_Mode;    /*!< Specifies the operating mode for the selected pins.
                                      This parameter can be a value of @ref GPIOMode_TypeDef */
}GPIO_InitTypeDef;
```

然后看一下这个GPIO_Pin的说明，他说这个参数在GPIO_pins_define里面定义了，我们还是一样选中，Ctrl+F，find next。

可以看到这里有个宏定义的列表，我们选择GPIO_Pin_13，复制，然后填在第二个位置。

代码：stm32f10x_gpio.h中GPIO_pins_define

```c
/** @defgroup GPIO_pins_define 
  * @{
  */

#define GPIO_Pin_0                 ((uint16_t)0x0001)  /*!< Pin 0 selected */
#define GPIO_Pin_1                 ((uint16_t)0x0002)  /*!< Pin 1 selected */
#define GPIO_Pin_2                 ((uint16_t)0x0004)  /*!< Pin 2 selected */
#define GPIO_Pin_3                 ((uint16_t)0x0008)  /*!< Pin 3 selected */
#define GPIO_Pin_4                 ((uint16_t)0x0010)  /*!< Pin 4 selected */
#define GPIO_Pin_5                 ((uint16_t)0x0020)  /*!< Pin 5 selected */
#define GPIO_Pin_6                 ((uint16_t)0x0040)  /*!< Pin 6 selected */
#define GPIO_Pin_7                 ((uint16_t)0x0080)  /*!< Pin 7 selected */
#define GPIO_Pin_8                 ((uint16_t)0x0100)  /*!< Pin 8 selected */
#define GPIO_Pin_9                 ((uint16_t)0x0200)  /*!< Pin 9 selected */
#define GPIO_Pin_10                ((uint16_t)0x0400)  /*!< Pin 10 selected */
#define GPIO_Pin_11                ((uint16_t)0x0800)  /*!< Pin 11 selected */
#define GPIO_Pin_12                ((uint16_t)0x1000)  /*!< Pin 12 selected */
#define GPIO_Pin_13                ((uint16_t)0x2000)  /*!< Pin 13 selected */
#define GPIO_Pin_14                ((uint16_t)0x4000)  /*!< Pin 14 selected */
#define GPIO_Pin_15                ((uint16_t)0x8000)  /*!< Pin 15 selected */
#define GPIO_Pin_All               ((uint16_t)0xFFFF)  /*!< All pins selected */

#define IS_GPIO_PIN(PIN) ((((PIN) & (uint16_t)0x00) == 0x00) && ((PIN) != (uint16_t)0x00))

#define IS_GET_GPIO_PIN(PIN) (((PIN) == GPIO_Pin_0) || \
                              ((PIN) == GPIO_Pin_1) || \
                              ((PIN) == GPIO_Pin_2) || \
                              ((PIN) == GPIO_Pin_3) || \
                              ((PIN) == GPIO_Pin_4) || \
                              ((PIN) == GPIO_Pin_5) || \
                              ((PIN) == GPIO_Pin_6) || \
                              ((PIN) == GPIO_Pin_7) || \
                              ((PIN) == GPIO_Pin_8) || \
                              ((PIN) == GPIO_Pin_9) || \
                              ((PIN) == GPIO_Pin_10) || \
                              ((PIN) == GPIO_Pin_11) || \
                              ((PIN) == GPIO_Pin_12) || \
                              ((PIN) == GPIO_Pin_13) || \
                              ((PIN) == GPIO_Pin_14) || \
                              ((PIN) == GPIO_Pin_15))

/**
  * @}
  */
```

编写代码：main.c中配置好了GPIO_InitStructure.GPIO_Pin

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC,ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
	GPIO_InitStructure.GPIO_Speed = ;
	GPIO_Init(GPIOC,);
	
	while(1)
	{
	
	}
}
```

###### GPIO_InitStructure.GPIO_Speed

第三个参数也是一样，右键，跳到定义，选中，Ctrl+F， find next。

代码：stm32f10x_gpio.h中GPIOSpeed_TypeDef

```c
typedef enum
{ 
  GPIO_Speed_10MHz = 1,
  GPIO_Speed_2MHz, 
  GPIO_Speed_50MHz
}GPIOSpeed_TypeDef;
```

在这里选中50MHz的速度复制，在这里粘贴，最后别忘了分号。

编写代码：main.c中配置好了GPIO_InitStructure.GPIO_Speed

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC,ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOC,);

	while(1)
	{
	
	}
}
```

那结构体变量就有了，我们就可以填这个GPIO_Init的第二个参数了。在这里有说明，这个第二个参数是一个指向结构体的指针，所以这里我们需要传递结构体的地址。

那我们复制结构体的名字粘贴到这个位置，然后前面加上一个取地址(&)的符号，最后打成一个右括号，分号，这个GPIO模式配置就完成了。

编写代码：GPIO模式配置完成

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC,ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOC,&GPIO_InitStructure);

	while(1)
	{
	
	}
}
```

这个配置的操作方式看上去比较难理解，但是STM32的这种方式都是固定的，大家多打几次就知道怎么用了。

最后我们设置端口的高低电平来进行点灯。这里有个函数GPIO_SetBits，这个就可以把指定端口设置为高电平。大家也可以右键去看一下参数的说明，那这里我就直接填了。

第一个是GPIOC，第二个是GPIO_Pin_13。这一句就可以将PC13号口置为高电平，接下来自低电瓶也有函数叫GPIO_ResetBits。参数同样是GPIO_Pin_13，这一句就可以将PC13号口置为低电平。

那我们依次试一下，我们先把高电平的注释掉。编译。下载。

编写代码：设置PC13号口为低电平，点亮LED灯

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC,ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOC,&GPIO_InitStructure);
	
	//GPIO_SetBits(GPIOC,GPIO_Pin_13);
	GPIO_ResetBits(GPIOC,GPIO_Pin_13);

	while(1)
	{
	
	}
}
```

可以看到灯已经亮了。

图：PC13号口的LED灯亮



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-78.png" alt = "P4-新建工程-78.png" width = "70%" height = "auto">
</div>
<br>

再把低电平的注释掉，编译。

编写代码：设置PC13号口为高电平，熄灭LED灯

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC,ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOC,&GPIO_InitStructure);
	
	GPIO_SetBits(GPIOC,GPIO_Pin_13);
	//GPIO_ResetBits(GPIOC,GPIO_Pin_13);

	while(1)
	{
	
	}
}
```

可以看到灯就灭了。

图：PC13号口的LED灯灭



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-79.png" alt = "P4-新建工程-79.png" width = "70%" height = "auto">
</div>
<br>

这就是使用库函数的基本操作了。这节把下节的课也带着讲了。这至于STM32 GPIO口的结构，结构体和库函数的操作，这些我们下节再详细解释。

本节我们先来用一下，来测试一下我们的工程。

有关新建工程的部分，我们这节大概就介绍完了。最后还要补充几点，来看一下PPT。

## 新建工程里启动文件的选择

首先是新建工程里的启动文件选择，我们新建工程第一个加的就是启动文件。这个启动文件有很多类型，至于选择哪一个，我们要根据芯片型号来选择。

图：STM32F1系列的启动文件一览



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-81.png" alt = "P4-新建工程-81.png" width = "70%" height = "auto">
</div>
<br>

### STM32F1系列型号分类表

我们看到这个表，这里是STM32F1系列中的型号分类。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-80.png" alt = "P4-新建工程-80.png" width = "70%" height = "auto">
</div>
<br>

其中根据Flash的大小，分为了小容量产品，Flash为16~32K，简写为LD(Low Density)。

中容量产品，Flash为16~128K，简写为MD(Medium Density)。

大容量产品，Flash为256~512克，简写为HD(High Density)。

加大容量产品，Flash大于512K，简写为XL(EXtra Large)。

那STM32F100的系列，ST把它叫做超值系列，简写为VL(Value Line)。

SDM32F105和107。ST把它叫做互联型产品(CL, Connectivity Line)。这个就没有根据Flash大小再分类了。

所以就有了这个表。

### STM32F1系列型号分类表的使用

**1、STM32F100型号**  
如果你使用STM32F100的型号，就选择带VL的启动文件，然后再根据Flash的大小选择LD、MD还是HD。

**2、STM32F101/102/103的型号**  
如果你使用STM32F101/102/103的型号，就选择不带VL的，然后根据Flash的大小选择LD、MD、HD还是XL。

**3、STM3F105/107的型号**  
如果你使用STM3F105/107的型号，直接选择CL的启动文件即可。

那在这里我们可以看一下，这个启动文件后面带的字母和我这个表的字母都是对应的。

这样我们就知道了我们这个STM32F103的芯片就需要选择这四列(LD、MD、HD还是XL)。又因为C8T6的Flash是64K所以选择MD的启动文件(也就是startup_stm32f10x_md.s)。

这就是STM32F1系列的型号分类和启动文件的选取。

## 新建工程步骤

接下来我们再总结一下新建工程的步骤。

### 第一步

第一步、建立工程文件夹，Keil中新建工程，选择型号。  

### 第二步

第二步、工程文件夹里建立Start、Library、User等文件夹，复制固件库里面的文件到工程文件夹。  

这一步是为了添加工程文件准备的。建文件夹是因为文件比较多，需要分类管理一下。  

这个文件夹的名称和数量没有限制，大家也可以根据自己的理解来建，这都是可以的。  

另外需要用的文件一定要复制到工程文件夹里面来，不要添加工程文件夹外面的文件，要不然你外面的文件一旦挪位置，工程里就找不到文件了。所以我们要复制文件到工程里来，保持工程的独立性。

### 第三步

第三步、工程里对应建立Start、Library、User等同名称的分组，然后将文件夹内的文件添加到工程分组里。

这一步的原因是在Keil里方便管理文件，因为Keil没法直接添加文件夹，所以还得重复一下。  

这个添加文件我是把所有的.h文件和.c文件都添加进来了。因为.h文件是不参与编译的，所以其他很多工程都是不添加.h文件的。  

但是我认为把.h文件加进来比较方便，而且.h文件也是需要经常打开看的，所以我比较习惯把所有的文件都添加进来。  

### 第四步

第四步、工程选项，C/C++，Include Paths内声明所有包含头文件的文件夹

这一步是因为你这个Start、Library等文件夹是你自己建的，Keil软件它并不知道。所以你要用自己文件夹里面的.h文件，就必须声明一下这个路径，那最好就是你自己建的所有文件夹都声明一下，这样就不会出现.h文件找不到的问题了。

### 第五步

第五步、工程选项，C/C++，Define内定义USE_STDPERIPH_DRIVER(标准外设驱动)，使用标准外设驱动这个字符串。

这是使用库函数的条件编译，使用库函数就必须定义这个。另外其他的工程在这个位置还声明了一个STM32F10X_MD的字符串。

那根据我的调查，Keil5在新建工程后自动就帮我们声明好了，这个不需要再额外申请声明了。所以在这个位置只需要声明使用USE_STDPERIPH_DRIVER(标准外设驱动)的字符串即可。

### 第六步

第六步、工程选项，Debug，下拉列表选择对应调试器，Settings，Flash Download里勾选Reset and Run。

这个就是选择调试器来进行下载的选项了。我们用STLINK就选择STLINK的那一项即可。

### 结束

这就是新建工程的基本步骤。新建工程也是很灵活的，大家只要符合要求，都可以编译通过。每个人建工程的风格也不同，大家学会了之后也可以建立一个属于自己风格的工程。

## 工程架构

最后我再额外的讲一下这个工程的架构，来看一下这个工程的每个文件都是干啥的，为啥需要这些文件？

图：工程架构



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-82.png" alt = "P4-新建工程-82.png" width = "70%" height = "auto">
</div>
<br>

### startup启动文件

我们看到这个图，首先是startup启动文件，这个是程序执行的最基本的文件。

图：Keil中启动文件 



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-83.png" alt = "P4-新建工程-83.png" width = "70%" height = "auto">
</div>
<br>

我们可以看到Keil中启动文件是用汇编写的，启动文件内定义了中断向量表、中断服务函数等。

这个中断服务函数中有个复位中断，这就是整个程序的入口，当STM32上电复位或者按下复位按键之后，程序就会进入复位中断函数执行。

复位中断函数主要就做了两件事情。第一个是调用SystemInit函数，第二个是调用main函数。

图：复位中断函数



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-90.png" alt = "P4-新建工程-90.png" width = "70%" height = "auto">
</div>
<br>

对应启动文件的这里。

图：startup_stm32f10x_md.s中 复位中断函数



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-84.png" alt = "P4-新建工程-84.png" width = "70%" height = "auto">
</div>
<br>

这里可以看到这是复位的中断函数，然后调用SystemInit，再调用main，然后程序就结束了。

当然实际上单片机的程序永远也不会结束，所以在main函数的最后一定是一个死循环。

图：main.c中 while(1)死循环



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-85.png" alt = "P4-新建工程-85.png" width = "70%" height = "auto">
</div>
<br>

SystemInit函数就是定义在这个system开头的.c文件里的。

那在Keil里我们也可以看到这个函数的定义。

这里的简介写了这个函数的作用，是设置微控制器的启动，初始化嵌入式闪存接口、锁相环、更新系统内核的时钟变量。

下面的note写的是这个函数仅在复位后需要调用。

那下面这些就是用来配置这些东西的，这个也不需要我们更改，我们只需要知道在main函数之前，单片机就已经执行了一堆东西了。

帮我们把这个闪存接口、时钟等一系列杂碎的东西都配置好了。

代码：system_stm32f10x.c中SystemInit函数定义

```c
/**
  * @brief  Setup the microcontroller system
  *         Initialize the Embedded Flash Interface, the PLL and update the 
  *         SystemCoreClock variable.
  * @note   This function should be used only after reset.
  * @param  None
  * @retval None
  */
void SystemInit (void)
{
  /* Reset the RCC clock configuration to the default reset state(for debug purpose) */
  /* Set HSION bit */
  RCC->CR |= (uint32_t)0x00000001;

  /* Reset SW, HPRE, PPRE1, PPRE2, ADCPRE and MCO bits */
#ifndef STM32F10X_CL
  RCC->CFGR &= (uint32_t)0xF8FF0000;
#else
  RCC->CFGR &= (uint32_t)0xF0FF0000;
#endif /* STM32F10X_CL */   
  
  /* Reset HSEON, CSSON and PLLON bits */
  RCC->CR &= (uint32_t)0xFEF6FFFF;

  /* Reset HSEBYP bit */
  RCC->CR &= (uint32_t)0xFFFBFFFF;

  /* Reset PLLSRC, PLLXTPRE, PLLMUL and USBPRE/OTGFSPRE bits */
  RCC->CFGR &= (uint32_t)0xFF80FFFF;

#ifdef STM32F10X_CL
  /* Reset PLL2ON and PLL3ON bits */
  RCC->CR &= (uint32_t)0xEBFFFFFF;

  /* Disable all interrupts and clear pending bits  */
  RCC->CIR = 0x00FF0000;

  /* Reset CFGR2 register */
  RCC->CFGR2 = 0x00000000;
#elif defined (STM32F10X_LD_VL) || defined (STM32F10X_MD_VL) || (defined STM32F10X_HD_VL)
  /* Disable all interrupts and clear pending bits  */
  RCC->CIR = 0x009F0000;

  /* Reset CFGR2 register */
  RCC->CFGR2 = 0x00000000;      
#else
  /* Disable all interrupts and clear pending bits  */
  RCC->CIR = 0x009F0000;
#endif /* STM32F10X_CL */
    
#if defined (STM32F10X_HD) || (defined STM32F10X_XL) || (defined STM32F10X_HD_VL)
  #ifdef DATA_IN_ExtSRAM
    SystemInit_ExtMemCtl(); 
  #endif /* DATA_IN_ExtSRAM */
#endif 

  /* Configure the System clock frequency, HCLK, PCLK2 and PCLK1 prescalers */
  /* Configure the Flash Latency cycles and enable prefetch buffer */
  SetSysClock();

#ifdef VECT_TAB_SRAM
  SCB->VTOR = SRAM_BASE | VECT_TAB_OFFSET; /* Vector Table Relocation in Internal SRAM. */
#else
  SCB->VTOR = FLASH_BASE | VECT_TAB_OFFSET; /* Vector Table Relocation in Internal FLASH. */
#endif 
}
```

另外在启动文件还定义了STM32所有的其他中断，这些中断达到触发条件后就会自动执行。那在启动文件(system_stm32f10x.c)这下面，这都是其他的中断调用了。

图：STM32所有的其他中断



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-89.png" alt = "P4-新建工程-89.png" width = "70%" height = "auto">
</div>
<br>

这个中转函数的定义，就是在stm32f10x_it.c里面。

图：中断函数的定义在stm32f10x_it.c里面



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-88.png" alt = "P4-新建工程-88.png" width = "70%" height = "auto">
</div>
<br>

我们打开Keil可以看到。这些就是中转函数的定义。

最后ST还建议我们把自己的中断写在这个位置上。当然我们还是习惯在哪用中断就写在哪里，写在别的地方也是可以的。

图：stm32f10x_it.c里 ST还建议我们把自己的中断写在这个位置上



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-91.png" alt = "P4-新建工程-91.png" width = "70%" height = "auto">
</div>
<br>

那这些就是中断部分的执行逻辑了。

另外你也可以自己定义一些用户文件，来封装一些模块供主函数和中断调用，这些也都是没问题的，同时也有利于我们程序结构的模块化，要不然所有的程序都堆在主函数里，那主函数也太长了，是吧？

### STM32的资源：外设和内核外设的寄存器描述+库函数文件

到此为止，这个工程结构主动执行的部分就介绍完了。剩下右边的就是被动执行的东西了，相当于STM32的资源。我们在主函数或者中断函数里就可以调用这些资源。

右上角这2个stm32f10x.h和core_cm3这些文件就是外设和内核外设的寄存器描述。

图：工程架构中stm32f10x.h和core_cm3这些文件



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-92.png" alt = "P4-新建工程-92.png" width = "70%" height = "auto">
</div>
<br>


在Keil中我们可以看到。这里面都是寄存器和寄存器每一位的名字，还有地址信息等，如果直接调用这些寄存器来使用STM32，那就是寄存器的开发方式。

我们已经试过了，这种方式会有一些弊端，也比较麻烦。

所以ST公司就提供了下面这两个文件，这个就是库函数的文件。

图：ST公司提供的库函数文件



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-93.png" alt = "P4-新建工程-93.png" width = "70%" height = "auto">
</div>
<br>

在Keil中可以看到这每个外设都提供了一大堆的函数，这些函数封装了寄存器的操作，给我们提供更加人性化的函数调用方式。只要学会了操作套路，那配置一个外设就是很简单的，连手册都不需要看一下。

这个conf文件就是用来配置头文件的包含关系的。

在Keil中我们可以看到在这里conf文件include了所有的库函数头文件。同时我们在stm32f10x.h的最后又包含了conf。

图：stm32f10x.h的最后又包含了conf



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-94.png" alt = "P4-新建工程-94.png" width = "70%" height = "auto">
</div>
<br>

所以在使用这些库函数时，我们只需要包含stm32f10x.h这一个头文件，就相当于包含了所有的库函数头文件。这样我们就可以任意的调用库函数了。

这些就是整个工程的结构和每个文件的作用。好，以上就是我们我们本节课的所有内容。本节课我们建好了基于库函数的STM32工程，我们下一节就开始从这个工程上学习STM32的第一个外设GPIO了。
## 临时解决一个小问题：头文件报错



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-72.png" alt = "P4-新建工程-72.png" width = "70%" height = "auto">
</div>
<br>

### 1.点击魔术棒

在Target栏目下，将ARM Compiler的级别选为如下图所示。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P4-%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B-74.png" alt = "P4-新建工程-74.png" width = "70%" height = "auto">
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





