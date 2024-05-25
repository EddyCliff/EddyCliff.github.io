---
title: "【温故而知新】单片机基础知识回顾"
date: 2024-05-01T00:17:58+08:00
lastmod: 2024-05-01T00:17:58+08:00
author: ["Eddy"]
keywords: 
- MCU
- 单片机
categories: 
- 
tags: 
- MCU
- 单片机
description: 
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/c2.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/STM32-standard-library-learning-JKD/P11-EXTI%E5%A4%96%E9%83%A8%E4%B8%AD%E6%96%AD-1.jpg" alt = "P11-EXTI外部中断-1.jpg" width = "70%" height = "auto">
</div>
<br>

## 一、单片机三要素？

单片机最小系统的三要素为：电源、晶振和复位电路。

## 二、单片机最小系统由哪几个部分组成？

单片机最小系统由五部分组成，分别是MCU(微处理控制单元)、电源、时钟电路/晶振、复位电路、程序加载口。其中三要素是电源、时钟电路和复位电路。

## 三、IO口工作方式

上拉输入，下拉输入，推挽输出，开漏输出。

## 四、UART有几根线？

串行，异步，全双工。2根线 Rx，Tx。

UART是通用异步串行数据总线，是一种全双工传输总线。UART在单片机中是最基本的配置，作为单片机的串口，它有TX(数据发送接口)和RX(数据接收接口)两个接口。使用UART总线接口进行通信的时候，两个设备间的TX和RX相连，RX和TX相连即可。

## 五、SPI是什么？有几条线？几种模式？

串行，同步，全双工。3根或4根。

SPI，是一种高速的，全双工，同步的通信总线，在芯片的管脚上只占用四根线。

它以主从方式工作，这种模式通常有一个主设备和一个或多个从设备。

四根线分别是：SDI(数据输入)、SDO(数据输出)、SCLK(时钟)、CS(片选)。

需要说明的是，我们SPI通信有4种不同的模式，不同的从设备可能在出厂是就是配置为某种模式，这是不能改变的；但我们的通信双方必须是工作在同一模式下，所以我们可以对我们的主设备的SPI模式进行配置，通过CPOL（时钟极性）和CPHA（时钟相位）来控制我们主设备的通信模式。

CPOL（时钟极性）决定了通信过程中的有效电平：极性是0，表示空闲状态是低电平，有效电平是高电平；极性是1，表示空闲状态是高电平，有效电平是低电平。

CPHA（时钟相位）决定了采集的时候是在时钟脉冲的第一个还是第二个边沿。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

## 六、使用I2C总线时需要考虑哪些问题

一般需要考虑的问题包括：信号线上拉电阻、信号线负载电容、信号线串联电阻。

## 七、I2C需不需要上拉电阻？为什么？

上拉电阻是指将不确定的信号钳位在高电平，同时起限流作用的电阻。由于I2C通信是开漏输出的(只能输出低电平不能输出高电平)，因此需要加上拉电阻，使其可以输出高电平。

## 八、DMA有什么用

DMA用来提供在外设和存储器之间或者存储器和存储器之间的高速数据传输。无须CPU的干预，通过DMA数据可以快速地移动。这就节省了CPU的资源来做其他操作。

## 九、STM32 中断是怎么进入到中断服务程序的

在STM32中，为了区分不同的中断，每个设备有自己的中断号。系统有0-255一共256个中断。系统有一张中断向量表，用于存放256个中断的中断服务程序入口地址。每个入口地址对应一段代码，即中断服务程序。

## 十、单片机上电后没有运转，首先要检查什么

1. 检查电源是否正常，若装有复位芯片，也需查看复位芯片是否工作正常。
    
2. 检查硬件复位电路是否正常。
    
3. 查看外部晶振是否启振，一般用示波器X10挡位，应选取较高带宽.
    
4. 查看BOOT位设置启动方式是否正确。

## 十一、请说明总线接口USRT、I2C、USB的异同点

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-1.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>



## 十二、I2C总线通信原理

目录

1）I2C总线简介

2）I2C总线协议

3）I2C总线读写操作

4）STM32F0-I2C控制器特性

### I2C总线简介

**I2C总线介绍**

I2C总线（也称IIC或I2C）是由PHILIPS公司开发的两线式串行总线，用于连接微控制器及其外围设备，是微电子通信控制领域广泛采用的一种总线标准。它是同步通信的一种特殊形式，具有接口线少，控制方式简单，期间封装形式少，通信速率高等优点。

**I2C总线特征**

- 两条总线线路：一条串行数据SDA，一条串行时钟线SCL来完成数据的传输及外围器件的扩展。
    
- I2C总线上的每一个设备都可以作为主设备或者从设备，而且每一个设备都对应一个唯一的地址。
    
- I2C总线数据传输速率在标准模式下可达100kbit/s，快速模式性爱可达400kbit/s，高速模式下可达3.4Mbit/s。一般可通过I2C总线接口可编程时钟来实现传输速率的调整，同时也跟所接的上拉电阻的阻值有关。
    
- I2C总线上的主设备与从设备之间以字节（8位）为单位进行单双工的数据传输。
    

### I2C总线物理-拓扑结构

I2C 总线在物理连接上非常简单，分别由SDA(串行数据线)和SCL(串行时钟线)及上拉电阻组成。通信原理是通过对SCL和SDA线高低电平时序的控制，来产生I2C总线协议所需要的信号进行数据的传递。在总线空闲状态时，这两根线一般被上面所接的上拉电阻拉高，保持着高电平。I2C通信方式为半双工，只有一根SDA线，同一时间只可以单向通信，485也为半双工，SPI和uart为双工。



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-2.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>


### I2C总线协议

- I2C协议规定：总线上数据的传输必须以一个起始信号作为开始条件，以一个结束信号作为传输的停止条件。起始和结束信号总是由主设备产生。总线在空闲状态时，SCL和SDA都保持着高电平。
    
- 起始信号：当SCL为高电平而SDA由高到低的跳变，表示产生一个起始条件。
    
- 结束信号：当SCL为高而SDA由低到高的跳变，表示产生一个停止条件。
    
- 数据传输：
    
- 数据传输以**字节**为单位，主设备在SCL线上产生每个时钟脉冲的过程中将在SDA线上传输一个数据位，数据在时钟的**高电平**被采样，一个字节按数据位从**高位到低位**的顺序进行传输。
    
- 主设备在传输有效数据之前，要先**指定从设备的地址**，**一般为7位**，然后再发生数据传输的方向位，**0**表示主设备向从设备**写**数据，**1**表示主设备向从设备**读**数据
    
- 应答信号：
    
- **接收数据的器件**在接收到8bit数据后，向发送数据的器件发出**低电平**的应答信号，表示已收到数据。这个信号可以是主控器件发出的，也可以是从动器件发出的。总之，由接收数据的器件发出。
    

**通俗来讲：**

I2C协议规定，总线上数据的传输**必须**以一个起始信号作为开始条件，以一个结束信号作为传输的停止条件。起始和结束信号**总是**由主设备产生(意味着从设备不可以主动通信？所有的通信都是主设备发起的，主可以发出询问的command，然后等待从设备的通信)。

**起始和结束信号产生条件**：

总线在空闲状态时，SCL和SDA都保持着高电平：

- 当SCL为高电平而SDA由高到低的跳变，表示产生一个起始条件；
    
- 当SCL为高而SDA由低到高的跳变，表示产生一个停止条件。
    

在起始条件产生后，总线处于忙状态，由本次数据传输的主从设备独占，其他I2C器件无法访问总线；而在停止条件产生后，本次数据传输的主从设备将释放总线，总线再次处于空闲状态。

**起始和结束如图所示**：

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-3.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>


再来看看在这个过程中数据的传输是如何进行的。前面我们已经提到过，数据传输以字节为单位（8bit位）。

主设备在SCL线上产生每个时钟脉冲的过程中将在SDA线上传输一个数据位，当一个字节按数据位从高位到低位的顺序传输完后，紧接着从设备将拉低SDA线，回传给主设备一个**应答位**， 此时才认为一个字节真正的被传输完成。

当然，并不是所有的字节传输都必须有一个应答位，比如：当从设备不能再接收主设备发送的数据时，从设备将回传一个否 定应答位。

**传输的过程**如图所示：

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-4.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

在前面我们还提到过，I2C总线上的每一个设备都对应一个唯一的地址，主从设备之间的数据传输是建立在地址的基础上，也就是说，主设备在传输有效数据之前要先指定从设备的地址，地址指定的过程和上面数据传输的过程一样，只不过大多数从设备的地址是7位的，

然后协议规定再给地址添加一个最低位用来表示接下来数据传输的方向，0表示主设备向从设备写数据，1表示主设备向从设备读数据。

**向指定设备发送数据的格式**如图所示：(每一最小包数据由9bit组成，8bit内容+1bit ACK, 如果是地址数据，则8bit包含1bit方向)：

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-5.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

### I2C总线读写操作

对I2C总线的操作实际就是主从设备之间的读写操作。**大致可分为以下三种操作情况**：

1、主设备往从设备中写数据。数据传输格式如下：

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-6.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>



2、主设备从从设备中读数据。数据传输格式如下：


<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-7.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

3、主设备往从设备中写数据，然后重启起始条件，紧接着从从设备中读取数据；或者是主设备从从设备中读数据，然后重启起始条件，紧接着主设备往从设备中写数据。数据传输格式如下：



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-8.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>


### STM32F0-I2C控制器特性

- 软件模拟I2C时序

由于直接控制GPIO引脚电平产生通讯时序时，需要由 CPU 控制每个时刻的引脚状态，所以称之为“软件模拟协议”方式。

- 硬件控制产生I2C时序

STM32的I2C片上外设专门负责I2C通讯协议，只要配置好该外设，它就会自动根据协议要求产生通讯信号，收发数据并缓存起来，CPU只要检测外设的状态和访问数据寄存器，就能完成数据收发。这种由硬件外设处理I2C协议的方式减轻了CPU的工作，且使软件设计更加简单。

- I2C的主要特点

I2C总线规范rev03兼容性：

- 从机模式和主机模式
    
- 多主机功能
    
- 标准模式（高达100kHz）
    
- 快速模式（高达400kHz）
    
- 超快速模式（高达1MHz）
    
- 7位和10位地址模式
    
- 软件复位
    
- 1字节缓冲带DMA功能
    

### 总结

启示信号：SCL高，SDA出现下降沿

结束信号：SCL高，SDA出现上升沿

应答信号：在数据传输后(8位)，从机拉低SDA，则为应答ACK

写：在地址（7位）后加上0，发三（从机地址，寄存器地址，数据）应答三；

读：先假写（7位后+0），再读（7位后+1），启动两次，发三（从机地址，寄存器地址，从机地址）

读一（读就行了）应答3，最后没有应答，从机写完数据之后就释放总线。

## 十三、STM32总线协议通信的相关概念

### 同步通信和异步通信

- 通信，最少要有两个对象，一个收，一个发
    
- 同步通信：
    

一般情况下同步通信指的是通信双方根据同步信号进行通信的方式。比如通信双方有一个共同的时钟信号，大家根据时钟信号的变化进行通信。

SPI I2C就是同步通信。



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-9.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>


- 异步通信：

是指数据传输速度匹配依赖于通信双方自己独立的系统时钟，大家约定好通信的速度。异步通信不需要同步信号，但是并不是说通信的过程不同步。

优点：不受时钟的限制

缺点：通讯速度相比同步通信更慢，通信数据相比同步通信更小

UART就是异步通信。



### 串行通信和并行通信的区别

- 串行通信：指的是同一时刻只能收或发一个bit位信息、因此只用1根信号线即可。
    
- 并行通信：指的是同一时刻可以收或发多个bit位的信息，因此需要多根信号线才行。
    
- 串行传输：数据按位顺序传输
    
- 优点：占用引脚资源较少
    
- 缺点：速度相对较慢
    

比如我们通信的距离非常长，假如选择并行通信的话就要接很多的电器线路，就合很耗费电器线路的资源。

- 并行传输：数据各个位同时传输
    
- 优点：速度快
    
- 缺点：占用引脚资源多（IO口资源是有限的）
    

### 单工，半双工，全双工



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-10.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>


- 单工：要么收，要么发，只能做接收设备或者发送设备。比如收音机
    
- 半双工：可以收，可以发，但是不能同时收发，比如对讲机
    
- 全双工：可以在同一时刻既接收又发送。比如手机。


## 十四、SPI总线通信协议原理

我们之前讲解了 I2C，I2C 是串行通信的一种，只需要两根线就可以完成主机和从机之间的通信，但是 I2C 的速度最高只能到 400KHz，如果对于访问速度要求比价高的话 I2C 就不适合了。

本章我们就来学习一下另外一个和 I2C 一样广泛使用的串行通信：SPI，SPI 全称是 Serial Perripheral Interface，也就是串行外围设备接口。SPI 是 Motorola 公司推出的一种同步串行接口技术，是一种高速、全双工的同步通信总线，SPI 时钟频率相比 I2C 要高很多，最高可以工作在上百 MHz。SPI 以主从方式工作，通常是有一个主设备和一个或多个从设备，一般 SPI 需要4 根线，但是也可以使用三根线(单向传输)，本章我们讲解标准的 4 线 SPI，这四根线如下：

①、CS/SS，Slave Select/Chip Select，这个是片选信号线，用于选择需要进行通信的从设备。

I2C 主机是通过发送从机设备地址来选择需要进行通信的从机设备的，SPI 主机不需要发送从机设备，直接将相应的从机设备片选信号拉低即可。

②、SCK，Serial Clock，串行时钟，和 I2C 的 SCL 一样，为 SPI 通信提供时钟。

③、MOSI/SDO，Master Out Slave In/Serial Data Output，简称主出从入信号线，这根数据线只能用于主机向从机发送数据，也就是主机输出，从机输入。

④、MISO/SDI，Master In Slave Out/Serial Data Input，简称主入从出信号线，这根数据线只能用户从机向主机发送数据，也就是主机输入，从机输出。

SPI 通信都是由主机发起的，主机需要提供通信的时钟信号。主机通过 SPI 线连接多个从设备的结构如图所示



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-11.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

SPI 有四种工作模式，通过串行时钟极性(CPOL)和相位(CPHA)的搭配来得到四种工作模式：

①、CPOL=0，串行时钟空闲状态为低电平。

②、CPOL=1，串行时钟空闲状态为高电平，此时可以通过配置时钟相位(CPHA)来选择具体的传输协议。

③、CPHA=0，串行时钟的第一个跳变沿(上升沿或下降沿)采集数据。

④、CPHA=1，串行时钟的第二个跳变沿(上升沿或下降沿)采集数据。

SPI四种工作模式



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-12.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

跟 I2C 一样，SPI 也是有时序图的，以 CPOL=0，CPHA=0 这个工作模式为例，SPI 进行全双工通信的时序如图所示：

图：SPI时序图



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-13.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

从图可以看出，SPI 的时序图很简单，不像 I2C 那样还要分为读时序和写时序，因为 SPI 是全双工的，所以读写时序可以一起完成。图 27.1.1.3 中，CS 片选信号先拉低，选中要通信的从设备，然后通过 MOSI 和 MISO 这两根数据线进行收发数据，MOSI 数据线发出了0XD2 这个数据给从设备，同时从设备也通过 MISO 线给主设备返回了 0X66 这个数据。

## 十五、STM32启动方式

3种

1、从Flash启动，将Flash地址0x0800 0000映射到0x00000000，这样启动以后就相当于从0x08000000开始的，这是我们最常用的模式；

2、从SRAM启动，将SRAM地址0x20000000映射到0x00000000，这样启动以后就相当于从0x20000000开始的，用于调试，笔者基本没用过

3、从系统存储器启动(System memory)，将系统存储器地址0x1FFFF000映射到0x00000000，这样启动以后就相当于从0x1FFFF000开始执行的，值得注意的是这个系统存储器里面存储的其实是STM32自带的Bootloader代码，这其实是一个官方的IAP，它提供了可以通过UART1接口将用户的代码下载到Flash中的功能,然后将boot0置0，复位单片机，便可以Flash启动。

注：系统内存存储器我们没办法烧些，因为他是一个只读的ROM，我们只能读取，但是可以通过从系统存储器启动，通过串口UART1烧录代码。



<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Exp-sticker/%E5%8D%95%E7%89%87%E6%9C%BA%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E5%9B%9E%E9%A1%BE-14.png" alt = "INIT.jpg" width = "70%" height = "auto">
</div>
<br>

## 十六、STM32上电到main函数之前做了什么事？

```c
; Reset handler

Reset_Handler PROC :定义函数

			  EXPORT Reset_Handler [WEAK]

	IMPORT __main : 引入函数

	IMPORT SystemInit : 引入函数

		LDR R0, =SystemInit : 将函数放到R0

		BLX R0 : 执行

		LDR R0, =__main

		BX R010 ENDP
```

1、初始化堆栈指针SP=_initial_sp，启动方式不同，指向地址不同。

2、初始化PC（通用寄存器指针）指针=Reset_Handler，若果是Flash启动就是0x0800 0000

3、SystemInit（配置系统时钟）

4、__main(MDK自带的函数)，初始化堆栈，.data .bss

5、最终调用main 函数去到C 的世界

## 十七、开漏输出和推挽输出的区别？

推挽输出的最大特点是可以真正能真正的输出高电平和低电平，在两种电平下都具有驱动能力。常说的与推挽输出相对的就是开漏输出，对于开漏输出和推挽输出的区别最普遍的说法就是开漏输出无法真正输出高电平，即高电平时没有驱动能力，需要借助外部上拉电阻完成对外驱动。

所谓的驱动能力，就是指输出电流的能力。