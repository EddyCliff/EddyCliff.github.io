---
title: "嵌入式开发-STM32标准库学习：OLED显示屏"
date: 2024-05-13T00:17:58+06:00
lastmod: 2024-05-13T00:17:58+06:00
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
- OLED
- OLED显示屏
- STM32标准库开发
- 嵌入式开发
- STM32F1
description: "本博客主要讲述了如何将OLED显示屏驱动程序模块整合至STM32工程中，并详细阐述了该驱动程序的设计与实现细节。重点讲解了硬件连接方式，尤其是如何通过PB6和PB7端口为OLED屏幕提供电源并通过四针脚接口进行数据传输。同时，介绍了如何根据现有的工程文件创建新的项目，并在其中嵌入OLED显示屏的驱动代码。文档还探讨了利用特定软件的调试模式对程序进行分析和优化的方法，涵盖了在线仿真和计算机模拟两种调试方式，及其对应的启动调试步骤和常用调试命令。此外，文中强调了通过这些调试手段可以有效地诊断和修复程序中的错误，加深对程序执行流程的理解。最后，文档简要介绍了 Keil 软件中的调试模式和相关功能，如命令窗口、反汇编窗口和符号窗口的使用方法，为进一步的程序调试提供了工具和技巧。"
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

本小节我们来学习一下如何使用OLED显示屏的驱动函数模块。

首先我们还是看一下接线图，我们打开4-1 OLED显示屏的图片看一下。

图：接线图 OLED显示屏

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F.jpg" alt = "P10-OLED显示屏.jpg" width = "70%" height = "auto">
</div>
<br>

这里我把OLED插在了面包板的右下角，以后我们就一直把这个屏幕插在这里，需要用的时候随时可以使用，而且放在右下角也不是很占地方。

......具体的连接 省略一万字

图：接线 实物图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F.png" alt = "P10-OLED显示屏.png" width = "70%" height = "auto">
</div>
<br>

## 编写OLED显示屏 Keil工程 

然后我们回到工程文件夹，复制一下按键控制led的工程文件夹，改个名字叫4-1 OLED显示屏。打开工程。

接着我们回到工程文件夹里，我给大家准备的OLED驱动函数模块就放到这个1-4的文件夹里了。大家可以下载程序源码查看，那我们打开这个文件夹里面有两个版本，一个是4针脚I2C版本，另一个是7针脚SPI版本。

我们打开4针脚版本的文件夹，里面有三个文件，我们直接全部选中复制到OLED显示屏这个工程里来。

### OLED.c

我们打开分别看一下这三个文件都是什么东西。首先OLED.c里面就是函数的主体代码了，里面包括了引脚配置、引脚初始化、I2C通信的基本时序和OLED用户调用的代码等。

这些函数我都已经写好了，绝大部分都不需要更改。我们需要更改的就只有上面的这两部分代码。

第一部分是引脚配置，这里选择的是你硬件电路上把SCL和SDA这两个引脚接在了哪两个端口上。比如我这里SCL接在了PB8，那这个地方就是GPIOB、GPIO_Pin_8。

```c
/*引脚配置*/
#define OLED_W_SCL(x)		GPIO_WriteBit(GPIOB, GPIO_Pin_8, (BitAction)(x))
#define OLED_W_SDA(x)		GPIO_WriteBit(GPIOB, GPIO_Pin_9, (BitAction)(x))
```

如果你换个端口，比如接在PA6上，那这个地方就要改成GPIOA，GPIO_Pin_6。下面这个SDA的引脚配置也是一样，SDA接在了哪个位置，就改成GPIO啥，GPIO_Pin_啥。

然后下面这个，OLED_I2C_Init函数里也得更改。这里面就是把SCL和SGA的两个引脚都初始化为开漏输出模式。这部分代码大家学了上一节GPIO之后应该就已经非常熟悉了。具体更改就是使用到的GPIO外设都先用RCC开启一下时钟，然后下面初始化GPIOB的Pin8，再初始化GPIOB的Pin9，这样就完成了。

所以对于这个模块来说，我这里默认用的是SCL接PB8，SDA接PB9。

如果你想修改，那先把上面这两行引角配置改一下。

再把下面这里的端口初始化改一下。剩下的都不需要修改，就可以直接使用我这个OLED驱动函数模块了。这就是OLED.c里面的东西。

编写代码：需要修改的Hardware/OLED.c中的代码

```c
#include "stm32f10x.h"
#include "OLED_Font.h"

/*引脚配置*/
#define OLED_W_SCL(x)		GPIO_WriteBit(GPIOB, GPIO_Pin_8, (BitAction)(x))
#define OLED_W_SDA(x)		GPIO_WriteBit(GPIOB, GPIO_Pin_9, (BitAction)(x))

/*引脚初始化*/
void OLED_I2C_Init(void)
{
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);
	
	GPIO_InitTypeDef GPIO_InitStructure;
 	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_8;
 	GPIO_Init(GPIOB, &GPIO_InitStructure);
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;
 	GPIO_Init(GPIOB, &GPIO_InitStructure);
	
	OLED_W_SCL(1);
	OLED_W_SDA(1);
}

```

### OLED.h

接下来OLED.h里面，这里就是外部可调用函数的声明，其他的也没啥了。

### OLED_Font.h

最后是OLED_Font.h文件，这里存的是OLED的字库数据，因为这个OLED显示屏是不带字库的，想要显示字符图形，还得先定义字符的点阵数据。

那这里就是这些字符的点阵数据，也就是字库OLED.c文件的显示函数会用到这些数据，当然字库也是不用我们修改的。

### main.c

编写main.c，上面先引入头文件。

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "OLED.h"
```

main函数里主循环之前先调用OLED_Init();初始化OLED。

**OLED_ShowChar()**

然后试一下OLED_ShowChar显示一个字符。先写行列坐标，坐标的定义是这样的，总共4行16列。

那我们写1行1列。第三个参数写一个字符，字符需要用单引号括起来，然后A，这样就完成了，我们试一下编译。下载。

```c
OLED_ShowChar(1, 1, 'A');				//1行1列显示字符A
```

可以看到OLED的1行1列显示了一个字符A。

图：OLED的1行1列显示了一个字符A


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-1.png" alt = "P10-OLED显示屏-1.png" width = "70%" height = "auto">
</div>
<br>

**OLED_ShowString()**

然后OLED_ShowString显示字符串，1行3列显示字符串，字符串需要用双引号括起来，然后hello word!再看一下。

```c
OLED_ShowString(1, 3, "HelloWorld!");	//1行3列显示字符串HelloWorld!
```

图：OLED的1行3列显示字符串

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-2.png" alt = "P10-OLED显示屏-2.png" width = "70%" height = "auto">
</div>
<br>

这里注意计算一下坐标和字符串长度，不要让它超出屏幕，这就是显示字符串。

**OLED_ShowNum()**

然后OLED_ShowNum()显示无符号十进制数字，在2行1列，显示12345这个数字，长度为5，看一下。

```c
OLED_ShowNum(2, 1, 12345, 5);			//2行1列显示十进制数字12345，长度为5
```

就是这个效果。

图：在2行1列，显示12345这个数字

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-3.png" alt = "P10-OLED显示屏-3.png" width = "70%" height = "auto">
</div>
<br>

如果你这个长度参数比数字长度长，比如6，那它就会在前面补0。

```c
OLED_ShowNum(2, 1, 12345, 6);			//2行1列显示十进制数字12345，长度为6
```

像这样

图：OLED_ShowNum 长度参数比数字长度长，那它就会在前面补0

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-4.png" alt = "P10-OLED显示屏-4.png" width = "70%" height = "auto">
</div>
<br>

如果这个长度参数比数字长度小，那比如4，它就会把高位的数据切掉。

```c
OLED_ShowNum(2, 1, 12345, 4);			//2行1列显示十进制数字12345，长度为4
```

像这样

图：OLED_ShowNum 长度参数比数字长度小，它就会把高位的数据切掉

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-5.png" alt = "P10-OLED显示屏-5.png" width = "70%" height = "auto">
</div>
<br>

最高位的1就没有了，这就是显示无符号十进制数字，这个只能显示无符号数。

**OLED_ShowSignedNum**

然后OLED_ShowSignedNum，这个是显示有符号十进制数字的函数。比如2行7列，显示12345，长度为5，编译一下。

```c
OLED_ShowSignedNum(2, 7, 12345, 5);		//2行7列显示有符号十进制数字12345，长度为5
```

下载。

图：OLED_ShowSignedNum 2行7列，显示12345

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-6.png" alt = "P10-OLED显示屏-6.png" width = "70%" height = "auto">
</div>
<br>

那现象就是这样，数字的前面会带一个正负号。

如果显示-66长度为2，再看一下。现象就是这样。

图：OLED_ShowSignedNum 显示-66长度为2


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-7.png" alt = "P10-OLED显示屏-7.png" width = "70%" height = "auto">
</div>
<br>

**OLED_ShowHexNum()**

然后是OLED_ShowHexNum()，显示十六进制数。比如3行1列显示0xAA55长度为4，看一下。

```c
OLED_ShowBinNum(3, 1, 0xAA55, 4);		//3行1列显示二进制数字0xAA55，长度为4
											//C语言无法直接写出二进制数字，故需要用十六进制表示
```

图：OLED_ShowHexNum() 3行1列显示0xAA55长度为4

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-8.png" alt = "P10-OLED显示屏-8.png" width = "70%" height = "auto">
</div>
<br>

**OLED_ShowBinNum()**

显示二进制数

```c
OLED_ShowBinNum(4, 1, 0xAA55, 16);		//4行1列显示二进制数字0xA5A5，长度为16
											//C语言无法直接写出二进制数字，故需要用十六进制表示
	
```

显示二进制数，这里写4行1列显示0xAA55这个数。因为C语言不能直接写二进制的数，所以这里我们只能用十六进制来代替。这个我还是比较奇怪的，为什么C语言作为一个底层计算机编程语言，却不支持直接写二进制的数呢？况且C语言最终还是要翻译成汇编语言的，汇编就支持直接写二进制，C语言却不支持。

所以说二进制比较长，一般都用十六进制来表示。但难免有的时候直接写二进制会更加方便，C语言里面直接就不支持写二进制，那咱也没办法，这里就只能用十六进制来写了。这个十六进制表示的二进制数是16位的，所以这个长度给16。

编译，下载，看一下。这里就显示的AA55对应的二进制数据。

图：OLED_ShowBinNum() 显示的AA55对应的二进制数据

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-9.png" alt = "P10-OLED显示屏-9.png" width = "70%" height = "auto">
</div>
<br>

**OLED_Clear**

最后还有一个函数OLED_Clear清屏，我们再最后调用一下看一下。这时候OLED就不会显示任何东西，因为在最后OLED_Clear把全屏都清屏了。

```c
OLED_Clear();
```

如果你只想清除部分字符，那可以用OLED_ShowString()，在你想要清掉的地方显示空格字符就行了。

好，这些就是我这个模块里所有的函数用法。

## Keil的调试模式

接下来的时间我给大家演示一下Keil的调试模式。这个调试模式我教程里可能用的比较少，但是它确实是一个非常强大的工具。

在这里给大家介绍一下，大家之后遇到一些复杂的问题，可以考虑一下这个调试模式，说不定就能很方便的解决你的问题了。那我们换一个LED闪烁的工程作为例子。

### Debug选项选择调试模式

然后工程选项debug，这里可以对调试选项进行配置。这里默认是选择右边的这一项，这个是在硬件上在线仿真，需要我们把STLINK和STM32都连接好。如果你不想连接硬件，也可以选择左边的使用仿真器这个选项，这样就是电脑模拟STM32的运行了。

图：Debug选项

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-10.png" alt = "P10-OLED显示屏-10.png" width = "70%" height = "auto">
</div>
<br>

那我们就使用硬件的在线仿真版。首先在进入调试模式之前，需要先连接好STM32之后编译一下，确保工程没有问题。

现在这里编译是没有问题的。然后点击这里的放大镜，里面带一个D的图标，进入调试模式。

### 开始调试

图：Keil调试模式

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-11.png" alt = "P10-OLED显示屏-11.png" width = "70%" height = "auto">
</div>
<br>

在这个界面里主窗口就是我们的C语言程序，上面这个窗口就是C语言翻译成的汇编程序的。这个你感兴趣的话，可以对照这里看一下，每句C语言实际上都执行了哪些操作。

然后左边的这个窗口是寄存器组和状态标志位等信息。这个是单片机硬件底层很重要的东西，如果你用汇编编程的话，这些东西都是必须要非常清楚的。

图：编译的界面

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-13.png" alt = "P10-OLED显示屏-13.png" width = "70%" height = "auto">
</div>
<br>

但如果你用的是C语言，那这些东西就不用管的。

#### 浅尝调试模式

之后上面这一部分是程序运行控制的。第一个是复位，第二个是全速运行，第三个是停止全速运行。然后接着这四个是单步运行、跳过当前行单步运行，跳出当前函数单步运行，跳到光标指定行单步运行。

图：程序运行控制

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-12.png" alt = "P10-OLED显示屏-12.png" width = "70%" height = "auto">
</div>
<br>

那我们可以试一下，这个黄色箭头指示的就是下一句将要执行的代码。我们当前的程序就是从main函数开始的。我们可以点一下单步运行，那它就执行到了下一行，然后再单步，他就进到了RCC这个函数里面来了。点击这个跳出函数，它就可以跳到函数外面来了。指定这一行，然后点击运行到光标指令行，那程序就运行到这个位置了。

我们可以点击程序左边这里深灰色的区域，设置断点，然后点击全速运行程序就会一直运行直到断点停下。如果没有断点的话，再全速运行程序就不会自动停下来了。

那我们就需要点击这里的停止按钮，这样程序就会停下来了，可以看到目前程序停在了Delay函数里面，然后我们点击RST复位，程序就会回到最开始的地方，这里我们可以看到，现在程序是在启动文件的复位中断函数里，说明复位后程序是从这里开始执行的。

好了，这就是调试模式下控制程序运行的方法。这个方法可以精确追踪我们的程序是如何运行的。如果你不清楚程序是如何一步步运行的。那在这个调试模式里单步运行探索一下，相信你对程序的运行逻辑就会有更深的理解。当然这只是调试模式下的一小部分功能，调试模式还有更强大的功能。

图：调试模式的各种按钮

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-15.png" alt = "P10-OLED显示屏-15.png" width = "70%" height = "auto">
</div>
<br>

接着我们来看一下上面的这一堆功能。第一个是命令窗口，我们点击它可以打开和关闭命令窗口。第二个是反汇编窗口，也是可以打开和关闭的。第三个是符号窗口，在这里我们可以实时查看程序中所有变量的值。

我们试一下，比如我想看一下这个GPIO_InitStructure结构体的值，那就可以在这个符号窗口里找一下，main.c里面，然后main函数这里就可以看到这个变量了，里面可以看到结构体的三个成员。如果想看一下结构体值的变化，可以在这里右键添加到watch一窗口。在这里就能看到结构体的值了。

图：查看GPIO_InitStructure结构体的值

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-14.png" alt = "P10-OLED显示屏-14.png" width = "70%" height = "auto">
</div>
<br>

比如我们在这里单步运行，这里就能看到这个变量值的变化了，还是非常方便的。

然后剩下的这里还有串口显示，逻辑分析仪等等，这些工具的功能也都是非常强大的，大家可以自行了解一下，我就不再演示了。

另外我们还可以点击这个外设菜单栏、系统资源查看，这里就可以看到所有的外设寄存器。比如我们选择GPIOA，右边的这里就会显示GPIOA外设的所有寄存器。

STM32实时执行程序，Keil软件实时显示外设寄存器状态。

图：查看所有外设的寄存器

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P10-OLED%E6%98%BE%E7%A4%BA%E5%B1%8F-16.png" alt = "P10-OLED显示屏-16.png" width = "70%" height = "auto">
</div>
<br>

所以当你以后遇到一个比较难调的程序，比如不清楚程序是如何执行的，或者想要看一大堆变量却不方便显示的，或者想看一下寄存器是不是配置正确了，都可以考虑一下使用这个Keil自带的调试模式。

最后再说明一下，如果你想修改程序的话，是不能直接在这个调试模式下修改的。修改程序得先退出调试模式，重新编译再进入调试模式，这个注意一下，以上这些就是对kill调试模式的简单介绍。

这个软件更多的功能还得需要大家自己慢慢探索了。探索的方法可以是这些按钮、菜单、都随便点点，看看有啥功能。或者百度搜索一下各个功能是咋用的，或者点这里的帮助。第一项这里可以打开Keil软件的官方帮助文档，这个帮助文档里有对这整个软件最权威最细致的介绍。如果你对某个功能不熟悉的话，可以在这里找一找。好，以上就是本节的全部内容了，我们下节再见。

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