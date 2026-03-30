---
template: guide
title: Stormkit.io
description: 如何使用 Stormkit.io 部署 Nuxt？
target: Static & Server
category: deployment
logo:
  light: "/img/companies/square/light/Stormkit.svg"
  dark: "/img/companies/square/dark/Stormkit.svg"
---
# Deploy with Stormkit

How to deploy Nuxt with Stormkit.io?

---

使用 [Stormkit.io](https://www.stormkit.io) 可以轻松构建、部署和扩展 Nuxt 应用程序，支持即时回滚、无服务器端逻辑、代码片段注入、多开发环境等功能。

## 前提条件

本指南假设您已有要部署的 Nuxt 项目。如果需要项目，请使用 [create-nuxt-app](https://github.com/nuxt/create-nuxt-app) 开始，或 Fork Stormkit 的 [Nuxt 示例](https://github.com/stormkit-dev/hackernews-nuxt)后继续。

## 设置

1. 访问 [app.stormkit.io](https://app.stormkit.io)，选择 git 提供商登录。
2. 登录后，Stormkit 会询问您的代码库在哪个提供商。再次点击提供商。
3. 对于 GitHub，点击"Connect more repositories"并授权 Stormkit 访问。
4. 然后选择仓库，这将在 Stormkit 上创建应用程序。
5. 在应用程序页面，找到**生产**环境并点击它。
6. 点击编辑配置应用程序，在此页面指定构建命令和环境变量。

## 静态站点

静态网站无需任何操作，使用 `nuxt generate` 构建的应用程序将立即处理。

## 单页应用程序

对于单页应用程序，只需提供一个将所有请求重定向到 `index.html` 的 `stormkit.config.yml`。在项目顶层创建 `stormkit.config.yml` 文件并指定以下规则：

```
app:
- redirects:
    - from: /*
      to: /index.html
      assets: false
```

## 混合应用程序

对于混合应用程序，需要在构建设置页面开启 `Serverless` 开关。这将使 Stormkit 从 lambda 而非 CDN 处理请求。混合无服务器应用程序的配置详情请参阅[此指南](https://www.stormkit.io/docs/deployments/configuration/nuxt#hybrid)。

## 使用 Stormkit 托管

Stormkit 为每次部署生成唯一 URL，可以使用这些链接预览应用程序。之后连接域名并发布任意部署，用户将看到该版本的应用程序。还可以同时发布多个版本，进行渐进式发布或 A/B 测试。

## 支持

如需更多支持，可以在 [Discord](https://discord.gg/6yQWhyY) 上与 Stormkit 开发者和其他社区成员聊天。
