---
type: entity
title: execute（本地终端执行工具）
tags: [工具, 控制台, 终端]
related: [console-vs-mcp-strategy, openclaw, progressive-tool-loading]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# execute（本地终端执行工具）

**execute** 是 [[openclaw|OpenClaw]] 采用的**统一本地能力入口**工具，通过单一接口调用本地终端命令（如 `curl`、`psql`、`grep` 等）。

## 设计动机

- 大模型在预训练中已具备终端命令的"世界知识"。
- 无需为每个命令单独定义 API 接口，即可实现**零学习成本**调用。
- 由于 `tools` 列表保持不变（始终只有一个 `execute`），可实现 **100% 缓存命中**。

## 在混合策略中的位置

参见 [[console-vs-mcp-strategy]]：

- 本地高频能力 → `execute` 控制台
- 远程鉴权服务 → [[mcp]]

## 参见

- [[console-vs-mcp-strategy]]
- [[prompt-cache-mechanism]]
- [[progressive-tool-loading]]