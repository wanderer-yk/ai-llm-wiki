---
type: entity
title: joycode
tags: [ide, 编码平台, 京东, 内部工具]
related: [mcp-server-ts, autobots]
created: 2026-06-23
updated: 2026-06-23
sources: ["[202601261426]MCP同时支持stdiostreamableHttpless和sse三种协议的MCP服务框架.html"]
---
# joycode

## 简介

joycode 是京东内部的 IDE / 编码平台。在 [[mcp-server-ts]] 框架的三协议落地验证中，joycode 成功联通了 stdio 和 streamableHttp 两种模式：

- **stdio 模式**：通过内网 npm 包 `@jd/demo-mcp-server`（npm.m.jd.com）接入 joycode。
- **streamableHttp 模式**：直接在 joycode 中联通运行。

joycode 的具体功能特性、MCP 集成方式和用户规模等细节在现有来源中未详细展开。