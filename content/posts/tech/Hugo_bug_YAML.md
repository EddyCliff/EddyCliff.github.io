---
title: "【Hugo网站搭建】bug日志 - 解决 YAML 解析错误"
date: 2023-08-30T00:17:58+08:00
lastmod: 2023-08-30T00:17:58+08:00
author: ["Eddy"]
keywords: 
- PicGo
- error
- bug
- image host
categories: 
- 
tags: 
- PicGo
- error
- bug
- image host
description: "解决 Hugo 构建过程中出现的 YAML 解析错误。了解错误背后的原因，探讨正确的 YAML 头部格式，并提供修复方案，确保 Hugo 网站构建顺利进行。"
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/office.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 项目背景

项目背景：在使用 `Hugo` 构建网站的过程中，可能会遇到各种各样的问题。本篇博客记录了一次在重建 `Hugo` 网站时遇到的 `YAML` 解析错误问题，以及如何解决这个问题的过程。

---

## 问题描述

在使用 `hugo server` 命令重新构建网站时，出现了以下错误信息：

### ERROR 1

```JSON
error building site: assemble: "D:\myblog\Eddy-hugo-papermod\content\posts\tech\PicGo.md:1:1": failed to unmarshal YAML: yaml: did not find expected key
```

这个错误提示指向了文件 `PicGo.md` 的第 1 行，表明在解析该文件的 `YAML` 头部时出现了问题。

```YAML
---
title: "【图床工具】PicGo-bug日记"unable to verify the first certificate""
---
```

### ERROR 2

```JSON
ERROR Rebuild failed: assemble: "D:\myblog\Eddy-hugo-papermod\content\posts\tech\PicGo_bug.md:13:1": failed to unmarshal YAML: yaml: line 13: did not find expected key
```

这个错误提示指向了文件 `PicGo.md` 的第 13行，表明在解析该文件的 `YAML` 头部时出现了问题。

```YAML
---
description: "解决 PicGo 图床工具中的 "unable to verify the first certificate" 错误"
---
```

---

## 原因分析

根据错误信息，这个问题是因为在 `YAML`头部中未找到预期的键（key）导致的。在 YAML 格式中，每个键都应该有对应的值，但在该文件的第 1 行 `YAML` 头部中，缺少了一个预期的键。

`Hugo` 使用 `YAML` 头部来存储文章的元信息，这些元信息用于配置文章的标题、日期、作者、标签等信息。如果 `YAML` 头部的格式不正确，`Hugo` 解析过程中就会出现问题，导致构建失败。

---

## 解决方案

为了解决这个问题，需要检查并修复 `PicGo.md` 文件的 `YAML` 头部。正确的`YAML`头部应该以三个连续的短横线 `---` 开始和结束，中间包含各个键值对。以下是一个示例的 `YAML` 头部：

```YAML
---
title: "文章标题"
date: 2023-08-25T00:00:00+08:00
author: "作者"
categories:
- 技术
tags:
- Hugo
- Markdown
---
```

请确保 `YAML` 头部的格式正确，每个键都有对应的值。如果问题仍然存在，请提供 `PicGo.md` 文件的 `YAML` 头部内容，以便更好地帮助诊断和解决问题。

### ERROR1

在标题中似乎包含了一个额外的引号。在标题 "【图床工具】PicGo-bug日记"unable to verify the first certificate"" 中，最后一个引号之后的内容似乎被认为是 `YAML` 内容的一部分，导致解析错误。

```YAML
---
title: "【图床工具】PicGo-bug日记\"unable to verify the first certificate\""
---
```

请注意，我在标题中的引号前添加了反斜杠（`\`），以便正确识别引号并避免引起 `YAML` 解析问题。同时，请确保标题格式正确，并且您的 `Hugo` 主题能够正确显示这样的标题。

### ERROR2

根据您提供的信息，问题出现在 `description` 字段的内容中，这里使用了双引号，导致 YAML 解析失败。

为了解决这个问题，您可以考虑以下两种方法：

**1.使用单引号：** 将 `description` 字段的内容使用单引号括起来，这样就不会与双引号冲突。

```YAML
description: '解决 PicGo 图床工具中的 "unable to verify the first certificate" 错误。了解错误原因、网络加速工具可能引发的问题，并探讨关闭 fastgithub 解决方案。同时介绍 PicGo 官方文档中的常见问题和解决方法，确保图床上传稳定可靠。'
```

**2.转义双引号：** 在 `description` 字段的内容中，使用反斜杠（\）来转义双引号，告诉解析器这些双引号不是表示字符串的边界。

```YAML
description: "解决 PicGo 图床工具中的 \"unable to verify the first certificate\" 错误。了解错误原因、网络加速工具可能引发的问题，并探讨关闭 fastgithub 解决方案。同时介绍 PicGo 官方文档中的常见问题和解决方法，确保图床上传稳定可靠。"
```

请根据您的偏好选择其中一种方法，并进行尝试。这应该可以解决 YAML 解析失败的问题。

---

## 总结

在 `Hugo` 网站构建的过程中，准确的 `YAML` 头部格式至关重要。通过本文，您了解了在构建过程中可能遇到的 `YAML` 解析错误，以及如何通过检查和修复 `YAML` 头部来解决问题。确保您的 `Hugo` 网站的元信息格式正确，将有助于避免类似的问题，使构建过程更加顺利。

