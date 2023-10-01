---
title: "【Hugo网站搭建】GitHub Action自动化部署Hugo博客"
date: 2023-09-06T00:17:58+08:00
lastmod: 2023-09-06T00:17:58+08:00
author: ["Eddy"]
keywords: 
- Github action
- Github
- Hugo
- Website construction
categories: 
- 
tags: 
- Github action
- Github
- Hugo
- Website construction
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
    image: "https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/BlogCover/website1.jpg" #图片路径例如：posts/tech/123/123.png
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

## 前言
在本文中，我们将介绍如何通过GitHub Action自动化部署Hugo博客。如果你是一个博客作者，你可能已经熟悉了使用Hugo构建和管理你的博客。Hugo是一个快速且灵活的静态网站生成器，但是手动部署博客可能会变得繁琐。为了解决这个问题，我们可以利用GitHub Action，这是一个GitHub提供的持续集成和持续交付（CI/CD）工具，来自动构建和部署我们的博客。

## 问题与挑战

在使用Hugo构建博客时，通常需要手动执行一系列命令来生成静态文件，然后将这些文件上传到托管博客的平台（如GitHub Pages）。这个过程可能会变得比较繁琐，特别是当你需要频繁更新博客内容时。

另一个问题是，虽然我们可以管理生成的静态文件，但博客的Markdown源文件通常分散在不同的位置，难以备份和版本管理。

为了解决这些问题，我们希望有一个简单且顺滑的方式来自动化博客的发布流程。

## GitHub Action简介

GitHub Action是GitHub提供的一项强大的自动化工具，它可以用于执行各种自动化任务，包括构建、测试和部署。你可以通过简单的配置文件来定义GitHub Action的工作流程。

在我们的情况下，我们将使用GitHub Action来自动构建并部署Hugo博客。

## 配置GitHub Action

要配置GitHub Action，首先需要在博客源文件的仓库中创建一个`.github/workflows`目录，并在该目录下创建一个`.yml`配置文件。我们将其命名为`deploy.yml`。

以下是一个自动化部署Hugo博客的示例`deploy.yml`配置文件：

```YAML
name: deploy

on:
  push:
    branches:
      - main  # 或者是你的源代码分支

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2
      with:
          submodules: true
          fetch-depth: 0

    - name: Set up Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: "latest"

    - name: Build and Deploy
      run: |
        hugo -F --cleanDestinationDir  # 生成静态文件
        mkdir -p public  # 确保public文件夹存在
        cp -r public/* ./  # 复制生成的静态文件到仓库根目录

    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}  # 你的个人访问令牌
        EXTERNAL_REPOSITORY: username/username.github.io  # 你的GitHub Pages仓库
        PUBLISH_BRANCH: main  # 或者是你的GitHub Pages分支
        PUBLISH_DIR: ./
        commit_message: ${{ github.event.head_commit.message }}
```

让我们来解释一下这个配置文件：

- `on` 表示GitHub Action的触发条件。在这个配置中，我们设置了`push`，并指定了触发的分支为`main`（或者你的源代码分支）。

- `jobs` 表示GitHub Action中的任务。我们定义了一个名为`deploy`的任务，其中`runs-on`指定了GitHub Action的运行环境，我们选择了`ubuntu-latest`。

- `steps` 是任务中的具体步骤，包括：

    - `Checkout repository`：用于检出博客源文件仓库，`submodules`配置为`true`可同步博客源仓库的子模块，即我们的主题模块。

    - `Set up Hugo`：使用Hugo的GitHub Action，设置Hugo的版本。

    - `Build and Deploy`：运行`hugo`命令生成博客的静态文件，并将生成的静态文件复制到仓库根目录。

    - `Deploy to GitHub Pages`：使用GitHub Pages的GitHub Action，将生成的静态文件发布到GitHub Pages仓库。

最后，我们需要在GitHub账户的设置中生成一个Personal Access Token，并将其添加到博客源文件仓库的`Settings -> Secrets -> Actions`中，命名为`PERSONAL_TOKEN`，以便GitHub Action能够获取到Token权限。

## 生成token

当您需要在GitHub Action中执行需要身份验证的操作时，例如推送到另一个仓库或访问私有仓库，您需要提供身份验证凭据。GitHub提供了一种安全的方式来存储这些凭据，即使用Personal Access Token（个人访问令牌）并将其添加到仓库的Secrets中。

以下是如何生成Personal Access Token并将其添加到GitHub仓库的Secrets中的详细步骤：

1. **生成Personal Access Token**：

    - 登录到您的`GitHub`账户。

    - 点击右上角的头像，然后选择`Settings`（设置）。

    - 在左侧导航栏中，点击`Developer settings`（开发者设置）。

    - 在下拉菜单中，选择`Personal access tokens`（个人访问令牌）。

    - 点击右上角的`Generate token`（生成令牌）按钮。

![1.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Github_action_hugo/1.png)

2. **配置Personal Access Token的权限**：

    当您生成`Personal Access Token`时，需要为其分配适当的权限。根据您的需求，可以为其分配不同的权限，例如访问仓库、读取用户资料等。请谨慎选择权限，最小化所需的权限以提高安全性。

![2.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Github_action_hugo/2.png)

3. **生成令牌**：

    在配置了权限后，滚动到页面底部，然后单击`Generate token`（生成令牌）。`GitHub`将生成一个令牌，并将其显示在屏幕上。请务必将此令牌复制到安全的地方，因为在生成后，您将无法再次查看完整的令牌。

4. **将令牌添加到GitHub仓库的Secrets**：

    - 转到包含您的博客源文件的GitHub仓库。

    - 在仓库顶部导航栏中，单击`Settings`（设置）。

    - 在左侧导航栏中，选择`Secrets`（密码）。

    - 单击`New repository secret`（新存储库密码）按钮。

    - 在`Name`字段中，输入`PERSONAL_TOKEN`（或您选择的名称）。

    - 在`Value`字段中，粘贴您在第3步中生成的`Personal Access Token`。

    - 单击`Add secret`（添加密码）按钮。

![3.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Github_action_hugo/3.png)

![4.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Github_action_hugo/4.png)

![5.png](https://testingcf.jsdelivr.net/gh/EddyCliff/ChartBed/Github_action_hugo/5.png)

现在，您的`Personal Access Token`已经以安全的方式存储在`GitHub`仓库的`Secrets`中，并可以在`GitHub Action`的工作流程中使用。

在上文中提到的`PERSONAL_TOKEN`将会被引用并传递给GitHub Action中的相关步骤，以便执行需要身份验证的操作。这个过程确保了敏感凭据的安全性，并且不会将它们直接硬编码到工作流程文件中，从而保护了您的GitHub仓库的安全性。

通过遵循这些步骤，您可以轻松地配置GitHub Action以自动化部署Hugo博客，并确保了令牌和其他敏感信息的安全使用。

## 实现自动发布

完成上述配置后，只需将博客的Markdown源文件编辑并推送到仓库，GitHub Action就会自动触发构建和部署过程。生成的博客页面将自动发布到GitHub Pages仓库。

通过这个自动化流程，我们不仅解决了手动发布的繁琐问题，还能够更好地管理博客的Markdown源文件。现在，每当我们编辑博客内容后，只需推送代码，等待几分钟，就可以通过我们的自定义域名访问更新后的博客。

这个自动化流程大大简化了博客的发布过程，让我们更专注于创作内容而不是繁杂的部署工作。

希望这篇文章对你理解如何利用`GitHub Action`自动化部署`Hugo`博客有所帮助。自动化可以显著提高工作效率，同时也为博客作者提供了更好的创作体验。

