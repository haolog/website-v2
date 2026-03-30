---
template: guide
title: Koyeb
description: 使用 Docker 将 Nuxt 部署到 Koyeb Serverless Platform
target: Server
category: deployment
logo:
  light: "/img/companies/square/light/Koyeb.svg"
  dark: "/img/companies/square/dark/Koyeb.svg"
---
# 将 Nuxt 部署到 Koyeb Serverless Platform

使用 Docker 将 Nuxt 部署到 Koyeb Serverless Platform

---

[Koyeb](https://www.koyeb.com) 是面向开发者的无服务器平台，用于在全球范围内部署应用程序。该平台支持无缝运行 Docker 容器、Web 应用和 API，具备基于 git 的部署、原生自动扩展、全球边缘网络以及内置服务网格和服务发现功能。

本指南介绍如何在 Koyeb 平台上对 Nuxt 应用进行 Docker 化并部署。

> Koyeb 支持从您喜欢的注册表部署 Docker 容器。本指南使用 Docker Hub 存储镜像，但您也可以自由使用其他容器注册表提供商，如 [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry) 或 [GitLab Container Registry](https://docs.gitlab.com/ee/user/packages/container_registry/)。

## 前提条件

成功完成本指南需要：

1. 要部署的 Nuxt 项目。可以使用 [create-nuxt-app](https://github.com/nuxt/create-nuxt-app) 创建 Nuxt 项目并开始。
2. 用于部署和运行 Docker 化 Nuxt 应用的 [Koyeb 账户](https://app.koyeb.com)
3. 用于推送 Docker 镜像并部署到 Koyeb 的 [Docker Hub](https://hub.docker.com/) 账户

## 开始

在 Nuxt 应用程序目录中运行以下命令安装依赖项：

```bash
yarn
```

安装完成后，启动应用程序确认一切正常：

```bash
yarn dev
```

## Docker 化应用程序

要对 Nuxt 应用进行 Docker 化，需要在项目目录中创建包含以下内容的 `Dockerfile`：

```dockerfile
FROM node:lts as builder

WORKDIR /app

COPY . .

RUN yarn install \
  --prefer-offline \
  --frozen-lockfile \
  --non-interactive \
  --production=false

RUN yarn build

RUN rm -rf node_modules && \
  NODE_ENV=production yarn install \
  --prefer-offline \
  --pure-lockfile \
  --non-interactive \
  --production=true

FROM node:lts

WORKDIR /app

COPY --from=builder /app  .

ENV HOST 0.0.0.0
EXPOSE 80

CMD [ "yarn", "start" ]
```

运行以下命令构建 Docker 镜像：

```bash
docker build . -t <YOUR_DOCKER_HUB_USERNAME>/my-nuxt-project
```

此命令将构建名为 `<YOUR_DOCKER_HUB>/my-nuxt-project` 的 Docker 镜像。构建完成后，可以在本地运行使用该镜像的容器，确认一切按预期工作：

```bash
docker run -p 3000:3000 <YOUR_DOCKER_HUB_USERNAME>/my-nuxt-project
```

打开浏览器访问 http://localhost:3000 查看项目的登录页面。

## 将 Docker 镜像推送到容器注册表

测试确认 Docker 镜像已构建并正常工作后，将其上传到容器注册表。本文档使用 Docker Hub 存储镜像。在终端运行以下命令推送镜像：

```bash
docker push <YOUR_DOCKER_HUB_USERNAME>/my-nuxt-project
```

## 在 Koyeb 上将 Nuxt 应用部署到生产环境

在 Koyeb 控制面板中，点击 **Create App** 按钮。

在表单的 `Docker image` 字段中输入之前创建的镜像名称，格式为 `<YOUR_DOCKER_HUB_USERNAME>/my-nuxt-project`。

勾选 `Use a private registry`，在选择框中点击 **Create Registry Secret**。

将打开一个模态框，要求填写：

- 创建的密钥名称，例如 `docker-hub-secret`
- 注册表提供商，用于生成包含私有注册表凭据的密钥，这里使用 Docker Hub
- Docker Hub 的用户名和密码。建议使用 Docker Hub 的[访问令牌](https://hub.docker.com/settings/security)代替密码。

填写所有字段后，点击 **Create** 按钮。

无需更改 _Path_，应用程序可以使用域名根目录 `/`。

将应用命名为 `nuxt-app`，点击 **Create App**。

_还可以增加部署区域、设置环境变量，以及根据需要定义水平扩展。_

将自动跳转到 Koyeb App 页面，可以查看 Nuxt 应用程序的部署进度。几秒钟后应用部署完成，点击以 `koyeb.app` 结尾的 _Public URL_。

您的 Nuxt 应用程序现在在 Koyeb 上运行，享有原生自动扩展、自动 HTTPS（SSL）、自动修复以及通过边缘网络的全球负载均衡。
