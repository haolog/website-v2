---
template: guide
title: Platform.sh
description: 如何将 Nuxt 部署到 Platform.sh？
target: Static & Server
category: deployment
logo:
  light: "/img/companies/square/light/Platformsh.svg"
  dark: "/img/companies/square/dark/Platformsh.svg"
---
# 将 Nuxt 部署到 Platform.sh

如何将 Nuxt 部署到 Platform.sh？

---

[Platform.sh](https://platform.sh/) 是一个功能完整的端到端持续部署云托管系统，支持暂存环境和生产环境。它可以托管用各种语言编写的静态和动态应用程序。

Platform.sh 具备以下功能：

- 构建、部署、管理和扩展应用程序。
- 任何分支都可以作为暂存环境，轻松创建和删除环境。
- 支持 Node.js、PHP、Python、Ruby、Go 或 Java 应用等大多数语言，可选择任意版本。
- 自动 TLS 证书
- 集成构建管道，可根据需要自定义应用程序的构建过程。
- 基础设施即代码：只需定义 3 个 YAML 文件，即可按需创建整个集群，轻松添加和删除服务。
- 可直接从 GitHub 和 GitLab 仓库部署代码。

## 设置

Platform.sh 为 Nuxt 预置了模板。点击以下链接将创建一个新的 Platform.sh 项目，并预置 Nuxt 示例应用程序，您可以对其进行自定义。

<p align="center">
<a href="https://console.platform.sh/projects/create-project?template=https://raw.githubusercontent.com/platformsh/template-builder/master/templates/nuxtjs/.platform.template.yaml&utm_content=nuxtjs&utm_source=nuxtjs_orgb&utm_medium=button&utm_campaign=deploy_on_platform" target="_blank">
    <img src="https://platform.sh/images/deploy/lg-blue.svg" alt="Deploy on Platform.sh" height="40px" width="180px" />
</a>
</p>

`README.md` 文件包含所提供默认配置的详细信息。Platform.sh 新账户前 30 天免费。
