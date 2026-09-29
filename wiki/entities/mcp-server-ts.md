---
type: entity
title: mcp-server-ts
tags: [mcp, mcp-server, typescript, 多协议, 框架, 京东]
related: [mcp, 蔡欣彤, 京东科技技术说, 行云, joycode, autobots, 多协议mcp-server框架, 模块化业务隔离, less模式, 配置优先级链]
created: 2026-06-23
updated: 2026-06-23
sources: ["[202601261426]MCP同时支持stdiostreamableHttpless和sse三种协议的MCP服务框架.html"]
---
# mcp-server-ts

## 简介

mcp-server-ts 是京东科技开发者 [[蔡欣彤]] 从 0 到 1 搭建的 TypeScript [[mcp]]-Server 框架，核心特征是单一代码库同时支持 stdio、streamableHttp 和 sse 三种传输协议。GitHub 仓库地址：https://github.com/XingtongCai/mcp-server-ts

## 设计动机

该框架针对 MCP 开发中的四种痛点而设计：（1）不同业务需要不同类型 MCP-Server；（2）不同平台对协议的要求不同；（3）相同功能因协议差异需重复开发；（4）研发人员学习成本高。通过一次开发三协议支持的架构，切换模式仅需修改启动命令，改动成本近乎为零。

## 核心架构

采用 [[模块化业务隔离]] 设计，将功能拆为独立模块。核心架构包含：

- **公共 server 文件模式**：`server.ts` 作为三模式共享的服务创建和 tools 注册入口，实现协议无关的业务逻辑集中管理。
- **路由分离注册**：router 文件夹为 sse 和 streamableHttp 分别注册路由。
- **工具入口逐个注册**：`tools/index.ts` 作为工具入口，每个工具独立注册。

业务开发者仅需关注 `tools/` 目录下的文件编写业务逻辑，核心协议层文件（index.ts、streamableHttp.ts、sse.ts、stdio.ts）无需修改。

## 协议实现

### stdio

通过 `npm run start` 启动，以标准输入输出方式通信。已发布为京东内网 npm 包 `@jd/demo-mcp-server`（npm.m.jd.com），在 [[joycode]] 成功联通。

### streamableHttp

通过 `npm run start:http` 启动，默认端口 3001。实现了 [[less模式]]（无 sessionId），与标准 MCP streamableHttp 协议存在差异。在 [[joycode]] 成功联通运行。

### sse

通过 `npm run start:sse` 启动，默认端口 3001。在 [[autobots]] 引入并落地权限拦截 MCP 服务。

## 生产配置

支持 [[环境变量驱动配置]]，通过 `MCP_HOST`、`MCP_PORT`、`MCP_DOMAIN`、`MCP_BASE_PATH` 四个环境变量控制，遵循 [[配置优先级链]]。内置 [[行云]] 部署脚本（xingyun/bin/control.sh）。

## 落地状态

三种协议均已验证通过并落地京东内部实际业务：
- stdio → [[joycode]]（@jd/demo-mcp-server）
- streamableHttp → [[joycode]]
- sse → [[autobots]]（权限拦截 MCP）

## 局限性

- less 模式（无 sessionId）与 MCP 标准协议存在差异，需要 sessionId 的场景需用户自行修改。
- 框架性能、并发能力、除 autobots 外的其他采用情况均未披露。