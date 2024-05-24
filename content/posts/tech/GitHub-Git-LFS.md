---
title: "【GitHub 使用指南】解决大文件上传问题：Git LFS 最佳实践"
date: 2023-09-12T00:17:58+08:00
lastmod: 2023-09-12T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github
- Git LFS
categories: 
- 
tags: 
- Github使用指南
- Github
- Git LFS
- Git命令
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/pc4.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 前言

在进行版本控制时，管理大文件是一个常见但棘手的问题。GitHub为了维护性能和资源，对大文件的存储有一些限制。本文将介绍如何使用Git LFS（Large File Storage）来高效管理大文件，以及如何应对可能遇到的问题并提供解决方案。

## 问题阐述

你可能会遇到两种常见的问题：

1. 如何将大于GitHub默认50MB文件大小限制的大文件上传到仓库？

2. 如何处理大于Git LFS的100MB限制的大文件？

本文将逐一回答这些问题，并帮助你了解如何优雅地处理大文件。

## Git LFS 简介

Git LFS（Large File Storage）是一种Git扩展，专门用于管理大文件。它的原理是将大文件存储在专门的外部存储服务器上，而不是将它们存储在Git仓库中。这可以显著减小Git仓库的大小，提高性能，并更好地管理大文件的版本控制。

下面是使用Git LFS的最佳实践：

### 1. 安装 Git LFS

首先，确保你已经安装了Git LFS。你可以在官方网站找到安装指南：[Git LFS 官方网站](https://git-lfs.github.com/)

### 2. 初始化 Git LFS

在你的Git仓库中，使用以下命令来初始化Git LFS：

```Shell
git lfs install
```

这将在你的Git仓库中启用Git LFS。

### 3. 跟踪大文件

选择需要跟踪的大文件，然后使用`git lfs track`命令告诉Git LFS要跟踪哪些文件。例如，如果你要跟踪所有`.mp4`和`.zip`文件，可以运行以下命令：

```Shell
git lfs track "*.mp4"
git lfs track "*.zip"
```

这会将这些文件类型的大文件添加到Git LFS的跟踪列表中。

### 4. 提交和推送

现在，当你添加、提交和推送这些大文件时，Git LFS将会自动处理它们。大文件将被上传到Git LFS服务器，而不是存储在Git仓库中。

```Shell
git add file.mp4
git commit -m "Add large video file"
git push origin main
```

### 5. 下载大文件

其他团队成员或克隆你的仓库的人只需运行`git clone`命令即可获得仓库的大文件。Git LFS会自动下载这些大文件。

```Shell
git clone <repository-url>
```

### 6. 管理存储库

你可以使用一些额外的Git LFS命令来管理存储库的大文件。例如，使用`git lfs ls-files`来查看当前跟踪的大文件列表。

```Shell
git lfs ls-files
```

通过这些步骤，你可以更好地管理大文件，减小Git仓库的大小，并确保大文件的版本控制不会影响整个Git仓库的性能。请注意，为充分利用Git LFS的功能，你需要使用支持Git LFS的Git仓库托管服务，例如GitHub、GitLab等。

## 常见问题解答

### Question1: 大文件将被上传到Git LFS服务器，而不是存储在Git仓库中。这是不是意味着在我的仓库中这些大文件不可见？

是的，使用Git LFS来管理大文件后，这些大文件不再以实际内容存储在Git仓库中。相反，Git LFS会将它们替换为指向Git LFS服务器上的指针文件。这些指针文件非常小，不包含大文件的实际数据。

这个方法的好处是你的Git仓库变得更加轻量，克隆和拉取仓库的速度更快，因为不需要下载大文件的实际数据，而是需要时从Git LFS服务器上获取。另外，你可以更好地管理大文件的版本控制，而不会使Git仓库变得庞大和不稳定。

### Question2: 那么在之后git clone该仓库，包括这些大文件也会一键拉取到本地吗？

是的，当你使用Git LFS来管理大文件的仓库时，其他人通过`git clone`命令克隆仓库时，大文件也会一并被拉取到本地。这是Git LFS的一个关键功能，它确保了大文件的可访问性。当你或其他人运行`git clone`命令时，Git LFS会自动检测仓库中跟踪的大文件，并从Git LFS服务器下载它们的实际数据。这意味着克隆的仓库会包括所有的大文件，以便你可以在本地工作和使用它们。

### Question3: 假如我这个文件有300MB，既超过了GitHub默认的50MB文件大小限制，也超过了Git LFS的100MB限制，那么该如何上传？

如果你的文件大小为300MB，超过了GitHub默认的50MB限制和Git LFS的100MB限制，GitHub不会接受这个大文件。对于这种情况，你需要考虑其他方法来存储和共享它。

以下是一些备选方案：

1. **云存储服务**：将大文件上传至云存储服务（如Google Drive、Dropbox、OneDrive等），然后在您的GitHub仓库中添加链接到这些文件的URL。这样，您可以通过链接访问大文件，而不必将它们存储在GitHub仓库中。

2. **分割文件**：如果可能的话，将大文件分割成多个小文件，每个小文件都在GitHub的文件大小限制内。然后将这些小文件上传到GitHub仓库，并在需要时重新组装它们。

3. **Git LFS托管**：如果有访问Git LFS服务器的权限，可以将大文件上传到自己托管的Git LFS服务器上，并将这些大文件的Git LFS指针链接到GitHub仓库。这种情况下，您需要自行管理Git LFS服务器。

4. **使用其他存储服务**：如果您的项目需要共享大文件，可能需要考虑使用专门用于存储大文件的服务，例如Amazon S3、Azure Blob存储等。这些服务通常可以更好地处理大文件。

总之，对于超过GitHub和Git LFS限制的大文件，需要采取额外的措施来管理和共享它们，具体方法取决于您的项目需求和可用资源。

