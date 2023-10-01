---
title: "【Hugo网站搭建】添加不蒜子Busuanzi页面浏览次数与阅读数据统计"
date: 2023-07-10T00:17:58+08:00
lastmod: 2023-07-10T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Hugo
- Busuanzi
categories: 
- 
tags: 
- Hugo
- Busuanzi
description: "Hugo添加不蒜子Busuanzi页面浏览次数与阅读数据统计"
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
## 前言

`Hugo`添加不蒜子Busuanzi页面浏览次数与阅读数据统计 

---

## 一、`Busuanzi`是什么？

`Busuanzi`是一个用于统计网站访问量的工具。它通常嵌入到网页中，可以追踪页面的浏览次数，方可数量积极其他一些基本的访问数据。这个工具可以帮助网站管理员了解他们的网站受欢迎程度和流量情况。

## 二、使用步骤

> 在`head.html`，`footer.html`，`single.html`，`config.yml`进行修改

### head.html

我的`papermod`的路径为`themes/Hugo-PaperMod/layouts/partials/head.html`

添加以下代码

```HTML
<!-- busuanzi -->
{{- if .Site.Params.busuanzi.enable -}}
  <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
  <meta name="referrer" content="no-referrer-when-downgrade">
{{- end -}}
```

![1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_Busuanzi/1.png)

### footer.html

用于在站点底部显示总访问量与访客数

我的`PaperMod`路径为`themes/Hugo-PaperMod/layouts/partials/footer.html`

添加以下代码，注意添加在`<footer>`代码块里

```HTML
<!-- busuanzi -->
{{ if .Site.Params.busuanzi.enable -}}
<div class="busuanzi-footer">
  <span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
  </span>
  <span id="busuanzi_container_site_uv">
    本站访客数<span id="busuanzi_value_site_uv"></span>人次
  </span>
</div>
{{- end -}}
```

![2.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_Busuanzi/2.png)

### single.html

用于显示每篇文章阅读量

我的`papermod`路径为`themes/Hugo-PaperMod/layouts/_default/single.html`

添加以下代码，加在`<header>`代码块内

```HTML
<!-- busuanzi -->
{{ if .Site.Params.busuanzi.enable -}}
<div  class="meta-item">&nbsp·&nbsp
  <span id="busuanzi_container_page_pv">本文阅读量<span id="busuanzi_value_page_pv"></span>次</span>
</div>
{{- end }}
```

![3.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Hugo_Busuanzi/3.png)

### config.yml

回到根目录的`config.yml`，在`params`里加上`busuanzi:`功能

显示统计即为`true`

```YAML
params:  
    busuanzi:
        enable: true
```

不想显示统计即为`false`

```YAML
params:  
    busuanzi:
        enable: false
```



---

## 总结

本文简单介绍了`Hugo`添加不蒜子Busuanzi站点访问量与阅读量统计的操作步骤和方法。

