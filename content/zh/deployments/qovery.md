---
template: guide
title: Qovery
description: 如何将 Nuxt 部署到 Qovery？
target: Static & Server
category: deployment
logo:
  light: "/img/companies/square/light/Qovery.svg"
  dark: "/img/companies/square/dark/Qovery.svg"
---
# 将 Nuxt 部署到 Qovery

如何将 Nuxt 部署到 Qovery？

---

[Qovery](https://qovery.com) 是运行在 AWS 和 Scaleway 账户上的全托管云平台，可以在一个地方托管静态站点、后端 API、数据库、cron 任务和所有其他应用程序。

## 前提条件

本指南假设您已有要部署的 Nuxt 项目。如果需要项目，请按照[入门](/docs/get-started/installation)指南操作。

## 设置

按照以下步骤在 Qovery 上设置 Nuxt：

### 1. 创建 Qovery 账户

如果还没有账户，请访问 [Qovery 控制台](https://console.qovery.com)创建账户。

### 2. 创建项目和应用程序

[请按照此教程操作](https://hub.qovery.com/guides/getting-started/deploy-your-first-application/)

## 部署

应用程序应该已经部署。点击部署日志可以实时查看状态。

## 持续部署

Qovery 现已连接到您的仓库，每次推送到 git 时都会**自动构建和发布站点**。

## 自定义域名

使用 Qovery 的[自定义域名](https://docs.qovery.com/guides/getting-started/setting-custom-domain/)指南，轻松为站点添加自定义域名。

## 支持

如需帮助，请在 [Discord](https://discord.qovery.com) 上与 Qovery 开发者聊天。
