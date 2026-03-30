---
template: guide
title: Dokku
description: 如何使用 Dokku 部署 Nuxt 应用？
target: Server
category: deployment
logo:
  light: "/img/companies/square/light/dokku.png"
  dark: "/img/companies/square/dark/dokku.png"
---

# 使用 Dokku 部署 Nuxt

如何使用 Dokku 部署 Nuxt 应用？

---

建议阅读 [Dokku 安装文档](http://dokku.viewdocs.io/dokku/getting-started/installation/) 以及 [使用 Dokku 在 Digital Ocean 上部署 Node.js 应用](http://jakeklassen.com/post/deploying-a-node-app-on-digital-ocean-using-dokku/)。

在本例中，我们将 Nuxt 应用称为 `my-nuxt-app`。

需要告知 Dokku 安装项目的 `devDependencies`（以便能够运行 `npm run build`）：

```bash
// 在 Dokku 服务器上
dokku config:set my-nuxt-app NPM_CONFIG_PRODUCTION=false YARN_PRODUCTION=false
```

同时，让应用监听主机 `0.0.0.0` 并以生产模式运行：

```bash
// 在 Dokku 服务器上
dokku config:set my-nuxt-app HOST=0.0.0.0 NODE_ENV=production
```

手动输入 `dokku config my-nuxt-app` 时，应该能看到以下 3 行：

![nuxt config vars Dokku](https://i.imgur.com/9FNsaoQ.png)

然后，通过项目 `app.json` 中的 `scripts.dokku.predeploy` 脚本告知 Dokku 运行 `npm run build`：

`在项目根目录创建名为 app.json 的文件`

```js
{
  "scripts": {
    "dokku": {
      "predeploy": "npm run build"
    }
  }
}
```

使用 [Procfile](http://dokku.viewdocs.io/dokku/deployment/methods/dockerfiles/#procfiles-and-multiple-processes) 运行 `npm run start` 来启动应用：

```
web: npm run start
```

最后，将应用推送到 Dokku：

```bash
// 推送前提交更改
git remote add dokku dokku@yourServer:my-nuxt-app
git push dokku master
```

Nuxt 应用现已托管在 Dokku 上！
