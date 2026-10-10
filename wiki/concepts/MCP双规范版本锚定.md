---
type: concept
title: MCP 双规范版本锚定
tags: [mcp, 协议版本, streamable-http, sse]
related: [mcp, 多协议mcp-server框架, less模式, MCP运行时动态管理]
sources: ["[202604220830]AI实践基于SpringAI从0到1构建AIAgent.html"]
created: 2026-10-10
updated: 2026-10-10
---
# MCP 双规范版本锚定

MCP 双规范版本锚定是[[aiagentdemo]]项目 MCP Client 连接策略的协议版本依据：**Streamable HTTP 属 MCP 2025-03-26 规范，SSE 属 MCP 2024-11-05 规范**；连接时优先尝试 Streamable HTTP，失败自动回退 SSE——使回退策略有据可查，而非盲目的协议探测。

```java
// 优先尝试 Streamable HTTP，失败后回退到 SSE
try {
    mcpClient = connectWithStreamableHttp(serverUrl);
    initResult = mcpClient.initialize();
} catch (Exception streamableException) {
    mcpClient = connectWithSse(serverUrl);
    initResult = mcpClient.initialize();
}
```

## 与京东多协议框架的互补

该 Client 侧「双协议降级」与京东 [[多协议mcp-server框架]] 的 Server 侧「单码库三协议」（stdio/streamableHttp/sse）构成 MCP 协议生态的互补两面：Server 侧兼容多协议以扩大客户端覆盖，Client 侧按规范版本新旧自动降级以兼容存量 Server。京东文中 [[less模式]] 指出其 streamableHttp 实现与标准 MCP 协议存在无 sessionId 的差异——本文的规范版本锚定（2025-03-26）恰好为评判这类实现偏差提供了版本基准。

## 局限

文章未覆盖 stdio 传输的适配；SimpleMcpServer（本项目 Server 侧）采用哪种传输协议未披露；MCP 连接鉴权处理未提及。