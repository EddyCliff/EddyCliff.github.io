---
title: "【Hugo网站搭建】实现代码高亮"
date: 2023-06-10T00:17:58+08:00
lastmod: 2023-06-11T00:17:58+08:00
author: ["Eddy"]
keywords: 
- HuGO
- code highlight
categories: 
- 
tags: 
- Hugo
- code highlight
description: "Hugo代码高亮"
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
    image: "https://eddyblog.cn/img/hugo.png" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---
## 前言：

`Hugo`使用`Chroma`插件进行代码高亮。

---

## Chroma:

`Hugo` 在 `v0.65.0` 版本之后使用了 [Chroma](https://github.com/alecthomas/chroma) 代码高亮插件，它是一个 Go 语言实现的非常漂亮并能快速生成的代码高亮工具。

---

## 操作步骤：

默认的代码高亮配置文件如下，你可以复制到你的配置文件`config.yml`内进行修改：

`yaml` 格式的配置文件：

```YAML
markup:
  highlight:
    codeFences: true
    guessSyntax: false
    hl_Lines: ""
    lineNoStart: 1
    lineNos: false
    lineNumbersInTable: true
    noClasses: true
    style: monokai
    tabWidth: 4
```

`toml` 格式的配置文件：

```Toml
[markup]
  [markup.highlight]
    anchorLineNos = false
    codeFences = true
    guessSyntax = true
    hl_Lines = ""
    lineAnchors = ""
    lineNoStart =1
    lineNos = true
    lineNumbersInTable = true
    noClasses = true
    style = "monokai"
    tabWidth = 4
```

`json` 格式的配置文件：

```JSON
{
    "markup":{
        "highlight":{
            "codeFences":true,
            "guessSyntax":false,
            "hl_Lines":"",
            "lineNoStart":1,
            "lineNos":false,
            "lineNumbersInTable":true,
            "noClasses":true,
            "style":"monokai",
            "tabWidth":4
        }
    }
}
```

配置文件中各个设置项的含义如下：

- codeFences：代码围栏功能，这个功能一般都要设为 true 的，不然很难看，就是干巴巴的-代码文字，没有颜色。

- guessSyntax：猜测语法，这个功能建议设置为 true, 如果你没有设置要显示的语言则会自动匹配。

- hl_Lines：高亮的行号，一般这个不设置，因为每个代码块我们可能希望让高亮的地方不一样。

- lineNoStart：行号从编号几开始，一般从 1 开始。

- lineNos：是否显示行号

- lineNumbersInTable：使用表来格式化行号和代码,而不是 标签。这个属性一般设置为 true.

- noClasses：使用 class 标签，而不是内嵌的内联样式

### 关于`"style"`

[https://xyproto.github.io/splash/docs/all.html](https://xyproto.github.io/splash/docs/all.html)

这个网页呈现了所有`markup`的`style`样式，如下图所示

![1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_Code_highlight/1.png)

