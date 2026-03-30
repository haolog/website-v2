---
template: guide
title: Cloudflare
description: 将 Nuxt 与 Cloudflare 结合使用时的注意事项
category: deployment
logo:
  light: "/img/companies/square/light/Cloudflare.svg"
  dark: "/img/companies/square/dark/Cloudflare.svg"
---
# 将 Nuxt 部署到 Cloudflare

将 Nuxt 与 Cloudflare 结合使用时的注意事项

---

大多数情况下，Nuxt 可以与非 Nuxt 自身生成或创建的第三方内容配合使用。但有时这类内容可能会引发问题，尤其是 Cloudflare 的"Minification and Security Options"。

因此，您需要确保在 Cloudflare 中取消勾选/禁用以下选项，否则不必要的重新渲染和水合错误可能会影响生产应用程序：

1. Speed > Optimization > Auto Minify：**取消勾选** JavaScript、CSS 和 HTML
2. Speed > Optimization > **禁用** "Rocket Loader™"
3. Speed > Optimization > **禁用** "Mirage"
4. Scrape Shield > **禁用** "Email Address Obfuscation"
5. Scrape Shield > **禁用** "Server-side Excludes"

通过这些设置，可以确保 Cloudflare 不会向 Nuxt 应用程序注入可能导致意外副作用的脚本。
