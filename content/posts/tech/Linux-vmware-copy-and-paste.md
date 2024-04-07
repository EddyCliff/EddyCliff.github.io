---
title: "【Linux应用开发指南】vmware实现复制粘贴功能"
date: 2023-08-19T00:17:58+08:00
lastmod: 2023-08-19T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Linux
- vmware
- vmware tools
- open vm tools
categories: 
- 
tags: 
- Linux
- vmware
description: "vmware实现复制粘贴功能"
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/linux.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---
## 前言

`VMware`自带的`VMware Tools`复制粘贴功能失败，所以安装`open-vm-tools`以使用复制粘贴功能。

---

## 一、`open-vm-tools`是什么？

`open-vm-tools` 是一组用于 VMware 虚拟机中的开源工具，它们提供了一些与虚拟机操作和管理相关的功能。这些工具与 VMware 虚拟化平台集成，可以在虚拟机中提供更好的性能和功能。以下是一些 `open-vm-tools` 提供的功能：

1. **虚拟机增强功能：** `open-vm-tools` 提供了与 VMware 虚拟化平台集成的增强功能，例如拖放文件、共享剪贴板、虚拟机自适应大小等。

2. **虚拟机信息获取：** 这些工具可以提供有关虚拟机配置、硬件和操作系统的信息，使你可以更好地监控和管理虚拟机。

3. **时钟同步：** `open-vm-tools` 可以帮助虚拟机与虚拟化主机进行时钟同步，确保虚拟机时间与主机时间保持一致。

4. **电源管理：** 这些工具允许你在虚拟机中进行电源管理操作，如重启、关机等。

5. **性能优化：** `open-vm-tools` 可以与虚拟化平台一起工作，优化虚拟机的性能，提供更好的资源管理和协作。

注意，`open-vm-tools` 适用于许多 Linux 发行版，并提供了虚拟机操作和管理方面的许多便利功能。如果你在 `VMware` 虚拟机中运行 `Linux` 操作系统，可以考虑安装和使用这些工具来提升虚拟机的性能和功能。安装 `open-vm-tools` 的方法可能会因你使用的 Linux 发行版而有所不同，通常你可以在操作系统的软件仓库中找到它。

## 二、操作步骤

1. 删除`VMware-tools`

> 判断自己的VMware是否安装了VMware tools

![1.png](https://cdn.jsdelivr.net/gh/EddyCliff/ChartBed/Linux_vmware_copy_and_paste/1.png)

如图，显示“重新安装`VMware tools`”则说明已经安装了VMware tools，然后执行以下命令删除`VMware tools`

```Shell
sudo vmware-uninstall-tools.pl
sudo rm -rf /usr/lib/vmware-tools
sudo apt-get autoremove open-vm-tools --purge
```

如果没安装过`VMware tools`则忽略第一步，直接进入第二步。



1. 安装 `open-vm-tools`

```Shell
sudo apt-get install open-vm-tools
sudo apt-get install open-vm-tools-desktop
```

安装完成后，重启虚拟机即可使用复制粘贴功能。



> Windows系统中复制粘贴快捷键是 `ctrl+c` `ctrl+v`

> Linux系统中复制粘贴快捷键是 `shift+ctrl+c` `shift+ctrl+v`

> 文件复制粘贴功能需使用鼠标右键，而不能直接使用快捷键。



