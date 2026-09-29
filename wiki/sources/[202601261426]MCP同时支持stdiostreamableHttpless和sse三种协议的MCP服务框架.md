---
type: source
title: "同时支持 stdio、streamableHttpless 和 sse 三种协议的 MCP 服务框架"
authors: [蔡欣彤]
year: 2026
url: "https://github.com/XingtongCai/mcp-server-ts"
venue: 京东科技技术说
tags: [mcp, mcp-server, typescript, stdio, streamablehttp, sse, 多协议, 京东]
related: [mcp-server-ts, mcp, 蔡欣彤, 京东科技技术说, 多协议mcp-server框架, 模块化业务隔离, less模式]
created: 2026-06-23
updated: 2026-06-23
sources: ["[202601261426]MCP同时支持stdiostreamableHttpless和sse三种协议的MCP服务框架.html"]
---
# 同时支持 stdio、streamableHttpless 和 sse 三种协议的 MCP 服务框架

## 概要

本文由京东科技开发者蔡欣彤撰写，发表于微信公众号「京东科技技术说」（IP 属地北京），发布时间为 2026 年 1 月 26 日。文章介绍了作者从 0 到 1 搭建的 TypeScript MCP-Server 框架 [[mcp-server-ts]]，该框架通过单一代码库同时支持 [[mcp]] 的三种传输协议：stdio、streamableHttp 和 sse，开发者仅需修改启动命令即可在协议间切换。

GitHub 仓库地址：https://github.com/XingtongCai/mcp-server-ts

## 背景与痛点

作者指出，MCP 服务在 AI 方向业务中使用频率高，但存在四种痛点：

1. **协议需求差异**：不同业务需要不同类型的 MCP-Server（本地 stdio vs 网络协议）。
2. **平台兼容性差异**：不同平台对协议的要求不同（streamableHttp vs sse）。
3. **重复开发**：相同 MCP 功能因平台协议差异需重复开发，浪费时间。
4. **学习成本**：不熟悉 MCP 的研发人员需要现学，拉长研发周期。

## 框架特点

- 一次开发支持三种协议，切换模式仅改启动命令，改动成本近乎为零。
- [[模块化业务隔离]]设计，开发者仅需在指定文件编写业务逻辑，核心协议层文件无需修改。
- [[环境变量驱动配置|环境变量配置]]使框架适用于生产环境。
- 内置日志模块。
- 支持京东内部部署平台 [[行云]]。

## 技术架构

### 目录结构

```
src/
  router/          # streamableHttp 和 sse 路由
    index.ts
    mcp.ts
    sse.ts
  tools/           # MCP 工具注册
    index.ts
    mockFunc.ts
  cli.ts           # 命令行入口
  server.ts        # 三模式共享的服务创建和 tools 注册入口
  sse.ts           # sse 模式入口
  stdio.ts         # stdio 模式入口
  streamableHttp.ts # streamableHttp 模式入口
xingyun/bin/        # 行云部署脚本
```

### 启动命令

| 命令 | 协议 | 默认端口 |
|------|------|---------|
| `npm run start` | stdio | — |
| `npm run start:http` | streamableHttp | 3001 |
| `npm run start:sse` | sse | 3001 |
| `npm run start -- -t http -p 3001` | 自定义协议和端口 | 指定值 |

package.json 定义了 build/start/start:http/start:sse/dev:http/stop/restart/inspector 共八个脚本。

### 生产环境配置

通过四个环境变量控制：
- `MCP_HOST`：服务地址（如 `192.168.1.100`）
- `MCP_PORT`：端口（如 `3001`）
- `MCP_DOMAIN`：域名（如 `mcp-server.internal.com`）
- `MCP_BASE_PATH`：基础路径（如 `/api/mcp`）

配置遵循 [[配置优先级链]]：端口优先级为环境变量 > 命令行参数 > 默认值；访问地址优先级为 `MCP_DOMAIN` > `MCP_HOST` > localhost。

## 关键设计决策

### 公共 server 文件模式

`server.ts` 作为三种模式共享的 MCP 服务创建和 tools 注册入口，实现协议无关的业务逻辑集中管理。各模式入口文件（stdio.ts / streamableHttp.ts / sse.ts）仅负责调用 SDK 配置传输细节。

### less 模式

streamableHttp 实现为 [[less模式]]（无 sessionId），与标准 MCP streamableHttp 协议（可能含 sessionId）存在差异。作者明确指出："StreamableHttp.ts 我支持的是 less，就是我不需要 sessionId，如果有需要的，这块需要再自己改一下。"

### 路由分离注册

router 文件夹为 sse 和 streamableHttp 分别注册路由：streamableHttp 通过 basePath 指定访问路径；sse 在 sse.ts 中定义 GET/POST 方法。

## 落地成果

三种协议已全部验证通过并落地实际业务：

- **stdio**：发布为京东内网 npm 包 `@jd/demo-mcp-server`（npm.m.jd.com），在 [[joycode]] 成功联通。
- **streamableHttp**：在 [[joycode]] 成功联通运行。
- **sse**：在 [[autobots]] 引入并落地权限拦截 MCP 服务。

## 跨团队关联

本框架是 [[mcp]] 协议工程化实践的典型案例，与 Wiki 中其他 MCP 相关内容形成互补：腾讯 [[codebuddy]] 将 MCP 作为三大武器库之一；[[knot]] 是知识库 MCP Server；转转回收团队 [[六步智能提示词生成法]] 使用 `@mcp.tool()` 装饰器封装工具。蔡欣彤的工作聚焦于 MCP-Server 框架本身的多协议支持，填补了 Wiki 中 MCP Server 工程化构建维度的空白。

## 开放问题

- 框架在京东内部除 autobots 权限拦截外的其他采用情况未披露。
- less 模式（无 sessionId）在实际业务中的功能限制未讨论。
- 框架性能和并发能力未提及。
- 是否有更广泛的开源推广计划不明。