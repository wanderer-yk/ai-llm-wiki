---
type: concept
title: Agent Loop
tags: [ai-agent, 框架设计, 上下文工程, 核心机制]
related: [上下文工程, agent框架三要素, react-agent, codeact架构, 极简agent设计哲学]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# Agent Loop

Agent 框架的核心运行机制，本质为一个 **While 循环**。由 [[sources/[202604131736]详尽地带你从零开始设计实现一个AIAgent框架|yabohe（腾讯技术工程）]]在 2026 年 4 月系统阐述。

## 核心机制

每次循环迭代包含三个步骤：

1. **LLM 推理**：将当前上下文（messages 列表）发送给 LLM，获取响应
2. **工具调用**：解析 LLM 响应中的 tool_calls，匹配并执行对应工具
3. **上下文更新**：将工具执行结果追加到 messages 列表，供下一轮推理使用

循环退出条件：
- LLM 不再请求工具调用（任务完成）
- 达到安全上限退出（MAX_TURNS=20）

## 架构位置

在完整框架架构图中，Agent Loop Core 位于中间层：

```
CLI REPL → Agent Loop Core → Tools Registry
              ├── LLM Call
              ├── Tool Call Parser
              ├── Tool Exec Engine
              ├── Response Formatter
              └── Context Manager
```

## 核心论断

> Agent 框架设计的核心 = 在 Agent Loop 这个 While 循环中设计如何管理上下文

这意味着 [[上下文工程]] 是 Agent 框架的真正变量所在——LLM Call 层已有成熟方案（如 LiteLLM），Tools Call 有最佳实践，而上下文管理是"低垂的果实"。

## 与其他概念的关系

- [[react-agent|ReAct]] 的推理+行动+观察循环是 Agent Loop 的理论基础
- [[codeact架构|CodeAct]] 将行动空间统一为可执行代码，是 Agent Loop 中工具调用的一种高级实现
- [[极简agent设计哲学]] 证明 Agent Loop 可在 279 行 Python 中完整实现

## 安全设计

- MAX_TURNS 安全上限防止无限循环
- shell_exec 和 python_exec 均设 30 秒超时保护
- 所有工具统一 `[error]` 前缀返回错误，由 LLM 上下文自行处理异常

## 局限性

文章未讨论上下文压缩/裁剪策略（messages 无限增长时的 Token 限制处理）。