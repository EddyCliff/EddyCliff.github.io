---
title: "嵌入式开发-STM32标准库学习：按键控制LED,光敏传感器控制蜂鸣器"
date: 2024-05-13T00:17:58+11:00
lastmod: 2024-05-13T00:17:58+11:00
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
- 按键控制LED
- 蜂鸣器
- 光敏传感器控制蜂鸣器
- STM32标准库开发
- 嵌入式开发
- STM32F1
description: ""
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

## 按键控制LED

## 按键控制LED-硬件电路连接

接下来我们来写一下GPIO输入部分的代码。先看一下第一个代码的接线图。

图：按键控制LED

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P8-%E6%8C%89%E9%94%AE%E6%8E%A7%E5%88%B6LED-%E5%85%89%E6%95%8F%E4%BC%A0%E6%84%9F%E5%99%A8%E6%8E%A7%E5%88%B6%E8%9C%82%E9%B8%A3%E5%99%A8.jpg" alt = "P8-按键控制LED-光敏传感器控制蜂鸣器.jpg" width = "70%" height = "auto">
</div>
<br>

这里我接了两个按键和2个LED，其中两个按键分别接在了PB1和PB11两个口上。按键一端接GPIO口，另一端接GND，就是我们PPT中的第一种解法。

图：按键的硬件电路

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P8-%E6%8C%89%E9%94%AE%E6%8E%A7%E5%88%B6LED-%E5%85%89%E6%95%8F%E4%BC%A0%E6%84%9F%E5%99%A8%E6%8E%A7%E5%88%B6%E8%9C%82%E9%B8%A3%E5%99%A8.png" alt = "P8-按键控制LED-光敏传感器控制蜂鸣器.png" width = "70%" height = "auto">
</div>
<br>

2个LED分别接在了PA1和PA2两个口。LED一端接GPIO口，另一端接VCC，就是低电平点亮的接法。这个按键、LED的数量和连接的端口都是随意的，具体接多少个，接到哪些端口就看你的需求了。

然后我们拿出插好STM32最小系统板的面包板，把两个按键插在PB1和PB11两个端口上。

再把2个LED插在PA1和PA2两个端口上。这样就完成了这个代码的硬件电路。

图：按键控制LED-实物接线图

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P8-%E6%8C%89%E9%94%AE%E6%8E%A7%E5%88%B6LED-%E5%85%89%E6%95%8F%E4%BC%A0%E6%84%9F%E5%99%A8%E6%8E%A7%E5%88%B6%E8%9C%82%E9%B8%A3%E5%99%A8-1.png" alt = "P8-按键控制LED-光敏传感器控制蜂鸣器-1.png" width = "70%" height = "auto">
</div>
<br>

## 按键控制LED-Keil工程准备

接下来我们打开工程文件夹，复制一下蜂鸣器工程的代码，改个名字叫3-4按键控制LED。然后点进去，打开工程。

接下来我们就需要在这个工程上完成LED和按键的驱动代码。但是如果把这两部分驱动代码都混在主函数里面，那代码就太乱了是吧？不容易管理，也不容易移植。

所以对于这种驱动代码而言，我们一般都会把它封装起来，单独放在另外的.c和.h文件里。这就是模块化编程的方式我在51单片机教程的模块化编程那节讲过，不会的可以去看一下，在这里我就直接操作了。

1、首先我们打开工程文件夹，再建一个文件夹叫Hardware，用来存放硬件驱动。

2、然后回到Keil还是一样的步骤。点击三个箱子的按钮，打开工程管理，新建一个组，也叫Hardware，挪个位置。

3、然后再点击魔术棒按钮，打开工程选项，选择C/C++，点击这里三个点的按钮，把刚才新建的Hardware文件夹添加到头文件路径列表中。Ok这样我们就又在工程添加了一个Hardware文件夹。

4、然后在Hardware这里右键添加新的文件，选择C文件起个名字叫LED，这个文件就用来封装LED的驱动程序。然后路径也别忘了，选择Hardware文件夹，存在这个文件夹里。

5、接着继续在Hardware处右键添加新的文件，选择H文件，起个名字也叫LED。下面的路径也是一样，选择Hardware文件夹。

这样我们就建好了LED.c和LED.h两个文件，用来封装LED驱动程序。LED.c用来存放驱动程序的主体代码，LED.h用来存放这个驱动可以对外提供的函数或变量的声明。

## 按键控制LED-编写工程

### 编写LED的代码-LED.h

这两个文件建好之后，还得添加一些必要的代码。首先.c文件的第一行，右键include一个stm32f10x的.h头文件。

要添加一个防止头文件重复包含的代码，这里格式都是固定的。就是#ifndef_LED_H ，如果没有定义LED这个字符串，那么#define_LED_H，那么就定义这个字符串，最后再加上#endif，这个是和#ifndef组成的。括号，我们的函数和变量声明就放在这个括号里面。最后注意这个文件要以空行结尾，这样就完成了。

代码：编写led.h

```c
#ifndef __LED_H
#define __LED_H

#endif

```

### 编写LED的代码-LED.c

接着我们就来封装一下LED的代码，我们打开led.c文件，首先先写一个LED初始化函数。

#### void LED_Init(void) 初始化LED

在这里写`void LED_Init(void)`。

这个函数是用来初始化LED的。那里面写的就是打开时钟、配置端口模式这些东西，这就是我们之前学的代码，也就是`RCC_APB2PeriphClockCmd`，我们的LED都是接在GPIO上的。所以第一个参数是`RCC_APB2Periph_GPIOA`。然后第二个参数写`ENABLE`，开启时钟。

接着配置端口模式。先定义一个结构体变量，GPIO_InitTypeDef。这里如果不显示代码提示的话，可以按一下快捷键，Ctrl+Alt+空格，这样就可以弹出代码提示框了。

我们选择GPIO_InitTypeDef变量名叫GPIO_InitStructure 。然后把结构体的成员用点运算符都引出来，先放在这儿，最后调用GPL elite函数。

第一个参数是GPIOA，初始化GPIOA外设。第二个是结构体变量名，前面加上取地址的符号，这里使用的是地址传递。

接着填一下参数。

第一个GPIO_Mode，我们写GPIO_Mode_Out_PP，推挽输出。

第二个GPIO_Pin，我们写GPIO_Pin_1再或上GPIO_Pin_2选中1号和2号口。

第三个GPIO_Speed，我们写GPIO_Speed_50MHz，选择50M的速度，这样LED初始化的代码就写完了。

编写代码：LED.c

```c
void LED_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);		//开启GPIOA的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1 | GPIO_Pin_2;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);						//将PA1和PA2引脚初始化为推挽输出
	
	/*设置GPIO初始化后的默认电平*/
	GPIO_SetBits(GPIOA, GPIO_Pin_1 | GPIO_Pin_2);				//设置PA1和PA2引脚为高电平
}
```

这个代码大家多打几遍，再根据代码提示，基本上就可以直接上手了，还是比较好用的是吧。那到这里我们只需要调用一个LED_Init的函数，LED的2个GPIO口就可以直接初始化完成了。那我们先试一下.

因为这个函数是需要被外部引用的，所以我们需要复制一下函数的第一行，把这个函数的第一行放在.h文件的这里，这样就是对模块外部声明，这个函数是可以被外部调用的函数。

编写代码：LED.h

```c
#ifndef __LED_H
#define __LED_H

void LED_Init(void);

#endif

```

那我们回到main.c先把蜂鸣器的这些代码删掉，然后在上面的这里写上include "LED.h"去包含led模块的头文件之后，在主函数里直接调用LED_Init，这样就完成了LED的初始化。

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header
#include "LED.h"

int main(void)
{
	/*模块初始化*/
	LED_Init();		//LED初始化
	
	while (1)
	{
		
	}
}

```

这里有提示一个警告，这是因为我们新写的代码还没有更新，软件还不知道有这个函数，那我们编译一下。这个是0错误0警告。

有时候这个软件也会时不时报告警告或错误。这个有可能是语法检查更新较慢，直接编译一下没有问题就行了，然后下载。

这里可以看到2个LED都亮起来了，这说明我们的端口配置和模块化编程是没有问题的。这里因为GPIO配置好了之后，默认就是低电平，所以我们还没操作LED，LED就都亮起来了。

那我们可以在LED edit函数的最后写上GPIO_SetBits()。这样初始化之后，如果不操作LED，LED就是熄灭的了。试一下编译没有错误，下载初始化之后，LED默认就是关闭的状态了。

编写代码：LED.c

```c
#include "stm32f10x.h"                  // Device header

/**
  * 函    数：LED初始化
  * 参    数：无
  * 返 回 值：无
  */
void LED_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);		//开启GPIOA的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1 | GPIO_Pin_2;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);						//将PA1和PA2引脚初始化为推挽输出
	
	/*设置GPIO初始化后的默认电平*/
	GPIO_SetBits(GPIOA, GPIO_Pin_1 | GPIO_Pin_2);				//设置PA1和PA2引脚为高电平
}

```

#### LED点亮和熄灭 的 函数实现

接着我们就来快速完善一下LED驱动程序模块。除了初始化，我们还需要点亮和熄灭LED的函数。

这里可以写一个void LED1_ON(void)，点亮LED1，里面写

GPIO_ResetBits(GPIOA, GPIO_Pin_1);		//设置PA1引脚为低电平。

我们暂且把PA1的LED叫LED1。

然后再来一个void LED1_OFF(void)，熄灭LED1。里面写

GPIO_SetBits(GPIOA, GPIO_Pin_1);		//设置PA1引脚为高电平

然后把这两个函数复制粘贴一下。

把下面的改成LED2，GPIO_Pin_2，这就是打开和关闭LED2的函数了。

当然我这里用了四个函数来实现两个灯的打开和关闭。如果你觉得函数太多了，那你也可以定义一个LED_Set函数。然后定义两个参数，一个参数选择操作哪个灯，另一个参数选择开还是关，这都是没问题的。根据你的实际需求来。

我这里LED比较少，就直接这样写了。

那我们把这四个函数都放在LED.h去文件里声明一下。

```c
#ifndef __LED_H
#define __LED_H

void LED_Init(void);
void LED1_ON(void);
void LED1_OFF(void);
void LED2_ON(void);
void LED2_OFF(void);

#endif

```

这样LED的驱动函数模块就封装好了。

我们在主函数里调用LED1_ON，Delay 500毫秒，LED1_OFF， Delay 500毫秒。编译。下载。看一下LED就在闪烁了。

我们再在这里加上LED2_OFF，然后这里LED2_ON，编译，下载。

编写代码：LED.c

```c
#include "stm32f10x.h"                  // Device header

/**
  * 函    数：LED初始化
  * 参    数：无
  * 返 回 值：无
  */
void LED_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);		//开启GPIOA的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1 | GPIO_Pin_2;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);						//将PA1和PA2引脚初始化为推挽输出
	
	/*设置GPIO初始化后的默认电平*/
	GPIO_SetBits(GPIOA, GPIO_Pin_1 | GPIO_Pin_2);				//设置PA1和PA2引脚为高电平
}

/**
  * 函    数：LED1开启
  * 参    数：无
  * 返 回 值：无
  */
void LED1_ON(void)
{
	GPIO_ResetBits(GPIOA, GPIO_Pin_1);		//设置PA1引脚为低电平
}

/**
  * 函    数：LED1关闭
  * 参    数：无
  * 返 回 值：无
  */
void LED1_OFF(void)
{
	GPIO_SetBits(GPIOA, GPIO_Pin_1);		//设置PA1引脚为高电平
}

/**
  * 函    数：LED2开启
  * 参    数：无
  * 返 回 值：无
  */
void LED2_ON(void)
{
	GPIO_ResetBits(GPIOA, GPIO_Pin_2);		//设置PA2引脚为低电平
}

/**
  * 函    数：LED2关闭
  * 参    数：无
  * 返 回 值：无
  */
void LED2_OFF(void)
{
	GPIO_SetBits(GPIOA, GPIO_Pin_2);		//设置PA2引脚为高电平
}

```

编写代码：main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "LED.h"


int main(void)
{
	/*模块初始化*/
	LED_Init();		//LED初始化
	
	while (1)
	{
		LED1_ON();
		LED1_OFF();
		Delay_ms(500);
		LED2_ON();
		LED2_OFF();
		Delay_ms(500);
	}
}

```

可以看到2个LED交替闪烁，这说明我们的驱动代码是没问题的。

在主函数里我们的代码也变得更加简洁。初始化就是初始化，开灯就是开灯，关灯就是关灯，不需要再管那些底层的各种参数了，这就是模块化编程的好处。

### 编写按键的代码

### 编写代码：按键1按下，LED1点亮，按键2按下，LED1熄灭

#### 新建Key.h和Key.c

接着我们就来开始写按键部分的代码了，同样我们也把按键封装在驱动函数模块里。我们在Hardware目录下右键添加新的文件，选择C文件起个名字叫Key，下面的路径，选择Hardware文件夹。

接着继续在Hardware出右键添加新的文件，选择H文件起个名字也叫Key，下面的路径也是写Hardware文件夹。

#### 编写Key.c

然后Key.c文件，右键添加stm32f10x头文件。

编写代码：Key.h

```c
#ifndef __KEY_H
#define __KEY_H

#endif
```

这样函数的基本框架就建好了。

##### 在Key.c中编写-按键初始化函数

我们到Key.c文件里先写一个按键初始化函数，void Key_Init(void) 在这里面我们将按键的两个端口都初始化为上拉输入的模式。

第一步还是开启时钟，RCC_APB2PeriphClockCmd。我们的按键是接在GPIOB上的。所以第一个参数是RCC_APB2Periph_GPIOB，然后第二个参数写ENABLE开启时钟。

第二步GPIO输出，定义一个结构体变量GPIO_InitTypeDef，变量名叫GPIO_InitStructur。然后把结构体的成员用点运算符都引出来，先放在这儿。最后调用GPIO_Init函数。

第一个参数是GPIOB初始化GPIOB外设。第二个参数是结构体变量名,前面加上取地址的符号，然后填一下参数。

第一个GPIO_Mode这里因为我们需要读取按键，所以我们需要选择GPIO_Mode_IPU，上拉输入。

第二个GPIO_Pin，因为我们案件接在了PB1和PB11口上，所以写GPIO_Pin_1 | GPIO_Pin_11选中1号和11号口。

第三个GPIO_Speed，这里的速度是GPIO的输出速度，在输入模式下，这个参数其实是没用的。

那我们还是填上GPIO_Speed_50MHz，这个也没啥影响，这样按键的初始化就完成了。

##### 在Key.c中编写-读取按键值的函数

接着我们再来写一个读取按键值的函数，我们在这下面写上

uint8_t Key_GetNum(void)，调用这个函数就可以返回按下按键的键码，它的返回值是uint8_t。

我们之前讲过，这就是unsigned char的意思。接着我们在里面再定一个变量，uint8_t KeyNum = 0。最后return KeyNum。将这个变量作为返回值。
按键键码默认给0。如果没有按键按下，就返回0。

编写代码：Key.c

```c
#include "stm32f10x.h"                  // Device header


/**
  * 函    数：按键初始化
  * 参    数：无
  * 返 回 值：无
  */
void Key_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);		//开启GPIOB的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1 | GPIO_Pin_11;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB, &GPIO_InitStructure);						//将PB1和PB11引脚初始化为上拉输入
}

/**
  * 函    数：按键获取键码
  * 参    数：无
  * 返 回 值：按下按键的键码值，范围：0~2，返回0代表没有按键按下
  * 注意事项：此函数是阻塞式操作，当按键按住不放时，函数会卡住，直到按键松手
  */
uint8_t Key_GetNum(void)
{
	uint8_t KeyNum = 0;		//定义变量，默认键码值为0
	
	return KeyNum;			//返回键码值，如果没有按键按下，所有if都不成立，则键码为默认值0
}

```

然后在中间这里我们就需要用到读取GPIO端口的功能。那先找一下GPIO的库函数文件，打开gpio.h这个文件，翻到最后来看一下这4个GPIO的读取函数。

代码：gpio.h中4个GPIO的读取函数

```c
uint8_t GPIO_ReadInputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
uint16_t GPIO_ReadInputData(GPIO_TypeDef* GPIOx);
uint8_t GPIO_ReadOutputDataBit(GPIO_TypeDef* GPIOx, uint16_t GPIO_Pin);
uint16_t GPIO_ReadOutputData(GPIO_TypeDef* GPIOx);
```

第一个是GPIO_ReadInputDataBit，这个函数是用来读取输入数据寄存器某一个端口的输入值，它的参数是GPIOx和GPIO_Pin用来指定某一个端口，返回值是uint8_t代表这个端口的高低电平。

读取按键我们就需要用到这个函数。

第二个GPIO_ReadInputData，这个函数比上一个函数少了个Bit，它是用来读取整个输入数据寄存器的，参数只有一个GPIOx用来指定外设，返回值是uint16_t是一个16位的数据，每一位代表一个端口值。

第三个GPIO_ReadOutputDataBit，这个函数是用来读取输出数据寄存器的某一个位，所以原则上来说它并不是用来读取端口的输入数据的。这个函数一般用于输出模式下，用来看一下自己输出的是什么，具体有什么用，我等会儿可以给大家演示一下。

最后一个GPIO_ReadOutputData，这个函数也是少了一个Bit，意思也是一样，是用来读取整个输出寄存器的。

我们打开PPT看一下这个图，对照一下这个图应该就好理解了。

图：根据GPIO位结构图来理解GPIO的4个读取函数

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P8-%E6%8C%89%E9%94%AE%E6%8E%A7%E5%88%B6LED-%E5%85%89%E6%95%8F%E4%BC%A0%E6%84%9F%E5%99%A8%E6%8E%A7%E5%88%B6%E8%9C%82%E9%B8%A3%E5%99%A8-2.png" alt = "P8-按键控制LED-光敏传感器控制蜂鸣器-2.png" width = "70%" height = "auto">
</div>
<br>

GPIO_ReadInputDataBit就是读取这里输入数据寄存器的某一位，GPIO_ReadInputData就是读取这整个输入数据寄存器器。GPIO_ReadOutputDataBit就是读取这里输出数据寄存器的某一位，GPIO_ReadOutputData就是读取这整个输出数据寄存器。

所以说如果你想读取GPIO口的话，需要用ReadInput的这两个函数。如果在输出模式下想要看一下现在输出了什么，才需要用到ReadOutput的这两个函数，这就是这四个函数的用途。

那么回到Key.c这里来，在这里我们需要读取外部输入的一个端口值，所以我们到gpio,h这里复制一下GPIO_ReadOutputDataBit这个函数放到按键读取函数这里，然后参数写GPIOB，GPIO_Pin_1，读取PB1端口的值。这里的返回值就是输入数据寄存器某一位的值，0代表低电平，1代表高电平。

所以我们加上if判断，把这个函数括起来。然后如果读取PB1端口值 == 0，就代表按键按下，我们进入if里操作。

这次按键刚按下会有个抖动，所以需要delay一段时间。那在这个文件里需要用的Delay函数，还需要在上面加上#include"Delay.h"。

然后在这里Delay_ms(20) 20毫秒消抖。

接着我们还需要检测一下按键松手的情况，因为我们的按键一般是松手之后才有动作的，所以在这里加上一个while循环。判断条件还是读取PB1 == 0。这个如果按键一直按下，就卡在这里，直到松手。

那下面松手之后再delay 20毫秒消一下按键松手的抖动，接着我们赋值KeyNum = 1，用这个变量将键码1传递出去，这就是PB1按键的检测。

接着PB11的按键也是一样，我们复制粘贴一下这部分。然后GPIO_Pin_1这里改成11， PB11的键码，我们定义为2。最后是将KeyNum这个变量返回回去，这样就完成了按键的读取。

编写代码：Key.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"

/**
  * 函    数：按键初始化
  * 参    数：无
  * 返 回 值：无
  */
void Key_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);		//开启GPIOB的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_1 | GPIO_Pin_11;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB, &GPIO_InitStructure);						//将PB1和PB11引脚初始化为上拉输入
}

/**
  * 函    数：按键获取键码
  * 参    数：无
  * 返 回 值：按下按键的键码值，范围：0~2，返回0代表没有按键按下
  * 注意事项：此函数是阻塞式操作，当按键按住不放时，函数会卡住，直到按键松手
  */
uint8_t Key_GetNum(void)
{
	uint8_t KeyNum = 0;		//定义变量，默认键码值为0
	
	if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_1) == 0)			//读PB1输入寄存器的状态，如果为0，则代表按键1按下
	{
		Delay_ms(20);											//延时消抖
		while (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_1) == 0);	//等待按键松手
		Delay_ms(20);											//延时消抖
		KeyNum = 1;												//置键码为1
	}
	
	if (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_11) == 0)			//读PB11输入寄存器的状态，如果为0，则代表按键2按下
	{
		Delay_ms(20);											//延时消抖
		while (GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_11) == 0);	//等待按键松手
		Delay_ms(20);											//延时消抖
		KeyNum = 2;												//置键码为2
	}
	
	return KeyNum;			//返回键码值，如果没有按键按下，所有if都不成立，则键码为默认值0
}

```

我们来试一下，先把两个函数的第一行都复制到Key.h里的声明。之后到main点C里面，首先#include"Key.h"，然后主循环之前初始化一下按键。接着我们先定义一个全局变量KeyNum，用来存一下键码的返回值。

这个注意一下，虽然我们这个也叫KeyNum，但是这个KeyNum和这里的KeyNum不是一个变量，这里是全局变量，Key函数里面的是局部变量，两者的作用域不一样。即使我在主函数这里再定义一个同名的KeyNum，
他们三个也都是不一样的。在函数里面的是局部变量，只能在本函数里使用。在函数外面的是全局变量，每个函数都可以使用。

在函数里优先使用自己的局部变量，如果没有才会使用外部的全局变量。那我就用这个全局变量来接返回值。编译一下。

在主循环里写KeyNum = Key_GetNum();不断读取键码值放在KeyNum变量里。接着if (KeyNum == 1)，这就是按键1按下了，我们就可以执行对应的操作了。比如LED1_ON()点亮LED1，然后继续。

if (KeyNum == 2)，按键2按下，LED1_OFF()熄灭LED1，我们试一下编译。下载。按一下右边的按键1，LED1点亮，按一下左边的按键2，LED1熄灭，这些就是按键的基本操作了。

### 编写代码：按键1按下LED状态翻转，按键2按下，LED2状态翻转

那么演示的程序是按一下熄灭，再按一下点亮，也就是按键按下，LED的状态取反。这个该怎么实现呢？这就需要用到GPIO_ReadOutput的函数了。

我们可以在LDE的驱动里再加一个函数，void LED1_Turn(void)，调用这个函数LED1的状态取反，在里面我们可以写if。

这整个逻辑就是调用GPIO_ReadOutputDataBit的函数读取当前的端口输出状态。如果当前输出0就给它置1，否则就置0，这样就实现了端口的电平翻转。这也是GPIO_ReadOutput函数的用途，一般用于输出模式下。

编写代码：Key.c中void LED1_Turn(void) 实现LED1状态翻转

```c
/**
  * 函    数：LED1状态翻转
  * 参    数：无
  * 返 回 值：无
  */
void LED1_Turn(void)
{
	if (GPIO_ReadOutputDataBit(GPIOA, GPIO_Pin_1) == 0)		//获取输出寄存器的状态，如果当前引脚输出低电平
	{
		GPIO_SetBits(GPIOA, GPIO_Pin_1);					//则设置PA1引脚为高电平
	}
	else													//否则，即当前引脚输出高电平
	{
		GPIO_ResetBits(GPIOA, GPIO_Pin_1);					//则设置PA1引脚为低电平
	}
}

```

接着我们复制一下这个函数，给LED2也加上翻转的功能，这里改成LED2，GPIO_Pin_2。最后把这两个函数的第一行放到头文件里声明一下。这样我们就可以在主函数里使用这两个功能了。

编写代码：Key.h

```c
#ifndef __LED_H
#define __LED_H

void LED_Init(void);
void LED1_ON(void);
void LED1_OFF(void);
void LED1_Turn(void);
void LED2_ON(void);
void LED2_OFF(void);
void LED2_Turn(void);

#endif

```

比如，按键1按下，LED1_Turn，按键2，LED2_Turn，我们再试一下编译。下载。这时的现象就和我们演示的一样了。

最后注意一下这个驱动函数模块写好之后，尽量在这些函数的上面加一些注释，说明一下函数的用途，参数的取值和返回值的意思这些东西。这样别人在使用你的函数驱动时才知道咋用，对吧？

就像STM32的库函数一样，在每个函数上面写一下这样的注释。那我这里时间关系就不在视频里写注释了，我课下会写好提供给大家的。大家自己写代码也尽量打打注释，这样方便自己和别人理解。

## 光敏传感器控制蜂鸣器

## 光敏传感器控制蜂鸣器-硬件电路连接

好，第一个代码到这里就结束了，接着我们来写一下第二个代码，我们打开工程文件夹，先看一下接线图。

图：接线图-光敏传感器控制蜂鸣器

<br>
<div align="center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P8-%E6%8C%89%E9%94%AE%E6%8E%A7%E5%88%B6LED-%E5%85%89%E6%95%8F%E4%BC%A0%E6%84%9F%E5%99%A8%E6%8E%A7%E5%88%B6%E8%9C%82%E9%B8%A3%E5%99%A8-1.jpg" alt = "P8-按键控制LED-光敏传感器控制蜂鸣器-1.jpg" width = "70%" height = "auto">
</div>
<br>

在这里接了两个模块，左边的蜂鸣器和之前的接法还是一样。VCC。GND接电源，控制脚接PB12号口。

右边接的就是光敏传感器模块了。VCC，GND同样是接电源，DO数字输出端接PB13号口。

那我们拿出面包板先接一下蜂鸣器。VCC接电源正，GND接电源负，控制脚接PB12。然后是光敏传感器，VCC接电源正，GND接电源负，DO输出脚接PB13，上电，可以看到光敏传感器的灯亮了。

当我们遮住光线时，输出指示灯灭，代表输出高电平，松手时输出指示灯亮代表输出低电平。这个电位器可以调节高低电平的判断阈值，大家可以试一下，我就不演示了。

### 编写蜂鸣器的驱动程序Buzzer.c和Buzzer.h

接着我们回到工程文件夹，复制一下3-4的代码，改一下名叫3-5光敏传感器控制蜂鸣器。然后打开工程。

这个程序应该还是比较简单的，大家可能一看就会。那我们的主要步骤还是驱动程序的封装。我们封装一下蜂鸣器。

工程Hardware下新建Buzzer.c和Buzzer.h。

编写代码：Hardware/Buzzer.h

```c
#ifndef __BUZZER_H
#define __BUZZER_H

#endif
```

Buzzer.c中的函数逻辑和LED.c中几乎一样，我们直接把LED.c复制过来修改。

编写代码：Hardware/Buzzer.c

蜂鸣器是GPIOB和GPIO_Pin_12

```c
#include "stm32f10x.h"                  // Device header

/**
  * 函    数：蜂鸣器初始化
  * 参    数：无
  * 返 回 值：无
  */
void Buzzer_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);		//开启GPIOB的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_12;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB, &GPIO_InitStructure);						//将PB12引脚初始化为推挽输出
	
	/*设置GPIO初始化后的默认电平*/
	GPIO_SetBits(GPIOB, GPIO_Pin_12);							//设置PB12引脚为高电平
}

/**
  * 函    数：蜂鸣器开启
  * 参    数：无
  * 返 回 值：无
  */
void Buzzer_ON(void)
{
	GPIO_ResetBits(GPIOB, GPIO_Pin_12);		//设置PB12引脚为低电平
}

/**
  * 函    数：蜂鸣器关闭
  * 参    数：无
  * 返 回 值：无
  */
void Buzzer_OFF(void)
{
	GPIO_SetBits(GPIOB, GPIO_Pin_12);		//设置PB12引脚为高电平
}

/**
  * 函    数：蜂鸣器状态翻转
  * 参    数：无
  * 返 回 值：无
  */
void Buzzer_Turn(void)
{
	if (GPIO_ReadOutputDataBit(GPIOB, GPIO_Pin_12) == 0)		//获取输出寄存器的状态，如果当前引脚输出低电平
	{
		GPIO_SetBits(GPIOB, GPIO_Pin_12);						//则设置PB12引脚为高电平
	}
	else														//否则，即当前引脚输出高电平
	{
		GPIO_ResetBits(GPIOB, GPIO_Pin_12);						//则设置PB12引脚为低电平
	}
}

```

这样蜂鸣器的驱动程序就完成了。我们把函数也都放到头文件里，声明一下。

编写代码：Hardware/Buzzer.h

```c
#ifndef __BUZZER_H
#define __BUZZER_H

void Buzzer_Init(void);
void Buzzer_ON(void);
void Buzzer_OFF(void);
void Buzzer_Turn(void);

#endif

```

我们在main.c中编写。

编写代码：Hardware/main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "Buzzer.h"


int main(void)
{
	/*模块初始化*/
	Buzzer_Init();			//蜂鸣器初始化
	
	while (1)
	{
		Buzzer_ON();
		Delay_ms(500);
		Buzzer_OFF();
		Delay_ms(500);
		Buzzer_Turn();
		Delay_ms(500);
		Buzzer_Turn();
		Delay_ms(500);
	}
}

```

编译。下载，这时听到蜂鸣器不断鸣响，说明驱动函数没有问题。

### 编写光敏传感器的驱动程序Buzzer.c和Buzzer.h

工程Hardware下新建LightSensor.c和LightSensor.h。

光敏传感器初始化函数中 GPIOB 上拉输入 GPIO_Pin_13

光敏传感器的读取函数，返回端口值。

编写代码：Hardware/LightSensor.c

```c
#include "stm32f10x.h"                  // Device header

/**
  * 函    数：光敏传感器初始化
  * 参    数：无
  * 返 回 值：无
  */
void LightSensor_Init(void)
{
	/*开启时钟*/
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);		//开启GPIOB的时钟
	
	/*GPIO初始化*/
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOB, &GPIO_InitStructure);						//将PB13引脚初始化为上拉输入
}

/**
  * 函    数：获取当前光敏传感器输出的高低电平
  * 参    数：无
  * 返 回 值：光敏传感器输出的高低电平，范围：0/1
  */
uint8_t LightSensor_Get(void)
{
	return GPIO_ReadInputDataBit(GPIOB, GPIO_Pin_13);			//返回PB13输入寄存器的状态
}

```

编写代码：Hardware/LightSensor.h

```c
#ifndef __LIGHT_SENSOR_H
#define __LIGHT_SENSOR_H

void LightSensor_Init(void);
uint8_t LightSensor_Get(void);

#endif

```

编写代码：User/main.c

```c
#include "stm32f10x.h"                  // Device header
#include "Delay.h"
#include "Buzzer.h"
#include "LightSensor.h"

int main(void)
{
	/*模块初始化*/
	Buzzer_Init();			//蜂鸣器初始化
	LightSensor_Init();		//光敏传感器初始化
	
	while (1)
	{
		if (LightSensor_Get() == 1)		//如果当前光敏输出1
		{
			Buzzer_ON();				//蜂鸣器开启
		}
		else							//否则
		{
			Buzzer_OFF();				//蜂鸣器关闭
		}
	}
}

```

现象就是遮住光敏传感器，蜂鸣器响，不遮住，就不响。这就完成了我们本节课的第二个代码。本节课的内容到这里就差不多了。

## GPIO总结

最后我们来总结一下GPIO的使用方法，总体上来说还是比较简单的。

首先初始化时钟，然后定义结构体，赋值结构体。GPIO_Mode可以选择我们讲的那8种输入输出模式。GPIO_Pin选择引脚，可以用按位或的方式同时选中多个引脚。GPIO_Speed选择输出速度，这个不是很重要，要求不高的话直接选50MHz就行了。

最后使用GPIO_Init函数将指定的GPIO外设初始化好，然后这里有8个读取和写入的函数，我们读写GPIO口主要就用这些函数就行，具体用法我也都讲过，之后我们学习了模块化编程的方法。

一般我们自己做一个产品的话，外围硬件比较多。这时候就要尽量把每个硬件的驱动函数单独提取出来，分装在.c和.h文件里，这样有利于简化主函数的逻辑。

在主函数中我们还有更重要的任务要完成，不要让这些驱动函数混在主函数里影响我们。另外把硬件驱动提取出来，也有利于我们移植程序，还有利于我们进行分工合作。

比如让别人来写驱动函数，你的主要精力就可以集中在主程序的逻辑上了。最后既然要做分装，那函数的注释就需要写清楚，这样可以方便使用你这个模块的人快速上手这些函数。

好，以上就是本节课的全部内容了，我们下节再见。

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