---
type: entity
title: Pi Agent
tags: [ai-agent, openclaw, 框架]
related: [openclaw, agent-loop, agent三层商用架构]
created: 2026-06-12
updated: 2026-10-09
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html", "[202604280830]你不知道的Agent原理架构与工程实践.html"]
---
# Pi Agent

[[openclaw|OpenClaw]] 底层 Agent Core。工具层极简，仅有 4 个核心方法：

| 方法 | 功能 |
|------|------|
| Read | 文件读取 |
| Write | 文件写入 |
| Edit | 文件编辑 |
| Shell | Shell 命令执行 |

## 设计理念

Pi Agent 采用极简工具集 + 事件机制 + Skills 扩展的架构。这种设计验证了一个核心观点：Agent 的智能不来自工具数量，而来自 [[上下文工程]] 的质量。该设计与 [[agent三层商用架构]] 的三层分离理念一致——框架仅提供基础工具，领域知识由 Skills 层承载。

## 工具集合口径注记（2026-10-09）

- 本页「4 核心工具」为 vivo 文极简口径；另有五层架构表与 AgentLoop 注册两口径（web/browser/MCP、message/cron 等），详见 [[pi-agent工具集合口径三变体]]。
