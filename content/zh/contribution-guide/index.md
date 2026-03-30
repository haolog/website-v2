---
template: post
title: 贡献指南
description: 欢迎任何形式的 Nuxt 贡献！
back: false
---

> 欢迎任何形式的 Nuxt 贡献！

## 问题报告

为项目做贡献的一个好方法是在遇到问题时提交详细的报告：[Bug 报告](https://github.com/nuxt/nuxt/issues/new?assignees=&labels=pending+triage%2C2.x&template=z-bug-report-2.yml)

提交时，请务必准备好可复现 Bug 的仓库或 [CodeSandBox](https://template.nuxtjs.org/)。Bug 的可复现性越高，我们就能越快着手修复。

## Pull Request

哪怕只是修正错别字，我们也欢迎您提交 Pull Request！

但重要的改进需要关联到现有的[功能请求](https://feature.nuxtjs.org/)或 [Bug 报告](https://bug.nuxtjs.org/)。

### 开始

1. 将 [Nuxt 仓库](https://github.com/nuxt/nuxt) [Fork](https://help.github.com/articles/fork-a-repo/) 到您自己的 GitHub 账户，然后将其[克隆](https://help.github.com/articles/cloning-a-repository/)到本地设备。
2. 运行 `npm install` 或 `yarn install` 以安装依赖模块。

> _**npm** 和 **yarn** 都存在安装依赖失败的情况。如需解决此问题，请删除示例应用的 `node_modules` 目录后重新安装，或在本地安装缺失的依赖。_

> 如需添加依赖模块，请使用 `yarn add`。`yarn.lock` 文件是所有 Nuxt 依赖关系的正确来源。

### 设置

在运行测试之前，请确保所有依赖包均已满足，并构建所有包：

```sh
yarn
yarn build
```

### 测试结构

包含 Bug 修复或新功能的优质 Pull Request 通常都包含测试。为了帮助您编写优质测试，下面介绍我们的测试结构：

#### Fixtures

Fixtures（位于 `tests/fixtures` 下）包含若干 Nuxt 应用程序。为了尽量缩短构建时间，我们不会为每个测试单独构建 Nuxt 应用，而是在运行实际单元测试之前构建 Fixtures（`yarn test:fixture`）。

提交 Pull Request 时，请务必**修改**或**添加新的 Fixture**（如适用），以确保变更内容得到正确反映。

另外，修改 Fixture 后，请不要忘记通过 `jest test/fixtures/my-fixture/my-fixture.test.js` 运行对应测试来**重新构建** Fixture！

#### 单元测试

单元测试位于 `tests/unit`，在 Fixture 构建完成后运行。每个测试都会使用新的 Nuxt 服务器，因此不存在共享状态（构建步骤的初始状态除外）。

添加单元测试后，可以直接运行它们：

```sh
jest test/unit/test.js
```

也可以运行整个单元测试套件：

```sh
yarn test:unit
```

请注意，您可能需要重新构建之前的 Fixture！

### 测试变更

在创建 Pull Request 的过程中，您可能需要检查 Fixture 是否正确设置，或调试当前的变更。

为此，您可以使用 Nuxt 脚本本身来启动 Fixture 或示例应用：

```sh
yarn nuxt examples/your-app
yarn nuxt test/fixtures/your-fixture-app
```

> `npm link` 也有类似效果，但已知存在一些问题。因此，建议直接调用 `yarn nuxt` 来运行示例。

### 示例

如果您正在开发较大的功能，请在 `examples/` 中设置示例应用。这对于理解变更内容很有帮助，也有助于 Nuxt 用户深入理解您所开发的功能。

### Lint

您可能已经注意到，我们使用 ESLint 来强制统一代码风格。提交变更前，请运行 `yarn lint` 检查代码风格是否正确。如有问题，可使用 `yarn lint --fix` 或 `npm run lint -- --fix`（不是错别字！）来修复大多数风格问题。如果仍有错误残留，则需要手动修复。

### 文档

添加新功能、进行重构或修改 Nuxt 行为时，您可能希望将变更记录在文档中。请向 [docs](https://github.com/nuxt/docs/pulls) 仓库提交 Pull Request。不必立即编写文档（但请在 Pull Request 足够成熟后尽快编写）。

### 最终检查清单

提交 Pull Request 时，会有一个简单的模板供您填写。请在检查清单中勾选所有适当的"回答（answers）"。

### 故障排除

#### 在 macOS 上调试测试

搜索 `getPort()` 可以发现它被用于在测试期间启动新的 Nuxt 进程。在 macOS 上有时无法正常工作，可能需要手动为测试设置端口。

另一个常见问题是，运行 Fixture 测试时 Nuxt 进程可能会在内存中挂起。出现幽灵进程时，后续测试往往无法正常运行。如果您怀疑遇到了此问题，请运行 `ps aux | grep -i node` 来排查挂起的测试进程。
