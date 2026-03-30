---
template: guide
title: 21YunBox
description: 如何将 Nuxt 部署到 21YunBox？
target: Static
category: deployment
logo:
  light: "/img/companies/square/light/Yunbox.svg"
  dark: "/img/companies/square/dark/Yunbox.svg"
---
# 在 21YunBox 上部署 Nuxt

如何将 Nuxt 部署到 21YunBox？

---

[21YunBox](https://www.21yunbox.com) 提供中国高速 CDN、持续部署、一键 HTTPS 以及[托管服务、后端 Web 服务等其他服务](https://www.21yunbox.com/docs/)，为在中国上线 Web 项目提供了完整的解决方案。

21YunBox 具备以下功能：

- 从 GitHub 和 Gitee 持续自动构建和部署
- 通过 [Let's Encrypt](https://letsencrypt.org) 自动颁发 SSL 证书
- 中国高速 CDN，即时缓存失效
- 无限制的[自定义域名](https://www.21yunbox.com/docs/#/custom-domains)
- 自动 [Brotli 压缩](https://en.wikipedia.org/wiki/Brotli)加速站点
- 原生 HTTP/2 支持
- HTTP → HTTPS 自动重定向
- 自定义 URL 重定向和重写

## 前提条件

本指南假设您已有要部署的 Nuxt 项目。如果需要项目，请使用 [create-nuxt-app](https://github.com/nuxt/create-nuxt-app) 开始，或 Fork 21YunBox 的 [Nuxt 示例](https://gitee.com/eryiyunbox-examples/nuxtjs)后继续。

## 设置

只需两个简单步骤即可在 21YunBox 上设置 Nuxt 站点：

1. 在 21YunBox 上创建新的 Web 服务，并授权 21YunBox 访问您的 GitHub 或 Gitee 仓库。
2. 创建时使用以下值：

   |                       |                                                     |
   | --------------------- | --------------------------------------------------- |
   | **环境**               | `Static Site`                                       |
   | **构建命令**           | `yarn && yarn generate`（或您自己的构建命令）         |
   | **发布目录**           | `./dist`（或您自己的输出目录）                        |

就这样！构建完成后，站点将发布到 21YunBox 的 URL（类似 `yoursite.21yunbox.com`）。

## 持续部署

21YunBox 现已连接到您的仓库，每次推送到 GitHub 时都会自动构建和发布站点。

## 21YunBox CDN 与缓存失效

21YunBox 将您的站点托管在中国超高速 CDN 上，确保中国所有用户获得最快的下载速度。

每次部署都会自动即时清除缓存，用户随时都能访问站点的最新内容。

## 自定义域名

使用 21YunBox 的[自定义域名](https://www.21yunbox.com/docs/#/custom-domains)指南，可以轻松为站点添加自定义域名。
