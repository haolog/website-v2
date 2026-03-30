---
template: guide
title: Digital Ocean
description: 如何在 DigitalOcean App Platform 上部署 Nuxt？
target: Static & Server
category: deployment
logo:
  light: "/img/companies/square/light/Digital_Ocean.svg"
  dark: "/img/companies/square/dark/Digital_Ocean.svg"
---
# 在 DigitalOcean App Platform 上部署 Nuxt

如何在 DigitalOcean App Platform 上部署 Nuxt？

---

[DigitalOcean App Platform](https://www.digitalocean.com/products/app-platform/) 使用简单、完全托管的解决方案，帮助您构建、部署和快速扩展应用程序，并管理基础设施、应用运行时和依赖项，只需几次点击即可将代码推送到生产环境。

App Platform 具备以下功能：

- 构建、部署、管理和扩展应用程序。
- 自动保护应用程序，创建、管理和更新 SSL 证书，并防御 DDoS 攻击。
- 支持 Node.js、静态站点、Python、Django、Go、PHP、Laravel、React、Ruby、Ruby on Rails、Gatsby、Hugo 和容器镜像。
- 直接从 GitHub 和 GitLab 仓库部署代码，推送更新的源代码时自动重新部署应用程序。
- 零基础设施管理，App Platform 使用开放的云原生标准，自动分析代码、创建容器并在 Kubernetes 集群上运行。
- 高可扩展性，支持水平和垂直扩展。

## 前提条件

本指南假设您已有要部署的 Nuxt 项目。如果需要项目，请使用 [create-nuxt-app](https://github.com/nuxt/create-nuxt-app) 开始。

## 设置

1. 关联仓库：在 DigitalOcean 创建新账户并连接 GitHub 或 GitLab 账户，然后选择要部署的仓库。
2. 选择仓库分支和部署站点的区域。
3. 根据您的网站选择合适的服务类型：

   | 类型       | 配置                                                                    |
   | ---------- | ---------------------------------------------------------------------- |
   | **Server** | Web 服务 - 构建命令 `yarn build` & 运行命令 `yarn start --hostname 0.0.0.0` |
   | **Static** | 静态站点 - 构建命令 `yarn generate` & 输出目录 `dist`                    |

   ::alert{type="warning"}
   <b>警告：</b>对于服务器类型，需要在 Web 服务设置中将 **HTTP 端口**从 8080 改为 **3000**。<br />更多详情请参阅[此文章](https://dev.to/tillsanders/deploy-nuxt-js-on-digitalocean-app-platform-in-5-minutes-or-less-2dij)。
   ::

   ![DO App platform Web Service Nuxt configuration](https://i.imgur.com/BhBu49J.png)
4. 如有环境变量，请手动输入键/值对。

流程完成并点击部署按钮后，构建完成的同时，站点将以自动创建的 URL 发布。

## 持续部署（CD）

App Platform 现已连接到您的仓库，每次推送新更改时都会自动构建和发布站点。

## 添加自定义域名

通过 Settings > Domains > Add domain 或参阅指南 [How to Manage Domains in App Platform](https://www.digitalocean.com/docs/app-platform/how-to/manage-domains/) 可以轻松为站点添加自定义域名。

## Deploy to DigitalOcean 按钮

Deploy to DigitalOcean 按钮允许在 App Platform 上启动应用程序。此按钮可嵌入 GitHub 仓库的 README 文件中，让浏览仓库的用户只需一键即可添加 .yaml 文件并部署代码。请查看 [How to Add a "Deploy to DigitalOcean" Button to Your Repository](https://www.digitalocean.com/docs/app-platform/how-to/add-deploy-do-button/)。
