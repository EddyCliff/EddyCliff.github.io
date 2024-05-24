---
title: "嵌入式开发-STM32标准库学习：GPIO输入"
date: 2024-05-13T00:17:58+10:00
lastmod: 2024-05-13T00:17:58+10:00
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
- LED
- 传感器模块
- STM32标准库开发
- 嵌入式开发
- STM32F1
description: "本博客首先介绍了通过按键控制LED的基础操作，随后转向光敏传感器控制蜂鸣器的应用，涵盖了硬件连接和预期现象的讨论。课程还强调了C语言中指针的重要性，并给出了简单介绍。特别提到了处理按键抖动的方法，建议通过增加延时来改善，以确保程序的稳定性和准确性。此外，课程还介绍了四种传感器模块的工作原理，包括光敏电阻、热敏电阻、对射式及反射式红外传感器，并讲述了如何通过这些模块获取外部模拟量的变化信息。在讨论模拟电压和数字电压的生成及转换过程中，突出了各种电子元件的作用和选择合适的输入模式的重要性。同时，还覆盖了C语言中的数据类型、宏定义、结构体和枚举的使用，强调了这些概念在程序设计中的重要作用。"
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

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/%E7%9F%A5%E8%AF%86%E8%92%99%E8%94%BD%E4%BA%86%E6%88%91%E7%9A%84%E5%8F%8C%E7%9C%BC.jpg" alt = "知识蒙蔽了我的双眼.jpg" width = "70%" height = "auto">
</div>
<br>

## INIT

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Blog-Common-Images/INIT.jpg" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

>**INIT：本节内容正式开始。action!**


## 引言

欢迎回来，本小节，我们来继续学习GPIO的输入部分。我们先看一下本节程序的现象，本节课一共要写两个程序，第一个是按键控制LED，第二个是光敏传感器控制蜂鸣器。

我们先来看一下第一个程序(3-4 按键控制LED)，编译，下载，然后看一下面包板，这里我已经连接了两个按键和两个LED。那我按下左边的按键，左边的LED点亮，再按一下熄灭。

如果按右边的按键，那右边的LED也会点亮、熄灭、点亮、熄灭，并且两个操作互不影响。一个按键控制一个LED的亮灭，这就是第一个程序的现象。



<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5.png" alt = "P7-GPIO输入.png" width = "70%" height = "auto">
</div>
<br>

图：第一个程序现象

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-2.png" alt = "P7-GPIO输入-2.png" width = "70%" height = "auto">
</div>
<br>


接着再看一下第二个程序(3-5 光敏传感器控制蜂鸣器)的现象。编译下载，然后看一下面包板，这里我连接了蜂鸣器模块和光敏电阻传感器模块。当我用手挡住光敏电阻，光线变暗时，蜂鸣器就会鸣响。当我的手拿开光线变亮，蜂鸣器就停止，这就是第二个程序的现象。

图：第二个程序现象

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-1.png" alt = "P7-GPIO输入-1.png" width = "70%" height = "auto">
</div>
<br>

好，那我们回到PPT。上两小节，我们已经把GPIO的结构和8种输入输出模式都讲完了。

所以本小节GPIO输入的部分，我们就直接从外部硬件设备开始讲了。

另外本小节还要再给大家介绍一下C语言的相关知识点，其中包括C语言的数据类型，宏定义、typedef、结构体和枚举这些知识点。这些知识点都是库函数里经常且反复出现的东西，我再给大家详细的介绍一下，了解了这些知识点，你对库函数的执行逻辑就会理解的更加清晰明了。

至于C语言的if else，加减乘除运算这些东西，这是最基本的东西，相信大家应该都已经会使用了，在这里我就不再赘述了。

另外还有一个重要的知识点，就是C语言的指针，我之前已经录制过了一期指针的视频，里面详细介绍了指针的含义和用法。如果你对C语言的指针还不熟悉的话，可以到我的视频主页里先看一下指针在那一期视频。

好，我们先来看一下GPIO输入模式下的硬件和电路。

## 按键

- 按键：常见的输入设备，按下导通，松手断开
- 按键抖动：由于按键内部使用的是机械式弹簧片来进行通断的，所以在按下和松手的瞬间会伴随有一连串的抖动

首先是按键，这个按键是最常见的输入设备了。按下导通，松手断开。下面这个图就是我们套件里的按键实物图。上面白色的是按钮，下面有两个引脚。当按钮按下去的时候，下面两个引脚就是接通的，松手后这两个引脚就自动断开了。这个执行逻辑太简单了是吧，我就不过多介绍了。

图：按键实物图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-3.png" alt = "P7-GPIO输入-3.png" width = "70%" height = "auto">
</div>
<br>

接着看一下在单片机中应用按键时候的一个现象，就是按键抖动现象。

按键抖动现象，就是由于按键内部使用的是机械式弹簧片来进行通断的，所以在按下和松手的瞬间会伴随着一连串的抖动。

通过下面这个波形就可以看到，假设按键没按下是高电平，按下了就是低电平。那在按下的瞬间就信号由高电平变为低电平时，就会来回抖动几下。这个抖动会比较快，通常在5到10毫秒之间，人眼是分辨不出来的。但是对于高速运行的单片机而言，5到10毫秒的时间还是很漫长的。

图：波形 按键抖动

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-4.png" alt = "P7-GPIO输入-4.png" width = "70%" height = "auto">
</div>
<br>

所以我们要对这个抖动进行过滤，否则就会出现按键按一下单片机却反映了多次的现象。另外在按键松手的时候，也会有一小段时间的抖动，这个我们在程序中也要注意过滤，注意一下。最简单的过滤方法就是加一段延时，把这个抖动时间耗过去，这样就没问题了。

## 传感器模块

- 传感器模块：传感器元件（光敏电阻/热敏电阻/红外接收管等）的电阻会随外界模拟量的变化而变化，通过与定值电阻分压即可得到模拟电压输出，再通过电压比较器进行二值化即可得到数字电压输出


接着我们再来看一下传感器模块的介绍。我们套间里提供了4种传感器模块，分别是光敏电阻传感器、热敏电阻传感器、对射式红外传感器、反射式红外传感器，它们的电路结构和工作原理都差不多。

这些传感器模块，他们都是利用传感器元件，比如光敏电阻、热敏电阻、红外接收管等。这些软件的电阻会随外界的模拟量变化而变化。比如光线越强，光敏电阻的阻值就越小，温度越高，热敏电阻的阻值就越小，红外光线越强，红外接收管的阻值就越小。

但是电阻的变化不容易直接被观察，所以我们通常将传感器元件与定值电阻进行串联分压，这样就可以得到模拟电压的输出了。对电路来说，检测电压就非常容易了。

另外这个模块还可以通过电压比较器，来对这个模拟电压进行二值化，这样就可以得到数字电压输出了。

图：传感器模块实物图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-5.png" alt = "P7-GPIO输入-5.png" width = "70%" height = "auto">
</div>
<br>

### 传感器电路原理分析

我们看一下下面这个电路图，这个就是传感器模块的基本电路。

图：传感器模块基本电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-6.png" alt = "P7-GPIO输入-6.png" width = "70%" height = "auto">
</div>
<br>

#### 传感器电路原理分析-部分1-AO电压通过排针输出

我们先看一下这个部分。

图：传感器基本电路-部分1

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-7.png" alt = "P7-GPIO输入-7.png" width = "70%" height = "auto">
</div>
<br>


这个N1就是传感器元件所代表的可变电阻，它的组织可以根据环境的光线、温度等模拟量进行变化。上面这个R1，是和N1进行分压的定值电阻。

R1和N1串联，一端接在VCC正极，一端接在GND负极，这就构成了基本的分压电路。

左边这个C2是一个滤波电容，它是为了给中间的电压输出进行滤波的，用来滤除一些干扰，保证输出电压波形的平滑。一般我们在电路里遇到这种一端接在电路中，另一端接地的电容，都可以考虑一下这个是不是滤波电容的作用。

如果是滤波电容的作用，那这个电容就是用来保证电路稳定的，并不是电路的主要框架。这时候我们在分析电路的时候，就可以先把这个电容给抹掉，这样就可以使我们的电路分析更加简单。

那我们把这个电容抹掉，整个电路的主要框架就是定值电阻和传感器电阻的分压电路了。

在这里可以用分压定理来分析一下传感器电阻的阻值变化对输出电压的影响。

当然我们还可以用上下拉电阻的思维来分析。

当这个N1阻值变小时，下拉作用就会增强，中间的AO端的电压就会拉低。极端情况下N1阻值为0，AO输出被完全下拉，输出0V。

当N1阻值变大，下拉作用就会减弱，中间的引脚由于R1的上拉的作用，电压就会升高。极端情况下N1组织无穷大，相当于断路，输出电压被R1拉高至VCC，这是用上下拉电阻来分析电路的。

我可以举个例子来说明上下拉电阻的工作逻辑。AO这个输出端你可以把它想象成一个放在屋里的水平杆子。R1上拉电阻相当于拴在屋顶的弹簧，将杆子往上拉，N1下拉电阻相当于栓在地面的弹簧，将杆子往下拉。这个电阻的阻值越小，弹簧的拉力就越强。这个杆子的高度就相当于电路中的电压。

如果只有上拉弹簧或者下拉弹簧，那杆子肯定被拉到了屋顶或者地面，在电路中就相当于中间点的电压为VCC或者GND。当两个弹簧相互拉扯的时候，中间的输出端就会向拉力强的一端偏移，至于偏移多少就取决于两个弹簧的弹力之差了。

如果上下拉弹簧的弹力一致，则杆子会处于居中的位置，也就是电路输出二分之VCC的电压。如果上面的阻值小，拉力强，那输出电压就会变高。反之下面的阻值小，输出电压就会变低。如果阻值为0，在电路中就是短接的状态，那就相当于拉力无穷大了。如果上下拉电阻的阻值都为0，就是两个无穷大的力在对抗，在电路中呈现的状态就是电源短路。所以这种情况应该避免。

这个上拉电阻和下拉电阻在单片机电路中会经常出现，比如弱上拉、弱下拉、强上拉、强下拉等。这里强和弱就指电阻阻值的大小，也就是这个弹簧弹力的大小。

上拉和下拉就指是接到VCC还是GND，也就是这个杆子是拉向屋顶还是拉向地面，最终的话输出电压就是在弹簧拉扯下最终杆子的高低。大家以后再遇到上拉电阻和下拉电阻的分析，就可以尝试一下用我这个杆子和弹簧的模型来分析一下，相信你会有更加深刻的理解的。

那我们回到这个电路继续来看，在这两个电阻的分压下，AO就是我们想要的模拟电压输出了。所以这里可以看到，这个AO电压就直接通过这个排针输出了，这就是AO电压的由来，仅需两个电阻分压即可得到。

图：AO电压直接通过这个排针输出

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-8.png" alt = "P7-GPIO输入-8.png" width = "70%" height = "auto">
</div>
<br>

#### 传感器电路原理分析-部分2-数字输出

接下来这个模块还支持有数字输出，这个数字输出就是对AO进行二值化的输出。这里二值化是通过这个芯片LM393来完成的。

这个LM393是一个电压比较器，芯片里面有两个独立的电压比较器电路，然后剩下的是VCC和GND供电。那我们VCC就接到电路的VCC，GND也接到了电路的GND。这里有个电容，是一个电源供电的滤波电容。

##### 运算放大器分析

这个电压比较器其实就是一个运算放大器，有关运算放大器的知识，我在51单面机视频的AD/DA那一节也有讲过，大家不会的可以去看一下。

我画一下这个运算放大器当做比较器的情况。当这个同相输入端的电压大于反相输入端的电压时，输出就会瞬间升高为最大值也就是输出接VCC。反之，当同相输入端的电压小于反相输入端的电压时，输出就会瞬间降为最小值，也就是输出接GND。这样就可以对一个模拟电压进行二值化了。

图：当这个同相输入端的电压大于反相输入端的电压时，输出接VCC

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-9.png" alt = "P7-GPIO输入-9.png" width = "70%" height = "auto">
</div>
<br>

图：当同相输入端的电压小于反相输入端的电压时，输出接GND

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-10.png" alt = "P7-GPIO输入-10.png" width = "70%" height = "auto">
</div>
<br>

我们看一下实际的应用。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-12.png" alt = "P7-GPIO输入-12.png" width = "70%" height = "auto">
</div>
<br>

这里同样输入端IN+接到了AO这里，就是模拟电压端，IN-接了一个电位器，这个电位器的接法也是分压电阻的原理。

拧动电位器，IN-就会生成一个可调的阈值电压，两个电压进行比较，最终输出结果就是DO，数字电压输出。DO最终就接到了引脚的输出端，这就是数字电压的由来。

#### 传感器电路原理分析-部分3-指示灯

然后右边这里还有两个指示灯电路。左边的是电源指示灯，通电就亮。右边的是DO输出指示灯，它可以指示DO的输出电平，低电平点亮，高电平熄灭。

图：两个指示灯电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-11.png" alt = "P7-GPIO输入-11.png" width = "70%" height = "auto">
</div>
<br>

那右边DO这里还多了个R5上拉电阻，这个是为了保证默认输出为高电平的。

然后就是P1的排针，分别是VCC、GND、DO和AO。

图：排针

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-13.png" alt = "P7-GPIO输入-13.png" width = "70%" height = "auto">
</div>
<br>

#### 传感器实物图分析

图：传感器实物图

从左至右：光敏电阻传感器、热敏电阻传感器、电位器、用于循迹小车的红外传感器

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-14.png" alt = "P7-GPIO输入-14.png" width = "70%" height = "auto">
</div>
<br>

这个图就是四个传感器模块的，对于光敏电阻传感器来说，这个N1就是光敏电阻。对于热敏电阻传感器来说，这个N1就是热敏电阻。对于这个红外传感器来说，这个N1就是一个红外接收管。当然对应还会多一个点亮红外发射管的电路在这里，发射管发射红外光，接收管接收红外光，模拟电压就表示的是接收光的强度。

这个模块这里，电位器是直接换成了两个电阻进行分压，这样是数字输出就是固定阈值的二值化了。这个模块通常用来检测通断，所以阈值也不需要过多的调整。

最后一个模块也是一个红外发射管和接收管。只不过它是向下发射红外光，然后检测反射光的，这个可以用来做寻迹小车。

## 按键和传感器的硬件电路分析

接下来我们就来看一下按键和传感器模块的硬件电路。首先按键这里我给了4种接法，上面两个是下接按键的方式，下面两个是上接按键的方式。一般来说我们的按键都是用上两种方式，也就是下接的方式。这个原因跟LED的接法类似，是电路设计的习惯和规范。

图：按键和传感器的硬件电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-15.png" alt = "P7-GPIO输入-15.png" width = "70%" height = "auto">
</div>
<br>

### 按键的硬件电路分析

#### 按键的硬件电路-下接按键-情况1

第一排第一个图

我们先来看一下第一个图，这种接法是按键的最常用的接法了。在这里随便选取一个GPIO口，比如PA0，然后通过K1接到地。

当按键按下时，PA0被直接下达到GND，此时读取PA0口的电压就是低电瓶。当按键松手时，PA0被悬空，悬空会出现什么情况呢？就是引脚的电压不确定，对吧？

所以在这种接法下，必须要求PA0是上拉输入的模式，否则就会出现引脚电压不确定的错误现象。如果PA0是上拉输入的模式，那我们之前讲了引脚在悬空，PA0就是高电平。

所以这种方式下，按下按键，引脚为低电平，松手引脚为高电平。

#### 按键的硬件电路-下接按键-情况2

第一排第二个图

接着再看一下第二个图，相比较第一个图，在这里外部接了一个上拉电阻，这个意思大家就清楚了。

这个上拉，大家可以想象成一个弹簧，把这个端口往屋顶上拉。

当按键松手时，引脚由于上拉作用，自然保持为高电平。当按键按下时，引脚直接接到GND，也就是一股无穷大的力把这个引脚往下拉，那弹簧肯定对抗不了无穷大的力，所以引脚就为低电平。这种状态下引脚不会出现悬空状态。

所以此时PA0引脚可以配置为浮空输入或者上拉输入。

如果是上拉输入，那就是内外两个上拉电阻共同作用的，这时高电平就会更强一些，对应高电平就更加稳定。当然这样的话，当引脚被强行拉到低时，损耗也就会大一些。

#### 按键的硬件电路-上接按键-情况1

第二排第一个图

那接着第三个图，PA0通过按键接到3.3V，这样也是可以的，不过要求PA0必须要配置成下拉输入的模式。当按键按下时，引脚为高电平，松手时引脚回到默认值低电平，这要求单片机的引脚可以配置为下拉输入的模式。

一般单片机可能不一定有下拉输入的模式，所以最好还是用上面的接法，下面的作为扩展部分，大家了解一下即可。也就是最后一种接法。

#### 按键的硬件电路-上接按键-情况2(不要求掌握)

第二排第二个图

最后一种接法。就是在刚才的这种接法下面再外接一个下拉电阻。这个接法PA0需要配置为下拉输入模式或者浮空输入模式，至于分析，大家应该已经清楚了，我就不再细讲了。

#### 总结

总结一下，就是上面这两种接法按键按下时，引角是低电平，松手是高电瓶。下面这两种接法按键按下时是高电平，松手是低电平。左边两种接法必须要求引脚是上拉或者下拉输入的模式，右边两种接法可以允许引脚是浮空输入的模式。因为已经外置了上拉电阻和下拉电阻，一般我们都用上面两种接法，下面两种接法用的比较少。

那到这里按键的硬件电路就介绍完了。

### 传感器的硬件电路分析

再看一下传感器模块的电路。因为是使用模块的方案，所以电路还是非常简单的。

图：传感器模块的硬件电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-16.png" alt = "P7-GPIO输入-16.png" width = "70%" height = "auto">
</div>
<br>

这里VCC接3.3V，GND接GND，用于供电，DO数字输出随便接一个端口，比如PA0用于读取数字量。AO模拟输出，我们之后学ADC模数转换器的时候再用，现在还是不用接了。

好，我们本节的主要内容到这里就差不多了，接着就是C语言部分的学习了。

## C语言学习

### C语言数据类型

首先是C语言数据类型，这个C语言数据类型是个很基础的东西，大家刚学C语言就应该接触到这些东西了。我把这个列出来，主要是提几个注意事项。

图：C语言数据类型

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-17.png" alt = "P7-GPIO输入-17.png" width = "70%" height = "auto">
</div>
<br>

我们看一下这个表，左边这三列就是C语言数据类型的关键字、所占位数和数的表示范围的。

比如char符号，字符型、占八位，可以表示-128到127的整数。unsigned char 是无符号字符型、占八位，表示0到255。

然后下面就是short和unsigned short，占16位。int 和 unsigned int 是占32位的，这个需要注意一下。在51单片机中，int是占16位的，而在STM32中，int是占32位的。如果要用16位的数据，要用short来表示，这是和51单片机的不同之处，不要混淆了这个。

然后下面还有这个 long 和 unsigned long 占用的也是32位，跟int是一样的。所以别想着 chair short long 这样排下来，long 应该是64位的，但其实还是32位的。

如果想用64位的，需要用到 long long 和 unsigned long long这个数据类型。不过这个数实在是太大了，我们用的比较少。

然后是float和double，这些是用来存小数的。float和double都是带符号的数，没有符号的float和double。

#### ST给C语言数据类型重命名了

然后我们看一下右边，在这里写的是C语言stdint.h文件和ST对这些变量的重命名。

因为左边这些名字比较长，而且这个int的位数根据系统的不同还有可能不一样。还有就是这些名字有时候会名不对题，比如这个char本意是字符型数据的意思，按名字来说它就应该存放字符的，但是我们单片机通常用它来存放整数而不是字符。

所以综上各种原因，C语言和ST就给这些变量换了个名字。

C语言提供的有stdint这个头文件，使用了新的名字。

**char ➜ int8_t**

比如`int8_t`就是`char`的新名字，表示的意思就是8位整形数据。右边加个`_t`表示这是用 typedef 重新命名的变量类型。

typedef 是用来给变量类型重命名的，这个等会儿就讲。

然后这里 `unsigned char` 的新名字就是 `unit8_t` 意思是无符号8位整形数据。

然后`int16_t`、`uint16_t`、`int32_t`、`unit32_t`、`int64_t`、`unit64_t` 这些通过名字就知道意思了，就是16位整形、32位整型、64位整型的意思。

我们以后在写程序的时候，就会按照他的推荐，使用这些新的名字。大家要知道，他们代表的就是前面的这些数据类型，就是换个名字而已。

我们打开这个程序来看一下，这里先切回到蜂鸣器的程序，然后在gpio.h里找一下，这其中就出现了这些`unit8_t`、`unit16_t`。

图：gpio.h

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-18.png" alt = "P7-GPIO输入-18.png" width = "70%" height = "auto">
</div>
<br>

另外这里我还列举出了这个ST定义的名字，像`u8`、`u16`这些，这是ST库函数以前用的名字。比如我打开库函数手册，这个手册是老版本的，所以还能看到这个`u8`、`u16`这些。

图：老版本的库函数手册还有u8

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-19.png" alt = "P7-GPIO输入-19.png" width = "70%" height = "auto">
</div>
<br>

那在新版库函数这里已经换成了`uint8_t`和`uint16_t`这样的，不过这个新版本库函数仍然支持`u8`、`u16`这样的写法，比如我写一个`u8`，然后跳到定义。在这里可以看到，它就是把`uint8_t`又改了个名字叫`u8`而已。

图：把`uint8_t`又改了个名字叫`u8`而已

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-20.png" alt = "P7-GPIO输入-20.png" width = "70%" height = "auto">
</div>
<br>


然后`unit8_t`再转到定义，可以看到这个`uint8_t`就是`unsigned char`改个名字。

图：`uint8_t`就是`unsigned char`改个名字

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-21.png" alt = "P7-GPIO输入-21.png" width = "70%" height = "auto">
</div>
<br>

不过这个u8定义这里上面有注释，写了这是库函数老的类型，是为了传统的目的而保留的，说白了就是为了兼容老版本。

图：u8定义这里上面有注释，写了这是库函数老的类型

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-22.png" alt = "P7-GPIO输入-22.png" width = "70%" height = "auto">
</div>
<br>

所以目前我推荐大家使用stdint关键字这一列的数据类型定义，这是新版本库函数使用的方式，也是C语言stdint.h头文件里提供的官方定义。

### 宏定义

- 关键字：#define
- 用途：用一个字符串代替一个数字，便于理解，防止出错；提取程序中经常出现的参数，便于快速修改
- 定义宏定义：
```c
#define ABC 12345
```

- 引用宏定义：
```c
int a = ABC;	//等效于int a = 12345;
```

接下来我们再来看一下C语言中的宏定义，这个比较简单，我们在51单片机中都遇到过。

这个宏定义的关键字是`#define`，它的用途是用一个字符串代替一个数字，便于理解，防止出错。

比如我们在程序中经常用1代表高电平，0代表低电平，这个还算好理解啊，但是如果说1代表上拉输入，2代表下拉输入，3代表浮空输入等等，这时直接用数字来表示就非常麻烦了，是吧？那我们就可以用宏定义将数据参数映射到一个字符串上，这样就比较好理解。

然后第二个用途是提取程序中经常出现的参数，便于快速修改。

比如我们写程序里面出现了10个GPIO_Pin_0，这个Pin0是需要经常修改的，那如果一个个修改就太不方便了是吧？

这时候我们就可以用一个字符串来代替GPIO_Pin_0，然后需要修改的时候只需要修改一下定义即可。这些就是宏定义通常的用途。

定义宏定义：
```c
#define ABC 12345
```

引用宏定义：
```c
int a = ABC;	//等效于int a = 12345;
```


接现在我们看一下定义宏定义，这里写的是`#define ABC 12345`，然后这个意思就是用ABC这个字符串替代12345这个参数。我们引用这个宏定义是直接写`int a = ABC`，那它就等效于`int a = 12345`这个意思。

那我们打开程序可以看一下对不对？这个`GPIO_Pin_12`其实就是一个宏定义字符串。

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-23.png" alt = "P7-GPIO输入-23.png" width = "70%" height = "auto">
</div>
<br>

我们跳转到定义，可以看到，`GPIO_Pin_12`替换的是0x1000这个数据，左边这里是一个强制类型转换，是为了严谨性考虑的，我们暂时不用管的。那在这里就是0x1000表示12号口，这个是不容易理解的。

图：`GPIO_Pin_12`的宏定义

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-24.png" alt = "P7-GPIO输入-24.png" width = "70%" height = "auto">
</div>
<br>

所以我们用宏定义将12号口的数据改一下，名字叫`GPIO_Pin_12`，这样就比较好理解。那在这里写`GPIO_Pin_12`就等效于在这里我们把这个`((uint16_t)0x1000)` 复制下来，然后粘贴到这里。这是程序实际执行的内容。

```c
GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;
```

等效于

```c
GPIO_InitStructure.GPIO_Pin = ((uint16_t)0x1000);
```

我们来看看这个代码里的几处宏定义

```c
int main()
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);	//开启GPIOB的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;				//GPIO引脚，赋值为第12号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOB, &GPIO_InitStructure);

	while(1)
	{}
}


```

但这样不便理解，所以我们就使用了宏定义进行改名。

剩下的还有这个`RCC_APB2Periph_GPIOB`也是一个宏定义的替换，

图：`RCC_APB2Periph_GPIOB`宏定义

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-25.png" alt = "P7-GPIO输入-25.png" width = "70%" height = "auto">
</div>
<br>

还有这个`GPIOB`也是宏定义的替换，这些就是库函数中宏定义的用法。当然宏定义还有其他的很多用法，以后遇到再说，在这里就不再细说了。

图：`GPIOB`的宏定义

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-26.png" alt = "P7-GPIO输入-26.png" width = "70%" height = "auto">
</div>
<br>

### typedef

- 关键字：typedef
- 用途：将一个比较长的变量类型名换个名字，便于使用
- 定义typedef：
```c
typedef unsigned char uint8_t;
```
- 引用typedef：
```c
uint8_t a;	//等效于unsigned char a;
```

接下来是`typedef`，它的关键字就是`typedef`，它的用途和宏定义的用途差不多。它是将一个比较长的变量类型换一个名字便于使用，说白了也是换名字的一个语句。

这个`typedef`和宏定义有哪些区别呢？

- 首先宏定义的新名字在左边，`typedef`的新名字在右边。
- 然后是宏定义不需要分号，`typedef`后面必须加分号。
- 还有就是宏定义任何名字都可以换，而`typedef`只能专门给变量类型换名字，所以宏定义的改名范围要更宽一些。只不过对于变量类型重命名而言，使用`typedef`更加安全。

因为宏定义只是无脑改名，管你对不对。而`typedef`会对命名进行检查，如果不是变量类型的名字那是不行的。所以给变量类型重命名，我们一般用`typedef`。

比如`typedef unsigned char uint8_t;`把`unsigned char`重命名为`uint8_t`然后在程序中直接写`uint8_t`那它就等效于`unsigned char。`

当然原来的名字`unsigned char`还是能用的，这里的重命名只是增加了一个新名字，原名字也是可以用的。在程序中，我们这里的`uint8_t`、`uint16_t`等等，都是用`typedef`重命名的数据类型的。

### 结构体

- 关键字：struct
- 用途：数据打包，不同类型变量的集合
- 定义结构体变量：
```c
struct{char x; int y; float z;} StructName;
```
因为结构体变量类型较长，所以通常用typedef更改变量类型名
- 引用结构体成员：
```c
StructName.x = 'A';
StructName.y = 66;
StructName.z = 1.23;
```

或

```c
pStructName->x = 'A';	//pStructName为结构体的地址	
pStructName->y = 66;
pStructName->z = 1.23;
```

接下来我们就来介绍一下C语言的结构体，这个在库函数中出现的频率也是非常高的。

要理解库函数的运作逻辑，理解结构体那是非常必要的那在C语言中结构体是用来干什么的呢？其实结构体也是一种数据类型，比如char、short、int等，这些我们可以称作基本数据类型。然后数组就是一大堆基本数据类型的集合。然后比如定义`char a[10]`，这就是十个char型数据的集合，对吧？

那数组我们就可以称作组合数据类型，它是由许多基本数据类型组合而来的。数组组合的只能是相同的数据。比如这里就是十个char型数据的组合，或者`int b[20]`就是20个int型数据的组合。

数组只能组合相同类型的数据。如果我们想组合不同的数据类型该怎么办呢？于是C语言的结构体就出现了。

结构体也是一种组合数据类型，它的作用就是组合不同的数据类型。我们看一下它的用途，就是数据打包。

不同类型变量的集合，在一个复杂的程序里，用结构体将一些数据打包起来，将有利于我们管理或者传递这些数据，并且有利于程序员的理解。

#### 演示结构体的使用方法

接下来我将来演示一下结构体的使用方法。

对于C语言的数据来说，主要就是两个功能，一个是定义数据，一个是引用数据。既然结构体也是数据类型，那它就应该和其他数据类型差不多，也分为定义和使用。

首先定义数据，比如`int a`，这是定义一个`int`型的数据，名字叫a对吧？这个很简单。

```c
int a;
```

然后是数组，比如`int b[5]`，这是定义一个5个`int`型数据的数组，名字叫b这个大家也很熟悉。

```c
int b[5];
```

画个内存图就是，外面是一个打包的数组，名字叫b，里面连续存放了5个`int`型的数据，编号依次为01234，这就是数组。

图：数组内存图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-27.png" alt = "P7-GPIO输入-27.png" width = "70%" height = "auto">
</div>
<br>

那接下来就是结构体，我们就可以写`struct c`这就是定义一个结构体，名字叫c简单吧？

那当然是不可能这么简单的。既然是不同类型数据的组合，我们就得够告诉他是哪些数据的组合，对吧？

所以我们还得在这里加上一个附加说明，告诉他是哪些数据。我们要在`struct`的后面加上一对花括号，然后告诉他我们要打包哪些变量，比如`char x;` `int y;` `float z;`，这样才是结构体的完整定义。

```c
struct c
{
	char x;
	int y;
	float z;
};
```

这一句的意思就是定义一个结构体变量，名字叫c，其中包含了`char`型的x `int`型的y，和`float`型的z，3个子项。

画个内存图就是外面是一个打包的结构体叫c，里面放了三个变量，第一个是`char`型叫x，第二个是`int`型叫y，第三个是`float`型叫z，这就是结构体的定义。

图：结构体的内存图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P7-GPIO%E8%BE%93%E5%85%A5-28.png" alt = "P7-GPIO输入-28.png" width = "70%" height = "auto">
</div>
<br>

接着定义完了就要开始引用了，对吧？那`int a`引用就直接写，比如`a = 66`，这个直接调用名字就可以引用了，这个最简单。

然后就是数组的引用。数组的引用是数组名b，加上方括号取索引。比如数组的第0个元素`b[0] = 66`，这就是数组的引用。

最后就是结构体的引用了，结构体如何引用呢？我们需要写结构体名字c然后用.运算符取索引，这里的索引就不是0、1、2、3、4这样的，而是结构体子项的名字，比如c.x它等于一个字符A，`c.x = 'A'`，然后c.y它等于一个数66`c.y = 66`，c.z它等于一个小数1.23`c.z = 1.23`，这就是结构体变量的引用格式，就是结构体名称加上点，然后结构体子项的名称，来引用结构体的成员。

```c
struct c
{
	char x;
	int y;
	float z;
};

c.x = 'A';
c.y = 66;
c.z = 1.23;
```

那我们编译一下，可以看到这样是没问题的。接着我们再打印一下，看看数据到底对不对。

```c
int main()
{
	int a;
	a = 66;
	printf("a = %d\n", a);
	
	int b[5];
	b[1] = 66;
	b[2] = 77;
	b[3] = 88;
	printf("b[0] = %d\n",b[0]);
	printf("b[1] = %d\n",b[1]);
	printf("b[2] = %d\n",b[2]);

	struct c
	{
		char x;
		int y;
		float z;
	};

	c.x = 'A';
	c.y = 66;
	c.z = 1.23;

	printf("c.x = %c\n",c.x);
	printf("c.y = %d\n",c.y);
	printf("c.z = %f\n",c.z);
}
```


到这里类比基本变量和数组来理解结构体应该还算简单。接下来我就来介绍结构体的特殊用法了。

#### 结构体的特殊用法

首先第一个问题就是这个结构体的名字`struct c{char x;int y;float z;};`
太长了。这里如果想再定义一个结构体d的话，还得复制这一长串，然后再定义d，`struct d{char x;int y;float z;};`

```c
struct c
{
	char x;
	int y;
	float z;
};
struct d
{
	char x;
	int y;
	float z;
};
```

每次都写这么一大串，太麻烦了是吧？所以我们刚才讲的`typedef`的作用就显现出来了。

我们可以在这里写上`typedef`，把这一大串名字`struct c{char x;int y;float z;};`拿过来，然后起个新的名字，比如`StructName_t`。

```c
typedef struct c{char x;int y;float z;} StructName_t;
StructName_t c;
StructName_t d;
```

当给出来这个新名字之后，以后我们再定义这个结构体的时候，就可以直接用`StructName_t`换掉原来的那一长串，这样看上去就简单多了。

这里的`struct`包括花括号在内的是一个结构体的数据类型，然后是`typedef`，将结构体换个名字叫`StructName_t`。

```c
typedef struct c
{
	char x;
	int y;
	float z;
} StructName_t;
```

下面这里的这一个就是结构体的数据类型，然后后面跟着的是结构体变量的名字。

```c
StructName_t c;
```

那我们直接使用结构体变量的名字，然后用点来引出结构体成员的数据，这样就可以进行数据的写入和读取了。

```c
c.x = 'A';
c.y = 66;
c.z = 1.23;
```

学到这里，我们再来看一下库函数中的用法。

#### 结构体在库函数中的应用

```c
int main()
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);	//开启GPIOB的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;				//GPIO引脚，赋值为第12号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOB, &GPIO_InitStructure);

	while(1)
	{}
}

```

这一句是定义结构体类型的数据，数据名称叫`GPIO_InitStructure`。左边是结构体类型名称，我们转到定义这里，是不是就明白了。

代码：`GPIO_InitTypeDef`的定义

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

Struct包括花括号，这里是结构体数据类型，然后用`typedef`将这个类型换一个新名字，叫`GPIO_InitTypeDef` 是不是和我们演示的这里基本一样。

然后引用结构体成员，

```c
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
```

这里是不是也是结构体变量名，加点，然后结构体成员名这样的形式，右边就是赋值的数据了。

```c
//引用结构体成员：
	StructName.x = 'A';
	StructName.y = 66;
	StructName.z = 1.23;
//或	
	pStructName->x = 'A';	//pStructName为结构体的地址	
	pStructName->y = 66;
	pStructName->z = 1.23;
```

但是最后这里又多了一种引用方式，就是结构体的首地址。也就是结构体指针加上->这个运算符，再加上结构体成员名，这样也可以引用结构体成员。所以说结构体成员的引用方式有两种，一种是结构体变量名，点，结构体成员名，另一种是结构体指针名，->，结构体成员名。

为什么要加一种结构体指针的引用方式呢？

这是因为结构体是一种组合数据类型，在函数之间的数据传递中，通常用的是地址传递，而不是值传递。

有关地址传递、值传递还有指针的相关知识点，可以看一下我的指针教程。我在那个视频里把这两种传递方式的用法和利弊都讲了。

那我们回到这个问题来，既然是使用指针传递，子函数得到的就是结构体的首地址。这时候我们就可以用->这个运算符快速的引用结构体成员。

我们打开STM32程序，这里可以看到这个结构体变量在传递给`GPIO_Init`的函数时，传递的是结构体的地址，

```c
int main()
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);	//开启GPIOB的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;				//GPIO引脚，赋值为第12号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOB, &GPIO_InitStructure);

	while(1)
	{}
}
```

然后对应`GPIO_Init`的函数里，这里就是用结构体指针来接收的。

代码：gpio.c中 `GPIO_Init`的函数定义

```c
void GPIO_Init(GPIO_TypeDef* GPIOx, GPIO_InitTypeDef* GPIO_InitStruct)
{
    currentmode = ((uint32_t)GPIO_InitStruct->GPIO_Mode) & ((uint32_t)0x0F);
}
```

然后里面再引用结构体成员时，就可以直接使用->这个符号来引用。当然这里你直接用星号引出指针变量的内容，再用点来引用结构体成员也是可以的，只不过用->这个运算符更加方便而已。

回到主函数里面，我们可以看出来，这个结构体就是一个数据打包的过程。首先将参数写到结构体的这三个变量里，然后统一打包将结构体传递到函数里，这个函数里面再把这个结构体拆包出来读取变量，这就是使用结构体的整个过程。

你可能会想，为什么要这样麻烦呢？直接在这里多定义几个参数不就行了，这个确实没问题。但当这个函数需要十个八个参数时，参数就太多了不方便管理，所以这里就统一使用了结构体的传参方式，我们也只能按照他的安排来写了，是吧？

### 枚举

- 关键字：enum
- 用途：定义一个取值受限制的整型变量，用于限制变量取值范围；宏定义的集合
- 定义枚举变量：
```c
enum{FALSE = 0, TRUE = 1} EnumName;
```
因为枚举变量类型较长，所以通常用typedef更改变量类型名
- 引用枚举成员：
```c
EnumName = FALSE;
EnumName = TRUE;
```

那接下来最后一个就是C语言的枚举了。首先，枚举的关键词是这个，`enum`这个枚举跟结构体差不多，也是一种数据类型。

下面可以看到枚举的用途是定义一个取值受限制的整形变量量，用于限制变量取值范围。比如我们定义一个变量用来存储星期的值，那理论上这个变量只能取1到7的值，对吧？

但是如果你定义的是整形变量，那这个变量随意存什么都行，不会受到限制。这时可能会出现数据不合法，比如星期八的情况出现。所以这个时候如果你想要程序更加安全一些，就可以定义一个取值受限制的整形变量。这个变量就是枚举。

另外这个枚举变量也可以当做一个宏定义的集合，这个我们等会儿再看。

首先枚举也是一种数据类型是吧？那也是两个问题，定义和引用，定义和结构体差不多。这里写`enum`，然后是变量名字，比如week，接着同样需要在`enum`后面加上花括号来指定这个变量可以有哪些取值。比如MONDAY=1一，这里和结构体不一样，需要用逗号隔开。然后TUESDAY=2，然后等等我就不写了，这样就可以限制week这个变量的取值范围只能取在花括号里面的值。

```c
enum
{
	MONDAY = 1,
	TUESDAY = 2,
	WEDNESDAY = 3,
}week;
```

另外这里面的定义，如果数是按照顺序累加的那后面的赋值可以省略。

```C
enum
{
	MONDAY = 1,
	TUESDAY,
	WEDNESDAY,
}
```

比如这个=2，=3，因为是顺序的数可以去掉，编译器会自动填上顺序值的。同样这个变量类型名比较长，我们可以用`typedef`改一下名。

那就这样写，`typedef`，枚举类型，然后枚举新名字，比如叫Week_t。下面这里就可以直接用Week_t这样就定义为好了。

```c
typedef enum
{
	MONDAY = 1,
	TUESDAY,
	WEDNESDAY,
}Week_t;

Week_t week;
```

然后是引用。当给它赋值时，只能写`week = MONDAY`，这就等效于`week = 1`，这个赋值只能按枚举中的定义来，如果你写`week = 8`，那编译器就会报警告说你枚举中混入了其他变量。不过我这个编译器这样编译也没有提出警告，可能这个编译器比较宽容。

#### 枚举在Keil工程中的应用

那在Keil软件中要是这样写的话，就会报警告了。比如我们试一下这个`ENABLE`就是一个枚举值。我们转的定义可以看到它是一个这样的，没有句号。

```c
int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);	//开启GPIOB的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
	
	}
}
```

代码：转到枚举类型的定义

```c
typedef enum {DISABLE = 0, ENABLE = !DISABLE} FunctionalState;
```

我们复制一下枚举类型名试一下。在这里定义一个枚举变量a，然后赋值`a = ENABLE`或者`a = DISABLE`，`a = 100`，编译一下。就是1个警告。

```c
FunctionalState a;
a = ENABLE;
a = DISABLE;
a = 100;
```

说的就是枚举中混入了其他类型，这就是枚举的作用。只能在他给定的参数列表里赋值，不能赋其他的值。

不过在这里我赋值`a = 0`，看一下。他也不行，但是0确实是枚举中的值，这个我感觉有点不人性化。虽然0也是枚举中的值，但它只能赋值这个字符串，不能直接给0。如果想赋值0，还得进行一下类型强转把它转化为对应的枚举类型，这样才没有问题。这就是枚举的用法了。

```c
FunctionalState a;
a = ENABLE;
a = DISABLE;
a = 100;
a = 0;
```

在这个工程里，比如这个ENABLE就是枚举变量，还有下面的`GPIO_Mode`看一下，也是枚举变量，

```C
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
```

这个`GPIO_Mode`也是枚举。

```c
typedef enum
{
  GPIO_Mode_AIN = 0x0,
  GPIO_Mode_IN_FLOATING = 0x04,
  GPIO_Mode_IPD = 0x28,
  GPIO_Mode_IPU = 0x48,
  GPIO_Mode_Out_OD = 0x14,
  GPIO_Mode_Out_PP = 0x10,
  GPIO_Mode_AF_OD = 0x1C,
  GPIO_Mode_AF_PP = 0x18
}GPIOMode_TypeDef;
```

这个`GPIO_Speed`也是枚举。

```c
GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
```

```c
typedef enum
{ 
  GPIO_Speed_10MHz = 1,
  GPIO_Speed_2MHz, 
  GPIO_Speed_50MHz
}GPIOSpeed_TypeDef;
```

另外这个枚举值也不是必须赋值给枚举变量的。我们可以随意定一个变量，把枚举值赋给他都行。比如我这里写`int a = ENABLE`，编译看一下。这里只有个定义却没使用的警告，说明这也是没有问题的。

这样枚举中的定义就和宏定义差不多了。所以PPT的这里说枚举也是一个宏定义的集合，就是这个意思。

好，到此本小节的内容就已经全部讲完了。我们讲了宏定义、`typedef`、结构体、枚举这些东西。实际上这些东西都不是C语言最根本的语法，没有这些东西，C语言照样可以完成它的功能。

这些东西创造出来是为了我们编程人员更好的管理我们的工程。虽然表面上这些操作感觉是多此一举，但是当工程复杂起来之后，使用这些新的知识将使我们的编程更加的便捷，更容易理解，更不容易出错，所以大家现在需要学习一下这些知识点，在我们之后的STM32编程中也会不断的和他们打交道。那本小节就到这里，我们下一小节继续来学习GPIO输入的代码部分。

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