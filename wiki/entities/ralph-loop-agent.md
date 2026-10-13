---
type: entity
title: ralph-loop-agent
tags: [javascript, npm包, ralph-loop, ai-sdk]
related: [ralph-loop, 机器可验证完成条件, deepagents, kimi-cli]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# ralph-loop-agent

ralph-loop-agent 是一个社区实现的 JavaScript 包（GitHub 仓库：`vercel-labs/ralph-loop-agent`），基于 AI SDK 提供 `RalphLoopAgent` 类，用于在 JS 代码中以编程方式精细控制 Ralph Loop 循环（归属与功能描述均来自 [[sources/[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式]]）。注意：文中称其为"社区实现"，但仓库托管于 vercel-labs org，与 Vercel 官方/AI SDK 官方的确切关系待验证。

## 双层循环架构

外层 Ralph Loop（迭代控制）包裹内层 AI SDK Tool Loop（LLM ↔ tools 直到 done）；内层结束后由 `verifyCompletion` 判定——No 则注入 feedback 再迭代，Yes 则返回最终结果。

```
┌──────────────────────────────────────────────────────┐
│                   Ralph Loop (outer)                 │
│  ┌────────────────────────────────────────────────┐  │
│  │  AI SDK Tool Loop (inner)                      │  │
│  │  LLM ↔ tools ↔ LLM ↔ tools … until done       │  │
│  └────────────────────────────────────────────────┘  │
│                         ↓                            │
│  verifyCompletion: "Is the TASK actually complete?"  │
│                         ↓                            │
│       No? → Inject feedback → Run another iteration  │
│       Yes? → Return final result                     │
└──────────────────────────────────────────────────────┘
```

## API 面

构造参数：`model` / `instructions`（含明确完成条件）/ `tools` / `stopWhen: iterationCountIs(N)` / `verifyCompletion: async () => { complete, reason }` / `onIterationStart({ iteration })` / `onIterationEnd({ iteration, duration })`；`loop({ prompt })` 返回 `{ text, iterations, completionReason }`。

三大特性：①模型 + 含明确完成条件的任务说明；②`stopWhen`/`verifyCompletion` 定制退出逻辑；③事件钩子用于日志监控。

完整 Jest→Vitest 迁移示例代码见来源页 [[sources/[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式]]。

## 关联概念

`verifyCompletion` 的编程式结构检查是 [[机器可验证完成条件]] 的直接工程实现；`stopWhen` 迭代上限对应 [[ralph成本控制]] 的安全阀铁律。