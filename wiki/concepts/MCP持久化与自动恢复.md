---
type: concept
title: MCP 持久化与自动恢复
tags: [mcp, 持久化, mcp-servers.json]
related: [mcp, MCP运行时动态管理, MCP双规范版本锚定]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# MCP 持久化与自动恢复

MCP 持久化与自动恢复是[[aiagentdemo]]项目 MCP Client 的关键特性之一：MCP 连接的 URL 持久化到 **`mcp-servers.json`** 文件，应用重启时自动重连。在 `McpClient.connect()` 中体现为 `store.add(serverUrl)` 一行——连接成功即落盘，「下次启动自动恢复」。

## 在 MCP Client 三关键特性中的位置

文章 6.2 节将 MCP Client 关键特性收束为恰三条：

1. 传输协议自动适配（[[MCP双规范版本锚定]]）
2. 工具自动发现（SyncMcpToolCallbackProvider 将远程工具转 `ToolCallback` 注册）
3. **持久化与自动恢复（本页）**

## 意义

持久化机制使 [[MCP运行时动态管理]] 的运行时 connect 操作具备跨会话生命力：动态接入的 MCP 服务不会因应用重启而丢失，形成「运行时动态 + 持久化兜底」的完整连接生命周期管理。这与 OpenClaw/24h 打工人等系统中「文件即状态」的持久化哲学一致，但作用对象是 MCP 连接拓扑。

## 待核

`mcp-servers.json` 的具体格式（条目结构、是否含鉴权信息）未披露；store 的实现细节（本地文件 vs 数据库）未明确。