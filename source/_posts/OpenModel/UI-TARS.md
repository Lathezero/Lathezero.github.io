---
title: UI-TARS 本地部署
date: 2025-04-03 09:10:34
categories:
    - [Agent, UI-TARS]
tags: [Agent, UI-TARS]
description: UI-TARS 本地部署
keywords: Agent, UI-TARS, Agent-TARS, Manus, OpenManus
---

## 

### 先决条件

- [Node.js](https://nodejs.org/en/download/) >= 20
- [pnpm](https://pnpm.io/installation) >= 9

#### 技术栈

这是一个[单存储库](https://pnpm.io/workspaces)项目，包含以下技术：

- 跨平台框架：[Electron](https://www.electronjs.org/)
- 界面：
  - [React](https://react.dev/)
  - [Vite](https://vitejs.dev/)
  - [Chakra UI V2](https://v2.chakra-ui.com/)
- 状态管理和通信：
  - [Zustand](https://zustand.docs.pmnd.rs/)
  - [@ui-tars/electron-ipc](https://github.com/bytedance/UI-TARS-desktop/tree/main/packages/ui-tars/electron-ipc)
- 自动化框架/工具：
  - [nut.js](https://nutjs.dev/)
- 测试框架
  - [Vitest](https://vitest.dev/)
  - [Playwright](https://playwright.dev/)

### 项目结构

```bash
.
├── README.md
├── apps
│   ├── agent-tars
│   │   ├── src
│   │   │   ├── main
│   │   │   ├── preload
│   │   │   ├── renderer
│   │   │   └── vendor
│   └── ui-tars
│       └── src
│           ├── main
│           ├── preload
│           └── renderer
│ 

├── packages
│   ├── agent-infra
│   │   ├── browser
│   │   ├── browser-use
│   │   ├── logger
│   │   ├── mcp-client
│   │   ├── mcp-servers
│   │   ├── search
│   │   └── shared
│   ├── common
│   │   ├── configs
│   │   └── electron-build
│   └── ui-tars
│       ├── action-parser
│       ├── cli
│       ├── electron-ipc
│       ├── operators
│       ├── sdk
│       ├── shared
│       ├── tsconfig.node.json
│       ├── utio
│       └── visualizer
└── vitest.*.mts            # 单元测试配置
```

> **注意**：由于 Electron Forge 之前不支持 Pnpm 的提升机制（[electron/forge#2633](https://github.com/electron/forge/issues/2633)），要求将 `src` 目录放在顶级目录中，因此 `src` 目录位于顶级目录而不是 `apps/{main,preload,renderer}` 目录中。

#### 克隆存储库

```bash
$ git clone https://github.com/bytedance/ui-tars-desktop.git
$ cd ui-tars-desktop
```

### 开发

#### 安装依赖项

```bash
$ pnpm install
```

> **注意**：若安装依赖过程中出现报错，请尝试使用以下镜像重新安装，并且使用git bash；将以下内容添加到.npmrc文件后面
>
> ```
> registry=https://registry.npmmirror.com/
> electron_mirror=https://npmmirror.com/mirrors/electron/
> electron_builder_binaries_mirror=https://npmmirror.com/mirrors/electron-builder-binaries/
> sqlite3_binary_host_mirror=https://npmmirror.com/mirrors/sqlite3/
> # sass_binary_site=https://npmmirror.com/mirrors/node-sass/pnpm 
> chromedriver_cdnurl=https://npmmirror.com/mirrors/chromedriver/
> operadriver_cdnurl=https://npmmirror.com/mirrors/operadriver/
> puppeteer_download_base_url=https://npmmirror.com/mirrors/
> fse_binary_host_mirror=https://npmmirror.com/mirrors/fsevents
> ```

#### 运行应用程序

```bash
$ pnpm run dev:ui-tars    # 启动 UI-TARS Desktop
$ pnpm run dev:agent-tars # 启动 Agent-TARS Desktop
```

应用程序启动后，您可以在应用程序内看到 UI-TARS 界面。

> **注意**：在 MacOS 上，您需要授予运行命令所使用的应用（例如 iTerm2、Terminal）权限。

#### 主进程重载

默认情况下，`pnpm run dev` 只有前端热模块替换（HMR）热更新。如果您需要在调试期间同时重载主进程，可以执行 `pnpm run dev:w`。

```bash
$ pnpm run dev:w
```

#### 构建

在当前系统中运行 `pnpm run build`，它将输出到 `out/*` 目录中。

要构建其他系统的产品，请运行：

- Mac x64：`pnpm run publish:mac-x64`
- Mac ARM：`pnpm run publish:mac-arm64`
- Windows x64：`pnpm run publish:win32`
- Windows ARM：`pnpm run publish:win32-arm64`
