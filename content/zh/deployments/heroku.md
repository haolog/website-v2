---
template: guide
title: Heroku
description: 如何将 Nuxt 部署到 Heroku？
target: Server
category: deployment
logo:
  light: "/img/companies/square/light/Heroku.svg"
  dark: "/img/companies/square/dark/Heroku.svg"
---
# 将 Nuxt 部署到 Heroku

如何将 Nuxt 部署到 Heroku？

---

建议阅读 [Heroku Node.js 文档](https://devcenter.heroku.com/articles/nodejs-support)。

<div class="Promo__Video">
  <a href="https://vueschool.io/lessons/how-to-deploy-nuxtjs-to-heroku?friend=nuxt" target="_blank">
    <p class="Promo__Video__Icon">
      Watch a free lesson on <strong>How to deploy Nuxt to Heroku</strong> on Vue School
    </p>
  </a>
</div>

可以通过 [Heroku 控制台](https://devcenter.heroku.com/articles/heroku-dashboard)或 [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) 配置应用。

首先，创建应用，添加 Node.js [buildpack](https://devcenter.heroku.com/articles/buildpacks)，并配置应用监听主机 `0.0.0.0`：

```bash
heroku create myapp
heroku buildpacks:set heroku/nodejs
heroku config:set HOST=0.0.0.0
```

Heroku 控制台中应用的 Settings 部分应包含以下内容：

![nuxt config vars Heroku](https://user-images.githubusercontent.com/23453691/116850762-81ea0e00-abf1-11eb-9f70-260721a1d525.png)

最后，将应用推送到 Heroku：

```bash
git push heroku master
```

将非 master 分支部署到 Heroku：

```bash
git push heroku develop:master
```

其中 `develop` 是分支名称。

可选地，可以在 Heroku 控制台应用的 Deploy 部分配置从应用 GitHub 仓库的选定分支自动部署。

Nuxt 应用程序现已托管在 Heroku 上！
