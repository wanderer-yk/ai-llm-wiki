---
type: entity
title: autobots
tags: [平台, 京东, 内部工具, sse, 权限拦截]
related: [mcp-server-ts, joycode]
created: 2026-06-23
updated: 2026-06-23
sources: ["[202601261426]MCP同时支持stdiostreamableHttpless和sse三种协议的MCP服务框架.html"]
---
# autobots

## 简介

autobots 是京东内部的平台，支持 sse 模式的 MCP 服务接入。在 [[mcp-server-ts]] 框架的三协议落地验证中，autobots 引入了基于该框架开发的权限拦截 MCP 服务，是 sse 模式的实际业务验证场景。

## 权限拦截 MCP

基于 mcp-server-ts 框架在 autobots 业务中实际使用的 sse 模式 MCP 服务，用于业务中的权限拦截场景。这是该框架在京东内部落地的首个实际业务案例。

autobots 平台的具体功能、架构以及权限拦截 MCP 的技术细节在现有来源中未详细展开。