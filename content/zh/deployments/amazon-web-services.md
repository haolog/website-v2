---
template: guide
title: Amazon Web Services
description: 使用 S3 Amplify 和 CloudFront 在 AWS 上进行静态托管
target: Static
category: deployment
logo:
  light: "/img/companies/square/light/AWS_Light.svg"
  dark: "/img/companies/square/dark/AWS_Dark.svg"
---

# 将 Nuxt 部署到 Amazon Web Services

使用 S3 Amplify 和 CloudFront 在 AWS 上进行静态托管

---

AWS 是 Amazon Web Services 的缩写。
S3 是可配置静态网站托管的静态存储服务。CloudFront 是其 CDN（内容分发网络）。

## Amplify Console 与 AWS

使用 Amplify Console 在 AWS 上托管**静态生成的** Nuxt 应用程序，既强大又经济实惠。

首先，将您的 Nuxt 应用程序推送到任意 Git 提供商，然后访问 Amplify Console。如果您从未使用过 Amplify Hosting，请点击 **Deploy** 标题下的 **GET STARTED** 按钮，否则请点击 **Connect App** 按钮。

## From your existing code

在 "From your existing code" 页面，选择您的 Git 提供商并点击 **Continue**。

### Add repository branch

在 "Add repository branch" 页面，选择您要部署的仓库和分支，然后点击 **Next**。

## 构建设置

在 "Configure build settings" 页面，点击 "Build and test settings" 中的 `Edit` 按钮，并按如下方式进行修改：

1. 将 **build** 命令设置为 `npm run generate`。
2. 将 `baseDirectory` 的位置设置为 `dist`。

编辑完成后的配置如下所示：

```yml
version: 1
frontend:
  phases:
    preBuild:
      commands:
        - yarn install
    build:
      commands:
        - npm run generate
  artifacts:
    # 重要 - 请确认构建输出目录
    baseDirectory: dist
    files:
      - '**/*'
  cache:
    paths:
      - node_modules/**/*
```

然后点击 **Save** 和 **Next**。

### 审查

在审查页面，点击 **Save and deploy**。

之后，应用程序将开始部署，此过程需要几分钟。

当 `Provision`、`Build`、`Deploy` 和 `Verify` 全部变为绿色后，点击 Amplify Console 提供的 URL 即可查看您的网站。

## AWS 与 S3 + CloudFront

使用 S3 + CloudFront 在 AWS 上托管**静态生成的** Nuxt 应用程序，既强大又经济实惠。

> AWS 正在逐步更新。如果有遗漏的步骤，请提交 PullRequest 来更新本文档。

### 概述

使用多个 AWS 服务以极低的成本进行托管。简要概述如下：

- S3
  - 用于存放网站文件的云数据 "bucket"
  - 可配置为托管静态网站
- CloudFront
  - CDN（内容分发网络）
  - 免费提供 HTTPS 证书
  - 加快网站加载速度

网站推送流程如下：

```
Nuxt Generate -> Local folder -> AWS S3 Bucket -> AWS CloudFront CDN -> Browser
  [      nuxt generate       ]    [         gulp deploy          ]
  [                         deploy.sh                            ]
```

首先，使用 `nuxt generate`（<= v2.12）生成网站，然后使用 [Gulp](https://gulpjs.com/) 将文件发布到 S3 存储桶并使 CloudFront CDN 失效。

- [gulp](https://www.npmjs.com/package/gulp)
- [gulp-awspublish](https://www.npmjs.com/package/gulp-awspublish)
- [gulp-cloudfront-invalidate-aws-publish](https://www.npmjs.com/package/gulp-cloudfront-invalidate-aws-publish)
- [concurrent-transform](https://www.npmjs.com/package/concurrent-transform)（用于并行上传）

部署脚本需要设置以下环境变量：

- AWS_BUCKET_NAME="example.com"
- AWS_CLOUDFRONT="UPPERCASE"
- AWS_ACCESS_KEY_ID="key"
- AWS_SECRET_ACCESS_KEY="secret"

相关文件如下：

```
deploy.sh       -  执行 `nuxt generate` 和 `gulp deploy`
gulpfile.js     -  将文件推送到 S3 并使 CloudFront 失效的 `gulp deploy` 代码
```

### 配置方法

1. 创建 S3 存储桶并配置静态网站托管
2. 创建 CloudFront 分发
3. 配置安全访问权限
4. 在项目中配置构建脚本

### AWS：S3 存储桶与 CloudFront 分发的配置

步骤 1、2 请参照 [S3 与 CloudFront 配置教程](https://learnetto.com/blog/cloudfront-s3) 进行操作。

完成后您将获得以下数据：

- AWS_BUCKET_NAME="example.com"
- AWS_CLOUDFRONT="UPPERCASE"

### AWS：安全访问权限配置

步骤 3 需要创建一个具有以下权限的用户：

- 更新存储桶内容
- 使 CloudFront 分发失效（可更快地将变更传达给用户）

[使用此策略创建程序用户](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)：

> NOTE: 请将以下内容中的 2 处 `example.com` 替换为您的 S3 存储桶名称。此策略允许向指定存储桶推送内容，并使 CloudFront 分发失效。

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:ListBucket"],
      "Resource": ["arn:aws:s3:::example.com"]
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:PutObjectAcl",
        "s3:GetObject",
        "s3:GetObjectAcl",
        "s3:DeleteObject",
        "s3:ListMultipartUploadParts",
        "s3:AbortMultipartUpload"
      ],
      "Resource": ["arn:aws:s3:::example.com/*"]
    },
    {
      "Effect": "Allow",
      "Action": [
        "cloudfront:CreateInvalidation",
        "cloudfront:GetInvalidation",
        "cloudfront:ListInvalidations",
        "cloudfront:UnknownOperation"
      ],
      "Resource": "*"
    }
  ]
}
```

然后[获取访问密钥和 Secret。](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)

完成后您将获得以下数据：

- AWS_ACCESS_KEY_ID="key"
- AWS_SECRET_ACCESS_KEY="secret"

### 本地：配置项目构建脚本

4.1) 创建 `deploy.sh` 脚本。可选参考 [nvm（node 版本管理器）](https://github.com/creationix/nvm)。

```bash
#!/bin/bash

export AWS_ACCESS_KEY_ID="key"
export AWS_SECRET_ACCESS_KEY="secret"
export AWS_BUCKET_NAME="example.com"
export AWS_CLOUDFRONT="UPPERCASE"

# 加载 nvm（node 版本管理器），安装 node（.nvmrc 中指定的版本），并通过 npm 安装包
[ -s "$HOME/.nvm/nvm.sh" ] && source "$HOME/.nvm/nvm.sh" && nvm use
# 如果尚未安装，则通过 Npm 安装
[ ! -d "node_modules" ] && npm install

npm run generate
gulp deploy
```

4.2) 使 `deploy.sh` 可执行，并将其加入 Git 忽略列表（deploy.sh 包含密钥信息）

```bash
chmod +x deploy.sh
echo "
# Don't commit build files
node_modules
dist
.nuxt
.awspublish
deploy.sh
" >> .gitignore
```

4.3) 将 Gulp 添加到项目和命令行

```bash
npm install --save-dev gulp gulp-awspublish gulp-cloudfront-invalidate-aws-publish concurrent-transform
npm install -g gulp
```

4.4) 创建包含构建脚本的 `gulpfile.js`

```javascript
const gulp = require('gulp')
const awspublish = require('gulp-awspublish')
const cloudfront = require('gulp-cloudfront-invalidate-aws-publish')
const parallelize = require('concurrent-transform')

// https://docs.aws.amazon.com/cli/latest/userguide/cli-environment.html

const config = {
  // 必填
  params: {
    Bucket: process.env.AWS_BUCKET_NAME
  },
  credentials: {
    accessKeyId: process.env.AWS_ACCESS_KEY_ID,
    secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
    signatureVersion: 'v3'
  },

  // 可选
  deleteOldVersions: false, // NOT FOR PRODUCTION
  distribution: process.env.AWS_CLOUDFRONT, // CloudFront distribution ID
  region: process.env.AWS_DEFAULT_REGION,
  headers: {
    /* 'Cache-Control': 'max-age=315360000, no-transform, public', */
  },

  // 合理的默认值 - 这些文件和目录已加入 gitignore
  distDir: 'dist',
  indexRootPath: true,
  cacheFileName: '.awspublish',
  concurrentUploads: 10,
  wait: true // 等待 CloudFront 失效完成（约 30-60 秒）
}

gulp.task('deploy', function () {
  // 使用 S3 选项创建新的发布者
  // http://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#constructor-property
  const publisher = awspublish.create(config)

  let g = gulp.src('./' + config.distDir + '/**')
  // 发布者会添加 Content-Length、Content-Type 以及上述 headers
  // 如未指定，默认将 x-amz-acl 设置为 public-read
  g = g.pipe(
    parallelize(publisher.publish(config.headers), config.concurrentUploads)
  )

  // 使 CDN 失效
  if (config.distribution) {
    console.log('Configured with CloudFront distribution')
    g = g.pipe(cloudfront(config))
  } else {
    console.log(
      'No CloudFront distribution configured - skipping CDN invalidation'
    )
  }

  // 删除已移除的文件
  if (config.deleteOldVersions) {
    g = g.pipe(publisher.sync())
  }
  // 创建缓存文件以加速后续上传
  g = g.pipe(publisher.cache())
  // 创建缓存文件以加速后续上传
  g = g.pipe(awspublish.reporter())
  return g
})
```

4.5) 部署与调试

运行以下命令：

```bash
./deploy.sh
```

您应该会看到类似如下的输出：

```bash
$ ./deploy.sh

Found '/home/michael/scm/example.com/www/.nvmrc' with version <8>
Now using node v8.11.2 (npm v5.6.0)

> example.com@1.0.0 generate /home/michael/scm/example.com/www
> nuxt generate

  nuxt:generate Generating... +0ms
  nuxt:build App root: /home/michael/scm/example.com/www +0ms
  nuxt:build Generating /home/michael/scm/example.com/www/.nuxt files... +0ms
  nuxt:build Generating files... +36ms
  nuxt:build Generating routes... +10ms
  nuxt:build Building files... +24ms
  ████████████████████ 100%

Build completed in 7.009s



 DONE  Compiled successfully in 7013ms                                                                                                                                     21:25:22

Hash: 421d017116d2d95dd1e3
Version: webpack 3.12.0
Time: 7013ms
                                   Asset     Size  Chunks             Chunk Names
     pages/index.ef923f795c1cecc9a444.js  10.6 kB       0  [emitted]  pages/index
 layouts/default.87a49937c330bdd31953.js  2.69 kB       1  [emitted]  layouts/default
pages/our-values.f60c731d5c3081769fd9.js  3.03 kB       2  [emitted]  pages/our-values
   pages/join-us.835077c4e6b55ed1bba4.js   1.3 kB       3  [emitted]  pages/join-us
       pages/how.75f8cb5bc24e38bca3b3.js  2.59 kB       4  [emitted]  pages/how
             app.6dbffe6ac4383bd30a92.js   202 kB       5  [emitted]  app
          vendor.134043c361c9ad199c6d.js  6.31 kB       6  [emitted]  vendor
        manifest.421d017116d2d95dd1e3.js  1.59 kB       7  [emitted]  manifest
 + 3 hidden assets
Hash: 9fd206f4b4e571e9571f
Version: webpack 3.12.0
Time: 2239ms
             Asset    Size  Chunks             Chunk Names
server-bundle.json  306 kB          [emitted]
  nuxt: Call generate:distRemoved hooks (1) +0ms
  nuxt:generate Destination folder cleaned +10s
  nuxt: Call generate:distCopied hooks (1) +8ms
  nuxt:generate Static & build files copied +7ms
  nuxt:render Rendering url /our-values +0ms
  nuxt:render Rendering url /how +67ms
  nuxt:render Rendering url /join-us +1ms
  nuxt:render Rendering url / +0ms
  nuxt: Call generate:page hooks (1) +913ms
  nuxt: Call generate:page hooks (1) +205ms
  nuxt: Call generate:page hooks (1) +329ms
  nuxt: Call generate:page hooks (1) +361ms
  nuxt:generate Generate file: /our-values/index.html +2s
  nuxt:generate Generate file: /how/index.html +0ms
  nuxt:generate Generate file: /join-us/index.html +0ms
  nuxt:generate Generate file: /index.html +0ms
  nuxt:render Rendering url / +2s
  nuxt: Call generate:done hooks (1) +4ms
  nuxt:generate HTML Files generated in 11.8s +5ms
  nuxt:generate Generate done +0ms
[21:25:27] Using gulpfile ~/scm/example.com/www/gulpfile.js
[21:25:27] Starting 'deploy'...
Configured with CloudFront distribution
[21:25:27] [cache]  README.md
[21:25:27] [cache]  android-chrome-192x192.png
[21:25:27] [cache]  android-chrome-512x512.png
[21:25:27] [cache]  apple-touch-icon.png
[21:25:27] [cache]  browserconfig.xml
[21:25:27] [cache]  favicon-16x16.png
[21:25:27] [cache]  favicon-32x32.png
[21:25:27] [cache]  favicon.ico
[21:25:27] [cache]  favicon.svg
[21:25:27] [cache]  logo-branches.svg
[21:25:27] [cache]  logo-small.svg
[21:25:27] [cache]  logo.svg
[21:25:27] [cache]  mstile-150x150.png
[21:25:27] [cache]  og-image.jpg
[21:25:27] [cache]  safari-pinned-tab.svg
[21:25:27] [cache]  site.webmanifest
[21:25:28] [create] _nuxt/manifest.421d017116d2d95dd1e3.js
[21:25:29] [update] 200.html
[21:25:30] [create] videos/flag.jpg
[21:25:30] [create] _nuxt/vendor.134043c361c9ad199c6d.js
[21:25:34] [create] videos/flag.mp4
[21:25:34] [cache]  _nuxt/pages/how.75f8cb5bc24e38bca3b3.js
[21:25:34] [cache]  _nuxt/pages/join-us.835077c4e6b55ed1bba4.js
[21:25:34] [cache]  _nuxt/pages/our-values.f60c731d5c3081769fd9.js
[21:25:36] [update] our-values/index.html
[21:25:36] [create] _nuxt/layouts/default.87a49937c330bdd31953.js
[21:25:36] [create] _nuxt/app.6dbffe6ac4383bd30a92.js
[21:25:37] [create] _nuxt/pages/index.ef923f795c1cecc9a444.js
[21:25:38] [update] join-us/index.html
[21:25:38] [update] how/index.html
[21:25:43] [create] videos/flag.webm
[21:25:43] [update] index.html
[21:25:43] CloudFront invalidation created: I16NXXXXX4JDOA
[21:26:09] Finished 'deploy' after 42 s
```

另外，`CloudFront invalidation created: XXXX` 是 CloudFront invalidation npm 包的唯一输出。如果看不到该输出，说明它没有正常工作。
