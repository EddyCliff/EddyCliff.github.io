---
title: "嵌入式开发-STM32标准库学习：GPIO输出"
date: 2024-05-13T00:17:58+08:00
lastmod: 2024-05-13T00:17:58+08:00
author: ["Eddy"]
keywords: 
- STM32
- embedded
- Embedded development
- MCU
- STM32简介
- STM32标准库
- 嵌入式开发
- GPIO
- LED
- LED闪烁
- LED流水灯
- 蜂鸣器
- ARM
categories: 
- 
tags: 
- STM32
- embedded
- Embedded development
- MCU
- GPIO
- LED
- buzzer
- ARM
description: "本教程涵盖了STM32 GPIO的输出与输入功能，分为基础应用和理论知识两个部分。首先，通过LED闪烁、LED流水灯和蜂鸣器实验，介绍GPIO基本应用。随后，深入探讨了GPIO的理论知识，包括其作为通用I/O口的特性、工作模式及电平范围。进一步地，解析了STM32中GPIO的基本结构，涉及APB2外设总线、GPIO模块及其组成。此外，讨论了GPIO引脚的功能，包括输入和输出保护措施，以及施密特触发器的作用和应用。教程强调了STM32 GPIO的多样工作模式，如推挽输出、开漏输出等，及其在不同应用场景下的优势。最后，简述了面包板的应用，为读者提供了实际电路搭建的指导。"
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

## 引言

本节课我们来一起学习STM32的GPIO。本节课将会分为两个部分，总共四个小节。

第一部分我们主要学习GPIO的输出，第二部分我们主要学习GPIO的输入，另外在第二部分我也会给大家讲一些C语言的知识。因为这个库函数里面用了大量的结构体、指针、枚举等知识，这些知识应该都算是C语言中比较高级的部分了，初学者可能比较难理解，那我们会在这一节第二部分介绍。前半部分大家先跟着我来操作就行了。

好，我们先来看一下本节课程序的现象。本节课第一部分总共有三个程序，第一个是LED闪烁，第二个是LED流水灯，第三个是蜂鸣器。我们先来看一下第一个程序，编译，下载。

下载之后我们可以看到在这个面包板上有一个LED正在不断的闪烁，这就是第一个程序的现象。

图：程序一现象 LED闪烁

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA.png" alt = "P5-GPIO输出.png" width = "70%" height = "auto">
</div>
<br>



接下来再看一下第二个程序，LED流水灯。编译，下载。这时可以看到这里的8个LED正在以流水灯的方式不断循环点亮，这就是第二个程序的现象。

图：程序二现象 LED流水灯

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-1.png" alt = "P5-GPIO输出-1.png" width = "70%" height = "auto">
</div>
<br>

这接下来看一下第三个程序，蜂鸣器，编译，下载。这时候我们可以听到这个蜂鸣器正在滴滴鸣响，这就是第三个程序的现象。

图：程序三现象 蜂鸣器正在滴滴鸣响

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-2.png" alt = "P5-GPIO输出-2.png" width = "70%" height = "auto">
</div>
<br>

我们回到PPT，先来看一下GPIO的理论部分。

## GPIO简介

- GPIO（General Purpose Input Output）通用输入输出口
- 可配置为8种输入输出模式
- 引脚电平：0V~3.3V，部分引脚可容忍5V
- 输出模式下可控制端口输出高低电平，用以驱动LED、控制蜂鸣器、模拟通信协议输出时序等
- 输入模式下可读取端口的高低电平或电压，用于读取按键输入、外接模块电平信号输入、ADC电压采集、模拟通信协议接收数据等

**1、GPIO（General Purpose Input Output）通用输入输出口**

首先GPIO这四个字母是General Purpose Input Output的缩写，意为通用输入输出口，也就是我们俗称的IO口。

**2、可配置为8种输入输出模式**

这个GPIO根据使用场景可以配置为8种输入输出模式，具体是哪8种模式，我们等会儿再细说。

**3、引脚电平：0V~3.3V，部分引脚可容忍5V**

GPIO的引脚电平是0V~3.3V，数据0就是低电平，也就是0V，数据1就是高电平，也就是3.3V。

部分引脚可容忍5V，容忍5V的意思是可以在这个端口输入5V的电压，也认为是高电平。

但是对于输出而言，最大就只能输出3.3V，因为供电就只有3.3V。具体哪些端口能容忍5V，可以参考一下STM32的引脚定义。这里带FT(Five Tolerate)的就是可以容忍5V的，不带FT的就只能接入3.3V的电压，这个我们在第一节也讲过。

图：STM32的引脚定义



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-3.png" alt = "P5-GPIO输出-3.png" width = "70%" height = "auto">
</div>
<br>

**4、输出模式下可控制端口输出高低电平，用以驱动LED、控制蜂鸣器、模拟通信协议输出时序等**

GPIO在输出模式下可控制端口输出高低电平，用以驱动LED、控制蜂鸣器，模拟通用协议输出时序等，像我们刚才演示的LED和蜂鸣器的程序现象，就使用到了GPIO的输出模式。

另外在其他的应用场景，只要是可以用高低电平来进行控制的地方，都可以用GPIO来完成。

如果控制的是功率比较大的设备，只需要再加入驱动电路即可。

除此之外我们还可以用GPIO来模拟通信协议，比如I2C、SPI或者某个芯片特定的协议，我们都可以用GPIO的输出模式来模拟其中的输出时序部分。

**5、输入模式下可读取端口的高低电平或电压，用于读取按键输入、外接模块电平信号输入、ADC电压采集、模拟通信协议接收数据等**

GPIO在输入模式下可读取端口的高低电平或电压，用于读取按键输入、外接模块电平信号输入、ADC电压采集、模拟通信协议接收数据等。

这个输入模式最常见的就是读取按键了，用来捕获我们的按键按下事件。

另外也可以读取带有数字输出的一些模块，比如我们套件里的光敏电阻模块、热敏电阻模块等。如果这个模块输出的是模拟量，那GPIO还可以配置成模拟输入的模式，再配合内部的ADC外设，就能直接读取端口的模拟电压了。

除此之外，模拟通信协议时，接收通信线上的数据，也是靠GPIO的输入来完成的。

## GPIO基本结构

那看完介绍，我们再来看一下STM32中GPIO的基本结构。下面这有一个框图，这就是GPIO的整体构造。

图：GPIO基本结构

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-4.png" alt = "P5-GPIO输出-4.png" width = "70%" height = "auto">
</div>
<br>

其中左边的是APB2外设总线，也就是这个STM32系统结构图的这个位置。

在STM32中，所有的GPIO都是挂载在APB2外设总线上的。

图：STM32系统结构中所有的GPIO都是挂载在APB2外设总线上的

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-5.png" alt = "P5-GPIO输出-5.png" width = "70%" height = "auto">
</div>
<br>

其中GPIO外设的名称是按照GPIOA、GPIOB、GPIOC等等这样来命名的。

每个GPIO外设总共有16个引脚，编号是从0到15。

GPIOA的第0号引角，我们一般把它称作PA0，接着第1号就是PA1，然后以此类推，一直到PA15。GPIOB也是一样，从PB0一直到PB15这样来命名的。

在每个GPIO模块内，主要包含了寄存器和驱动器这些东西。

寄存器就是一段特殊的的存储器，内核可以通过APB2总线对寄存器进行读写，这样就可以完成输出电平和读取电平的功能。

这个寄存器的每一位对应一个引脚，其中输出寄存器写1，对应的引脚就会输出高电平，写0，就输出低电平。

输入寄存器读取为1，就证明对应的端口目前是高电平，读取为0，就是低电平。

因为STM32是32位的单片机，所以STM32内部的寄存器都是32位的。但这个端口只有16位，所以这个寄存器只有低16位对应的有端口，高16位是没有用到的。

图：STM32内部的寄存器都是32位的。但这个端口只有16位，所以这个寄存器只有低16位对应的有端口，高16位是没有用到的。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-6.png" alt = "P5-GPIO输出-6.png" width = "70%" height = "auto">
</div>
<br>

这个驱动器是用来增加信号的驱动能力的。寄存器只负责存储数据。如果要进行点灯这样的操作的话，还是需要驱动器来负责增大驱动能力。那这些就是GPL的的整体基本结构了。

## GPIO位结构

接下来我们再来看一下这个GPIO中每一位的具体电路结构，这个图就是STM32参考手册中的GPIO位结构的电路图了。

图：GPIO位结构

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-7.png" alt = "P5-GPIO输出-7.png" width = "70%" height = "auto">
</div>
<br>

其中左边这三个就是寄存器，中间这一部分是驱动器，右边这个就是某一个IO口的引脚了。

图：左边这三个就是寄存器，中间这一部分是驱动器，右边这个就是某一个IO口的引脚了

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-8.png" alt = "P5-GPIO输出-8.png" width = "70%" height = "auto">
</div>
<br>

整体结构可以分为两个部分，上面是输入部分，下面是输出部分。

图：上面是输入部分，下面是输出部分

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-9.png" alt = "P5-GPIO输出-9.png" width = "70%" height = "auto">
</div>
<br>

### 输入部分

我们先来看一下输入部分，首先是这个IO引脚，这里接了两个保护二极管，这个是对输入电压进行限幅的。

#### 两个保护二极管

图：两个保护二极管

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-10.png" alt = "P5-GPIO输出-10.png" width = "70%" height = "auto">
</div>
<br>

上面这个二极管接VDD，3.3V，下面接VSS，0V。

如果输入电压比3.3V还要高，那上方这个二极管就会导通。输入电压产生的电流就会直接流入VDD，而不会流入内部电路，这样就可以避免过高的电压对内部这些电路产生伤害。

图：输入电压比3.3V还要高，那上方这个二极管就会导通

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-12.png" alt = "P5-GPIO输出-12.png" width = "70%" height = "auto">
</div>
<br>

如果输入电压比0V还要低，这个电压是相对于VSS的电压，所以是可以有负电压的，那这时下方这个二极管就会导通，电流会从VSS直接流出去，而不会从内部电路汲取电流，也是可以保护内部电路的。

图：输入电压比0V还要低，下方这个二极管就会导通

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-11.png" alt = "P5-GPIO输出-11.png" width = "70%" height = "auto">
</div>
<br>

如果输入电压在0到3.3V之间，那两个二极管均不会导通。这时二极管对电路没有影响，这就是保护二极管的用途。

#### 上拉电阻和下拉电阻

接下来这根线就到了这个地方，这里连接了一个上拉电阻和一个下拉电阻，上拉电阻至VDD，下拉电阻至VSS。这个开关是可以通过程序进行配置的。

图：这里连接了一个上拉电阻和一个下拉电阻

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-13.png" alt = "P5-GPIO输出-13.png" width = "70%" height = "auto">
</div>
<br>

如果上面导通、下面断开，就是上拉输入模式。如果下面导通、上面断开就是下拉输入模式。如果两个都断开，就是浮空输入模式。

那这个上拉和下拉有什么作用呢？这个其实是为了给输入提供一个默认的输入电平的，因为对应一个数字的端口，输入不是高电平就是低电平。那如果输入引脚啥都不接，那到底是算高电平还是低电瓶平？这就不好说了。

实际情况是，如果输入啥都不接，这时输入就会处于一种浮空的状态，引脚的输入电平极易受外界干扰而改变，就像是一个物体悬浮在太空一样，它的位置是不确定的，受到一点儿扰动就会变化。

为了避免引脚悬空导致的输入数据不确定，我们就需要在这里加上上拉或者下拉电阻了。

如果接入上拉电阻，当引脚悬空时，还有上拉电阻来保证引脚的高电平。所以上拉输入又可以称作是默认为高电平的输入模式，下拉也是同理就是默认为低电平的输入方式。这就像是在太空的物体来到了地球上，如果不施加外力，由于重力的下拉作用，默认还是回到地面的。这个上拉电阻和下拉电阻的阻值都是比较大的，是一种弱上拉和弱下拉，目的是尽量不影响正常的输入操作。

图：当引脚悬空时，还有上拉电阻来保证引脚的高电平

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-14.png" alt = "P5-GPIO输出-14.png" width = "70%" height = "auto">
</div>
<br>

#### 施密特触发器

接着我们继续往下看，这里是一个肖特基触发器，实际上这个应该是施密特触发器。我查了英文的原文档，这里写的就是施密特触发器的英文，所以肖特基触发器应该是一个翻译错误。

图：施密特触发器

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-15.png" alt = "P5-GPIO输出-15.png" width = "70%" height = "auto">
</div>
<br>

这个施密特触发器的作用就是对输入电压进行整形的。它的执行逻辑是，如果输入电压大于某一阈值，输出就会瞬间升为高电平。如果输入电压小于某一阈值，输出就会瞬间降为低电平。

那举个例子，因为这个引脚的波形是外界输入的，虽然是数字信号，实际情况下可能会产生各种失真，比如有这样一个波形。这是一个夹杂了波动的高低变化电平信号，如果没有施密特触发器，那很有可能因为干扰而导致误判。

图：一个夹杂了波动的高低变化电平信号

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-16.png" alt = "P5-GPIO输出-16.png" width = "70%" height = "auto">
</div>
<br>

如果有了施密特触发器，比如定一个这样的阈值上限和下限，高于上限输出高，低于下限输出低。这样施密特触发器的输出就是，首先是低于下限，输出低，然后在这个位置高于上限，输出立即变为高。

虽然在这里信号由于波动再次低于上限了，但是对于施密特触发器来说，只有高于上限或者低于下限，输出才会变化。所以此时低于上限的情况，输出并不会变化，而是继续维持高电平，然后直到下次低于下限时，才会转为低电平。

这里信号即使在下限附近来回横跳，因为没有跳到上限上面去，所以输出仍然是稳定的，直到下一次高于上限，输出才会变为高。那这就是施密特触发器的输出信号。

图：蓝色线为施密特触发器的输出信号。绿线为阈值上限和下限

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-17.png" alt = "P5-GPIO输出-17.png" width = "70%" height = "auto">
</div>
<br>

可以看到相比较输入信号，经过整形的信号就很完美了。在这里使用了两个比较阈值来进行判断，中间留有一定的变化范围，这样可以有效的避免因信号波动造成的输出抖动现象。

图：在这里使用了两个比较阈值来进行判断，中间留有一定的变化范围

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-18.png" alt = "P5-GPIO输出-18.png" width = "70%" height = "auto">
</div>
<br>

接下来经过施密特触发器整形的波形就可以直接写入输入数据寄存器了。我们再用程序读取输入数据寄存器对应某一位的数据，就可以知道端口的输入电平了。

图：波形写入输入数据寄存器

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-19.png" alt = "P5-GPIO输出-19.png" width = "70%" height = "auto">
</div>
<br>

#### 连接到片上外设

最后上面这还有两路线路，这些就是连接到片上外设的一些端口，其中有模拟输入，这个是连接到ADC上的。因为ADC需要接收模拟量，所以这根线是接到施密特触发器前面的。

另一个是复用功能输入，这个是连接到其他需要读取端口的外设上的。比如串口的输入引脚等，这根线接收的是数字量，所以在施密特触发器后面。

图：连接到片上外设

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-20.png" alt = "P5-GPIO输出-20.png" width = "70%" height = "auto">
</div>
<br>

### 输出部分

#### 输出控制

接着我们再来看一下输出的部分。数字部分可以由输出数据寄存器或片上外设控制两种控制方式通过这个数据选择器接到了输出控制部分。

如果选择通过输出数据寄存器进行控制，就是普通的I/O口输出，写这个数据寄存器的某一位就可以操作对应的某个端口了。

那左边还有一个叫做位设置/清除寄存器，这个可以用来单独操作输出数据寄存器的某一位，而不影响其他位。因为这个输出数据寄存器同时控制16个端口，并且这个寄存器只能整体读写。所以如果想单独控制其中某一个端口而不影响其他端口的话，就需要一些特殊的操作方式。

#### 想单独控制其中某一个端口而不影响其他端口

##### 第一种方法

第一种方式是先读出这个寄存器，然后用按位与和按位或的方式更改某一位，最后再将更改后的数据写回去，在C语言中就是&=和|=的操作。

这种方法比较麻烦，效率不高，对于I/O口的操作而言不太合适。

##### 第二种方法

第二种方式是通过设置这个 **位设置/未清除寄存器**。

**位设置寄存器**

如果我们要对某一位进行的操作，在位设置寄存器的对应位写1即可，剩下不需要操作的位写0。

这样它内部就会有电路自动将输出数据寄存器对应位置为1，而剩下写0的位则保持不变。

图：在位设置寄存器的对应位写1即可，剩下不需要操作的位写0。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-21.png" alt = "P5-GPIO输出-21.png" width = "70%" height = "auto">
</div>
<br>

这样就保证了只操作其中某一位，而不影响其他位，并且这是一步到位的操作。

**位清除寄存器**

如果想对某一位进行清0的操作，就在位清除寄存器的对应位写1即可。这样内部电路就会把这一位清0了。

这就是第二种方式，也就是这个位设置和位清除寄存器的作用。

##### 第三种方法

另外还有第三种操作方式，就是读写STM32中的“位带”区域。这个位带的作用就跟51单片机的位寻址作用差不多。

在STM32中，专门分配的有一段地址区域，这段地址映射了RAM和外设寄存器所有位。

读写这段地址中的数据，就相当于读写所映射位置的某一位。这就是位带的操作方式，这个方式我们本课程暂时不会用到。

##### 我们采用第二种方法

我们的教程主要使用的是库函数来操作，库函数使用的就是读写位设置和位清除寄存器的方法。

#### 推挽、开漏或关闭三种输出方式

那我们继续看，接下来输出控制之后，就接到了两个MOS管，上面是P-MOS，下面是N-MOS。

这个MOS管就是一种电子开关，我们的信号来控制开关的导通和关闭，开关负责将I/O口接到VDD或者VSS。在这里可以选择推挽、开漏或关闭三种输出方式。

##### 推挽输出模式

在推挽输出模式下，P-MOS和N-MOS均有效，数据寄存器为1时，上管导通，下管断开，输出直接接到VDD，就是输出高电平。

图：数据寄存器为1时，输出高电平

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-26.png" alt = "P5-GPIO输出-26.png" width = "70%" height = "auto">
</div>
<br>

数据寄存器为0时，上管断开，下管导通，输出直接接到VSS，就是输出低电平。这种模式下，高低电平均有较强的驱动能力，所以推挽输出模式也可以叫强推输出模式。

图：数据寄存器为0时，输出低电平

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-25.png" alt = "P5-GPIO输出-25.png" width = "70%" height = "auto">
</div>
<br>

在推挽输出模式下，STM32对I/O口具有绝对的控制权，高低电平都有STM32说了算。

##### 开漏输出模式

在开漏输出模式下，这个P-MOS是无效的，只有N-MOS在工作。

数据寄存器为1时，下管断开，这时输出相当于断开，也就是高阻模式。

图：数据寄存器为1时，高阻模式

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-23.png" alt = "P5-GPIO输出-23.png" width = "70%" height = "auto">
</div>
<br>

数据寄存器为0时，下管导通，输出直接接到VSS，也就是输出低电平。

图：数据寄存器为0时，输出低电平

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-24.png" alt = "P5-GPIO输出-24.png" width = "70%" height = "auto">
</div>
<br>

这种模式下只有低电平有驱动能力，高电平是没有驱动能力的。那这个模式有什么用呢？这个开漏模式可以作为通信协议的驱动方式。比如I2C通信的引脚就是使用的开漏模式，在多机通信的情况下，这个模式可以避免各个设备的相互干扰。

另外开漏模式还可以用于输出5V的电平信号，比如在I/O口外接一个上拉电阻到5V的电源，当输出低电平时，由内部的N-MOS直接接VSS。输出高电平时由外部的上拉电阻拉高至5V。这样就可以输出5V的电平信号，用于兼容一些5V电平的设备，这就是开漏输出的主要用途。

图：开漏模式还可以用于输出5V的电平信号

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-22.png" alt = "P5-GPIO输出-22.png" width = "70%" height = "auto">
</div>
<br>

##### 关闭输出模式

剩下的一种状态就是关闭，这个是当引脚配置为输入模式的时候，这两个MOS管都无效，也就是输出关闭，端口的电平由外部信号的控制。

图：输出关闭，端口的电平由外部信号的控制

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-27.png" alt = "P5-GPIO输出-27.png" width = "70%" height = "auto">
</div>
<br>

这些就是GPIO结构的全部介绍了。

## GPIO模式

接下来我们就来看一下GPIO的8种工作模式。

通过配置GPIO的端口配置寄存器，上面这个位结构的电路就会根据我们的配置进行改变。比如开关的通断、N-MOS和P-MOS是否有效、数据选择器的选择等。

这个端口的电路就可以配置成以下8种模式，分别是浮空输入、上拉输入、下拉输入、模拟输入、开漏输出、推挽输出、复用开漏输出和复用推挽输出。

图：GPIO的8种模式

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-28.png" alt = "P5-GPIO输出-28.png" width = "70%" height = "auto">
</div>
<br>

### 浮空/上拉/下拉输入

我们依次来看一下，首先是前三个，浮空输入、上拉输入和下拉输入。

这三个模式的电路结构基本是一样的，区别就是上拉电阻和下拉电阻的连接，它们都属于数字的输入口，那特征就是，都可以读取端口的高低电平。

当引脚悬空时，上拉输入默认是高电平，下拉输入默认是低电平，而浮空输入的电平是不确定的，所以在使用浮空输入时，端口一定要接上一个连续的驱动源，不能出现悬空的状态。

图：浮空/上拉/下拉输入

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-29.png" alt = "P5-GPIO输出-29.png" width = "70%" height = "auto">
</div>
<br>

那我们来看一下这三种模式的电路结构，这里可以看到，在输入模式下，输出驱动器是断开的，端口只能输入而不能输出。

图：在输入模式下，输出驱动器是断开的，端口只能输入而不能输出

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-30.png" alt = "P5-GPIO输出-30.png" width = "70%" height = "auto">
</div>
<br>

上面这两个电阻可以选择为上拉工作、下拉工作或者都不工作，对应的就是上拉输入、下拉输入和浮空输入。

然后输入通过施密特触发器进行波形整形后，连接到输入数据寄存器。

另外右边这个输入保护，这里上面写的是VDD或者VDD_FT，这就是3.3V端口和容忍5V端口的区别。

下面可以看到，这里说VDD_FT对5V容忍，I/O引脚是特殊的，它与VDD不同。至于怎么个特殊法，手册也没有说，咱也不用管，就知道一下。这个容忍五V的引脚，它的上边保护二级管要做一下处理，要不然这里直接接VDD3.3V的话，外部再接入5V电压，就会导致上边二极管开启，并且产生比较大的电流，这个是不太妥当的。

图：输入保护

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-31.png" alt = "P5-GPIO输出-31.png" width = "70%" height = "auto">
</div>
<br>

### 模拟输入

接着我们再来看一下下面这一个模拟输入，特征是GPIO无效，引脚直接接入内部ADC。这个模拟输入可以说是ADC模数转换器的专属配置了。

图：模拟输入的电路结构

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-37.png" alt = "P5-GPIO输出-37.png" width = "70%" height = "auto">
</div>
<br>

我们看一下模拟输入的结构，这里输出是断开的，输入的施密特触发器也是关闭的无效状态。

图：整个GPIO的这些都是没用的，那就只剩下这一根线了

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-34.png" alt = "P5-GPIO输出-34.png" width = "70%" height = "auto">
</div>
<br>

所以整个GPIO的这些都是没用的，那就只剩下这一根线了，也就是从引脚直接接入片上外设，也就是ADC。所以当我们使用ADC的时候，将引脚配置为模拟输入就行了，其他时候一般用不到模拟输入。

图：从引脚直接接入片上外设

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-33.png" alt = "P5-GPIO输出-33.png" width = "70%" height = "auto">
</div>
<br>

### 开漏输出和推挽输出

接着我们再来看一下开漏输出和推挽输出，这两个电路结构也基本一样。
都是数字输出口，可以用于输出高低电平。

区别就是开漏输出的高电平呈现的是高阻态，没有驱动能力。而推挽输出的高低电平都是具有驱动能力的。

图：开漏输出和推挽输出的电路结构

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-36.png" alt = "P5-GPIO输出-36.png" width = "70%" height = "auto">
</div>
<br>

那这两种模式电路结构就是这样的，这时候输出是由输出数据寄存器控制的，这个P-MOS如果无效，就是开漏输出。如果P-MOS和N-MOS都有效，就是推挽输出。

另外我们还可以看到，在输出模式下，输入模式也是有效的。但是我们刚才的电路图，在输入模式下，输出都是无效的。

图：在输出模式下，输入模式也是有效的

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-38.png" alt = "P5-GPIO输出-38.png" width = "70%" height = "auto">
</div>
<br>

这是因为一个端口只能有一个输出，但可以有多个输入，所以当配置成输出模式的时候，内部也可以顺便输入一下，这个也是没啥影响的。

### 复用开漏输出和复用推挽输出

最后我们再来看一下复用开漏输出和复用推挽输出。这两模式跟普通的开漏输出和推挽输出也差不多，只不过是复用的输出，引脚电平是由片上外设控制的。

图：复用开漏输出和复用推挽输出的电路结构

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-39.png" alt = "P5-GPIO输出-39.png" width = "70%" height = "auto">
</div>
<br>

那我们看一下这两个模式的结构，可以看到通用的输出这里是没有连接的。引脚的控制权转移到了片上外设，由片上外设来控制。

在输入部分，片上外设也可以读取引脚的电平，同时普通输入也是有效的，顺便接收一下电平信号。

图：复用开漏输出和复用推挽输出中 输出由片上外设来控制，在输入部分，片上外设也可以读取引脚的电平，同时普通输入也是有效的

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-40.png" alt = "P5-GPIO输出-40.png" width = "70%" height = "auto">
</div>
<br>

其实在GPIO的这8种模式中，除了模拟输入这个模式会有关闭数字的输入功能，在其他的7个模式中，所有的输入都是有效的。这些就是STM32 GPIO的全部介绍了。

## STM32参考手册-GPIO

接着我们再打开这个STM32的参考手册，大概地看一下这个手册。我们打开这个GPIO和AFIO这一章，我们本节只讲GPIO，AFIO以后再讲。

手册：《stm32f10xxx参考手册(中文)》

那我们可以看到这个手册上来就说了GPIO的8种工作模式，下面就是介绍这些模式的。我刚才也给大家讲过了，剩下的还有一些没讲到的点。大家可以再看一下手册，然后这里有外设的GPIO配置，当我们使用这些片上外设的引脚时，可以参考这个表里给的配置。 

图：《stm32f10xxx参考手册(中文)》-P105-GPIO的8种工作模式

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-41.png" alt = "P5-GPIO输出-41.png" width = "70%" height = "auto">
</div>
<br>

图：《stm32f10xxx参考手册(中文)》-P110-外设的GPIO配置

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-42.png" alt = "P5-GPIO输出-42.png" width = "70%" height = "auto">
</div>
<br>

最后就是GPIO的寄存器。

### 端口配置寄存器

首先是GPIO配置寄存器，每一个端口的模式由4位进行配置，16个端口就需要64位。所以这里的配置寄存器有2个，一个是端口配置低寄存器，一个是端口配置高寄存器。具体怎么配置的，可以参考一下下面这个介绍。

图：每一个端口的模式由4位进行配置，端口配置低寄存器对应8个端口，端口配置高寄存器对应8个端口，一共就是16个端口

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-43.png" alt = "P5-GPIO输出-43.png" width = "70%" height = "auto">
</div>
<br>

另外这里还多出来一项GPIO输出的速度，这个在结构图里并没有说这个速度这个参数。

图：GPIO输出的速度

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-45.png" alt = "P5-GPIO输出-45.png" width = "70%" height = "auto">
</div>
<br>

这个GPIO的输出速度可以限制输出引脚的最大翻转速度。这个设计出来是为了低功耗和稳定性的。我们一般要求不高的时候，直接配置成50MHz就可以了。

### 端口输入数据寄存器

然后是端口输入数据寄存器，这个就是PPT中的这个寄存器。

图：端口输入数据寄存器在GPIO结构中的位置

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-44.png" alt = "P5-GPIO输出-44.png" width = "70%" height = "auto">
</div>
<br>

里面的低16位对应16个引脚，高16位没有使用。

### 端口输出数据寄存器

然后是端口输出数据寄存器，也就是这个寄存器。

图：端口输出数据寄存器在GPIO结构中的位置

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-46.png" alt = "P5-GPIO输出-46.png" width = "70%" height = "auto">
</div>
<br>

同样，低16位对应16个引脚，高16位没有使用。

图：端口输出数据寄存器，低16位对应16个引脚，高16位没有使用

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-47.png" alt = "P5-GPIO输出-47.png" width = "70%" height = "auto">
</div>
<br>

### 端口位设置/清除寄存器

接下来是端口位设置/清除寄存器，也就是这个寄存器。

图：端口位设置/清除寄存器在GPIO结构中的位置

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-48.png" alt = "P5-GPIO输出-48.png" width = "70%" height = "auto">
</div>
<br>

这个寄存器的高16位是进行位清除的，低16位是进行位设置的，写1就是设置或者清除，写0就是不产生影响。

图：端口位设置/清除寄存器，高16位是进行位清除的，低16位是进行位设置的

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-49.png" alt = "P5-GPIO输出-49.png" width = "70%" height = "auto">
</div>
<br>

### 端口位清除寄存器

下面这里还有一个端口位清除寄存器，这个寄存器的低16位和上面这个端口位设置/清除寄存器的高16位功能是一样的。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-50.png" alt = "P5-GPIO输出-50.png" width = "70%" height = "auto">
</div>
<br>

那为啥还要有这个寄存器呢？

这个是为了方便操作设置的。如果你只想单一的进行位设置或者位清除，那位设置时用上面这个计算器，位清除时用下面这个计算器。因为在设置和清除时使用的都是低16位的数据，这样就方便一些。

如果你想对多个端口同时进行位设置和位清除，那就使用第一个寄存器就行了。这样可以保证位设置和位清除的同步性。

当然你要对信号的同步性要求不高的话，先进行位设置，再进行位清除也是没问题的。

### 端口配置锁定寄存器

最后一个就是端口配置锁定寄存器，这个可以对端口的配置进行锁定，防止意外更改，使用方法看介绍，这个我们暂时用的不多。

有关STM32内部的GPIO外设我们就讲完了。

## STM32外部的设备和电路

### LED和蜂鸣器

接下来我们再来看一下STM32外部的设备和电路。首先是LED和蜂鸣器这些，相信大家对这些东西应该已经比较熟悉了，我就快速的过一下。

- LED：发光二极管，正向通电点亮，反向通电不亮
- 有源蜂鸣器：内部自带振荡源，将正负极接上直流电压即可持续发声，频率固定
- 无源蜂鸣器：内部不带振荡源，需要控制器提供振荡脉冲才可发声，调整提供振荡脉冲的频率，可发出不同频率的声音

#### LED

- LED：发光二极管，正向通电点亮，反向通电不亮

LED就是发光二极管，正向通电点亮，反向通电不亮。下面这个就是LED的电路符号，左边是正极，右边是负极。

图：LED的电路符号

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-51.png" alt = "P5-GPIO输出-51.png" width = "70%" height = "auto">
</div>
<br>

这个是LED的实物图，如果是引脚没有剪过的LED，那其中长角是正极，短角是负极。通过LED内部也可以看正负极，这里较小的一半是正极，较大的一半是负极，这就是LED。

图：LED的实物图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-53.png" alt = "P5-GPIO输出-53.png" width = "70%" height = "auto">
</div>
<br>

#### 蜂鸣器

接下来就是蜂鸣器了，蜂鸣器分为有源蜂鸣器和无源蜂鸣器。

##### 有源蜂鸣器

- 有源蜂鸣器：内部自带振荡源，将正负极接上直流电压即可持续发声，频率固定

有源蜂鸣器就是内部自带震荡源，将正负极接上直流电压即可持续发声，频率固定。我们本视频用的就是下面这个图所示的有源蜂鸣器。

图：有源蜂鸣器

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-54.png" alt = "P5-GPIO输出-54.png" width = "70%" height = "auto">
</div>
<br>

下面是它的内部电路，这里用了一个三极管开关来进行驱动。我们在VCC和GND分别接上正负极的供电，然后中间这个引脚接低电平，蜂鸣器就会响，接高电平，蜂鸣器关闭。

这里也有文字说明，写的是低电平触发，这就是有源蜂鸣器的用法。

##### 无源蜂鸣器

- 无源蜂鸣器：内部不带振荡源，需要控制器提供振荡脉冲才可发声，调整提供振荡脉冲的频率，可发出不同频率的声音

另外还有无源蜂鸣器，它是内部不带震荡源，需要控制器提供震荡脉冲才可发生发声。调整提供震荡脉冲的频率，可发出不同频率的声音。这个我们在51单片机的教程已经使用过了，本视频暂时不会涉及。

### LED和蜂鸣器的硬件电路

然后我们来看一下LED和蜂鸣器的硬件电路。

#### LED硬件电路

下图是使用STM32的GPIO口驱动LED的电路。

图：LED硬件电路(上面为低电平驱动，下面为高电平驱动)

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-55.png" alt = "P5-GPIO输出-55.png" width = "70%" height = "auto">
</div>
<br>

##### 低电平驱动的LED电路

下图这个是低电平驱动的电路，LED正极接3.3V，负极通过一个限流电阻接到PA0上。

图：低电平驱动的LED电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-58.png" alt = "P5-GPIO输出-58.png" width = "70%" height = "auto">
</div>
<br>

当PA0输出低电平时，LED两端就会产生电压差，就会形成正向导通电流，这样LED就会点亮了。

图：LED点亮

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-56.png" alt = "P5-GPIO输出-56.png" width = "70%" height = "auto">
</div>
<br>

当PA0输出高电平时，因为LED两端都是3.3V的电压，不会形成电流，所以高电平，LED就是熄灭。

这里的限流电阻一般都是要接的，一方面它可以防止LED因为电流过大而烧毁，另一方面它也可以调整LED的亮度。如果你觉得LED太亮比较刺眼的话，可以适当的增大限流电阻的阻值。

图：记得加上限流电阻

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-57.png" alt = "P5-GPIO输出-57.png" width = "70%" height = "auto">
</div>
<br>

当然我们本课程使用面包板插接LED的话，为了简化电路，就省去了这个限流电阻。大家自己设计电路的时候要注意加上。

##### 高电平驱动的LED电路

下面这个图就是高电平驱动的电路。

图：高电平驱动的LED电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-59.png" alt = "P5-GPIO输出-59.png" width = "70%" height = "auto">
</div>
<br>

LED负极接到GND，正极通过一个限流电阻接到PA0上，这时就是高电平点亮，低电平熄灭了。

那这两种驱动方式应该如何选择呢？

这就得看这个I/O口高低音平的驱动能力如何了。我们刚才介绍这个GPIO在推挽输出模式下，高低电平均有比较强的驱动能力，所以在这里这两种接法均可。

但是在单片机的电路里，一般倾向使用第一种接法(低电平驱动的LED电路)。因为很多单片机或者芯片都使用了高电平弱驱动，低电平强驱动的规则，这样可以一定程度上避免高低电平打架。所以如果高电平驱动能力弱，那就不能使用第二种连接方法了。

#### 蜂鸣器电路

接着看蜂鸣器电路，这里使用了三极管开关的驱动方案，三接管开关是最简单的驱动电路，对于功率稍微大一点的，直接用I/O口驱动会导致STM32负担过重，这时就可以用一个三极管驱动电路来完成驱动的任务。

图：蜂鸣器电路(上面是PNP三极管，下面是NPN三极管)

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-60.png" alt = "P5-GPIO输出-60.png" width = "70%" height = "auto">
</div>
<br>

#####  PNP三极管的蜂鸣器电路

图：PNP三极管的蜂鸣器电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-61.png" alt = "P5-GPIO输出-61.png" width = "70%" height = "auto">
</div>
<br>

上面这个图是PNP三极管的驱动电路，三极管的左边是基极，带箭头的是发射级，剩下的是集电极。它左边的基极给低电平，三极管就会导通，那通过3.3V和GND，就可以给蜂鸣器提供驱动电流了。基极给高电平，三极管截止，蜂鸣器就没有电流。

#####  NPN三极管的蜂鸣器电路

图：NPN三极管的蜂鸣器电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-62.png" alt = "P5-GPIO输出-62.png" width = "70%" height = "auto">
</div>
<br>

这个图是NPN三极管的驱动电路，同样左边是基极，带箭头的是发射极，剩下的是集电极。它的驱动逻辑跟上面的是相反的，基极给高电平导通，低电平断开。

另外注意一下这个PNP的三极管最好接在上边，NPN的三极管最好接在下边。这是因为三极管的通断，需要在发射极和基极之间产生一定的开启电压的。

图：PNP的三极管最好接在上边，NPN的三极管最好接在下边

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-63.png" alt = "P5-GPIO输出-63.png" width = "70%" height = "auto">
</div>
<br>

如果你把负载接在发射极这边，可能会导致三极管不能开启。

图：把负载接在发射极这边，可能会导致三极管不能开启

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-64.png" alt = "P5-GPIO输出-64.png" width = "70%" height = "auto">
</div>
<br>

那这些就是STM32外部的设备和电路了。

## 面包板

最后我们来看一下面包板的使用方法。这里第一个图就是面包板的正面。

图：面包板的正面

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-65.png" alt = "P5-GPIO输出-65.png" width = "70%" height = "auto">
</div>
<br>

下面这个是把它背面的双面胶撕掉的样子。这里一条一条的就是面包板内部的金属爪。

图：面包板背面的双面胶撕掉的样子

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-66.png" alt = "P5-GPIO输出-66.png" width = "70%" height = "auto">
</div>
<br>

下面这个图就是这个金属爪的示意图。当我们把元件的引脚插到面包板的孔里后，它内部的金属爪就会夹住引脚。

图：金属爪的示意图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-67.png" alt = "P5-GPIO输出-67.png" width = "70%" height = "auto">
</div>
<br>

再看一下背面的图，我们可以发现金属爪的排列规律，是中间的金属爪是竖着放的，上下四排是连在一起的四个整体的金属爪，那就对应这个面包板的孔的连接关系，就是在这里竖着的5个孔内部都是连接在一起的。

这样我们元件插在一纵排的不同孔位时，内部的金属爪就实现了线路的连接。而上下四排孔整体是连在一起的，这四排是用于供电的。

这里也标了，第一排是正极，第二排是负极，第三排也是正极，第四排也是负极。如果我们需要供电，就从上下的孔位中，用跳线引出来即可。

另外再说明一下，这个供电的引脚，有的面包板并不是一整排都是连接的。我以前用的面包板，这中间这四个地方都是断开的。如果是断开的，就需要像这样在中间用跳线把两边连在一起。当然我这是拆开的面包板，内部都是连在一起的，还挺意外的。

然后我们举个例子来演示一下面包板的使用。如果我们要用这个面包板实现电源直接点亮一个LED灯的电路。

就可以这样连接。首先把上面两排的供电引脚接上电源的正负极，然后用跳线将正极引下来到这个孔里，然后在纵向下面的孔，横着插一个限流电阻到右边的这个孔，接着再在右边纵向上面的孔，横着插一个LED到右边的这个孔，然后再用跳线把右边引到负极。

图：连接图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P5-GPIO%E8%BE%93%E5%87%BA-68.png" alt = "P5-GPIO输出-68.png" width = "70%" height = "auto">
</div>
<br>

这样整个电路的电流就是从电源正极出来，通过金属爪到这里，再通过跳线到这里，再通过金属爪下来，然后通过电阻，再通过金属转上去，然后通过LED，最后通过金属爪到跳线，然后从这里接回负极。

这样就用面包板实现了电源直接点亮一个LED灯的电路，这就是面包板的基本使用方法了。好，那我们本小节就到这里，下一小节我们将用面包板连接电路，然后写程序实现我们想要的功能。

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