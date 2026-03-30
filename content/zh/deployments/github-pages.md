---
template: guide
title: GitHub Pages
description: 如何使用 GitHub Pages 部署 Nuxt 应用？
target: Static
category: deployment
logo:
  light: "/img/companies/square/light/Github_Pages.svg"
  dark: "/img/companies/square/dark/Github_Pages.svg"
---
# 使用 GitHub Pages 部署 Nuxt

如何使用 GitHub Pages 部署 Nuxt 应用？

---

Nuxt 可以在任何静态托管上托管 Web 应用，例如 [GitHub Pages](https://pages.github.com/)。

要部署到 GitHub Pages，需要生成静态 Web 应用：

```bash
npm run generate
```

这将创建 `dist` 文件夹，其中包含部署到 GitHub Pages 托管所需的所有内容。项目仓库使用 `gh-pages` 分支，用户或组织站点使用 `master` 分支。

::alert{type="info"}
<b>信息：</b>如果您使用 GitHub Pages 的自定义域名并放置了 `CNAME` 文件，建议将 CNAME 文件放在 `static` 目录中。更多详情请参阅[文档](/docs/directory-structure/static)。
::

## 将仓库部署到 GitHub Pages

首先，确保您使用的是 [static target](/docs/features/deployment-targets)，因为我们托管在 GitHub Pages 上：

```js[nuxt.config.js]
export default {
  target: 'static'
}
```

如果您为特定仓库创建 GitHub Pages 且没有自定义域名，页面 URL 格式为：`http://<用户名>.github.io/<仓库名>`。

如果不添加 [router base](/docs/configuration-glossary/configuration-router) 就部署 `dist` 文件夹，访问部署的站点时会发现由于缺少资源导致站点无法正常工作。这是因为网站假设根目录为 `/`，但在此情况下为 `/<仓库名>`。

要解决此问题，需要在 `nuxt.config.js` 中添加 [router base](/docs/configuration-glossary/configuration-router#base) 配置：

```js[nuxt.config.js]
export default {
  target: 'static',
  router: {
    base: '/<repository-name>/'
  }
}
```

这样，所有生成的路径资源前都会加上 `/<仓库名>/`，下次将代码部署到仓库的 GitHub Pages 时，站点将正常工作。

## 通过命令行部署

也可以使用 [push-dir 包](https://github.com/L33T-KR3W/push-dir)：

首先通过 npm 安装：

```bash
npm install push-dir --save-dev
```

在 `package.json` 中添加 `deploy` 命令，项目仓库使用 `gh-pages` 分支，用户或组织站点使用 `master` 分支：

```js
"scripts": {
  "dev": "nuxt",
  "generate": "nuxt generate",
  "start": "nuxt start",
  "deploy": "push-dir --dir=dist --branch=gh-pages --cleanup"
},
```

然后生成并部署静态应用：

```bash
npm run generate
npm run deploy
```

## 构建服务器部署

可以进一步使用构建服务器，监控 GitHub 仓库的新提交，自动检出、编译和部署，而无需手动从本地安装编译和部署文件。

### GitHub Actions

使用 [GitHub Actions](https://github.com/features/actions)（GitHub 官方软件自动化工具）进行部署，如果没有工作流，需要创建新工作流或向现有工作流添加新步骤。

使用 [GitHub Pages Action](https://github.com/marketplace/actions/github-pages-action)，将 `dist` 文件夹中生成的文件推送到默认的 GitHub Pages 分支 `gh-pages`。

在现有工作流中添加以下步骤：

```yaml
- name: Deploy
  uses: peaceiris/actions-gh-pages@v3
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    publish_dir: ./dist
```

对于新工作流，将以下内容粘贴到 `.github/workflows` 目录中名为 `cd.yml` 的新文件中：

```yaml
name: cd

on: [push, pull_request]

jobs:
  cd:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os: [ubuntu-latest]
        node: [14]

    steps:
      - name: Checkout
        uses: actions/checkout@master

      - name: Setup node env
        uses: actions/setup-node@v2.1.2
        with:
          node-version: ${{ matrix.node }}

      - name: Install dependencies
        run: yarn

      - name: Generate
        run: yarn run generate

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

然后将其提交到仓库：

```bash
git add .github/workflows/cd.yml
git commit -m "Adding github pages deploy workflow"
git push origin
```

完成后，`gh-pages` 分支将更新，站点也将随之更新。

### Travis CI

使用 [Travis CI](https://travis-ci.org/)（面向开源项目的免费构建服务器）进行部署，使用 GitHub 账户登录，授予 Travis 查看仓库的权限，并通过切换显示列表中仓库名称旁边的开关来启用仓库的构建服务器。

![Travis Builder Server Enable](/img/docs/github_pages_travis_01.png)

然后点击仓库名称旁边的齿轮图标，配置构建服务器的常规设置，切换开关启用"Build only if .travis.yml is present"功能。

![Travis Builder Server Settings](/img/docs/github_pages_travis_02.png)

在同一页面向下滚动到 Environment Variables 部分，创建名为 `GITHUB_ACCESS_TOKEN` 的新变量，在值字段中粘贴之前创建的 GitHub 个人访问令牌，然后点击"Add"按钮。

![Travis Builder Server Environment Variables](/img/docs/github_pages_travis_03.png)

最后，在仓库根目录创建包含以下内容的 `.travis.yml` 配置文件：

```yaml
language: node_js
node_js:
  - '12'

cache:
  directories:
    - 'node_modules'

branches:
  only:
    - master

install:
  - npm install
  - npm run generate

script:
  - echo "Skipping tests"

deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_ACCESS_TOKEN # 在 travis-ci.org 控制台中设置并标记为安全 https://docs.travis-ci.com/user/deployment/pages/#Setting-the-GitHub-token
  target-branch: gh-pages
  local-dir: dist
  on:
    branch: master
```

然后将其提交到仓库：

```bash
git add .travis.yml
git commit -m "Adding travis deploy configuration"
git push origin
```

现在，每次向仓库提交更改时，Travis 中都会启动新的构建。

![Travis Builder Server Output](/img/docs/github_pages_travis_04.png)

完成后，GitHub Pages 站点将自动更新。

### Appveyor

使用 [Appveyor](https://www.appveyor.com)（面向开源项目的免费构建服务器）进行部署，注册新账户，选择 GitHub 认证选项，使用 GitHub 账户登录。

登录后，点击"New project"链接，点击显示列表中仓库名称旁边的"Add"按钮，启用仓库的构建服务器。

![Appveyor Builder Server Enable](/img/docs/github_pages_appveyor_01.png)

然后在仓库根目录创建包含以下内容的 `appveyor.yml` 配置文件：

```yaml
environment:
  # Nuxt 需要 node v12 或更高版本
  nodejs_version: '12'
  # 加密敏感数据 (https://ci.appveyor.com/tools/encrypt)
  github_access_token:
    secure: ENCRYPTED_GITHUB_ACCESS_TOKEN
  github_email:
    secure: ENCRYPTED_GITHUB_EMAIL

# 仅在 master 分支上运行
branches:
  only:
    - master

# 安装脚本（在克隆仓库后运行）
install:
  # 切换 nodejs 版本
  - ps: Install-Product node $env:nodejs_version
  # 安装模块
  - npm install
  # 生成静态文件
  - npm run generate
  # 配置全局 git 凭据存储 (https://www.appveyor.com/docs/how-to/git-push/)
  - git config --global credential.helper store
  - ps: Add-Content "$env:USERPROFILE\.git-credentials" "https://$($env:github_access_token):x-oauth-basic@github.com`n"
  - git config --global user.email $env:github_email
  # 部署到 GitHub Pages
  - npm run deploy

# 不运行测试
test: off

# 实际上不构建
build: off
```

**_注意_** 此配置假设 `package.json` 文件已按照[命令行部署](#command-line-deployment)说明进行配置。

但在提交此文件之前，需要使用 [Appveyor 加密工具](https://ci.appveyor.com/tools/encrypt)将 `ENCRYPTED_GITHUB_ACCESS_TOKEN` 和 `ENCRYPTED_GITHUB_EMAIL` 变量替换为加密后的 GitHub 个人访问令牌和 GitHub 邮箱地址。

更新后，将文件提交到仓库：

```bash
git add appveyor.yml
git commit -m "Adding appveyor deploy configuration"
git push origin
```

现在，每次向仓库提交更改时，Appveyor 中都会启动新的构建。

![Appveyor Builder Server Output](/img/docs/github_pages_appveyor_02.png)

完成后，GitHub Pages 站点将自动更新。
