---
template: guide
title: Cleavr
description: 如何使用 Cleavr 部署 Nuxt 应用？
target: Static & Server
category: deployment
logo:
  light: "/img/companies/square/light/cleavr.svg"
  dark: "/img/companies/square/dark/cleavr.svg"
---

# 使用 Cleavr 部署 Nuxt

如何使用 Cleavr 部署 Nuxt 应用？

---

[Cleavr](https://cleavr.io) 是一个集成了多种 VPS（云托管）提供商的服务器管理控制台，只需几次点击即可配置托管 Nuxt 应用的服务器并部署 Nuxt 应用。

Cleavr 具备以下功能：

- 为运行 Nuxt SSR 和静态应用配置和设置服务器
- 提供安全服务器和免费 SSL 证书
- 从 GitHub、GitLab 和 Bitbucket 仓库零停机部署代码
- 自动安装和配置用于 Nuxt SSR 应用的 PM2（启用集群模式时）
- 通过 GitHub Actions 集成，无需额外配置即可构建应用

## 前提条件

- Cleavr 账户已连接到 VPS 和版本控制（如 GitHub、GitLab、Bitbucket）提供商
- 有可部署的 Nuxt SSR 或静态项目
- 已有预配置的服务器

## 步骤 1：初始设置

可以使用 Flash Deploy 一次性完成新服务器的配置/设置和应用部署，也可以使用传统方式向现有服务器添加新 Nuxt 应用。这里介绍向现有服务器添加新应用的方法。

在 Cleavr 中，导航到要添加新应用的服务器，选择 **Add Site**。

根据部署目标选择 Nuxt SSR 或 Nuxt Static Web 应用类型，填写其余网站信息并点击 **Add**。

这将在服务器上添加站点，并在缺少环境依赖的情况下配置服务器。

站点添加成功后，进入 Web App 部分，点击已添加 Web 应用的 **Complete Setup**。

输入版本控制提供商、仓库和要部署的分支，点击 **Update**。

## 步骤 2：部署

Web 应用现已准备好部署。

在 Web 应用的部署页面，点击 **Deploy**。

部署流程将开始，片刻后完成。
