---
template: guide
title: Fume
description: 如何将 Nuxt 部署到 Fume
target: Static & Server
category: deployment
logo:
  light: "/img/companies/square/light/Fume.svg"
  dark: "/img/companies/square/dark/Fume.svg"
---
# 使用 Fume 部署 Nuxt

如何将 Nuxt 部署到 Fume

---

[Fume](https://fume.app/) 是一个基于 AWS 的运营管理平台。

Fume 具备以下功能：

- 使用 Lambda 和 CloudFront 支持 Server 和 Static 的无服务器架构。
- 一键回滚的[自动化](https://github.com/marketplace/actions/fume-deployment)部署
- 按环境的指标和成本预测
- 域名控制 - 导入主机、颁发证书、将记录映射到环境
- 集成 Slack、Discord 和其他协作平台的通知功能

## 设置

按照以下步骤，2 分钟内即可获得生产 URL：

- 前往 [Fume](https://fume.app)，连接并接入您的 AWS 账户
- 创建团队和 Nuxt 项目
- 在项目根目录中运行以下命令

::code-group
```bash [Yarn]
yarn global add fume-cli
fume deploy
```
```bash [NPM]
npm install -g fume-cli
fume deploy
```
::
