---
template: guide
title: Bip
description: 如何使用 Bip 部署 Nuxt 应用？
target: Static
category: deployment
logo:
  light: "/img/companies/square/light/bip.png"
  dark: "/img/companies/square/dark/bip.png"
---
# 使用 Bip 部署 Nuxt

如何使用 Bip 部署 Nuxt 应用？

---

[Bip](https://bip.sh) 是一个商业托管服务，为 Nuxt 静态网站提供零停机部署、全球 CDN、SSL、无限带宽等功能。按域名按量付费。

以下指南将介绍如何通过几个简单步骤将 Nuxt 静态网站部署到 Bip。

## 前提条件

- 已安装 [Yarn](https://yarnpkg.com/getting-started/install)。
- 已安装 Bip CLI，并拥有 Bip 账户和可用域名。详情请参阅 [Bip 入门指南](https://bip.sh/getstarted)。

## 步骤 1：初始设置

首先需要准备好 Nuxt 项目以便部署和分享。如果需要项目，请使用 [create-nuxt-app](https://github.com/nuxt/create-nuxt-app)：

使用 Yarn 创建新项目：

```bash
yarn create nuxt-app <project-name>
```

按照提示配置 Nuxt 项目。在"Deployment target"中，确保选择"Static (Static/JAMstack hosting)"。

完成后，进入新目录：

```bash
cd <project-name>
```

然后需要使用 Bip 初始化项目，此操作只需执行一次：

```bash
bip init
```

按照提示操作，系统会询问您要部署到哪个域名。Bip 会检测到您正在使用 Nuxt，并自动设置项目配置（如源文件目录等）。

## 步骤 2：部署

现在可以部署网站了，运行：

```bash
yarn generate && bip deploy
```

就这样！片刻后网站即可部署完成。
