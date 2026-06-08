---
type: entity
title: MCP（Model Context Protocol）
tags: [协议, 工具, 远程服务]
related: [console-vs-mcp-strategy, prompt-cache-mechanism, progressive-tool-loading]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# MCP（Model Context Protocol）

**MCP（Model Context Protocol，模型上下文协议）** 是一种用于远程服务的**标准化接口与鉴权**协议，其核心价值在于**动态工具发现与加载**。

## 在 Agent 架构中的角色

### 优势

- 标准化远程服务接入，提供鉴权与动态发现能力。
- 支持运行时按需加载工具，提升灵活性。

### 矛盾

MCP 的动态加载能力与 [[prompt-cache-mechanism|Prompt 缓存]] 的稳定性要求存在根本冲突：

- 动态改变 `tools` 列表会破坏缓存前缀。
- 这使得"完全动态的 MCP 工具"与"高缓存命中"难以兼得。

### 推荐用法

文章提出 [[console-vs-mcp-strategy|控制台 + MCP 混合策略]]：

- **本地高频能力** → 用控制台（单个 `execute` 工具）
- **远程鉴权服务** → 用 MCP

## 参见

- [[console-vs-mcp-strategy]]
- [[progressive-tool-loading]]
- [[prompt-based-tool-injection]]