---
title: "【浅谈LLC谐振变换器】- 章节1:LLC入门"
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

> **谐振变换器-第2节**
> 
感谢您打开这篇博客，在这里，每次启程都承载着新的发现与学习的机会。

本节为《浅谈谐振LLC谐振变换器》系列第2节，本系列围绕LLC谐振变换器展开，一个在现代电子设备中不可或缺的神秘组件。它利用巧妙的物理原理有效减少能量损失，保证了我们设备的高效运行。

在接下来的学习中，无论您是新手还是老手，我都希望这次学习能为您带来新的灵感和知识。请您准备好您的工具包，我们即将启程，深入探索LLC谐振变换器的奇妙世界。期待在旅途中与您共同探讨、学习，并享受这一探索之旅。  

本系列学习路线：LLC的定义及优势 - LLC入门 - LLC工作原理 - LLC特性分析
## 谐振变换器

功率变换器设计目标： 

高功率密度，小设计尺寸，高转换效率

措施： 

提高开关频率，采用高频工作降低无源器件的尺寸，如变压器和滤波电感、 电容等器件。但存在的开关损耗却对高频工作带来不利影响

谐振变换器优点： 

谐振变换器的基本变换单元是谐振电路，由于电路的谐振作用，使得电压或者电流都能周期性地通过零点，这样开关器件就能够在零电压或者零电流的条件下导通或者关断，开关器件实现了软开关，使得开关损耗降低。因此，开关损耗和噪声可大幅度减少。常规谐振器使用串联的电感电容作为谐振网络。负载连接有两种基本结构，串联和并联。

**四种谐振回路：**

![01image.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/01image.png)



## **LLC**

LLC谐振变换器（也称为LLC串并联谐振变换器）通常被选择用于电源设计中，因为它集成了串联谐振变换器（SRC）和并联谐振变换器（PRC）的优点，并且解决了它们的一些局限性。以下是选择LLC谐振变换器的几个关键原因：

1. **零电压开关（ZVS）**：LLC谐振变换器可以在较宽的负载范围内提供ZVS，这减少了开关损耗，从而提高了整体效率。在低负载条件下，SRC可能无法保持ZVS，而LLC可以。

2. **最小化电磁干扰（EMI）**：由于其软开关特性，LLC变换器能够减少EMI，这对于符合严格EMI标准的应用非常重要。

3. **负载调节能力**：LLC变换器能够更好地处理广泛的负载变化，并且仍然保持高效的能量转换，而SRC和PRC在这方面的表现可能不够理想。

4. **变频操作**：LLC变换器允许在不同频率下操作，以优化效率和输出特性，而SRC和PRC可能只在单一频率下最优化。

5. **宽输入电压范围**：LLC谐振变换器可以适应宽广的输入电压变化，这使得它们适用于变动输入电压的应用场景。

6. **效率和功率密度**：LLC谐振变换器因为可以维持高效率，使得它们能够实现更高的功率密度，这对于需要小型化解决方案的应用非常关键。

7. **简化的控制策略**：与SRC或PRC相比，LLC变换器可以使用更简单的控制策略，这降低了设计的复杂性和成本。

8. **热性能**：由于较低的开关损耗，LLC变换器一般有更好的热性能，这减少了冷却需求和相关成本。

9. **更高的可靠性**：减少了开关损耗和运行在较低的应力下，LLC变换器的可靠性更高，这对于要求长寿命应用的场景特别有价值。

由于这些优势，LLC变换器在众多需要高效、高密度和高可靠性电源的应用中成为了首选。然而，它们的设计和控制可能比SRC或PRC更复杂，需要精确的电感和电容匹配以及优化的控制算法来实现最佳性能。因此，LLC变换器的选择应基于应用的具体需求和可接受的设计复杂性。

![01image1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/LLC_Resonant_Converters/01image1.png)



## **参考/感谢：**

[1]Bo Yang. Topology Investigation for Front End DC/DC Power Conversion for Distributcd Power System. 

[2]ST. Simplified Analysis and Design of Series resonant LLC Half-bridge Converters.

[3][https://www.bilibili.com/video/BV1Pr4y1g7ad/?spm_id_from=333.788&vd_source=c57cc7d724946a8cfa6381f148e147d5](https://www.bilibili.com/video/BV1Pr4y1g7ad/?spm_id_from=333.788&vd_source=c57cc7d724946a8cfa6381f148e147d5)

🚉尊敬的旅客朋友们，已到站，本站是：LLC入门，请需要下车的乘客带好随身物品有序下车。

⏩下一站是：LLC工作原理