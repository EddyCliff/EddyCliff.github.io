---
title: "【浅谈LLC谐振变换器】- 章节0:LLC的定义以及优势"
date: 2023-11-05T00:17:58+08:00
lastmod: 2023-11-05T00:17:58+08:00
author: ["Eddy"]
keywords: 
- LLC resonant converter
- switching mode power supply
- Power technology
categories: 
- 
tags: 
- LLC resonant converter
- switching mode power supply
- Power technology
description: 
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/pcb2.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---
> **谐振变换器-第1节**
> 
感谢您打开这篇博客，在这里，每次启程都承载着新的发现与学习的机会。

本节将开始《浅谈谐振LLC谐振变换器》系列，本系列围绕LLC谐振变换器展开，一个在现代电子设备中不可或缺的神秘组件。它利用巧妙的物理原理有效减少能量损失，保证了我们设备的高效运行。

在接下来的学习中，无论您是新手还是老手，我都希望这次学习能为您带来新的灵感和知识。请您准备好您的工具包，我们即将启程，深入探索LLC谐振变换器的奇妙世界。期待在旅途中与您共同探讨、学习，并享受这一探索之旅。  

本系列学习路线：LLC的定义及优势 - LLC入门 - LLC工作原理 - LLC特性分析  

## LLC的定义

### LLC概念

LLC谐振转换器是使用带有电容器和两个电感器的三元件谐振电路，它拥有高工作频率，并在宽输出负载调节范围内为高功率系统实现良好的效率。

这减少了所需元件的尺寸和组件的数量，使得最终成本额降低。

LLC转换器像其他谐振转换器一样，由3个模块组成：电源开关，谐振回路，二极管整流器。

首先打开MOS开关管，将直流电压转换为高频方波，方波进入谐振回路后，消除了方波的谐波，输出基频的正弦波，正弦波通过高频变压器传输到转换器的次级侧，根据输出要求升高或降低电压，最后，正弦波通过二极管整流器转换为稳定的直流输出。

### LLC的电源开关

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image.png" width = "70%" height = "auto">
</div>


LLC的电源开关可以在全桥或者半桥拓扑中实现，

它们的主要区别是：

全桥拓扑结构生成无直流偏移的方波输出，幅度等于输入电压，范围从正波到负波。

半桥拓扑生成了一个方波，该方波使得输出电压的一半发生偏移。

因此，全桥拓扑和半桥拓扑这两种拓扑结构存在振幅差异。

**全桥拓扑：**

- 缺点：需要耗费更多晶体管，这使得它价格高昂一些，此外，由于晶体管数量更多，电路中将有更多RDS处于开启状态，这会增加传导的损耗。

- 优点：全桥拓扑将变压器传输比降低一半，从而最大限度地减少变压器中的铜损耗。

**半桥拓扑：**

- 优点：实施起来更具成本优势，并且增加了电容器两端RMS电流的优势，将此电路减少了原来的15%。

- 缺点：增加了开关损耗。

**综上：**

我们简易功率低于1kw的应用应该使用半桥电源开关拓扑。高于1kw的使用全桥电源开关拓扑。



## 为什么我们在隔离谐振转换器中只使用LLC而不用其他谐振回路？

**主要有两个原因：**

**原因1：**

每当我们设计一个系统时，我们都需要考虑该系统的成本竞争力。

通常情况下，半桥拓扑可以提供成本最低的开关网络。

我们需要找到一个可以使用变压器参数的谐振回路，一旦能够实现这一点，我们就可以减少谐振电路的额外元件数量。

理想变压器的磁化电感与其输入绕组并联，我们可以将其用作与谐振回路输出端口并联的励磁电感Lm。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image1.png" width = "70%" height = "auto">
</div>


在现实世界中，变压器总是带有与变压器绕组串联的漏电感，如果我们以某种方式使用这个串联漏电感作为谐振电感Lr，我们就能降低电路的成本。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image2.png" width = "70%" height = "auto">
</div>


**原因2：**

LLC的谐振回路由一个谐振电容器和两个电感器组成。

与电容器和变压器串联的谐振电感器以及并联的磁化电感器。

这里容器的作用是滤除方波中的谐波，并提供变压器输入端基本开关频率的正弦波。

谐振回路的增益会根据频率和施加到输出侧的负载而变化。

我们可以这么认为，当电源能够很好的提供电量的时候，这个电源就是一个稳健的设计。

那么负载调节量以及谐振回路的响应是否会随负载变化而给设计带来挑战呢？

我们必须考虑开关频率和输出负载来调整谐振回路的响应，以确保转换器通过设计谐振回路的增益在广泛的负载范围内高效的运行。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image3.png" width = "70%" height = "auto">
</div>


由于谐振回路的双电感器，LLC转换器的工作范围较宽。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image4.png" width = "70%" height = "auto">
</div>


为了解其工作原理，我们将根据要求考虑容器对重载和轻载的响应。

让我们将变压器的整个次级侧视为谐振回路的负载端。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image5.png" width = "70%" height = "auto">
</div>

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image6.png" width = "70%" height = "auto">
</div>

#### 仅含Lm和Cr的回路

如果谐振回路仅由谐振电容器和磁化电感器组成，让我们看一看在一定负载范围内的谐振增益是怎么样的。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image12.png" width = "70%" height = "auto">
</div>


当负载较轻时，谐振回路的增益有一个明显的峰值，然而，重负载的增益没有上限，它能够响应衰减变化并且仅在非常高的频率下能实现单位增益。

---

#### 仅含Lr和Cr的回路

现在，如果谐振回路仅由谐振电容器和与之串联的谐振电感器Lr组成，则结果会有所不同。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image7.png" width = "70%" height = "auto">
</div>


当负载较轻时，这中间的增益不会超过单位增益。

但当负载较重时，谐振回路达到单位增益的速度则比并联电感器Lm快得多。

---

因此，通过谐振回路中实施两个电感器，我们得到了频率增益响应，确保转换器能够响应更大范围的负载。

此外，它可以在整个负载范围内实现稳定的控制。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image11.jpeg" width = "70%" height = "auto">
</div>


#### LLC的两个谐振频率

对LLC来说，有两个谐振频率，

一个谐振频率fr是利用谐振电感Lr谐振电容Cr组成；

另一个谐振频率fm是利用谐振电感Lr，励磁电感Lm，谐振电容Cr一起组成；

$f_{r1}=\frac{1}{2\cdot\pi\cdot\sqrt{L_r\cdot C_r}}$

$f_{r2}=\frac1{2\cdot\pi\cdot\sqrt{\left(L_r+L_m\right)\cdot C_r}}$

这既可以利用PR（并联谐振）的升压功能，也可以利用SR（串联谐振）特性的降压功能。

---

让我们看看谐振参数变化时产生的影响

随着Lm/Lr比例增加，增益曲线的峰值降低，这意味着电压调节范围减小了。

如果Lr/Cr比例增加，两个谐振频率之间的差异会增加，这意味着工作频率变化增加。

因此两个比例（Lm/Lr）（Lr/Cr）都需要足够低，使得电路具有较宽的电压调节范围和较窄的工作频率变化。

---

然而，电流ILm在输入端的开关网络和谐振回路之间循环 ,并且由于隔离的原因不会传输到输出端。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00screenshots.gif" width = "70%" height = "auto">
</div>

如果降低Lm以在宽电压调节范围内保持低Lm/Lr比例，ILm数值会增加，因此会降低转换器的效率。

因此，在设计过程中必须考虑效率和稳压的范围。

正如我们所见，转换器的效率来自谐振回路，LLC转换器的谐振回路可在初级侧和次级侧实现软开关。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image8.png" width = "70%" height = "auto">
</div>


它在输入开关上启用ZVS，此外，如果开关频率介于两个谐振频率之间，我们可以在输出整流器上实现零电流开关。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image9.png" width = "70%" height = "auto">
</div>


LLC拓扑可节省电路板空间，LLC拓扑没有输出电感器，这意味着所有电感器可以更轻松地集成到单个磁性结构，以节省面积和成本。

<div align = "center">
<img src = "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/00image10.png" width = "70%" height = "auto">
</div>


简而言之，可以通过LLC串并联谐振变换器来提高开关频率以减小转换器尺寸，同时保持较高的转换器效率。

比如，我们可以使用氮化镓（GaN）FET设计一个1kw输出的LLC串联谐振转换器，其开关频率为1MHz，并实现高达95%的效率，与此同时它的尺寸和重量会缩小到可以装进我们的午餐盒里。

这就是LLC谐振变换器被用于高端消费电子产品，服务器和电信站等精密系统的电源应用的原因。



## 参考/感谢

[1][https://www.bilibili.com/video/BV1Sh411T7XV/?spm_id_from=333.999.0.0](https://www.bilibili.com/video/BV1Sh411T7XV/?spm_id_from=333.999.0.0)


🚉尊敬的旅客朋友们，已到站，本站是：LLC的定义及优势，请需要下车的乘客带好随身物品有序下车。

⏩下一站是：LLC入门