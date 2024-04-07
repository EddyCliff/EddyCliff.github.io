---
title: "【GitHub使用指南】解决Git操作错误 - Github Action问题分析"
date: 2023-09-12T00:17:58+08:00
lastmod: 2023-09-12T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- error
- Github action
categories: 
- 
tags: 
- Github
- error
- Github action
description: ""
weight: 3
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/book2.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 问题描述

当使用`GitHub Actions`执行工作流程时，有时会遇到错误消息，其中一个常见的错误是：`Action failed with 'not found deploy key or tokens`。这个错误通常表示`GitHub Actions`在执行过程中找不到有效的部署密钥或访问令牌。这篇博客将帮助你了解如何解决这个问题。

## 错误原因分析

在`GitHub Actions`的工作流程中，有些操作需要使用密钥或令牌来访问GitHub存储库或执行其他敏感操作，例如推送更改到仓库。这些密钥或令牌通常存储在GitHub存储库的`Secrets`中。当出现`not found deploy key or tokens`错误时，这意味着`GitHub Actions`无法找到所需的密钥或令牌。

## 解决步骤

以下是解决`not found deploy key or tokens`的步骤：

### 1. 检查密钥或令牌是否存在

首先，确保你的个人访问令牌（`Personal Access Token`）或部署密钥已正确设置并存储在`GitHub`存储库的`Secrets`中。登录到`GitHub`，导航到你的存储库，然后点击右上角的`Settings`。在左侧导航栏中，选择`Secrets`。确保你需要的密钥或令牌存在于这里。

### 2. 确保Secrets名称正确

在`GitHub Actions`配置文件中，你引用了一个`Secrets`，例如：

```YAML
PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
```

确保名称大小写正确，与存储库中的`Secrets`名称完全匹配。GitHub Actions对大小写敏感。

### 3. 检查Secrets权限

确保你的个人访问令牌或部署密钥具有足够的权限来执行所需的操作。如果你需要推送更改到仓库，确保访问令牌有推送权限。不同操作可能需要不同的权限，因此要确保你的密钥或令牌拥有所需的权限。

### 4. 检查工作流程配置

在GitHub Actions工作流程文件中，比如`main.yml`，你需要确保部分，比如`uses`部分，引用了正确的Secrets。检查配置文件中的这些引用，确保Secrets的名称与你在Secrets中设置的名称匹配。

### 5. 检查工作流程触发条件

确保你的工作流程按照正确的触发条件运行。如果你的工作流程只应在某些条件下运行，请确保这些条件已正确配置。触发条件通常在工作流程文件的顶部定义。

### 6. 监视GitHub Actions日志

在GitHub存储库中，转到"Actions"标签，然后选择你的工作流程。查看详细的日志以获取更多信息，以便确定错误发生的位置。日志通常会提供有关哪一步出现了问题的线索。

GitHub Actions是一个强大的自动化工具，但需要小心处理密钥和令牌，以确保安全性。希望这些步骤能帮助你解决这个问题并顺利执行你的工作流程。

