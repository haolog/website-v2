---
template: guide
title: Azure Portal
description: 如何将 Nuxt 应用部署到 Azure Portal？
target: Server
category: deployment
logo:
  light: "/img/companies/square/light/Azure.svg"
  dark: "/img/companies/square/dark/Azure.svg"
---
# 将 Nuxt 部署到 Azure Portal

如何将 Nuxt 应用部署到 Azure Portal？

---

## 前提条件

- 配置项目时需要选择后端。即使不需要，否则无法启动站点。
- 服务器需要 Node 8 或更高版本。

## 如果已有不含后端的项目？

不用担心，为现有项目添加 express 服务器很简单。

在项目根目录创建名为 `server` 的新文件夹，然后在 `server` 文件夹中创建 `index.js`，并将以下内容粘贴到 `index.js` 中：

```
const express = require('express')
const consola = require('consola')
const { loadNuxt } = require('nuxt-start')
const app = express()

async function start () {
  const nuxt = await loadNuxt(isDev ? 'dev' : 'start')
  await nuxt.listen(process.env.PORT, process.env.HOST)
}

start()

```

然后编辑 nuxt.config.js：

修改前：

```
import pkg from './package'

export default {
... config
}
```

修改后：

```
module.exports = {
... config
}

```

**请记得删除 config 中对 pkg 对象的引用。**

就这样！

对于 Azure App Service 部署，请确保在 App Service &rsaquo; Settings &rsaquo; Configuration &rsaquo; Application settings 中设置以下两个环境变量（应用程序设置）：

```
HOST: '0.0.0.0'
NODE_ENV: 'production'
```

## 如何在 DevOps 中为 Web 应用设置 Node 版本

可以通过发布管道中 "Deploy Azure Web Service" 任务的应用设置来设置服务器上的 Node 版本。

在应用设置字段的 "Application and Configuration Settings" 中添加：

```
-WEBSITE_NODE_DEFAULT_VERSION 10.16.3
```

建议使用 LTS 版本。

## 构件

如果您正在使用 Azure DevOps 运行构建管道并希望存储构件，以 `.` 开头的文件需要显式移动到构件文件夹。然后创建构件存档，并从发布部署中下载。

## 运行 Web 服务器

Azure Portal 需要 `web.config` 文件。如果未提供，将自动创建。但此文件**不适用于 Nuxt**。请在仓库中添加 web.config 文件。对于最新版本的 `Nuxt`，服务器文件位于 `server/index.js`。web.config 中不指定正确路径 `server/index.js` 而是 `server`。请参阅以下 web.config 示例。否则日志中会显示 Vue 找不到路由的错误。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
     当 iinode 用于在 IIS 或 IIS Express 后面运行节点进程时，
     需要此配置文件。更多详情请参阅：

     https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
-->

<configuration>
  <system.webServer>
    <!-- WebSocket 支持详情请参阅 https://azure.microsoft.com/en-us/blog/introduction-to-websockets-on-windows-azure-web-sites/ -->
    <webSocket enabled="false" />
    <handlers>
      <!-- 表示 server.js 文件是由 iisnode 模块处理的 Node.js 站点 -->
      <add name="iisnode" path="server" verb="*" modules="iisnode"/>
    </handlers>
    <rewrite>
      <rules>
        <!-- 不拦截 node-inspector 调试请求 -->
        <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
          <match url="^server\/debug[\/]?" />
        </rule>

        <!-- 首先判断传入 URL 是否与 /public 文件夹中的物理文件匹配 -->
        <rule name="StaticContent">
          <action type="Rewrite" url="public{REQUEST_URI}"/>
        </rule>

        <!-- 所有其他 URL 映射到 Node.js 站点入口点 -->
        <rule name="DynamicContent">
          <conditions>
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
          </conditions>
          <action type="Rewrite" url="server"/>
        </rule>
      </rules>
    </rewrite>

    <!-- 'bin' 目录对 Node.js 没有特殊意义，但可以将应用放在那里 -->
    <security>
      <requestFiltering>
        <hiddenSegments>
          <remove segment="bin"/>
        </hiddenSegments>
      </requestFiltering>
    </security>

    <!-- 确保错误响应保持原样 -->
    <httpErrors existingResponse="PassThrough" />

    <!--
      以下选项可控制 Node 在 IIS 中的托管方式：
        * watchedFiles：以分号分隔的文件列表，监视其更改以重启服务器
        * node_env：作为 NODE_ENV 环境变量传递给 node
        * debuggingEnabled - 控制是否启用内置调试器

      完整选项列表请参阅 https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
    -->
    <!--<iisnode watchedFiles="web.config;*.js"/>-->
  </system.webServer>
</configuration>
```
