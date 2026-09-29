---
type: concept
title: 多协议 MCP-Server 框架
tags: [mcp, mcp-server, 架构模式, 多协议, stdio, streamablehttp, sse]
related: [mcp-server-ts, mcp, 模块化业务隔离, less模式, 配置优先级链]
created: 2026-06-23
updated: 2026-06-23
sources: ["[202601261426]MCP同时支持stdiostreamableHttpless和sse三种协议的MCP服务框架.html"]
---
# 多协议 MCP-Server 框架

## 定义

多协议 MCP-Server 框架是一种架构模式，指单一代码库通过切换启动命令即可在 [[mcp]] 的多种传输协议（stdio / streamableHttp / sse）间切换的 MCP-Server 设计方案。

## 背景问题

该架构模式针对"MCP 重复造轮子痛点"而设计：不同业务场景需要不同类型的 MCP-Server（本地 stdio vs 网络协议），不同平台对协议的要求不同（streamableHttp vs sse），导致相同 MCP 功能因协议差异需重复开发，造成研发资源浪费和学习成本增加。

## 核心设计

[[mcp-server-ts]] 是该模式的典型实现，其核心设计包括：

1. **公共 server 文件模式**：`server.ts` 作为三模式共享的服务创建和 tools 注册入口，实现协议无关的业务逻辑集中管理。
2. **模式入口分叉**：`stdio.ts` / `streamableHttp.ts` / `sse.ts` 分别作为各协议的入口文件，仅负责调用 SDK 配置传输细节。
3. **路由分离注册**：router 文件夹为 sse 和 streamableHttp 分别注册路由。
4. **模块化业务隔离**：业务开发者仅需在 `tools/` 目录下编写业务逻辑，无需理解协议层细节。

## 价值

- 一次开发支持三种协议，切换模式仅改启动命令，改动成本近乎为零。
- 降低 MCP 使用门槛，不熟悉 MCP 的开发者也能快速创建服务。
- 通过 [[环境变量驱动配置]] 适用于生产环境。

## 与其他工具的关系

Wiki 中多个团队使用了 MCP 但各自独立处理协议问题：腾讯 [[codebuddy]] 将 MCP 作为三大武器库之一接入；[[knot]] 是知识库 MCP Server；转转回收团队使用 `@mcp.tool()` 装饰器封装工具（见 [[mcp工具封装模式]]）。多协议 MCP-Server 框架解决了这些实践中普遍存在的"每个平台单独对接 MCP 协议"的重复劳动问题。