---
title: "【浅谈LLC谐振变换器】- 章节2:LLC工作原理"
date: 2023-11-05T00:17:58+08:00
lastmod: 2023-11-05T00:17:58+08:00
author: ["Eddy"]
keywords: 
- LLC resonant converter
- switching mode power supply
- Power technology
- LLC谐振变换器
- LLC电力电子变压器
categories: 
- 
tags: 
- LLC resonant converter
- switching mode power supply
- Power technology
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/pcb2.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---
> **谐振变换器-第3节**
> 
感谢您打开这篇博客，在这里，每次启程都承载着新的发现与学习的机会。

本节为《浅谈谐振LLC谐振变换器》系列第3节，本系列围绕LLC谐振变换器展开，一个在现代电子设备中不可或缺的神秘组件。它利用巧妙的物理原理有效减少能量损失，保证了我们设备的高效运行。 

本系列学习路线：LLC的定义及优势 - LLC入门 - LLC工作原理 - LLC特性分析
#### ZVS和ZCS软开关

**硬开关**

![02image.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/02image.png)

开关管在开通和关断时，开关管源漏极电压与电流会出现重叠的现象，即有额外的开关损耗产生，这种开通与关断称为硬开关。

**软开关**

![02image1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/02image1.png)

在开关管导通前，使用特定的措施使得开关管两端电压预先下降到零，随后开通开关管，即可实现软开通（ZVS，零电压开通）；
在开关管关断前，使用特定的措施便得开关管的电流预先下降到零，随后关闭开关管，即可实现软关断（ZCS，零电流关断）



#### 半桥LLC主电路结构分析

![02image2.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/02image2.png)

整流网络有两种情况：全波整流和全桥整流。

全波整流适用于低压高电流场合，全桥整流适用于高压低电流场合。



#### 三种频率条件下的模态与关键波形分析

LLC谐振变换器存在以下三种工作模式

> 开关频率fs 谐振频率fr

工作模式1：fs<fr

该模式下，励磁电感Lm部分时间参与谐振，LLC谐振变换器不仅能实现原边侧开关管零电压开通（ZVS），也能同时实现副边侧整流部分二极管零电流关断（ZCS）。

工作模式2：fs=fr

该模式下，励磁电感Lm始终被输出电压嵌位，一直未参与谐振，其两端电压一直被输出电压嵌位在nVo。 整流二极管电流临界连续，因此同样能够实现ZCS关断。

工作模式3：fs>fr

该模式下，励磁电感Lm始终被输出电压嵌位，一直未参与谐振。原边开关管可实现ZVS开通，但副边侧整流二极管上的电流不能ZCS关断，且换流时因反向恢复产生损耗。

综上，fs≤fr时，可以实现原边ZVS开通和ZSC关断。fs>fr时，可以实现原边ZVS开通，不能实现副边ZSC关断。



工作模式1：一共有8种模态。

1）Q1 OFF，Q2 ON

2）Q1 OFF，Q2 OFF

3）Q1 OFF，Q2 OFF

4）Q1 ON，Q2 OFF

5）Q1 ON，Q2 OFF

6）Q1 OFF，Q2 OFF

7）Q1 OFF，Q2 OFF

8）Q1 OFF，Q2 ON



工作模式2：一共有6种模态。

1）Q1 OFF，Q2 ON

2）Q1 OFF，Q2 OFF

3）Q1 ON，Q2 OFF

4）Q1 ON，Q2 OFF

5）Q1 OFF，Q2 OFF

6）Q1 OFF，Q2 ON



工作模式3：一共有6种模态。

1）Q1 OFF，Q2 ON

2）Q1 OFF，Q2 OFF

3）Q1 ON，Q2 OFF

4）Q1 ON，Q2 OFF

5）Q1 OFF，Q2 OFF

6）Q1 OFF，Q2 ON



LLC谐振半桥电容模式（fs~fr2）：为什么必须避免

当fs接近fr2时，会遇到电容模式，虽然在电容模式下可以实现ZCS，但是ZVS会丢失。

这会导致：

- Q1和Q2的硬开关

- Q1和Q2体二极管反向恢复：导通时高电流尖峰，额外功耗，mosfet很容易爆炸。

- 产生高水平的电磁干扰

- 在HB中点的大的负电压尖峰可能会导致控制IC失效



反馈回路符号可以由负变正：

- 在电容模式下，能量与频率的关系是相反的

- 转换器的工作频率将跑向最小值（假如mosfet没有爆炸）



## 参考/感谢

[1]ST SimplifiedAnalysis and Design of Series resonantLLC HalfbridgeConverters 

[2][https://www.bilibili.com/video/BV143411u74r/?spm_id_from=333.788&vd_source=c57cc7d724946a8cfa6381f148e147d5](https://www.bilibili.com/video/BV143411u74r/?spm_id_from=333.788&vd_source=c57cc7d724946a8cfa6381f148e147d5)

🚉尊敬的旅客朋友们，已到站，本站是：LLC工作原理，请需要下车的乘客带好随身物品有序下车。

⏩下一站是：LLC特性分析

