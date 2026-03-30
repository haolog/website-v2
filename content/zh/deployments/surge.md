---
template: guide
title: Surge
description: 如何将 Nuxt 应用部署到 Surge？
target: Static
category: deployment
logo:
  light: "/img/companies/square/light/Surge.svg"
  dark: "/img/companies/square/dark/Surge.svg"
---
# 将 Nuxt 部署到 Surge

如何将 Nuxt 应用部署到 Surge？

---

Nuxt 可以在任何静态托管上托管 Web 应用，例如 [Surge](https://surge.sh/)。

要使用 Surge 部署，首先在计算机上安装 Surge：

```bash
npm install -g surge
```

然后使用 Nuxt 生成 Web 应用：

```bash
npm run generate
```

这将创建 `dist` 文件夹，其中包含所有内容，可以部署到静态托管。

然后将其部署到 Surge：

```bash
surge dist/
```

就这样 :)

如果您的项目有[动态路由](/docs/directory-structure/pages#dynamic-pages)且使用 Nuxt <= v2.12，请查看 [`generate` 配置](/docs/configuration-glossary/configuration-generate)，告知 Nuxt 如何生成动态路由。

::alert{type="warning"}
使用 `nuxt generate` 生成 Web 应用时，传递给 [asyncData](/docs/features/data-fetching) 的[上下文](/docs/internals-glossary/context)不包含 `req` 和 `res`。
::
