---
template: guide
title: Hostman
description: 如何将 Nuxt 部署到 Hostman？
target: Static
category: deployment
logo:
  light: "/img/companies/square/light/Hostman.svg"
  dark: "/img/companies/square/dark/Hostman.svg"
---
# 将 Nuxt 部署到 Hostman

如何将 Nuxt 部署到 Hostman？

---

[Hostman](https://hostman.com/) 是面向初创公司和新项目的云托管提供商，可以消除大多数日常 DevOps 操作，为开发者节省时间，为企业节省资金。Hostman 采用服务概念，简化复杂架构的开发，并支持一键扩展。

Hostman 提供以下功能：

- 构建和部署静态网站、Web 应用程序、Docker 容器和数据库。
- 可以看到应用程序运行的实际硬件和实际负载，出现问题时可以评估，一切都非常透明。
- 可以通过 SSH 进入 Docker 容器，提供抽象和透明的完美平衡。
- Hostman 自动为所有域名配置 SSL 证书，并设置 CDN 以尽可能快速地分发内容。
- Hostman 自动化 CI/CD，每次将新提交推送到仓库时立即拉取代码、构建并启动。
- 无供应商锁定。
- Hostman 支持 22 个框架。

## 前提条件

本指南假设您已有要部署的 Nuxt 项目。如果需要项目，请使用 [create-nuxt-app](https://github.com/nuxt/create-nuxt-app)。

## 设置

---

<strong>步骤 1. 创建服务</strong>

要部署 Nuxt 静态网站，请点击[控制台](https://dashboard.hostman.com/)左上角的创建，选择前端应用或静态网站。

![Hostman dashboard](https://i.imgur.com/bEePHDo.png)

<strong>步骤 2. 选择要部署的项目</strong>

如果您使用 GitHub、GitLab 或 Bitbucket 账户登录 Hostman，此时将显示您的仓库（包括私有仓库）。

选择要部署的项目，该项目应包含运行 yarn create-nuxt-app 命令后自动创建的 Nuxt 应用目录。

要访问其他仓库，请点击 <strong>Connect another repository</strong>。

如果未使用 Git 账户凭据登录，现在可以访问所需账户并选择项目。

<strong>步骤 3. 配置构建设置</strong>

接下来将显示 Website customization 页面。

![Build configuration](https://i.imgur.com/gIgl5EH.png)

从框架列表中选择 <strong>Static website</strong> 选项。

<strong>Directory with app</strong> 是指构建后项目文件所在的目录。对于 Nuxt，目录为 dist。

标准的<strong>构建命令</strong>为：

`yarn build`

这将启动框架命令 nuxt generate，创建包含项目文件的 dist 目录。

如果项目的构建过程需要，可以在此修改命令，多个命令可以用 '&&' 分隔输入。

<strong>步骤 4. 部署</strong>

点击 <strong>Deploy</strong> 启动构建过程。

启动后，部署日志将开始填充。如果代码有问题，日志中会显示警告或错误消息，指出问题所在。

通常日志包含所有必要的调试数据，但如果需要帮助解决问题，也欢迎通过聊天联系我们。

部署完成后，您将收到电子邮件通知，日志中也会显示类似条目：

![Log entry](https://i.imgur.com/KwzMxTb.png)

<strong>全部完成！</strong>

您的项目已启动并准备就绪。
