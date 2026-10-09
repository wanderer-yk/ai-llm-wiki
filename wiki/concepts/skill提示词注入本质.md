---
type: concept
title: Skill 提示词注入本质
created: 2026-10-09
updated: 2026-10-09
tags: [claude-code, skills, prompt-engineering]
related: [claude-code, api请求位置决定论, skill流程非代码化, skill双执行模式, skill列表token预算, rules与skills等价论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# Skill 提示词注入本质

Skill 提示词注入本质是本文对 Skills 机制的源码级定谳：**Skills 是「提示词注入」机制，不是函数调用**——`tool_use` 只是触发器，真正能力来自触发后注入对话历史的 SKILL.md Markdown 指令文本。Skills 表现形式上像「标准化工作流」，但源码中没有任何代码逻辑控制执行步骤（见 [[skill流程非代码化]]）。

## 两阶段注入机制（定谳）

1. **列表常驻注册**：Skill 列表经 `skill_listing` attachment 注入 messages（`isMeta: true`），预算为上下文窗口 1%（见 [[skill列表token预算]]）；
2. **触发后指令注入**：模型（或用户 `/skill-name`）输出 `tool_use: { name: "Skill", input: { skill, args } }`，客户端读取 SKILL.md，将其包装为 `isMeta: true` 的 user 消息注入对话历史；`tool_result` 仅返回标签 `"Launching skill: commit"`。

## Inline 模式执行流程（默认）

```text
模型输出 tool_use → 读取本地 SKILL.md → 包装为 isMeta: true 的 user 消息注入
→ tool_result 仅返回 "Launching skill: commit"
→ 下一轮 API 调用时对话历史已含完整 Skill 指令
→ 模型按指令调用工具（Read、Edit、Bash 等）执行任务
```

Fork 模式（`context: 'fork'`）提供独立执行生命周期，见 [[skill双执行模式]]。

## 工程定位

相比手动 `@commit-rules.md`，手动 `/commit` 多绕的 `tool_use` → 读文件 → 注入步骤「本质上只是提供了额外的工程便利」，对模型效果几乎一样——Skills 的真正价值在自动化触发、可发现可分发与执行隔离（[[skills真正价值三场景]]、[[rules与skills等价论]]）。
