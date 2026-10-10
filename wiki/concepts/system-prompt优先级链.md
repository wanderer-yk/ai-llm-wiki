---
type: concept
title: system-prompt优先级链
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, prompt-engineering, system-prompt]
related: [system-prompt动态组装机制, cacheScope分级缓存, 六大系统内置AgentTool]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# system-prompt优先级链

**system-prompt 优先级链**是 [[claude-code]] 在多来源 System Prompt（默认/自定义/Agent/协调器/覆盖）之间做唯一性决策的固定顺序，由 `utils/systemPrompt.ts` 的 `buildEffectiveSystemPrompt()` 实现。

## 五级优先级（原文还原）

```markdown
优先级从高到低：
1. overrideSystemPrompt  — 强制覆盖（如循环模式下使用）→ 直接返回，忽略一切
2. Coordinator prompt    — 协调器模式激活时的专用 prompt
3. Agent prompt          — 用户定义的 Agent 的 prompt（替换默认）
4. customSystemPrompt    — 通过 --system-prompt 参数传入的自定义 prompt
5. defaultSystemPrompt   — 标准组装流程构建的默认 prompt
另外：appendSystemPrompt 始终追加到最后（除非 override 模式）
```

## 设计要点

- 冲突消解是「高优先级直接替换」而非合并：override 模式忽略一切（用于循环等全托管场景）；Agent prompt 替换默认（见 [[六大系统内置AgentTool]]）。
- `appendSystemPrompt` 的「始终追加」语义保证了用户附加指令在任何模式下都不丢失，是链上唯一的非互斥项。

证据等级：二手网络整理资料，源码路径与行为未经官方验证。
