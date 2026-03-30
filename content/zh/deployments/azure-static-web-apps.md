---
template: guide
title: Azure Static Web Apps
description: 如何使用 Azure Static Web Apps 部署 Nuxt 应用？
target: Static
category: deployment
logo:
  light: "/img/companies/square/light/Azure.svg"
  dark: "/img/companies/square/dark/Azure.svg"
---
# 使用 Azure Static Web Apps 部署

如何使用 Azure Static Web Apps 部署 Nuxt 应用？

---

您可以使用 Azure Static Web Apps 将静态站点部署到 Azure。Azure Static Web Apps 利用 GitHub Actions，每次 git push 都可以重新构建静态站点，因此您需要将应用托管在 GitHub 上。

要将应用部署到 Azure Static Web Apps，需要依次进行两项配置。首先，Azure 从 package.json 读取构建命令，因此需要修改构建命令，静态站点需要使用 generate 命令。

`package.json`

```json
build: "nuxt generate"
```

其次，添加 routes.json 文件，这对于获取自定义 404 页面和 SPA 回退页面非常重要。

`static/routes.json`

```jsx
{
 "routes": [],
 "platformErrorOverrides": [
   {
    "errorType": "NotFound",
    "serve": "/200.html",
    "statusCode": 200
   }
 ]
}
```

为了让想要尝试部署到 Azure Static Web Apps 的用户，我们创建了一个包含所有配置的小型演示应用。使用时请克隆该应用并添加到 GitHub 仓库，然后按照"在 Azure Static Web Apps 上部署应用"的步骤操作。

[克隆演示应用](https://github.com/debs-obrien/nuxtjs-azure-static-app)

# 在 Azure Static Web Apps 上部署应用

### 步骤 1：**创建 Azure Static Web Apps**

1. 前往 [Azure Portal](https://portal.azure.com/)。
2. 点击 **Create a Resource**，搜索并选择 **Static App**。
3. 从 *Subscription* 下拉列表中选择订阅，或使用默认值。
4. 点击 *Resource group* 下拉菜单下的 **New** 链接，在 *New resource group name* 中输入 **nuxt** 并点击 **OK**。
5. 在 **Name** 文本框中输入应用程序的全局唯一名称，有效字符为 `a-z`、`A-Z`、`0-9` 和 `-`。建议使用仓库名称命名应用程序。
6. 从 *Region* 下拉菜单中选择最近的区域。

![Azure Portal 资源和应用设置](https://user-images.githubusercontent.com/13063165/82118135-71891b00-9775-11ea-8284-aa94d17a3bc3.png)

### 步骤 2：**添加 GitHub 仓库**

Azure App Service Static App 需要访问存储 Nuxt 应用的仓库，并可以自动部署提交：

1. 点击 **Sign in with GitHub button**
2. 选择为 Nuxt 项目创建的仓库的 **Organization**，也可以使用您的 GitHub 用户名。
3. 找到并选择之前创建的仓库名称。
4. 从 *Branch* 下拉菜单中选择 **master** 作为分支。

![如何添加到 GitHub](https://user-images.githubusercontent.com/13063165/82118359-38ea4100-9777-11ea-9c5e-7ba5c4da708e.png)

### 步骤 3：**配置构建流程**

Azure App Service Static App 可以自动安装 npm 包、运行 `npm run build`，还需要明确指定构建后将静态应用复制到哪个文件夹并从中提供服务。

1. 点击 **Build** 标签配置静态输出文件夹。
2. 在 *App artifact location* 文本框中输入 **dist**。

![Azure Portal 构建设置](https://user-images.githubusercontent.com/13063165/82118277-71d5e600-9776-11ea-88ad-48cf0793905d.png)

### 步骤 4：**审查并创建**

1. 点击 **Review + Create** 按钮确认所有详情正确。
2. 点击 **Create** 开始创建资源，同时为部署配置 GitHub Action。
3. 部署完成后，点击 **Go to resource**。

![Azure Portal 部署完成消息](https://user-images.githubusercontent.com/13063165/82118390-67681c00-9777-11ea-9778-671dc768393e.png)

4. 点击 *URL* 链接打开已部署的应用程序。

![包含已部署应用 URL 的资源页面](https://user-images.githubusercontent.com/13063165/82118042-d001c980-9774-11ea-94f5-57d995aa5391.png)

恭喜，您的静态站点现已托管在 Azure Static Web Apps 上！

## 重新构建静态应用并监控部署

现在只需修改代码并推送更改即可。推送更改将触发 GitHub Action，自动重新构建新站点。点击 GitHub 仓库的 Actions 标签可以监控工作流，选择最近运行的提交可以查看更多详情，确认部署完成或在部署出错时查看日志。

![GitHub Actions 页面](https://user-images.githubusercontent.com/13063165/82118249-34715880-9776-11ea-92e2-dbd21bbf7cb6.png)

## 您知道吗？

### **如何处理动态路由**

如果处理 `_id.vue` 等动态页面，需要将这些路由添加到 nuxt.config.js 的 generate 属性中。

[请查看文档了解如何处理动态路由。](/docs/configuration-glossary/configuration-generate#routes)

<div class="Alert">
如果您使用 Nuxt 2.13 或更高版本，内置爬虫会爬取站点内的链接，无需担心此问题。
</div>

### 如何添加错误页面

要避免显示默认 404 页面，可以在 layouts 文件夹中创建 `error.vue` 文件。

### 如何添加 SPA 回退

如果希望某些页面不生成而作为单页应用运行，可以使用 nuxt.config 文件中的 generate.excludes 属性进行配置。

[请查看 SPA 回退文档](/docs/configuration-glossary/configuration-generate#exclude)
