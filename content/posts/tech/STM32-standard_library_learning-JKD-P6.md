---
title: "嵌入式开发-STM32标准库学习：实现LED闪烁与流水灯，蜂鸣器"
date: 2024-05-13T00:17:58+08:00
lastmod: 2024-05-13T00:17:58+08:00
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
- LED闪烁
- LED流水灯
- STM32标准库开发
- 嵌入式开发
- STM32F1
- GPIO
- 蜂鸣器
description: "本节内容指导如何使用面包板、STM32最小系统板及ST Link开发一个简单的LED闪烁程序。首先，介绍搭建硬件电路所需步骤，包括正确连接电源、STM32板、LED和相关跳线。接着，在Keil5 软件中创建新工程，复制必要文件并配置工程设置以完成LED闪烁程序的编写、编译、下载和测试。此外，还介绍了如何利用一个辅助工具清理编译产生的中间文件，以便更好地分享工程。该指南详细讲解了如何使用RCC和GPIO外设及其库函数来控制LED的亮灭，包括设置工作模式、方向和速度。特别提到了四种GPIO输出函数和它们在控制LED亮灭方面的应用。通过主循环实现LED闪烁功能，并讨论了不同驱动模式下LED的性能差异。进一步地，本节还扩展到LED流水灯的制作，介绍了如何利用按位操作来控制多路LED的亮灭。最后，提供了学习STM32库函数的建议，包括查看库函数源码、利用官方文档和在线资源等方法。整个内容旨在帮助初学者掌握STM32开发的基础知识和技能。"
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

## LED闪烁 接线图

本小节我们就来编写代码，完成上一小节演示的三个示例程序。首先我们需要先搭建一下面包板电路，我会把每个代码的接线图都放到工程文件夹的第一个文件夹里，大家可以下载程序源码的文件查看。

`D:\江科大 STM32\程序源码\程序源码\STM32Project-有注释版\1-1 接线图`

图：LED闪烁

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8.jpg" alt = "P6-LED闪烁LED流水灯蜂鸣器.jpg" width = "70%" height = "auto">
</div>
<br>

## LED闪烁 硬件搭建

我们打开接线图，打开3-1 LED闪烁的图片，这就是本节第一个程序的硬件电路。

那我们拿出面包板，把正极的红线朝上，注意别拿反了。然后拿出STM32最小系统版，在这里按照图示位置，上边和右边留两个孔，下边留三个孔，然后插到面包板上。

接着拿出跳线，将最小系统板的正负极引到面包板的供电引脚上，这样上下四排供电引脚就可以通过最小系统板获取电源了。这里G引到负极，3.3引到正极的。下面也是一样。

图：拿出跳线，将最小系统板的正负极引到面包板的供电引脚上

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8.png" alt = "P6-LED闪烁LED流水灯蜂鸣器.png" width = "70%" height = "auto">
</div>
<br>

跳线连接完成后，我们把STLINK按照上面的引脚标识符连接到最小系统上。

这里引脚并不是按照顺序来的，注意别插错线了。我们这整个系统的供电是STLINK的3.3V接到最小系统，然后最小系统又通过跳线接到上下4排的供电孔的。

最后我们拿出一个LED，长脚正极接到正极供电孔，短脚负极接到PA0端口上。这里使用的是低电平点亮的操作方式，为了方便就没有接限流电阻。

图：硬件电路搭建

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-1.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-1.png" width = "70%" height = "auto">
</div>
<br>

这样我们的硬件电路就搭建完成了，我们把STLINK插到电脑上。这里的电源灯亮起。

## LED闪烁-新建工程

### 文件准备

好，那我们打开Keil软件。然后新建一个工程，这里新建工程比较麻烦，我再给大家快速演示一遍，以后我们就直接复制现有的工程。

我们点击Project->new project，选择存放工程的文件夹，在这里再新建一个文件夹，我们可以按Ctrl+Shift+N 快捷键新建文件夹，然后起个名字叫3-1LED闪烁，点进去起个工程名叫Project。保存。

接着选择芯片STM32F103C8，ok这个窗口×掉。

然后在文件管理里打开我们的工程文件夹，再新建三个文件夹，分别叫Start、Library、User。

打开固件库文件。找到启动文件。

按Ctrl+A全选，再按Ctrl+C复制。然后把它们放到Start文件夹下，再找到stm32f10x和system的两个文件，Ctrl+C复制粘贴到Start文件夹下，再找到core_cm3的两个文件，复制粘贴到Start文件夹下，这样Start文件夹的文件就复制完成了。

然后找到标准外设驱动的文件夹，打开src全选复制。粘贴到Library文件夹下。然后打开inc，全选复制，也粘贴到Library文件夹下，这样Library文件夹的文件也就复制完成了。

最后打开project文件夹，然后打开后缀是template的文件夹，按住Ctrl键，选择这里的main、 conf、2个it文件，复制粘贴到User文件夹下，这里main.c我们也直接复制过来，这样就不用再新建文件了。

到此为止我们的工程文件就复制完成了。

### 在Keil中对工程设置

#### 新建三个组Start、Library、User，并添加文件

然后回到Keil，点击这个三个箱子的工程文件管理按钮，把默认的这个组×掉，点这个按钮再新建三个组，叫Start、Library、User。

然后选中Start，在右边点击添加文件，打开Start文件夹，文件类型选所有文件。首先添加后缀为md的启动文件，然后按住Ctrl，把其他的.c和.h文件都选中，然后Add，这样Start组里的文件就添加好了。

然后是Library，点添加文件，打开Library文件夹，然后文件类型选所有文件，Ctrl+A全选，Add，这样Library组里的文件就添加好了。

然后是User添加文件，打开User，文件类型选所有文件，全选，Add，最后点击ok，这样我们工程里的组和文件就都添加好了。

#### C/C++ 选项设置

##### Include Paths 添加头文件路径

接着点击魔术棒按钮，打开工程选项，选择C/C++的，在Include Paths栏，把我们自己建的文件夹路径都添加进来。Start、Library、User, 然后OK。

##### Define 添加宏定义

在Define栏写上USE_STDPERIPH_DRIVER(USE使用、下划线、STD标准、PERIPH外设、下划线、DRIVER驱动)这个字符串。

##### Debug 调试器选择STLINK

最后是Debug，调试器选择STLINK。

##### 调试器设置 

Debug -> ST-Link Debugger -> Settings -> Flash Download 

然后设置、Flash下载、勾上复位并执行这个勾，最后确定。

Ok这样工程选项就配置好了。

#### 将main.c中内容清空

接着我们打开这个main.c，把它这里面的原来的代码全都删掉了，然后右键添加头文件，写上主函数，这样整个工程就建好了，我们编译测试一下。
没有错误，没有警告，然后下载测试也是没有问题的。

### 批处理工具-删除工程编译中产生的中间文件

最后再给大家分享个小工具，我把它放到了工程的第二个文件夹里，这个东西是一个批处理文件，它可以把工程编译产生的中间文件都删掉。

图：批处理工具 `D:\江科大 STM32\程序源码\程序源码\STM32Project-有注释版\1-2 keilkill批处理`

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-2.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-2.png" width = "70%" height = "auto">
</div>
<br>

我们可以把它复制到工程文件夹里，放在这里。因为这个工程编译产生的文件比较大。我们可以看一下这个LED闪烁的工程就有20Mb。

这里主要占空间的就是Listing和Objects这两个文件夹，这些都是工程的中间文件。如果你要把工程分享给别人的话，可以先双击一下这个批处理文件。这时他就会把这些中间文件都删掉，我们再看一下，这样就只要2Mb左右的大小了。然后你就可以把这个文件夹打包，把工程分享给别人了。

图：批处理工具在工程文件夹中 `D:\Keil5Project\STM32Project\3-1 LED闪烁`


<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-3.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-3.png" width = "70%" height = "auto">
</div>
<br>

## LED闪烁-编写工程

好，那我们回到Keil中，正式开始点亮一个LED。

上节课我们也介绍过，操作STM32的GPIO总共需要3个步骤。第一步使用RCC开启GPIO的时钟。第二步使用GPIO_Init函数初始化GPIO。第三步，使用输出或者输入的函数控制GPIO口。

在这里总共涉及了RCC和GPIO两个外设。

### RCC的库函数

我们先来看一下这两个外设都有哪些库函数吧。我们可以在Library中找到rcc.h这个文件，双击打开，然后拖到最下面。

在.h文件的最下面，一般都是库函数所有函数的声明，在这里我们可以看到RCC有很多的库函数，但实际上这里的大部分函数我们都不会用到，我们最常用的只有这三个函数。RCC AHB外设时钟控制，RCC APB2外设时钟控制，RCC APB1外设时钟控制。

图：stm32f10x_rcc.h中 我们常用的三个RCC函数

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-4.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-4.png" width = "70%" height = "auto">
</div>
<br>

我们可以右键跳到定义，这时就来到了.c文件里的函数定义上面。上面这有一个函数的介绍，这个AHB外设时钟控制的函数就是使能或者失能AHB外设时钟的。

下面介绍第一个参数就是选择哪个外设。这里说STM32互联型的设备可以在在这个列表选择，其他设备在下面这个列表选择。

接着第二个参数就是ENABLE或者DISABLE。

然后下面的AP2外设时钟控制和APB1外设时钟控制都是一样的操作方法。第一个参数选择外设，第二个参数使能或者失能。如果你不清楚哪个外设是连接在哪个总线上的，还可以在这个列表找一下，列表中出现的就肯定是这个总线的外设，对吧？

那RCC的库函数就介绍完了，最主要的就是这三个函数，其他的函数基本都用不到。

### GPIO的库函数

接着我们再看一下GPIO的库函数，我们打开gpio.h(stm32f10x_gpio.h)的文件，然后拖到最后，这些就是GPIO的全部库函数了。

我们目前需要了解的就是前面的这些函数。

图：我们需要了解的GPIO的库函数

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-5.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-5.png" width = "70%" height = "auto">
</div>
<br>

#### 库函数GPIO_DeInit

第一个就是GPIO_DeInit，参数可以写GPIOA，GPIOB等等。

调用这个函数之后，所指定的GPIO外设就会被复位，这就是这个函数的用途。

#### 库函数GPIO_AFIODeInit

第二个GPIO_AFIODeInit也是一样，可以复位AFIO外设，这个AFIO我们后面再讲。

#### 库函数GPIO_Init

接着第三个GPIO_Init就是非常重要的函数了。

这个函数的作用是，用结构体的参数来初始化GPIO口。我们需要先定义一个结构体变量，然后再给结构体赋值，最后调用这个函数。

这个函数内部就会自动读取结构体的值，然后自动把外设的各个参数配置好。这种Init函数在STM32中基本所有的外设都有。一般我们初始化外设都是使用这个Init的函数来完成的。

#### 库函数GPIO_StructInit 

第四个是GPIO_StructInit， 这个函数可以把结构体变量赋一个默认值。

#### 库函数GPIO_Read读取函数(共4个)

接下来这四个就是GPIO的读取函数了。然后下面跟着的四个就是GPO的写入函数。

#### 库函数GPIO写入函数(共4个)

GPIO_SetBits()到GPIO_Write()

这些函数就可以实现读写GPL口的功能。

然后剩下的这些函数我们暂时不会用到，所以这里面重要的函数就是GPIO_Init和这8个读写函数。好，那我们就来试试用这些函数来操作GPIO吧。

### 开始写main.c

我们回到main.c文件，首先调用的是，RCC里面的APB2外设时钟控制函数。我们复制粘贴到这里，然后右键跳转到定义，我们需要点亮PA0 口的LED，所以选择RCC_APB2外设_GPIOA这一项，放到第一个参数。

然后继续第二个参数，选择ENABLE，放到第二个参数，这样时钟就开启了。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效

	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
											
	}
}

```

接着调用GPIO_Init的函数。跳转定义，第一个参数选择GPIOA。

第二个参数是一个结构体，我们先把结构体类型复制下来，在GPIO_Init上面粘贴，起个名字，叫GPIO_Initstructure。

这里这个结构体实际上也是一种局部变量。在有些老的编译器，它要求所有的局部变量定义必须放到函数的最前面。如果你的编译器是这样的话，就需要把这一行提到最前面去。那我这个编译器是支持在函数中间定义变量的，所以就放在这个位置。接着我们复制结构体名字，用点把结构体的成员都引出来。

然后还是一个套路，右键跳转看一下说明，复制粘贴一下参数。这样看上去库函数还是很简单的是吧？基本上都是复制粘贴，复制粘贴。

这就印证了程序员的最高境界，Ctrl+C，Ctrl+V走天下，对吧。那我们选择这个GPIOMode_TypeDef，Ctrl+F搜索一下。

然后这里就是GPIO的8种工作模式。AIN是模拟输入，IN_FLOATING是浮空输入，IPD是下拉输入，IPU是上拉输入，Out_OD是开漏输出，Out_PP是推挽输出，AF_OD是复用开漏，AF_PP是复用推挽。

那我们点灯用的是推挽输出。所以复制Out_PP这一项粘贴到GPIO_ Mode这里，接下来GPIO_Pin选择引脚，我们继续右键跳转。

这里GPIO_Pin有多个定义，我们选择member这一项。然后选中这个，CtrL+F，这里因为我们用的是GPIOA外设的0号引脚，所以选择GPIO_Pin_0放到这里。

接着第三个，还是一样的套路。输出速度选择50MHz就行了。最后把GPIO初始化结构体的地址放到GPIO_Init的第二个参数，这样初始化就完成了。

当这个GPIO_Init函数执行完，这个GPIOA外设的0号引脚就自动被配置为推挽输出、50MHz的速度了。

它内部的主要执行逻辑就是读取结构体的参数，执行一堆判断和运算，最后写入到GPIO配置寄存器。至于操作的细节我们就不用再关心了。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;				//GPIO引脚，赋值为第0号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
											
	}
}

```


到现在GPIO初始化就已经完成了，我们接下来就可以使用GPIO的这些输入输出函数了。本小节先介绍这4个GPIO的输出函数。

第一个GPIO_SetBits，第一个参数是GPIOx，第二个参数是GPIO_Pin。这个函数可以把指定的端口设置为高电平。

第二个GPIO_ResetBits，参数和上面的一样，这个可以把指定的端口设置为低电平。

第三个GPIO_WriteBit，这个函数有三个参数，前两个也是指定端口的。第三个是BitVal，这个是根据第三个参数的值来设置指定的端口。

第四个是GPIO_Write，第一个参数是GPIOx选择外设，第二个参数是PortVal，这个函数可以同时对16个端口进行写入操作。

那我们分别来用一下试试。

#### 尝试GPIO_ResetBits

首先试一下GPIO_ResetBits。可以看一下函数说明。第一个是GPIOx，可以是A到G。第二个是要写入的GPIO_Pin_x，x可以是0到15。

那我们就写入GPIOA，GPIO_Pin_0。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header


int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;				//GPIO引脚，赋值为第0号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
	
		GPIO_ResetBits(GPIOA, GPIO_Pin_0);					//将PA0引脚设置为低电平
		
	}
}

```

这样就行了，编译一下，没有错误，下载看一下。可以看到这个LED就已经点亮了，这说明我们端口配置的没问题，而且PA0已经输出了低电平。

#### 尝试GPIO_SetBits

那我们再换GPIO_SetBits函数试试。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;				//GPIO引脚，赋值为第0号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
	
		GPIO_SetBits(GPIOA, GPIO_Pin_0);					//将PA0引脚设置为高电平
		
	}
}

```

这个参数是一样的，不用更改，编译下载。可以看到LED就已经熄灭了。

#### 尝试GPIO_WriteBit

然后我们再试一下第三个函数，GPIO_WriteBit. 这里前两个参数也是一样的。第三个参数我们转到定义看一下。第三个参数的介绍是指定写入的数据值。这个参数可以是BitAction这个枚举中的一个值。Bit_RESET 是清除端口值，也就是置低电平。Bit_SET是设置端口值，也就是置高电平。

我们先复制一下Bit_RESET ，放到这里编译下载。可以看到LED又亮起来了。再把这个改成Bit_SET编译下载。可以看到LED又熄灭了。这就是这三个函数的用法。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;				//GPIO引脚，赋值为第0号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
		
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_RESET);		//将PA0引脚设置为低电平
		
	}
}

```

第四个函数GPIO_Write我们下一个例程再介绍，接下来我们先来完成LED闪烁的任务。为了实现LED闪烁，我们就需要在主循环里写上点量LED，延时一段时间，熄灭LED，延时一段时间这样的逻辑吧。

点亮LED，我们可以用这一句。

```c
GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_RESET);	
```

这样就是点亮RED了。然后复制一下改成 Bit_SET，这就是熄灭LED，

```c
GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_SET);
```

#### Delay()延时函数

那中间需要加两个延时函数来进行一段延时。

这里我已经给大家提供了一个现成的延时函数代码，在工程的第三个文件夹里，这两个就是延时函数的模块。我们先把它们添加到工程里面来。

我们先复制，打开LED闪烁的工程，这里再新建一个文件夹，名称可以叫System，存放系统的资源。然后把Delay粘贴到这里。

回到Keil软件，点击三个箱子的按钮，添加组也叫System。把它往上挪个位置，然后右边添加文件，打开System把Delay的两个文件添加进来，然后OK，这样Delay文件就添加完成了。

最后不要忘了点击魔术棒按钮，添加这个新文件夹的头文件路径。这样就完成了。

我们可以打开Delay.h去看一下，这里就是三个延时函数，分别是微秒延时、毫秒延时和秒延时。

代码：Keil工程/System/Delay.h

```c
#ifndef __DELAY_H
#define __DELAY_H

void Delay_us(uint32_t us);
void Delay_ms(uint32_t ms);
void Delay_s(uint32_t s);

#endif

```


打开.c文件可以看到这些函数的定义，这里是用SysTick定时器来实现的延时，具体怎么实现的，大家不用管的。这个延时函数一般都是直接拿来用就行了，不需要修改什么。

那我们回到main.c使用这个延时函数模块，需要先在上面写上

```c
#include"Delay.h"
```

然后复制毫秒延时函数放在这里，参数给500，这样就可以延时500毫秒了。然后复制一下下面这里也给500毫秒的延时，我们试一下编译。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;				//GPIO引脚，赋值为第0号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
	
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_RESET);		//将PA0引脚设置为低电平
		Delay_ms(500);										//延时500ms
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_SET);			//将PA0引脚设置为高电平
		Delay_ms(500);										//延时500ms
		
	}
}

```

没有错误，下载，我们看一下，这时LED就开始闪烁了。如果我们把这个延时改短点，比如100毫秒，编译下载，这样LED就闪的更快了。这就是第一个LED闪烁的代码了。这里先把100改回成500，这里除了WriteBit函数外，还可以用SetBits和ResetBits来实现。那我把它们都写上给大家参考一下。

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;				//GPIO引脚，赋值为第0号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
		/*设置PA0引脚的高低电平，实现LED闪烁，下面展示2种方法*/
		
		/*方法1：GPIO_ResetBits设置低电平，GPIO_SetBits设置高电平*/
		GPIO_ResetBits(GPIOA, GPIO_Pin_0);					//将PA0引脚设置为低电平
		Delay_ms(500);										//延时500ms
		GPIO_SetBits(GPIOA, GPIO_Pin_0);					//将PA0引脚设置为高电平
		Delay_ms(500);										//延时500ms
		
		/*方法2：GPIO_WriteBit设置低/高电平，由Bit_RESET/Bit_SET指定*/
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_RESET);		//将PA0引脚设置为低电平
		Delay_ms(500);										//延时500ms
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_SET);			//将PA0引脚设置为高电平
		Delay_ms(500);										//延时500ms
		
	}
}

```

最后大家可能觉得调用这些函数只能填它给的指定参数。如果我们非要给一个数，1是高电平，0是低电平，这样该怎么办呢？

那我们可以复制一下这个WriteBit函数，然后把Bit_RESET改成0， Bit_SET改成1。但如果直接这样编译的话，会有两个警告，说的是枚举类型中混入了其他类型的变量。

所以如果想直接写1和0的话，需要在这里加上强制类型转换，把一和零类型转换为BitAction的枚举类型。这样再编译就没有问题了。然后下载，这样也是没有问题的。

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;				//GPIO引脚，赋值为第0号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
		/*设置PA0引脚的高低电平，实现LED闪烁，下面展示3种方法*/
		
		/*方法1：GPIO_ResetBits设置低电平，GPIO_SetBits设置高电平*/
		GPIO_ResetBits(GPIOA, GPIO_Pin_0);					//将PA0引脚设置为低电平
		Delay_ms(500);										//延时500ms
		GPIO_SetBits(GPIOA, GPIO_Pin_0);					//将PA0引脚设置为高电平
		Delay_ms(500);										//延时500ms
		
		/*方法2：GPIO_WriteBit设置低/高电平，由Bit_RESET/Bit_SET指定*/
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_RESET);		//将PA0引脚设置为低电平
		Delay_ms(500);										//延时500ms
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, Bit_SET);			//将PA0引脚设置为高电平
		Delay_ms(500);										//延时500ms
		
		/*方法3：GPIO_WriteBit设置低/高电平，由数据0/1指定，数据需要强转为BitAction类型*/
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, (BitAction)0);		//将PA0引脚设置为低电平
		Delay_ms(500);										//延时500ms
		GPIO_WriteBit(GPIOA, GPIO_Pin_0, (BitAction)1);		//将PA0引脚设置为高电平
		Delay_ms(500);										//延时500ms
	}
}

```

### 验证推挽输出和开漏输出的驱动问题

最后我们再研究一下推挽输出和开漏输出的驱动问题。我们把这个LED拔掉，然后把长脚插到PA0口，短脚插到负极，这样LED就是高电平点亮的方式。可以看到LED也是正常闪烁，说明在推挽模式下，高低电平都是有驱动能力的。

那我们把这个端口的模式换成Out_OD，开漏输出模式，编译下载。可以看到LED就不亮了，现在LED还是高电平点亮的方式。LED不亮，说明开漏输出的模式高电平是没有驱动能力的。

我们把LED再改回低电平驱动的方式，可以看到LED又亮起来了。这说明开漏模式的低电平是有驱动能力的，这就印证了我们上一小节讲的推挽输出和开漏输出的特性。

推挽输出高低电均有驱动能力，开漏输出高电平相当于高阻态，没有驱动能力，低电平有驱动能力。那我们把这个改回推挽输出，一般输出用推挽模式就行了，特殊的地方才会用到开漏模式。

好，这就是LED闪烁代码的全部内容了。

## LED流水灯

### LED流水灯接线图

我们关掉Keil来看一下第二个代码，LED流水灯，先来看一下接线图。这里需要拿出8个LED，正极都插到正极的供电孔，负极依次插到PA0到PA7的端口。

图：LED流水灯 接线图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-1.jpg" alt = "P6-LED闪烁LED流水灯蜂鸣器-1.jpg" width = "70%" height = "auto">
</div>
<br>

那我们来插一下电路。这里可以先插两边的LED，然后掰弯一下，再插中间的LED这样方便一些。

图：LED流水灯接线

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-6.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-6.png" width = "70%" height = "auto">
</div>
<br>

插完电路后，我们复制一下LED闪烁的工程，改个名字叫3-2LED流水灯。这样直接复制现有工程，就不用再费时间新建工程了。然后我们来修改一下这个代码，实现LED流水灯的程序。

### 编写工程

在这里第一句打开GPIOA的时钟。因为我们连接的都是GPIOA的端口，所以第一句不用变的。接着初始化端口的这一部分，这里只初始化了GPIO的0号端口。我们流水灯用的是GPIOA的0到7号端口，所以这里要加一些端口，那怎么加呢？在这里我们可以直接在GPIO_Pin_0后面加上，或，GPIO_Pin_1再加上，或，GPIO_Pin_3，这样就可以一次性把三个端口都初始化了。

为什么可以这样来使用呢？我们转到定义看一下，这里可以看到Pin0对应的数据是0x0001，然后Pin1、Pin2、Pin3依次为0x0002，0x0004，0x0008。如果把这个16进制换成二进制的话，就是0000 0000 000 0001，然后是0010、0100、1000。在这里每个端口对应一个位，如果把它们进行按位或的操作，比如Pin0、Pin1和Pin2按位或，那结果就是0111，这样就相当于同时选中了三个端口，这就是按位或的操作逻辑。

图：位或

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-7.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-7.png" width = "70%" height = "auto">
</div>
<br>

最后我们还可以看到这里有个GPIO_Pin_ALL，它对应的数据就是0xFFFF，也就是所有位都为1，这样就相当于选中了所有的引脚。

在这里除了这个GPIO_Pin可以用按位或的操作方式外，这个时钟控制的这一项(RCC_APB2Periph_GPIOA)也是可以用按位或的操作方式来选择多个外设的。你看它这里的定义也是一样的，数据的规律是每一位对应一个外设。

图：stm32f10x_rcc.h

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81LED%E6%B5%81%E6%B0%B4%E7%81%AF%E8%9C%82%E9%B8%A3%E5%99%A8-8.png" alt = "P6-LED闪烁LED流水灯蜂鸣器-8.png" width = "70%" height = "auto">
</div>
<br>

还有这个GPIO_SetBits这里也可以用按照位或选择多个引脚，这样就能同时设置多个引脚了。所以这个函数名字也多了个S叫SetBits，所以ResetBits也是一个意思，在这个函数介绍里也写了这个参数可以是GPIO_Pin_x的任意“组合”，说的就是这个方式。

那介绍完按位或的这种操作方式，我们就可以在这里使用位或把这八个引脚都选上。当然这里我就直接写GPIO_Pin_ALL了，这样就把16个端口全部配置为了推挽输出模式。

然后是主循环里面，我们先把这些删掉。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_All;				//GPIO引脚，赋值为所有引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
		
	}
}

```

现在为了同时控制16个端口，我们就可以使用GPIO_Write这个函数了。我们复制过来，第一个参数是GPIOX，我们直接写GPIOA。第二个参数我们转到定义看一下，这里写的是，指定写到输出数据寄存器的值，下面我们可以看到，这第二个参数就是直接写到GPIO的ODR寄存器里的，所以我们可以写0x0001，对应二进制就是0000 0000 0000 0001。

因为C语言不支持直接写二进制，所以这里只能转化为16进制来写。这16个二进制分别对应PA0到PA15，总共16个端口，最低位对应PA0，然后往上依次是PA1、PA2一直到PA15。

因为是低电平点亮，所以前面再加一个按位取反的符号，这样就是第一个LED点亮，其他都熄灭了，接着Delay 500毫秒。然后复制一下。

然后这里依次改为02，这里对应二进制就是0010，04，0100，08，1000，10。高八位我们暂时不用，我们编译看一下。没有错误下载可以看到LED依次点亮。

如果想快一点的话，这里可以改成100毫秒。

再试一下。现在就快一些了。如果你想换一种形式的流水灯，这里的数据对应更改一下就行了。或者定义一个数组，依次取出数组中的数据来进行花式点灯，这都是没问题的。在这里我就不过多演示了，那我们第二个程序LED流水灯到这里就完成了。

编写代码：main.c(完整版)

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);	//开启GPIOA的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_All;				//GPIO引脚，赋值为所有引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOA, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOA的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
		/*使用GPIO_Write，同时设置GPIOA所有引脚的高低电平，实现LED流水灯*/
		GPIO_Write(GPIOA, ~0x0001);	//0000 0000 0000 0001，PA0引脚为低电平，其他引脚均为高电平，注意数据有按位取反
		Delay_ms(100);				//延时100ms
		GPIO_Write(GPIOA, ~0x0002);	//0000 0000 0000 0010，PA1引脚为低电平，其他引脚均为高电平
		Delay_ms(100);				//延时100ms
		GPIO_Write(GPIOA, ~0x0004);	//0000 0000 0000 0100，PA2引脚为低电平，其他引脚均为高电平
		Delay_ms(100);				//延时100ms
		GPIO_Write(GPIOA, ~0x0008);	//0000 0000 0000 1000，PA3引脚为低电平，其他引脚均为高电平
		Delay_ms(100);				//延时100ms
		GPIO_Write(GPIOA, ~0x0010);	//0000 0000 0001 0000，PA4引脚为低电平，其他引脚均为高电平
		Delay_ms(100);				//延时100ms
		GPIO_Write(GPIOA, ~0x0020);	//0000 0000 0010 0000，PA5引脚为低电平，其他引脚均为高电平
		Delay_ms(100);				//延时100ms
		GPIO_Write(GPIOA, ~0x0040);	//0000 0000 0100 0000，PA6引脚为低电平，其他引脚均为高电平
		Delay_ms(100);				//延时100ms
		GPIO_Write(GPIOA, ~0x0080);	//0000 0000 1000 0000，PA7引脚为低电平，其他引脚均为高电平
		Delay_ms(100);				//延时100ms
	}
}

```

## 蜂鸣器

接下来是第三个程序，蜂鸣器。通过前两个程序的学习，这个就应该简单多了。

### 蜂鸣器接线图

我们先看一下接线图。

图：蜂鸣器接线图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P6-LED%E9%97%AA%E7%83%81-LED%E6%B5%81%E6%B0%B4%E7%81%AF-%E8%9C%82%E9%B8%A3%E5%99%A8.jpg" alt = "P6-LED闪烁LED流水灯蜂鸣器.jpg" width = "70%" height = "auto">
</div>
<br>

这个VCC正极接到正极供电孔，GND负极接到负极供电孔。然后I/O控极就随便选择一个I/O口接上就行了。

这里我选择的是PB12号口，那大家注意一下，这个A15、B3、B4这3个口大家先别选。我们从引脚定义图可以看到，这三个口默认是JTAG的调试端口，如果要用作普通端口的话，还需要再进行一些配置。我之前在使用的时候就没注意到这一点，我说我明明已经配置好了，这三个端口怎么就不输出呢？最后才发现这三个是调试端口。

好，那我们就来连一下电路。首先拿出蜂鸣器模块和3根公对母的杜邦线，带孔的这一端插在蜂鸣器上，另一端橙色的是VCC，插在正极供电孔，灰色的是GND，插在负极供电孔，红色的是控制脚，插在PB12号口。这样我们硬件电路就完成了。

我们给PB12输出低电平，蜂鸣器就会响，输出高电平，蜂鸣器就不响。那我们复制一下LED闪烁的程序，改个名字叫3-3蜂鸣器，打开工程。

### 编写工程

这里程序相信大家应该都已经会写了吧。首先是时钟，因为我们用的是PB口，所以这里改为GPIOB。然后是端口，PB12号，所以这里是Pin12。端口模式，仍然是推挽输出。速度50M。

初始化这里也应该是对GPIOB的初始化。到这里我们的PB12号口就已经初始化好了。我然后是输出，我们把这些先删掉，这两个地方都改为B、12。这就完成了，我们看一下编译。下载可以听到蜂鸣器已经在响了，我们让它换个响的模式，这里改成这样复制一下，响100毫秒，停100毫秒，再响100毫秒，再停700毫秒，编译下载。这样的声音就和我们最开始演示的一样了。

编写代码：main.c(完整版)

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

int main(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);	//开启GPIOB的时钟
															//使用各个外设前必须开启时钟，否则对外设的操作无效
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;					//定义结构体变量
	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;		//GPIO模式，赋值为推挽输出模式
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;				//GPIO引脚，赋值为第12号引脚
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;		//GPIO速度，赋值为50MHz
	
	GPIO_Init(GPIOB, &GPIO_InitStructure);					//将赋值后的构体变量传递给GPIO_Init函数
															//函数内部会自动根据结构体的参数配置相应寄存器
															//实现GPIOB的初始化
	
	/*主循环，循环体内的代码会一直循环执行*/
	while (1)
	{
		GPIO_ResetBits(GPIOB, GPIO_Pin_12);		//将PB12引脚设置为低电平，蜂鸣器鸣叫
		Delay_ms(100);							//延时100ms
		GPIO_SetBits(GPIOB, GPIO_Pin_12);		//将PB12引脚设置为高电平，蜂鸣器停止
		Delay_ms(100);							//延时100ms
		GPIO_ResetBits(GPIOB, GPIO_Pin_12);		//将PB12引脚设置为低电平，蜂鸣器鸣叫
		Delay_ms(100);							//延时100ms
		GPIO_SetBits(GPIOB, GPIO_Pin_12);		//将PB12引脚设置为高电平，蜂鸣器停止
		Delay_ms(700);							//延时700ms
	}
}

```

## 如何学习库函数的使用

最后再给大家介绍几种使用库函数的方法。第一种就是像我这样，先打开点儿.h文件的最后，看一下都有哪些函数，然后在右键转到定义，查看一下函数和参数的用法。这里全都是英文的，如果看不懂的话，借助一下翻译软件就行了。

第二种就是打开我提供资料文件夹里的这个库函数用户手册。这里面有所有函数的介绍和使用方法。这个文档是中文的，看起来比较好理解，而且这个函数下面都还给了例子，要用的话直接复制过来就行了。不过这个用户手册的版本并不对应我们现在用的这个库函数的版本，我们看一下。

我们使用的扩函数是V3.5.0版本的。这个用户手册是老版本库函数的，所以有部分用法会有些出入，但是整体上的差异都不大，参考这个用户手册也是没有问题的。

那V3.5.0的库函数有没有这样的用户手册呢？我从网上了解的是目前还是没有的。ST公司并没有发布V3.5.0版本的库函数用户手册，而是在这个固件库压缩包中给了这样的一个帮助文档，我们可以打开看一下。可以找到GPIO这一节。

来看一下这种帮助文档，可以像这样跳来跳去的。如果你习惯这种方式的话，也可以参考这个文档。不过这个只有英文的版本。

最后一种方式就是百度搜索，参考一下别人的代码。我们可以打开百度搜索STM32 GPIO，这里都有介绍。我们搜索一下GPIO初始化程序。可以点开看看。这里都有例子，直接参考一下拿过来用就行了。

像我在学习STM32的时候，也会经常去参考别人的程序，要不然怎么知道该用哪些函数，是吧？好，那我们本小节的任务到这里就全部完成了。我们下小节再来继续学习GPIO的输入部分。

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