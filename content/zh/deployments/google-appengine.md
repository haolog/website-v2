---
template: guide
title: Google App Engine
description: 如何将 Nuxt 部署到 Google App Engine？
target: Server
category: deployment
logo:
  light: "/img/companies/square/light/Google_engine_app.svg"
  dark: "/img/companies/square/dark/Google_engine_app.svg"
---
# 将 Nuxt 部署到 Google App Engine

如何将 Nuxt 部署到 Google App Engine？

---

部署到 [Google App Engine](https://cloud.google.com/appengine/) 是在 Google 云服务上托管通用 Nuxt 应用的快速简便解决方案。

本指南将介绍如何在本地构建应用，然后将整个项目文件夹上传到 Google App Engine。上传后，Google App Engine 将自动启动 package.json 中的 `start` 脚本，应用即可立即使用。

## 开始

确保您已有 Google Cloud 账户，并在 [Google App Engine](https://cloud.google.com/appengine/) 上设置了项目和空的 Google App Engine。此外，请按照[此处](https://cloud.google.com/sdk/)的说明从 Google 下载并安装 Cloud SDK（CLI），并使用您的 Google Cloud 账户登录。

## 配置应用程序

要将通用 Nuxt 应用部署到 App Engine，只需添加一个名为 `app.yaml` 的文件。在项目根目录创建该文件并添加以下内容：

```yaml
runtime: nodejs10

instance_class: F2

handlers:
  - url: /_nuxt
    static_dir: .nuxt/dist/client
    secure: always

  - url: /(.*\.(gif|png|jpg|ico|txt))$
    static_files: static/\1
    upload: static/.*\.(gif|png|jpg|ico|txt)$
    secure: always

  - url: /.*
    script: auto
    secure: always

env_variables:
  HOST: '0.0.0.0'
```

对于灵活环境，最小配置如下：

```yaml
runtime: nodejs
env: flex
```

## 构建和部署应用程序

使用 `npm run build` 或 `yarn build` 构建应用程序。

现在应用程序已准备好上传到 Google App Engine，运行以下命令：

```
gcloud app deploy app.yaml --project [project-id]
```

Nuxt 应用程序现已托管在 Google App Engine 上！

## 更多信息

- app.yaml 文件中的 `instance_class` 属性设置应用实例的类别。F2 实例并非完全免费，但具备运行 Nuxt 应用所需的最小内存。
- 确保 package.json 中的 `start` 是部署后要运行的命令。如果通常使用 `start:prod` 或其他命令运行，应用程序将无法按预期工作。

请确保在 deploy 命令中指定的是 `project-id` 而非 `project-name`，两者不同但容易混淆。
