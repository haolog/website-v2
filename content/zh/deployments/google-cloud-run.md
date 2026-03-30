---
template: guide
title: Google Cloud Run
description: 如何将 Nuxt 部署到 Google Cloud Run？
target: Server
category: deployment
logo:
  light: "/img/companies/square/light/Google_Cloud_run.svg"
  dark: "/img/companies/square/dark/Google_Cloud_run.svg"
---
# 将 Nuxt 部署到 Google Cloud Run

如何将 Nuxt 部署到 Google Cloud Run？

---

[Google Cloud Run](https://cloud.google.com/run) 是一个全托管计算平台，用于快速、安全地部署和扩展容器化应用程序。

本指南将介绍如何使用 Dockerfile 将整个项目文件夹上传到 Google Cloud Build。上传后，Cloud Build 将自动生成容器，然后部署到 Google Cloud Run，并使用 package.json 中的 `start` 脚本启动容器。

## 开始

确保您拥有 Google Cloud 账户和项目，并以编辑者身份访问 Cloud Build 和 Cloud Run。此外，请按照 Google [此处](https://cloud.google.com/sdk/)的说明下载并安装 Cloud SDK（CLI），并使用您的 Google Cloud 账户登录。如果不想下载 Cloud SDK，可以从 Google Cloud Console 使用 gcloud CLI。

让我们做一些检查！

如果 Cloud Build API 和 Cloud Run API 未启用，请启用它们：

```bash
# 启用 Cloud Build
$ gcloud services enable cloudbuild.googleapis.com

# 启用 Cloud Run
$ gcloud services enable run.googleapis.com
```

进入应用程序目录并安装依赖项：

```bash
# yarn 用户
$ yarn

# npm 用户
$ npm install
```

在本地启动应用程序：

```bash
# yarn 用户
$ yarn dev

# npm 用户
$ npm run dev
```

确认一切正常运行。

## 容器化应用程序

现在，让我们使用 Cloud Build 创建容器。

需要在 Nuxt 应用程序中添加 `Dockerfile`。在项目根目录创建名为 `Dockerfile` 的新文件并添加以下内容：

yarn 用户：

```Dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY . ./
RUN yarn

EXPOSE 8080

ENV HOST=0.0.0.0
ENV PORT=8080

RUN yarn build

CMD [ "yarn", "start" ]
```

npm 用户：

```Dockerfile
FROM node:14

WORKDIR /usr/src/app

COPY . ./
RUN npm install

EXPOSE 8080

ENV HOST=0.0.0.0
ENV PORT=8080

RUN npm run build

CMD [ "npm", "run", "start" ]
```

运行以下命令启动构建过程：

`gcloud builds submit --tag gcr.io/<YOUR_GOOGLE_CLOUD_PROJECT_ID>/my-nuxt-app-name:1.0.0 .`

！注意：如果要实现持续交付或通过 .env 文件进行配置，需要使用 [Cloud Build 配置文件](https://cloud.google.com/cloud-build/docs/build-config)。

## 将应用程序部署到 Cloud Run

运行以下命令部署应用程序：

`gcloud run deploy --image=gcr.io/<YOUR_GOOGLE_CLOUD_PROJECT_ID>/my-nuxt-app-name:1.0.0 --platform managed --port 3000`

如果要设置公共访问，请允许未经身份验证的调用。

请注意，Cloud Run 应用程序的默认并发值为 80（每个容器实例最多同时处理 80 个请求）。可以这样指定并发值：

`gcloud run deploy --image=gcr.io/<YOUR_GOOGLE_CLOUD_PROJECT_ID>/my-nuxt-app-name:1.0.0 --platform managed --port 3000 --concurrency <YOUR_CONCURRENCY_VALUE>`

运行以下命令确认部署是否成功：

`gcloud run services list --platform managed`

将显示 Cloud Run 服务列表，点击部署的 URL 查看结果！
