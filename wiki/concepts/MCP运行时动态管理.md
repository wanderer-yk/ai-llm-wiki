---
type: concept
title: MCP 运行时动态管理
tags: [mcp, 运行时, rest-api, 工具管理]
related: [mcp, 渐进式工具加载, less模式, 多协议mcp-server框架, MCP持久化与自动恢复, MCP双规范版本锚定]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# MCP 运行时动态管理

MCP 运行时动态管理是[[aiagentdemo]]项目的 MCP Client 能力：通过 REST API 在**不重启应用**的情况下增/删/查 MCP 连接，且工具**即时生效**——区别于传统「配置文件 + 重启加载」的静态 MCP 接入方式。

## 三端点表

| 接口 | 方法 | 说明 |
|------|------|------|
| `/api/manage/mcp/connect` | POST | 连接新的 MCP 服务，工具立即可用 |
| `/api/manage/mcp/disconnect` | POST | 断开 MCP 服务，移除对应工具 |
| `/api/manage/mcp/list` | GET | 查看所有 MCP 服务及其工具列表 |

## 与工具加载谱系的关系

- **vs [[渐进式工具加载]]（vivo）**：vivo 方案在会话中按需追加工具到 messages 尾部、保留缓存；本方案在连接层动态增删工具集，两者作用于不同层次（消息层 vs 连接层），可组合；
- **vs [[多协议mcp-server框架]]（京东）**：京东解决 Server 侧「单码库三协议」的提供问题，本方案解决 Client 侧「运行时增删连接」的消费问题，构成 MCP 协议生态的互补两面；
- **与 [[MCP持久化与自动恢复]] 的配合**：运行时 connect 的 URL 同时持久化到 `mcp-servers.json`，动态接入的连接在重启后仍能自动恢复——动态性与持久性兼顾。

## 待核

connect 端点请求体格式（URL 如何传入）未披露；disconnect 后工具从 Agent 移除的底层机制（是否运行时重建 ChatClient）未说明；MCP 连接的鉴权处理未提及。