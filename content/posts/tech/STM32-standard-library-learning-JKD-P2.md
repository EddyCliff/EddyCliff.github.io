---
title: "嵌入式开发-STM32标准库学习：STM32简介"
date: 2024-05-11T00:17:58+08:00
lastmod: 2024-05-11T00:17:58+08:00
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
categories: 
- 
tags: 
- STM32
- MCU
- ARM Cortex
- 开发板
- STM32简介
- STM32标准库
- 嵌入式开发
- STM32F1
- STM32F1最小系统电路
description: "STM32是基于ARM Cortex-M内核由ST公司开发的32位微控制器，拥有高性能和丰富片上资源，适合嵌入式系统应用。它包括多个系列，满足不同场景需求，例如高性能、主流、超低功耗和无线等。STM32F103C8T6是其中一款，采用ARM Cortex-M3内核，具有72MHz主频、20Kb RAM和64Kb Flash，适用于多种应用。该芯片拥有全面的外设资源，如NVIC、SysTick定时器、RCC、GPIO、AFIO、EXTI、TIM定时器、ADC、DMA、USART、I2C/SPI和USB OTG等，提供高度灵活且可定制化的平台。文章还介绍了该芯片的引脚定义、电源管理、启动配置和最小系统板构成，强调了正确配置电源、时钟和下载电路的重要性。通过连接稳压供电模块、复位电路、Boot配置电路、LED测试电路和下载电路，可以构建出能够稳定工作的STM32最小系统板。此外，文档推荐参考官方资料以深入理解STM32的工作原理和应用。"
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



## P2 STM32简介

### 1. STM32简介

1、STM32是ST公司基于ARM Cortex-M内核开发的32位微控制器

- ST：

	- 指ST Microelectronics（意法半导体公司）。

- M：

	- 就是微控制器 Minicontroller的首字母，微控制器就是MCU，也就是单片机。

- 32：

	- 表示计算机处理器位数，STM32是一款32位的单片机，先比较8位的51单片机，性能更强。

- ARM Cortex-M内核：

	- 这个也就是STM32内部的核心部分，这个内核是ARM公司设计的，它在STM32中占据极其重要的地位，比如我们程序指令的执行，加减乘除的运算，都是在内核里完成的。
	- 它相当于整个芯片的CPU，就像我们现在的电脑厂商一样，可以拿着Intel或者AMD的CPU，然后自己完善外围电路，就可以推出自己品牌的电脑。
	- STM32也就是ST公司拿着ARM公司设计的内核，再完善外围电路，整个封装起来，就做成了STM32。其他厂商，也可以拿着ARM公司设计的内核，来做他们自己的芯片，这些芯片，都叫做基于ARM内核的芯片。

2、STM32常应用在嵌入式领域，如智能车、无人机、机器人、无线通信、物联网、工业控制、娱乐电子产品等
- 智能车：
	- 可以用STM32做一个循迹小车，读取光电传感器或摄像头的数据，然后驱动电机前进或者转弯。
- 无人机：
	- 可以用STM32读取陀螺仪加速度计的姿态数据，然后控制算法去控制电机的速度，从而保证飞机稳定飞行。
-  机器人：
	- 我们可以用STM32驱动舵机，去控制机器人的关节，然后让机器人运动。
- 无线通信：
	- 我们可以给STM32连接上一些2.4G无线模块或者蓝牙，WIFI模块，这样STM32就具备无线通信的能力了。
- 物联网：
	- 借助无线模块来通信，比如蓝牙，WIFI，ZigBee这些，再通过STM32驱动继电器来控制220V电路的通断。
-  工业控制：
	- 比如有一些国产的PLC，它内部的主控就是一块STM32。PLC这个东西一般都是工厂用来进行工业控制。
- 娱乐电子产品：
	- 比如做一个爱心流水灯。

3、STM32功能强大、性能优异、片上资源丰富、功耗低，是一款经典的嵌入式微控制器

- 目前我们做一些需要单片机控制的东西，一般都可以考虑一下STM32。

### 2.STM32家族系列

下图为一系列的STM32 MCU - 32位的ARM Cortex-M内核的芯片

STM32主要有四个系列

- 高性能系列(High Performance)：STM32F2、F4、F7、H7

	- STM32F2：398 CoreMark 120 MHz主频的Cortex-M3 内核。
	- STM32H7：目前最强的STM32芯片，550MHz的Cortex-M7 内核和240MHz的Cortex-M4 内核。是一个双核控制器。
- 主流系列(Mainstream)：STM32F0、F1、F3
	- 本次我们使用的就是就是STM32F1系列，177 CoreMark 72 MHz主频的Cortex-M3 内核。

- 超低功耗系列(Ultra-low-power)

- 无线系列(Wireless)

💡小知识：

CoreMark：就是一个内核跑分，跑分越高，性能越好。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-25.png" alt = "P2-STM32简介-25.png" width = "70%" height = "auto">
</div>
<br>

### 3.ARM

刚才我们说了STM32使用ARM Cortex-M3的内核，那这个到底是怎样的内核呢🤔？ARM公司还有哪些内核呢🤔？

1、ARM既指ARM公司，也指ARM处理器内核

2、ARM公司是全球领先的半导体知识产权（IP）提供商，全世界超过95%的智能手机和平板电脑都采用ARM架构
- ARM公司只设计ARM内核，而不生产实物。实际的内核是各大半导体产商连同芯片一起制作出来的。
- ARM公司授权给各大厂商他的设计，然后再收取授权费作为盈利方式。

3、ARM公司设计ARM内核，半导体厂商完善内核周边电路并生产芯片

下图就是STM32的一个图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B.png" alt = "P2-STM32简介.png" width = "70%" height = "auto">
</div>
<br>

蓝色部分就是整个的STM32芯片，内部处于CPU地位的就是ARM公司设计的内核。

右边这些橙色部分就是ST公司设计的外围电路，比如存储器，还有一些片上的外设资源等。

如果ST还觉得资源不够多，还可以继续增加外围电路，这都是没问题的。

下图就是ARM内核的各种型号

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-26.png" alt = "P2-STM32简介-26.png" width = "70%" height = "auto">
</div>
<br>

经典系列(图中蓝色部分)：

比如ARM7、ARM9、ARM11，我们把这些型号称为经典的ARM处理器。

当然ARM内核也是从ARM1、ARM2等等这样发展过来的。

在ARM11之后，ARM可能觉得这样命名没意思了🤣。(为了迎合时代的发展和市场的变化)

ARM更改了命名方式，推出了Cortex系列的内核。并且一下子推出了三种子型号用于不同的场景。

它们分别是Cortex-A系列，Cortex-R系列，Cortex-M系列。

这三个系列加起来正好构成了 A R M 三个字母🤣。很有讲究。

根据应用场景来看，R系列和M系列适用于嵌入式领域。A系列适用于高端应用型领域。

A系列(图中橙色部分)：

A系列就是Application的意思，它包含这么多型号。

A系列主要用于手机领域，像现在苹果的手机芯片，高通的手机芯片，联发科的手机芯片，基本都是采用的ARM内核架构的。

A系列也是ARM内核中性能最高，发展最快的系列。

现在A系列推出的型号的非常多的，远不止图片上这几种型号，有兴趣的可以关注一下。

苹果的M1芯片就是基于ARM架构的，说起来最近推出的Ipad Pro新品好像搭载了M4芯片(题外话🤣)。

ARM架构推进到了电脑领域，可见ARM的发展趋势还是非常好的。

与A系列对比下来，R系列和M系列的型号发展就比较慢了。

R系列(图中绿色部分)：

R系列就是RealTime的意思，主要面向实时性很高的场景。比如硬盘控制器。

R系列的应用场景还是比较小的，R系列的内核型号也不是很多。

M系列(图中绿色部分)：

M系列就是Microcontroller的意思，主要应用在单片机领域，像我们的STM32，使用的就是M系列的内核。

它的型号也有M0、M1、M3、M4这些，不同型号的内核性能是不同的。

说到这里，基于ARM Cortex-M内核的STM32是什么意思相信大家就可以清楚了。

### 4.STM32F103C8T6

我们本次使用的是STM32F103C8T6这个型号的STM32。

- 系列：主流系列STM32F1
- 内核：ARM Cortex-M3
- 主频：72MHz
- RAM：20K（SRAM）
- ROM：64K（Flash）
- 供电：2.0~3.6V（标准3.3V）
- 封装：LQFP48

💡具体解释：

RAM：

RAM就是运行内存，实际的存储介质是SRAM。

ROM：

ROM就是程序存储器，实际的存储介质是Flash闪存。

供电：

这个STM32的供电是2.0~3.6V（标准3.3V），而我们以前使用的51单片机是5V供电，还有USB输出电压也是5V。

5V电压在这里是不能直接给STM32供电的，如果是5V电压，需要加一个稳压芯片，把电压降到3.3V，再给STM32供电。

封装：

它的封装是LQFP48，也就是图片中这种比较小的封装，总共有48个引脚。
如果你画板子的话，需要了解一下它的封装。


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-27.png" alt = "P2-STM32简介-27.png" width = "70%" height = "auto">
</div>
<br>

### 5.片上资源/外设

片上资源，又叫外设。英文是Peripheral。

下面这个表里就是STM32F1系列的外设资源。这就是以后我们学习的内容。

我们主要学习的就是STM32的外设。

通过程序配置外设，来完成我们想要的功能。

这个表里，前两个深颜色的，是位于Cortex-M3内核里面的外设，剩下的都是内核外的外设。


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-28.png" alt = "P2-STM32简介-28.png" width = "70%" height = "auto">
</div>
<br>

接下来，我们就一一地看一下这些外设都是干什么的。

我接下来介绍的这些东西大家有个大概印象就可以了，以后我们还会经常和这些东西打交道。

现在只是一个见面介绍，里面可能会存在很多专业名词。

- NVIC：嵌套向量中断控制器
	- 这个是内核里面用于管理中断的设备，比如配置中断优先级这些东西。
- SysTick：系统滴答定时器
	- 这个是内核里面的一个定时器，主要用来给操作系统提供定时服务。
	- 这个STM32是可以加入操作系统的，比如FreeRTOS，UCOS等。如果用了这些操作系统，就需要SysTick提供定时来进行任务切换的功能。
	- 在本次学习中，我们没有用到操作系统，我们可以用这个定时器来完成Delay函数的功能。
-  RCC：复位是时钟控制
	- 这个可以对系统的时钟进行配置，还有就是使能各模块的时钟。
	- 在STM32中，其他的外设在上电的情况下默认是没有时钟的。不给时钟的情况下，操作外设是无效的，外设也不会工作。这样的目的是降低功耗。
	- 所以，在操作外设之前，必须要先使能它的时钟。这就需要我们用RCC来完成时钟的使能。
- GPIO：通用IO口
	- 我们可以用GPIO来点灯，读取按键等。这也是单片机最基本的一个功能。
- AFIO：复用IO口
	- 它可以完成复用功能端口的重定义，还有中断端口的配置。
- EXTI：外部中断
	- 配置好外部中断后，当引脚有电平变化时，就可以触发中断，让CPU来处理任务。
- TIM：定时器
	- 是整个STM32最常用，功能最多的外设。
	- TIM分为高级定时器、通用定时器、基本定时器三种类型。
	- 其中高级定时器最复杂，常用的是通用定时器，这个定时器不仅可以完成中断的任务，还可以完成测频率、生成PWM波形、配置成专用的编码器接口等功能。像PWM波形，就是我们电机驱动、舵机驱动最基本的要求了。
- ADC：模数转换器
	- 这个STM32内置了12位的AD转换器，可以直接读取IO口的模拟电压值，无需外部连接AD芯片，使用非常方便。
- DMA：直接内存访问
	- 这个可以帮助CPU完成搬运大量数据这样的繁杂任务
- USART：同步/异步串口
	- 我们平常用的UART是异步串口的意思，这里的USART是既支持同步串口，又支持异步串口。当然我们实际中还是用异步串口比较多。
- I2C：I2C通信
- SPI：SPI通信
	- I2C和SPI是常用的两种通信协议，STM32也内置了它们的控制器，可以用硬件来输出时序波形。使用起来更加高效，当然用IO口来模拟时序波形也是没有问题的。
- CAN：CAN通信
	- CAN通信协议一般用于汽车领域
- USB：USB通信
	- 这个就太常见了，生活中常见的消费电子产品基本都是USB的。
	- 利用这个STM32的USB外设，可以做一个模拟鼠标、模拟U盘等设备。
- RTC：实时时钟
	- 在STM32内部完成年月日、时分秒的计时功能，而且可以接外部备用电池，即使掉电也能正常运行。
- CRC：CRC校验
	- 是一种数据的校验方式，用于判断数据的正确性。有了这个外设的支持，进行CRC校验就会更加方便一些。
- PWR：电源控制
	- 可以让芯片进入睡眠模式等状态，来达到省电的目的。
- BKP：备份寄存器
	- 这是一段存储器，当系统掉电时，仍可由备用电池保持数据，这个根据需要，可以完成一些特殊功能。
- IWDG：独立看门狗
- WWDG：窗口看门狗
	- 当单片机因为电磁干扰死机或者程序设计不合理出现死循环时，看门狗可以及时复位芯片，保证系统的稳定。
- DAC：数模转换器
	- 它可以在IO口直接输出模拟电压，是ADC模数转换的逆过程。
- SDIO：SD卡接口
	- 可以用来读取SD卡
- FSMC：可变静态存储控制器
	- 可以用来扩展内存，或者配置成其他总线协议，用于某些硬件的操作。
- USB OTG：USB主机接口
	- 用OTG功能，可以让STM32作为主机去读取其他USB设备。

以上就是STM32F1系列所有外设的大致介绍了。
在我们之后的学习中，经常会和这些字母缩写打交道。所以大家先熟悉一下这些字母的意思。

至于具体的功能，我们之后会慢慢介绍。

📢注意：

这是STM32F1整个系列的所有外设，并不是所有型号都拥有全部的外设。
比如我们这款C8T6芯片，就没有后面这四个外设。

🤔具体有哪些外设？每个外设有几个呢？

这个时候我们就需要查询数据手册。（手册为《STM32F103x8B数据手册（中文）》）


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-29.png" alt = "P2-STM32简介-29.png" width = "70%" height = "auto">
</div>
<br>

由数据手册我们可以得知

我们使用的STM32F103C8T6 对应的就是图中的STM32F103Cx

64K的闪存、20K的SRAM

3个通用定时器、1个高级定时器、没有基本定时器

2个SPI、2个I2C、3个USART、1个USB、1个CAN总线

37个IO口、2个10通道的12位ADC

在这个表里没有出现的外设，你就要确认一下它是否存在，要是操作到了不存在的外设，它是不会工作的。

### 6.命名规则

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-30.png" alt = "P2-STM32简介-30.png" width = "70%" height = "auto">
</div>
<br>


我们来看一下芯片的命名规则，看一下这个型号的每一位字母和数字代表的意义。

以后我们看到其他型号的STM32，也都可以对照这个命名规则了解它的大致参数。

### 7.芯片系统结构

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-31.png" alt = "P2-STM32简介-31.png" width = "70%" height = "auto">
</div>
<br>


我们来看一下这个芯片的系统结构，这个结构看起来比较复杂，目前不需要每个部分都弄懂，大致的了解一下即可。

了解这个系统结构有利于你加深对STM32的认识，便于今后的学习。

我们可以把这个图分为四个部分。

第一部分：内核  

左上角的就是Cortex-M3的内核，内核引出来了三条总线，分别是ICode指令总线，DCode数据总线，System系统总线。

ICode指令总线：  
这个ICode总线和DCode总线主要是用来连接Flash内存的，Flash里面存储的就是我们编写的程序。

ICode总线就是用来加载程序指令的。

DCode数据总线：  
DCode总线是用来加载数据的，比如常量和调试数据这些。

System系统总线：   
System系统总线就连接到了其他东西上面，比如SRAM，用于存储程序运行时的变量数据。还有FSMC，这个本次的芯片中不会用到。

第二部分：Flash

第三部分：外设种类和分布

AHB系统总线：  
下面这个AHB系统总线就是用来挂载主要的外设的。

AHB的意思是先进高性能总线，挂载的一般是最基本的或者性能比较高的外设。比如复位和时钟控制这些最基本的电路，还有SDIO，也是挂载在AHB上的。

再后来就是两个桥接，接到了APB2和APB1两个外设总线上。

APB：  
这个APB的意思就是先进外设总线，用于连接一般的外设。

因为AHB和APB的总线协议、总线速度、还有数据传送格式的差异，所以中间需要两个桥接，来完成数据的转换和缓存。

AHB的整体性能比APB高一些，其中APB2的性能又比APB1高一些。

APB2：  
APB2一般是和AHB同频率，都是72MHz，APB1一般是36MHz。

所以APB2一般连接的是一般外设中稍微重要的部分。比如GPIO端口还有一些外设的1号选手等，比如USART1、SPI1、TIM1、TIM8。这个TIM8和TIM1一样，也是高级定时器，所以也是重要的外设。

还有ADC、EXTI、AFIO也是接在APB2上。

APB1：  
其他的像这些2、3、4、5号外设还有DAC、PWR、BKP等，这些是次要一点的外设，都会分配到APB1上。

当然在使用的时候，个人一般感觉不到APB2和APB1的性能差异。只需要知道这个外设是挂载到哪个总线上的就可以了。

第四部分：DMA

这个DMA可以把它当做内核CPU的小秘书。比如有一些大量的数据的搬运的活，让CPU来干的就太浪费时间了。

比如我有个外设ADC模式转换，这个模式转换可以配置成连续模式，比如一毫秒转换一次，转换完的数据必须得转运出来，否则数据就会被覆盖丢失。如果直接让CPU来干这个活，那CPU每过一毫秒就得来转运一下数据，这样会费时费力，影响CPU的正常工作。而且这个活就是简单的数据搬运，对吧？也没必要CPU来干这活。于是DMA这个小秘书就出现了，它主要就是干这些像数据搬运这样简单且反复要干的事情。

DMA通过DMA总线连接到总线矩阵上，它可以拥有和CPU一样的总线控制权，用于访问这些外设小弟。当需要DMA搬运数据时，外设小弟就会通过请求线发送DMA请求，然后DMA就会获得总线控制权，访问并转运数据。整个过程不需要CPU的参与，省下来CPU的时间来干其他的事情，这就是DM的用途。

到这里我们整个系统结构图就已经大概清楚了。

### 8.引脚定义

接着我们再来看一下这个芯片的引脚定义。引脚定义对于我们使用这个芯片而言还是非常重要的。一般我们拿到一个新的芯片时，需要着重地看一下它的引脚定义。有的时候这个芯片的引脚定义看完了，我们就可以大概知道这个芯片是怎么使用的了。

下图就是我给大家做的STM32F103C8T6的引脚定义表。以后我们开发STM32会经常用到这个表。大家可以把这个图片存下来，方便以后查看。


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-32.png" alt = "P2-STM32简介-32.png" width = "70%" height = "auto">
</div>
<br>

我们来看一下这个芯片的银角都是干啥用的。这个就是C8T6这个芯片的引脚序号和引脚名称的示意图。

在左上角有个小黑点，代表它左边的引脚是一号引脚，然后逆时针依次排列直到48号引脚。

上面这个表就是每个引脚的名称和功能，在这里我都做了颜色标记。标红色的是电源相关的引脚，标蓝色的是最小系统相关的引脚，标绿色的是IO口、功能口这些引脚。

我们来依次看一下。
#### 表头

首先看一下表头。  
引脚号和引脚名称：  
前两列是引脚号和引脚名称，和上面的芯片引脚一一对应。

**类型：**  
接着是类型，S代表电源、I代表输入，O代表输出、IO代表输入输出。

**I/O口电平：**  
然后I/O口电平代表I/O口所能容忍的电压。这里有FT的，代表它能容忍5V的电压，没有FT的就只能容忍3.3V的电压。如果没有FT的需要接5V的电平，就需要加装电平转换电路了。

**主功能：**
主功能就是上电后默认的功能，一般和引脚名称相同。如果不同的话，引脚的实际功能是主功能，而不是引脚名称的功能。

**默认复用功能：**
默认复用功能就是I/O口上同时连接的外设功能引脚，这个配置I/O口的时候可以选择是通用I/O口还是复用功能。

**重定义功能：**  
最后一个是重定义功能。这个的作用是，如果有两个功能同时复用在了一个I/O口上，而你确实需要用到这两个功能，那你可以把其中一个复用功能重映射到其他端口上。当然前提是这个重定义功能的表里有对应的端口。

#### 引脚定义

接着我们来依次看一下引角定义。  
**引脚1：VBAT**  
第一个引脚是VBAT，它是备用电池供电的引脚。在这个引脚可以接一个3V的电池，当系统电源断电时，备用电池可以给内部的RTC时钟和备份寄存器提供电源。

**引脚2：PC13-TAMPER-RTC**  
二号引脚是I/O口或者侵入检测或者RTC，I/O口可以根据程序输出或读取高低电平，是最基本也是最常用的功能了。侵入检测可以用来做安全保障的功能。比如你的产品安全性比较高，可以在外壳加一些防拆的触点，然后接上电路到这个引脚上。若有人强行拆开设备，那触点断开，这个引脚的电平变化就会触发STM32的侵入信号，然后就会清空数据来保证安全。RTC的引脚可以用来输出RTC校准时钟，RTC闹钟脉冲或者秒脉冲。

**引脚3：PC14-OSC32_IN**  
**引脚4：PC14-OSC32_OUT**  
3、4号引脚是I/O口或者接32.768KHz的RTC晶振。

**引脚5：OSC_IN**  
**引脚6：OSC_OUT**
5、6号引脚接系统的主晶振，一般是8MHz的。然后芯片内有锁相环电路，可以对这个8MHz的频率进行倍频，最终产生72MHz的频率，作为系统的主时钟。

**引脚7：NRST**  
7号NRST是系统复位引脚，N代表它是低音平复位的。

**引脚8：VSSA**  
**引脚9：VDDA**  
8、9号引脚是内部模拟部分的电源，比如ADC、RC震荡器等。VSS是负极，接GND，VDD是正极，接3.3V。

**引脚10-引脚19：I/O口**  
接着10号引脚到19号引脚都是I/O口，其中PA0还兼具了WKUP的功能，这个可以用于唤醒处于待机模式的STM32。
20号引脚是I/O口或者BOOT1引脚，BOOT引脚是用来配置启动模式的，这个我们等会儿再讲。另外这个I/O口的引脚名称我没有加粗，这个意思是我推荐优先使用这些加粗的I/O口。没有加粗的I/O口可能需要进行配置，或者兼具其他功能，使用的时候需要留意一下。

**引脚21-引脚22：I/O口**  
接着21号、22号也都是I/O口。

**引脚23-引脚24：**  
23、24号的VSS_1和VDD_1是系统的主电源口。同样的VSS是负极，VDD是正极。

**引脚35：VSS_2**  
**引脚36：VDD_2**  
**引脚47：VSS_3**  
**引脚48：VSS_3**  
另外下面还有VSS_2、VDD_2、VSS_3、VDD_3都是系统的主电源口，这里STM32内部采用了分区供电的方式，所以供电口会比较多。在使用时把VSS都接GND，VDD都接3.3V即可。

**引脚25-引脚33：I/O口**  
接着25到33号都是I/O口。

**引脚34，引脚37-40：调试端口**  
34号加上37号到40号，这些是I/O口或者调试端口。上面默认的主功能是调试端口，调试端口就是用来调试程序下载程序的。  
这个STM32支持SWD和JTAG两种调试方式。SWD需要两根线，分别是SWDIO和SWCLKG。  
JTAG需要五根线，分别是JTMS、JTCK、JTDI、JTDO、NJTRST。
我们教程使用的是STLINK来下载调试程序的，STLINK用的是SWD的方式。所以只需要占用PA13、PA14这两个I/O口。  
在使用SWD的调试方式时，剩下的PA15、PB3、PB4可以切换为普通的I/O口来使用，但要在程序中进行配置，不配置的话默认是不会用作I/O口的。

**引脚41-引脚43，引脚45-引脚46：I/O口**  
下面的41号到43号，45到46号都是I/O口。

**引脚44：BOOT0**  
最后剩下的44号BOOT0，和刚才介绍的BOOT1一样，也是用来做启动配置的。

那我这个表也是复制数据手册里的，在数据手册里也有引脚定义的表。

图：《STM32F103x8B数据手册（中文）》-P20页


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-33.png" alt = "P2-STM32简介-33.png" width = "70%" height = "auto">
</div>
<br>

这个表把所有封装的引脚定义都放在一起了，看起来不太方便。所以我就整理了这个表，可以方便大家观看。

### 9.启动配置

那有关I/O口的部分到这里就介绍完了，然后我们来看一下STM32的启动配置，也就是刚才我们看到的BOOT0和BOOT1两根引脚的功能。  


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-34.png" alt = "P2-STM32简介-34.png" width = "70%" height = "auto">
</div>
<br>

这个启动配置的作用就是指定程序开始运行的位置。一般情况下，程序都是在Flash程序存储器开始执行。  
但是在某些情况下，我们也可以让程序在别的地方开始执行，用以完成特殊的功能。  
在这里我们看一下，在STM32F10xxx里，可以通过配置BOOT0和BOOT1引脚，来选择三种不同的启动模式。

**1、BOOT1接X，BOOT为0：正常的执行Flash闪存里面的程序**  
当BOOT0引脚为0，就是接地的意思，这个时候BOOT1接X，就是无论接什么，启动模式都是主闪存存储器的模式。这时候主闪存存储器被选为启动区域，也就是正常的执行Flash闪存里面的程序，这种模式是最常用的模式，一般情况下都是这个配置。

**2、BOOT1接0，BOOT为1：串口下载**  
那当BOOT1接0，BOOT接1，接1就是接到3.3V电源正的意思。  
那启动模式就是系统存储器，说明是系统存储器被选为启动区域，这个解释不太好理解。其实这个模式就是用来做串口下载用的，这个系统存储器存的就是STM32中一段BootLoader程序。BootLoader程序的作用就是是接收串口的数据，然后刷新到主闪存中，这样就可以使用串口下载程序了。一般我们需要串口下载程序的时候会配置到这个模式上。

🤔那什么时候我们需要用到串口下载呢？  
我们可以看到这个引脚定义表。刚才我们说了引脚34，引脚37-40这五个是调试端口，他们既可以用来下载程序，也可以作为普通I/O口使用。  
如果我们在程序中把这五个端口全部配置成了I/O口，那这就坏了。因为这个芯片没有调试端口了，也就下载不了程序了。所以在你配置这几个端口的时候要小心一点，不要把它们全部都变成普通I/O口了。

那如果全部变成I/O口了，下载不进去程序了，这就需要用到串口的方式下载程序。如果想使用串口下载，就需要配置BOOT1为0，BOOT0为1。当然串口下载也不光是用来救急的。如果你没有STLINK，也没有JLINK，那就可以使用串口来进行下载程序，这样就多了一种下载程序的方式。

那有关串口下载是如何操作的，我之后会再讲。

**2、BOOT1接1，BOOT为1：**  
然后就是BOOT1接1，BOOT0接1的情况。这时配置的是内置SRAM启动，这个模式主要是用来进行程序调试的，现阶段用的比较少，我们本次学习中也不会用的。

最后，下面还有一段话，是在系统复位后，SYSCLK的第四个上传沿BOOT引脚的值将被锁定。用户可以通过设置BOOT1和BOOT0引脚的状态来选择复位后的启动模式。这个意思是BOOT引脚的值是在上电复位后的一瞬间有效，之后就随便了。

在这个引脚定义中，我们可以看到这个BOOT1和PB2是在同一个引脚上的。也就是在上电的瞬间，是BOOT1的功能，当第四个时钟过之后，就是PB2的功能。

那这些就是有关BOOT引脚的部分。

看完了引脚定义和这个BOOT的配置，我们对这个芯片是怎么用的，大概就有思路了。在这个表里，如果我们想让STM32正常工作，那么首先就需要把电源部分和最小系统部分的电路连接，也就是这个表中标红色和蓝色的部分。我刚才已经把这些引脚的功能和作用都介绍了，那我们接下来就可以看一下STM32最小系统电路了。

### 10.最小系统电路


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-35.png" alt = "P2-STM32简介-35.png" width = "70%" height = "auto">
</div>
<br>

一般来说，我们单片机只有一个芯片是无法工作的，我们需要为它连接最基本的电路，这些最基本的电路就叫做最小系统电路。  

#### STM32及供电

我们来看一下这个电路，右边这一部分就是STM32及供电的部分。其中我们可以看到这三个分区供电的主电源和模拟部分电源都连接了供电引脚。


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-36.png" alt = "P2-STM32简介-36.png" width = "70%" height = "auto">
</div>
<br>



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-37.png" alt = "P2-STM32简介-37.png" width = "70%" height = "auto">
</div>
<br>


VSS都连接了GND，VDD都连接了3V3，也就是3.3V。在这个3.3V和GND之间，一般会连接一个滤波电容，这个电容可以保证供电电压的稳定。像我们在设计电路的时候，一般只要遇到供电，都会习惯上的加上几个滤波电容。当然加这个滤波电容也是非常必要的。大家自己设计电路的时候可以注意一下。剩下还有一个VBAT是接备用电池的，如果需要接备用电池，那就这样来接。

可以选择一个3V的纽扣电池，正极接VBAT，负极接GND就行了。备用电池是给RTC和备份寄存器服务的，如果不需要这些功能就不用接备用电池，那这个VBAT直接接3.3V即可，或者悬空也是没有问题的。  

这就是整个供电的部分，可以说STM32的供电还是比较多的，而且芯片四周都有供电引脚。这个要是自己画板子的话就会深有体会。这个供电实在是有点多，走线还是很头疼的。但是虽然头疼，还是要把这些供电都接好啊。

#### 晶振



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-38.png" alt = "P2-STM32简介-38.png" width = "70%" height = "auto">
</div>
<br>

接着我们再来看一下晶振部分电路，这个就是一个典型的晶振电路。在这里接了一个8MHz的主时钟晶振，STM32的主晶振一般都是8MHz。8MHz经过内部锁相环倍频得到72MHz的主频。

这个晶振的两根引脚分别通过这两个网络编号连接到STM32的5、6号引脚。另外还需要接两个20pF的电容，作为启震电容，电容的另一端接地即可。那这就是晶振电路。

如果你需要RTC功能的话，还需要再接一个32.768KHz的晶振，电路和这个一样，接在3、4号引脚。这个OSC32就是32.768KHz的晶振的意思，为什么要用32.768KHz的呢？因为32768是2的15次方，内部RTC电路经过2的15次方倍频，就可以生成1秒的时间信号了。  
我们知道在计算机世界，二的次方数是一个很神奇的数字，有很多数字都和2的次方有关。

#### 复位电路



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-39.png" alt = "P2-STM32简介-39.png" width = "70%" height = "auto">
</div>
<br>

接着我们再来看一下复位电路，这个复位电路是一个10k的电阻和0.1uF(微法)的电容组成的，它用来给单片机提供复位信号。这中间的NRST接在STM32的7号引脚，NRST是低电平复位的。

当这个复位电路在上电的瞬间，电容是没有电的。电源通过电阻开始向电容充电，并且此时电容呈现的是短路状态，那NRST引脚就会产生低电平。当电容逐渐充满电时，电容就相当于断路，此时NRST就会被R1上拉为高电平。

那上电瞬间的波形就是先低电平，然后逐渐高电平。这个低电平就可以提供STM32的上电复位信号。当然电容充电还是非常快的，所以在我们看来，单片机就在上电的一瞬间复位了，这就是复位电路的作用。

电容左边还并联了一个按键，这个可以提供一个手动复位的功能。当我们按下按键时，电容被放电，并且NRST引脚也通过按键被直接接地了。

这就相当于手动产生了低电平复位信号。按键松手后，NRST又回归高电平，此时单片机就从复位状态转为工作状态。平时我们也可以见到这种复位按键，一般在设备上有个小孔，当设备死机并且还不方便断电重启时，我们就可以拿一个针戳一下这个小孔里的按键，这样就会使设备复位了。这就是手动复位的功能。按下按键，程序就从头开始运行的意思。

#### 启动配置

下面是这个启动配置，这个H1相当于开关的作用。拨动这个开关，就可以让BOOT引角选择3.3V，还是GND了。在我们这个最小系统板上使用的是这种跳线帽来充当开关的功能。

当跳线帽插在左边2个脚时，就相当于接地。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-40.png" alt = "P2-STM32简介-40.png" width = "70%" height = "auto">
</div>
<br>

插在右边2个脚就相当于接3.3V，这样就可以配置BOOT的高低电平了。  



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-41.png" alt = "P2-STM32简介-41.png" width = "70%" height = "auto">
</div>
<br>

当然你要自己设计电路的话，接一个拨码开关也是没问题的，这样比插跳线帽还方便一些。

#### 下载端口

最后剩下的就是下载端口了。如果你是用STLINK下载程序的话，那需要把SWDIO和SWCLK这两个引脚引出来，方便接线。另外再把3.3V和GND引出来，这个GND是必须引出来的。3.3V如果你板子自己有供电的话，可以不引，不过建议还是都引出来，这样方便一些。

这些就是有关最小系统电路的内容。如果你自己画板子的话，可以参考一下这个电路。当然如果你是用STM32最小系统板来设计电路的话，就不需要这个最小系统了。因为这个最小系统板上已经包含了这些电路了，那我们再看一下我提供的资料文件夹，在这个 模块资料 第一个文件夹里面的这个核心版原理图，就是我们这个最小系统版的原理图。

图：江科大 STM32/模块资料/1-STM32F103C8T6核心板/STM32F103C8T6核心板原理图



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-42.png" alt = "P2-STM32简介-42.png" width = "70%" height = "auto">
</div>
<br>

我们来看一下这个最小系统和我给的那个最小系统有哪些不同呢？可以看到他这个最小系统的东西比我给的那个要多一些。

我们来依次看一下。

图中第一行第一个：  
第一个是复位电路和我给的是一致的。

图中第一行第二个：  
第二个是BOOT配置电路，也是一样的。

图中第一行第三个：   
第三个它在这里接了两个测试的LED，一个直接接到了VCC3V3和GND，这个是电源测试指示灯，另一个接到了PC13，这是一个I/O口的测试灯。

图中第一行第四个：  
接着第四个的下载电路也是一样的，它在这个供电也加了一个电源滤波，这个作用也是稳定供电的。

图中第二行第一个：  
然后这个是一个稳压芯片，用于给5V的电降到3.3V，给STM32供电。它左边的输入是这个USB的5V电源，右边输出的是3.3V。这个芯片没有给型号。我根据它的外形和引脚推测，这个芯片的型号应该是XC6204。XC6204是一个3.3V的稳压芯片，另外还有XC6206、AMS1117等，这些都是常用的稳压芯片。大家设计电路的时候可以参考一下。

图中第二行第二个：



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-43.png" alt = "P2-STM32简介-43.png" width = "70%" height = "auto">
</div>
<br>

这两排就是引脚的排针了，这两个排针把芯片的引脚都引出来，方便我们接线的。

图中第二行第三个：



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-44.png" alt = "P2-STM32简介-44.png" width = "70%" height = "auto">
</div>
<br>

中间这个大的是STF32F103C8T6的芯片。

图中第二行第四个，图中第三行第三个：



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-45.png" alt = "P2-STM32简介-45.png" width = "70%" height = "auto">
</div>
<br>

右上角和左下角是四个STM32供电的滤波电容。

图中第三行第一个：  

![[P2-STM32简介-46.png]]

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-46.png" alt = "P2-STM32简介-46.png" width = "70%" height = "auto">
</div>
<br>

左边这个是USB的接口，它接的PA11和PA12是STM32的USB引脚。可以进行USB通信，另外这个USB还可以提供5V的供电。这个电经过我们刚才介绍的这个稳压芯片降到3.3V，剩下的这些电路都是3.3V的供电。

剩下的这些电路都是3.3V的供电。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-47.png" alt = "P2-STM32简介-47.png" width = "70%" height = "auto">
</div>
<br>


最后就是这两个晶振电路了，这个金正电路和我给的基本是一样的，上面这一部分是32.768KHz的，晶振接到了STM32的PC14和PC15。PC14和PC15这两引脚就是OSC32的那两个引脚，这里名字有点不一样，但是是一个地方。下面这一部分是8MHz的主时钟晶振，接的是STM32的OSC这两个引脚OSCIN和OSCOUT。在这里多了一个1M的电阻(R6)，这个电阻也是有一些作用的，不过也可以不接，影响不大。大家感兴趣的话可以了解一下。

### 10.实物图

#### 正面图



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-48.png" alt = "P2-STM32简介-48.png" width = "70%" height = "auto">
</div>
<br>

最后我们再看一下这个最小系统版的实物图，大概的认识一下板子上的元件。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-4.png" alt = "P2-STM32简介-4.png" width = "70%" height = "auto">
</div>
<br>

STM32F103C8T6：  
这个中间的黑色小芯片就是STM32F103C8T6。

两个跳线帽：  
左边这两个跳线帽，是用来配置BOOT引脚的。

复位按键：  
下面这个是复位按键。

USB接口：  
最左边是这个USB接口，它可以进行USB通信，也可以为板子供电。

晶振：  
右边这个金属外壳的是8MHz的主时钟晶振，这个黑色的是32.768KHz的RTC晶振。

LED：  
然后在右边是2个LED，上面这个是PWR电源指示灯，下面这个是接在PC13口的测试灯。

调试接口：  
最右边是SWD的调试接口，用来下载程序的。

排针：  
上下两排是用于接线的排针。

#### 背面图



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B%201.png" alt = "P2-STM32简介 1.png" width = "70%" height = "auto">
</div>
<br>

然后看一下背面。

这个五个脚的小芯片，就是3.3V稳压芯片，剩下的这些就是电容电阻这些小元件了。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-5.png" alt = "P2-STM32简介-5.png" width = "70%" height = "auto">
</div>
<br>

介绍到这里，大家对这个最小系统版应该都了解了。有关STM32的介绍到这里就结束了。本节我们把STM32的基本情况，ARM内核、参数、外设、命名规则、系统结构、引脚定义、启动配置、最小系统都介绍了。这些就是有关于STM32最基本的介绍。如果想要继续深入了解STM32，可以看一下我提供资料里的这些文档，这些文档是官方提供的最详细的介绍了。想要学好STM32，看好这些文档也是必须的。那本节的STM32介绍就到这里，我们下节再见。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P2-STM32%E7%AE%80%E4%BB%8B-6.png" alt = "P2-STM32简介-6.png" width = "70%" height = "auto">
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









































