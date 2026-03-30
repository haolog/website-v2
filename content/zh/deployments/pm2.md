---
template: guide
title: PM2
description: 如何使用启用 PM2 集群模式部署 Nuxt？
target: Server
category: deployment
logo:
  light: "/img/companies/square/light/pm2.png"
  dark: "/img/companies/square/dark/pm2.png"
---
# 使用 PM2 部署 Nuxt

如何使用启用 PM2 集群模式部署 Nuxt？

---

使用 [PM2](https://pm2.keymetrics.io/)（Process Manager 2）部署是在服务器或 VM 上托管通用 Nuxt 应用程序的快速简便解决方案。

本指南将介绍如何在本地构建应用程序，然后使用启用集群模式的 PM2 配置文件提供服务。集群模式允许应用程序跨多个 CPU 扩展，从而防止停机。

## 开始

确保服务器上已安装 pm2。如果未安装，请通过 yarn 或 npm 全局安装：

```bash
# 使用 yarn 安装 pm2
$ sudo yarn global add pm2 --prefix /usr/local

# 使用 npm 安装 pm2
$ npm install pm2 -g
```

## 配置应用程序

使用 PM2 提供通用 Nuxt 应用程序只需添加一个名为 `ecosystem.config.js` 的文件。在项目根目录创建该文件并添加以下内容：

```javascript
module.exports = {
  apps: [
    {
      name: 'NuxtAppName',
      exec_mode: 'cluster',
      instances: 'max', // 或实例数量
      script: './node_modules/nuxt/bin/nuxt.js',
      args: 'start'
    }
  ]
}
```

## 构建和提供应用程序

使用 `npm run build` 构建应用程序。

然后使用 `pm2 start` 提供应用程序。

使用 `pm2 ls` 查看状态。

Nuxt 应用程序现已提供服务！

## 更多信息

此解决方案保证此服务器上的应用程序零停机（还需要通过冗余或高可用云解决方案防止服务器故障）。

PM2 的其他配置请参阅[此处](https://pm2.keymetrics.io/docs/usage/application-declaration/#general)。
